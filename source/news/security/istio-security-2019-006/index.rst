istio-security-2019-006
===========================

{{< security_bulletin >}}

Envoy, and subsequently Istio, are vulnerable to the following DoS
attack. An infinite loop can be triggered in Envoy if the option
``continue_on_listener_filters_timeout`` is set to ``True``. This has
been the case for Istio since the introduction of the Protocol Detection
feature in Istio 1.3 A remote attacker may trivially trigger that
vulnerability, effectively exhausting Envoy’s CPU resources and causing
a denial-of-service attack.

Impact and detection
--------------------

Both Istio gateways and sidecars are vulnerable to this issue. If you
are running one of the affected releases, your cluster is vulnerable.

Mitigation
----------

-  Workaround: The exploitation of that vulnerability can be prevented
   by customizing Istio installation (as described in `installation
   options </docs/reference/config/installation-options/#pilot-options>`_
   ), using Helm to override the following options:

   {{< text plain >}} –set
   pilot.env.PILOT_INBOUND_PROTOCOL_DETECTION_TIMEOUT=0s –set
   global.proxy.protocolDetectionTimeout=0s

-  For Istio 1.3.x deployments: update to `Istio
   1.3.5 </news/releases/1.3.x/announcing-1.3.5>`_ or later.

{{< boilerplate “security-vulnerability” >}}
