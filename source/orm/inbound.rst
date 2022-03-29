Inbound
#######

.. _orm_in_messages:

Inbound Messages
================

.. _orm_in_orm_o01:

ORM - General Order Message (Event O01)
---------------------------------------
Supported HL7 version: 2.3.1 both for RAD-4 and RAD-13

.. _orm_o01_event:

Trigger Event
^^^^^^^^^^^^^
The Department System Scheduler/Order Filler determines procedures which need to be performed to fill the order, what
Procedure Steps need to be performed for each Procedure, and timing and necessary resources.
Note: This transaction shall be used the first time a particular Study Instance UID is sent from the Department System
Scheduler/Order Filler to the Image Manager or Report Manager. If the Study Instance UID has been sent previously, then
Procedure Updated (Transaction RAD-13) shall be used.

.. _orm_o01_segments:

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are processed from an incoming ORM^O01^ORM_O01 message:

.. csv-table:: Supported segments of ORM^O01^ORM_O01 (HL7 v2.3.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_231`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_231`, Patient Identification, R, [1..1], 3
   PV1 - :ref:`tab_pv1_231`, Patient Visit, R, [1..1], 3
   ORC - :ref:`tab_orc_231`, Common Order, R, [1..*], 4
   OBR - :ref:`tab_obr_231`, Order Detail, R, [1..*], 4
   NTE - :ref:`tab_nte`, Notes and Comments (for Detail), O, [0..*], 4
   ZDS - :ref:`tab_zds_orm_omg`, Additional identification information, C, [0..1],
   OBX - :ref:`tab_obx`, Observation / Result, O, [0..*], 7

.. _orm_o01_actions:

Performed Actions
^^^^^^^^^^^^^^^^^
Patient Demographic Information are extracted from the PID and PV1 segments of the received message and mapped
into corresponding DICOM attributes as defined in :ref:`adt_in_pid_dicom`. Optionally, if the received message also contains
OBX segments, then patient demographic attributes are checked in these segments as well [#Note16]_. If a Patient record
with the extracted primary Patient ID already exists in the database, that Patient record will get updated. If there is
no such Patient record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.
Based on the information received in the ORC and OBR segments, Modality Worklist Item is created/updated in the archive
for the created/updated patient. If the message contains ZDS segment, the specified Study Instance UID will be used else
system will generate a Study Instance UID for the Modality Worklist Item attributes.

.. [#hl7NoPatientCreateMessageType] The creation of new Patient records will be suppressed for message types which are
   listed by configuration parameter *HL7 No Patient Create Message Type(s)*  of |product|.

.. _orm_in_omg_o19:

OMG - General Clinical Order Message (Event O19)
------------------------------------------------
Supported HL7 version: 2.5.1 (EYECARE-21 and EYECARE-22)

Trigger Event
^^^^^^^^^^^^^
Same as specified in :numref:`orm_o01_event`. This message is sent for eyecare profile.

Supported Segments
^^^^^^^^^^^^^^^^^^
.. csv-table:: Supported segments of OMG^O19^OMG_O19 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   PV1 - :ref:`tab_pv1_251`, Patient Visit, R, [1..1], 3
   ORC - :ref:`tab_orc_251`, Common Order, R, [1..*], 4
   TQ1 - :ref:`tab_tq1_251`, Timing/Quantity, R, [1..*], 4
   OBR - :ref:`tab_obr_251`, Order Detail, R, [1..*], 4
   NTE - :ref:`tab_nte`, Notes and Comments (for Detail), O, [0..*], 4
   ZDS - :ref:`tab_zds_orm_omg`, Additional identification information, C*, [0..*],
   OBX - :ref:`tab_obx`, Observation / Result, O, [0..*], 7

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`orm_o01_actions`.

.. _orm_in_omi_o23:

OMI - Imaging Order Message (Event O23)
---------------------------------------
Supported HL7 version: 2.5.1 (RAD-4 and RAD-13)

Trigger Event
^^^^^^^^^^^^^
Same as specified in :numref:`orm_o01_event`.

Supported Segments
^^^^^^^^^^^^^^^^^^   
.. csv-table:: Supported segments of OMI^O23^OMI_O23 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   PID - :ref:`tab_pid_251`, Patient Identification, R, [1..1], 3
   PV1 - :ref:`tab_pv1_251`, Patient Visit, R, [1..1], 3
   ORC - :ref:`tab_orc_251`, Common Order, R, [1..*], 4
   TQ1 - :ref:`tab_tq1_251`, Timing/Quantity, R, [1..1], 4
   OBR - :ref:`tab_obr_251`, Order Detail, R, [1..*], 4
   NTE - :ref:`tab_nte`, Notes and Comments (for Detail), O, [0..*], 4
   IPC - :ref:`tab_ipc_251`, Imaging Procedure Control, R, [1..*], 4
   OBX - :ref:`tab_obx`, Observation / Result, O, [0..*], 7

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`orm_o01_actions`, with the exception that Study Instance UID will be taken from IPC
segment.

.. _orm_in_segments:

Inbound Message Segments
========================

.. _orm_in_msh:

MSH - Message Header segment
----------------------------
Same as specified in :ref:`tab_msh_231` or :ref:`tab_msh_251`

.. _orm_in_pid:

PID - Patient Identification segment
------------------------------------
Same as specified in :ref:`tab_pid_231` or :ref:`tab_pid_251`

.. _orm_in_pv1:

PV1 - Patient Visit segment
---------------------------

.. csv-table:: Patient Visit segment (HL7 v2.3.1)
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


.. csv-table:: Patient Visit segment (HL7 v2.5.1)
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

.. csv-table:: Order Control segment - (HL7 v2.3.1)
   :name: tab_orc_231
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name, Note
   :widths: 8, 8, 8, 8, 8, 12, 48, 8

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
   18, , CE, O, , 00232, **Entering Device**, [#Note14]_
   19, 120, XCN, O, , 00233, Action By


ORC - Order Control segment
---------------------------

.. csv-table:: Order Control segment - (HL7 v2.5.1)
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

.. csv-table:: Timing/Quantity segment - (HL7 v2.5.1 & Eyecare)
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

.. csv-table:: Observation Request segment - (HL7 v2.3.1)
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
   44, 80, CE, O, 0088, 00393, **Procedure Code**
   45, 80, CE, O, 0340, 01036, Procedure Code Modifier


OBR - Observation Request segment
---------------------------------

.. csv-table:: Observation Request segment - (HL7 v2.5.1)
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
   44, 250, CE, O, 0088, 00393, **Procedure Code**
   45, 250, CE, O, 0340, 01036, Procedure Code Modifier
   46, 250, CE, R2, 0411, 01474, Placer Supplemental Service Information
   47, 250, CE, R2, 0411, 01475, Filler Supplemental Service Information
   48, 250, CWE, R2, 0476, 01646, Medically Necessary Duplicate Procedure Reason
   49, 2, IS, O, 0507, 01647, Result Handling
   50, 250, CWE, O, , 02286, Parent Universal Service Identifier


.. _orm_in_nte:

NTE - Notes and Comments (for Detail) segment
---------------------------------------------

.. csv-table:: Notes and Comments (for Detail) segment
   :name: tab_nte
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00096, Set ID - NTE
   2, 8, ID, R2, 0105, 00097, Source of Comment
   3, 10240, FT, R, , 00098, **Comment**
   4, 60, CE, 0, , 01318, Comment Type

.. _orm_in_zds:

ZDS - Z segment
---------------

.. csv-table:: Z segment (HL7 v2.3.1 & Eyecare)
   :name: tab_zds_orm_omg
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 200, RP, R, , Z0001, **Study Instance UID**


.. _orm_in_ipc:

IPC - Imaging Procedure Control segment
---------------------------------------

.. csv-table:: Imaging Procedure Control segment (HL7 v2.5.1)
   :name: tab_ipc_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name, Note
   :widths: 8, 8, 8, 8, 8, 12, 48, 8

   1, 80, EI, R, , 00237, **Accession Identifier**
   2, 22, EI, R, , 00216, **Requested Procedure ID**
   3, 70, EI, R, , 00217, **Study Instance UID**
   4, 22, EI, R, , 00238, **Scheduled Procedure Step ID**
   5, 16, CE, R+, , 00239, **Modality**
   6, 250, CE, R2, , 00246, **Protocol Code**
   7, , EI, O, , 01663, **Scheduled Station Name**, [#Note14]_
   8, 250, CE, O, , 01664, **Scheduled Procedure Step Location**
   9, , ST, O, , 01665, **Scheduled Station AE Title**, [#Note14]_

.. _orm_in_obx:

OBX - Observation / Results segment
-----------------------------------

.. csv-table:: Observation / Results segment
   :name: tab_obx
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 4, SI, O, , 00569, Set ID - OBX
   2, 2, ID, C, 0125, 00570, Value Type
   3, 250, CE, R, , 00571, **Observation Identifier**
   4, 20, ST, C, , 00572, Observation Sub-ID
   5, 99999¹, varies, C, , 00573, **Observation Value**

Element names in **bold** indicates that the field is used by |product|.

.. _orm_in_dicom:

HL7 Order to DICOM MWL Mapping
==============================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. _orm_in_orm_o01_dicom:

ORM - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for (HL7 v2.3.1 and v2.5.1)
   :name: orm_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note15]_
   **Patient Identification**
   Same as Patient Identification in :ref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :ref:`adt_in_pid_dicom`
   Patient's Weight, "(0010, 1030)", Observation Value, 00573, OBX:5, [#Note16]_
   Patient's Size, "(0010, 1020)", Observation Value, 00573, OBX:5, [#Note16]_
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, [#Note8]_
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'='ALTERED', 'N'='UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)", , , , [#Note13]_
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Quantity/Timing, 00221.4, ORC:7.4
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Quantity/Timing, 00221.4, ORC:7.4
   >Modality, "(0008, 0060)", Diagnostic Serv Sect ID, 00257, OBR:24
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34.1, [#Note4]_
   >Scheduled Procedure Step Description, "(0040, 0007)", Universal Service ID, 00238.4.5, OBR:4.5, [#Note10]_
   >Scheduled Station Name, "(0040, 0010)", , , , [#Note5]_
   >Scheduled Protocol Code Sequence, "(0040, 0008)", , , , [#Note10]_
   >>Code Value, "(0008, 0100)", Universal Service ID, 00238.4.4, OBR:4.4
   >>Code Scheme Designator, "(0008, 0102)", Universal Service ID, 00238.4.6, OBR:4.6
   >>Code Meaning, "(0008, 0104)", Universal Service ID, 00238.4.5, OBR:4.5
   >Scheduled Procedure Step ID, "(0040, 0009)", Filler Field 1, 00253, OBR:20
   >Scheduled Procedure Step Status, "(0040, 0020)", "Order Control, Order Status", "00215, 00219", "ORC:1, ORC:5", [#Note9]_
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Placer field 2, 00252, OBR:19
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, [#Note6]_
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , [#Note7]_
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, [#Note11]_
   Requested Procedure Code Sequence, "(0032, 1064)", , , , [#Note11]_
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2
   Study Instance UID, "(0020, 000D)", Study Instance UID, Z0001.1, ZDS:1.1
   Requested Procedure Priority, "(0040, 1003)", Quantity/Timing, 00221.6, ORC:7.6, [#Note1]_
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order, 00216.1, ORC:2.1, [#Note2]_
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order, 00216.2, ORC:2.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Placer Order, 00216.3, ORC:2.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Placer Order, 00216.4, ORC:2.4, [#Note2]_
   Filler Issuer and Number, "(0040, 2017)", Filler Order, 00217.1, ORC:3.1, [#Note2]_
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order, 00217.2, ORC:3.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Filler Order, 00217.3, ORC:3.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Filler Order, 00217.4, ORC:3.4, [#Note2]_
   **Visit Identification**
   Route of Admissions, "(0038, 0016)", Patient Class, 00132, PV1:2, [#Note17]_
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, [#Note3]_
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.4.1, PV1:19.4.1, [#Note3]_
   >Universal Entity ID, "(0040, 0032)", Visit Number, 00149.4.2, PV1:19.4.2, [#Note3]_
   >Universal Entity ID Type, "(0040, 0033)", Visit Number, 00149.4.3, PV1:19.4.3, [#Note3]_

.. _orm_in_omi_o23_dicom:

OMI - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for (HL7 v2.5.1)
   :name: omi_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note15]_
   **Patient Identification**
   Same as Patient Identification in :ref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :ref:`adt_in_pid_dicom`
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, [#Note8]_
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'='ALTERED', 'N'='UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)", Scheduled Station AE Title, 01665, IPC:9, [#Note12]_
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Start Date/Time, 01633, TQ1:7
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Start Date/Time, 01633, TQ1:7
   >Modality, "(0008, 0060)", Modality, 00239, IPC:5
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34.1, [#Note4]_
   >Scheduled Procedure Step Description, "(0040, 0007)", Protocol Code, 00246.2, IPC:6.2
   >Scheduled Station Name, "(0040, 0010)", Scheduled Station Name, 01663, IPC:7
   >Scheduled Procedure Step Location, "(0040, 0011)", Scheduled Procedure Step Location, 01664, IPC:8
   >Scheduled Protocol Code Sequence, "(0040, 0008)"
   >>Code Value, "(0008, 0100)", Protocol Code, 00246.1, IPC:6.1
   >>Code Scheme Designator, "(0008, 0102)", Protocol Code, 00246.3, IPC:6.3
   >>Code Meaning, "(0008, 0104)", Protocol Code, 00246.2, IPC:6.2
   >Scheduled Procedure Step ID, "(0040, 0009)", Scheduled Procedure Step ID, 00238, IPC:4
   >Scheduled Procedure Step Status, "(0040, 0020)", "Order Control, Order Status", "00215, 00219", "ORC:1, ORC:5", [#Note9]_
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Requested Procedure ID, 00216, IPC:2
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, [#Note6]_
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , [#Note7]_
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, [#Note11]_
   Requested Procedure Code Sequence, "(0032, 1064)", , , , [#Note11]_
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2
   Study Instance UID, "(0020, 000D)", Study Instance UID, 00217, IPC:3
   Requested Procedure Priority, "(0040, 1003)", Start Date/Time, 01633, TQ1:9, [#Note1]_
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Accession Identifier, 01330, IPC:1
   Issuer Of Accession Number Sequence, "(0008, 0051)"
   >Local Namespace Entity ID, "(0040, 0031)", Accession Identifier, 01330.2, IPC:1.2
   >Universal Entity ID, "(0040, 0032)", Accession Identifier, 01330.2, IPC:1.3
   >Universal Entity ID Type, "(0040, 0033)", Filler Order #, 01330.2, IPC:1.4
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1, [#Note2]_
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Placer Order #, 00216.3, ORC:2.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Placer Order #, 00216.4, ORC:2.4, [#Note2]_
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1, [#Note2]_
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Filler Order #, 00217.3, ORC:3.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Filler Order #, 00217.4, ORC:3.4, [#Note2]_
   **Visit Identification**
   Route of Admissions, "(0038, 0016)", Patient Class, 00132, PV1:2, [#Note17]_
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, [#Note3]_
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.4.1, PV1:19.4.1, [#Note3]_
   >Universal Entity ID, "(0040, 0032)", Visit Number, 00149.4.2, PV1:19.4.2, [#Note3]_
   >Universal Entity ID Type, "(0040, 0033)", Visit Number, 00149.4.3, PV1:19.4.3, [#Note3]_

.. _orm_in_omg_o19_dicom:

OMG - HL7 order mapping to DICOM Modality Worklist Attributes
-------------------------------------------------------------

.. csv-table:: HL7 order mapping to DICOM Modality Worklist Attributes for Eyecare
   :name: omg_to_dicom
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note

   **SOP Common**
   Specific Character Set, "(0008, 0005)", Character Set, 00692, MSH:18, [#Note15]_
   **Patient Identification**
   Same as Patient Identification in :ref:`adt_in_pid_dicom`
   **Patient Demographic**
   Same as Patient Demographic in :ref:`adt_in_pid_dicom`
   **Patient Medical**
   Patient State, "(0038, 0500)", Danger Code, 00246, OBR:12
   Pregnancy Status, "(0010, 21C0)", Ambulatory Status, 00145, PV1:15, [#Note8]_
   Medical Alerts, "(0010, 2000)", Relevant Clinical Info, 00247, OBR:13
   Patient's Sex Neutered, "(0010, 2203)", Administrative Sex, 00111.2, PID:8.2, "'Y'='ALTERED', 'N'='UNALTERED'"
   **Scheduled Procedure Step**
   Scheduled Procedure Step Sequence, "(0040, 0100)"
   >Scheduled Station AE Title, "(0040, 0001)", , , , [#Note13]_
   >Scheduled Procedure Step Start Date, "(0040, 0002)", Start Date/Time, 01633, TQ1:7
   >Scheduled Procedure Step Start Time, "(0040, 0003)", Start Date/Time, 01633, TQ1:7
   >Modality, "(0008, 0060)", Diagnostic Serv Sect ID, 00257, OBR:24
   >Scheduled Performing Physician's Name, "(0040, 0006)", Technician, 00266, OBR:34.1, [#Note4]_
   >Scheduled Procedure Step Description, "(0040, 0007)", Universal Service ID, 00238.4.5, OBR:4.5, [#Note10]_
   >Scheduled Station Name, "(0040, 0010)", , , , [#Note5]_
   >Scheduled Protocol Code Sequence, "(0040, 0008)", , , , [#Note10]_
   >>Code Value, "(0008, 0100)", Universal Service ID, 00238.4.4, OBR:4.4
   >>Code Scheme Designator, "(0008, 0102)", Universal Service ID, 00238.4.6, OBR:4.6
   >>Code Meaning, "(0008, 0104)", Universal Service ID, 00238.4.5, OBR:4.5
   >Scheduled Procedure Step ID, "(0040, 0009)", Filler Field 1, 00253, OBR:20
   >Scheduled Procedure Step Status, "(0040, 0020)", "Order Control, Order Status", "00215, 00219", "ORC:1, ORC:5", [#Note9]_
   **Requested Procedure**
   Requested Procedure ID, "(0040, 1001)", Placer field 2, 00252, OBR:19
   Reason for Requested Procedure, "(0040, 1002)", Reason for Study, 00263.2, OBR:31.2, [#Note6]_
   Reason for Requested Procedure Code Sequence, "(0040, 100A)", , , , [#Note7]_
   >Code Value, "(0008, 0100)", Reason for Study, 00263.1, OBR:31.1
   >Code Scheme Designator, "(0008, 0102)", Reason for Study, 00263.3, OBR:31.3
   >Code Meaning, "(0008, 0104)", Reason for Study, 00263.2, OBR:31.2
   Requested Procedure Description, "(0032, 1060)", Procedure Code, 00393.2, OBR:44.2, [#Note11]_
   Requested Procedure Code Sequence, "(0032, 1064)", , , , [#Note11]_
   >Code Value, "(0008, 0100)", Procedure Code, 00393.1, OBR:44.1
   >Code Scheme Designator, "(0008, 0102)", Procedure Code, 00393.3, OBR:44.3
   >Code Meaning, "(0008, 0104)", Procedure Code, 00393.2, OBR:44.2
   Study Instance UID, "(0020, 000D)", Study Instance UID, Z0001.1, ZDS:1.1
   Requested Procedure Priority, "(0040, 1003)", Start Date/Time, 01633, TQ1:9, [#Note1]_
   Requested Procedure Comments, "(0040, 1400)", Comment, 00098, NTE:3
   Patient Transport Arrangements, "(0040, 1004)", Transportation Mode, 00262, OBR:30
   **Imaging Request**
   Accession Number, "(0008, 0050)", Placer Field 1, 00251, OBR:18
   Requesting Physician, "(0032, 1032)", Ordering Provider, 00226, OBR:16
   Referring Physician's Name, "(0008, 0090)", Referring Doctor, 00138, PV1:8
   Placer Issuer and Number, "(0040, 2016)", Placer Order #, 00216.1, ORC:2.1, [#Note2]_
   Order Placer Identifier Sequence, "(0040, 0026)"
   >Local Namespace Entity ID, "(0040, 0031)", Placer Order #, 00216.2, ORC:2.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Placer Order #, 00216.3, ORC:2.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Placer Order #, 00216.4, ORC:2.4, [#Note2]_
   Filler Issuer and Number, "(0040, 2017)", Filler Order #, 00217.1, ORC:3.1, [#Note2]_
   Order Filler Identifier Sequence, "(0040, 0027)"
   >Local Namespace Entity ID, "(0040, 0031)", Filler Order #, 00217.2, ORC:3.2, [#Note2]_
   >Universal Entity ID, "(0040, 0032)", Filler Order #, 00217.3, ORC:3.3, [#Note2]_
   >Universal Entity ID Type, "(0040, 0033)", Filler Order #, 00217.4, ORC:3.4, [#Note2]_
   **Visit Identification**
   Route of Admissions, "(0038, 0016)", Patient Class, 00132, PV1:2, [#Note17]_
   Admission ID, "(0038, 0010)", Visit Number, 00149.1, PV1:19.1, [#Note3]_
   Issuer of Admission ID Sequence, "(0038, 0014)"
   >Item, "(FFFE, E000)"
   >Local Namespace Entity ID, "(0040, 0031)", Visit Number, 00149.4.1, PV1:19.4.1, [#Note3]_
   >Universal Entity ID, "(0040, 0032)", Visit Number, 00149.4.2, PV1:19.4.2, [#Note3]_
   >Universal Entity ID Type, "(0040, 0033)", Visit Number, 00149.4.3, PV1:19.4.3, [#Note3]_


.. csv-table:: HL7 status mapping to DICOM status
   :name: status_mapping
   :header: HL7 Status, DICOM Status

   S - STAT, STAT
   A - ASAP, HIGH
   R - Routine, ROUTINE
   P - Pre-op, HIGH
   C - Callback, HIGH
   T - Timing, MEDIUM

.. _orm_in_err:

HL7 ORM - Error Mapping
=======================

Following table gives an overview of error codes and messages sent by |product| for incoming HL7 ADT messages triggering
error conditions.

.. csv-table:: Error Codes Mapping and Usage
   :name: tab_hl7_orm_error
   :header: Error Code,Error Code Meaning,Error Location,User Message,Notes

   **Error Common**
   Same as Error Codes Mapping and Usage in :ref:`tab_hl7_error`
   **Patient Management specific**
   Same as Error Codes Mapping and Usage in :ref:`tab_hl7_adt_error` specific to PID segment.
   **Procedure Management specific**
   101,Required Field Missing,ORC^1^1^1^1,Invalid order control in field 1 and/or invalid order status in field 5,
   ,,ZDS^1^1^1^1,Missing study instance uid,[#Note18]_
   ,,IPC^1^3^1^1,Missing study instance uid,[#Note19]_
   ,,OBR^1^18^1^1,Missing accession number,[#Note18]_
   ,,IPC^1^1^1^1,Missing accession number,[#Note19]_
   102,Data Type Error,ORC^1^7^1^4,Invalid scheduled procedure step start date and/or time,[#Note18]_
   ,,TQ1^1^7^1^1,Invalid scheduled procedure step start date and/or time,[#Note19]_


.. [#Note1] Only the suggested values of the HL7 Priority component of Quantity/Timing. These values shall be
   mapped to the DICOM enumerated fields for Priority. See :ref:`status_mapping`

.. [#Note2] Attributes (0040,2016) and (0040, 2017) are designed to incorporate the HL7 components of Placer Issuer and
    Number, and Filler Issuer and Number. In a healthcare enterprise with multiple issuers of patient identifiers, both the
    issuer name and number are required to guarantee uniqueness.

.. [#Note3] either field PID-18 Patient Account Number or field PV1-19 Visit Number or both may be valued depending on the
    specific national requirements. Whenever field PV1-19 Visit Number in an order message is valued, its components shall
    be used to populate Admission ID (0038,0010) and Issuer of Admission ID (0038,0011) attributes in the MWL responses. In
    the case where field PV1-19 Visit Number is not valued, these attributes shall be valued from components of field PID-18
    Patient Account Number. This requires that Visit Numbers be unique across all account numbers.

.. [#Note4] For : HL7 v2.3.1 and v2.5.1 : Field OBR-34 Technician in ORM or OMG message is repeatable. Its data type is CM,
    with the following components: <name (CN)> ^ <start date/time (TS)> ^ <end date/time (TS)> ^ <point of care (IS)> ^
    <room(IS)> ^ <bed (IS)> ^ <facility (HD)> ^ <location status (IS)> ^ <patient location type (IS)> ^ <building (IS)> ^
    <floor (IS)>.
    - Thus, in mapping value to the DICOM attribute Scheduled Performing Physician (0040,0006), only sub-components of the
    first component of the first repetition of that field shall be used.

.. [#Note5] Populated only if matching hl7OrderScheduledStation found in configured hl7OrderScheduledStation in archive device.

.. [#Note6] Maybe either a code or text value; if a code, then the code meaning (display name) should be used; see also (0040,100A)

.. [#Note7] OBR:31 may be either a code or text value; if a text value, then the DSS may map it to a code to use in the DICOM
   attribute; see also (0040,1002).

.. [#Note8] "B6" must be mapped to DICOM. Enumerated value "3" (definitely pregnant)

.. [#Note9] The values present in ORC fields 1 and 5 decide the Scheduled Procedure Step Status that is applied to the MWL.
   The enumerated combinations of values in fields 1 and 5 of ORC segment currently supported by the archive are
   NW_SC, NW_IP, CA_CA, DC_CA, XO_SC, XO_CM where the first two letters eg. "NW" represent value
   in field 1 and the next letter(s) after the "_" eg. "SC" represent value in field 5.
   These combinations can be mapped to different Scheduled Procedure Step Status supported by archive :
   SCHEDULED, ARRIVED, READY, STARTED, DEPARTED, CANCELLED, DISCONTINUED, COMPLETED. One can map multiple combinations of
   ORC:1_ORC:5 to a scheduled procedure step status.

.. [#Note10] Alternatively, it may be read from OBR:4 Components 1 to 3 by configuring it as
   `hl7scheduledprotocolcodeinorder on Archive device level <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveDevice.html#hl7scheduledprotocolcodeinorder>`_
   or as `hl7scheduledprotocolcodeinorder on Archive HL7 Application Extension level <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7scheduledprotocolcodeinorder>`_.
   Then it implies that Scheduled Procedure Step Description & Code Meaning in Scheduled Protocol Code Sequence will be
   read from component 2, Code Value and Code Scheme Designator in Scheduled Protocol Code Sequence will be read from
   components 1 and 3 respectively.

.. [#Note11] Although OBR:44 field is optional in HL7 order message, it is required to be supported by the archive which acts
   as a SCP when queried for Modality Worklist entries. Refer `Attributes for the Modality Worklist Information Model <http://dicom.nema.org/medical/dicom/current/output/html/part04.html#table_K.6-1>`_.
   Currently archive does not set any default value to these attributes when this field is missing in HL7 order message.

.. [#Note12] Although IPC:9 field is optional in HL7 order message, it is required to be supported by the archive which acts
   as a SCP when queried for Modality Worklist entries. Refer `Attributes for the Modality Worklist Information Model <http://dicom.nema.org/medical/dicom/current/output/html/part04.html#table_K.6-1>`_.
   Currently if this field is missing in HL7 order message, the Scheduled Station AE Title is selected according configured rule
   `Default Scheduled Station <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7OrderScheduledStation.html>`_
   configured on archive device level. One must note that, if this configuration is deleted as well by the user then no value will be set
   for Scheduled Station AE Title by the archive.

.. [#Note13] This attribute may be configured to be read from field 18 of ORC segment for HL7 v3 and eyecare messages. The configuration can be done as
   `hl7ScheduledStationAETInOrder on Archive device level <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveDevice.html#hl7ScheduledStationAETInOrder>`_
   or as `hl7ScheduledStationAETInOrder on Archive HL7 Application Extension level <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7ScheduledStationAETInOrder>`_.
   Currently if not configured as explained above or if this field is missing in HL7 order message, then the Scheduled
   Station AE Title is selected according configured rule `Default Scheduled Station <http://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7OrderScheduledStation.html>`_
   configured on archive device level. One must note that, if this default configuration is deleted as well by the user then no value will be set
   for Scheduled Station AE Title by the archive.

.. [#Note14] This field may contain multiple values encoded as HL7 repeating field despite `current HL7v2 <http://www.hl7.eu/refactored/segIPC.html>`_
   not allowing multiple values for this field.

.. [#Note15] `HL7 DICOM Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/archiveHL7Application.html#hl7dicomcharacterset>`_
   if configured, is selected to specify Specific Character Set. Else, MSH-18 if present in the incoming HL7 message, :ref:`tab_hl7_dicom_charset` 
   is selected to specify Specific Character Set. If MSH-18 is absent, then
   `HL7 Default Character Set <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7Application.html#hl7defaultcharacterset>`_
   is selected to specify Specific Character Set.

.. [#Note16] If OBX:6 = "kg" and OBX:3.2 = "Body Weight", then OBX:5 is mapped to DICOM attribute Patient's Weight.
   If OBX:6 = "m" and OBX:3.2 = "Body Height", then OBX:5 is mapped to DICOM attribute Patient's Size.

.. [#Note17] Route of Admissions (0038, 0016) DICOM attribute shall be mapped to value present in PV1:2. If this field is
   absent, default "U" (denoting Patient Class as Unknown) shall be used.

.. [#Note18] Applicable only for HL7 v2.3 ORM^O01 or OMG^O19 messages

.. [#Note19] Applicable only for HL7 v2.5 OMI^O23 messages