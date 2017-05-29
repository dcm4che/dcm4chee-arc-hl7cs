Patient Management Service
""""""""""""""""""""""""""

The Patient Management Service processes ADT messages received from external Patient Demographics Sources, updating the
patient information of archived DICOM objects, and send ADT messages to external Patient Demographics Consumers on
patient information changes initiated via the Web UI or proprietary RESTful services of |product|.

The functionality provided by the Patient Management Service of |product| is compliant with the requirements for
following IHE Integration Profiles and Actors with Option:

.. csv-table::
   :header: "Integration Profile", "Actor", "Option"
   :widths: 40, 40, 20

      "Patient Information Reconciliation", "Image Manager/Image Archive", "HL7 v2.5.1"
      "Patient Administration Management", "Patient Demographics Consumer", "Merge"
      "Patient Administration Management", "Patient Demographics Source", "Merge"

as specified in the IHE Technical Frameworks for `Radiology <http://ihe.net/Technical_Frameworks/#radiology>`_
and `IT Infrastructure <http://ihe.net/Technical_Frameworks/#IT>`_.

Update Patient Information on receive of ADT message
''''''''''''''''''''''''''''''''''''''''''''''''''''

Create new or update existing Patient
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ADT messages

.. csv-table::
   :name: adt_update
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

      "ADT/ACK - Admit/Visit Notification (Event A01)", "2.3.1, 2.5.1"
      "ADT/ACK - Transfer a Patient (Event A02)", "2.3.1, 2.5.1"
      "ADT/ACK - Discharge/End Visit (Event A03)", "2.3.1, 2.5.1"
      "ADT/ACK - Register a Patient (Event A04)", "2.3.1, 2.5.1"
      "ADT/ACK - Pre-Admit a Patient (Event A05)", "2.3.1, 2.5.1"
      "ADT/ACK - Change an Outpatient to an Inpatient (Event A06)", "2.3.1, 2.5.1"
      "ADT/ACK - Change an Inpatient to an Outpatient (Event A07)", "2.3.1, 2.5.1"
      "ADT/ACK - Update Patient Information (Event A08)", "2.3.1, 2.5.1"
      "ADT/ACK - Cancel Transfer (Event A12)", "2.3.1, 2.5.1"
      "ADT/ACK - Cancel Discharge/End Visit  (Event A13)", "2.3.1, 2.5.1"
      "ADT/ACK - Add Person or Patient Information (Event A28)", "2.5"
      "ADT/ACK - Update Person Information (Event A31)", "2.5"

are processed the same: Patient IDs and other Patient Information are extracted from the PID segment
of the received ADT message and mapped into corresponding DICOM attributes. If a Patient record with the
extracted primary Patient ID already exists in the database, that Patient record will get updated. If there is no such
Patient record, a new Patient record will be inserted into the database.

On retrieve of DICOM objects, the potentially updated DICOM attributes from the the Patient record of the DB will be
merged with the original DICOM attributes of the stored DICOM objects, so the changes in the Patient information are
reflected in the retrieved DICOM objects.

Merge two Patients
^^^^^^^^^^^^^^^^^^

On receive of ADT message

.. csv-table::
   :name: adt_a40
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

      "ADT/ACK - Merge Patient - Patient Identifier List (Event A40)", "2.3.1, 2.5, 2.5.1"

the PID segment is processed as for the ADT messages in :numref:`adt_update`. Additionally, Patient ID and
- if present - Patient Demographic Information are extracted from the MRG segment. If a Patient record with the
extracted Patient ID from the MRG segment already exists in the database, all associated Study, MPPS and MWL records
will be moved to the Patient record with the Patient ID from the PID segment. If there is no Patient record which
Patient ID matches the Patient ID from the MRG segment, a new Patient record will be inserted into the database.
Therefore there will be always a Patient Record with the Patient ID from the MRG segment, which contains a reference
to the *dominant* Patient Record with the Patient ID, marking them as *merged*.

Subsequently received HL7 messages referring a *merged* Patient by its Patient ID will be rejected, whereas DICOM
objects to a *merged* Patient will be accepted. Particularly, if the Patient ID in the first received DICOM object of
a Study matches the Patient ID of a *merged* Patient record in the database, the new Study record will be associated
with the *dominant* Patient record, so the stale Patient Information in the received DICOM object will be replaced by
the updated Patient Information in the *dominant* Patient record on retrieve of DICOM objects of that Study.

Change Patient ID of Patient
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default configuration, the ADT message

.. csv-table::
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

      "ADT/ACK - Change Patient Identifier List (Event A47)", "2.5"

is processed as the ADT message in :numref:`adt_a40`, with the exception that the message will be rejected, if
a Patient record with the extracted primary Patient ID from the PID segment already exists in the database. This
allows to recognize a stale Patient ID in the received DICOM object and replace them by the new Patient ID as in the
merge case.

The behavior can be changed by

.. tabularcolumns:: |p{4cm}|l|p{8cm}|l|
.. csv-table:: Archive Device Attribute (LDAP Object: dcmArchiveDevice)
   :header: Name, Type, Description, LDAP Attribute
   :widths: 20, 7, 60, 13

   "HL7 Track Changed Patient ID",boolean,"Enable to keep track of the prior Patient ID on a change of the Patient ID by HL7 ADT^A47 or by the RESTful Patient Update Service. Enabled if absent.","hl7TrackChangedPatientID"

With ``HL7 Track Changed Patient ID = false``, instead of the *merging* of two Patient records, the Patient record
referred by the Patient ID in the MRG will be updated with the new Patient ID from the PID segment. Consequently,
subsequently received HL7 messages with the previous Patient ID will be accepted, causing the insert of a new
Patient record in the database with the previous Patient ID. Also the receive of DICOM objects with the previous
Patient ID will then cause the insert of a new Patient record, associated with the new received Study.

Send ADT message on Patient Information changes
'''''''''''''''''''''''''''''''''''''''''''''''

|product| provide RESTful services for Patient Management, which are also used by its UI. |product| may be configured
to send ADT messages to external Patient Demographics Consumers to synchronize them with the changes of the
Patient Information performed by the RESTful services.

.. csv-table:: Emitted ADT messages
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

      "ADT/ACK - Add Person or Patient Information (Event A28)", "2.5"
      "ADT/ACK - Update Person Information (Event A31)", "2.5"
      "ADT/ACK - Merge Patient - Patient Identifier List (Event A40)", "2.5"
      "ADT/ACK - Change Patient Identifier List (Event A47)", "2.5"
