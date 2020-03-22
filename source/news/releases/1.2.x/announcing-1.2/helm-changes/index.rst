helm-changes
=======================

The tables below show changes made to the installation options used to
customize Istio install using Helm between Istio 1.1 and Istio 1.2. The
tables are grouped in to three different categories:

-  The installation options already in the previous release but whose
   values have been modified in the new release.
-  The new installation options added in the new release.
-  The installation options removed from the new release.

.. raw:: html

   <!-- Run python scripts/tablegen.py to generate this table -->

.. raw:: html

   <!-- AUTO-GENERATED-START -->

Modified configuration options
------------------------------

Modified ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============= =================== ================= =============== ===============
Key           Old Default Value   New Default Value Old Description New Description
============= =================== ================= =============== ===============
``kiali.hub`` ``docker.io/kiali`` ``quay.io/kiali``
``kiali.tag`` ``v0.14``           ``v0.20``
============= =================== ================= =============== ===============

Modified ``prometheus`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================== ================= ================= =============== ===============
Key                Old Default Value New Default Value Old Description New Description
================== ================= ================= =============== ===============
``prometheus.tag`` ``v2.3.1``        ``v2.8.0``
================== ================= ================= =============== ===============

Modified ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``global.ta | ``release-1 | ``1.2.0-rc. | ``Default t | ``Default t |
| g``         | .1-latest-d | 3``         | ag for Isti | ag for Isti |
|             | aily``      |             | o images.`` | o images.`` |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``128Mi``   | ``1024Mi``  |             |             |
| oxy.resourc |             |             |             |             |
| es.limits.m |             |             |             |             |
| emory``     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``5s``      | ``300s``    | ``Configure | ``Configure |
| oxy.dnsRefr |             |             |  the DNS re |  the DNS re |
| eshRate``   |             |             | fresh rate  | fresh rate  |
|             |             |             | for Envoy c | for Envoy c |
|             |             |             | luster of t | luster of t |
|             |             |             | ype STRICT_ | ype STRICT_ |
|             |             |             | DNS 5 secon | DNS This mu |
|             |             |             | ds is the d | st be given |
|             |             |             | efault refr |  it terms o |
|             |             |             | esh rate us | f seconds.  |
|             |             |             | ed by Envoy | For example |
|             |             |             | ``          | , 300s is v |
|             |             |             |             | alid but 5m |
|             |             |             |             |  is invalid |
|             |             |             |             | .``         |
+-------------+-------------+-------------+-------------+-------------+

Modified ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``mixer.ada | ``true``    | ``false``   | ``Setting t | ``Setting t |
| pters.useAd |             |             | his to fals | his to fals |
| apterCRDs`` |             |             | e sets the  | e sets the  |
|             |             |             | useAdapterC | useAdapterC |
|             |             |             | RDs mixer s | RDs mixer s |
|             |             |             | tartup argu | tartup argu |
|             |             |             | ment to fal | ment to fal |
|             |             |             | se``        | se``        |
+-------------+-------------+-------------+-------------+-------------+

Modified ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===================== ================= ================= =============== ===============
Key                   Old Default Value New Default Value Old Description New Description
===================== ================= ================= =============== ===============
``grafana.image.tag`` ``5.4.0``         ``6.1.6``
===================== ================= ================= =============== ===============

New configuration options
-------------------------

New ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================================ ============= ===========
Key                                          Default Value Description
============================================ ============= ===========
``tracing.podAntiAffinityLabelSelector``     ``[]``
``tracing.podAntiAffinityTermLabelSelector`` ``[]``
============================================ ============= ===========

New ``sidecarInjectorWebhook`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.podAntiAffinityLa |                       |                       |
| belSelector``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.podAntiAffinityTe |                       |                       |
| rmLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                | ``You can use the fie |
| ook.neverInjectSelect |                       | ld called alwaysInjec |
| or``                  |                       | tSelector and neverIn |
|                       |                       | jectSelector which wi |
|                       |                       | ll always inject the  |
|                       |                       | sidecar or always ski |
|                       |                       | p the injection on po |
|                       |                       | ds that match that la |
|                       |                       | bel selector, regardl |
|                       |                       | ess of the global pol |
|                       |                       | icy. See https://isti |
|                       |                       | o.io/docs/setup/kuber |
|                       |                       | netes/additional-setu |
|                       |                       | p/sidecar-injection/m |
|                       |                       | ore-control-adding-ex |
|                       |                       | ceptions``            |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.alwaysInjectSelec |                       |                       |
| tor``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+

New ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``global.logging.leve | ``"default:info"``    |                       |
| l``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.logLev | ``""``                | ``Log level for proxy |
| el``                  |                       | , applies to gateways |
|                       |                       |  and sidecars.  If le |
|                       |                       | ft empty, "warning" i |
|                       |                       | s used. Expected valu |
|                       |                       | es are: trace\|debug\ |
|                       |                       | |info\|warning\|error |
|                       |                       | \|critical\|off``     |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.compon | ``""``                | ``Per Component log l |
| entLogLevel``         |                       | evel for proxy, appli |
|                       |                       | es to gateways and si |
|                       |                       | decars. If a componen |
|                       |                       | t level is not set, t |
|                       |                       | hen the global "logLe |
|                       |                       | vel" will be used. If |
|                       |                       |  left empty, "misc:er |
|                       |                       | ror" is used.``       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.exclud | ``""``                |                       |
| eOutboundPorts``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.datad | ``"$(HOST_IP):8126"`` |                       |
| og.address``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.imagePullSec | ``[]``                | ``Lists the secrets y |
| rets``                |                       | ou need to use to pul |
|                       |                       | l Istio images from a |
|                       |                       |  secure registry.``   |
+-----------------------+-----------------------+-----------------------+
| ``global.localityLbSe | ``{}``                |                       |
| tting``               |                       |                       |
+-----------------------+-----------------------+-----------------------+

New ``galley`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=========================================== ============= ===========
Key                                         Default Value Description
=========================================== ============= ===========
``galley.nodeSelector``                     ``{}``
``galley.tolerations``                      ``[]``
``galley.podAntiAffinityLabelSelector``     ``[]``
``galley.podAntiAffinityTermLabelSelector`` ``[]``
=========================================== ============= ===========

New ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

========================================== ============= ===========
Key                                        Default Value Description
========================================== ============= ===========
``mixer.tolerations``                      ``[]``
``mixer.podAntiAffinityLabelSelector``     ``[]``
``mixer.podAntiAffinityTermLabelSelector`` ``[]``
``mixer.templates.useTemplateCRDs``        ``false``
========================================== ============= ===========

New ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================================ ============= ===========
Key                                          Default Value Description
============================================ ============= ===========
``grafana.tolerations``                      ``[]``
``grafana.podAntiAffinityLabelSelector``     ``[]``
``grafana.podAntiAffinityTermLabelSelector`` ``[]``
============================================ ============= ===========

New ``prometheus`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=============================================== ============= ===========
Key                                             Default Value Description
=============================================== ============= ===========
``prometheus.tolerations``                      ``[]``
``prometheus.podAntiAffinityLabelSelector``     ``[]``
``prometheus.podAntiAffinityTermLabelSelector`` ``[]``
=============================================== ============= ===========

New ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================================== ============= ===========
Key                                                                Default Value Description
================================================================== ============= ===========
``gateways.istio-ingressgateway.sds.resources.requests.cpu``       ``100m``
``gateways.istio-ingressgateway.sds.resources.requests.memory``    ``128Mi``
``gateways.istio-ingressgateway.sds.resources.limits.cpu``         ``2000m``
``gateways.istio-ingressgateway.sds.resources.limits.memory``      ``1024Mi``
``gateways.istio-ingressgateway.resources.requests.cpu``           ``100m``
``gateways.istio-ingressgateway.resources.requests.memory``        ``128Mi``
``gateways.istio-ingressgateway.resources.limits.cpu``             ``2000m``
``gateways.istio-ingressgateway.resources.limits.memory``          ``1024Mi``
``gateways.istio-ingressgateway.applicationPorts``                 ``""``
``gateways.istio-ingressgateway.tolerations``                      ``[]``
``gateways.istio-ingressgateway.podAntiAffinityLabelSelector``     ``[]``
``gateways.istio-ingressgateway.podAntiAffinityTermLabelSelector`` ``[]``
``gateways.istio-egressgateway.resources.requests.cpu``            ``100m``
``gateways.istio-egressgateway.resources.requests.memory``         ``128Mi``
``gateways.istio-egressgateway.resources.limits.cpu``              ``2000m``
``gateways.istio-egressgateway.resources.limits.memory``           ``256Mi``
``gateways.istio-egressgateway.tolerations``                       ``[]``
``gateways.istio-egressgateway.podAntiAffinityLabelSelector``      ``[]``
``gateways.istio-egressgateway.podAntiAffinityTermLabelSelector``  ``[]``
``gateways.istio-ilbgateway.tolerations``                          ``[]``
================================================================== ============= ===========

New ``certmanager`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================ ============= ===========
Key                                              Default Value Description
================================================ ============= ===========
``certmanager.replicaCount``                     ``1``
``certmanager.nodeSelector``                     ``{}``
``certmanager.tolerations``                      ``[]``
``certmanager.podAntiAffinityLabelSelector``     ``[]``
``certmanager.podAntiAffinityTermLabelSelector`` ``[]``
================================================ ============= ===========

New ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``kiali.podAntiAffini | ``[]``                |                       |
| tyLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.podAntiAffini | ``[]``                |                       |
| tyTermLabelSelector`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.vie | ``false``             | ``Bind the service ac |
| wOnlyMode``           |                       | count to a role with  |
|                       |                       | only read access``    |
+-----------------------+-----------------------+-----------------------+

New ``istiocoredns`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================= ============= ===========
Key                                               Default Value Description
================================================= ============= ===========
``istiocoredns.tolerations``                      ``[]``
``istiocoredns.podAntiAffinityLabelSelector``     ``[]``
``istiocoredns.podAntiAffinityTermLabelSelector`` ``[]``
================================================= ============= ===========

New ``security`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================================= ============= ===========
Key                                           Default Value Description
============================================= ============= ===========
``security.tolerations``                      ``[]``
``security.citadelHealthCheck``               ``false``
``security.podAntiAffinityLabelSelector``     ``[]``
``security.podAntiAffinityTermLabelSelector`` ``[]``
============================================= ============= ===========

New ``nodeagent`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================================== ============= ===========
Key                                            Default Value Description
============================================== ============= ===========
``nodeagent.tolerations``                      ``[]``
``nodeagent.podAntiAffinityLabelSelector``     ``[]``
``nodeagent.podAntiAffinityTermLabelSelector`` ``[]``
============================================== ============= ===========

New ``pilot`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

========================================== ============= ===========
Key                                        Default Value Description
========================================== ============= ===========
``pilot.tolerations``                      ``[]``
``pilot.podAntiAffinityLabelSelector``     ``[]``
``pilot.podAntiAffinityTermLabelSelector`` ``[]``
========================================== ============= ===========

Removed configuration options
-----------------------------

Removed ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``kiali.dashboard.use | ``username``          | ``This is the key nam |
| rnameKey``            |                       | e within the secret w |
|                       |                       | hose value is the act |
|                       |                       | ual username.``       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.pas | ``passphrase``        | ``This is the key nam |
| sphraseKey``          |                       | e within the secret w |
|                       |                       | hose value is the act |
|                       |                       | ual passphrase.``     |
+-----------------------+-----------------------+-----------------------+

Removed ``security`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

========================= ============= ===========
Key                       Default Value Description
========================= ============= ===========
``security.replicaCount`` ``1``
========================= ============= ===========

Removed ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=========================================== ============= ===========
Key                                         Default Value Description
=========================================== ============= ===========
``gateways.istio-ingressgateway.resources`` ``{}``
=========================================== ============= ===========

Removed ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================= ============= ===========
Key               Default Value Description
================= ============= ===========
``mixer.enabled`` ``true``
================= ============= ===========

Removed ``servicegraph`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``servicegraph.ingres | ``false``             |                       |
| s.enabled``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.servic | ``http``              |                       |
| e.name``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.replic | ``1``                 |                       |
| aCount``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.servic | ``ClusterIP``         |                       |
| e.type``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.servic | ``{}``                |                       |
| e.annotations``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.enable | ``false``             |                       |
| d``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.image` | ``servicegraph``      |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.servic | ``8088``              |                       |
| e.externalPort``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.ingres | ``servicegraph.local` | ``Used to create an I |
| s.hosts``             | `                     | ngress record.``      |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.nodeSe | ``{}``                |                       |
| lector``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``servicegraph.promet | ``http://prometheus:9 |                       |
| heusAddr``            | 090``                 |                       |
+-----------------------+-----------------------+-----------------------+

.. raw:: html

   <!-- AUTO-GENERATED-END -->
