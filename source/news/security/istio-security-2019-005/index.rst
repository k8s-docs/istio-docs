istio-security-2019-005
==========================

{{< security_bulletin >}}

Envoy, and subsequently Istio, are vulnerable to the following DoS
attack. Upon receiving each incoming request, Envoy will iterate over
the request headers to verify that the total size of the headers stays
below a maximum limit. A remote attacker may craft a request that stays
below the maximum request header size but consists of many thousands of
small headers to consume CPU and result in a denial-of-service attack.

Impact and detection
--------------------

Both Istio gateways and sidecars are vulnerable to this issue. If you
are running one of the affected releases, your cluster is vulnerable.

Mitigation
----------

-  For Istio 1.1.x deployments: update all control plane components
   (Pilot, Mixer, Citadel, and Galley) and then `upgrade the data
   plane </docs/setup/upgrade/cni-helm-upgrade/#sidecar-upgrade>`_ to
   `Istio 1.1.16 </news/releases/1.1.x/announcing-1.1.16>`_ or later.
-  For Istio 1.2.x deployments: update all control plane components
   (Pilot, Mixer, Citadel, and Galley) and then `upgrade the data
   plane </docs/setup/upgrade/cni-helm-upgrade/#sidecar-upgrade>`_ to
   `Istio 1.2.7 </news/releases/1.2.x/announcing-1.2.7>`_ or later.
-  For Istio 1.3.x deployments: update all control plane components
   (Pilot, Mixer, Citadel, and Galley) and then `upgrade the data
   plane </docs/setup/upgrade/cni-helm-upgrade/#sidecar-upgrade>`_ to
   `Istio 1.3.2 </news/releases/1.3.x/announcing-1.3.2>`_ or later.

{{< boilerplate “security-vulnerability” >}}
