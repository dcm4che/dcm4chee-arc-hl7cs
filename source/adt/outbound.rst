Outbound
########

The HL7 messages mentioned below are sent to other HL7 applications/receivers if the feature Synchronize External Receivers
explained in https://github.com/dcm4che/dcm4chee-arc-light/wiki/Patient-Information has been configured in the archive.

.. _adt_out_messages:

Outbound Messages
=================

.. _adt_out_a28:

ADT/ACK - Add Person or Patient Information (Event A28)
-------------------------------------------------------
Supported HL7 version: 2.5

Trigger Event
^^^^^^^^^^^^^
This message is sent when a new patient is created in the archive by using archive UI and RESTful service.
Patient IDs and other Patient Information for the new Patient record are sent in the PID segment of the
outgoing ADT message mapped from corresponding DICOM attributes as defined in :ref:`dicom_in_pid_adt`.

.. _adt_in_a28_segments:

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are sent in an outgoing ADT^A28^ADT_A05 message:

.. csv-table:: Supported segments of ADT^A28^ADT_A05 (HL7 v2.5)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH, Message Header, R, [1..1], 2
   PID, Patient Identification, R, [1..1], 3


Expected Actions
^^^^^^^^^^^^^^^^
HL7 Application/Receiver should have the ability to receive and process the demographics of a new patient, as well as
related information. Field *MSH-9 Message Type* is valued as **ADT^A28^ADT_A05**.
It is expected that the receiver shall create a new patient record for the patient identified if there is no current
record for the Patient ID (defined by the field PID-3). This message shall not be used by the receiver to update
information in an existing patient record.

.. _adt_out_a31:

ADT/ACK - Update Person Information (Event A31)
-----------------------------------------------
Supported HL7 version: 2.5

Trigger Event
^^^^^^^^^^^^^
This message is sent when an existing patient is updated in the archive by using archive UI and RESTful service.
Patient IDs and other Patient Information for the updated Patient record are sent in the PID segment of the
outgoing ADT message mapped from corresponding DICOM attributes as defined in :ref:`dicom_in_pid_adt`.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a28_segments`.

Expected Actions
^^^^^^^^^^^^^^^^
HL7 Application/Receiver should have the ability to receive and process the demographics of an updated patient.
Field *MSH-9 Message Type* is valued as **ADT^A31^ADT_A05**.
It is expected that the receiver shall update the patient information of an existing patient record for the patient
identified by the Patient ID (defined by the field PID-3).

.. _adt_out_a40:

ADT/ACK - Merge Patient - Patient Identifier List (Event A40)
-------------------------------------------------------------
Supported HL7 version: 2.5

Trigger Event
^^^^^^^^^^^^^
This message is sent when a one or more patients are merged with a target patient in the archive by using archive UI
and RESTful service.
Patient IDs and other Patient Information for the target Patient record are sent in the PID segment of the
outgoing ADT message mapped from corresponding DICOM attributes as defined in :ref:`dicom_in_pid_adt`.
Patient ID and the Patient name for the old Patient record are sent in the MRG segment of the outgoing ADT
message and mapped from corresponding DICOM attributes as defined in :ref:`dicom_in_mrg_adt`.

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are sent in an outgoing ADT^A40^ADT_A39 message:

.. csv-table:: Supported segments of ADT^A40^ADT_A39 (HL7 v2.3.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH, Message Header, 2
   PID, Patient Identification, 3
   MRG, Merge Information, 3

.. csv-table:: Supported segments of ADT^A40^ADT_A39 (HL7 v2.5)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH, Message Header, R, [1..1], 2
   PID, Patient Identification, R, [1..1], 3
   MRG, Merge Information, R, [1..1], 3

Expected Actions
^^^^^^^^^^^^^^^^
HL7 Application/Receiver should have the ability to receive and process the demographics of merged patients, as well as
related information. Field *MSH-9 Message Type* is valued as **ADT^A40^ADT_A39**.
It is expected that after receiving a Patient Merge message (A40) the receiving system will perform updates to reflect
the fact that two patient records have been merged into a single record. If the correct target patient was not known to
the receiving system, it is expected that the receiving system will create a patient record using the patient identifiers
and demographics from the available PID segment data.

If the receiving application is an Image Manager/Image Archive, it is the responsibility of the Image Manager and the
Report Manager to ensure that the patient information has been updated in the diagnostic reports and evidence objects
(e.g., images, Key Image Notes, Grayscale Softcopy Presentation States, Evidence Documents, etc.) they manage when they
are retrieved.

.. _adt_out_a47:

ADT/ACK - Change Patient Identifier List (Event A47)
----------------------------------------------------
Supported HL7 version: 2.5

Trigger Event
^^^^^^^^^^^^^
This message is sent when an patient ID of an existing patient is updated in the archive by using archive UI and RESTful
service. Patient IDs and other Patient Information for the Patient record with changed patient identifiers are sent in
the PID segment of the outgoing ADT message mapped from corresponding DICOM attributes as defined in
:ref:`dicom_in_pid_adt`.

Supported Segments
^^^^^^^^^^^^^^^^^^
Same as specified in :numref:`adt_in_a28_segments`.

Expected Actions
^^^^^^^^^^^^^^^^
HL7 Application/Receiver should have the ability to receive and process the change in patient identifiers list of a patient.
Field *MSH-9 Message Type* is valued as **ADT^A47^ADT_A30**.
It is expected that the receiver shall change the patient identifiers list of an existing patient record for the patient
identified by the Patient ID (defined by the field PID-3).

.. _adt_out_segments:

Outbound Message Segments
=========================

.. _adt_out_pid:

PID - Patient Identification Segment
------------------------------------
.. csv-table:: PID - Patient Identification segment (HL7 v2.5)
   :name: tab_pid_251
   :header: SEQ, LEN, DT, Usage, Card., TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1, 4, SI, O, [0..1], , 00104, Set ID - PID
   2, 20, CX, O, [0..0], , 00105, Patient ID
   3, 250, CX, R, [1..*], , 00106, **Patient Identifier List**
   4, 20, CX, O, [0..0], , 00107, Alternate Patient ID - PID
   5, 250, XPN, R, [1..*], , 00108, **Patient Name**
   6, 250, XPN, O, [0..1], , 00109, **Mother’s Maiden Name**
   7, 26, TS, CE, [0..1], , 00110, **Date/Time of Birth**
   8, 1, IS, CE, [1..1], 0001, 00111, **Administrative Sex**
   9, 250, XPN, O, [0..1], , 00112, Patient Alias
   10, 250, CE, O, [0..1], 0005, 00113, Race
   11, 250, XAD, CE, [0..*], , 00114, Patient Address
   12, 4, IS, X, [0..1], 0289, 00115, County Code
   13, 250, XTN, O, [0..*], , 00116, Phone Number - Home
   14, 250, XTN, O, [0..*], , 00117, Phone Number - Business
   15, 250, CE, O, [0..1], 0296, 00118, Primary Language
   16, 250, CE, O, [0..1], 0002, 00119, Marital Status
   17, 250, CE, O, [0..1], 0006, 00120, Religion
   18, 250, CX, C, [0..1], , 00121, Patient Account Number
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
   35, 250, CE, CE, [0..1], 0446, 01539, Species Code
   36, 250, CE, C, [0..1], 0447, 01540, Breed Code
   37, 80, ST, O, [0..1], , 01541, Strain
   38, 250, CE, O, [0..2], , 01542, Production Class Code
   39, 250, CWE, O, [0..*], , 01840, Tribal Citizenship

Element names in **bold** indicates that the field is sent by |product|.

.. _adt_out_mrg:

MRG - Merge Segment
-------------------
.. csv-table:: MRG - Merge segment (HL7 v2.5)
   :header: SEQ, LEN, DT, Usage, Card., TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1, 250, CX, R, [1..*], , 00211, **Prior Patient Identifier List**
   2, 250, CX, X, [0..0], , 00212, Prior Alternate Patient ID
   3, 250, CX, O, [0..1], , 00213, Prior Patient Account Number
   4, 250, CX, X, , [0..0], 00214, Prior Patient ID
   5, 250, CX, X, [0..0], , 01279, Prior Visit Number
   6, 250, CX, X, [0..0], , 01280, Prior Alternate Visit ID
   7, 250, XPN, O, [0..*], , 01281, **Prior Patient Name**

Element names in **bold** indicates that the field is sent by |product|.


.. _adt_out_dicom:

DICOM to HL7 ADT Mapping
========================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component/sub-component value is not listed if the HL7 element should not contain multiple components/sub-components.

.. csv-table:: DICOM Patient Attributes to HL7 ADT mapping of PID segment
   :name: dicom_in_pid_adt
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :ref:`tab_hl7_dicom_charset`
   **Patient Identification**
   Patient's Name, "(0010, 0010)", Patient  Name, 00108, PID:5
   Patient ID, "(0010, 0020)", Patient Identifier List, 00106.1, PID:3.1
   Issuer of Patient ID, "(0010, 0021)", Patient Identifier List, 00106.4.1, PID:3.4.1
   Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >Item, "(FFFE, E000)"
   >Universal Entity ID, "(0040, 0032)", Patient Identifier List, 00106.4.2, PID:3.4.2
   >Universal Entity ID Type, "(0040, 0033)", Patient Identifier List, 00106.4.3, PID:3.4.3
   Patient's Mother's Birth Name, "(0010, 1060)", Mother’s Maiden Name, 00109, PID:6
   **Patient Demographic**
   Patient's Birth Date, "(0010, 0030)", Date/Time of Birth, 00110, PID:7
   Patient's Sex, "(0010, 0040)", Administrative Sex, 00111.1, PID:8.1
   **Patient Medical**
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2

.. csv-table:: HL7 ADT mapping of MRG segment to DICOM Patient Attributes
   :name: dicom_in_mrg_adt
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :ref:`tab_hl7_dicom_charset`
   **Patient Identification**
   Patient's Name, "(0010, 0010)", Prior Patient  Name, 01281, MRG:7
   Patient ID, "(0010, 0020)", Prior Patient Identifier List, 00211.1, MRG:1.1
   Issuer of Patient ID, "(0010, 0021)", Prior Patient Identifier List, 00211.1.1, MRG:1.1.1
   Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >Universal Entity ID, "(0040, 0032)", Prior Patient Identifier List, 00211.1.2, MRG:1.1.2
   >Universal Entity ID Type, "(0040, 0033)", Prior Patient Identifier List, 00211.1.3, MRG:1.1.3
