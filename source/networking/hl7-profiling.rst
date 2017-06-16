HL7 Profiling Conventions
=========================

This document utilizes the HL7 Profiling Conventions specified in
`IHE Radiology Technical Framework, Volume 2 <http://ihe.net/uploadedFiles/Documents/Radiology/IHE_RAD_TF_Vol2.pdf>`_,
Section 2.3.

The HL7 tables included in this document have been modified from the corresponding HL7 standard documents. Such a
modification is called a profile using static definitions as described for HL7 constrainable message profiles;
refer to HL7 v2.5.1, Chapter 2, Section 2.12.6.

The static definition of a profiled message is represented within tables in this document. The message level table
represents the profiled message structure with its list of usable segments. The segment level table represents the
profiled content of one segment with its usable fields.

.. _static_definition_segment:

Static definition - Segment level and Data Type level
-----------------------------------------------------

The Segment table and the Data Type table each contain 8 columns (HL7 v2.3.1 messages use only 7 columns) as described
below:

SEQ
   Position (sequence) of the field within the segment.

LEN
   Maximum length of the field.

   Since version 2.5, the HL7 standard also defines the maximum length of each component with a field.
   Profiled HL7 messages shall conform to the HL7 standard if not otherwise stated in this document.

DT
   Field Data Type

Usage
   Usage of the field (column noted as OPT in HL7 v2.3.1 message static definition.)

   The coded values used in this column are

   R
      Required: A compliant sending application shall populate all **R** elements with a non-empty value. A compliant
      receiving application may ignore the information conveyed by required elements. A compliant receiving application
      shall not raise an error due to the presence of a required element, but may raise an error due to the
      absence of a required element.

      **R** in **bold** indicates that the field is used by |product|.

   R+
      Required as extension: This is a field optional in the original HL7 standard but required in the profiled messages.
      Only HL7 v2.3.1 messages use this notation to indicate the difference between OPT in the profiles and in the
      base HL7 standard.

      **R+** in **bold** indicates that the field is used by |product|.

   RE
      Required but may be empty. (**R2** in HL7 v2.3.1 messages)

      The element may be missing from the message, but shall be sent by the sending application if there is relevant
      data. A conformant sending application shall be capable of providing all **RE** elements. If the conformant
      sending application knows a value for the element, then it shall send that value. If the conformant sending
      application does not know a value, then that element may be omitted. Receiving applications may ignore data
      contained in the element, but shall be able to successfully process the message if the element is omitted
      (no error message should be generated if the element is missing).

      **RE** (**R2** in HL7 v2.3.1 messages) in **bold** indicates that the field is used by |product|.

   O
      Optional. The usage for this field within the message is not defined. The sending application may choose to
      populate the field; the receiving application may choose to ignore the field.

      **O** in **bold** indicates that the field is used by |product|.

   C
      Conditional. This usage has an associated condition predicate. (See HL7 v2.5.1, Chapter 2, Section 2.12.6.6,
      "Condition Predicate".)

      If the predicate is satisfied: A compliant sending application shall populate the element. A compliant receiving
      application may ignore data in the element. It may raise an error if the element is not present.

      If the predicate is NOT satisfied: A compliant sending application shall NOT populate the element. A compliant
      receiving application shall NOT raise an error if the condition predicate is false and the element is not present,
      though it may raise an error if the element IS present.

      The condition predicate is not explicitly defined when it depends on functional characteristics of the system
      implementing the transaction and it does not affect data consistency.

      **C** in **bold** indicates that the field is used by |product|.

   CE
      Conditional but may be empty. This usage has an associated condition predicate. (See HL7 Version 2.5, Chapter 2,
      Section 2.12.6.6, "Condition Predicate".)

      If the conforming sending application knows the required values for the element, then the application must
      populate the element. If the conforming sending application does not know the values required for this element,
      then the element shall be omitted.

      The conforming sending application must be capable of populating the element (when the predicate is true) for all
      **CE** elements. If the element is present, the conformant receiving application may ignore the values of that
      element. If the element is not present, the conformant receiving application shall not raise an error due to the
      presence or absence of the element.

      If the predicate is NOT satisfied: The conformant sending application shall not populate the element. The
      conformant receiving application may raise an application error if the element is present.

      **CE** in **bold** indicates that the field is used by |product|.

   X
      Not supported. For conformant sending applications, the element will not be sent.

      Conformant receiving applications may ignore the element if it is sent, or may raise an application error.

Cardinality
   Minimum and maximum number of occurrences for the field in the context of this Transaction.

   This column is not used in profiled HL7 v2.3.1 message.

TBL#
   Table reference (for fields using a set of defined values)

ITEM#
   HL7 unique reference for this field

Element Name
   Name of the field in a Segment table. / Component Name: Name of a subfield in a Data Type table.

:numref:`sample_profile` provides a sample profile for an imaginary HL7 segment. Tables for actual segments
are copied from the corresponding HL7 standard versions with modifications made only to the OPT (Usage) column.

.. csv-table:: Sample HL7 Profile
   :name: sample_profile
   :header: SEQ,LEN,DT,OPT,TBL#,ITEM #,Element Name
   :widths: 8, 8, 8, 8, 8, 12, 48

   1,1,ST,**R**,,xx001,Element 1
   2,4,ST,O,,xx002,Element 2
   3,180,HD,**R2**,,xx003,Element 3
   4,180,HD,**C**,,xx004,Element 4
   5,180,HD,O,,xx005,Element 5
   6,180,HD,R,,xx006,Element 6

.. note:: This sample table is made for HL7 v2.3.1 message definition in this document. For HL7 v2.5.1, one more column
   **Cardinality** will be added between columns **OPT** and **TBL#**.

The lengths of the fields specified in the **LEN** column of profiling tables shall be interpreted in accordance with
HL7 standard, where it indicates the calculated length of the single occurrence of the field based on the expected
maximum lengths of its individual components.

As such, |product| requires that the receiving actors are able to properly process the fields where each occurrence is
up to the maximum length specified in the HL7 profiling tables. Sending actors shall be able to generate messages where
single occurrences of fields do not exceed maximum lengths specified in the profiling tables. Both receiving and
sending actors shall take into account the mapping of values between HL7 and DICOM
(see :ref:`hl7_and_dicom_mapping_considerations`) so that values of components that are mapped into DICOM do not exceed
length limitations of that standard.

Static definition - Message level
---------------------------------

The message table representing the static definition contains 5 columns (HL7 v2.3.1 messages use only 3 columns) as
described below:

Segment
   gives the segment name, and places the segment within the hierarchy of the message structure designed by HL7.

   The beginning and end lines of a segment group (see HL7 v2.5.1, Chapter 2, Section 2.5.2 for definition) are
   designated in this column by --- (3 dashes). The square brackets and braces that designate optionality and
   repeatability are hidden.

Meaning
   Meaning of the segment as defined by HL7.

Usage
   Usage of the segment. Same coded values used in the segment level: **R**, **RE**, **O**, **C**, **CE** and **X**
   (see :ref:`static_definition_segment`)

   This column is not used in HL7 v2.3.1 messages.

Cardinality
   Minimum and maximum number of occurrences authorized for this segment in the context of the profiled HL7 message.

   This column is not used in HL7 v2.3.1 messages.

HL7 chapter
   Reference of the HL7 standard document chapter that describes this segment.