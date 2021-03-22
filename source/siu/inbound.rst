Inbound
#######

.. _siu_in_messages:

Inbound Messages
================

.. _siu_in_s12:

ADT/ACK - Notification of new appointment booking (Event S12)
-------------------------------------------------------------
Supported HL7 versions: 2.3.1

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that a new appointment has been booked. The information provided in the SCH segment
and the other detail segments as appropriate describe the appointment that has been booked by the filler application.

Field *MSH-9 Message Type* shall be valued **SIU^S12^SIU_S12**. The third component is optional for HL7 v2.3.1.

.. _siu_in_s12_actions:

Performed Actions
^^^^^^^^^^^^^^^^^
The received message is accepted and acknowledged. Although the received HL7 message contains PID segment, the Patient
Update service is not invoked.

.. _siu_in_s13:

ADT/ACK - Notification of appointment rescheduling (Event S13)
--------------------------------------------------------------
Supported HL7 versions: 2.3.1

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that an existing appointment has been rescheduled. The information in the SCH segment
and the other detail segments as appropriate describe the new date(s) and time(s) to which the previously booked appointment
has been moved. Additionally, it describes the unchanged information in the previously booked appointment.

Field *MSH-9 Message Type* shall be valued **SIU^S13^SIU_S13**. The third component is optional for HL7 v2.3.1.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`siu_in_s12_actions`.

.. _siu_in_s15:

ADT/ACK - Notification of appointment cancellation (Event S15)
--------------------------------------------------------------
Supported HL7 versions: 2.3.1

Trigger Event
^^^^^^^^^^^^^
A remote HL7 Application notifies that an existing appointment has been canceled. A cancel event is used to stop a valid
appointment from taking place. For example, if a patient scheduled for an exam cancels his/her appointment, then the
appointment is canceled on the filler application.

Field *MSH-9 Message Type* shall be valued **SIU^S15^SIU_S15**. The third component is optional for HL7 v2.3.1.

Performed Actions
^^^^^^^^^^^^^^^^^
Same as specified in :numref:`siu_in_s12_actions`.