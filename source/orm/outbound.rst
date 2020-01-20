Outbound
########

The General Clinical Order Message HL7 message is sent to other HL7 applications/receivers if the feature
Synchronize External Receivers explained in https://github.com/dcm4che/dcm4chee-arc-light/wiki/Requested-Procedures
has been configured in the archive.

.. _orm_out_messages:

Outbound Messages
=================

.. _orm_out_omg_o19:

OMG - General Clinical Order Message (Event O19)
------------------------------------------------
Supported HL7 version: 2.5.1

Trigger Event
^^^^^^^^^^^^^
This message is sent out by the archive :
- when study received by the archive has modality worklist entries referencing it in the archive
- on receive of MPPS (Modality Performed Procedure Step).

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are sent in an outgoing OMG^O19^OMG_O19 message:

.. csv-table:: Supported segments of ADT^A28^ADT_A05 (HL7 v2.5)
   :header: Segment, Meaning, Usage, HL7 chapter
   :widths: 15, 40, 15, 15

   MSH, Message Header, R, 2
   PID - :ref:`tab_pid_omg_251`, Patient Identification, O, 3
   PV1 - :ref:`tab_pv1_251`, Patient Visit, O, 3
   ORC - :ref:`tab_orc_251`, Common Order, R, 4
   TQ1 - :ref:`tab_tq1_251`, Timing and Quantity, R, 4
   OBR - :ref:`tab_obr_251`, Order Detail, R, 7

Expected Actions
^^^^^^^^^^^^^^^^
The DSS/Order Filler shall process the order status based upon the internal application (such as the procedure is completed
and is ready for interpretation). The DSS/Order Filler is recommended to convey the order status to the user of the system.

.. _orm_out_segments:

Outbound Message Segments
=========================

.. _orm_out_pid:

PID - Patient Identification segment
---------------------------

.. csv-table:: PID - Patient Identification segment (HL7 v2.5.1)
   :name: tab_pid_omg_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 01627, Set ID - PID
   2, 20, CX, O, [0..0], , 00105, Patient ID
   3, 250, CX, R, [1..*], , 00106, **Patient Identifier List**
   4, 20, CX, O, [0..0], , 00107, Alternate Patient ID - PID
   5, 250, XPN, R, [1..*], , 00108, **Patient Name**
   6, 250, XPN, O, [0..1], , 00109, Mother’s Maiden Name
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

.. _orm_out_pv1:

PV1 - Patient Visit segment
---------------------------

.. csv-table:: PV1 - Patient Visit segment (HL7 v2.5.1)
   :name: tab_pv1_251
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
   19, 20, CX, C, , 00149, Visit Number
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
   51, 1, IS, C, 0326, 01226, Visit Indicator
   52, 60, XCN, O, 0010, 01224, Other Healthcare Provider

.. _orm_out_orc:

ORC - Order Control segment
---------------------------

.. csv-table:: ORC - Order Control segment (HL7 v2.5.1)
   :name: tab_orc_251
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

.. _orm_out_tq1:

TQ1 - Timing/Quantity segment
-----------------------------

.. csv-table:: TQ1 - Timing/Quantity segment (HL7 v2.5.1)
   :name: tab_tq1_251
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
   9, 250, CWE, O, 0485, 01635, Priority
   10, 250, TX, O, , 01636, Condition Text
   11, 250, TX, O, 0065, 01637, Text Instruction
   12, 10, ID, C, 0472, 01638, Conjunction
   13, 20, CQ, O, , 01639, Occurrence Duration
   14, 10, NM, O, , 01640, Total Occurrences

.. _orm_out_obr:

OBR - Observation Request segment
---------------------------------

.. csv-table:: OBR - Observation Request segment (HL7 v2.5.1)
   :name: tab_obr_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 22, EI, R, , 00216, **Placer Order Number**
   3, 22, EI, O, , 00217, **Filler Order Number**
   4, 250, CE, O, , 00238, Universal Service ID
   5, 2, ID, O, , 00239, Priority
   6, 26, TS, O, , 00240, Requested Date/Time
   7, 26, TS, O, , 00241, Observation Date/Time
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 250, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 250, CE, O, , 00246, Danger Code
   13, 300, ST, C, , 00247, Relevant Clinical Info
   14, 26, TS, X, , 00248, Specimen Received Date/Time
   15, 300, SPS, X, 0070, 00249, Specimen Source
   16, 250, XCN, O, , 00226, Ordering Provider
   17, 250, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, O, , 00251, **Placer Field 1**
   19, 60, ST, O, , 00252, **Placer Field 2**
   20, 60, ST, O, , 00253, Filler Field 1
   21, 60, ST, O, , 00254, Filler Field 2
   22, 26, TS, O, , 00255, Results Rpt/Status Chng - Date/Time
   23, 40, MOC, O, , 00256, Charge to Practice
   24, 10, ID, O, 0074, 00257, Diagnostic Service Sect ID
   25, 1, ID, O, 0123, 00258, Result Status
   26, 400, PRL, O, , 00259, Parent Result
   27, 200, TQ, X, , 00221, Quantity/Timing
   28, 250, XCN, O, , 00260, Result Copies To
   29, 200, EIP, C, , 00261, Parent
   30, 20, ID, O, 0124, 00262, Transportation Mode
   31, 250, CE, O, , 00263, Reason For Study
   32, 200, NDL, O, , 00264, Principal Result Interpreter
   33, 200, NDL, O, , 00265, Assistant Result Interpreter
   34, 200, NDL, O, , 00266, Technician
   35, 200, NDL, O, , 00267, Transcriptionist
   36, 26, TS, O, , 00268, Scheduled Date/Time
   37, 4, NM, O, , 01028, Number of Sample Containers
   38, 250, CE, O, , 01029, Transport Logistics of Collected Sample
   39, 250, CE, O, , 01030, Collector's Comment
   40, 250, CE, O, , 01031, Transport Arrangement Responsibility
   41, 30, ID, O, 0224, 01032, Transport Arranged
   42, 1, ID, O, 0225, 01033, Escort Required
   43, 250, CE, O, , 01034, Planned Patient Transport Comment
   44, 250, CE, O, 0088, 00393, Procedure Code
   45, 250, CE, O, 0340, 01036, Procedure Code Modifier
   46, 250, CE, O, 0411, 01474, Placer Supplemental Service Information
   47, 250, CE, O, 0411, 01475, Filler Supplemental Service Information
   48, 250, CWE, O, 0476, 01646, Medically Necessary Duplicate Procedure Reason
   49, 2, IS, O, 0507, 01647, Result Handling
   50, 250, CWE, O, , 02286, Parent Universal Service Identifier

Element names in **bold** indicates that the field is used by |product|.

.. _orm_out_dicom:

DICOM to HL7 Order Mapping
==========================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component/sub-component value is not listed if the HL7 element should not contain multiple components/sub-components.

.. _orm_out_omg_o19_dicom:

OMG - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: DICOM Modality Worklist Attributes to HL7 order mapping
   :name: dicom_to_omg
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :ref:`tab_hl7_dicom_charset`
   **Scheduled Procedure Step**
   , , Order Control, 00215, ORC:1, Set to XO
   , , Order Status, 00219, ORC:5, Set to CM
   , , Start Date/Time, 01633, TQ1:7, [#Note1]_
   , , Start Date/Time, 01633, TQ1:7, [#Note1]_
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Placer field 2, 00252, OBR:19
   **Imaging Request**
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2


.. [#Note1] If the Procedure Status Update is triggered by MPPS, this value is populated from the
   `Performed Procedure Step Start Date and Time` of MPPS attributes. Alternatively, if the Procedure Status Update is
   triggered when a Study (which has MWL entries referencing it) is completely received, then this value is populated
   from the created time of the task. (The `task` here refers to a task created in database for sending out the HL7 notification.)
   Refer `Synchronize external HL7 receivers on updates of Requested Procedures <https://github.com/dcm4che/dcm4chee-arc-light/wiki/Requested-Procedures>`_