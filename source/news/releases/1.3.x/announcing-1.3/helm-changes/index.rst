helm-changes
================

The tables below show changes made to the installation options used to
customize Istio install using Helm between Istio 1.2 and Istio 1.3. The
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

============= ================= ================= =============== ===============
Key           Old Default Value New Default Value Old Description New Description
============= ================= ================= =============== ===============
``kiali.tag`` ``v0.20``         ``v1.1.0``
============= ================= ================= =============== ===============

Modified ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+-------------+-------------+-------------+-------------+
| Key         | Old Default | New Default | Old         | New         |
|             | Value       | Value       | Description | Description |
+=============+=============+=============+=============+=============+
| ``global.ta | ``1.2.0-rc. | ``release-1 | ``Default t | ``Default t |
| g``         | 3``         | .3-latest-d | ag for Isti | ag for Isti |
|             |             | aily``      | o images.`` | o images.`` |
+-------------+-------------+-------------+-------------+-------------+

Modified ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

========================================================  =========  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========
                          Key                                Old      Default    Value  New  Default  Value  Old  Description  New  Description
========================================================  =========  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========
``gateways.istio-egressgateway.resources.limits.memory``  ``256Mi``  ``1024Mi``
========================================================  =========  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========

Modified ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======================  =======  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========
         Key              Old     Default    Value  New  Default  Value  Old  Description  New  Description
======================  =======  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========
``tracing.jaeger.tag``  ``1.9``  ``1.12``
``tracing.zipkin.tag``  ``2``    ``2.14.2``
======================  =======  ==========  =====  ===  =======  =====  ===  ===========  ===  ===========

New configuration options
-------------------------

New ``tracing`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------------------+-------------------+-----------------------+
|           Key           |   Default Value   |      Description      |
+=========================+===================+=======================+
| ``tracing.tolerations`` | ``[]``            |                       |
+-------------------------+-------------------+-----------------------+
| ``tracing.jaeger.imag   | ``all-in-one``    |                       |
| e``                     |                   |                       |
+-------------------------+-------------------+-----------------------+
| ``tracing.jaeger.span   | ``badger``        | ``spanStorageType val |
| StorageType``           |                   | ue can be "memory" an |
|                         |                   | d "badger" for all-in |
|                         |                   | -one image``          |
+-------------------------+-------------------+-----------------------+
| ``tracing.jaeger.pers   | ``false``         |                       |
| ist``                   |                   |                       |
+-------------------------+-------------------+-----------------------+
| ``tracing.jaeger.stor   | ``""``            |                       |
| ageClassName``          |                   |                       |
+-------------------------+-------------------+-----------------------+
| ``tracing.jaeger.acce   | ``ReadWriteMany`` |                       |
| ssMode``                |                   |                       |
+-------------------------+-------------------+-----------------------+
| ``tracing.zipkin.imag   | ``zipkin``        |                       |
| e``                     |                   |                       |
+-------------------------+-------------------+-----------------------+

New ``sidecarInjectorWebhook`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================  ========  =====  ===========
                      Key                         Default   Value  Description
================================================  ========  =====  ===========
``sidecarInjectorWebhook.rollingMaxSurge``        ``100%``
``sidecarInjectorWebhook.rollingMaxUnavailable``  ``25%``
``sidecarInjectorWebhook.tolerations``            ``[]``
================================================  ========  =====  ===========

New ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------------------+-----------------------+------------------------+-----------------------+-----+
|          Key           |     Default Value     |      Description       |                       |     |
+========================+=======================+========================+=======================+=====+
| ``global.proxy.init.r  | ``100m``              |                        |                       |     |
| esources.limits.cpu``  |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.init.r  | ``50Mi``              |                        |                       |     |
| esources.limits.memor  |                       |                        |                       |     |
| y``                    |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.init.r  | ``10m``               |                        |                       |     |
| esources.requests.cpu  |                       |                        |                       |     |
| ``                     |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.init.r  | ``10Mi``              |                        |                       |     |
| esources.requests.mem  |                       |                        |                       |     |
| ory``                  |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``false``             |                        |                       |     |
| ccessLogService.enabl  |                       |                        |                       |     |
| ed``                   |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | :literal:`\           | `example               | ``example: 15000``    |     |
| ccessLogService.host`` | : accesslog-service.i |                        |                       |     |
| `                      | stio-system`          |                        | `glo                  |     |
|                        | bal.proxy.envoyAccess |                        |                       |     |
|                        | LogService.port` \    | `                      |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``DISABLE``           | ``DISABLE, SIMPLE, MU  |                       |     |
| ccessLogService.tlsSe  |                       | TUAL, ISTIO_MUTUAL``   |                       |     |
| ttings.mode``          |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | :literal:`\           | `example               | ``example: /etc/istio |     |
| ccessLogService.tlsSe  | : /etc/istio/als/cert | /als/key.pem``         |                       |     |
| ttings.clientCertific  | -chain.pem`           |                        | `glob                 |     |
| ate``                  | al.proxy.envoyAccessL |                        |                       |     |
|                        | ogService.tlsSettings |                        |                       |     |
|                        | .privateKey` \        | `                      |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | :literal:`\           | `example               | ``example: als.somedo |     |
| ccessLogService.tlsSe  | : /etc/istio/als/root | main``                 |                       |     |
| ttings.caCertificates  | -cert.pem`            |                        | `globa                |     |
| ``                     | l.proxy.envoyAccessLo |                        |                       |     |
|                        | gService.tlsSettings. |                        |                       |     |
|                        | sni` \                | `                      |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``[]``                |                        |                       |     |
| ccessLogService.tlsSe  |                       |                        |                       |     |
| ttings.subjectAltName  |                       |                        |                       |     |
| s``                    |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``3``                 |                        |                       |     |
| ccessLogService.tcpKe  |                       |                        |                       |     |
| epalive.probes``       |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``10s``               |                        |                       |     |
| ccessLogService.tcpKe  |                       |                        |                       |     |
| epalive.time``         |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.envoyA  | ``10s``               |                        |                       |     |
| ccessLogService.tcpKe  |                       |                        |                       |     |
| epalive.interval``     |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.protoc  | ``10ms``              | ``Automatic protocol   |                       |     |
| olDetectionTimeout``   |                       | detection uses a set   |                       |     |
|                        |                       | of heuristics to dete  |                       |     |
|                        |                       | rmine whether the con  |                       |     |
|                        |                       | nection is using TLS   |                       |     |
|                        |                       | or not (on the server  |                       |     |
|                        |                       | side), as well as th   |                       |     |
|                        |                       | e application protoco  |                       |     |
|                        |                       | l being used (e.g., h  |                       |     |
|                        |                       | ttp vs tcp). These he  |                       |     |
|                        |                       | uristics rely on the   |                       |     |
|                        |                       | client sending the fi  |                       |     |
|                        |                       | rst bits of data. For  |                       |     |
|                        |                       | server first protoco   |                       |     |
|                        |                       | ls like MySQL, MongoD  |                       |     |
|                        |                       | B, etc., Envoy will t  |                       |     |
|                        |                       | imeout on the protoco  |                       |     |
|                        |                       | l detection after the  |                       |     |
|                        |                       | specified period, de   |                       |     |
|                        |                       | faulting to non mTLS   |                       |     |
|                        |                       | plain TCP traffic. Se  |                       |     |
|                        |                       | t this field to tweak  |                       |     |
|                        |                       | the period that Envo   |                       |     |
|                        |                       | y will wait for the c  |                       |     |
|                        |                       | lient to send the fir  |                       |     |
|                        |                       | st bits of data. (MUS  |                       |     |
|                        |                       | T BE >=1ms)``          |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.proxy.enable  | ``ubuntu:xenial``     | ``Image used to enabl  |                       |     |
| CoreDumpImage``        |                       | e core dumps. This is  |                       |     |
|                        |                       | only used, when "ena   |                       |     |
|                        |                       | bleCoreDump" is set t  |                       |     |
|                        |                       | o true.``              |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.defaultToler  | ``[]``                | ``Default node tolera  |                       |     |
| ations``               |                       | tions to be applied t  |                       |     |
|                        |                       | o all deployments so   |                       |     |
|                        |                       | that all pods can be   |                       |     |
|                        |                       | scheduled to a partic  |                       |     |
|                        |                       | ular nodes with match  |                       |     |
|                        |                       | ing taints. Each comp  |                       |     |
|                        |                       | onent can overwrite t  |                       |     |
|                        |                       | hese default values b  |                       |     |
|                        |                       | y adding its tolerati  |                       |     |
|                        |                       | ons block in the rele  |                       |     |
|                        |                       | vant section below an  |                       |     |
|                        |                       | d setting the desired  |                       |     |
|                        |                       | values. Configure th   |                       |     |
|                        |                       | is field in case that  |                       |     |
|                        |                       | all pods of Istio co   |                       |     |
|                        |                       | ntrol plane are expec  |                       |     |
|                        |                       | ted to be scheduled t  |                       |     |
|                        |                       | o particular nodes wi  |                       |     |
|                        |                       | th specified taints.`` |                       |     |
|                        |                       | `                      |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.meshID``      | ``""``                | ``Mesh ID means Mesh   |                       |     |
|                        |                       | Identifier. It should  |                       |     |
|                        |                       | be unique within the   |                       |     |
|                        |                       | scope where meshes w   |                       |     |
|                        |                       | ill interact with eac  |                       |     |
|                        |                       | h other, but it is no  |                       |     |
|                        |                       | t required to be glob  |                       |     |
|                        |                       | ally/universally uniq  |                       |     |
|                        |                       | ue. For example, if a  |                       |     |
|                        |                       | ny of the following a  |                       |     |
|                        |                       | re true, then two mes  |                       |     |
|                        |                       | hes must have differe  |                       |     |
|                        |                       | nt Mesh IDs: - Meshes  |                       |     |
|                        |                       | will have their tele   |                       |     |
|                        |                       | metry aggregated in o  |                       |     |
|                        |                       | ne place - Meshes wil  |                       |     |
|                        |                       | l be federated togeth  |                       |     |
|                        |                       | er - Policy will be w  |                       |     |
|                        |                       | ritten referencing on  |                       |     |
|                        |                       | e mesh from the other  |                       |     |
|                        |                       | If an administrator    |                       |     |
|                        |                       | expects that any of t  |                       |     |
|                        |                       | hese conditions may b  |                       |     |
|                        |                       | ecome true in the fut  |                       |     |
|                        |                       | ure, they should ensu  |                       |     |
|                        |                       | re their meshes have   |                       |     |
|                        |                       | different Mesh IDs as  |                       |     |
|                        |                       | signed. Within a mult  |                       |     |
|                        |                       | icluster mesh, each c  |                       |     |
|                        |                       | luster must be (manua  |                       |     |
|                        |                       | lly or auto) configur  |                       |     |
|                        |                       | ed to have the same M  |                       |     |
|                        |                       | esh ID value. If an e  |                       |     |
|                        |                       | xisting cluster 'join  |                       |     |
|                        |                       | s' a multicluster mes  |                       |     |
|                        |                       | h, it will need to be  |                       |     |
|                        |                       | migrated to the new    |                       |     |
|                        |                       | mesh ID. Details of m  |                       |     |
|                        |                       | igration TBD, and it   |                       |     |
|                        |                       | may be a disruptive o  |                       |     |
|                        |                       | peration to change th  |                       |     |
|                        |                       | e Mesh ID post-instal  |                       |     |
|                        |                       | l. If the mesh admin   |                       |     |
|                        |                       | does not specify a va  |                       |     |
|                        |                       | lue, Istio will use t  |                       |     |
|                        |                       | he value of the mesh'  |                       |     |
|                        |                       | s Trust Domain. The b  |                       |     |
|                        |                       | est practice is to se  |                       |     |
|                        |                       | lect a proper Trust D  |                       |     |
|                        |                       | omain value.``         |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+
| ``global.localityLbSe  | ``true``              |                        |                       |     |
| tting.enabled``        |                       |                        |                       |     |
+------------------------+-----------------------+------------------------+-----------------------+-----+

New ``galley`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================  ========  =====  ===========
              Key                 Default   Value  Description
================================  ========  =====  ===========
``galley.rollingMaxSurge``        ``100%``
``galley.rollingMaxUnavailable``  ``25%``
================================  ========  =====  ===========

New ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+---------------+-----------------------+
|          Key          | Default Value |      Description      |
+=======================+===============+=======================+
| ``mixer.policy.rollin | ``100%``      |                       |
| gMaxSurge``           |               |                       |
+-----------------------+---------------+-----------------------+
| ``mixer.policy.rollin | ``25%``       |                       |
| gMaxUnavailable``     |               |                       |
+-----------------------+---------------+-----------------------+
| ``mixer.telemetry.rol | ``100%``      |                       |
| lingMaxSurge``        |               |                       |
+-----------------------+---------------+-----------------------+
| ``mixer.telemetry.rol | ``25%``       |                       |
| lingMaxUnavailable``  |               |                       |
+-----------------------+---------------+-----------------------+
| ``mixer.telemetry.rep | ``100``       | ``Set reportBatchMaxE |
| ortBatchMaxEntries``  |               | ntries to 0 to use th |
|                       |               | e default batching be |
|                       |               | havior (i.e., every 1 |
|                       |               | 00 requests). A posit |
|                       |               | ive value indicates t |
|                       |               | he number of requests |
|                       |               | that are batched bef  |
|                       |               | ore telemetry data is |
|                       |               | sent to the mixer se  |
|                       |               | rver``                |
+-----------------------+---------------+-----------------------+
| ``mixer.telemetry.rep | ``1s``        | ``Set reportBatchMaxT |
| ortBatchMaxTime``     |               | ime to 0 to use the d |
|                       |               | efault batching behav |
|                       |               | ior (i.e., every 1 se |
|                       |               | cond). A positive tim |
|                       |               | e value indicates the |
|                       |               | maximum wait time si  |
|                       |               | nce the last request  |
|                       |               | will telemetry data b |
|                       |               | e batched before bein |
|                       |               | g sent to the mixer s |
|                       |               | erver``               |
+-----------------------+---------------+-----------------------+

New ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+-----------------------+-------------+
|          Key          |     Default Value     | Description |
+=======================+=======================+=============+
| ``grafana.env``       | ``{}``                |             |
+-----------------------+-----------------------+-------------+
| ``grafana.envSecrets` | ``{}``                |             |
| `                     |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``1``                 |             |
| .datasources.datasour |                       |             |
| ces.type.orgId``      |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``http://prometheus:9 |             |
| .datasources.datasour | 090``                 |             |
| ces.type.url``        |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``proxy``             |             |
| .datasources.datasour |                       |             |
| ces.type.access``     |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``true``              |             |
| .datasources.datasour |                       |             |
| ces.type.isDefault``  |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``5s``                |             |
| .datasources.datasour |                       |             |
| ces.type.jsonData.tim |                       |             |
| eInterval``           |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.datasources | ``true``              |             |
| .datasources.datasour |                       |             |
| ces.type.editable``   |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.dashboardPr | ``'istio'``           |             |
| oviders.dashboardprov |                       |             |
| iders.providers.orgId |                       |             |
| .folder``             |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.dashboardPr | ``file``              |             |
| oviders.dashboardprov |                       |             |
| iders.providers.orgId |                       |             |
| .type``               |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.dashboardPr | ``false``             |             |
| oviders.dashboardprov |                       |             |
| iders.providers.orgId |                       |             |
| .disableDeletion``    |                       |             |
+-----------------------+-----------------------+-------------+
| ``grafana.dashboardPr | ``/var/lib/grafana/da |             |
| oviders.dashboardprov | shboards/istio``      |             |
| iders.providers.orgId |                       |             |
| .options.path``       |                       |             |
+-----------------------+-----------------------+-------------+

New ``prometheus`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

====================  ==============  =====  ===========
        Key              Default      Value  Description
====================  ==============  =====  ===========
``prometheus.image``  ``prometheus``
====================  ==============  =====  ===========

New ``gateways`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=======================================================  ========  =====  ===========
                          Key                            Default   Value  Description
=======================================================  ========  =====  ===========
``gateways.istio-ingressgateway.rollingMaxSurge``        ``100%``
``gateways.istio-ingressgateway.rollingMaxUnavailable``  ``25%``
``gateways.istio-egressgateway.rollingMaxSurge``         ``100%``
``gateways.istio-egressgateway.rollingMaxUnavailable``   ``25%``
``gateways.istio-ilbgateway.rollingMaxSurge``            ``100%``
``gateways.istio-ilbgateway.rollingMaxUnavailable``      ``25%``
=======================================================  ========  =====  ===========

New ``certmanager`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

=====================  ===========================  =====  ===========
         Key                     Default            Value  Description
=====================  ===========================  =====  ===========
``certmanager.image``  ``cert-manager-controller``
=====================  ===========================  =====  ===========

New ``kiali`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===================================  ==============================  =====  ===========  ==========  ======  ===  ===========
                Key                             Default              Value  Description
===================================  ==============================  =====  ===========  ==========  ======  ===  ===========
``kiali.image``                      ``kiali``
``kiali.tolerations``                ``[]``
``kiali.dashboard.auth.strategy``    ``login``                       ``Can  be           anonymous,  login,  or   openshift``
``kiali.security.enabled``           ``true``
``kiali.security.cert_file``         ``/kiali-cert/cert-chain.pem``
``kiali.security.private_key_file``  ``/kiali-cert/key.pem``
===================================  ==============================  =====  ===========  ==========  ======  ===  ===========

New ``istiocoredns`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======================================  ========  =====  ===========
                 Key                    Default   Value  Description
======================================  ========  =====  ===========
``istiocoredns.rollingMaxSurge``        ``100%``
``istiocoredns.rollingMaxUnavailable``  ``25%``
======================================  ========  =====  ===========

New ``security`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+---------------+-----------------------+
|          Key          | Default Value |      Description      |
+=======================+===============+=======================+
| ``security.replicaCou | ``1``         |                       |
| nt``                  |               |                       |
+-----------------------+---------------+-----------------------+
| ``security.rollingMax | ``100%``      |                       |
| Surge``               |               |                       |
+-----------------------+---------------+-----------------------+
| ``security.rollingMax | ``25%``       |                       |
| Unavailable``         |               |                       |
+-----------------------+---------------+-----------------------+
| ``security.workloadCe | ``2160h``     | ``90*24hour = 2160h`` |
| rtTtl``               |               |                       |
+-----------------------+---------------+-----------------------+
| ``security.enableName | ``true``      | ``Determines Citadel  |
| spacesByDefault``     |               | default behavior if t |
|                       |               | he ca.istio.io/env or |
|                       |               | ca.istio.io/override  |
|                       |               | labels are not found  |
|                       |               | on a given namespace  |
|                       |               | . For example: consid |
|                       |               | er a namespace called |
|                       |               | "target", which has   |
|                       |               | neither the "ca.istio |
|                       |               | .io/env" nor the "ca. |
|                       |               | istio.io/override" na |
|                       |               | mespace labels. To de |
|                       |               | cide whether or not t |
|                       |               | o generate secrets fo |
|                       |               | r service accounts cr |
|                       |               | eated in this "target |
|                       |               | " namespace, Citadel  |
|                       |               | will defer to this op |
|                       |               | tion. If the value of |
|                       |               | this option is "true  |
|                       |               | " in this case, secre |
|                       |               | ts will be generated  |
|                       |               | for the "target" name |
|                       |               | space. If the value o |
|                       |               | f this option is "fal |
|                       |               | se" Citadel will not  |
|                       |               | generate secrets upon |
|                       |               | service account crea  |
|                       |               | tion.``               |
+-----------------------+---------------+-----------------------+

New ``pilot`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-----------------------+---------------+-----------------------+
|          Key          | Default Value |      Description      |
+=======================+===============+=======================+
| ``pilot.rollingMaxSur | ``100%``      |                       |
| ge``                  |               |                       |
+-----------------------+---------------+-----------------------+
| ``pilot.rollingMaxUna | ``25%``       |                       |
| vailable``            |               |                       |
+-----------------------+---------------+-----------------------+
| ``pilot.enableProtoco | ``false``     | ``if protocol sniffin |
| lSniffing``           |               | g is enabled. Default |
|                       |               | to false.``           |
+-----------------------+---------------+-----------------------+

Removed configuration options
-----------------------------

Removed ``global`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================  =========  =====  ===========
              Key                  Default   Value  Description
================================  =========  =====  ===========
``global.sds.useTrustworthyJwt``  ``false``
``global.sds.useNormalJwt``       ``false``
``global.localityLbSetting``      ``{}``
================================  =========  =====  ===========

Removed ``mixer`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===================================  =========  =====  ===========
                Key                   Default   Value  Description
===================================  =========  =====  ===========
``mixer.templates.useTemplateCRDs``  ``false``
===================================  =========  =====  ===========

Removed ``grafana`` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------------------+-----------------------+-------------+
|          Key           |     Default Value     | Description |
+========================+=======================+=============+
| ``grafana.dashboardPr  | ``false``             |             |
| oviders.dashboardprov  |                       |             |
| iders.providers.disab  |                       |             |
| leDeletion``           |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.dashboardPr  | ``file``              |             |
| oviders.dashboardprov  |                       |             |
| iders.providers.type`` |                       |             |
|                        |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.dashboardPr  | ``'istio'``           |             |
| oviders.dashboardprov  |                       |             |
| iders.providers.folde  |                       |             |
| r``                    |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``true``              |             |
| .datasources.datasour  |                       |             |
| ces.isDefault``        |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``http://prometheus:9 |             |
| .datasources.datasour  | 090``                 |             |
| ces.url``              |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``proxy``             |             |
| .datasources.datasour  |                       |             |
| ces.access``           |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``5s``                |             |
| .datasources.datasour  |                       |             |
| ces.jsonData.timeInte  |                       |             |
| rval``                 |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.dashboardPr  | ``/var/lib/grafana/da |             |
| oviders.dashboardprov  | shboards/istio``      |             |
| iders.providers.optio  |                       |             |
| ns.path``              |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``true``              |             |
| .datasources.datasour  |                       |             |
| ces.editable``         |                       |             |
+------------------------+-----------------------+-------------+
| ``grafana.datasources  | ``1``                 |             |
| .datasources.datasour  |                       |             |
| ces.orgId``            |                       |             |
+------------------------+-----------------------+-------------+

.. raw:: html

   <!-- AUTO-GENERATED-END -->
