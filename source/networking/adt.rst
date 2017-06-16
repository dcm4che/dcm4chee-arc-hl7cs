Patient Management Service
==========================

The Patient Management Service processes ADT messages received from external Patient Demographics Sources, updating the
patient information of archived DICOM objects, and send ADT messages to external Patient Demographics Consumers on
patient information changes initiated via the Web UI or proprietary RESTful services of |product|.

The functionality provided by the Patient Management Service of |product| is compliant with the requirements for
following IHE Integration Profiles and Actors with Option:

.. csv-table::
   :header: "Integration Profile", "Actor", "Option"
   :widths: 40, 40, 20

   "Patient Information Reconciliation", "Image Manager/Image Archive", "HL7 v2.5.1"
   "Patient Administration Management", "Patient Demographics Consumer", "Merge"
   "Patient Administration Management", "Patient Demographics Source", "Merge"

as specified in the IHE Technical Frameworks for `Radiology <http://ihe.net/Technical_Frameworks/#radiology>`_
and `IT Infrastructure <http://ihe.net/Technical_Frameworks/#IT>`_.

.. _adt_inbound:

Inbound Messages
----------------

.. _adt_in_update:

Patient Registration and Update Messages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|product| supports following Patient Registration and Update Messages:

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
   "ADT/ACK - Cancel Admit/Visit Notification (Event A11)", "2.3.1, 2.5.1"
   "ADT/ACK - Cancel Transfer (Event A12)", "2.3.1, 2.5.1"
   "ADT/ACK - Cancel Discharge/End Visit  (Event A13)", "2.3.1, 2.5.1"
   "ADT/ACK - Add Person or Patient Information (Event A28)", "2.5"
   "ADT/ACK - Update Person Information (Event A31)", "2.5"
   "ADT/ACK - Cancel Pre-Admit (Event A38)", "2.3.1, 2.5.1"

Message Semantics (HL7 v2.3.1)
""""""""""""""""""""""""""""""

Required segments are defined below. Other segments are optional.

.. csv-table:: Patient Registration and Update Message
   :header: Segment,Meaning,HL7 Chapter
   :widths: 25, 50, 25

   MSH,Message Header,2
   EVN,Event Type,3
   PID,Patient Identification,3
   PV1,Patient Visit,3

|product| only makes use of the MSH and the PID segment, which shall be constructed as defined in
:ref:`message_control_231` and :ref:`pid_231`.

|product| acknowledges each message by sending an HL7 ACK message back to the sender as specified in
:ref:`ack_message_231`.

Message Semantics (HL7 v2.5)
""""""""""""""""""""""""""""

Required segments are defined below. Other segments are optional.

.. csv-table:: Patient Registration and Update Message
   :header: Segment,Meaning,Usage,Card.,HL7 chapter
   :widths: 15,40,15,15,15

   MSH,Message Header,**R**,[1..1],2
   EVN,Event Type,R,[1..1],2
   PID,Patient Identification,**R**,[1..1],3
   PV1,Patient Visit,R,[1..1],3

|product| only makes use of the MSH and the PID segment, which shall be constructed as defined in
:ref:`message_control_25` and :ref:`pid_25`.

|product| acknowledges each message by sending an HL7 ACK message back to the sender as specified in
:ref:`ack_message_25`.

Actions
"""""""

Patient IDs and other Patient Information are extracted from the PID segment of the received ADT message and mapped
into corresponding DICOM attributes as defined in :ref:`hl7_adt_dicom`. If a Patient record with the extracted primary
Patient ID already exists in the database, that Patient record will get updated. If there is no such Patient record a
new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.

On retrieve of DICOM objects, the potentially updated DICOM attributes from the Patient record of the DB will be
merged with the original DICOM attributes of the stored DICOM objects, so the changes in the Patient information are
reflected in the retrieved DICOM objects.

.. [#hl7NoPatientCreateMessageType] The creation of new Patient records can be suppressed for individual message types by

   .. tabularcolumns:: |p{4cm}|l|p{8cm}|l|
   .. csv-table:: Archive Device Attribute (LDAP Object: dcmArchiveDevice)
      :header: Name, Type, Description, LDAP Attribute
      :widths: 20, 7, 60, 13

      "HL7 No Patient Create Message Type(s)",string,"Message Type(s) (MessageType^TriggerEvent) of HL7 messages which are only processed, if there is already a Patient record in the database, which Patient ID matches the Patient ID in the PID or MRG segment of the message. Thus no new Patient record will be created by messages of the specified types. May be overwritten by configured values for particular Archive HL7 Application.","hl7NoPatientCreateMessageType_"

.. _adt_in_merge:

Patient Merge Message
^^^^^^^^^^^^^^^^^^^^^

|product| supports following Patient Merge Message:

.. csv-table::
   :name: adt_a40
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

   "ADT/ACK - Merge Patient - Patient Identifier List (Event A40)", "2.3.1, 2.5, 2.5.1"

Message Semantics (HL7 v2.3.1)
""""""""""""""""""""""""""""""

Required segments are defined below. Other segments are optional.

.. csv-table:: Patient Information Message
   :header: Segment,Meaning,HL7 Chapter
   :widths: 25, 50, 25

   MSH,Message Header,2
   EVN,Event Type,3
   PID,Patient Identification,3
   MRG,Merge Information,3

|product| only makes use of the MSH and the PID segment, which shall be constructed as defined in
:ref:`message_control_231`, :ref:`pid_231` and :ref:`mrg_231`.

|product| acknowledges each message by sending an HL7 ACK message back to the sender as specified in
:ref:`ack_message_231`.

Message Semantics (HL7 v2.5)
""""""""""""""""""""""""""""

Required segments are defined below. Other segments are optional.

.. csv-table:: Patient Information Message
   :header: Segment,Meaning,Usage,Card.,HL7 chapter
   :widths: 15,40,15,15,15

   MSH,Message Header,**R**,[1..1],2
   EVN,Event Type,R,[1..1],2
   PID,Patient Identification,**R**,[1..1],3
   MRG,Merge Information,**R**,[1..1],3

|product| only makes use of the MSH, PID and the MRG segment, which shall be constructed as defined in
:ref:`message_control_25`, :ref:`pid_25` and :ref:`mrg_25`.

|product| acknowledges each message by sending an HL7 ACK message back to the sender as specified in
:ref:`ack_message_25`.

Actions
"""""""

Patient IDs and other Patient Information for the dominant Patient record are extracted from the PID segment of the
received ADT message and mapped into corresponding DICOM attributes as defined in :ref:`hl7_adt_dicom`. If a Patient
record with the extracted primary Patient ID already exists in the database, that Patient record will get updated.
If there is no such Patient record a new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_.

Patient ID and the Patient name for the old Patient record are extracted from the MRG segment of the received ADT
message and mapped into corresponding DICOM attributes as defined in :ref:`hl7_adt_dicom`. If a Patient record with the
extracted primary Patient ID already exists in the database, all associated Study, MPPS and MWL records
will be moved to the Patient record with the Patient ID from the PID segment. If there is no such Patient record a
new Patient record will be inserted into the database [#hl7NoPatientCreateMessageType]_. Therefore there will be always
a Patient Record with the Patient ID from the MRG segment, which contains a reference to the *dominant* Patient Record
with the Patient ID, marking them as *merged*.

Subsequently received HL7 messages referring a *merged* Patient by its Patient ID will be rejected, whereas DICOM
objects to a *merged* Patient will be accepted. Particularly, if the Patient ID in the first received DICOM object of
a Study matches the Patient ID of a *merged* Patient record in the database, the new Study record will be associated
with the *dominant* Patient record, so the stale Patient Information in the received DICOM object will be replaced by
the updated Patient Information in the *dominant* Patient record on retrieve of DICOM objects of that Study.

.. _adt_in_change_pid:

Changing Patient ID Message
^^^^^^^^^^^^^^^^^^^^^^^^^^^

|product| supports following Patient MergeChanging Patient ID Message:

.. csv-table::
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

   "ADT/ACK - Change Patient Identifier List (Event A47)", "2.5"

Message Semantics (HL7 v2.5)
""""""""""""""""""""""""""""

Required segments are defined below. Other segments are optional.

.. csv-table:: Patient Information Message
   :header: Segment,Meaning,Usage,Card.,HL7 chapter
   :widths: 15,40,15,15,15

      MSH,Message Header,**R**,[1..1],2
      EVN,Event Type,R,[1..1],2
      PID,Patient Identification,**R**,[1..1],3
      MRG,Merge Information,**R**,[1..1],3

|product| only makes use of the MSH, PID and the MRG segment, which shall be constructed as defined in
:ref:`message_control_25`, :ref:`pid_25` and :ref:`mrg_25`.

|product| acknowledges each message by sending an HL7 ACK message back to the sender as specified in
:ref:`ack_message_25`.

Actions
"""""""

Patient IDs and other Patient Information for the Patient record are extracted from the PID segment of the
received ADT message and mapped into corresponding DICOM attributes as defined in :ref:`hl7_adt_dicom`. If a Patient
record with the extracted primary Patient ID already exists in the database, the message will be rejected.

Patient ID and the Patient name for the old Patient record are extracted from the MRG segment of the received ADT
message and mapped into corresponding DICOM attributes as defined in :ref:`hl7_adt_dicom`.

Further behavior depends on

.. tabularcolumns:: |p{4cm}|l|p{8cm}|l|
.. csv-table:: Archive Device Attribute (LDAP Object: dcmArchiveDevice)
   :header: Name, Type, Description, LDAP Attribute
   :widths: 20, 7, 60, 13

   "HL7 Track Changed Patient ID",boolean,"Enable to keep track of the prior Patient ID on a change of the Patient ID by HL7 ADT^A47 or by the RESTful Patient Update Service.","hl7TrackChangedPatientID"

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

Consequently, subsequently received HL7 messages with the previous Patient ID will be accepted, causing the insert of a new
Patient record in the database with the previous Patient ID. Also the receive of DICOM objects with the previous
Patient ID will then cause the insert of a new Patient record, associated with the new received Study.

.. _adt_outbound:

Outbound Messages
-----------------

|product| provide RESTful services for Patient Management, which are also used by its UI. |product| may be configured
to send ADT messages to external Patient Demographics Consumers to synchronize them with the changes of the
Patient Information performed by the RESTful services.

.. csv-table:: Emitted ADT messages
   :header: "HL7 message", "HL7 version"
   :widths: 80, 20

   "ADT/ACK - Add Person or Patient Information (Event A28)", "2.5"
   "ADT/ACK - Update Person Information (Event A31)", "2.5"
   "ADT/ACK - Merge Patient - Patient Identifier List (Event A40)", "2.5"
   "ADT/ACK - Change Patient Identifier List (Event A47)", "2.5"

.. _adt_in_seg:

Input Segment Description
-------------------------

.. _pid_231:

PID - Patient Identification segment (HL7 v2.3.1)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:numref:`tab_pid_231` identifies required and optional fields of the PID segment.

OPT value in **bold** indicates that the field is used by |product|.

.. csv-table:: PID segment
   :name: tab_pid_231
   :header: SEQ,LEN,DT,OPT,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1,4,SI,O,,00104,SetID -Patient ID
   2,20,CX,**O**,,00105,Patient ID
   3,20,CX,**R**,,00106,Patient Identifier List
   4,20,CX,**O**,,00107,Alternate Patient ID
   5,48,XPN,**R**,,00108,Patient Name
   6,48,XPN,**O**,,00109,Mother’s Maiden Name
   7,26,TS,**R2**,,00110,Date/Time of Birth
   8,1,IS,**R**,0001,00111,Sex
   9,48,XPN,**O**,,00112,Patient Alias
   10,80,CE,R2,0005,00113,Race
   11,1,06,XAD,R2,00114,Patient Address
   12,4,IS,O,,00115,County Code
   13,40,XTN,O,,00116,Phone Number - Home
   14,40,XTN,O,,00117,Phone Number - Business
   15,60,CE,O,0296,00118,Primary Language
   16,1,IS,O,0002,00119,Marital Status
   17,80,CE,O,0006,00120,Religion
   18,20,CX,C,,00121,Patient Account Number
   19,16,ST,O,,00122,SSN Number – Patient
   20,25,DLN,O,,00123,Driver's License Number - Patient
   21,20,CX,O,,00124,Mother's Identifier
   22,80,CE,O,0189,00125,Ethnic Group
   23,60,ST,O,,00126,Birth Place
   24,1,ID,O,0136,00127,Multiple Birth Indicator
   25,2,NM,O,,00128,Birth Order
   26,80,CE,O,0171,00129,Citizenship
   27,60,CE,O,0172,00130,Veterans Military Status
   28,80,CE,O,,00739,Nationality
   29,26,TS,O,,00740,Patient Death Date and Time
   30,1,ID,O,0136,00741,Patient Death Indicator

Patient IDs included in the PID-3 field shall include Assigning Authority (Component 4). The first subcomponent
(namespace ID) of Assigning Authority shall be populated. If the second and third subcomponents (universal ID and
universal ID type) are also populated, they shall reference the same entity as is referenced in the first subcomponent.

This field may be populated with various identifiers assigned to the patient by various assigning authorities.

.. _pid_25:

PID - Patient Identification segment (HL7 v2.5)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:numref:`tab_pid_25` identifies required and optional fields of the PID segment

Usage value in **bold** indicates that the field is used by |product|.

.. csv-table:: PID - Patient Identification segment
   :name: tab_pid_25
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,4,SI,O,[0..1],,00104,Set ID - PID
   2,20,CX,**O**,[0..0],,00105,Patient ID
   3,250,CX,**R**,[1..*],,00106,Patient Identifier List
   4,20,CX,**O**,[0..0],,00107,Alternate Patient ID - PID
   5,250,XPN,**R**,[1..*],,00108,Patient Name
   6,250,XPN,**O**,[0..1],,00109,Mother’s Maiden Name
   7,26,TS,**CE**,[0..1],,00110,Date/Time of Birth
   8,1,IS,**CE**,[1..1],0001,00111,Administrative Sex
   9,250,XPN,**O**,[0..1],,00112,Patient Alias
   10,250,CE,O,[0..1],0005,00113,Race
   11,250,XAD,CE,[0..*],,00114,Patient Address
   12,4,IS,X,[0..1],0289,00115,County Code
   13,250,XTN,O,[0..*],,00116,Phone Number - Home
   14,250,XTN,O,[0..*],,00117,Phone Number - Business
   15,250,CE,O,[0..1],0296,00118,Primary Language
   16,250,CE,O,[0..1],0002,00119,Marital Status
   17,250,CE,O,[0..1],0006,00120,Religion
   18,250,CX,C,[0..1],,00121,Patient Account Number
   19,16,ST,X,[0..1],,00122,SSN Number - Patient
   20,25,DLN,X,[0..1],,00123,Driver's License Number - Patient
   21,250,CX,O,[0..*],,00124,Mother's Identifier
   22,250,CE,O,[0..1],0189,00125,Ethnic Group
   23,250,ST,O,[0..1],,00126,Birth Place
   24,1,ID,O,[0..1],0136,00127,Multiple Birth Indicator
   25,2,NM,O,[0..1],,00128,Birth Order
   26,250,CE,O,[0..1],0171,00129,Citizenship
   27,250,CE,O,[0..1],0172,00130,Veterans Military Status
   28,250,CE,X,[0..0],0212,00739,Nationality
   29,26,TS,CE,[0..1],,00740,Patient Death Date and Time
   30,1,ID,C,[0..1],0136,00741,Patient Death Indicator
   31,1,ID,CE,[0..1],0136,01535,Identity Unknown Indicator
   32,20,IS,CE,[0..*],0445,01536,Identity Reliability Code
   33,26,TS,CE,[0..1],,01537,Last Update Date/Time
   34,241,HD,O,[0..1],,01538,Last Update Facility
   35,250,CE,**CE**,[0..1],0446,01539,Species Code
   36,250,CE,**C**,[0..1],0447,01540,Breed Code
   37,80,ST,O,[0..1],,01541,Strain
   38,250,CE,O,[0..2],,01542,Production Class Code
   39,250,CWE,O,[0..*],,01840,Tribal Citizenship

Patient IDs included in the PID-3 field shall include Assigning Authority (Component 4). The first subcomponent
(namespace ID) of Assigning Authority shall be populated. If the second and third subcomponents (universal ID and
universal ID type) are also populated, they shall reference the same entity as is referenced in the first subcomponent.

This field may be populated with various identifiers assigned to the patient by various assigning authorities.

.. _mrg_231:

MRG - Merge segment (HL7 v2.3.1)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:numref:`tab_mrg_231` identifies required and optional fields of the MRG segment:

OPT value in **bold** indicates that the field is used by |product|.

.. csv-table:: MRG - Merge segment
   :name: tab_mrg_231
   :header: SEQ,LEN,DT,OPT,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1,20,CX,**R**,,00211,Prior Patient Identifier List
   2,20,CX,O,,00212,Prior Alternate Patient ID
   3,20,CX,O,,00213,Prior Patient Account Number
   4,20,CX,R2,,00214,Prior Patient ID
   5,20,CX,O,,01279,Prior Visit Number
   6,20,CX,O,,01280,Prior Alternate Visit ID
   7,48,XPN,**R2**,,01281,Prior Patient Name

.. _mrg_25:

MRG - Merge segment (HL7 v2.5)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:numref:`tab_mrg_25` identifies required and optional fields of the MRG segment:

Usage value in **bold** indicates that the field is used by |product|.

.. csv-table:: MRG - Merge segment
   :name: tab_mrg_25
   :header: SEQ,LEN,DT,Usage,Card.,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 8, 12, 40

   1,250,CX,**R**,[1..*],,00211,Prior Patient Identifier List
   2,250,CX,X,[0..0],,00212,Prior Alternate Patient ID
   3,250,CX,O,[0..1],,00213,Prior Patient Account Number
   4,250,CX,X,,[0..0],00214,Prior Patient ID
   5,250,CX,X,[0..0],,01279,Prior Visit Number
   6,250,CX,X,[0..0],,01280,Prior Alternate Visit ID
   7,250,XPN,**O**,[0..*],,01281,Prior Patient Name

.. _adt_out_seg:

Output Segment Description
--------------------------

<TODO>

.. _hl7_adt_dicom:

HL7 ADT mapping to DICOM Patient Attributes
-------------------------------------------

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component / sub-component value is not listed if the HL7 element does not contain multiple components / sub-components.

.. csv-table:: HL7 ADT mapping of PID segment to DICOM Patient Attributes
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note
   :widths: 20, 12, 20, 12, 12, 26

   **SOP Common**,,,,,
   Specific Character Set, "(0008,0005)", Character Set, 00692, MSH:18, :numref:`tab_hl7_dicom_charset`
   **Patient Identification**,,,,,
   Patient's Name, "(0010,0010)", Patient  Name, 00108, PID:5,
   Patient ID, "(0010,0020)", Patient Identifier List, 00106.1, PID:3.1,
   Issuer of Patient ID, "(0010,0021)", Patient Identifier List, 00106.4.1, PID:3.4.1,
   Issuer of Patient ID Qualifiers Sequence, "(0010,0024)",,,,
   >Universal Entity ID, "(0040,0032)", Patient Identifier List, 00106.4.2, PID:3.4.2,
   >Universal Entity ID Type, "(0040,0033)", Patient Identifier List, 00106.4.3, PID:3.4.3,
   Other Patient IDs Sequence, "(0010,1002)",,,,
   >Item,,,,, if PID:2 not empty
   >Patient ID, "(0010,0020)", Patient ID, 00105.1, PID:2.1,
   >Issuer of Patient ID, "(0010,0021)", Patient ID, 00105.4.1, PID:2.4.1, "set to ``CHIP``, if PID:2.4.1 empty"
   >Issuer of Patient ID Qualifiers Sequence, "(0010,0024)",,,,
   >>Universal Entity ID, "(0040,0032)", Patient Identifier List, 00105.4.2, PID:2.4.2,
   >>Universal Entity ID Type, "(0040,0033)", Patient Identifier List, 00105.4.3, PID:2.4.3,
   >Type of Patient ID, "(0010,0022)",,,, set to ``RFID``
   >Item,,,,, if PID:4 not empty
   >Patient ID, "(0010,0020)", Alternate Patient ID - PID, 00107.1, PID:4.1,
   >Issuer of Patient ID, "(0010,0021)", Alternate Patient ID - PID, 00107.4.1, PID:4.4.1, "set to ``TATTOO``, if PID:4.4.1 empty"
   >Type of Patient ID, "(0010,0022)",,,, set to ``BARCODE``
   >Issuer of Patient ID Qualifiers Sequence, "(0010,0024)",,,,
   >>Universal Entity ID, "(0040,0032)", Patient Identifier List, 00107.4.2, PID:4.4.2,
   >>Universal Entity ID Type, "(0040,0033)", Patient Identifier List, 00107.4.3, PID:4.4.3,
   Patient's Mother's Birth Name, "(0010,1060)", Mother’s Maiden Name, 00109, PID:6,
   **Patient Demographic**,,,,,
   Patient's Birth Date, "(0010,0030)", Date/Time of Birth, 00110, PID:7,
   Patient's Sex, "(0010,0040)", Administrative Sex, 00111.1, PID:8.1,
   Responsible Person, "(0010,2297)", Patient Alias, 00112, PID:9,
   Responsible Person Role, "(0010,2298)",,, "set to ``OWNER``, if PID:9 is not empty"
   Patient Species Description, "(0010,2201)", Species Code, 01539.2, PID:35.2,
   Patient Species Code Sequence, "(0010,2202)",,,,
   >Code Value, "(0008,0100)", Species Code, 01539.1, PID:35.1,
   >Coding Scheme Designator, "(0008,0102)", Species Code, 01539.3, PID:35.3,
   >Code Meaning, "(0008,0104)", Species Code, 01539.2, PID:35.2,
   Patient Breed Description, "(0010,2292)", Breed Code, 01540.2, PID:36.2,
   Patient Breed Code Sequence, "(0010,2293)",,,,
   >Code Value, "(0008,0100)", Breed Code, 01540.1, PID:36.1,
   >Coding Scheme Designator, "(0008,0102)", Breed Code, 01540.3, PID:36.3,
   >Code Meaning, "(0008,0104)", Breed Code, 01540.2, PID:36.2,
   **Patient Medical**,,,,,
   Patient's Sex Neutered, "(0010,2203)", Administrative Sex, 00111.2, PID:8.2, "`Y`⇒`ALTERED`, `N`⇒`UNALTERED`"

.. csv-table:: HL7 ADT mapping of MRG segment to DICOM Patient Attributes
   :header: DICOM Attribute, DICOM Tag, HL7 Field, HL7 Item #, HL7 Segment, Note
   :widths: 20, 12, 20, 12, 12, 26

   **SOP Common**,,,,,
   Specific Character Set, "(0008,0005)", Character Set, 00692, MSH:18, :numref:`tab_hl7_dicom_charset`
   ***Patient Identification**,,,,,
   Patient's Name, "(0010,0010)", Prior Patient  Name, 01281, MRG:7,
   Patient ID, "(0010,0020)", Prior Patient Identifier List, 00211.1, MRG:1.1,
   Issuer of Patient ID, "(0010,0021)", Prior Patient Identifier List, 00211.1.1, MRG:1.1.1,
   Issuer of Patient ID Qualifiers Sequence, "(0010,0024)",,,,
   >Universal Entity ID, "(0040,0032)", Prior Patient Identifier List, 00211.1.2, MRG:1.1.2,
   >Universal Entity ID Type, "(0040,0033)", Prior Patient Identifier List, 00211.1.3, MRG:1.1.3,
