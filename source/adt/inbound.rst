Inbound
#######

.. _adt_in_messages:

Inbound Messages
================

.. _adt_in_a01:

ADT/ACK - Admit/Visit Notification (Event A01)
----------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a patient has arrived at a healthcare facility for an episode of care in which
the patient is assigned to an inpatient bed. Such an episode is commonly referred to as "inpatient" care.

Field *MSH-9 Message Type* shall be valued **ADT^A01^ADT_A01**. The third component is optional for HL7 v2.3.1.

.. _adt_in_a01_segments:

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ADT^A01^ADT_A01 message:

.. csv-table:: Supported segments of ADT^A01^ADT_A01 (HL7 v2.3.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH - :ref:`tab_msh_231`, Message Header, 2
   PID - :ref:`tab_pid_231`, Patient Identification, 3
   NTE - :ref:`tab_nte_231`, Notes and Comments (for PID), 2

.. csv-table:: Supported segments of ADT^A01^ADT_A01 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   NTE - :ref:`tab_nte_251`, Notes and Comments (for PID), O, [0..1], 2

.. _adt_in_a01_actions:

Performed Actions
^^^^^^^^^^^^^^^^^
Patient IDs and other Patient Information are extracted from the PID segment of the received ADT message and mapped
into corresponding DICOM attributes as defined in :ref:`adt_in_pid_dicom`. If a Patient record with the extracted
primary Patient ID already exists in the database, that Patient record will get updated. If there is no such Patient
record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.

On retrieve of DICOM objects, the potentially updated DICOM attributes from the Patient record of the DB will be
merged with the original DICOM attributes of the stored DICOM objects, so the changes in the Patient information are
reflected in the retrieved DICOM objects.

.. [#hl7NoPatientCreateMessageType] The creation of new Patient records will be suppressed for message types which are
   listed by configuration parameter *HL7 No Patient Create Message Type(s)*  of |product|.


.. _adt_in_a02:

ADT/ACK - Transfer a Patient (Event A02)
----------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a patient is being transferred from one location to another. The new location
will be reflected in the institution’s bed census. 

Field *MSH-9 Message Type* shall be valued **ADT^A02^ADT_A02**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a03:

ADT/ACK - Discharge/End Visit (Event A03)
-----------------------------------------
Supported HL7 version: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a patient’s stay at a healthcare facility has ended. Inpatient encounters are
generally closed by an A03. Outpatient encounters may or may not be closed by an A03, depending on the healthcare
organization policies.

Field *MSH-9 Message Type* shall be valued **ADT^A03^ADT_A03**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a04:

ADT/ACK - Register a Patient (Event A04)
----------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a patient has arrived at a healthcare facility for an episode of care in which
the patient is not assigned to a bed. Examples of such episodes include outpatient visits, ambulatory care encounters,
and emergency room visits.

Field *MSH-9 Message Type* shall be valued **ADT^A04^ADT_A01**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a05:

ADT/ACK - Pre-Admit a Patient (Event A05)
-----------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application communicate information that has been collected about a patient to be admitted as an inpatient
(or to be registered as an outpatient).

Field *MSH-9 Message Type* shall be valued **ADT^A05^ADT_A05**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a06:

ADT/ACK - Change an Outpatient to an Inpatient (Event A06)
----------------------------------------------------------
Supported HL7 version: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that it has been decided to admit a patient that was formerly in a non-admitted
status, such as Emergency.

Field *MSH-9 Message Type* shall be valued **ADT^A06^ADT_A06**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a07:

ADT/ACK - Change an Inpatient to an Outpatient (Event A07)
----------------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a patient is no longer in an "admitted" status, but is still being seen for an
episode of care..

Field *MSH-9 Message Type* shall be valued **ADT^A07^ADT_A06**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a08:

ADT/ACK - Update Patient Information (Event A08)
------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that some non-movement-related information (such as address, date of birth, etc.) has
changed for a patient. It is used when information about the patient has changed not related to any other trigger event.

Field *MSH-9 Message Type* shall be valued **ADT^A08^ADT_A01**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a10:

ADT/ACK - Patient Arriving - Tracking (Event A10)
-------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application sends this event when a patient arrives at a new location in the healthcare facility (inpatient
or outpatient) (via trigger event A09).

Field *MSH-9 Message Type* shall be valued **ADT^A10^ADT_A09**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

Additionally, if configuration parameter *HL7 Patient Arrival Message Type*  of |product| is configured as *ADT^A10*,
SPS Status of any MWL items (which are in **SCHEDULED** status) associated with this patient shall be changed to **ARRIVED**.

.. _adt_in_a11:

ADT/ACK - Cancel Admit/Visit Notification (Event A11)
-----------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application cancels a previous notification that a patient has been admitted for an inpatient stay (via
trigger event A01) or registered for an outpatient visit (via trigger event A04).

Field *MSH-9 Message Type* shall be valued **ADT^A11^ADT_A09**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a12:

ADT/ACK - Cancel Transfer (Event A12)
-------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application cancels a previous notification (via trigger event A02) that a patient was being moved from
one location to another.

Field *MSH-9 Message Type* shall be valued **ADT^A12^ADT_A12**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a13:

ADT/ACK - Cancel Discharge/End Visit  (Event A13)
-------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application cancels a previous notification (via trigger event A03) that a patient’s stay at a healthcare
facility had ended.

Field *MSH-9 Message Type* shall be valued **ADT^A13^ADT_A01**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a28:

ADT/ACK - Add Person or Patient Information (Event A28)
-------------------------------------------------------
Supported HL7 version: 2.5 (ITI-30)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application communicates the demographics of a new patient, as well as related information.

Field *MSH-9 Message Type* shall be valued **ADT^A28^ADT_A05**.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a31:

ADT/ACK - Update Person Information (Event A31)
-----------------------------------------------
Supported HL7 version: 2.5 (ITI-30)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application updates the demographics of an existing patient.

Field *MSH-9 Message Type* shall be valued **ADT^A31^ADT_A05**.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a38:

ADT/ACK - Cancel Pre-Admit (Event A38)
--------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-31)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application cancels a previous notification (via trigger event A05) that a patient was to be updated to
pre-admitted (or pre-registered) status.

Field *MSH-9 Message Type* shall be valued **ADT^A38^ADT_A38**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_segments`.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a01_actions`.

.. _adt_in_a40:

ADT/ACK - Merge Patient - Patient Identifier List (Event A40)
-------------------------------------------------------------
Supported HL7 versions: 2.3.1, 2.5.1 (ITI-30)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies the merge of records for a patient that was incorrectly filed under two different
identifiers. This message is only used to merge two patient identifiers of the same type, or two lists of patient
identifiers. It is not used to update other patient demographics information. The A31 trigger event should be used
for this purpose.

Field *MSH-9 Message Type* shall be valued **ADT^A40^ADT_A39**. The third component is optional for HL7 v2.3.1.

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ADT^A40^ADT_A39 message:

.. csv-table:: Supported segments of ADT^A40^ADT_A39 (HL7 v2.3.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH - :ref:`tab_msh_231`, Message Header, 2
   PID - :ref:`tab_pid_231`, Patient Identification, 3
   NTE - :ref:`tab_nte_231`, Notes and Comments, 2
   MRG - :ref:`tab_mrg_231`, Merge Information, 3

.. csv-table:: Supported segments of ADT^A40^ADT_A39 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   NTE - :ref:`tab_nte_251`, Notes and Comments, O, [0..1], 2
   MRG - :ref:`tab_mrg_251`, Merge Information, R, [1..1], 3

The "incorrect supplier identifier" identified in the MRG segment (*MRG-1 Prior Patient Identifier List*) is to be
merged with the required "correct target identifier" in the PID segment (*PID-3 Patient Identifier List*). The
"incorrect supplier identifier" would then logically never be referenced in future transactions.

Performed Actions
^^^^^^^^^^^^^^^^^
Patient IDs and other Patient Information for the dominant Patient record are extracted from the PID segment of the
received ADT message and mapped into corresponding DICOM attributes as defined in :ref:`adt_in_pid_dicom`. If a
Patient record with the extracted primary Patient ID already exists in the database, that Patient record will get updated.
If there is no such Patient record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.

Patient ID and the Patient name for the old Patient record are extracted from the MRG segment of the received ADT
message and mapped into corresponding DICOM attributes as defined in :ref:`adt_in_mrg_dicom`. If a Patient record
with the extracted primary Patient ID already exists in the database, all associated Study, MPPS and MWL records
will be moved to the Patient record with the Patient ID from the PID segment. If there is no such Patient record a
new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_. Therefore there will be always
a Patient Record with the Patient ID from the MRG segment, which contains a reference to the *dominant* Patient Record
with the Patient ID, marking them as *merged*.

Subsequently received HL7 messages referring a *merged* Patient by its Patient ID will be rejected, whereas DICOM
objects to a *merged* Patient will be accepted. Particularly, if the Patient ID in the first received DICOM object of
a Study matches the Patient ID of a *merged* Patient record in the database, the new Study record will be associated
with the *dominant* Patient record, so the stale Patient Information in the received DICOM object will be replaced by
the updated Patient Information in the *dominant* Patient record on retrieve of DICOM objects of that Study.

.. _adt_in_a47:

ADT/ACK - Change Patient Identifier List (Event A47)
----------------------------------------------------
Supported HL7 version: 2.5 (ITI-30)

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies the change of a patient identifier list for a patient.

That is, a single *PID-3 patient identifier list value* has been found to be incorrect and has been changed.
This message is not used to update other patient demographics information. The A31 trigger event should be used for
this purpose.

Field  *MSH-9 Mesage Type* shall be valued **ADT^A47^ADT_A30**.

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ADT^A47^ADT_A30 message:

.. csv-table:: Supported Segments of ADT^A47^ADT_A30 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   NTE - :ref:`tab_nte_251`, Notes and Comments, O, [0..1], 2
   MRG - :ref:`tab_mrg_251`, Merge Information, R, 1..1], 3

The "incorrect supplier identifier" value is stored in the MRG segment (*MRG-1 Prior Patient Identifier List*) and is
to be changed to the "correct target patient ID" value stored in the PID segment (*PID-3 Patient Identifier List*).

Performed Actions
^^^^^^^^^^^^^^^^^
The "correct" Patient IDs and other Patient Information for the Patient record are extracted from the PID segment of
the received ADT message and mapped into corresponding DICOM attributes as defined in :ref:`adt_in_pid_dicom`. If a
Patient record with the extracted primary Patient ID already exists in the database, the message will be rejected.

The "incorrect" Patient ID and the prior Patient name are extracted from the MRG segment of the received ADT message
and mapped into corresponding DICOM attributes as defined in :ref:`adt_in_mrg_dicom`.

Further behavior depends on if *HL7 Track Changed Patient ID* is enabled/disabled by a correspondent configuration
parameter of |product|:

HL7 Track Changed Patient ID enabled
''''''''''''''''''''''''''''''''''''
A new Patient record with Patient IDs and other Patient Information from the PID segment will be inserted into the
database. If a Patient record with the prior Patient ID from the MRG segment already exists in the database, all
associated Study, MPPS and MWL records will be moved to the Patient record with the Patient ID from the PID segment. If
there is no such Patient record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.
Therefore there will be always a Patient Record with the Patient ID from the MRG segment, which contains a reference to
the *dominant* Patient Record with the Patient ID, marking them as *merged*.

Subsequently received HL7 messages referring a *merged* Patient by its Patient ID will be rejected, whereas DICOM
objects to a *merged* Patient will be accepted. Particularly, if the Patient ID in the first received DICOM object of
a Study matches the Patient ID of a *merged* Patient record in the database, the new Study record will be associated
with the *dominant* Patient record, so the stale Patient Information in the received DICOM object will be replaced by
the updated Patient Information in the *dominant* Patient record on retrieve of DICOM objects of that Study.

HL7 Track Changed Patient ID disabled
'''''''''''''''''''''''''''''''''''''
If a Patient record with the previous Patient ID from the MRG segment already exists in the database, it will be updated
with the Patient IDs and other Patient Information from the PID segment. If there is no such Patient record a new Patient
record with the Patient IDs and other Patient Information from the PID segment will be inserted into the database
[#hl7NoPatientCreateMessageType]_.

Consequently, subsequently received HL7 messages with the previous Patient ID will be accepted, causing the insert of a
new Patient record in the database with the previous Patient ID. Also the receive of DICOM objects with the previous
Patient ID will then cause the insert of a new Patient record, associated with the new received Study.

.. _adt_in_segments:

Inbound Message Segments
========================

.. _adt_in_msh:

MSH - Message Header segment
----------------------------
Same as specified in :ref:`tab_msh_231` or :ref:`tab_msh_251`

.. _adt_in_pid:

PID - Patient Identification segment
------------------------------------
.. csv-table:: PID - Patient Identification segment (HL7 v2.3.1)
   :name: tab_pid_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00104, SetID -Patient ID
   2, 20, CX, O, , 00105, **Patient ID**
   3, 20, CX, R, , 00106, **Patient Identifier List**
   4, 20, CX, O, , 00107, **Alternate Patient ID**
   5, 48, XPN, R, , 00108, **Patient Name**
   6, 48, XPN, O, , 00109, **Mother’s Maiden Name**
   7, 26, TS, R2, , 00110, **Date/Time of Birth**
   8, 1, IS, R, 0001, 00111, **Sex**
   9, 48, XPN, O, , 00112, **Patient Alias**
   10, 80, CE, R2, 0005, 00113, Race
   11, 1, 06, XAD, R2, 00114, **Patient Address**
   12, 4, IS, O, , 00115, County Code
   13, 40, XTN, O, , 00116, Phone Number - Home
   14, 40, XTN, O, , 00117, Phone Number - Business
   15, 60, CE, O, 0296, 00118, Primary Language
   16, 1, IS, O, 0002, 00119, Marital Status
   17, 80, CE, O, 0006, 00120, Religion
   18, 20, CX, C, , 00121, **Patient Account Number**
   19, 16, ST, O, , 00122, SSN Number – Patient
   20, 25, DLN, O, , 00123, Driver's License Number - Patient
   21, 20, CX, O, , 00124, Mother's Identifier
   22, 80, CE, O, 0189, 00125, Ethnic Group
   23, 60, ST, O, , 00126, Birth Place
   24, 1, ID, O, 0136, 00127, Multiple Birth Indicator
   25, 2, NM, O, , 00128, Birth Order
   26, 80, CE, O, 0171, 00129, Citizenship
   27, 60, CE, O, 0172, 00130, Veterans Military Status
   28, 80, CE, O, , 00739, Nationality
   29, 26, TS, O, , 00740, Patient Death Date and Time
   30, 1, ID, O, 0136, 00741, Patient Death Indicator

.. csv-table:: PID - Patient Identification segment (HL7 v2.5.1)
   :name: tab_pid_251
   :header: SEQ, LEN, DT, Usage, Card., TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1, 4, SI, O, [0..1], , 00104, Set ID - PID
   2, 20, CX, O, [0..0], , 00105, **Patient ID**
   3, 250, CX, R, [1..*], , 00106, **Patient Identifier List**
   4, 20, CX, O, [0..0], , 00107, **Alternate Patient ID - PID**
   5, 250, XPN, R, [1..*], , 00108, **Patient Name**
   6, 250, XPN, O, [0..1], , 00109, **Mother’s Maiden Name**
   7, 26, TS, CE, [0..1], , 00110, **Date/Time of Birth**
   8, 1, IS, CE, [1..1], 0001, 00111, **Administrative Sex**
   9, 250, XPN, O, [0..1], , 00112, Patient Alias
   10, 250, CE, O, [0..1], 0005, 00113, Race
   11, 250, XAD, CE, [0..*], , 00114, **Patient Address**
   12, 4, IS, X, [0..1], 0289, 00115, County Code
   13, 250, XTN, O, [0..*], , 00116, Phone Number - Home
   14, 250, XTN, O, [0..*], , 00117, Phone Number - Business
   15, 250, CE, O, [0..1], 0296, 00118, Primary Language
   16, 250, CE, O, [0..1], 0002, 00119, Marital Status
   17, 250, CE, O, [0..1], 0006, 00120, Religion
   18, 250, CX, C, [0..1], , 00121, **Patient Account Number**
   19, 16, ST, X, [0..1], , 00122, SSN Number - Patient
   20, 25, DLN, X, [0..1], , 00123, Driver's License Number - Patient
   21, 250, CX, O, [0..*], , 00124, Mother's Identifier
   22, 250, CE, O, [0..1], 0189, 00125, Ethnic Group
   23, 250, ST, O, [0..1], , 00126, Birth Place
   24, 1, ID, O, [0..1], 0136, 00127, Multiple Birth Indicator
   25, 2, NM, O, [0..1], , 00128, Birth Order
   26, 250, CE, O, [0..1], 0171, 00129, Citizenship
   27, 250, CE, O, [0..1], 0172, 00130, Veterans Military Status
   28, 250, CE, X, [0..0], 0212, 00739, Nationality
   29, 26, TS, CE, [0..1], , 00740, Patient Death Date and Time
   30, 1, ID, C, [0..1], 0136, 00741, Patient Death Indicator
   31, 1, ID, CE, [0..1], 0136, 01535, Identity Unknown Indicator
   32, 20, IS, CE, [0..*], 0445, 01536, Identity Reliability Code
   33, 26, TS, CE, [0..1], , 01537, Last Update Date/Time
   34, 241, HD, O, [0..1], , 01538, Last Update Facility
   35, 250, CE, CE, [0..1], 0446, 01539, **Species Code**
   36, 250, CE, C, [0..1], 0447, 01540, **Breed Code**
   37, 80, ST, O, [0..1], , 01541, Strain
   38, 250, CE, O, [0..2], , 01542, Production Class Code
   39, 250, CWE, O, [0..*], , 01840, Tribal Citizenship

Element names in **bold** indicates that the field is used by |product|.

Patient IDs included in the PID-3 field shall include Assigning Authority (Component 4). The first subcomponent
(namespace ID) of Assigning Authority shall be populated. If the second and third subcomponents (universal ID and
universal ID type) are also populated, they shall reference the same entity as is referenced in the first subcomponent.

This field may be populated with various identifiers assigned to the patient by various assigning authorities.

.. _adt_in_nte:

NTE - Notes and Comments segment (for PID)
------------------------------------------
.. csv-table:: NTE - Notes and Comments segment (for PID) (HL7 v2.3.1)
   :name: tab_nte_231
   :header: SEQ, LEN, DT, OPT, RP/#, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , , 00096, SetID - NTE
   2, 4, ID, O, , 0105, 00097, Source of Comment
   3, 64k, FT, O, Y, , 00098, **Comment**

.. csv-table:: NTE - Notes and Comments segment (for PID) (HL7 v2.5.1)
   :name: tab_nte_251
   :header: SEQ, LEN, DT, OPT, RP/#, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , , 00096, SetID - NTE
   2, 4, ID, O, , 0105, 00097, Source of Comment
   3, 65536, FT, O, Y, , 00098, **Comment**
   4, 250, CE, O, , 0364, 01318, Comment Type

Element names in **bold** indicates that the field is used by |product|.

.. _adt_in_mrg:

MRG - Merge segment
-------------------
.. csv-table:: MRG - Merge segment (HL7 v2.3.1)
   :name: tab_mrg_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 20, CX, R, , 00211, **Prior Patient Identifier List**
   2, 20, CX, O, , 00212, Prior Alternate Patient ID
   3, 20, CX, O, , 00213, Prior Patient Account Number
   4, 20, CX, R2, , 00214, Prior Patient ID
   5, 20, CX, O, , 01279, Prior Visit Number
   6, 20, CX, O, , 01280, Prior Alternate Visit ID
   7, 48, XPN, R2, , 01281, **Prior Patient Name**

.. csv-table:: MRG - Merge segment (HL7 v2.5.1)
   :name: tab_mrg_251
   :header: SEQ, LEN, DT, Usage, Card., TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1, 250, CX, R, [1..*], , 00211, **Prior Patient Identifier List**
   2, 250, CX, X, [0..0], , 00212, Prior Alternate Patient ID
   3, 250, CX, O, [0..1], , 00213, Prior Patient Account Number
   4, 250, CX, X, , [0..0], 00214, Prior Patient ID
   5, 250, CX, X, [0..0], , 01279, Prior Visit Number
   6, 250, CX, X, [0..0], , 01280, Prior Alternate Visit ID
   7, 250, XPN, O, [0..*], , 01281, **Prior Patient Name**

Element Names in **bold** indicates that the field is used by |product|.

.. _adt_in_dicom:

HL7 ADT to DICOM Mapping
========================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. csv-table:: HL7 ADT mapping of PID segment to DICOM Patient Attributes
   :name: adt_in_pid_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note1]_
   **Patient Identification**
   Patient's Name, "(0010, 0010)", Patient  Name, 00108, PID:5
   Patient ID, "(0010, 0020)", Patient Identifier List, 00106.1, PID:3.1
   Issuer of Patient ID, "(0010, 0021)", Patient Identifier List, 00106.4.1, PID:3.4.1
   Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >Item, "(FFFE, E000)"
   >Universal Entity ID, "(0040, 0032)", Patient Identifier List, 00106.4.2, PID:3.4.2
   >Universal Entity ID Type, "(0040, 0033)", Patient Identifier List, 00106.4.3, PID:3.4.3
   Other Patient IDs Sequence, "(0010, 1002)"
   >Patient ID, "(0010, 0020)", Patient ID, 00105.1, PID:2.1
   >Issuer of Patient ID, "(0010, 0021)", Patient ID, 00105.4.1, PID:2.4.1, "set to ``CHIP``, if PID:2.4.1 empty"
   >Type of Patient ID, "(0010, 0022)", , , , set to ``RFID``
   >Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >>Universal Entity ID, "(0040, 0032)", Patient Identifier List, 00105.4.2, PID:2.4.2
   >>Universal Entity ID Type, "(0040, 0033)", Patient Identifier List, 00105.4.3, PID:2.4.3
   >Item, "(FFFE, E000)"
   >Patient ID, "(0010, 0020)", Alternate Patient ID - PID, 00107.1, PID:4.1
   >Issuer of Patient ID, "(0010, 0021)", Alternate Patient ID - PID, 00107.4.1, PID:4.4.1, "set to ``TATTOO``, if PID:4.4.1 empty"
   >Type of Patient ID, "(0010, 0022)", , , , set to ``BARCODE``
   >Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >>Universal Entity ID, "(0040, 0032)", Patient Identifier List, 00107.4.2, PID:4.4.2
   >>Universal Entity ID Type, "(0040, 0033)", Patient Identifier List, 00107.4.3, PID:4.4.3
   Patient's Mother's Birth Name, "(0010, 1060)", Mother’s Maiden Name, 00109, PID:6
   **Patient Demographic**
   Patient's Birth Date, "(0010, 0030)", Date/Time of Birth, 00110, PID:7
   Patient's Sex, "(0010, 0040)", Administrative Sex, 00111.1, PID:8.1
   Responsible Person, "(0010, 2297)", Patient Alias, 00112, PID:9
   Responsible Person Role, "(0010, 2298)", , , , "set to ``OWNER``, if PID:9 is not empty"
   Patient's Address, "(0010, 1040)", Patient Address, 00114, PID:11
   Patient Species Description, "(0010, 2201)", Species Code, 01539.2, PID:35.2
   Patient Species Code Sequence, "(0010, 2202)"
   >Code Value, "(0008, 0100)", Species Code, 01539.1, PID:35.1
   >Coding Scheme Designator, "(0008, 0102)", Species Code, 01539.3, PID:35.3
   >Code Meaning, "(0008, 0104)", Species Code, 01539.2, PID:35.2
   Patient Breed Description, "(0010, 2292)", Breed Code, 01540.2, PID:36.2
   Patient Breed Code Sequence, "(0010, 2293)"
   >Code Value, "(0008, 0100)", Breed Code, 01540.1, PID:36.1
   >Coding Scheme Designator, "(0008, 0102)", Breed Code, 01540.3, PID:36.3
   >Code Meaning, "(0008, 0104)", Breed Code, 01540.2, PID:36.2
   Patient Comments, "(0010, 4000)", Comment, 00098, NTE:3
   **Patient Medical**
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'⇒'ALTERED', 'N'⇒'UNALTERED'"

.. csv-table:: HL7 ADT mapping of MRG segment to DICOM Patient Attributes
   :name: adt_in_mrg_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note1]_
   **Patient Identification**
   Patient's Name, "(0010, 0010)", Prior Patient  Name, 01281, MRG:7
   Patient ID, "(0010, 0020)", Prior Patient Identifier List, 00211.1, MRG:1.1
   Issuer of Patient ID, "(0010, 0021)", Prior Patient Identifier List, 00211.4.1, MRG:1.4.1
   Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >Universal Entity ID, "(0040, 0032)", Prior Patient Identifier List, 00211.4.2, MRG:1.4.2
   >Universal Entity ID Type, "(0040, 0033)", Prior Patient Identifier List, 00211.4.3, MRG:1.4.3

.. _adt_in_err:

HL7 ADT - Error Mapping
=======================

Following table gives an overview of error codes and messages sent by |product| for incoming HL7 ADT messages triggering
error conditions.

.. csv-table:: Error Codes Mapping and Usage
   :name: tab_hl7_adt_error
   :header: Error Code,Error Code Meaning,Error Location,User Message,Notes

   **Error Common**
   Same as Error Codes Mapping and Usage in :ref:`tab_hl7_error`
   **Patient Management specific**
   101,Required Field Missing,PID^1^3^1^1,Missing patient identifier,
   ,,MRG^1^1^1^1,Missing prior patient identifier,
   204,Unknown Key Identifier,PID^1^3^1^1,,[#Note2]_
   ,,MRG^1^1^1^1,,[#Note2]_
   205,Duplicate Key Identifier,PID^1^3,Either previous or new Patient ID has missing issuer and change patient id tracking is enabled. Disable change patient id tracking feature and retry update,
   ,,MRG^1^1^1^1,Prior patient identifier matches patient identifier,
   207,Application Internal Error,,,[#Note3]_

.. [#Note1] `HL7 DICOM Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7dicomcharacterset>`_
   if configured, is selected to specify Specific Character Set. Else, MSH-18 if present in the incoming HL7 message, :ref:`tab_hl7_dicom_charset` 
   is selected to specify Specific Character Set. If MSH-18 is absent, then
   `HL7 Default Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7Application.html#hl7defaultcharacterset>`_
   is selected to specify Specific Character Set.

.. [#Note2] Message stating respective patient identifier refers to an already merged patient record. Depends on configured
   `HL7 Referred Merged Patient Policy <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7referredmergedpatientpolicy>_`.

.. [#Note3] User message in ERR:7 is set to exception message. This exception pertains to HL7 ADT message processing
   triggered internal application failure.