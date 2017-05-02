HL7 Conformance Statement Overview
**********************************

|product| is a networked computer system used for archiving DICOM objects. It allows external systems
to send DICOM objects to it for permanent storage, retrieve information about such objects, and retrieve
the DICOM objects themselves. It also provides services to maintain the consistency of patient and
ordering information with external systems.

In order to offer these services |product| utilizes and supports different medical communication standards.
Apart from DICOM services, which are described in a dedicated Conformance Statement, |product| uses HL7 V2
services to communicate with other medical systems.

.. csv-table:: HL7 Network Services
   :header: "HL7 message", "HL7 version", "Sender", "Receiver"
   :widths: 65, 15, 10, 10
   :file: network-services.csv
