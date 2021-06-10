HL7 Studies Prefetch Service
============================

The HL7 Studies Prefetch Service prefetches studies from external archive on receive of HL7 messages received by HL7
application of archive according to configurable `HL7 Prefetch Rules <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7PrefetchRule.html>`_.

The studies to be prefetched from external archive can be limited by specifying `Entity Selectors <https://dcm4chee-arc-cs.readthedocs.io/en/latest/networking/config/hl7PrefetchRule.html#dcmentityselector>_`
which specifies the matching keys used to select studies to be prefetched from the external archive.