Patient Management Service
==========================

The Patient Management Service processes ADT messages received from remote HL7 Applications, updating the
patient information of archived DICOM objects, and send ADT messages to remote HL7 Applications on
patient information changes initiated via the Web UI or proprietary RESTful services of |product|.

The functionality provided by the Patient Management Service of |product| is compliant with the requirements for
following IHE Integration Profiles and Actors with Option:

.. csv-table::
   :header: "Integration Profile", "Actor", "Option"
   :widths: 40, 40, 20

   "Patient Information Reconciliation", "Image Manager/Image Archive", "HL7 v2.5.1"
   "Patient Administration Management", "Patient Demographics Consumer", "Merge"
   "Patient Administration Management", "Patient Demographics Source", "Merge"

as specified in the IHE Technical Frameworks for `Radiology <http://ihe.net/Technical_Frameworks/#radiology>`_
and `IT Infrastructure <http://ihe.net/Technical_Frameworks/#IT>`_.

.. toctree::

   inbound
   outbound
