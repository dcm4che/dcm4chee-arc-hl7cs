Patient Management Service
""""""""""""""""""""""""""

The Patient Management Service processes ADT messages received from external Patient Demographics Sources, updating the
patient information of archived DICOM objects, and send ADT messages to external Patient Demographics Consumers on
patient information changes initiated via the Web UI or proprietary RESTful services of |product|.

Create/Update Patient on receive of ADT message
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ADT messages

.. csv-table::
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
      "ADT/ACK - Add Person or Patient Information (Event A28)", "2.5"
      "ADT/ACK - Update Person Information (Event A31)", "2.5"

are processed the same: Patient IDs and Patient Demographic Information are extracted from the PIR segment
of the received ADT message and mapped into corresponding DICOM attributes. If a Patient record with the
extracted primary Patient ID already exists in the database, that Patient record will get updated. If there is no such
Patient record, a new Patient record will be inserted into the database.

On retrieve of DICOM objects, the potentially updated DICOM attributes from the the Patient record of the DB will be
merged with the original DICOM attributes of the stored DICOM objects, so the changes in the Patient information are
reflected in the retrieved DICOM objects.

Merge Patient on receive of ADT Merge Patient message
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

<TODO>

Change Patient ID on receive of ADT Change Patient Identifier List message
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

<TODO>

Send ADT Add Person or Patient Information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

<TODO>




