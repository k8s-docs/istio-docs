bugs
====================================

Oh no! You found a bug? We’d love to hear about it.

Product bugs
------------

Search our `issue database <https://github.com/istio/istio/issues/>`_
to see if we already know about your problem and learn about when we
think we can fix it. If you don’t find your problem in the database,
please open a `new
issue <https://github.com/istio/istio/issues/new/choose>`_ and let us
know what’s going on.

If you think a bug is in fact a security vulnerability, please visit
`Reporting Security
Vulnerabilities </about/security-vulnerabilities/>`_ to learn what to
do.

Kubernetes cluster state archives
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you’re running on Kubernetes, consider including a cluster state
archive with your bug report. For convenience, you can run a dump script
to produce an archive containing all of the relevant state from your
Kubernetes cluster:

-  Run via ``curl``:

   .. code:: sh

      $ curl {{< github_file >}}/tools/dump_kubernetes.sh
      \| sh -s – -z

-  Run locally, from the release directory’s root:

   .. code:: sh

      $ @tools/dump_kubernetes.sh@ -z

Then attach the produced ``istio-dump.tar.gz`` with your reported
problem.

If you are unable to use the dump script, please attach your own archive
containing:

-  Pods, services, deployments, and endpoints across all namespaces:

   .. code:: sh

      $ kubectl get pods,services,deployments,endpoints
   –all-namespaces -o yaml > k8s_resources.yaml

-  Secret names in ``istio-system``:

   .. code:: sh

      $ kubectl –namespace istio-system get secrets

-  configmaps in the ``istio-system`` namespace:

   .. code:: sh

      $ kubectl –namespace istio-system get cm -o yaml


-  Current and previous logs from all Istio components and sidecar

-  Pilot logs:

   .. code:: sh

      $ kubectl logs -n istio-system -l istio=pilot -c
   discovery $ kubectl logs -n istio-system -l istio=pilot -c
   istio-proxy

-  All Istio configuration artifacts:

   .. code:: sh

      $ kubectl get $(kubectl get crd –no-headers \| awk
   ‘{printf “%s,”,$1}END{printf
   “attributemanifests.config.istio.io:raw-latex:`\n`”}’)
   –all-namespaces

Documentation bugs
------------------

Search our `documentation issue
database <https://github.com/istio/istio.io/issues/>`_ to see if we
already know about your problem and learn about when we think we can fix
it. If you don’t find your problem in the database, please navigate to
the page with the problem, then select the gear menu at the top right of
this page, and finally chose *Report a Site Bug*.
