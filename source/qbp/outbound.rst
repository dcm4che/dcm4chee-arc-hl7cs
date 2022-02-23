Outbound
########

The `Patient Demographics Query - Find Candidates <https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_TF_Vol2a.pdf#page=154>`_
HL7 message is sent to other HL7 applications/receivers which act as Patient Demographics Suppliers requesting information
about patients whose demographic data match data provided in query message.

This message may be triggered either by `Patient Demographics Query services <https://petstore.swagger.io/index.html?url=https://raw.githubusercontent.com/dcm4che/dcm4chee-arc-light/master/dcm4chee-arc-ui2/src/swagger/openapi.json#/PDQ-RS>`_
or by patient verification scheduler.

.. _qbp_out_message:

Outbound Message
================

.. _qbp_out_qbp_q22:

QBP - Query by Parameter Message (Event Q22)
--------------------------------------------
Supported HL7 version: 2.5.1 (ITI-21)

Trigger Event
^^^^^^^^^^^^^
This message is sent out by the archive when patient demographics is to be requested from external patient demographics
suppliers

Supported Segments
^^^^^^^^^^^^^^^^^^
The following segments are sent in an outgoing QBP^Q22^QBP_Q22 message:

.. csv-table:: Supported segments of QBP^Q22^QBP_Q22 (HL7 v2.5.1)
   :header: Segment, Meaning, Usage, Card., HL7 chapter
   :widths: 15, 40, 15, 15, 15

   MSH - :ref:`tab_msh_251`, Message Header, R, [1..1], 2
   QPD - :ref:`tab_qbp_qpd_out_251`, Query Parameter Definition, R, [1..1], 5
   RCP - :ref:`tab_qbp_rcp_out_251`, Response Control Parameter, R, [1..1], 5

Expected Actions
^^^^^^^^^^^^^^^^
The patient demographics suppliers acting as receiver shall follow stated `Expected Actions <https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_TF_Vol2a.pdf#page=160>`_.

.. _qbp_out_segments:

Outbound Message Segments
=========================

.. _qbp_out_msh:

MSH - Message Header segment
----------------------------
Same as specified in :ref:`tab_msh_251`

.. _qbp_out_qpd:

QPD - Query Parameter Definition segment
----------------------------------------

.. csv-table:: QPD - Query Parameter Definition segment (HL7 v2.5.1)
   :name: tab_qbp_qpd_out_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 250, CE, R, 0471, 01375, **Message Query Name**
   2, 32, ST, C, , 00696, **Query Tag**
   3-n, 256, varies, , , 01435, **User Parameters (in successive fields)**

.. _qbp_out_rcp:

RCP - Response Control Parameter segment
----------------------------------------

.. csv-table:: RCP - Response Control Parameter segment (HL7 v2.5.1)
   :name: tab_qbp_rcp_out_251
   :header: SEQ, LEN, DT, OPT, TBL#, ITEM #, Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1, 1, ID, O, 0091, 00027, **Query Priority**
   2, 10, CQ, O, 0126, 00031, Quantity Limited Request
   3, 250, CE, O, 0394, 01440, Response Modality
   4, 26, TS, C, , 01441, Execution and Delivery Time
   5, 1, ID, O, 0395, 01443, Modify Indicator
   6, 512, SRT, O, , 01624, Sort-by Field
   7, 256, ID, , , 01594, Segment group inclusion

Element names in **bold** indicates that the field is used by |product|.

.. _qbp_out_dicom:

DICOM to HL7 Query by Parameter Message Mapping
===============================================

Mappings between HL7 and DICOM are illustrated in the following manner:

- Element Name (HL7 item_number.component.sub-component #/ DICOM (group, element))
- The component/sub-component value is not listed if the HL7 element should not contain multiple components/sub-components.

.. _qbp_out_qbp_q22_dicom:

QBP - DICOM Patient Attributes to HL7 Query by Parameter Message mapping
------------------------------------------------------------------------

.. csv-table:: DICOM Patient Attributes to HL7 Query by Parameter Message mapping
   :name: dicom_to_qbp
   :header: HL7 Field, HL7 Item #, HL7 Segment, DICOM Attribute, DICOM Tag, Note

   Message Query Name, 01375, QPD:1, , , Set to 'IHE PDQ Query'
   Query Tag, 00696, QPD:2, , , Set to 'QRY' + value from MSH:9
   User Parameters (in successive fields), 01435, QPD:3, , , [#Note1]_
   , , , Patient ID, "(0010, 0020)", Sent as '@PID.3.1^' + value of Patient ID
   , , , Issuer of Patient ID, "(0010, 0021)", Sent as '@PID.3.4.1^' + value of Issuer of Patient ID
   , , , Issuer of Patient ID Qualifiers Sequence, "(0010, 0024)",
   , , , >Item, "(FFFE, E000)",
   , , , >Universal Entity ID, "(0040, 0032)", Sent as '@PID.3.4.2^' + value of Universal Entity ID
   , , , >Universal Entity ID Type, "(0040, 0033)", Sent as '@PID.3.4.3^' + value of Universal Entity ID Type
   Query Priority, 00027, RCP:1, , , Set to I

.. [#Note1] The patient ID together with any of the assigning authorities is sent in QPD:3 with shown syntax each of the
   user parameters separated by HL7 repetition character ~