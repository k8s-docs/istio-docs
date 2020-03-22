verify-crds
=================================

Wait for all Istio CRDs to be created:

.. code:: sh

      $ kubectl -n istio-system wait –for=condition=complete
job –all
