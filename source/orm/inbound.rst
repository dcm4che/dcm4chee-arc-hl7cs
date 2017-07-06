Inbound
#######

.. _orm_in_messages:

Inbound Messages
================

.. _orm_in_orm_o01:

ORM - General Order Message (Event O01)
---------------------------------------
Supported HL7 version: 2.3.1

.. _orm_in_o01_event

Trigger Event
^^^^^^^^^^^^^
The Department System Scheduler/Order Filler determines procedures which need to be performed to fill the order, what
Procedure Steps need to be performed for each Procedure, and timing and necessary resources.
Note: This transaction shall be used the first time a particular Study Instance UID is sent from the Department System
Scheduler/Order Filler to the Image Manager or Report Manager. If the Study Instance UID has been sent previously, then
Procedure Updated (Transaction RAD-13) shall be used.

.. _orm_in_o01_segments:

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ORM^O01^ORM_O01 message:

.. csv-table:: Supported segments of ORM^O01^ORM_O01 (HL7 v2.3.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH, Message Header, 2
   PID, Patient Identification, 3
   PV1, Patient Visit, 3
   ORC, Common Order, 4
   OBR, Order Detail, 4
   ZDS, Additional identification information (custom for IHE),

.. _orm_in_o01_actions

Performed Actions
^^^^^^^^^^^^^^^^^
Patient Demographic Information are extracted from the PID and PV1 segments of the received message and mapped
into corresponding DICOM attributes as defined in :numref:`adt_in_pid_dicom`. If a Patient record with the extracted
primary Patient ID already exists in the database, that Patient record will get updated. If there is no such Patient
record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.
Based on the information received in the ORC and OBR segments, Modality Worklist Item is created/updated in the archive
for the created/updated patient. If the message contains ZDS segment, the specified Study Instance UID will be used else
system will generate a Study Instance UID for the Modality Worklist Item attributes.

.. [#hl7NoPatientCreateMessageType] The creation of new Patient records will be suppressed for message types which are
   listed by configuration parameter *HL7 No Patient Create Message Type(s)*  of |product|.

.. _orm_in_omg_o19:

OMG - General Clinical Order Message (Event O19)
------------------------------------------------
Supported HL7 version: 2.5.1

Trigger Event
^^^^^^^^^^^^^
Same as specified in :numref:`orm_in_o01_event`. This message is sent for eyecare profile.

Supported Segments
^^^^^^^^^^^^^^^^^^
.. csv-table:: Supported segments of OMG^O19^OMG_O19 (HL7 v2.5.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH, Message Header, 2
   PID, Patient Identification, 3
   PV1, Patient Visit, 3
   ORC, Common Order, 4
   TQ1, Timing and Quantity, 4
   OBR, Order Detail, 7
   ZDS, Additional identification information (custom for IHE),

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`orm_in_o01_actions`.

.. _orm_in_o23:

OMI - Imaging Order Message (Event O23)
---------------------------------------
Supported HL7 version: 2.5.1

Trigger Event
^^^^^^^^^^^^^
Same as specified in :numref:`orm_in_o01_actions`.

Supported Segments
^^^^^^^^^^^^^^^^^^
.. csv-table:: Supported segments of OMI^O23^OMI_O23 (HL7 v2.5.1)
   :header: Segment, Meaning, HL7 Chapter
   :widths: 25, 50, 25

   MSH, Message Header, 2
   PID, Patient Identification, 3
   PV1, Patient Visit, 3
   ORC, Common Order, 4
   TQ1, Timing and Quantity, 4
   OBR, Order Detail, 7
   IPC, Imaging Procedure Control, 4

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`orm_in_orm_o01_actions`, with the exception that Study Instance UID will be taken from IPC
segment.

.. _orm_in_segments:

Inbound Message Segments
========================

.. _orm_in_pid:

PID - Patient Identification segment
------------------------------------
Same as specified in  :numref:`tab_pid_231` and :numref:`tab_pid_251`

.. _orm_in_pv1:

PV1 - Patient Visit Information segment
---------------------------------------

.. csv-table:: PV1 - Patient Visit Information segment (HL7 v2.3.1)
   :name: tab_pv1_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00131, Set ID - PV1
   2, 1, IS, R, 0004, 00132, Patient Class
   3, 80, PL, C, , 00133, Assigned Patient Location
   4, 2, IS, O, 0007, 00134, Admission Type
   5, 20, CX, O, , 00135, Preadmit Number
   6, 80, PL, O, , 00136, Prior Patient Location
   7, 60, XCN, C, 0010, 00137, Attending Doctor
   8, 60, XCN, C, 0010, 00138, **Referring Doctor**
   9, 60, XCN, R2, 0010, 00139, Consulting Doctor
   10, 3, IS, C, 0069, 00140, Hospital Service
   11, 80, PL, O, , 00141, Temporary Location
   12, 2, IS, O, 0087, 00142, Preadmit Test Indicator
   13, 2, IS, O, 0092, 00143, Readmission Indicator
   14, 3, IS, O, 0023, 00144, Admit Source
   15, 2, IS, C, 0009, 00145, **Ambulatory Status**
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
   51, 1, IS, C, 0326, 01226, Visit Indicator
   52, 60, XCN, O, 0010, 01224, Other Healthcare Provider

.. csv-table:: PV1 - Patient Visit Information segment (HL7 v2.5.1)
   :name: tab_pv1_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00131, Set ID - PV1
   2, 1, IS, R, 0004, 00132, Patient Class
   3, 80, PL, C, , 00133, Assigned Patient Location
   4, 2, IS, O, 0007, 00134, Admission Type
   5, 250, CX, O, , 00135, Preadmit Number
   6, 80, PL, C, , 00136, Prior Patient Location
   7, 250, XCN, O, 0010, 00137, Attending Doctor
   8, 250, XCN, O, 0010, 00138, **Referring Doctor**
   9, 250, XCN, X, 0010, 00139, Consulting Doctor
   10, 3, IS, O, 0069, 00140, Hospital Service
   11, 80, PL, C, , 00141, Temporary Location
   12, 2, IS, O, 0087, 00142, Preadmit Test Indicator
   13, 2, IS, O, 0092, 00143, Readmission Indicator
   14, 6, IS, O, 0023, 00144, Admit Supplier
   15, 2, IS, C, 0009, 00145, **Ambulatory Status**
   16, 2 , IS, O, 0099, 00146, VIP Indicator
   17, 250, XCN, O, 0010, 00147, Admitting Doctor
   18, 2, IS, O, 0018, 00148, Patient Type
   19, 250, CX, C, , 00149, **Visit Number**
   20, 50, FC, O, 0064, 00150, Financial Class
   21, 2, IS, O, 0032, 00151, Charge Price Indicator
   22, 2, IS, O, 0045, 00152, Courtesy Code
   23, 2, IS, O, 0046, 00153, Credit Rating
   24, 2, IS, O, 0044, 00154, Contract Code
   25, 8, DT, O, , 00155, Contract Effective Date
   26, 12, NM, O, , 00156, Contract Amount
   27, 3, NM, O, , 00157, Contract Period
   28, 2, IS, O, 0073, 00158, Interest Code
   29, 4, IS, O, 0110, 00159, Transfer to Bad Debt Code
   30, 8, DT, O, , 00160, Transfer to Bad Debt Date
   31, 10, IS, O, 0021, 00161, Bad Debt Agency Code
   32, 12, NM, O, , 00162, Bad Debt Transfer Amount
   33, 12, NM, O, , 00163, Bad Debt Recovery Amount
   34, 1, IS, O, 0111, 00164, Delete Account Indicator
   35, 8, DT, O, , 00165, Delete Account Date
   36, 3, IS, O, 0112, 00166, Discharge Disposition
   37, 47, DLD, O, 0113, 00167, Discharge to Location
   38, 250, CE, O, 0114, 00168, Diet Type
   39, 2, IS, O, 0115, 00169, Servicing Facility
   40, 1, IS, X, 0116, 00170, Bed Status
   41, 2, IS, O, 0117, 00171, Account Status
   42, 80, PL, C, , 00172, Pending Location
   43, 80, PL, O, , 00173, Prior Temporary Location
   44, 26, TS, RE, , 00174, Admit Date/Time
   45, 26, TS, RE, , 00175, Discharge Date/Time
   46, 12, NM, O, , 00176, Current Patient Balance
   47, 12, NM, O, , 00177, Total Charges
   48, 12, NM, O, , 00178, Total Adjustments
   49, 12, NM, O, , 00179, Total Payments
   50, 250, CX, O, 0203, 00180, Alternate Visit ID
   51, 1, IS, C, 0326, 01226, Visit Indicator
   52, 250, XCN, X, 0010, 01274, Other Healthcare Provider


.. _orm_in_orc:

ORC - Order Control segment
---------------------------

.. csv-table:: ORC - Order Control segment (HL7 v2.3.1)
   :name: tab_orc_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 2, ID, R, 0119, 00215, **Order Control**
   2, 22, EI, R, , 00216, **Placer Order Number**
   3, 22, EI, O, , 00217, **Filler Order Number**
   4, 22, EI, C, , 00218, Placer Group Number
   5, 2, ID, O, 0038, 00219, **Order Status**
   6, 1, ID, O, 0121, 00220, Response Flag
   7, 200, TQ, R, , 00221, **Quantity/Timing**
   8, 200, CM, C, , 00222, Parent
   9, 26, TS, R, , 00223, Date/Time of Transaction
   10, 120, XCN, R2, , 00224, Entered By
   11, 120, XCN, O, , 00225, Verified By
   12, 120, XCN, R, , 00226, Ordering Provider
   13, 80, PL, O, , 00227, Enterer's Location
   14, 40, XTN, R2, , 00228, Callback Phone Number
   15, 26, TS, O, , 00229, Order Effective Date/Time
   16, 200, CE, O, , 00230, Order Control Code Reason
   17, 60, CE, R, , 00231, Entering Organization
   18, 60, CE, O, , 00232, **Entering Device**
   19, 120, XCN, O, , 00233, Action By

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
   9, 26, TS, R, , 00223, Date/Time of Transaction
   10, 250, XCN, R2, , 00224, Entered By
   11, 250, XCN, O, , 00225, Verified By
   12, 250, XCN, R, , 00226, Ordering Provider
   13, 80, PL, O, , 00227, Enterer's Location
   14, 250, XTN, R2, , 00228, Callback Phone Number
   15, 26, TS, O, , 00229, Order Effective Date/Time
   16, 250, CE, O, , 00230, Order Control Code Reason
   17, 250, CE, R, , 00231, Entering Organization
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


.. _orm_in_tq1:

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
   9, 250, CWE, O, 0485, 01635, **Priority**
   10, 250, TX, O, , 01636, Condition Text
   11, 250, TX, O, 0065, 01637, Text Instruction
   12, 10, ID, C, 0472, 01638, Conjunction
   13, 20, CQ, O, , 01639, Occurrence Duration
   14, 10, NM, O, , 01640, Total Occurrences


.. _orm_in_obr:

OBR - Observation Request segment
---------------------------------

.. csv-table:: OBR - Observation Request segment (HL7 v2.3.1)
   :name: tab_obr_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 75, EI, R, , 00216, Placer Order Number
   3, 75, EI, O, , 00217, Filler Order Number
   4, 200, CE, R, , 00238, **Universal Service ID**
   5, 2, ID, O, , 00239, Priority
   6, 26, TS, O, , 00240, Requested Date/Time
   7, 26, TS, O, , 00241, Observation Date/Time
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 60, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 60, CE, R2, , 00246, **Danger Code**
   13, 300, ST, C, , 00247, **Relevant Clinical Info**
   14, 26, TS, O, , 00248, Specimen Received Date/Time
   15, 300, CM, C, 0070, 00249, Specimen Source
   16, 80, XCN, R, , 00226, **Ordering Provider**
   17, 40, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, O, , 00251, **Placer Field 1**
   19, 60, ST, O, , 00252, **Placer Field 2**
   20, 60, ST, O, , 00253, **Filler Field 1**
   21, 60, ST, O, , 00254, Filler Field 2
   22, 26, TS, O, , 00255, Results Rpt/Status Chng - Date/Time
   23, 40, CM, O, , 00256, Charge to Practice
   24, 10, ID, O, 0074, 00257, **Diagnostic Service Sect ID**
   25, 1, ID, O, 0123, 00258, Result Status
   26, 400, CM, O, , 00259, Parent Result
   27, 200, TQ, R, , 00221, Quantity/Timing
   28, 150, XCN, O, , 00260, Result Copies To
   29, 150, CM, C, , 00261, Parent
   30, 20, ID, R2, 0124, 00262, **Transportation Mode**
   31, 300, CE, R2, , 00263, **Reason For Study**
   32, 200, CM, O, , 00264, Principal Result Interpreter
   33, 200, CM, O, , 00265, Assistant Result Interpreter
   34, 200, CM, O, , 00266, **Technician**
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
   :name: tab_obr_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00237, SetID - OBR
   2, 22, EI, R, , 00216, Placer Order Number
   3, 22, EI, O, , 00217, Filler Order Number
   4, 250, CE, R, , 00238, Universal Service ID
   5, 2, ID, O, , 00239, Priority
   6, 26, TS, O, , 00240, Requested Date/Time
   7, 26, TS, O, , 00241, Observation Date/Time
   8, 26, TS, O, , 00242, Observation End Date/Time
   9, 20, CQ, O, , 00243, Collection Volume
   10, 250, XCN, O, , 00244, Collection Identifier
   11, 1, ID, O, 0065, 00245, Specimen Action Code
   12, 250, CE, R2, , 00246, **Danger Code**
   13, 300, ST, C, , 00247, **Relevant Clinical Info**
   14, 26, TS, X, , 00248, Specimen Received Date/Time
   15, 300, SPS, X, 0070, 00249, Specimen Source
   16, 250, XCN, R, , 00226, **Ordering Provider**
   17, 250, XTN, O, , 00250, Order Callback Phone Number
   18, 60, ST, O, , 00251, Placer Field 1
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
   30, 20, ID, R2, 0124, 00262, **Transportation Mode**
   31, 250, CE, R2, , 00263, **Reason For Study**
   32, 200, NDL, O, , 00264, Principal Result Interpreter
   33, 200, NDL, O, , 00265, Assistant Result Interpreter
   34, 200, NDL, O, , 00266, **Technician**
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


.. _orm_in_zds:

ZDS - Z segment
---------------

.. csv-table:: ZDS - Z segment (HL7 v2.3.1)
   :name: tab_zds_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 200, RP, R, , Z0001, **Study Instance UID**


.. _orm_in_ipc:

IPC - Imaging Procedure Control segment
---------------------------------------

.. csv-table:: IPC - Imaging Procedure Control segment (HL7 v2.5.1)
   :name: tab_ipc_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 80, EI, R, , 00237, **Accession Identifier**
   2, 22, EI, R, , 00216, **Requested Procedure ID**
   3, 70, EI, R, , 00217, **Study Instance UID**
   4, 22, EI, R, , 00238, **Scheduled Procedure Step ID**
   5, 16, CE, R+, , 00239, **Modality**
   6, 250, CE, R2, , 00246, **Protocol Code**
   7, , EI, , , 01663, **Scheduled Station Name**
   8, , CWE, , , 01664, **Scheduled Procedure Step Location**
   9, , ST, , , 01665, **Scheduled Station AE Title**


Element names in **bold** indicates that the field is used by |product|.

HL7 Order to DICOM MWL Mapping
==============================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. _orm_in_dicom:

ORM - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for (HL7 v2.3.1)
   :name: orm_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :numref:`tab_hl7_dicom_charset`
   **Patient Identification**
   Same as Patient Identification in :numref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :numref:`adt_in_pid_dicom`
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, See note 8
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'⇒'ALTERED', 'N'⇒'UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)", , , ORC:18, Generated by DSS
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Quantity/Timing, 00221.4, ORC:7.4, Generated by DSS
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Quantity/Timing, 00221.4, ORC:7.4, Generated by DSS
   >Modality, "(0008, 0060)", Diagnostic Serv Sect ID, 00257, OBR:24, Generated by DSS
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34, See note 4
   >Scheduled Procedure Step Description, "(0040, 0007)", Universal Service ID, 00238.2.2, OBR:4.2.2, Generated by DSS
   >Scheduled Station Name, "(0040, 0010)", , , , See note 5
   >Scheduled Protocol Code Sequence, "(0040, 0008)"
   >>Code Value, "(0008, 0100)", Universal Service ID, 00238.2.1, OBR:4.2.1, Generated by DSS
   >>Code Scheme Designator, "(0008, 0102)", Universal Service ID, 00238.2.3, OBR:4.2.3, Generated by DSS
   >>Code Meaning, "(0008, 0104)", Universal Service ID, 00238.2.2, OBR:4.2.2, Generated by DSS
   >Scheduled Procedure Step ID, "(0040, 0009)", Filler Field 1, 00253, OBR:20, Generated by DSS
   >Scheduled Procedure Step Status, "(0040, 0020)", Order Control_Order Status, 00215_00219, ORC:1_ORC:5, Generated by DSS
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Placer field 2, 00252, OBR:19, Generated by DSS
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, See note 6
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , See note 7
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Requested Procedure Code Sequence, "(0032, 1064)"
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1, Generated by DSS
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3, Generated by DSS
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Study Instance UID, "(0020, 000D)", Study Instance UID, Z0001, ZDS:1, Generated by DSS
   Requested Procedure Priority, "(0040, 1003)", Quantity/Timing, 00221.6, ORC:7.5, See note 1
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18, Generated by DSS
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1, See note 2
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2, See note 2
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1, See note 2
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2, See note 2
   **Visit Identification**
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, See note 3
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.2, PV1:19.2, See note 3


.. _omi_in_dicom:

OMI - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for (HL7 v2.5.1)
   :name: omi_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :numref:`tab_hl7_dicom_charset`
   **Patient Identification**
   Same as Patient Identification in :numref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :numref:`adt_in_pid_dicom`
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, See note 8
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'⇒'ALTERED', 'N'⇒'UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)", Scheduled Station AE Title, 01665, IPC:9, Generated by DSS
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Start Date/Time, 01633, TQ1:7, Generated by DSS
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Start Date/Time, 01633, TQ1:7, Generated by DSS
   >Modality, "(0008, 0060)", Modality, 00239, IPC:5, Generated by DSS
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34, See note 4
   >Scheduled Procedure Step Description, "(0040, 0007)", Protocol Code, 00246.2, IPC:6.2, Generated by DSS
   >Scheduled Station Name, "(0040, 0010)", Scheduled Station Name, 01663, IPC:7, Generated by DSS
   >Scheduled Procedure Step Location, "(0040, 0011)", Scheduled Procedure Step Location, 01664, IPC:8, Generated by DSS
   >Scheduled Protocol Code Sequence, "(0040, 0008)"
   >>Code Value, "(0008, 0100)", Protocol Code, 00246.1, IPC:6.1, Generated by DSS
   >>Code Scheme Designator, "(0008, 0102)", Protocol Code, 00246.3, IPC:6.3, Generated by DSS
   >>Code Meaning, "(0008, 0104)", Protocol Code, 00246.2, IPC:6.2, Generated by DSS
   >Scheduled Procedure Step ID, "(0040, 0009)", Scheduled Procedure Step ID, 00238, IPC:4, Generated by DSS
   >Scheduled Procedure Step Status, "(0040, 0020)", Order Control_Order Status, 00215_00219, ORC:1_ORC:5, Generated by DSS
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Requested Procedure ID, 00216, IPC:2, Generated by DSS
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, See note 6
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , See note 7
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Requested Procedure Code Sequence, "(0032, 1064)"
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1, Generated by DSS
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3, Generated by DSS
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Study Instance UID, "(0020, 000D)", Study Instance UID, 00217, IPC:3, Generated by DSS
   Requested Procedure Priority, "(0040, 1003)", Start Date/Time, 01633, TQ1:9, See note 1
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Accession Identifier, 01330, IPC:1, Generated by DSS
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1, See note 2
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2, See note 2
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1, See note 2
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2, See note 2
   **Visit Identification**
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, See note 3
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.2, PV1:19.2, See note 3

.. _omg_in_dicom:

OMG - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for Eyecare
   :name: omg_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, :numref:`tab_hl7_dicom_charset`
   **Patient Identification**
   Same as Patient Identification in :numref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :numref:`adt_in_pid_dicom`
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, See note 8
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'⇒'ALTERED', 'N'⇒'UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)"
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Start Date/Time, 01633, TQ1:7, Generated by DSS
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Start Date/Time, 01633, TQ1:7, Generated by DSS
   >Modality, "(0008, 0060)", Diagnostic Serv Sect ID, 00257, OBR:24, Generated by DSS
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34, See note 4
   >Scheduled Procedure Step Description, "(0040, 0007)", Universal Service ID, 00238.2.2, OBR:4.2.2, Generated by DSS
   >Scheduled Station Name, "(0040, 0010)", , , , See note 5
   >Scheduled Protocol Code Sequence, "(0040, 0008)"
   >>Code Value, "(0008, 0100)", Universal Service ID, 00238.2.1, OBR:4.2.1, Generated by DSS
   >>Code Scheme Designator, "(0008, 0102)", Universal Service ID, 00238.2.3, OBR:4.2.3, Generated by DSS
   >>Code Meaning, "(0008, 0104)", Universal Service ID, 00238.2.2, OBR:4.2.2, Generated by DSS
   >Scheduled Procedure Step ID, "(0040, 0009)", Filler Field 1, 00253, OBR:20, Generated by DSS
   >Scheduled Procedure Step Status, "(0040, 0020)", Order Control_Order Status, 00215_00219, ORC:1_ORC:5, Generated by DSS
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Placer field 2, 00252, OBR:19, Generated by DSS
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, See note 6
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , See note 7
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Requested Procedure Code Sequence, "(0032, 1064)"
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1, Generated by DSS
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3, Generated by DSS
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2, Generated by DSS
   Study Instance UID, "(0020, 000D)", Study Instance UID, Z0001, ZDS:1, Generated by DSS
   Requested Procedure Priority, "(0040, 1003)", Start Date/Time, 01633, TQ1:9, See note 1
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18, Generated by DSS
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1, See note 2
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2, See note 2
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1, See note 2
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2, See note 2
   **Visit Identification**
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, See note 3
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.2, PV1:19.2, See note 3


Note 1 :  Only the suggested values of the HL7 Priority component of Quantity/Timing shall be used for IHE. These values
shall be mapped to the DICOM enumerated fields for Priority as:

.. csv-table:: HL7 status mapping to DICOM status
   :name: status_mapping
   :header: HL7 Status, DICOM Status

   S - STAT, STAT
   A - ASAP, HIGH
   R - Routine, ROUTINE
   P - Pre-op, HIGH
   C - Callback, HIGH
   T - Timing, MEDIUM

Note 2 : Attributes (0040,2016) and (0040, 2017) are designed to incorporate the HL7 components of Placer Issuer and
Number, and Filler Issuer and Number. In a healthcare enterprise with multiple issuers of patient identifiers, both the
issuer name and number are required to guarantee uniqueness.

Note 3 : either field PID-18 Patient Account Number or field PV1-19 Visit Number or both may be valued depending on the
specific national requirements. Whenever field PV1-19 Visit Number in an order message is valued, its components shall
be used to populate Admission ID (0038,0010) and Issuer of Admission ID (0038,0011) attributes in the MWL responses. In
the case where field PV1-19 Visit Number is not valued, these attributes shall be valued from components of field PID-18
Patient Account Number. This requires that Visit Numbers be unique across all account numbers.

Note 4 : For : HL7 v2.3.1 and v2.5.1 : Field OBR-34 Technician in ORM or OMG message is repeatable. Its data type is CM,
with the following components: <name (CN)> ^ <start date/time (TS)> ^ <end date/time (TS)> ^ <point of care (IS)> ^
<room(IS)> ^ <bed (IS)> ^ <facility (HD)> ^ <location status (IS)> ^ <patient location type (IS)> ^ <building (IS)> ^
<floor (IS)>.
• Thus, in mapping value to the DICOM attribute Scheduled Performing Physician (0040,0006), only sub-components of the
first component of the first repetition of that field shall be used.

Note 5 : Populated only if matching hl7OrderScheduledStation found in configured hl7OrderScheduledStation in archive device.

Note 6 : Maybe either a code or text value; if a code, then the code meaning (display name) should be used; see also (0040,100A)

Note 7 : OBR:31 may be either a code or text value; if a text value, then the DSS may map it to a code to use in the DICOM
attribute; see also (0040,1002).

Note 8 : "B6" must be mapped to DICOM. Enumerated value "3" (definitely pregnant)