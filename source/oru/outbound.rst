Outbound
########

The Unsolicited Observation HL7 message is sent to other HL7 applications/receivers if HL7 receivers are to be
`notified about the availability of the imaging results <https://www.ihe.net/uploadedFiles/Documents/Radiology/IHE_RAD_Suppl_EBIW.pdf#page=71>`_.
This notification can be triggered on receive of studies either by :

- DICOM Studies stored conforming to `Storage AE Specification <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/specs/storage/storage.html>`_
- DICOM Studies / Bulkdata stored using `STOW-RS Services <https://petstore.swagger.io/index.html?url=https://raw.githubusercontent.com/dcm4che/dcm4chee-arc-light/master/dcm4chee-arc-ui2/src/swagger/openapi.json#/STOW-RS>`_
- `Reports stored by HL7 ORU <https://dcm4chee-arc-hl7cs.readthedocs.io/en/latest/oru/inbound.html>`_

.. _oru_out_messages:

Outbound Messages
=================

.. _oru_out_oru_r01:

ORU - Unsolicited Observation Message (Event R01)
-------------------------------------------------
Supported HL7 version: 2.5.1 (RAD-128)

Trigger Event
^^^^^^^^^^^^^
This message is sent out by the archive :

- when study received by the archive and notification about availability of imaging results is configured.

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are sent in an outgoing ORU^R01^ORU_R01 message:

.. csv-table:: Supported segments of ORU^R01^ORU_R01 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_out_251`, Patient Identification, R, [1..1], 3
   PV1 - :ref:`tab_pv1_oru_251`, Patient Visit, R, [1..1], 3
   ORC - :ref:`tab_orc_oru_251`, Common Order, O, [1..1], 4
   TQ1 - :ref:`tab_tq1_oru_251`, Timing and Quantity, R, [1..1], 4
   OBR - :ref:`tab_obr_oru_251`, Order Request Segment, R, [1..1], 7
   OBX - :ref:`tab_obx_oru_251`, Observation Result Segment, R, [1..1], 7

Expected Actions
^^^^^^^^^^^^^^^^
The Receiver shall accept and process the message.The Receiver shall support receiving multiple imaging result messages
for the same DICOM Study Instance UID. That is, multiple imaging Series may each result in a separate notification message
despite being part of a single DICOM Study. Receiver actions subsequent to receiving an image result will depend on
internal business logic and/or the profile in which the transaction is being performed.

.. _oru_out_segments:

Outbound Message Segments
=========================

.. _oru_out_msh:

MSH - Message Header segment
----------------------------
Same as specified in :ref:`tab_msh_251`

.. _oru_out_pid:

PID - Patient Identification segment
------------------------------------

Same as specified in :ref:`tab_pid_out_251`

.. _oru_out_pv1:

PV1 - Patient Visit segment
---------------------------

.. csv-table:: PV1 - Patient Visit segment (HL7 v2.5.1)
   :name: tab_pv1_oru_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 01627, Set ID - PV1
   2, 1, IS, R, , 00132, **Patient Class**
   3, 80, PL, C, , 00133, Assigned Patient Location
   4, 2, IS, O, 0007, 00134, Admission Type
   5, 20, CX, O, , 00135, Preadmit Number
   6, 80, PL, O, , 00136, Prior Patient Location
   7, 60, XCN, C, 0010, 00137, Attending Doctor
   8, 60, XCN, C, 0010, 00138, Referring Doctor
   9, 60, XCN, R2, 0010, 00139, Consulting Doctor
   10, 3, IS, C, 0069, 00140, Hospital Service
   11, 80, PL, O, , 00141, Temporary Location
   12, 2, IS, O, 0087, 00142, Preadmit Test Indicator
   13, 2, IS, O, 0092, 00143, Readmission Indicator
   14, 3, IS, O, 0023, 00144, Admit Source
   15, 2, IS, C, 0009, 00145, Ambulatory Status
   16, 2 , IS, O, 0099, 00146, VIP Indicator
   17, 60, XCN, C, 0010, 00147, Admitting Doctor
   18, 2, IS, O, 0018, 00148, Patient Type
   19, 20, CX, C, , 00149, **Visit Number**
   20, 50, FC, O, 0064, 00150, Financial Class
   21, 2, IS, O, 0032, 00151, Charge Price Indicator
   22, 2, IS, O, 0045, 00152, Courtesy Code
   23, 2, IS, O, 0046, 00153, Credit Rating
   24, 2, IS, O, 0044, 00154, Contract Code
   25, 8, DT, O, , 00155, Contract Effective Date
   26, 12, NM, O, , 00156, Contract Amount
   27, 3, NM, O, , 00157, Contract Period
   28, 2, IS, O, 0073, 00158, Interest Code
   29, 1, IS, O, 0110, 00159, Transfer to Bad Debt Code
   30, 8, DT, O, , 00160, Transfer to Bad Debt Date
   31, 10, IS, O, 0021, 00161, Bad Debt Agency Code
   32, 12, NM, O, , 00162, Bad Debt Transfer Amount
   33, 12, NM, O, , 00163, Bad Debt Recovery Amount
   34, 1, IS, O, 0111, 00164, Delete Account Indicator
   35, 8, DT, O, , 00165, Delete Account Date
   36, 3, IS, O, 0112, 00166, Discharge Disposition
   37, 25, CM, O, 0113, 00167, Discharge to Location
   38, 80, CE, O, 0114, 00168, Diet Type
   39, 2, IS, O, 0115, 00169, Servicing Facility
   40, 1, IS, O, 0116, 00170, Bed Status
   41, 2, IS, O, 0117, 00171, Account Status
   42, 80, PL, O, , 00172, Pending Location
   43, 80, PL, O, , 00173, Prior Temporary Location
   44, 26, TS, O, , 00174, Admit Date/Time
   45, 26, TS, O, , 00175, Discharge Date/Time
   46, 12, NM, O, , 00176, Current Patient Balance
   47, 12, NM, O, , 00177, Total Charges
   48, 12, NM, O, , 00178, Total Adjustments
   49, 12, NM, O, , 00179, Total Payments
   50, 20, CX, O, 0203, 00180, Alternate Visit ID
   51, 1, IS, C, 0326, 01226, **Visit Indicator**
   52, 60, XCN, O, 0010, 01224, Other Healthcare Provider

.. _oru_out_orc:

ORC - Order Control segment
---------------------------

.. csv-table:: ORC - Order Control segment (HL7 v2.5.1)
   :name: tab_orc_oru_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 2, ID, R, 0119, 00215, **Order Control**
   2, 22, EI, R, , 00216, **Placer Order Number**
   3, 22, EI, X, , 00217, **Filler Order Number**
   4, 22, EI, C, , 00218, Placer Group Number
   5, 2, ID, O, 0038, 00219, **Order Status**
   6, 1, ID, O, 0121, 00220, Response Flag
   7, 200, TQ, X, , 00221, Quantity/Timing
   8, 200, EIP, C, , 00222, Parent
   9, 26, TS, O, , 00223, Date/Time of Transaction
   10, 250, XCN, O, , 00224, Entered By
   11, 250, XCN, O, , 00225, Verified By
   12, 250, XCN, O, , 00226, Ordering Provider
   13, 80, PL, O, , 00227, Enterer's Location
   14, 250, XTN, O, , 00228, Callback Phone Number
   15, 26, TS, O, , 00229, Order Effective Date/Time
   16, 250, CE, O, , 00230, Order Control Code Reason
   17, 250, CE, O, , 00231, Entering Organization
   18, 250, CE, O, , 00232, Entering Device
   19, 250, XCN, O, , 00233, Action By
   20, 250, CE, O, 0339, 01310, Advanced Beneficiary Notice Code
   21, 250, XON, O, , 01311, Ordering Facility Name
   22, 250, XAD, O, , 01312, Ordering Facility Address
   23, 250, XTN, O, , 01313, Ordering Facility Phone Number
   24, 250, XAD, O, , 01314, Ordering Provider Address
   25, 250, CWE, O, , 01473, Order Status Modifier
   26, 60, CWE, C, 0552, 01641, Advanced Beneficiary Notice Override Reason
   27, 26, TS, O, , 01642, Filler's Expected Availability Date/Time
   28, 250, CWE, O, 0177, 00615, Confidentiality Code
   29, 250, CWE, O, 0482, 01643, Order Type
   30, 250, CNE, O, 0483, 01644, Enterer Authorization Mode
   31, 250, CWE, O, , 02286, Parent Universal Service Identifier

.. _oru_out_tq1:

TQ1 - Timing/Quantity segment
-----------------------------

.. csv-table:: TQ1 - Timing/Quantity segment (HL7 v2.5.1)
   :name: tab_tq1_oru_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 01627, Set ID - TQ1
   2, 20, CQ, O, , 01628, Quantity
   3, 540, RPT, O, 0335, 01629, Repeat Pattern
   4, 20, TM, O, , 01630, Explicit Time
   5, 20, CQ, O, , 01631, Relative Time and Units
   6, 20, CQ, O, , 01632, Service Duration
   7, 26, TS, R, , 01633, **Start Date/Time**
   8, 26, TS, O, , 01634, End Date/Time
   9, 250, CWE, O, 0485, 01635, **Priority**
   10, 250, TX, O, , 01636, Condition Text
   11, 250, TX, O, 0065, 01637, Text Instruction
   12, 10, ID, C, 0472, 01638, Conjunction
   13, 20, CQ, O, , 01639, Occurrence Duration
   14, 10, NM, O, , 01640, Total Occurrences

.. _oru_out_obr:

OBR - Observation Request segment
---------------------------------

.. csv-table:: OBR - Observation Request segment (HL7 v2.5.1)
   :name: tab_obr_oru_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 22, EI, R2, , 00216, **Placer Order Number**
   3, 22, EI, R2, , 00217, **Filler Order Number**
   4, 250, CE, R, , 00238, **Universal Service ID**
   5, 2, ID, X, , 00239, Priority (retired)
   6, 26, TS, X, , 00240, Requested Date/Time
   7, 26, TS, R, , 00241, **Observation Date/Time**
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 250, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 250, CE, X, , 00246, Danger Code
   13, 300, ST, C, , 00247, Relevant Clinical Info
   14, 26, TS, X, , 00248, Specimen Received Date/Time
   15, 300, SPS, X, 0070, 00249, Specimen Source
   16, 250, XCN, O, , 00226, Ordering Provider
   17, 250, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, R, , 00251, **Placer Field 1**
   19, 60, ST, R2, , 00252, **Placer Field 2**
   20, 60, ST, O, , 00253, Filler Field 1
   21, 60, ST, O, , 00254, Filler Field 2
   22, 26, TS, O, , 00255, Results Rpt/Status Chng - Date/Time
   23, 40, MOC, O, , 00256, Charge to Practice
   24, 10, ID, R, 0074, 00257, **Diagnostic Service Sect ID**
   25, 1, ID, R, 0123, 00258, **Result Status**
   26, 400, PRL, O, , 00259, Parent Result
   27, 200, TQ, R, , 00221, **Quantity/Timing**
   28, 250, XCN, O, , 00260, Result Copies To
   29, 200, EIP, C, , 00261, Parent
   30, 20, ID, O, 0124, 00262, Transportation Mode
   31, 250, CE, R2, , 00263, **Reason For Study**
   32, 200, NDL, R2, , 00264, Principal Result Interpreter
   33, 200, NDL, R2, , 00265, Assistant Result Interpreter
   34, 200, NDL, R2, , 00266, **Technician**
   35, 200, NDL, O, , 00267, Transcriptionist
   36, 26, TS, O, , 00268, Scheduled Date/Time
   37, 4, NM, O, , 01028, Number of Sample Containers
   38, 250, CE, O, , 01029, Transport Logistics of Collected Sample
   39, 250, CE, O, , 01030, Collector's Comment
   40, 250, CE, O, , 01031, Transport Arrangement Responsibility
   41, 30, ID, O, 0224, 01032, Transport Arranged
   42, 1, ID, O, 0225, 01033, Escort Required
   43, 250, CE, O, , 01034, Planned Patient Transport Comment
   44, 250, CE, R, 0088, 00393, **Procedure Code**
   45, 250, CE, O, 0340, 01036, Procedure Code Modifier
   46, 250, CE, O, 0411, 01474, Placer Supplemental Service Information
   47, 250, CE, O, 0411, 01475, Filler Supplemental Service Information
   48, 250, CWE, O, 0476, 01646, Medically Necessary Duplicate Procedure Reason
   49, 2, IS, O, 0507, 01647, Result Handling
   50, 250, CWE, O, , 02286, Parent Universal Service Identifier

.. _oru_out_obx:

OBX - Observation Result segment
--------------------------------

.. csv-table:: OBX - Observation Result segment (HL7 v2.5.1)
   :name: tab_obx_oru_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00569, **SetID - OBX**
   2, 2, ID, C, 0125, 00570, **Value Type**
   3, 250, CE, R, , 00571, **Observation Identifier**
   4, 20, ST, C, , 00572, Observation Sub-ID
   5, 99999^1, varies, C, , 00573, **Observation Value**
   6, 250, CE, O, , 00574, Units
   7, 60, ST, O, , 00575, References Range
   8, 5, IS, O, 0078, 00576, Abnormal Flags
   9, 5, NM, O, , 00577, Probability
   10, 2, ID, O, 0080, 00578, Nature of Abnormal Test
   11, 1, ID, R, 0085, 00579, **Observation Result Status**
   12, 26, TS, O, , 00580, Effective Date of Reference Range
   13, 20, ST, O, , 0581, User Defined Access Checks
   14, 26, TS, O, , 00582, Date/Time of Observation
   15, 250, CE, O, , 00583, Producer's ID
   16, 250, XCN, O, , 00584, Responsible Observer
   17, 250, CE, O, , 00936, Observation Method
   18, 22, EI, O, , 01479, Equipment Instance Identifier
   19, 26, TS, O, , 01480, Date/Time of the Analysis

Element names in **bold** indicates that the field is used by |product|.

.. _oru_out_dicom:

DICOM to HL7 Unsolicited Observation Message Mapping
====================================================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component/sub-component value is not listed if the HL7 element should not contain multiple components/sub-components.

.. _oru_out_oru_r01_dicom:

ORU - DICOM Image Attributes to HL7 Unsolicited Observation Message mapping
---------------------------------------------------------------------------

.. csv-table:: DICOM Modality Worklist Attributes to HL7 Unsolicited Observation Message mapping
   :name: dicom_to_oru
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :ref:`tab_hl7_dicom_charset`
   Patient's Name, "(0010, 0010)", Patient  Name, 00108, PID:5
   Patient ID, "(0010, 0020)", Patient Identifier List, 00106.1, PID:3.1
   Issuer of Patient ID, "(0010, 0021)", Patient Identifier List, 00106.4.1, PID:3.4.1
   Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)"
   >Item, "(FFFE, E000)"
   >Universal Entity ID, "(0040, 0032)", Patient Identifier List, 00106.4.2, PID:3.4.2
   >Universal Entity ID Type, "(0040, 0033)", Patient Identifier List, 00106.4.3, PID:3.4.3
   Patient's Birth Date, "(0010, 0030)", Date/Time of Birth, 00110, PID:7
   Patient's Sex, "(0010, 0040)", Administrative Sex, 00111.1, PID:8.1
   Route of Admissions, "(0038, 0016)", Patient Class, 00132, PV1:2, [#Note9]_
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.4.1, PV1:19.4.1
   >Universal Entity ID, "(0040, 0032)", Visit Number, 00149.4.2, PV1:19.4.2
   >Universal Entity ID Type, "(0040, 0033)", Visit Number, 00149.4.3, PV1:19.4.3
   , , Visit Indicator, 01226, PV1:51, Set to V
   , , Order Control, 00215, ORC:1, Set to SC
   , , Order Status, 00219, ORC:5, Set to CM
   , , Start Date/Time, 01633, TQ1:7, [#Note1]_
   , , Start Date/Time, 01633, TQ1:7, [#Note1]_
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18
   Issuer of Accession Number Sequence, "(0008, 0051)", Placer Field 2 #, 00252, OBR:19, [#Note8]_
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2
   >Universal Entity ID, "(0040, 0032)", Placer Order #, 00216.3, ORC:2.3
   >Universal Entity ID Type, "(0040, 0033)", Placer Order #, 00216.4, ORC:2.4
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2
   >Universal Entity ID, "(0040, 0032)", Filler Order #, 00217.3, ORC:3.3
   >Universal Entity ID Type, "(0040, 0033)", Filler Order #, 00217.4, ORC:3.4
   , , Priority, 01635, TQ1:9, Set to R^Routine^HL70078
   , , Quantity/Timing, 00221, OBR:27, Set to ^^^^^R
   , , Universal Service ID, 00238, OBR:4, [#Note2]_
   , , Observation Date/Time, 00241, OBR:7, [#Note3]_
   Institutional Department Type Code Sequence, "(0008, 1041)"
   >Code Value, "(0008, 0100)", Diagnostic Service Sect ID #, 00257, OBR:24, [#Note7]_
   , , Result Status, 00258, OBR:25, Set to R
   , , Reason For Study, 00263, OBR:31, [#Note4]_
   , , Technician, 00266, OBR:34, [#Note5]_
   , , Procedure Code, 00393, OBR:44, [#Note6]_
   , , SetID - OBX, 00569, OBX:1, Set to 1
   , , Value Type, 00570, OBX:2, Set to ST
   , , Observation Identifier, 00571, OBX:3, Set to 113014^DICOM Study^DCM
   Study Instance UID, "(0020, 000D)", Observation Value, 00573, OBX:5
   , , Observation Result Status, 00579, OBX:11, Set to O


.. [#Note1] This value is populated from the created time of the task. (The `task` here refers to a task created in
    database for sending out the HL7 notification.)

.. [#Note2] This field shall contain a procedure code in the first three components:
    OBR-4.1 Identifier, OBR-4.2 text code meaning, OBR-4.3 coding system. The use of codes from a standardized coding
    system for procedures, such as the RadLex Playbook LOINC codes, is 1385 recommended. In order of preference,
    the procedure code may be taken from:
    - Procedure Code Sequence (0008,1032)
    - Requested Procedure Code Sequence (0032,1064)

.. [#Note3] Observation Date/Time shall contain a date/time representative of the imaging procedure. When choosing
    the date/time to use, consider that an EMR might use this date/time to find other clinical entries for the patient
    at or near this time which might provide context for the imaging procedure. The date/time might be taken from one of
    the following attributes in the associated DICOM image objects:
    - Study Date (0008,0020) & Study Time (0008,0030)
    - Series Date (0008,0021) & Series Time (0008,0031)

.. [#Note4] This field shall be valued, if known. This might be taken from one of the following attributes in the
    associated DICOM image objects:
    - Reason for Performed Procedure Code Sequence (0040,1012)
    - Reason for the Requested Procedure (0040,1002) or Code Sequence (0040,100A)1425
    - Reason for Visit (0032,1066) or Code Sequence (0032,1066)
    - Admitting Diagnoses Description (0008,1080) or Code Sequence (0008,1084)

.. [#Note5] This field shall be valued, if the person who acquired the images is known. This might be taken from one of
    the following attributes in the associated DICOM image objects:
    - Operators' Name (0008,1070) or Operator Identification Sequence (0008,1072)
    - Performing Physician's Name (0008,1050) or Performing Physician Identification Sequence (0008,1052)

.. [#Note6] Procedure Code shall match OBR-4.

.. [#Note7] Value set to `RAD` as fallback, if no `Institutional Department Type Code Sequence (0008, 1041)` found in
    the object's attributes.

.. [#Note8] As `Encoding of Assigning Authority in ST data field OBR-19 needs to be clarified <https://groups.google.com/forum/#!topic/ihe-rad-tech/rgijW_U-1a8>`_
    and `it has been captured into Change Proposals <https://groups.google.com/forum/#!topic/ihe-rad-tech/IEDUwf4JGD8>`_,
    temporarily the `HL7v2 Hierarchic Designator Macro Attributes <http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_10.14.html#table_10-17>`_
    of `Issuer Of Accession Number Sequence` have been encoded as per
    `Example 3: ISO OID encoded in an ST subcomponent <http://hl7.eu/refactored/dtST.html>`_

.. [#Note9] Route of Admissions (0038, 0016) DICOM attribute, if present, shall be mapped to PV1:2. If this DICOM attribute
   is absent, default "U" (denoting Patient Class as Unknown) shall be used.