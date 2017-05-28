Introduction
************

.. _revision:

Revision History
================

.. csv-table:: Revision History
   :header: "Document Version", "Date of Issue", "Author", "Description"

   |release|, |today|, gz, Initial Draft

.. _audience:

Audience
========
This document is written for the people that need to understand how |product| will integrate into their
healthcare facility. This includes both those responsible for overall imaging network policy and architecture,
as well as integrators who need to have a detailed understanding of the HL7 features of the product. This
document contains some basic HL7 definitions so that any reader may understand how this product implements
HL7 features. However, integrators are expected to fully understand all the HL7 terminology, how the tables
in this document relate to the product's functionality, and how that functionality integrates with other devices
that support compatible HL7 features.

.. _remarks:

Remarks
=======
The scope of this HL7 Conformance Statement is to facilitate integration between |product| and other
HL7 v2 products. The Conformance Statement should be read and understood in conjunction with the HL7 Standard.
HL7 by itself does not guarantee interoperability. The Conformance Statement does, however, facilitate a
first-level comparison for interoperability between different applications supporting compatible HL7 functionality.

This Conformance Statement is not supposed to replace validation with other HL7 equipment to ensure proper exchange
of intended information. In fact, the user should be aware of the following important issues:

* The comparison of different Conformance Statements is just the first step towards assessing interconnectivity and
  interoperability between the product and other HL7 conformant equipment.

* Test procedures should be defined and executed to validate the required level of interoperability with specific
  compatible HL7 equipment, as established by the healthcare facility.

* |product| has participated in an industry-wide testing program sponsored by Integrating the Healthcare
  Enterprise (IHE). The IHE Integration Statement for |product|, together with the IHE Technical Framework,
  may facilitate the process of validation testing.

.. _terms:

Terms and Definitions
=====================
Informal definitions are provided for the following terms used in this Conformance Statement. The HL7 Standard is
the authoritative source for formal definitions of these terms.

.. glossary::

   ACK
      General Acknowledgment message.

   ADT
      Admission, Discharge and Transfer.

   DICOM
      Digital Imaging and Communications in Medicine.

   DICOM SR
      DICOM Structured Reporting.

   HL7
      Health Level Seven (HL7) is an application protocol for electronic data exchange in health care environments.

   HL7 Application
      An end point of a HL7 information exchange; i.e., the software that sends or receives HL7 messages.
      A single device may have multiple HL7 Applications.

   HL7 Qualified Application Name
      The externally known name and facility used to identify a HL7 application to other HL7 applications on the network.

   MLLP
      Minimal Lower Layer Protocol.

   MPPS
      Modality Performed Procedure Step.

   MWL
      Modality Worklist.

   ORM
      General Order message.

   ORU
      Unsolicited Transmission of an Observation.
