HL7 Notification Service
========================

The archive can be configured to send out HL7 notifications to interested external HL7 systems / receivers on updates of
HL7 orders in the archive or on receive of studies in the archive.
The Observation Reporting Management Service converts HL7 ORU messages received from remote HL7 applications to DICOM
Text SR / Encapsulated CDA / Encapsulated PDF objects, to make them accessible via the DICOM query/retrieve services. It
can also be configured to send out HL7 ORU notifications on availability of imaging results i.e. DICOM studies.

.. toctree::

   Outbound OMG^O19 <../orm/outbound>
   Outbound ORU^R01 <../oru/outbound>
