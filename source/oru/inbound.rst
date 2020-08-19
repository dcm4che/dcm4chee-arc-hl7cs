Inbound
#######

.. _oru_in_messages:

Inbound Messages
================

.. _oru_in_r01:

ORU - Unsolicited Observation Result Message (Event R01)
--------------------------------------------------------
Supported HL7 version: 2.3.1 (RAD-28), 2.5.1 (RAD-128)

Trigger Event
^^^^^^^^^^^^^
When DICOM Structured Reports are verified and finalized by the Report Manager, the Report Manager sends unsolicited
ORU transactions to the Image Manager. Image Manager creates/updates the study with the referenced Structured Report in
the HL7 message.

.. _oru_in_segments:

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ORU^R01^ORU_R01 message:

.. csv-table:: Supported segments of ORU^R01^ORU_R01 (HL7 versions 2.3.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH - :ref:`tab_msh_231`, Message Header, 2
   PID - :ref:`tab_pid_231`, Patient Identification, 3
   PV1 - :ref:`tab_pv1_231`, Patient Visit, 3
   OBR - :ref:`tab_obr_231_oru`, Order Detail, 4
   OBX - :ref:`tab_obx_231`, Observation Results, 7

.. csv-table:: Supported segments of ORU^R01^ORU_R01 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   PV1 - :ref:`tab_pv1_251`, Patient Visit, O, [0..1], 3
   OBR - :ref:`tab_obr_251_oru`, Order Detail, R, [1..*], 4
   OBX - :ref:`tab_obx_251`, Order Detail, R, [1..*], 4

Performed Actions
^^^^^^^^^^^^^^^^^
Patient Demographic Information are extracted from the PID segment of the received message and mapped into corresponding
DICOM attributes as defined in :ref:`adt_in_pid_dicom`. If a Patient record with the extracted primary Patient ID
already exists in the database, that Patient record will get updated. If there is no such Patient record a new Patient
record will be inserted into the database [#hl7NoPatientCreateMessageType]_.
Based on the information received in the OBR and OBX segments, a SR object is stored to the study.

.. [#hl7NoPatientCreateMessageType] The creation of new Patient records will be suppressed for message types which are
   listed by configuration parameter *HL7 No Patient Create Message Type(s)*  of |product|.

.. _oru_segments:

Inbound Message Segments
========================

.. _oru_in_msh:

MSH - Message Header segment
----------------------------
Same as specified in :ref:`tab_msh_231` or :ref:`tab_msh_251`

.. _oru_in_pid:

PID - Patient Identification segment
------------------------------------
Same as specified in :ref:`tab_pid_231` or :ref:`tab_pid_251`

.. _oru_in_obr:

OBR - Observation Request segment
---------------------------------
.. csv-table:: OBR - Observation Request segment (HL7 v2.3.1)
   :name: tab_obr_231_oru
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 75, EI, R, , 00216, **Placer Order Number**
   3, 75, EI, O, , 00217, **Filler Order Number**
   4, 200, CE, R, , 00238, **Universal Service ID**
   5, 2, ID, O, , 00239, Priority
   6, 26, TS, O, , 00240, Requested Date/Time
   7, 26, TS, O, , 00241, **Observation Date/Time**
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 60, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 60, CE, R2, , 00246, Danger Code
   13, 300, ST, C, , 00247, Relevant Clinical Info
   14, 26, TS, O, , 00248, Specimen Received Date/Time
   15, 300, CM, C, 0070, 00249, Specimen Source
   16, 80, XCN, R, , 00226, Ordering Provider
   17, 40, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, O, , 00251, **Placer Field 1**
   19, 60, ST, O, , 00252, Placer Field 2
   20, 60, ST, O, , 00253, Filler Field 1
   21, 60, ST, O, , 00254, Filler Field 2
   22, 26, TS, O, , 00255, Results Rpt/Status Chng - Date/Time
   23, 40, CM, O, , 00256, Charge to Practice
   24, 10, ID, O, 0074, 00257, Diagnostic Service Sect ID
   25, 1, ID, O, 0123, 00258, **Result Status**
   26, 400, CM, O, , 00259, Parent Result
   27, 200, TQ, R, , 00221, Quantity/Timing
   28, 150, XCN, O, , 00260, Result Copies To
   29, 150, CM, C, , 00261, Parent
   30, 20, ID, R2, 0124, 00262, Transportation Mode
   31, 300, CE, R2, , 00263, Reason For Study
   32, 200, CM, O, , 00264, **Principal Result Interpreter**
   33, 200, CM, O, , 00265, Assistant Result Interpreter
   34, 200, CM, O, , 00266, Technician
   35, 200, CM, O, , 00267, Transcriptionist
   36, 26, TS, O, , 00268, Scheduled Date/Time
   37, 4, NM, O, , 01028, Number of Sample Containers
   38, 60, CE, O, , 01029, Transport Logistics of Collected Sample
   39, 200, CE, O, , 01030, Collector's Comment
   40, 60, CE, O, , 01031, Transport Arrangement Responsibility
   41, 30, ID, R2, 0224, 01032, Transport Arranged
   42, 1, ID, O, 0225, 01033, Escort Required
   43, 200, CE, O, , 01034, Planned Patient Transport Comment
   44, 80, CE, O, 0088, 00393, Procedure Code
   45, 80, CE, O, 0340, 01036, Procedure Code Modifier

.. csv-table:: OBR - Observation Request segment (HL7 v2.5.1)
   :name: tab_obr_251_oru
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 22, EI, R, , 00216, **Placer Order Number**
   3, 22, EI, O, , 00217, **Filler Order Number**
   4, 250, CE, R, , 00238, **Universal Service ID**
   5, 2, ID, O, , 00239, Priority
   6, 26, TS, O, , 00240, Requested Date/Time
   7, 26, TS, O, , 00241, **Observation Date/Time**
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 250, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 250, CE, R2, , 00246, Danger Code
   13, 300, ST, C, , 00247, Relevant Clinical Info
   14, 26, TS, X, , 00248, Specimen Received Date/Time
   15, 300, SPS, X, 0070, 00249, Specimen Source
   16, 250, XCN, R, , 00226, Ordering Provider
   17, 250, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, O, , 00251, **Placer Field 1**
   19, 60, ST, O, , 00252, Placer Field 2
   20, 60, ST, O, , 00253, Filler Field 1
   21, 60, ST, O, , 00254, Filler Field 2
   22, 26, TS, O, , 00255, Results Rpt/Status Chng - Date/Time
   23, 40, MOC, O, , 00256, Charge to Practice
   24, 10, ID, O, 0074, 00257, Diagnostic Service Sect ID
   25, 1, ID, O, 0123, 00258, **Result Status**
   26, 400, PRL, O, , 00259, Parent Result
   27, 200, TQ, X, , 00221, Quantity/Timing
   28, 250, XCN, O, , 00260, Result Copies To
   29, 200, EIP, C, , 00261, Parent
   30, 20, ID, R2, 0124, 00262, Transportation Mode
   31, 250, CE, R2, , 00263, Reason For Study
   32, 200, NDL, O, , 00264, **Principal Result Interpreter**
   33, 200, NDL, O, , 00265, Assistant Result Interpreter
   34, 200, NDL, O, , 00266, Technician
   35, 200, NDL, O, , 00267, Transcriptionist
   36, 26, TS, O, , 00268, Scheduled Date/Time
   37, 4, NM, O, , 01028, Number of Sample Containers
   38, 250, CE, O, , 01029, Transport Logistics of Collected Sample
   39, 250, CE, O, , 01030, Collector's Comment
   40, 250, CE, O, , 01031, Transport Arrangement Responsibility
   41, 30, ID, R2, 0224, 01032, Transport Arranged
   42, 1, ID, O, 0225, 01033, Escort Required
   43, 250, CE, O, , 01034, Planned Patient Transport Comment
   44, 250, CE, O, 0088, 00393, Procedure Code
   45, 250, CE, O, 0340, 01036, Procedure Code Modifier
   46, 250, CE, R2, 0411, 01474, Placer Supplemental Service Information
   47, 250, CE, R2, 0411, 01475, Filler Supplemental Service Information
   48, 250, CWE, R2, 0476, 01646, Medically Necessary Duplicate Procedure Reason
   49, 2, IS, O, 0507, 01647, Result Handling
   50, 250, CWE, O, , 02286, Parent Universal Service Identifier


.. _oru_in_obx:

OBX - Observation Request segment
---------------------------------
.. csv-table:: OBX - Observation/Result segment (HL7 v2.3.1)
   :name: tab_obx_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00569, SetID - OBX
   2, 3, ID, C, 0125, 00570, Value Type
   3, 80, CE, R, , 00571, **Observation Identifier**
   4, 20, ST, C, , 00572, Observation Sub-ID
   5, 65536³, *, C, , 00573, **Observation Value**
   6, 60, CE, O, , 00574, Units
   7, 60, ST, O, , 00575, References Range
   8, 5, ID, O, 0078, 00576, Abnormal Flags
   9, 5, NM, O, , 00577, Probability
   10, 2, ID, O, 0080, 00578, Nature of Abnormal Test
   11, 1, ID, R, 0085, 00579, Observation Result Status
   12, 26, TS, O, , 00580, Date Last Obs Normal Values
   13, 20, ST, O, , 00581, User Defined Access Checks
   14, 26, TS, O, , 00582, Date/Time of the Observation
   15, 60, CE, O, , 00583, Producer's ID
   16, 80, XCN, O, , 00584, Responsible Observer
   17, 60, CE, O, , 00936, Observation Method

.. csv-table:: OBX - Observation/Result segment (HL7 v2.5.1)
   :name: tab_obx_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00569, SetID - OBX
   2, 2, ID, C, 0125, 00570, Value Type
   3, 250, CE, R, , 00571, **Observation Identifier**
   4, 20, ST, C, , 00572, Observation Sub-ID
   5, 99999, Varies, C, , 00573, **Observation Value**
   6, 250, CE, O, , 00574, Units
   7, 60, ST, O, , 00575, References Range
   8, 5, IS, O, 0078, 00576, Abnormal Flags
   9, 5, NM, O, , 00577, Probability
   10, 2, ID, O, 0080, 00578, Nature of Abnormal Test
   11, 1, ID, R, 0085, 00579, Observation Result Status
   12, 26, TS, O, , 00580, Effective Date of Reference Range
   13, 20, ST, O, , 00581, User Defined Access Checks
   14, 26, TS, O, , 00582, Date/Time of the Observation
   15, 250, CE, O, , 00583, Producer's ID
   16, 250, XCN, O, , 00584, Responsible Observer
   17, 250, CE, O, , 00936, Observation Method
   18, 22, EI, O, , 01479, Equipment Instance Identifier
   19, 26, TS, O, , 01480, Date/Time of Analysis

Element names in **bold** indicates that the field is used by |product|.

.. _oru_in_dicom:

HL7 ORU to DICOM SR Mapping
===========================

Inverse of the mapping specified by `IHE Transaction Structured Report Export [RAD-28] <http://ihe.net/uploadedFiles/Documents/Radiology/IHE_RAD_TF_Vol2.pdf#page=283>`_
has been used.

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. csv-table:: HL7 ORU to DICOM Structured Report Attributes mapping
   :name: oru_obr_obx_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Notes/Default values

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note9]_
   **Patient Identification**
   Same as Patient Identification in :ref:`adt_in_pid_dicom`
   **Structured Report Export**
   Content Date, "(0008, 0023)", Observation Date/Time, 00241, OBR:7
   Content Time, "(0008, 0033)", Observation Date/Time, 00241, OBR:7
   Accession Number, "(0008, 0050)", Placer field 1, 00251, OBR:18
   SOP Class UID, "(0008, 0016)",,,, 1.2.840.10008.5.1.4.1.1.88.11
   Modality, "(0008, 0060)",,,, SR
   SOP Instance UID, "(0008, 0018)",,, OBX[1]:5, [#Note6]_
   Study Instance UID, "(0020, 000D)",,, OBX[2]:5, [#Note4]_
   Series Instance UID, "(0020, 000E)",,, OBX[3]:5, [#Note5]_
   Instance Number, "(0020, 0013)",,,, 1
   Value Type, "(0040, A040)",,,, CONTAINER
   Continuity Of Content, "(0040, A050)",,,, SEPARATE
   Concept Name Code Sequence, "(0040, A043)"
   >>Code Value, "(0008, 0100)",,,, 11528-7
   >>Code Scheme Designator, "(0008, 0102)",,,, LN
   >>Code Meaning, "(0008, 0104)",,,, Radiology Report
   Verifying Observer Sequence, "(0040, A073)"
   >Verifying Organization, "(0040, A027)",,,, Default Value : Verifying Organization
   >Verifying Observer Name, "(0040, A075)", Principal Result Interpreter, 00264, OBR:32.1, [#Note8]_
   >Verification DateTime, "(0040, A030)", Observation Date/Time, 00241, OBR:7
   Referenced Request Sequence, "(0040, A370)"
   >Study Instance UID, "(0020, 000D)",,, OBX[1]:5, [#Note4]_
   >Requested Procedure Description, "(0032, 1060)", Universal Service ID, 00238, OBR:4.2
   >Requested Procedure Code Sequence, "(0032, 1064)", Universal Service ID
   >>Code Value, "(0008, 0100)",, 00238.1, OBR:4.1
   >>Code Scheme Designator, "(0008, 0102)",, 00238.3, OBR:4.3
   >>Code Meaning, "(0008, 0104)",, 00238.2, OBR:4.2
   >Placer Order Number Imaging Service Request, "(0040, 2016)", Placer Order Number, 00216, OBR:2, [#Note7]_
   >Filler Order Number Imaging Service Request, "(0040, 2017)", Filler Order Number, 00217, OBR:3, [#Note7]_
   Completion Flag, "(0040, A491)", Result Status, 00258, OBR:25, [#Note1]_
   Verification Flag, "(0040, A493)", Result Status, 00258, OBR:25, [#Note2]_
   Content Sequence, "(0040, A730)",,,, [#Note3]_
   Item 1
   >Relationship Type, "(0040, A010)",,,, HAS CONCEPT MOD
   >Value Type, "(0040, A040)",,,, CODE
   >Concept Name Code Sequence, "(0040, A043)"
   >>Code Value, "(0008, 0100)",,,, 121049
   >>Code Scheme Designator, "(0008, 0102)",,,, DCM
   >>Code Meaning, "(0008, 0104)",,,, Language of Content Item and Descendants
   >Concept Code Sequence, "(0040, A168)"
   >>Code Value, "(0008, 0100)",,,, eng
   >>Code Scheme Designator, "(0008, 0102)",,,, ISO639_2
   >>Code Meaning, "(0008, 0104)",,,, English
   Item 2
   >Relationship Type, "(0040, A010)",,,, HAS OBS CONTEXT
   >Value Type, "(0040, A040)",,,, PNAME
   >Concept Name Code Sequence, "(0040, A043)"
   >>Code Value, "(0008, 0100)",,,, 121008
   >>Code Scheme Designator, "(0008, 0102)",,,, DCM
   >>Code Meaning, "(0008, 0104)",,,, Person Observer Name
   >Person Name, "(0040, A123)", Principal Result Interpreter, 00264, OBR:32.1
   Item 3
   >Relationship Type, "(0040, A010)",,,, HAS OBS CONTEXT
   >Value Type, "(0040, A040)",,,, CODE
   >Concept Name Code Sequence, "(0040, A043)"
   >>Code Value, "(0008, 0100)",,,, 121023
   >>Code Scheme Designator, "(0008, 0102)",,,, DCM
   >>Code Meaning, "(0008, 0104)",,,, Procedure Code
   >Concept Code Sequence, "(0040, A168)"
   >>Code Value, "(0008, 0100)",, 00238.1, OBR:4.1
   >>Code Scheme Designator, "(0008, 0102)",, 00238.3, OBR:4.3
   >>Code Meaning, "(0008, 0104)",, 00238.2, OBR:4.2
   Item 4
   >Relationship Type, "(0040, A010)",,,, CONTAINS
   >Value Type, "(0040, A040)",,,, CONTAINER
   >Concept Name Code Sequence, "(0040, A043)"
   >>Code Value, "(0008, 0100)",,,, 121070
   >>Code Scheme Designator, "(0008, 0102)",,,, DCM
   >>Code Meaning, "(0008, 0104)",,,, Findings
   >Continuity Of Content, "(0040, A050)",,,, SEPARATE
   >Content Sequence, "(0040, A730)"
   >>Relationship Type, "(0040, A010)",,,, CONTAINS
   >>Value Type, "(0040, A040)",,,, TEXT
   >>Concept Name Code Sequence, "(0040, A043)"
   >>>Code Value, "(0008, 0100)",,,, 121071
   >>>Code Scheme Designator, "(0008, 0102)",,,, DCM
   >>>Code Meaning, "(0008, 0104)",,,, Finding
   >>Text Value, "(0040, A160)",,, OBX:3/component='SR Text'


.. [#Note1] If the value of this field is P, then CompletionFlag is set to PARTIAL. In all other cases it is set to COMPLETE

.. [#Note2] If the value of this field is P or F, then VerificationFlag is set to VERIFIED. In all other cases it is set to UNVERIFIED

.. [#Note3] This sequence is present only if Field 32 (i.e. Principal Result Interpreter) is present in OBR segment.

.. [#Note4] If OBX field[3] component is Study Instance UID, then value is taken from OBX:5; else value is system generated.

.. [#Note5] If OBX field[3] component is Series Instance UID, then value is taken from OBX:5; else value is system generated.

.. [#Note6] If OBX field[3] component is SR Instance UID, then value is taken from OBX:5; else value is system generated.

.. [#Note7] If the Placer and/or Filler order number are not provided by the Referenced Request Sequence, it is assumed that the
    Report Manager is able to obtain values.

.. [#Note8] If absent "UNKNOWN" is used.

.. [#Note9] `HL7 DICOM Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7dicomcharacterset>`_
   if configured, is selected to specify Specific Character Set. Else, MSH-18 if present in the incoming HL7 message, :ref:`tab_hl7_dicom_charset` 
   is selected to specify Specific Character Set. If MSH-18 is absent, then
   `HL7 Default Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7Application.html#hl7defaultcharacterset>`_
   is selected to specify Specific Character Set.