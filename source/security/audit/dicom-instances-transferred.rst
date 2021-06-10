DICOM Instances Transferred
===========================

Trigger Events
--------------

This message is emitted by the archive on receive of HL7 ORU^R01 message.

Message Structure
-----------------

.. csv-table:: Entities in DICOM Instances Transferred Audit Message

    :ref:`event-identification-instances-transferred`
    :ref:`active-participant-destination-instances-transferred`
    :ref:`active-participant-source-instances-transferred`
    :ref:`audit-general-message-audit-source`
    :ref:`participant-object-study-instances-transferred`
    :ref:`participant-object-patient-instances-transferred`

.. csv-table:: Event Identification
   :name: event-identification-instances-transferred
   :widths: 30, 5, 65
   :header: Field Name, Opt, Description

   EventID, M, "| EV (110104, DCM, 'DICOM Instances Transferred')"
   EventActionCode, M, "Create ⇒ 'C'"
   EventDateTime, M, | The time at which the event occurred
   EventOutcomeIndicator, M, "| Success ⇒ '0'
   | Minor failure ⇒ '4'"
   EventOutcomeDescription, M, | Error/Exception message when EventOutcomeIndicator ⇒ '4'

.. csv-table:: Active Participant : Destination
   :name: active-participant-destination-instances-transferred
   :widths: 30, 5, 65
   :header: Field Name, Opt, Description

   UserID, M, "AE title associated with HL7 Receiving Application and Facility"
   UserIsRequestor, M, "false"
   UserIDTypeCode, U, "EV (110119, DCM, 'Station AE Title')"
   UserTypeCode, U, "Application ⇒ '2'"
   RoleIDCode, M, "| EV (110152, DCM, 'Destination Role ID')"
   NetworkAccessPointID, U, | Hostname/IP Address of calling host
   NetworkAccessPointTypeCode, U, "| NetworkAccessPointID is host name ⇒ '1'
   | NetworkAccessPointID is an IP address ⇒ '2'"

.. csv-table:: Active Participant : Source
   :name: active-participant-source-instances-transferred
   :widths: 30, 5, 65
   :header: Field Name, Opt, Description

   UserID, M, "HL7 Sending Application and Facility Name"
   UserIDTypeCode, U, "EV (HL7APP, 99DCM4CHEE, 'Application and Facility')"
   UserTypeCode, U, "Application ⇒ '2'"
   UserIsRequestor, M, | true
   RoleIDCode, M, "| EV (110153, DCM, 'Source Role ID')"
   NetworkAccessPointID, U, | Hostname/IP Address of initiating system
   NetworkAccessPointTypeCode, U, "| NetworkAccessPointID is host name ⇒ '1'
   | NetworkAccessPointID is an IP address ⇒ '2'"

.. csv-table:: Participant Object Identification : Study
   :name: participant-object-study-instances-transferred
   :widths: 30, 5, 65
   :header: Field Name, Opt, Description

   ParticipantObjectID, M, Study Instance UID or 1.2.40.0.13.1.15.110.3.165.1 if unknown
   ParticipantObjectTypeCode, M, System ⇒ '2'
   ParticipantObjectTypeCodeRole, M, Report ⇒ '3'
   ParticipantObjectIDTypeCode, M, "EV (110180, DCM, 'Study Instance UID')"
   ParticipantObjectDetail, U, "Base-64 encoded study date if Study has StudyDate(0008,0020) attribute"
   ParticipantObjectDataLifeCycle, U, "OriginationCreation ⇒ '1'"
   ParticipantObjectDescription, U
   SOPClass, MC, Sop Class UID and Number of instances with this sop class. eg. <SOPClass UID='1.2.840.10008.5.1.4.1.1.88.22' NumberOfInstances='4'/>
   Accession, U, Accession Number

.. csv-table:: Participant Object Identification : Patient
   :name: participant-object-patient-instances-transferred
   :widths: 30, 5, 65
   :header: Field Name, Opt, Description

   ParticipantObjectID, M, Patient ID or <none> if unknown
   ParticipantObjectTypeCode, M, Person ⇒ '1'
   ParticipantObjectTypeCodeRole, M, Patient ⇒ '1'
   ParticipantObjectIDTypeCode, M,  "EV (2, RFC-3881, 'Patient Number')"
   ParticipantObjectName, U, Patient Name

Sample Message
--------------

Reports stored using HL7 ORU^R01

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <AuditMessage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://www.dcm4che.org/DICOM/audit-message.rnc">
       <EventIdentification EventActionCode="C" EventDateTime="2019-02-15T17:05:47+01:00" EventOutcomeIndicator="0">
          <EventID csd-code="110104" codeSystemName="DCM" originalText="DICOM Instances Transferred" />
       </EventIdentification>
       <ActiveParticipant UserID="DCM4CHEE" AlternativeUserID="27673" UserIsRequestor="false" UserTypeCode="2" NetworkAccessPointID="localhost" NetworkAccessPointTypeCode="1">
          <RoleIDCode csd-code="110152" codeSystemName="DCM" originalText="Destination Role ID" />
          <UserIDTypeCode csd-code="110119" codeSystemName="DCM" originalText="Station AE Title" />
       </ActiveParticipant>
       <ActiveParticipant UserID="MESA_RPT_MGR|EAST_RADIOLOGY" UserIsRequestor="true" UserTypeCode="2" NetworkAccessPointID="localhost" NetworkAccessPointTypeCode="1">
          <RoleIDCode csd-code="110153" codeSystemName="DCM" originalText="Source Role ID" />
          <UserIDTypeCode csd-code="HL7APP" codeSystemName="99DCM4CHEE" originalText="Application and Facility" />
       </ActiveParticipant>
       <AuditSourceIdentification AuditSourceID="dcm4chee-arc">
          <AuditSourceTypeCode csd-code="4" />
       </AuditSourceIdentification>
       <ParticipantObjectIdentification ParticipantObjectID="2.25.185448987116626056864758237726880870790" ParticipantObjectTypeCode="2" ParticipantObjectTypeCodeRole="3" ParticipantObjectDataLifeCycle="1">
          <ParticipantObjectIDTypeCode csd-code="110180" originalText="Study Instance UID" codeSystemName="DCM" />
          <ParticipantObjectDescription>
             <Accession Number="ACC1" />
             <SOPClass UID="1.2.840.10008.5.1.4.1.1.88.11" NumberOfInstances="1" />
          </ParticipantObjectDescription>
       </ParticipantObjectIdentification>
       <ParticipantObjectIdentification ParticipantObjectID="P3^^^MINIRIS" ParticipantObjectTypeCode="1" ParticipantObjectTypeCodeRole="1">
          <ParticipantObjectIDTypeCode csd-code="2" originalText="Patient Number" codeSystemName="RFC-3881" />
          <ParticipantObjectName>Miller^John</ParticipantObjectName>
       </ParticipantObjectIdentification>
    </AuditMessage>