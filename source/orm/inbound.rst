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

.. _orm_in_dicom:

HL7 Order to DICOM MWL Mapping
==============================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. table:: HL7 order mapping to DICOM Modality Worklist Attributes

   +------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DICOM Attribute                   | DICOM Tag   |  HL7 Field                       |  HL7 Item #    |  HL7 Segment | Note                            |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | **SOP Common**                                                                                                                                       |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Specific Character Set            | (0008,0005) | Character Set                    | 00692          | MSH:18       | :numref:`tab_hl7_dicom_charset` |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | **Scheduled Procedure Step**                                                                                                                         |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Scheduled Procedure Step          | (0040,0100) |                                                                                                    |
   | Sequence                          |             |                                                                                                    |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Station AE Title       | (0040,0001) | OMI: Scheduled Station           | OMI: 01665     | ORM ORC:18   | Generated by DSS                |
   |                                   |             |      AE Title                    |                | OMI IPC:9    |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step         | (0040,0002) | ORM: Quantity/Timing             | ORM: 00221.4   | ORM ORC:7.4  | Generated by DSS                |
   |  Start Date                       |             | OMI: Start Date/Time             | OMI: 01633     | OMI TQ1:7    |                                 |
   |                                   |             | OMG: Start Date/Time             | OMG: 01633     | OMG TQ1:7    |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step         | (0040,0003) | ORM: Quantity/Timing             | ORM: 00221.4   | ORM ORC:7.4  | Generated by DSS                |
   |  Start Time                       |             | OMI: Start Date/Time             | OMI: 01633     | OMI TQ1:7    |                                 |
   |                                   |             | OMG: Start Date/Time             | OMG: 01633     | OMG TQ1:7    |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Modality                         | (0008,0060) | ORM: Diagnostic Serv Sect ID     | ORM: 00257     | ORM OBR:24   | Generated by DSS                |
   |                                   |             | OMI: Modality                    | OMI: 00239     | OMI IPC:5    |                                 |
   |                                   |             | OMG: Diagnostic Serv Sect ID     | OMG: 00257     | OMG OBR:24   |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Performing             | (0040,0006) | Technician                       | 00266          | OBR:34       | See note 4                      |
   |  Physician's Name                 |             |                                  |                |              |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step         | (0040,0007) | ORM: Universal Service ID        | ORM: 00238.2.2 | ORM OBR:4.2.2| Generated by DSS                |
   |  Description                      |             | OMI: Protocol Code               | OMI: 00246.2   | OMI IPC:6.2  |                                 |
   |                                   |             | OMG: Universal Service ID        | OMG: 00238.2.2 | OMG OBR:4.2.2|                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Station Name           | (0040,0010) | OMI: Scheduled Station Name      | OMI: 01663     | OMI IPC:7    | Generated by DSS                |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step         | (0040,0011) | OMI: Scheduled Procedure Step    | OMI: 01664     | OMI IPC:8    | Generated by DSS                |
   |  Location                         |             |      Location                    |                |              |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Protocol Code          | (0040,0008) | ORM: Universal Service ID        |                |              | Generated by DSS                |
   |  Sequence                         |             | OMI: Protocol Code               |                |              |                                 |
   |                                   |             | OMG: Universal Service ID        |                |              |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >>Code Value                      | (0008,0100) |                                  | ORM: 00238.2.1 | ORM OBR:4.2.1|                                 |
   |                                   |             |                                  | OMI: 00246.1   | OMI IPC:6.1  |                                 |
   |                                   |             |                                  | OMG: 00238.2.1 | OMG OBR:4.2.1|                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >>Code Scheme Designator          | (0008,0102) |                                  | ORM: 00238.2.3 | ORM OBR:4.2.3|                                 |
   |                                   |             |                                  | OMI: 00246.3   | OMI IPC:6.3  |                                 |
   |                                   |             |                                  | OMG: 00238.2.3 | OMG OBR:4.2.3|                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >>Code Meaning                    | (0008,0104) |                                  | ORM: 00238.2.2 | ORM OBR:4.2.2|                                 |
   |                                   |             |                                  | OMI: 00246.2   | OMI IPC:6.2  |                                 |
   |                                   |             |                                  | OMG: 00238.2.2 | OMG OBR:4.2.2|                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step ID      | (0040,0009) | ORM: Filler Field 1              | ORM: 00253     | ORM OBR:20   | Generated by DSS                |
   |                                   |             | OMI: Scheduled Procedure Step ID | OMI: 00238     | OMI IPC:4    |                                 |
   |                                   |             | OMG: Filler Field 1              | OMG: 00253     | OMG OBR:20   |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Scheduled Procedure Step Status  | (0040,0020) | Order Control, Order Status      | 00215, 00219   | ORC:1, ORC:5 | Generated by DSS                |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requested Procedure               |             |                                  |                |                                                |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requested Procedure ID            | (0040,1001) | ORM: Placer field 2              | ORM: 00252     | ORM OBR:19   | Generated by DSS                |
   |                                   |             | OMI: Requested Procedure ID      | OMI: 00216     | OMI IPC:2    |                                 |
   |                                   |             | OMG: Placer field 2              | ORM: 00252     | ORM OBR:19   |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Reason for requested Procedure    | (0040,1002) | Reason for Study                 | 00263.2        | OBR:31.2     | maybe either a code or text     |
   |                                   |             |                                  |                |              | value; if a code, then the code |
   |                                   |             |                                  |                |              | meaning (display name) should be|
   |                                   |             |                                  |                |              | used; see also (0040,100A)      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Reason for Requested Procedure    | (0040,100A) | Reason for Study                 |                |              | see note of (0040,1002)         |
   | Code Sequence                     |             |                                  |                |              |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Value                       | (0008,0100) |                                  | 00263.1        | OBR:31.1     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Scheme Designator           | (0008,0102) |                                  | 00263.2        | OBR:31.3     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Meaning                     | (0008,0104) |                                  | 00263.3        | OBR:31.2     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requested Procedure Description   | (0032,1060) | Procedure Code                   | 00393.2        | OBR:44.2     | Generated by DSS                |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requested Procedure Code Sequence | (0032,1064) | Procedure Code                   |                |              | Generated by DSS                |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Value                       | (0008,0100) |                                  | 00393.1        | OBR:44.1     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Scheme Designator           | (0008,0102) |                                  | 00393.3        | OBR:44.3     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Code Meaning                     | (0008,0104) |                                  | 00393.2        | OBR:44.2     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Study Instance UID                | (0020,000D) | Study Instance UID               | ORM: Z0001     | ORM ZDS:1    | Generated by DSS                |
   |                                   |             |                                  | OMI: 00217     | OMI IPC:3    |                                 |
   |                                   |             |                                  | OMG: Z0001     | OMG ZDS:1    |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requested Procedure Priority      | (0040,1003) | ORM: Quantity/Timing             | ORM: 00221.6   | ORM ORC:7.5  | See note 1                      |
   |                                   |             | OMI: Start Date/Time             | OMG: 01633     | OMG TQ1:9    |                                 |
   |                                   |             | OMG: Start Date/Time             | OMG: 01633     | OMG TQ1:9    |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient Transport Arrangements    | (0040,1004) | Transportation Mode              | 00262          | OBR:30       |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Imaging Request                                                                                                                                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Accession Number                  | (0008,0050) | ORM: Placer Field 1              | ORM: 00251     | ORM OBR:18   | Generated by DSS                |
   |                                   |             | OMI: Accession Identifier        | OMI: 01330     | OMI IPC:1    |                                 |
   |                                   |             | OMG: Placer Field 1              | OMG: 00251     | OMG OBR:18   |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Requesting Physician              | (0032,1032) | Ordering Provider                | 00226          | OBR:16       |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Referring Physician's Name        | (0008,0090) | Referring Doctor                 | 00138          | PV1:8        |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Placer Issuer and Number          | (0040,2016) | Placer Order #                   | 00216.1        | ORC:2.1      | See note 2                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Order Placer Identifier Sequence  | (0040,0026) | Placer Order #                   |                |              | See note 2                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Local Namespace Entity ID        | (0040,0031) |                                  | 00216.2        | ORC:2.2      |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Filler Issuer and Number          | (0040,2017) | Filler Order #                   | 00217.1        | ORC:3.1      | See note 2                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Order Filler Identifier Sequence  | (0040,0027) | Filler Order #                   |                |              | See note 2                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Local Namespace Entity ID        | (0040,0031) |                                  | 00217.2        | ORC:3.2      |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Visit Identification                                                                                                                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Admission ID                      | (0038,0010) | Visit Number                     | 00149.1        | PV1:19.1     | See note 3                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Issuer of Admission ID Sequence   | 0038,0014)  | Visit Number                     |                |              | See note 3                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | >Local Namespace Entity ID        | (0040,0031) |                                  | 00149.2        | PV1:19.2     |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient Identification            | Same as Patient Identification in :numref:`adt_in_pid_dicom`                                                     |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient Demographic               | Same as Patient Demographic in :numref:`adt_in_pid_dicom`                                                        |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient Medical                                                                                                                                      |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient State                     | (0038,0500) | Danger Code                      | 00246          | OBR:12       |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Pregnancy Status                  | (0010,21C0) | Ambulatory Status                | 00145          | PV1:15       | "B6" must be mapped to DICOM    |
   |                                   |             |                                  |                |              | enumerated value "3" (definitely|
   |                                   |             |                                  |                |              | pregnant)                       |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Medical Alerts                    | (0010,2000) | Relevant Clinical Info           | 00247          | OBR:13       |                                 |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+
   | Patient's Sex Neutered            | Same as mentioned in Patient Medical in :numref:`adt_in_pid_dicom`                                               |
   +-----------------------------------+-------------+----------------------------------+----------------+--------------+---------------------------------+


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