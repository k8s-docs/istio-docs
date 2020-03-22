change-notes
=====================

The tables below show changes made to the installation options used to
customize Istio install using Helm between Istio 1.0 and Istio 1.1. The
tables are grouped in to three different categories:

-  The installation options already in the previous release but whose
   values or descriptions have been modified in the new release.
-  The new installation options added in the new release.
-  The installation options removed from the new release.

.. raw:: html

   <!-- Run python scripts/tablegen.py to generate this table -->

.. raw:: html

   <!-- AUTO-GENERATED-START -->

Modified configuration options
------------------------------

Modified ``servicegraph`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``servicegr | ``servicegr | ``servicegr |             | ``Used to c |
| aph.ingress | aph.local`` | aph.local`` |             | reate an In |
| .hosts``    |             |             |             | gress recor |
|             |             |             |             | d.``        |
+-------------+-------------+-------------+-------------+-------------+

Modified ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

====================== ================= ================= =============== ===============
Key                    Old Default Value New Default Value Old Description New Description
====================== ================= ================= =============== ===============
``tracing.jaeger.tag`` ``1.5``           ``1.9``
====================== ================= ================= =============== ===============

Modified ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``global.hu | ``gcr.io/is | ``gcr.io/is |             | ``Default h |
| b``         | tio-release | tio-release |             | ub for Isti |
|             | ``          | ``          |             | o images.Re |
|             |             |             |             | leases are  |
|             |             |             |             | published t |
|             |             |             |             | o docker hu |
|             |             |             |             | b under 'is |
|             |             |             |             | tio' projec |
|             |             |             |             | t.Daily bui |
|             |             |             |             | lds from pr |
|             |             |             |             | ow are on g |
|             |             |             |             | cr.io, and  |
|             |             |             |             | nightly bui |
|             |             |             |             | lds from ci |
|             |             |             |             | rcle on doc |
|             |             |             |             | ker.io/isti |
|             |             |             |             | onightly``  |
+-------------+-------------+-------------+-------------+-------------+
| ``global.ta | ``release-1 | ``release-1 |             | ``Default t |
| g``         | .0-latest-d | .1-latest-d |             | ag for Isti |
|             | aily``      | aily``      |             | o images.`` |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``10m``     | ``100m``    |             |             |
| oxy.resourc |             |             |             |             |
| es.requests |             |             |             |             |
| .cpu``      |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``"/dev/std | ``""``      |             |             |
| oxy.accessL | out"``      |             |             |             |
| ogFile``    |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``false``   | ``false``   |             | ``If set, n |
| oxy.enableC |             |             |             | ewly inject |
| oreDump``   |             |             |             | ed sidecars |
|             |             |             |             |  will have  |
|             |             |             |             | core dumps  |
|             |             |             |             | enabled.``  |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``enabled`` | ``enabled`` |             | ``This cont |
| oxy.autoInj |             |             |             | rols the 'p |
| ect``       |             |             |             | olicy' in t |
|             |             |             |             | he sidecar  |
|             |             |             |             | injector.`` |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``true``    | ``false``   |             | ``If enable |
| oxy.envoySt |             |             |             | d is set to |
| atsd.enable |             |             |             |  true, host |
| d``         |             |             |             |  and port m |
|             |             |             |             | ust also be |
|             |             |             |             |  provided.  |
|             |             |             |             | Istio no lo |
|             |             |             |             | nger provid |
|             |             |             |             | es a statsd |
|             |             |             |             |  collector. |
|             |             |             |             | ``          |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``istio-sta | :literal:`\ |             | ``example:  |
| oxy.envoySt | tsd-prom-br | |  | `examp |             | 9125``      |
| atsd.host`` | idge``      | le: statsd- |             |             |
|             |             | svc.istio-s |             |             |
|             |             | ystem` | |  |             |             |
|             |             | `global.pro |             |             |
|             |             | xy.envoySta |             |             |
|             |             | tsd.port` | |             |             |
|             |             |  `9125` \|` |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``global.pr | ``proxy_ini | ``proxy_ini |             | ``Base name |
| oxy_init.im | t``         | t``         |             |  for the pr |
| age``       |             |             |             | oxy_init co |
|             |             |             |             | ntainer, us |
|             |             |             |             | ed to confi |
|             |             |             |             | gure iptabl |
|             |             |             |             | es.``       |
+-------------+-------------+-------------+-------------+-------------+
| ``global.co | ``false``   | ``false``   |             | ``controlPl |
| ntrolPlaneS |             |             |             | aneMtls ena |
| ecurityEnab |             |             |             | bled. Will  |
| led``       |             |             |             | result in d |
|             |             |             |             | elays start |
|             |             |             |             | ing the pod |
|             |             |             |             | s while sec |
|             |             |             |             | rets arepro |
|             |             |             |             | pagated, no |
|             |             |             |             | t recommend |
|             |             |             |             | ed for test |
|             |             |             |             | s.``        |
+-------------+-------------+-------------+-------------+-------------+
| ``global.di | ``false``   | ``true``    |             | ``disablePo |
| sablePolicy |             |             |             | licyChecks  |
| Checks``    |             |             |             | disables mi |
|             |             |             |             | xer policy  |
|             |             |             |             | checks.if m |
|             |             |             |             | ixer.policy |
|             |             |             |             | .enabled==t |
|             |             |             |             | rue then di |
|             |             |             |             | sablePolicy |
|             |             |             |             | Checks has  |
|             |             |             |             | affect.Will |
|             |             |             |             |  set the va |
|             |             |             |             | lue with sa |
|             |             |             |             | me name in  |
|             |             |             |             | istio confi |
|             |             |             |             | g map - pil |
|             |             |             |             | ot needs to |
|             |             |             |             |  be restart |
|             |             |             |             | ed to take  |
|             |             |             |             | effect.``   |
+-------------+-------------+-------------+-------------+-------------+
| ``global.en | ``true``    | ``true``    |             | ``EnableTra |
| ableTracing |             |             |             | cing sets t |
| ``          |             |             |             | he value wi |
|             |             |             |             | th same nam |
|             |             |             |             | e in istio  |
|             |             |             |             | config map, |
|             |             |             |             |  requires p |
|             |             |             |             | ilot restar |
|             |             |             |             | t to take e |
|             |             |             |             | ffect.``    |
+-------------+-------------+-------------+-------------+-------------+
| ``global.mt | ``false``   | ``false``   |             | ``Default s |
| ls.enabled` |             |             |             | etting for  |
| `           |             |             |             | service-to- |
|             |             |             |             | service mtl |
|             |             |             |             | s. Can be s |
|             |             |             |             | et explicit |
|             |             |             |             | ly usingdes |
|             |             |             |             | tination ru |
|             |             |             |             | les or serv |
|             |             |             |             | ice annotat |
|             |             |             |             | ions.``     |
+-------------+-------------+-------------+-------------+-------------+
| ``global.on | ``false``   | ``false``   |             | ``Whether t |
| eNamespace` |             |             |             | o restrict  |
| `           |             |             |             | the applica |
|             |             |             |             | tions names |
|             |             |             |             | pace the co |
|             |             |             |             | ntroller ma |
|             |             |             |             | nages;If no |
|             |             |             |             | t set, cont |
|             |             |             |             | roller watc |
|             |             |             |             | hes all nam |
|             |             |             |             | espaces``   |
+-------------+-------------+-------------+-------------+-------------+
| ``global.co | ``true``    | ``true``    |             | ``Whether t |
| nfigValidat |             |             |             | o perform s |
| ion``       |             |             |             | erver-side  |
|             |             |             |             | validation  |
|             |             |             |             | of configur |
|             |             |             |             | ation.``    |
+-------------+-------------+-------------+-------------+-------------+

Modified ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``gateways. | ``LoadBalan | ``LoadBalan |             | ``change to |
| istio-ingre | cer #change | cer``       |             |  NodePort,  |
| ssgateway.t |  to NodePor |             |             | ClusterIP o |
| ype``       | t, ClusterI |             |             | r LoadBalan |
|             | P or LoadBa |             |             | cer if need |
|             | lancer if n |             |             |  be``       |
|             | eed be``    |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``gateways. | ``true``    | ``false``   |             |             |
| istio-egres |             |             |             |             |
| sgateway.en |             |             |             |             |
| abled``     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| ``gateways. | ``ClusterIP | ``ClusterIP |             | ``change to |
| istio-egres |  #change to | ``          |             |  NodePort o |
| sgateway.ty |  NodePort o |             |             | r LoadBalan |
| pe``        | r LoadBalan |             |             | cer if need |
|             | cer if need |             |             |  be``       |
|             |  be``       |             |             |             |
+-------------+-------------+-------------+-------------+-------------+

Modified ``certmanager`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=================== ================= ================= =============== ===============
Key                 Old Default Value New Default Value Old Description New Description
=================== ================= ================= =============== ===============
``certmanager.tag`` ``v0.3.1``        ``v0.6.2``
=================== ================= ================= =============== ===============

Modified ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============= ===================== ================= =============== ===============
Key           Old Default Value     New Default Value Old Description New Description
============= ===================== ================= =============== ===============
``kiali.tag`` ``istio-release-1.0`` ``v0.14``
============= ===================== ================= =============== ===============

Modified ``security`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``security. | ``true # in | ``true``    |             | ``indicate  |
| selfSigned` | dicate if s |             |             | if self-sig |
| `           | elf-signed  |             |             | ned CA is u |
|             | CA is used. |             |             | sed.``      |
|             | ``          |             |             |             |
+-------------+-------------+-------------+-------------+-------------+

Modified ``pilot`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======================= ================= ================= =============== ===============
Key                     Old Default Value New Default Value Old Description New Description
======================= ================= ================= =============== ===============
``pilot.autoscaleMax``  ``1``             ``5``
``pilot.traceSampling`` ``100.0``         ``1.0``
======================= ================= ================= =============== ===============

New configuration options
-------------------------

New ``istio_cni`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===================== ============= ===========
Key                   Default Value Description
===================== ============= ===========
``istio_cni.enabled`` ``false``
===================== ============= ===========

New ``servicegraph`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================= ============= ===========
Key                           Default Value Description
============================= ============= ===========
``servicegraph.nodeSelector`` ``{}``
============================= ============= ===========

New ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================================ ======================== ===========
Key                                          Default Value            Description
============================================ ======================== ===========
``tracing.nodeSelector``                     ``{}``
``tracing.zipkin.hub``                       ``docker.io/openzipkin``
``tracing.zipkin.tag``                       ``2``
``tracing.zipkin.probeStartupDelay``         ``200``
``tracing.zipkin.queryPort``                 ``9411``
``tracing.zipkin.resources.limits.cpu``      ``300m``
``tracing.zipkin.resources.limits.memory``   ``900Mi``
``tracing.zipkin.resources.requests.cpu``    ``150m``
``tracing.zipkin.resources.requests.memory`` ``900Mi``
``tracing.zipkin.javaOptsHeap``              ``700``
``tracing.zipkin.maxSpans``                  ``500000``
``tracing.zipkin.node.cpus``                 ``2``
============================================ ======================== ===========

New ``sidecarInjectorWebhook`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``sidecarInjectorWebh | ``{}``                |                       |
| ook.nodeSelector``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``false``             | ``If true, webhook or |
| ook.rewriteAppHTTPPro |                       |  istioctl injector wi |
| be``                  |                       | ll rewrite PodSpec fo |
|                       |                       | r livenesshealth chec |
|                       |                       | k to redirect request |
|                       |                       |  to sidecar. This mak |
|                       |                       | es liveness check wor |
|                       |                       | keven when mTLS is en |
|                       |                       | abled.``              |
+-----------------------+-----------------------+-----------------------+

New ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``global.monitoringPo | ``15014``             | ``monitoring port use |
| rt``                  |                       | d by mixer, pilot, ga |
|                       |                       | lley``                |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.e | ``false``             |                       |
| nabled``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.g | ``ingressgateway``    | ``Gateway used for k8 |
| atewayName``          |                       | s Ingress resources.  |
|                       |                       | By default it isusing |
|                       |                       |  'istio:ingressgatewa |
|                       |                       | y' that will be insta |
|                       |                       | lled by setting'gatew |
|                       |                       | ays.enabled' and 'gat |
|                       |                       | eways.istio-ingressga |
|                       |                       | teway.enabled'flags t |
|                       |                       | o true.``             |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.e | ``false``             | ``enableHttps will ad |
| nableHttps``          |                       | d port 443 on the ing |
|                       |                       | ress.It REQUIRES that |
|                       |                       |  the certificates are |
|                       |                       |  installed  in theexp |
|                       |                       | ected secrets - enabl |
|                       |                       | ing this option witho |
|                       |                       | ut certificateswill r |
|                       |                       | esult in LDS rejectio |
|                       |                       | n and the ingress wil |
|                       |                       | l not work.``         |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.cluste | ``"cluster.local"``   | ``cluster domain. Def |
| rDomain``             |                       | ault value is "cluste |
|                       |                       | r.local".``           |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``128Mi``             |                       |
| ces.requests.memory`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``2000m``             |                       |
| ces.limits.cpu``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``128Mi``             |                       |
| ces.limits.memory``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.concur | ``2``                 | ``Controls number of  |
| rency``               |                       | Proxy worker threads. |
|                       |                       | If set to 0 (default) |
|                       |                       | , then start worker t |
|                       |                       | hread for each CPU th |
|                       |                       | read/core.``          |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.access | ``""``                | ``Configure how and w |
| LogFormat``           |                       | hat fields are displa |
|                       |                       | yed in sidecar access |
|                       |                       |  log. Setting toempty |
|                       |                       |  string will result i |
|                       |                       | n default log format` |
|                       |                       | `                     |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.access | ``TEXT``              | ``Configure the acces |
| LogEncoding``         |                       | s log for sidecar to  |
|                       |                       | JSON or TEXT.``       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.dnsRef | ``5s``                | ``Configure the DNS r |
| reshRate``            |                       | efresh rate for Envoy |
|                       |                       |  cluster of type STRI |
|                       |                       | CT_DNS5 seconds is th |
|                       |                       | e default refresh rat |
|                       |                       | e used by Envoy``     |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.privil | ``false``             | ``If set to true, ist |
| eged``                |                       | io-proxy container wi |
|                       |                       | ll have privileged se |
|                       |                       | curityContext``       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.status | ``15020``             | ``Default port for Pi |
| Port``                |                       | lot agent health chec |
|                       |                       | ks. A value of 0 will |
|                       |                       |  disable health check |
|                       |                       | ing.``                |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.readin | ``1``                 | ``The initial delay f |
| essInitialDelaySecond |                       | or readiness probes i |
| s``                   |                       | n seconds.``          |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.readin | ``2``                 | ``The period between  |
| essPeriodSeconds``    |                       | readiness probes.``   |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.readin | ``30``                | ``The number of succe |
| essFailureThreshold`` |                       | ssive failed probes b |
|                       |                       | efore indicating read |
|                       |                       | iness failure.``      |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.kubevi | ``""``                | ``pod internal interf |
| rtInterfaces``        |                       | aces``                |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyM | ``false``             |                       |
| etricsService.enabled |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyM | :literal:`\| `example | ``example: 15000``    |
| etricsService.host``  | : metrics-service.ist |                       |
|                       | io-system` | | `globa |                       |
|                       | l.proxy.envoyMetricsS |                       |
|                       | ervice.port` \|`      |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.tracer | ``"zipkin"``          | ``Specify which trace |
| ``                    |                       | r to use. One of: lig |
|                       |                       | htstep, zipkin``      |
+-----------------------+-----------------------+-----------------------+
| ``global.policyCheckF | ``false``             | ``policyCheckFailOpen |
| ailOpen``             |                       |  allows traffic in ca |
|                       |                       | ses when the mixer po |
|                       |                       | licy service cannot b |
|                       |                       | e reached.Default is  |
|                       |                       | false which means the |
|                       |                       |  traffic is denied wh |
|                       |                       | en the client is unab |
|                       |                       | le to connect to Mixe |
|                       |                       | r.``                  |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.light | ``""``                | ``example: lightstep- |
| step.address``        |                       | satellite:443``       |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.light | ``""``                | ``example: abcdefg123 |
| step.accessToken``    |                       | 4567``                |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.light | ``true``              | ``example: true\|fals |
| step.secure``         |                       | e``                   |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.light | ``""``                | ``example: /etc/light |
| step.cacertPath``     |                       | step/cacert.pem``     |
+-----------------------+-----------------------+-----------------------+
| ``global.tracer.zipki | ``""``                |                       |
| n.address``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultNodeS | ``{}``                | ``Default node select |
| elector``             |                       | or to be applied to a |
|                       |                       | ll deployments so tha |
|                       |                       | t all pods can becons |
|                       |                       | trained to run a part |
|                       |                       | icular nodes. Each co |
|                       |                       | mponent can overwrite |
|                       |                       |  these defaultvalues  |
|                       |                       | by adding its node se |
|                       |                       | lector block in the r |
|                       |                       | elevant section below |
|                       |                       |  and settingthe desir |
|                       |                       | ed values.``          |
+-----------------------+-----------------------+-----------------------+
| ``global.meshExpansio | ``false``             |                       |
| n.enabled``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.meshExpansio | ``false``             | ``If set to true, the |
| n.useILB``            |                       |  pilot and citadel mt |
|                       |                       | ls and the plain text |
|                       |                       |  pilot portswill be e |
|                       |                       | xposed on an internal |
|                       |                       |  gateway``            |
+-----------------------+-----------------------+-----------------------+
| ``global.multiCluster | ``false``             | ``Set to true to conn |
| .enabled``            |                       | ect two kubernetes cl |
|                       |                       | usters via their resp |
|                       |                       | ectiveingressgateway  |
|                       |                       | services when pods in |
|                       |                       |  each cluster cannot  |
|                       |                       | directlytalk to one a |
|                       |                       | nother. All clusters  |
|                       |                       | should be using Istio |
|                       |                       |  mTLS and musthave a  |
|                       |                       | shared root CA for th |
|                       |                       | is model to work.``   |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultPodDi | ``true``              |                       |
| sruptionBudget.enable |                       |                       |
| d``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.useMCP``     | ``true``              | ``Use the Mesh Contro |
|                       |                       | l Protocol (MCP) for  |
|                       |                       | configuring Mixer and |
|                       |                       | Pilot. Requires galle |
|                       |                       | y (--set galley.enabl |
|                       |                       | ed=true).``           |
+-----------------------+-----------------------+-----------------------+
| ``global.trustDomain` | ``""``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.outboundTraf | ``ALLOW_ANY``         |                       |
| ficPolicy.mode``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.enabled` | ``false``             | ``SDS enabled. IF set |
| `                     |                       |  to true, mTLS certif |
|                       |                       | icates for the sideca |
|                       |                       | rs will bedistributed |
|                       |                       |  through the SecretDi |
|                       |                       | scoveryService instea |
|                       |                       | d of using K8S secret |
|                       |                       | s to mount the certif |
|                       |                       | icates.``             |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.udsPath` | ``""``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.useTrust | ``false``             |                       |
| worthyJwt``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.useNorma | ``false``             |                       |
| lJwt``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.meshNetworks | ``{}``                |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.enableHelmTe | ``false``             | ``Specifies whether h |
| st``                  |                       | elm test is enabled o |
|                       |                       | r not.This field is s |
|                       |                       | et to false by defaul |
|                       |                       | t, so 'helm template  |
|                       |                       | ...'will ignore the h |
|                       |                       | elm test yaml files w |
|                       |                       | hen generating the te |
|                       |                       | mplate``              |
+-----------------------+-----------------------+-----------------------+

New ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``mixer.env.GODEBUG`` | ``gctrace=1``         |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.env.GOMAXPROC | ``"6"``               | ``max procs should be |
| S``                   |                       |  ceil(cpu limit + 1)` |
|                       |                       | `                     |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.enable | ``false``             | ``if policy is enable |
| d``                   |                       | d, global.disablePoli |
|                       |                       | cyChecks has affect.` |
|                       |                       | `                     |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.replic | ``1``                 |                       |
| aCount``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.autosc | ``true``              |                       |
| aleEnabled``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.autosc | ``1``                 |                       |
| aleMin``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.autosc | ``5``                 |                       |
| aleMax``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.cpu.ta | ``80``                |                       |
| rgetAverageUtilizatio |                       |                       |
| n``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.ena | ``true``              |                       |
| bled``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.rep | ``1``                 |                       |
| licaCount``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.aut | ``true``              |                       |
| oscaleEnabled``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.aut | ``1``                 |                       |
| oscaleMin``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.aut | ``5``                 |                       |
| oscaleMax``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.cpu | ``80``                |                       |
| .targetAverageUtiliza |                       |                       |
| tion``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.ses | ``false``             |                       |
| sionAffinityEnabled`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.loa | ``enforce``           | ``disabled, logonly o |
| dshedding.mode``      |                       | r enforce``           |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.loa | ``100ms``             | ``based on measuremen |
| dshedding.latencyThre |                       | ts 100ms p50 translat |
| shold``               |                       | es to p99 of under 1s |
|                       |                       | . This is ok for tele |
|                       |                       | metry which is inhere |
|                       |                       | ntly async.``         |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.res | ``1000m``             |                       |
| ources.requests.cpu`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.res | ``1G``                |                       |
| ources.requests.memor |                       |                       |
| y``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.res | ``4800m``             | ``It is best to do ho |
| ources.limits.cpu``   |                       | rizontal scaling of m |
|                       |                       | ixer using moderate c |
|                       |                       | pu allocation.We have |
|                       |                       |  experimentally found |
|                       |                       |  that these values wo |
|                       |                       | rk well.``            |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.res | ``4G``                |                       |
| ources.limits.memory` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.podAnnotation | ``{}``                |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.nodeSelector` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.kube | ``true``              |                       |
| rnetesenv.enabled``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.stdi | ``false``             |                       |
| o.enabled``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.stdi | ``true``              |                       |
| o.outputAsJson``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.prom | ``true``              |                       |
| etheus.enabled``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.prom | ``10m``               |                       |
| etheus.metricsExpiryD |                       |                       |
| uration``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.adapters.useA | ``true``              | ``Setting this to fal |
| dapterCRDs``          |                       | se sets the useAdapte |
|                       |                       | rCRDs mixer startup a |
|                       |                       | rgument to false``    |
+-----------------------+-----------------------+-----------------------+

New ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``grafana.image.repos | ``grafana/grafana``   |                       |
| itory``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.image.tag`` | ``5.4.0``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.ingress.ena | ``false``             |                       |
| bled``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.ingress.hos | ``grafana.local``     | ``Used to create an I |
| ts``                  |                       | ngress record.``      |
+-----------------------+-----------------------+-----------------------+
| ``grafana.persist``   | ``false``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.storageClas | ``""``                |                       |
| sName``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.accessMode` | ``ReadWriteMany``     |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.security.se | ``grafana``           |                       |
| cretName``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.security.us | ``username``          |                       |
| ernameKey``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.security.pa | ``passphrase``        |                       |
| ssphraseKey``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.nodeSelecto | ``{}``                |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.contextPath | ``/grafana``          |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``1``                 |                       |
| .datasources.apiVersi |                       |                       |
| on``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``prometheus``        |                       |
| .datasources.datasour |                       |                       |
| ces.type``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``1``                 |                       |
| .datasources.datasour |                       |                       |
| ces.orgId``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``http://prometheus:9 |                       |
| .datasources.datasour | 090``                 |                       |
| ces.url``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``proxy``             |                       |
| .datasources.datasour |                       |                       |
| ces.access``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``true``              |                       |
| .datasources.datasour |                       |                       |
| ces.isDefault``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``5s``                |                       |
| .datasources.datasour |                       |                       |
| ces.jsonData.timeInte |                       |                       |
| rval``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``true``              |                       |
| .datasources.datasour |                       |                       |
| ces.editable``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``1``                 |                       |
| oviders.dashboardprov |                       |                       |
| iders.apiVersion``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``1``                 |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.orgId |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``'istio'``           |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.folde |                       |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``file``              |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.type` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``false``             |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.disab |                       |                       |
| leDeletion``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``/var/lib/grafana/da |                       |
| oviders.dashboardprov | shboards/istio``      |                       |
| iders.providers.optio |                       |                       |
| ns.path``             |                       |                       |
+-----------------------+-----------------------+-----------------------+

New ``prometheus`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``prometheus.retentio | ``6h``                |                       |
| n``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.nodeSele | ``{}``                |                       |
| ctor``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.scrapeIn | ``15s``               | ``Controls the freque |
| terval``              |                       | ncy of prometheus scr |
|                       |                       | aping``               |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.contextP | ``/prometheus``       |                       |
| ath``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.ingress. | ``false``             |                       |
| enabled``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.ingress. | ``prometheus.local``  | ``Used to create an I |
| hosts``               |                       | ngress record.``      |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.security | ``true``              |                       |
| .enabled``            |                       |                       |
+-----------------------+-----------------------+-----------------------+

New ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``gateways.istio-ingr | ``false``             | ``If true, ingress ga |
| essgateway.sds.enable |                       | teway fetches credent |
| d``                   |                       | ials from SDS server  |
|                       |                       | to handle TLS connect |
|                       |                       | ions.``               |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``node-agent-k8s``    | ``SDS server that wat |
| essgateway.sds.image` |                       | ches kubernetes secre |
| `                     |                       | ts and provisions cre |
|                       |                       | dentials to ingress g |
|                       |                       | ateway.This server ru |
|                       |                       | ns in the same pod as |
|                       |                       |  ingress gateway.``   |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``true``              |                       |
| essgateway.autoscaleE |                       |                       |
| nabled``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``80``                |                       |
| essgateway.cpu.target |                       |                       |
| AverageUtilization``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``[]``                |                       |
| essgateway.loadBalanc |                       |                       |
| erSourceRanges``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``[]``                |                       |
| essgateway.externalIP |                       |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``{}``                |                       |
| essgateway.podAnnotat |                       |                       |
| ions``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15029``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-kiali``       |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-prometheus``  |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-grafana``     |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15032``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-tracing``     |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15443``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tls``               |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15020``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``status-port``       |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15011``             |                       |
| essgateway.meshExpans |                       |                       |
| ionPorts.targetPort`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tcp-pilot-grpc-tls` |                       |
| essgateway.meshExpans | `                     |                       |
| ionPorts.name``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15004``             |                       |
| essgateway.meshExpans |                       |                       |
| ionPorts.targetPort`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tcp-mixer-grpc-tls` |                       |
| essgateway.meshExpans | `                     |                       |
| ionPorts.name``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``8060``              |                       |
| essgateway.meshExpans |                       |                       |
| ionPorts.targetPort`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tcp-citadel-grpc-tl |                       |
| essgateway.meshExpans | s``                   |                       |
| ionPorts.name``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``853``               |                       |
| essgateway.meshExpans |                       |                       |
| ionPorts.targetPort`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tcp-dns-tls``       |                       |
| essgateway.meshExpans |                       |                       |
| ionPorts.name``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``"sni-dnat"``        | ``A gateway with this |
| essgateway.env.ISTIO_ |                       |  mode ensures that pi |
| META_ROUTER_MODE``    |                       | lot generates an addi |
|                       |                       | tionalset of clusters |
|                       |                       |  for internal service |
|                       |                       | s but without Istio m |
|                       |                       | TLS, toenable cross c |
|                       |                       | luster routing.``     |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``{}``                |                       |
| essgateway.nodeSelect |                       |                       |
| or``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``true``              |                       |
| ssgateway.autoscaleEn |                       |                       |
| abled``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``80``                |                       |
| ssgateway.cpu.targetA |                       |                       |
| verageUtilization``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``{}``                |                       |
| ssgateway.podAnnotati |                       |                       |
| ons``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``15443``             |                       |
| ssgateway.ports.targe |                       |                       |
| tPort``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``tls``               |                       |
| ssgateway.ports.name` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``"sni-dnat"``        |                       |
| ssgateway.env.ISTIO_M |                       |                       |
| ETA_ROUTER_MODE``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``{}``                |                       |
| ssgateway.nodeSelecto |                       |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``true``              |                       |
| ateway.autoscaleEnabl |                       |                       |
| ed``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``80``                |                       |
| ateway.cpu.targetAver |                       |                       |
| ageUtilization``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``{}``                |                       |
| ateway.podAnnotations |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``{}``                |                       |
| ateway.nodeSelector`` |                       |                       |
+-----------------------+-----------------------+-----------------------+

New ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``kiali.contextPath`` | ``/kiali``            |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.nodeSelector` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.ingress.hosts | ``kiali.local``       | ``Used to create an I |
| ``                    |                       | ngress record.``      |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.sec | ``kiali``             |                       |
| retName``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.use | ``username``          |                       |
| rnameKey``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.pas | ``passphrase``        |                       |
| sphraseKey``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.prometheusAdd | ``http://prometheus:9 |                       |
| r``                   | 090``                 |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.createDemoSec | ``false``             | ``When true, a secret |
| ret``                 |                       |  will be created with |
|                       |                       |  a default username a |
|                       |                       | nd password. Useful f |
|                       |                       | or demos.``           |
+-----------------------+-----------------------+-----------------------+

New ``istiocoredns`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=================================== ====================================== ===========
Key                                 Default Value                          Description
=================================== ====================================== ===========
``istiocoredns.enabled``            ``false``
``istiocoredns.replicaCount``       ``1``
``istiocoredns.coreDNSImage``       ``coredns/coredns:1.1.2``
``istiocoredns.coreDNSPluginImage`` ``istio/coredns-plugin:0.2-istio-1.1``
``istiocoredns.nodeSelector``       ``{}``
=================================== ====================================== ===========

New ``security`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================= ============= ===========
Key                           Default Value Description
============================= ============= ===========
``security.enabled``          ``true``
``security.createMeshPolicy`` ``true``
``security.nodeSelector``     ``{}``
============================= ============= ===========

New ``nodeagent`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================= ================== ===============================================
Key                           Default Value      Description
============================= ================== ===============================================
``nodeagent.enabled``         ``false``
``nodeagent.image``           ``node-agent-k8s``
``nodeagent.env.CA_PROVIDER`` ``""``             ``name of authentication provider.``
``nodeagent.env.CA_ADDR``     ``""``             ``CA endpoint.``
``nodeagent.env.Plugins``     ``""``             ``names of authentication provider's plugins.``
``nodeagent.nodeSelector``    ``{}``
============================= ================== ===============================================

New ``pilot`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``pilot.autoscaleEnab | ``true``              |                       |
| led``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.env.PILOT_PUS | ``100``               |                       |
| H_THROTTLE``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.env.GODEBUG`` | ``gctrace=1``         |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.cpu.targetAve | ``80``                |                       |
| rageUtilization``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.nodeSelector` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.keepaliveMaxS | ``30m``               | ``The following is us |
| erverConnectionAge``  |                       | ed to limit how long  |
|                       |                       | a sidecar can be conn |
|                       |                       | ectedto a pilot. It b |
|                       |                       | alances out load acro |
|                       |                       | ss pilot instances at |
|                       |                       |  the cost ofincreasin |
|                       |                       | g system churn.``     |
+-----------------------+-----------------------+-----------------------+

Removed configuration options
-----------------------------

Removed ``ingress`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``ingress.service.por | ``32000``             |                       |
| ts.nodePort``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.sel | ``ingress``           |                       |
| ector.istio``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.autoscaleMi | ``1``                 |                       |
| n``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.loa | ``""``                |                       |
| dBalancerIP``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.enabled``   | ``false``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.ann | ``{}``                |                       |
| otations``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.por | ``http``              |                       |
| ts.name``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.por | ``https``             |                       |
| ts.name``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.autoscaleMa | ``5``                 |                       |
| x``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.replicaCoun | ``1``                 |                       |
| t``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``ingress.service.typ | ``LoadBalancer #chang |                       |
| e``                   | e to NodePort, Cluste |                       |
|                       | rIP or LoadBalancer i |                       |
|                       | f need be``           |                       |
+-----------------------+-----------------------+-----------------------+

Removed ``servicegraph`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===================================== ====================== ===========
Key                                   Default Value          Description
===================================== ====================== ===========
``servicegraph``                      ``servicegraph.local``
``servicegraph.ingress``              ``servicegraph.local``
``servicegraph.service.internalPort`` ``8088``
===================================== ====================== ===========

Removed ``telemetry-gateway`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======================================= ================== ===========
Key                                     Default Value      Description
======================================= ================== ===========
``telemetry-gateway.prometheusEnabled`` ``false``
``telemetry-gateway.gatewayName``       ``ingressgateway``
``telemetry-gateway.grafanaEnabled``    ``false``
======================================= ================== ===========

Removed ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================= =================== ===========
Key                           Default Value       Description
============================= =================== ===========
``global.hyperkube.tag``      ``v1.7.6_coreos.0``
``global.k8sIngressHttps``    ``false``
``global.crds``               ``true``
``global.hyperkube.hub``      ``quay.io/coreos``
``global.meshExpansion``      ``false``
``global.k8sIngressSelector`` ``ingress``
``global.meshExpansionILB``   ``false``
============================= =================== ===========

Removed ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

====================================================== ================== ===========
Key                                                    Default Value      Description
====================================================== ================== ===========
``mixer.autoscaleMin``                                 ``1``
``mixer.istio-policy.cpu.targetAverageUtilization``    ``80``
``mixer.autoscaleMax``                                 ``5``
``mixer.istio-telemetry.autoscaleMin``                 ``1``
``mixer.prometheusStatsdExporter.tag``                 ``v0.6.0``
``mixer.istio-telemetry.autoscaleMax``                 ``5``
``mixer.istio-telemetry.cpu.targetAverageUtilization`` ``80``
``mixer.istio-policy.autoscaleEnabled``                ``true``
``mixer.istio-telemetry.autoscaleEnabled``             ``true``
``mixer.replicaCount``                                 ``1``
``mixer.prometheusStatsdExporter.hub``                 ``docker.io/prom``
``mixer.istio-policy.autoscaleMin``                    ``1``
``mixer.istio-policy.autoscaleMax``                    ``5``
====================================================== ================== ===========

Removed ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================== ============= ===========
Key                                Default Value Description
================================== ============= ===========
``grafana.image``                  ``grafana``
``grafana.service.internalPort``   ``3000``
``grafana.security.adminPassword`` ``admin``
``grafana.security.adminUser``     ``admin``
================================== ============= ===========

Removed ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================== ======================== ===========
Key                                                Default Value            Description
================================================== ======================== ===========
``gateways.istio-ilbgateway.replicaCount``         ``1``
``gateways.istio-egressgateway.replicaCount``      ``1``
``gateways.istio-ingressgateway.replicaCount``     ``1``
``gateways.istio-ingressgateway.ports.name``       ``tcp-pilot-grpc-tls``
``gateways.istio-ingressgateway.ports.name``       ``tcp-citadel-grpc-tls``
``gateways.istio-ingressgateway.ports.name``       ``http2-prometheus``
``gateways.istio-ingressgateway.ports.name``       ``http2-grafana``
``gateways.istio-ingressgateway.ports.targetPort`` ``15011``
``gateways.istio-ingressgateway.ports.targetPort`` ``8060``
================================================== ======================== ===========

Removed ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================== ============================== ===========
Key                                Default Value                  Description
================================== ============================== ===========
``tracing.service.internalPort``   ``9411``
``tracing.replicaCount``           ``1``
``tracing.jaeger.ingress``         ``jaeger.local``
``tracing.ingress``                ``tracing.local``
``tracing.jaeger``                 ``jaeger.local``
``tracing``                        ``jaeger.local tracing.local``
``tracing.jaeger.ingress.hosts``   ``jaeger.local``
``tracing.jaeger.ingress.enabled`` ``false``
``tracing.ingress.hosts``          ``tracing.local``
``tracing.jaeger.ui.port``         ``16686``
================================== ============================== ===========

Removed ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============================== ============= ===========
Key                            Default Value Description
============================== ============= ===========
``kiali.dashboard.username``   ``admin``
``kiali.dashboard.passphrase`` ``admin``
============================== ============= ===========

Removed ``pilot`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

====================== ============= ===========
Key                    Default Value Description
====================== ============= ===========
``pilot.replicaCount`` ``1``
====================== ============= ===========

.. raw:: html

   <!-- AUTO-GENERATED-END -->
