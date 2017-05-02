Security
========

.. _security-profiles:

Security Profiles
"""""""""""""""""

.. _secure-transport-connection-profiles:

Secure Transport Connection Profiles
''''''''''''''''''''''''''''''''''''

|product| supports the *Basic TLS Secure Transport Connection Profile* and the
*AES TLS Secure Transport Connection Profile* as specified in
`DICOM Standard, Part 15, Annex B.1 <http://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_B.1.html>`_
and `Annex B.3 <http://dicom.nema.org/medical/dicom/current/output/chtml/part15/sect_B.3.html>`_.

By default configuration, TLS 1.0, TLS 1.1 and TLS 1.2 are enabled, use of TLS 1.2 is preferred.

Also other cyphersuite options than the two in compliance with *AES TLS Secure Transport Connection Profile*:

- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA

may be configured.

Beside DICOM DIMSE service connections, also HL7 v2 and HTTP connections can be secured by use of TLS.

IP ports on which an implementation accepts TLS connections are configurable.

The private key and the Certificate used by an instance of |product| to identify itself in the TLS negotiation
with remote applications has to be provided in a local keystore file in PKCS12 or JKS (Java Key Store) format
on the application host. Certficates of Certificate Authorities (CA) to validate Certificates received
from remote applications during the TLS negotiation can also be provided in a local keystore file in JKS format
or at the central LDAP server, used as configuration backend for all instances of |product|.

.. _security-association-level-security:

Association Level Security
""""""""""""""""""""""""""

|product| can be configured to check the Receiving Application and Facility in received HL7 v2 messages. Each
HL7 Application provided by |product| can be configured to accept HL7 v2 messages from only a limited list of Sending
Application and Facility names.

.. _security-application-level-security:
