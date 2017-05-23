Application Data Flow
^^^^^^^^^^^^^^^^^^^^^

The core component of |product| is a Java Enterprise Application deployed in `WildFly AS <http://www.wildfly.org/>`_,
which provides DICOM services over the DICOM Upper Layer protocol (DUL) and HTTP, HL7 v2 services over the Minimal Lower
Layer Protocol (MLLP), various proprietary RESTful services and a Web UI accessable by HTML 5 compliant web browsers.

Conceptually the HL7 network services may be modeled as the following separate HL7 applications, though they may share
one HL7 Application name, or one HL7 Application may have multiple instances identified by different HL7 Application
names, with different configuration.

- :doc:`fktdefs/adt`, which processes ADT messages received from external Patient Demographics Sources, updating the
  patient information of archived DICOM objects, and send ADT messages to external Patient Demographics Consumers on
  patient information changes initiated via the Web UI or proprietary RESTful services of |product|.
- :doc:`fktdefs/orm`, which processes HL7 order messages from external Order Filler applications, updating
  order information in the provided DICOM Modality Worklist and already archived DICOM objects, and send HL7 order
  messages to external Order Fillers on procedure status updates.
- :doc:`fktdefs/oru`, which converts HL7 ORU messages received from external HL7 report applications to DICOM Text SR
  objects, to make them accessible via the DICOM query/retrieve services.
- :doc:`fktdefs/fwd`, which forwards arbitrary HL7 message received from one external HL7 application to another
  external HL7 application according configurable rules.

.. only:: html

   .. figure:: application-data-flow-diagram.svg

      Application Data Flow Diagram

.. only:: latex

   .. figure:: application-data-flow-diagram.pdf

      Application Data Flow Diagram
