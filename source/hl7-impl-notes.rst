HL7 Implementation Notes
========================

The HL7 Services provided by |product| are compliant with the HL7 Implementation Notes specified in
`IHE Radiology Technical Framework, Volume 2 <http://ihe.net/uploadedFiles/Documents/Radiology/IHE_RAD_TF_Vol2.pdf>`_,
Section 2.4.

Common HL7 Message Implementation Requirements
----------------------------------------------

Network Guidelines
^^^^^^^^^^^^^^^^^^

The HL7 standards do not define a network communications protocol. The HL7 v2.1 standard defines lower layer protocols
in an appendix. These definitions were moved to the Implementation Guide in 2.2 and subsequent versions, but are not
HL7 requirements.

|product| follows the recommendations made by the IHE Framework:

#. Applications shall use the Minimal Lower Layer Protocol (MLLP) defined in Appendix C of the HL7 Implementation Guide.

#. An application that wants to send a message (initiate a transaction) will initiate a network connection to start
   the transaction. The receiver application will respond with an acknowledgement or response to query but will not
   initiate new transactions on this network connection

Acknowledgement Mode
^^^^^^^^^^^^^^^^^^^^

Applications that receive HL7 messages shall send acknowledgments using the HL7 Original Mode (versus Enhanced
Acknowledgment Mode).

HL7 Versioning
^^^^^^^^^^^^^^

The support of particular versions of HL7 v2 dependent on the HL7 message type by |product| corresponds to the
different versions of HL7 selected by the IHE Technical Framework for different
`IHE Domains <https://www.ihe.net/IHE_Domains/>`_. Particularly, the
`IHE Radiology Technical Framework <https://www.ihe.net/Technical_Frameworks/#radiology>`_ specifies the use of
HL7 v2.3.1 and optionally of HL7 v2.5.1, the
`IHE IT Infrastructure Technical Framework <https://www.ihe.net/Technical_Frameworks/#IT>`_ the use of
HL7 v2.5 for HL7 based transactions.

|product| comply with the message structure and contents defined by the specified version(s) of the HL7 standard as
defined in the IHE transaction technical specification. It is acceptable if the HL7 standard version value (MSH-12) in
a conformant message is higher than that specified in the IHE transaction as long as the message structure and contents
meet the requirements of the specification.

HL7 v2.3.1 Message Implementation Requirements
----------------------------------------------

.. _ack_message_231:

Acknowledgement Message
^^^^^^^^^^^^^^^^^^^^^^^

Each HL7 message shall be acknowledged by the HL7 ACK message sent by the receiver of an HL7 message to its sender.
The segments of the ACK message listed below are required, and their detailed descriptions are provided in the
following subsections. The ERR segment is optional and will not be included in ACK messages sent by |product|.

.. csv-table:: Common ACK Message static definition
   :header: Segment,Meaning,Chapter in HL7 v2.3.1
   :widths: 25, 50, 25

   MSH - :ref:`tab_msh_231`,Message Header,2
   MSA - :ref:`tab_msa_231`,Message Acknowledgement,2
   [ERR] - :ref:`tab_err_231`,Error,2

.. _message_control_231:

Message Control
^^^^^^^^^^^^^^^

The MSH (message header) segment contains control information set in the beginning of each message sent.

.. csv-table:: MSH - Message Header segment (HL7 v2.3.1)
   :name: tab_msh_231
   :header: SEQ,LEN,DT,OPT,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1,1,ST,R,,00001,**Field Separator**
   2,4,ST,R,,00002,**Encoding Characters**
   3,180,HD,R+,,00003,**Sending Application**
   4,180,HD,R+,,00004,**Sending Facility**
   5,180,HD,R+,,00005,**Receiving Application**
   6,180,HD,R+,,00006,**Receiving Facility**
   7,26,TS,R,,00007,Date/Time Of Message
   8,40,ST,O,,00008,Security
   9,13,CM,R,0076/ 0003,00009,**Message Type**
   10,20,ST,R,,00010,**Message Control ID**
   11,3,PT,R,,00011,Processing ID
   12,60,VID,R,0104,00012,Version ID
   13,15,NM,X,,00013,Sequence Number
   14,180,ST,X,,00014,Continuation Pointer
   15,2,ID,X,0155,00015,Accept Acknowledgment Type
   16,2,ID,X,0155,00016,Application Acknowledgment Type
   17,3,ID,O,0399,00017,Country Code
   18,16,ID,C,0211,00692,**Character Set**
   19,250,CE,O,,00693,Principal Language Of Message
   20,20,ID,X,0356,01317,Alternate Character Set Handling Scheme

Element names in **bold** indicates that the field is used by |product|.

|product| only supports the HL7-recommended values for the fields *MSH-1-Field Separator* (= ``|``) and
*MSH-2-Encoding Characters* (= ``^~\&``).

|product| does not support the sequence number protocol (it does not make use of field *MSH-13-Sequence Number*).

Field *MSH-18-Character Set* contains the character set for the entire message. Refer to
`HL7 Table 0211 - Alternate character sets <https://www.hl7.org/fhir/v2/0211/index.html>`_ for valid values.

Examples of valid values:

``8859/1``
   The printable characters from the ISO 8859/1 Character set used by Western Europe.
``ISO IR87``
   Code for the Japanese Graphic Character set for information interchange (JIS X 0208-1990).
``GB 18030-2000``
   Code for the Chinese Character Set (GB 18030-2000).
``UNICODE UTF-8``
   UCS Transformation Format, 8-bit form.

*MSH-18-Character Set* shall only be valued if the message uses a character set other than the 7-bit ASCII character set.
Though the field is repeatable in HL7, |product| supports only one occurrence (i.e., one character set).
The character set specified in this field is used for the encoding of all of the characters within the message.

*MSH-20-Alternate Character Set Handling Scheme* is not supported by |product|.


Acknowledgement Modes
^^^^^^^^^^^^^^^^^^^^^

This segment contains information sent while acknowledging another message.

.. csv-table:: MSA - Message Acknowledgement
   :name: tab_msa_231
   :header: SEQ,LEN,DT,OPT,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1,2,ID,R,0008,00018,**Acknowledgment Code**
   2,0,T,R,,0010,**Message Control ID**
   3,0,T,O,,00020,**Text Message**
   4,5,M,X,,00021,Expected Sequence Number
   5,1,ID,X,0102,00022,Delayed Acknowledgment Type
   6,100,X,O,,00023,Error Condition

Element names in **bold** indicates that the field is used by |product|.

In case that |product| does not recognize either the message type (MSH-9.1) or the trigger event (MSH-9.2) in a
message, *MSA-1-Acknowledgement code* of the acknowledgement contain the value ``AR``.

If the *MSA-1-Acknowledgement code* identifies an error condition, |product| may provide an error message in
*MSA-3-Text Message*.

Error Modes
^^^^^^^^^^^

This segment contains information sent while acknowledging another message in error cases. The segment is documented only
for reference and backward compatibility. It is not used by the |product|.

.. csv-table:: ERR - Error Segment
   :name: tab_err_231
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,80,CM,R,Y,,00024,Error Code and Location

HL7 v2.5 Message Implementation Requirements
--------------------------------------------

.. _ack_message_25:

Acknowledgement Message
^^^^^^^^^^^^^^^^^^^^^^^

Each HL7 message shall be acknowledged by the HL7 ACK message sent by the receiver of an HL7 message to its sender.
The segments of the ACK message listed below are required, and their detailed descriptions are provided in the
following subsections. The ERR segment is optional and will be included in ACK messages sent by |product| from version 5.10.6 onwards.

.. csv-table:: Common ACK Message static definition
   :header: Segment,Meaning,Usage,Card.,HL7 chapter
   :widths: 15,40,15,15,15

   MSH - :ref:`tab_msh_251`,Message Header,R,[1..1],2
   MSA - :ref:`tab_msa_251`,Message Acknowledgement,R,[1..1],2
   [ERR] - :ref:`tab_err_251`,Error,C,[0..*],2

.. _message_control_25:

Message Control
^^^^^^^^^^^^^^^

The MSH (message header) segment contains control information set in the beginning of each message sent.

.. csv-table:: MSH - Message Header segment (HL7 v2.5.1)
   :name: tab_msh_251
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,1,SI,R,[1..1],,00001,**Field Separator**
   2,4,ST,R,[1..1],,00002,**Encoding Characters**
   3,227,HD,R,[1..1],,00003,**Sending Application**
   4,227,HD,R,[1..1],,00004,**Sending Facility**
   5,227,HD,R,[1..1],,00005,**Receiving Application**
   6,227,HD,R,[1..1],,00006,**Receiving Facility**
   7,26,TS,R,[1..1],,00007,Date/Time of Message
   8,40,ST,X,[0..0],,00008,Security
   9,15,MSG,R,[1..1],,00009,**Message Type**
   10,20,ST,R,[1..1],,00010,**Message Control Id**
   11,3,PT,R,[1..1],,00011,Processing Id
   12,60,VID,R,[1..1],,00012,Version ID
   13,15,NM,X,[0..1],,00013,Sequence Number
   14,180,ST,X,[0..0],,00014,Continuation Pointer
   15,2,ID,X,[0..0],0155,00015,Accept Acknowledgement Type
   16,2,ID,X,[0..0],0155,00016,Application Acknowledgement Type
   17,3,ID,RE,[1..1],0399,00017,Country Code
   18,16,ID,C,[0..1],0211,00692,**Character Set**
   19,250,CE,RE,[1..1],,00693,Principal Language of Message
   20,20,ID,X,[0..0],0356,01317,Alternate Character Set Handling Scheme
   21,427,EI,RE,[0..*],,01598,Message Profile Identifier

Element names in **bold** indicates that the field is used by |product|.

|product| only supports the HL7-recommended values for the fields *MSH-1-Field Separator* (= ``|``) and
*MSH-2-Encoding Characters* (= ``^~\&``).

|product| does not support the sequence number protocol (it does not make use of field *MSH-13-Sequence Number*).

Field *MSH-18-Character Set* contains the character set for the entire message. Refer to
`HL7 Table 0211 - Alternate character sets <https://www.hl7.org/fhir/v2/0211/index.html>`_ for valid values.

Examples of valid values:

``8859/1``
   The printable characters from the ISO 8859/1 Character set used by Western Europe.
``ISO IR87``
   Code for the Japanese Graphic Character set for information interchange (JIS X 0208-1990).
``GB 18030-2000``
   Code for the Chinese Character Set (GB 18030-2000).
``UNICODE UTF-8``
   UCS Transformation Format, 8-bit form.

*MSH-18-Character Set* shall only be valued if the message uses a character set other than the 7-bit ASCII character set.
Though the field is repeatable in HL7, |product| supports only one occurrence (i.e., one character set).
The character set specified in this field is used for the encoding of all of the characters within the message.

*MSH-20-Alternate Character Set Handling Scheme* is not supported by |product|.

Acknowledgement Modes
^^^^^^^^^^^^^^^^^^^^^

This segment contains information sent while acknowledging another message.

.. csv-table:: MSA - Message Acknowledgement
   :name: tab_msa_251
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,2,ID,R,[1..1],0008,00018,**Acknowledgement code**
   2,20,ST,R,[1..1],,00010,**Message Control Id**
   3,80,ST,O,[0..1],,00020,**Text Message**
   4,15,NM,X,[0..0],,00021,Expected Sequence Number
   5,,,X,[0..0],,00022,Delayed Acknowledgment Type
   6,250,CE,X,[0..0],0357,00023,Error Condition

Element names in **bold** indicates that the field is used by |product|.

In case that |product| does not recognize either the message type (MSH-9.1) or the trigger event (MSH-9.2) in a
message, *MSA-1-Acknowledgement code* of the acknowledgement contain the value ``AR``.

If the *MSA-1-Acknowledgement code* identifies an error condition, |product| may provide an error message in
*MSA-3-Text Message*.

Error Modes
^^^^^^^^^^^

This segment is used to add error codes and comments to acknowledgment messages.

.. csv-table:: ERR - Error Segment
   :name: tab_err_251
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,493,ELD,B,Y,,00024,Error Code and Location
   2,18,ERL,O,Y,,01812,**Error Location**
   3,705,CWE,R,,0357,01813,**HL7 Error Code**
   4,2,ID,R,,0516,01814,**Severity**
   5,705,CWE,O,,0533,01815,Application Error Code
   6,80,ST,O,Y/10,,01816,Application Error Parameter
   7,2048,TX,O,,,01817,Diagnostic Information
   8,250,TX,O,,,01818,**User Message**
   9,20,IS,O,Y,0517,01819,Inform Person Indicator
   10,705,CWE,O,,0518,01820,Override Type
   11,705,CWE,O,Y,0519,01821,Override Reason Code
   12,652,XTN,O,Y,,01822,Help Desk Contact Point

Element names in **bold** indicates that the field is used by |product|.

.. _hl7_and_dicom_mapping_considerations:

HL7 and DICOM Mapping Considerations
------------------------------------

Field lengths are explicitly defined in the DICOM standard, but an HL7 element might consist of multiple components
that do not have a defined maximum length. It is recognized that there are some HL7 component lengths that could be
longer than the DICOM attribute lengths. Data values for mapped fields are required not to exceed the smaller of either
the HL7 or the DICOM field length definitions. Systems supporting alternative character sets must take into account the
number of bytes per character in such sets.

|product| maps the value of *MSH-18-Character Set* to the corresponding code value of DICOM attribute
*(0008,0005) Specific Character Set*:

.. csv-table:: Mapping of *MSH-18-Character Set* to *(0008,0005) Specific Character Set*
   :name: tab_hl7_dicom_charset
   :widths: 30, 30, 40
   :header: HL7 MSH-18,"DICOM (0008,0005)",Character Set

   8859/1,ISO_IR 100,Latin alphabet No. 1
   8859/2,ISO_IR 101,Latin alphabet No. 2
   8859/3,ISO_IR 109,Latin alphabet No. 3
   8859/4,ISO_IR 110,Latin alphabet No. 4
   8859/5,ISO_IR 144,Cyrillic
   8859/6,ISO_IR 127,Arabic
   8859/7,ISO_IR 126,Greek
   8859/8,ISO_IR 138,Hebrew
   8859/9,ISO_IR 148,Latin alphabet No. 5
   ISO IR14,ISO_IR 13,Japanese (JIS X 0201-1976)
   ISO IR87,ISO 2022 IR 87,Japanese (JIS X 0208-1990)
   ISO IR159,ISO 2022 IR 159,Japanese (JIS X 0212-1990)
   KS X 1001,ISO 2022 IR 149,Korean
   CNS 11643-1992,ISO_IR 166,Thai
   UNICODE UTF-8,ISO_IR 192,Unicode in UTF-8
   GB 18030-2000,GB18030,Chinese Character Set (GB 18030-2000)

Error Codes Mapping
-------------------

Following table gives an overview of error codes and messages sent by |product| for incoming HL7 messages triggering
error conditions.

.. csv-table:: Error Codes Mapping and Usage
   :name: tab_hl7_error
   :header: Error Code,Error Code Meaning,Error Location,User Message,Notes

   101,Required Field Missing,MSH^1^3^1^1,Missing Sending Application,
   ,,MSH^1^4^1^1,Missing Sending Facility,
   ,,MSH^1^5^1^1,Missing Receiving Application,
   ,,MSH^1^6^1^1,Missing Receiving Facility,
   ,,MSH^1^7^1^1,Missing Date/Time of Message,
   ,,MSH^1^9^1^1,Missing Message Type,
   ,,MSH^1^10^1^1,Missing Message Control ID,
   103,Table Value Not Found,MSH^1^3^1^1,Sending Application and/or Facility not recognized,[#Note1]_
   ,,MSH^1^5^1^1,Receiving Application and/or Facility not recognized,[#Note2]_
   200,Unsupported Message Type,MSH^1^9^1^1,Message Type - Message Code not supported,
   201,Unsupported Event Code,MSH^1^9^1^2,Message Type - Trigger Event not supported,
   207,Application Internal Error,,No HL7 Message Listener configured,[#Note3]_

.. [#Note1] Caused by mismatch of Sending Application with Facility in incoming HL7 messages with configured list of
   `Accepted Sending Application(s) <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7Application.html#hl7acceptedsendingapplication>`

.. [#Note2] Caused by misconfigured `Network Connection <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/networkConnection.html>`
   of a `HL7 Application <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7Application.html>` in a
   device.

.. [#Note3] Caused by incorrect HL7 application configuration or issues in HL7 service registry initialization in archive.