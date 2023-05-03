HL7 Export Studies Service
==========================

The HL7 Export Studies Service exports studies from archive to remote destinations on receive of HL7 messages received by
HL7 application of archive according to configurable `HL7 Export Rules <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7ExportRule.html>`_.

The studies to be exported from own archive can be limited by specifying `Entity Selectors <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7PrefetchRule.html#dcmentityselector>`_
which specifies the matching keys used to select studies to be exported from own archive.
