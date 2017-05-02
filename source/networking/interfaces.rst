Network Interfaces
^^^^^^^^^^^^^^^^^^

.. _interface-physical-network-interface:

Physical Network Interface
""""""""""""""""""""""""""

The application is indifferent to the physical medium over which TCP/IP executes, which is dependent on the
underlying operating system and hardware

.. _interface-hl7-communication-protocol:

HL7 Communication protocol
""""""""""""""""""""""""""

The application uses the Minimal Lower Layer Protocol (MLLP) defined in Appendix C of the HL7 Implementation Guide.

.. _interface-additional-protocols:

Additional Protocols
""""""""""""""""""""

When host names rather than IP addresses are used in the configuration properties to specify presentation addresses
for remote HL7 Applications, the application is dependent on the name resolution mechanism of the underlying
operating system.

.. _interface-ip-support:

IPv4 and IPv6 Support
"""""""""""""""""""""

This product supports both IPv4 and IPv6. It does not utilize any of the optional configuration identification or
security features of IPv6.
