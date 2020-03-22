installation-options
===========================================

.. warning::

   Installing Istio with Helm is in the process of
deprecation, however, you can use these Helm configuration options when
`installing Istio with {{< istioctl
>}} </docs/setup/install/istioctl/>`_ by prepending the string
“``values.``” to the option name. For example, instead of this ``helm``
command:

.. code:: sh

      $ helm template … –set
global.controlPlaneSecurityEnabled=true

You can use this ``istioctl`` command:

.. code:: sh

      $ istioctl manifest generate … –set
values.global.controlPlaneSecurityEnabled=true

Refer to `customizing the
configuration </docs/setup/install/istioctl/#customizing-the-configuration>`_
for details.

.. warning::

   This document is unfortunately out of date with the
latest changes in the set of supported options. To get the exact set of
supported options, please see the `Helm
charts <%7B%7B%3C%20github_tree%20%3E%7D%7D/install/kubernetes/helm/istio>`_.


.. raw:: html

   <!-- Run python scripts/tablegen.py to generate this table -->

.. raw:: html

   <!-- AUTO-GENERATED-START -->

``certmanager`` options
-----------------------

================================================ =========================== ===========
Key                                              Default Value               Description
================================================ =========================== ===========
``certmanager.enabled``                          ``false``
``certmanager.replicaCount``                     ``1``
``certmanager.hub``                              ``quay.io/jetstack``
``certmanager.image``                            ``cert-manager-controller``
``certmanager.tag``                              ``v0.6.2``
``certmanager.resources``                        ``{}``
``certmanager.nodeSelector``                     ``{}``
``certmanager.tolerations``                      ``[]``
``certmanager.podAntiAffinityLabelSelector``     ``[]``
``certmanager.podAntiAffinityTermLabelSelector`` ``[]``
================================================ =========================== ===========

``galley`` options
------------------

=========================================== ============= ===========
Key                                         Default Value Description
=========================================== ============= ===========
``galley.enabled``                          ``true``
``galley.replicaCount``                     ``1``
``galley.rollingMaxSurge``                  ``100%``
``galley.rollingMaxUnavailable``            ``25%``
``galley.image``                            ``galley``
``galley.nodeSelector``                     ``{}``
``galley.tolerations``                      ``[]``
``galley.podAntiAffinityLabelSelector``     ``[]``
``galley.podAntiAffinityTermLabelSelector`` ``[]``
=========================================== ============= ===========

``gateways`` options
--------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``gateways.enabled``  | ``true``              |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``true``              |                       |
| essgateway.enabled``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
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
|                       |                       | ateway. This server r |
|                       |                       | uns in the same pod a |
|                       |                       | s ingress gateway.``  |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``100m``              |                       |
| essgateway.sds.resour |                       |                       |
| ces.requests.cpu``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``128Mi``             |                       |
| essgateway.sds.resour |                       |                       |
| ces.requests.memory`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``2000m``             |                       |
| essgateway.sds.resour |                       |                       |
| ces.limits.cpu``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``1024Mi``            |                       |
| essgateway.sds.resour |                       |                       |
| ces.limits.memory``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``istio-ingressgatewa |                       |
| essgateway.labels.app | y``                   |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``ingressgateway``    |                       |
| essgateway.labels.ist |                       |                       |
| io``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``true``              |                       |
| essgateway.autoscaleE |                       |                       |
| nabled``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``1``                 |                       |
| essgateway.autoscaleM |                       |                       |
| in``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``5``                 |                       |
| essgateway.autoscaleM |                       |                       |
| ax``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``100%``              |                       |
| essgateway.rollingMax |                       |                       |
| Surge``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``25%``               |                       |
| essgateway.rollingMax |                       |                       |
| Unavailable``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``100m``              |                       |
| essgateway.resources. |                       |                       |
| requests.cpu``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``128Mi``             |                       |
| essgateway.resources. |                       |                       |
| requests.memory``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``2000m``             |                       |
| essgateway.resources. |                       |                       |
| limits.cpu``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``1024Mi``            |                       |
| essgateway.resources. |                       |                       |
| limits.memory``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``80``                |                       |
| essgateway.cpu.target |                       |                       |
| AverageUtilization``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``""``                |                       |
| essgateway.loadBalanc |                       |                       |
| erIP``                |                       |                       |
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
| essgateway.serviceAnn |                       |                       |
| otations``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``{}``                |                       |
| essgateway.podAnnotat |                       |                       |
| ions``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``LoadBalancer``      | ``change to NodePort, |
| essgateway.type``     |                       |  ClusterIP or LoadBal |
|                       |                       | ancer if need be``    |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15020``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``status-port``       |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``80``                |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``http2``             |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``31380``             |                       |
| essgateway.ports.node |                       |                       |
| Port``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https``             |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``31390``             |                       |
| essgateway.ports.node |                       |                       |
| Port``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``tcp``               |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``31400``             |                       |
| essgateway.ports.node |                       |                       |
| Port``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15029``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-kiali``       |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15030``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``https-prometheus``  |                       |
| essgateway.ports.name |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``15031``             |                       |
| essgateway.ports.targ |                       |                       |
| etPort``              |                       |                       |
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
| ``gateways.istio-ingr | ``istio-ingressgatewa |                       |
| essgateway.secretVolu | y-certs``             |                       |
| mes.secretName``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``/etc/istio/ingressg |                       |
| essgateway.secretVolu | ateway-certs``        |                       |
| mes.mountPath``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``istio-ingressgatewa |                       |
| essgateway.secretVolu | y-ca-certs``          |                       |
| mes.secretName``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``/etc/istio/ingressg |                       |
| essgateway.secretVolu | ateway-ca-certs``     |                       |
| mes.mountPath``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``""``                |                       |
| essgateway.applicatio |                       |                       |
| nPorts``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``"sni-dnat"``        | ``A gateway with this |
| essgateway.env.ISTIO_ |                       |  mode ensures that pi |
| META_ROUTER_MODE``    |                       | lot generates an addi |
|                       |                       | tional set of cluster |
|                       |                       | s for internal servic |
|                       |                       | es but without Istio  |
|                       |                       | mTLS, to enable cross |
|                       |                       |  cluster routing.``   |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``{}``                |                       |
| essgateway.nodeSelect |                       |                       |
| or``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``[]``                |                       |
| essgateway.toleration |                       |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``[]``                |                       |
| essgateway.podAntiAff |                       |                       |
| inityLabelSelector``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ingr | ``[]``                |                       |
| essgateway.podAntiAff |                       |                       |
| inityTermLabelSelecto |                       |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``false``             |                       |
| ssgateway.enabled``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``istio-egressgateway |                       |
| ssgateway.labels.app` | ``                    |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``egressgateway``     |                       |
| ssgateway.labels.isti |                       |                       |
| o``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``true``              |                       |
| ssgateway.autoscaleEn |                       |                       |
| abled``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``1``                 |                       |
| ssgateway.autoscaleMi |                       |                       |
| n``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``5``                 |                       |
| ssgateway.autoscaleMa |                       |                       |
| x``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``100%``              |                       |
| ssgateway.rollingMaxS |                       |                       |
| urge``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``25%``               |                       |
| ssgateway.rollingMaxU |                       |                       |
| navailable``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``100m``              |                       |
| ssgateway.resources.r |                       |                       |
| equests.cpu``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``128Mi``             |                       |
| ssgateway.resources.r |                       |                       |
| equests.memory``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``2000m``             |                       |
| ssgateway.resources.l |                       |                       |
| imits.cpu``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``1024Mi``            |                       |
| ssgateway.resources.l |                       |                       |
| imits.memory``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``80``                |                       |
| ssgateway.cpu.targetA |                       |                       |
| verageUtilization``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``{}``                |                       |
| ssgateway.serviceAnno |                       |                       |
| tations``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``{}``                |                       |
| ssgateway.podAnnotati |                       |                       |
| ons``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``ClusterIP``         | ``change to NodePort  |
| ssgateway.type``      |                       | or LoadBalancer if ne |
|                       |                       | ed be``               |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``http2``             |                       |
| ssgateway.ports.name` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``https``             |                       |
| ssgateway.ports.name` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``15443``             |                       |
| ssgateway.ports.targe |                       |                       |
| tPort``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``tls``               |                       |
| ssgateway.ports.name` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``istio-egressgateway |                       |
| ssgateway.secretVolum | -certs``              |                       |
| es.secretName``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``/etc/istio/egressga |                       |
| ssgateway.secretVolum | teway-certs``         |                       |
| es.mountPath``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``istio-egressgateway |                       |
| ssgateway.secretVolum | -ca-certs``           |                       |
| es.secretName``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``/etc/istio/egressga |                       |
| ssgateway.secretVolum | teway-ca-certs``      |                       |
| es.mountPath``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``"sni-dnat"``        |                       |
| ssgateway.env.ISTIO_M |                       |                       |
| ETA_ROUTER_MODE``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``{}``                |                       |
| ssgateway.nodeSelecto |                       |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``[]``                |                       |
| ssgateway.tolerations |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``[]``                |                       |
| ssgateway.podAntiAffi |                       |                       |
| nityLabelSelector``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-egre | ``[]``                |                       |
| ssgateway.podAntiAffi |                       |                       |
| nityTermLabelSelector |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``false``             |                       |
| ateway.enabled``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``istio-ilbgateway``  |                       |
| ateway.labels.app``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``ilbgateway``        |                       |
| ateway.labels.istio`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``true``              |                       |
| ateway.autoscaleEnabl |                       |                       |
| ed``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``1``                 |                       |
| ateway.autoscaleMin`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``5``                 |                       |
| ateway.autoscaleMax`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``100%``              |                       |
| ateway.rollingMaxSurg |                       |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``25%``               |                       |
| ateway.rollingMaxUnav |                       |                       |
| ailable``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``80``                |                       |
| ateway.cpu.targetAver |                       |                       |
| ageUtilization``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``800m``              |                       |
| ateway.resources.requ |                       |                       |
| ests.cpu``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``512Mi``             |                       |
| ateway.resources.requ |                       |                       |
| ests.memory``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``""``                |                       |
| ateway.loadBalancerIP |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``"internal"``        |                       |
| ateway.serviceAnnotat |                       |                       |
| ions.cloud.google.com |                       |                       |
| /load-balancer-type`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``{}``                |                       |
| ateway.podAnnotations |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``LoadBalancer``      |                       |
| ateway.type``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``grpc-pilot-mtls``   |                       |
| ateway.ports.name``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``grpc-pilot``        |                       |
| ateway.ports.name``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``8060``              |                       |
| ateway.ports.targetPo |                       |                       |
| rt``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``tcp-citadel-grpc-tl |                       |
| ateway.ports.name``   | s``                   |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``tcp-dns``           |                       |
| ateway.ports.name``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``istio-ilbgateway-ce |                       |
| ateway.secretVolumes. | rts``                 |                       |
| secretName``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``/etc/istio/ilbgatew |                       |
| ateway.secretVolumes. | ay-certs``            |                       |
| mountPath``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``istio-ilbgateway-ca |                       |
| ateway.secretVolumes. | -certs``              |                       |
| secretName``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``/etc/istio/ilbgatew |                       |
| ateway.secretVolumes. | ay-ca-certs``         |                       |
| mountPath``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``{}``                |                       |
| ateway.nodeSelector`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``gateways.istio-ilbg | ``[]``                |                       |
| ateway.tolerations``  |                       |                       |
+-----------------------+-----------------------+-----------------------+

``global`` options
------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``global.hub``        | :literal:`\| `Default | ``Default tag for Ist |
|                       |  hub for Istio images | io images.``          |
|                       | . Releases are publis |                       |
|                       | hed to docker hub und |                       |
|                       | er 'istio' project. D |                       |
|                       | aily builds from prow |                       |
|                       |  are on gcr.io` | | ` |                       |
|                       | global.tag` \|`       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.logging.leve | ``"default:info"``    |                       |
| l``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.monitoringPo | ``15014``             | ``monitoring port use |
| rt``                  |                       | d by mixer, pilot, ga |
|                       |                       | lley and sidecar inje |
|                       |                       | ctor``                |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.e | ``false``             |                       |
| nabled``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.g | ``ingressgateway``    | ``Gateway used for k8 |
| atewayName``          |                       | s Ingress resources.  |
|                       |                       | By default it is usin |
|                       |                       | g 'istio:ingressgatew |
|                       |                       | ay' that will be inst |
|                       |                       | alled by setting 'gat |
|                       |                       | eways.enabled' and 'g |
|                       |                       | ateways.istio-ingress |
|                       |                       | gateway.enabled' flag |
|                       |                       | s to true.``          |
+-----------------------+-----------------------+-----------------------+
| ``global.k8sIngress.e | ``false``             | ``enableHttps will ad |
| nableHttps``          |                       | d port 443 on the ing |
|                       |                       | ress. It REQUIRES tha |
|                       |                       | t the certificates ar |
|                       |                       | e installed  in the e |
|                       |                       | xpected secrets - ena |
|                       |                       | bling this option wit |
|                       |                       | hout certificates wil |
|                       |                       | l result in LDS rejec |
|                       |                       | tion and the ingress  |
|                       |                       | will not work.``      |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.init.r | ``100m``              |                       |
| esources.limits.cpu`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.init.r | ``50Mi``              |                       |
| esources.limits.memor |                       |                       |
| y``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.init.r | ``10m``               |                       |
| esources.requests.cpu |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.init.r | ``10Mi``              |                       |
| esources.requests.mem |                       |                       |
| ory``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.image` | ``proxyv2``           |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.cluste | ``"cluster.local"``   | ``cluster domain. Def |
| rDomain``             |                       | ault value is "cluste |
|                       |                       | r.local".``           |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``100m``              |                       |
| ces.requests.cpu``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``128Mi``             |                       |
| ces.requests.memory`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``2000m``             |                       |
| ces.limits.cpu``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.resour | ``1024Mi``            |                       |
| ces.limits.memory``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.concur | ``2``                 | ``Controls number of  |
| rency``               |                       | Proxy worker threads. |
|                       |                       |  If set to 0, then st |
|                       |                       | art worker thread for |
|                       |                       |  each CPU thread/core |
|                       |                       | .``                   |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.access | ``""``                |                       |
| LogFile``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.access | ``""``                | ``Configure how and w |
| LogFormat``           |                       | hat fields are displa |
|                       |                       | yed in sidecar access |
|                       |                       |  log. Setting to empt |
|                       |                       | y string will result  |
|                       |                       | in default log format |
|                       |                       | ``                    |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.access | ``TEXT``              | ``Configure the acces |
| LogEncoding``         |                       | s log for sidecar to  |
|                       |                       | JSON or TEXT.``       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``false``             |                       |
| ccessLogService.enabl |                       |                       |
| ed``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | :literal:`\| `example | ``example: 15000``    |
| ccessLogService.host` | : accesslog-service.i |                       |
| `                     | stio-system` | | `glo |                       |
|                       | bal.proxy.envoyAccess |                       |
|                       | LogService.port` \|`  |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``DISABLE``           | ``DISABLE, SIMPLE, MU |
| ccessLogService.tlsSe |                       | TUAL, ISTIO_MUTUAL``  |
| ttings.mode``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | :literal:`\| `example | ``example: /etc/istio |
| ccessLogService.tlsSe | : /etc/istio/als/cert | /als/key.pem``        |
| ttings.clientCertific | -chain.pem` | | `glob |                       |
| ate``                 | al.proxy.envoyAccessL |                       |
|                       | ogService.tlsSettings |                       |
|                       | .privateKey` \|`      |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | :literal:`\| `example | ``example: als.somedo |
| ccessLogService.tlsSe | : /etc/istio/als/root | main``                |
| ttings.caCertificates | -cert.pem` | | `globa |                       |
| ``                    | l.proxy.envoyAccessLo |                       |
|                       | gService.tlsSettings. |                       |
|                       | sni` \|`              |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``[]``                |                       |
| ccessLogService.tlsSe |                       |                       |
| ttings.subjectAltName |                       |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``3``                 |                       |
| ccessLogService.tcpKe |                       |                       |
| epalive.probes``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``10s``               |                       |
| ccessLogService.tcpKe |                       |                       |
| epalive.time``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyA | ``10s``               |                       |
| ccessLogService.tcpKe |                       |                       |
| epalive.interval``    |                       |                       |
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
| ``global.proxy.dnsRef | ``300s``              | ``Configure the DNS r |
| reshRate``            |                       | efresh rate for Envoy |
|                       |                       |  cluster of type STRI |
|                       |                       | CT_DNS This must be g |
|                       |                       | iven it terms of seco |
|                       |                       | nds. For example, 300 |
|                       |                       | s is valid but 5m is  |
|                       |                       | invalid.``            |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.protoc | ``10ms``              | ``Automatic protocol  |
| olDetectionTimeout``  |                       | detection uses a set  |
|                       |                       | of heuristics to dete |
|                       |                       | rmine whether the con |
|                       |                       | nection is using TLS  |
|                       |                       | or not (on the server |
|                       |                       |  side), as well as th |
|                       |                       | e application protoco |
|                       |                       | l being used (e.g., h |
|                       |                       | ttp vs tcp). These he |
|                       |                       | uristics rely on the  |
|                       |                       | client sending the fi |
|                       |                       | rst bits of data. For |
|                       |                       |  server first protoco |
|                       |                       | ls like MySQL, MongoD |
|                       |                       | B, etc., Envoy will t |
|                       |                       | imeout on the protoco |
|                       |                       | l detection after the |
|                       |                       |  specified period, de |
|                       |                       | faulting to non mTLS  |
|                       |                       | plain TCP traffic. Se |
|                       |                       | t this field to tweak |
|                       |                       |  the period that Envo |
|                       |                       | y will wait for the c |
|                       |                       | lient to send the fir |
|                       |                       | st bits of data. (MUS |
|                       |                       | T BE >=1ms)``         |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.privil | ``false``             | ``If set to true, ist |
| eged``                |                       | io-proxy container wi |
|                       |                       | ll have privileged se |
|                       |                       | curityContext``       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.enable | ``false``             | ``If set, newly injec |
| CoreDump``            |                       | ted sidecars will hav |
|                       |                       | e core dumps enabled. |
|                       |                       | ``                    |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.enable | ``ubuntu:xenial``     | ``Image used to enabl |
| CoreDumpImage``       |                       | e core dumps. This is |
|                       |                       |  only used, when "ena |
|                       |                       | bleCoreDump" is set t |
|                       |                       | o true.``             |
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
| ``global.proxy.includ | ``"*"``               |                       |
| eIPRanges``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.exclud | ``""``                |                       |
| eIPRanges``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.exclud | ``""``                |                       |
| eOutboundPorts``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.kubevi | ``""``                | ``pod internal interf |
| rtInterfaces``        |                       | aces``                |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.includ | ``"*"``               |                       |
| eInboundPorts``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.exclud | ``""``                |                       |
| eInboundPorts``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.autoIn | ``enabled``           | ``This controls the ' |
| ject``                |                       | policy' in the sideca |
|                       |                       | r injector.``         |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyS | ``false``             | ``If enabled is set t |
| tatsd.enabled``       |                       | o true, host and port |
|                       |                       |  must also be provide |
|                       |                       | d. Istio no longer pr |
|                       |                       | ovides a statsd colle |
|                       |                       | ctor.``               |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy.envoyS | :literal:`\| `example | ``example: 9125``     |
| tatsd.host``          | : statsd-svc.istio-sy |                       |
|                       | stem` | | `global.pro |                       |
|                       | xy.envoyStatsd.port`  |                       |
|                       | \|`                   |                       |
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
| ``                    |                       | r to use. One of: zip |
|                       |                       | kin, lightstep, datad |
|                       |                       | og, stackdriver. If u |
|                       |                       | sing stackdriver trac |
|                       |                       | er outside GCP, set e |
|                       |                       | nv GOOGLE_APPLICATION |
|                       |                       | _CREDENTIALS to the G |
|                       |                       | CP credential file.`` |
+-----------------------+-----------------------+-----------------------+
| ``global.proxy_init.i | ``proxy_init``        | ``Base name for the p |
| mage``                |                       | roxy_init container,  |
|                       |                       | used to configure ipt |
|                       |                       | ables.``              |
+-----------------------+-----------------------+-----------------------+
| ``global.imagePullPol | ``IfNotPresent``      |                       |
| icy``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.controlPlane | ``false``             | ``controlPlaneSecurit |
| SecurityEnabled``     |                       | yEnabled enabled. Wil |
|                       |                       | l result in delays st |
|                       |                       | arting the pods while |
|                       |                       |  secrets are propagat |
|                       |                       | ed, not recommended f |
|                       |                       | or tests.``           |
+-----------------------+-----------------------+-----------------------+
| ``global.disablePolic | ``true``              | ``disablePolicyChecks |
| yChecks``             |                       |  disables mixer polic |
|                       |                       | y checks. if mixer.po |
|                       |                       | licy.enabled==true th |
|                       |                       | en disablePolicyCheck |
|                       |                       | s has affect. Will se |
|                       |                       | t the value with same |
|                       |                       |  name in istio config |
|                       |                       |  map - pilot needs to |
|                       |                       |  be restarted to take |
|                       |                       |  effect.``            |
+-----------------------+-----------------------+-----------------------+
| ``global.policyCheckF | ``false``             | ``policyCheckFailOpen |
| ailOpen``             |                       |  allows traffic in ca |
|                       |                       | ses when the mixer po |
|                       |                       | licy service cannot b |
|                       |                       | e reached. Default is |
|                       |                       |  false which means th |
|                       |                       | e traffic is denied w |
|                       |                       | hen the client is una |
|                       |                       | ble to connect to Mix |
|                       |                       | er.``                 |
+-----------------------+-----------------------+-----------------------+
| ``global.enableTracin | ``true``              | ``EnableTracing sets  |
| g``                   |                       | the value with same n |
|                       |                       | ame in istio config m |
|                       |                       | ap, requires pilot re |
|                       |                       | start to take effect. |
|                       |                       | ``                    |
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
| ``global.tracer.datad | ``"$(HOST_IP):8126"`` |                       |
| og.address``          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.mtls.enabled | ``false``             | ``Default setting for |
| ``                    |                       |  service-to-service m |
|                       |                       | tls. Can be set expli |
|                       |                       | citly using destinati |
|                       |                       | on rules or service a |
|                       |                       | nnotations.``         |
+-----------------------+-----------------------+-----------------------+
| ``global.imagePullSec | ``[]``                | ``Lists the secrets y |
| rets``                |                       | ou need to use to pul |
|                       |                       | l Istio images from a |
|                       |                       |  private registry.``  |
+-----------------------+-----------------------+-----------------------+
| ``global.arch.amd64`` | ``2``                 |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.arch.s390x`` | ``2``                 |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.arch.ppc64le | ``2``                 |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.oneNamespace | ``false``             | ``Whether to restrict |
| ``                    |                       |  the applications nam |
|                       |                       | espace the controller |
|                       |                       |  manages; If not set, |
|                       |                       |  controller watches a |
|                       |                       | ll namespaces``       |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultNodeS | ``{}``                | ``Default node select |
| elector``             |                       | or to be applied to a |
|                       |                       | ll deployments so tha |
|                       |                       | t all pods can be con |
|                       |                       | strained to run a par |
|                       |                       | ticular nodes. Each c |
|                       |                       | omponent can overwrit |
|                       |                       | e these default value |
|                       |                       | s by adding its node  |
|                       |                       | selector block in the |
|                       |                       |  relevant section bel |
|                       |                       | ow and setting the de |
|                       |                       | sired values.``       |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultToler | ``[]``                | ``Default node tolera |
| ations``              |                       | tions to be applied t |
|                       |                       | o all deployments so  |
|                       |                       | that all pods can be  |
|                       |                       | scheduled to a partic |
|                       |                       | ular nodes with match |
|                       |                       | ing taints. Each comp |
|                       |                       | onent can overwrite t |
|                       |                       | hese default values b |
|                       |                       | y adding its tolerati |
|                       |                       | ons block in the rele |
|                       |                       | vant section below an |
|                       |                       | d setting the desired |
|                       |                       |  values. Configure th |
|                       |                       | is field in case that |
|                       |                       |  all pods of Istio co |
|                       |                       | ntrol plane are expec |
|                       |                       | ted to be scheduled t |
|                       |                       | o particular nodes wi |
|                       |                       | th specified taints.` |
|                       |                       | `                     |
+-----------------------+-----------------------+-----------------------+
| ``global.configValida | ``true``              | ``Whether to perform  |
| tion``                |                       | server-side validatio |
|                       |                       | n of configuration.`` |
+-----------------------+-----------------------+-----------------------+
| ``global.meshExpansio | ``false``             |                       |
| n.enabled``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.meshExpansio | ``false``             | ``If set to true, the |
| n.useILB``            |                       |  pilot and citadel mt |
|                       |                       | ls and the plaintext  |
|                       |                       | pilot ports will be e |
|                       |                       | xposed on an internal |
|                       |                       |  gateway``            |
+-----------------------+-----------------------+-----------------------+
| ``global.multiCluster | ``false``             | ``Set to true to conn |
| .enabled``            |                       | ect two kubernetes cl |
|                       |                       | usters via their resp |
|                       |                       | ective ingressgateway |
|                       |                       |  services when pods i |
|                       |                       | n each cluster cannot |
|                       |                       |  directly talk to one |
|                       |                       |  another. All cluster |
|                       |                       | s should be using Ist |
|                       |                       | io mTLS and must have |
|                       |                       |  a shared root CA for |
|                       |                       |  this model to work.` |
|                       |                       | `                     |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultResou | ``10m``               |                       |
| rces.requests.cpu``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.defaultPodDi | ``true``              |                       |
| sruptionBudget.enable |                       |                       |
| d``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.priorityClas | ``""``                |                       |
| sName``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.useMCP``     | ``true``              | ``Use the Mesh Contro |
|                       |                       | l Protocol (MCP) for  |
|                       |                       | configuring Mixer and |
|                       |                       |  Pilot. Requires gall |
|                       |                       | ey (--set galley.enab |
|                       |                       | led=true).``          |
+-----------------------+-----------------------+-----------------------+
| ``global.trustDomain` | ``""``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.meshID``     | ``""``                | ``Mesh ID means Mesh  |
|                       |                       | Identifier. It should |
|                       |                       |  be unique within the |
|                       |                       |  scope where meshes w |
|                       |                       | ill interact with eac |
|                       |                       | h other, but it is no |
|                       |                       | t required to be glob |
|                       |                       | ally/universally uniq |
|                       |                       | ue. For example, if a |
|                       |                       | ny of the following a |
|                       |                       | re true, then two mes |
|                       |                       | hes must have differe |
|                       |                       | nt Mesh IDs: - Meshes |
|                       |                       |  will have their tele |
|                       |                       | metry aggregated in o |
|                       |                       | ne place - Meshes wil |
|                       |                       | l be federated togeth |
|                       |                       | er - Policy will be w |
|                       |                       | ritten referencing on |
|                       |                       | e mesh from the other |
|                       |                       |  If an administrator  |
|                       |                       | expects that any of t |
|                       |                       | hese conditions may b |
|                       |                       | ecome true in the fut |
|                       |                       | ure, they should ensu |
|                       |                       | re their meshes have  |
|                       |                       | different Mesh IDs as |
|                       |                       | signed. Within a mult |
|                       |                       | icluster mesh, each c |
|                       |                       | luster must be (manua |
|                       |                       | lly or auto) configur |
|                       |                       | ed to have the same M |
|                       |                       | esh ID value. If an e |
|                       |                       | xisting cluster 'join |
|                       |                       | s' a multicluster mes |
|                       |                       | h, it will need to be |
|                       |                       |  migrated to the new  |
|                       |                       | mesh ID. Details of m |
|                       |                       | igration TBD, and it  |
|                       |                       | may be a disruptive o |
|                       |                       | peration to change th |
|                       |                       | e Mesh ID post-instal |
|                       |                       | l. If the mesh admin  |
|                       |                       | does not specify a va |
|                       |                       | lue, Istio will use t |
|                       |                       | he value of the mesh' |
|                       |                       | s Trust Domain. The b |
|                       |                       | est practice is to se |
|                       |                       | lect a proper Trust D |
|                       |                       | omain value.``        |
+-----------------------+-----------------------+-----------------------+
| ``global.outboundTraf | ``ALLOW_ANY``         |                       |
| ficPolicy.mode``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.enabled` | ``false``             | ``SDS enabled. IF set |
| `                     |                       |  to true, mTLS certif |
|                       |                       | icates for the sideca |
|                       |                       | rs will be distribute |
|                       |                       | d through the SecretD |
|                       |                       | iscoveryService inste |
|                       |                       | ad of using K8S secre |
|                       |                       | ts to mount the certi |
|                       |                       | ficates.``            |
+-----------------------+-----------------------+-----------------------+
| ``global.sds.udsPath` | ``""``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.meshNetworks | ``{}``                |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.localityLbSe | ``true``              |                       |
| tting.enabled``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``global.enableHelmTe | ``false``             | ``Specifies whether h |
| st``                  |                       | elm test is enabled o |
|                       |                       | r not. This field is  |
|                       |                       | set to false by defau |
|                       |                       | lt, so 'helm template |
|                       |                       |  ...' will ignore the |
|                       |                       |  helm test yaml files |
|                       |                       |  when generating the  |
|                       |                       | template``            |
+-----------------------+-----------------------+-----------------------+

``grafana`` options
-------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``grafana.enabled``   | ``false``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.replicaCoun | ``1``                 |                       |
| t``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.image.repos | ``grafana/grafana``   |                       |
| itory``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.image.tag`` | ``6.1.6``             |                       |
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
| ``grafana.security.en | ``false``             |                       |
| abled``               |                       |                       |
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
| ``grafana.tolerations | ``[]``                |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.env``       | ``{}``                |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.envSecrets` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.podAntiAffi | ``[]``                |                       |
| nityLabelSelector``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.podAntiAffi | ``[]``                |                       |
| nityTermLabelSelector |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.contextPath | ``/grafana``          |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.service.ann | ``{}``                |                       |
| otations``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.service.nam | ``http``              |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.service.typ | ``ClusterIP``         |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.service.ext | ``3000``              |                       |
| ernalPort``           |                       |                       |
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
| ces.type.orgId``      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``http://prometheus:9 |                       |
| .datasources.datasour | 090``                 |                       |
| ces.type.url``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``proxy``             |                       |
| .datasources.datasour |                       |                       |
| ces.type.access``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``true``              |                       |
| .datasources.datasour |                       |                       |
| ces.type.isDefault``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``5s``                |                       |
| .datasources.datasour |                       |                       |
| ces.type.jsonData.tim |                       |                       |
| eInterval``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.datasources | ``true``              |                       |
| .datasources.datasour |                       |                       |
| ces.type.editable``   |                       |                       |
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
| iders.providers.orgId |                       |                       |
| .folder``             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``file``              |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.orgId |                       |                       |
| .type``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``false``             |                       |
| oviders.dashboardprov |                       |                       |
| iders.providers.orgId |                       |                       |
| .disableDeletion``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``grafana.dashboardPr | ``/var/lib/grafana/da |                       |
| oviders.dashboardprov | shboards/istio``      |                       |
| iders.providers.orgId |                       |                       |
| .options.path``       |                       |                       |
+-----------------------+-----------------------+-----------------------+

``cni`` options
---------------

=============== ============= ===========
Key             Default Value Description
=============== ============= ===========
``cni.enabled`` ``false``
=============== ============= ===========

``istiocoredns`` options
------------------------

================================================= ====================================== ===========
Key                                               Default Value                          Description
================================================= ====================================== ===========
``istiocoredns.enabled``                          ``false``
``istiocoredns.replicaCount``                     ``1``
``istiocoredns.rollingMaxSurge``                  ``100%``
``istiocoredns.rollingMaxUnavailable``            ``25%``
``istiocoredns.coreDNSImage``                     ``coredns/coredns:1.1.2``
``istiocoredns.coreDNSPluginImage``               ``istio/coredns-plugin:0.2-istio-1.1``
``istiocoredns.nodeSelector``                     ``{}``
``istiocoredns.tolerations``                      ``[]``
``istiocoredns.podAntiAffinityLabelSelector``     ``[]``
``istiocoredns.podAntiAffinityTermLabelSelector`` ``[]``
================================================= ====================================== ===========

``kiali`` options
-----------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``kiali.enabled``     | ``false``             | ``Note that if using  |
|                       |                       | the demo yaml when in |
|                       |                       | stalling via Helm, th |
|                       |                       | is default will be tr |
|                       |                       | ue.``                 |
+-----------------------+-----------------------+-----------------------+
| ``kiali.replicaCount` | ``1``                 |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.hub``         | ``quay.io/kiali``     |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.image``       | ``kiali``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.tag``         | ``v1.1.0``            |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.contextPath`` | ``/kiali``            | ``The root context pa |
|                       |                       | th to access the Kial |
|                       |                       | i UI.``               |
+-----------------------+-----------------------+-----------------------+
| ``kiali.nodeSelector` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.tolerations`` | ``[]``                |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.podAntiAffini | ``[]``                |                       |
| tyLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.podAntiAffini | ``[]``                |                       |
| tyTermLabelSelector`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.ingress.enabl | ``false``             |                       |
| ed``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.ingress.hosts | ``kiali.local``       | ``Used to create an I |
| ``                    |                       | ngress record.``      |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.aut | ``login``             | ``Can be anonymous, l |
| h.strategy``          |                       | ogin, or openshift``  |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.sec | ``kiali``             | ``You must create a s |
| retName``             |                       | ecret with this name  |
|                       |                       | - one is not provided |
|                       |                       |  out-of-box.``        |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.vie | ``false``             | ``Bind the service ac |
| wOnlyMode``           |                       | count to a role with  |
|                       |                       | only read access``    |
+-----------------------+-----------------------+-----------------------+
| ``kiali.dashboard.gra | :literal:`\| `If you  | ``If you have Jaeger  |
| fanaURL``             | have Grafana installe | installed and it is a |
|                       | d and it is accessibl | ccessible to client b |
|                       | e to client browsers, | rowsers, then set thi |
|                       |  then set this to its | s property to its ext |
|                       |  external URL. Kiali  | ernal URL. Kiali will |
|                       | will redirect users t |  redirect users to th |
|                       | o this URL when Grafa | is URL when Jaeger tr |
|                       | na metrics are to be  | acing is to be shown. |
|                       | shown.` | | `kiali.da | ``                    |
|                       | shboard.jaegerURL` \| |                       |
|                       | `                     |                       |
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
| ``kiali.security.enab | ``true``              |                       |
| led``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.security.cert | ``/kiali-cert/cert-ch |                       |
| _file``               | ain.pem``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``kiali.security.priv | ``/kiali-cert/key.pem |                       |
| ate_key_file``        | ``                    |                       |
+-----------------------+-----------------------+-----------------------+

``mixer`` options
-----------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``mixer.image``       | ``mixer``             |                       |
+-----------------------+-----------------------+-----------------------+
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
| ``mixer.policy.rollin | ``100%``              |                       |
| gMaxSurge``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.policy.rollin | ``25%``               |                       |
| gMaxUnavailable``     |                       |                       |
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
| ``mixer.telemetry.rol | ``100%``              |                       |
| lingMaxSurge``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.rol | ``25%``               |                       |
| lingMaxUnavailable``  |                       |                       |
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
|                       |                       | pu allocation. We hav |
|                       |                       | e experimentally foun |
|                       |                       | d that these values w |
|                       |                       | ork well.``           |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.res | ``4G``                |                       |
| ources.limits.memory` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.rep | ``100``               | ``Set reportBatchMaxE |
| ortBatchMaxEntries``  |                       | ntries to 0 to use th |
|                       |                       | e default batching be |
|                       |                       | havior (i.e., every 1 |
|                       |                       | 00 requests). A posit |
|                       |                       | ive value indicates t |
|                       |                       | he number of requests |
|                       |                       |  that are batched bef |
|                       |                       | ore telemetry data is |
|                       |                       |  sent to the mixer se |
|                       |                       | rver``                |
+-----------------------+-----------------------+-----------------------+
| ``mixer.telemetry.rep | ``1s``                | ``Set reportBatchMaxT |
| ortBatchMaxTime``     |                       | ime to 0 to use the d |
|                       |                       | efault batching behav |
|                       |                       | ior (i.e., every 1 se |
|                       |                       | cond). A positive tim |
|                       |                       | e value indicates the |
|                       |                       |  maximum wait time si |
|                       |                       | nce the last request  |
|                       |                       | will telemetry data b |
|                       |                       | e batched before bein |
|                       |                       | g sent to the mixer s |
|                       |                       | erver``               |
+-----------------------+-----------------------+-----------------------+
| ``mixer.podAnnotation | ``{}``                |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.nodeSelector` | ``{}``                |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.tolerations`` | ``[]``                |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.podAntiAffini | ``[]``                |                       |
| tyLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``mixer.podAntiAffini | ``[]``                |                       |
| tyTermLabelSelector`` |                       |                       |
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
| ``mixer.adapters.useA | ``false``             | ``Setting this to fal |
| dapterCRDs``          |                       | se sets the useAdapte |
|                       |                       | rCRDs mixer startup a |
|                       |                       | rgument to false``    |
+-----------------------+-----------------------+-----------------------+

``nodeagent`` options
---------------------

============================================== ================== ===============================================
Key                                            Default Value      Description
============================================== ================== ===============================================
``nodeagent.enabled``                          ``false``
``nodeagent.image``                            ``node-agent-k8s``
``nodeagent.env.CA_PROVIDER``                  ``""``             ``name of authentication provider.``
``nodeagent.env.CA_ADDR``                      ``""``             ``CA endpoint.``
``nodeagent.env.Plugins``                      ``""``             ``names of authentication provider's plugins.``
``nodeagent.nodeSelector``                     ``{}``
``nodeagent.tolerations``                      ``[]``
``nodeagent.podAntiAffinityLabelSelector``     ``[]``
``nodeagent.podAntiAffinityTermLabelSelector`` ``[]``
============================================== ================== ===============================================

``pilot`` options
-----------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``pilot.enabled``     | ``true``              |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.autoscaleEnab | ``true``              |                       |
| led``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.autoscaleMin` | ``1``                 |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.autoscaleMax` | ``5``                 |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.rollingMaxSur | ``100%``              |                       |
| ge``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.rollingMaxUna | ``25%``               |                       |
| vailable``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.image``       | ``pilot``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.sidecar``     | ``true``              |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.traceSampling | ``1.0``               |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.enableProtoco | ``false``             | ``if protocol sniffin |
| lSniffing``           |                       | g is enabled. Default |
|                       |                       |  to false.``          |
+-----------------------+-----------------------+-----------------------+
| ``pilot.resources.req | ``500m``              |                       |
| uests.cpu``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.resources.req | ``2048Mi``            |                       |
| uests.memory``        |                       |                       |
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
| ``pilot.tolerations`` | ``[]``                |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.podAntiAffini | ``[]``                |                       |
| tyLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.podAntiAffini | ``[]``                |                       |
| tyTermLabelSelector`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``pilot.keepaliveMaxS | ``30m``               | ``The following is us |
| erverConnectionAge``  |                       | ed to limit how long  |
|                       |                       | a sidecar can be conn |
|                       |                       | ected to a pilot. It  |
|                       |                       | balances out load acr |
|                       |                       | oss pilot instances a |
|                       |                       | t the cost of increas |
|                       |                       | ing system churn.``   |
+-----------------------+-----------------------+-----------------------+

``prometheus`` options
----------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``prometheus.enabled` | ``true``              |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.replicaC | ``1``                 |                       |
| ount``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.hub``    | ``docker.io/prom``    |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.image``  | ``prometheus``        |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.tag``    | ``v2.8.0``            |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.retentio | ``6h``                |                       |
| n``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.nodeSele | ``{}``                |                       |
| ctor``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.tolerati | ``[]``                |                       |
| ons``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.podAntiA | ``[]``                |                       |
| ffinityLabelSelector` |                       |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.podAntiA | ``[]``                |                       |
| ffinityTermLabelSelec |                       |                       |
| tor``                 |                       |                       |
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
| ``prometheus.service. | ``{}``                |                       |
| annotations``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.service. | ``false``             |                       |
| nodePort.enabled``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.service. | ``32090``             |                       |
| nodePort.port``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``prometheus.security | ``true``              |                       |
| .enabled``            |                       |                       |
+-----------------------+-----------------------+-----------------------+

``security`` options
--------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``security.enabled``  | ``true``              |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.replicaCou | ``1``                 |                       |
| nt``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.rollingMax | ``100%``              |                       |
| Surge``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.rollingMax | ``25%``               |                       |
| Unavailable``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.enableName | ``true``              | ``determines whether  |
| spacesByDefault``     |                       | namespaces without th |
|                       |                       | e ca.istio.io/env and |
|                       |                       |  ca.istio.io/override |
|                       |                       |  labels should be tar |
|                       |                       | geted by the Citadel  |
|                       |                       | instance for secret c |
|                       |                       | reation``             |
+-----------------------+-----------------------+-----------------------+
| ``security.image``    | ``citadel``           |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.selfSigned | ``true``              | ``indicate if self-si |
| ``                    |                       | gned CA is used.``    |
+-----------------------+-----------------------+-----------------------+
| ``security.createMesh | ``true``              |                       |
| Policy``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.nodeSelect | ``{}``                |                       |
| or``                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.toleration | ``[]``                |                       |
| s``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.citadelHea | ``false``             |                       |
| lthCheck``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.workloadCe | ``2160h``             | ``90*24hour = 2160h`` |
| rtTtl``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.enableName | ``true``              | ``Determines Citadel  |
| spacesByDefault``     |                       | default behavior if t |
|                       |                       | he ca.istio.io/env or |
|                       |                       |  ca.istio.io/override |
|                       |                       |  labels are not found |
|                       |                       |  on a given namespace |
|                       |                       | . For example: consid |
|                       |                       | er a namespace called |
|                       |                       |  "target", which has  |
|                       |                       | neither the "ca.istio |
|                       |                       | .io/env" nor the "ca. |
|                       |                       | istio.io/override" na |
|                       |                       | mespace labels. To de |
|                       |                       | cide whether or not t |
|                       |                       | o generate secrets fo |
|                       |                       | r service accounts cr |
|                       |                       | eated in this "target |
|                       |                       | " namespace, Citadel  |
|                       |                       | will defer to this op |
|                       |                       | tion. If the value of |
|                       |                       |  this option is "true |
|                       |                       | " in this case, secre |
|                       |                       | ts will be generated  |
|                       |                       | for the "target" name |
|                       |                       | space. If the value o |
|                       |                       | f this option is "fal |
|                       |                       | se" Citadel will not  |
|                       |                       | generate secrets upon |
|                       |                       |  service account crea |
|                       |                       | tion.``               |
+-----------------------+-----------------------+-----------------------+
| ``security.podAntiAff | ``[]``                |                       |
| inityLabelSelector``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``security.podAntiAff | ``[]``                |                       |
| inityTermLabelSelecto |                       |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+

``sidecarInjectorWebhook`` options
----------------------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``sidecarInjectorWebh | ``true``              |                       |
| ook.enabled``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``1``                 |                       |
| ook.replicaCount``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``100%``              |                       |
| ook.rollingMaxSurge`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``25%``               |                       |
| ook.rollingMaxUnavail |                       |                       |
| able``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``sidecar_injector``  |                       |
| ook.image``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``false``             |                       |
| ook.enableNamespacesB |                       |                       |
| yDefault``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``{}``                |                       |
| ook.nodeSelector``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.tolerations``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.podAntiAffinityLa |                       |                       |
| belSelector``         |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``[]``                |                       |
| ook.podAntiAffinityTe |                       |                       |
| rmLabelSelector``     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``sidecarInjectorWebh | ``false``             | ``If true, webhook or |
| ook.rewriteAppHTTPPro |                       |  istioctl injector wi |
| be``                  |                       | ll rewrite PodSpec fo |
|                       |                       | r liveness health che |
|                       |                       | ck to redirect reques |
|                       |                       | t to sidecar. This ma |
|                       |                       | kes liveness check wo |
|                       |                       | rk even when mTLS is  |
|                       |                       | enabled.``            |
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

``tracing`` options
-------------------

+-----------------------+-----------------------+-----------------------+
| Key                   | Default Value         | Description           |
+=======================+=======================+=======================+
| ``tracing.enabled``   | ``false``             |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.provider``  | ``jaeger``            |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.nodeSelecto | ``{}``                |                       |
| r``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.tolerations | ``[]``                |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.podAntiAffi | ``[]``                |                       |
| nityLabelSelector``   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.podAntiAffi | ``[]``                |                       |
| nityTermLabelSelector |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.hub` | ``docker.io/jaegertra |                       |
| `                     | cing``                |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.imag | ``all-in-one``        |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.tag` | ``1.12``              |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.memo | ``50000``             |                       |
| ry.max_traces``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.span | ``badger``            | ``spanStorageType val |
| StorageType``         |                       | ue can be "memory" an |
|                       |                       | d "badger" for all-in |
|                       |                       | -one image``          |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.pers | ``false``             |                       |
| ist``                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.stor | ``""``                |                       |
| ageClassName``        |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.jaeger.acce | ``ReadWriteMany``     |                       |
| ssMode``              |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.hub` | ``docker.io/openzipki |                       |
| `                     | n``                   |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.imag | ``zipkin``            |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.tag` | ``2.14.2``            |                       |
| `                     |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.prob | ``200``               |                       |
| eStartupDelay``       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.quer | ``9411``              |                       |
| yPort``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.reso | ``300m``              |                       |
| urces.limits.cpu``    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.reso | ``900Mi``             |                       |
| urces.limits.memory`` |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.reso | ``150m``              |                       |
| urces.requests.cpu``  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.reso | ``900Mi``             |                       |
| urces.requests.memory |                       |                       |
| ``                    |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.java | ``700``               |                       |
| OptsHeap``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.maxS | ``500000``            |                       |
| pans``                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.zipkin.node | ``2``                 |                       |
| .cpus``               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.service.ann | ``{}``                |                       |
| otations``            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.service.nam | ``http``              |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.service.typ | ``ClusterIP``         |                       |
| e``                   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.service.ext | ``9411``              |                       |
| ernalPort``           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ``tracing.ingress.ena | ``false``             |                       |
| bled``                |                       |                       |
+-----------------------+-----------------------+-----------------------+

.. raw:: html

   <!-- AUTO-GENERATED-END -->
