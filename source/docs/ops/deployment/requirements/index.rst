requirements
=============================

To be part of a mesh, Kubernetes pods and services must satisfy the
following requirements:

-  **Service association**: A pod must belong to at least one Kubernetes
   service even if the pod does NOT expose any port. If a pod belongs to
   multiple `Kubernetes
   services <https://kubernetes.io/docs/concepts/services-networking/service/>`_,
   the services cannot use the same port number for different protocols,
   for instance HTTP and TCP.

-  **Application UIDs**: Ensure your pods do **not** run applications as
   a user with the user ID (UID) value of **1337**.

-  **``NET_ADMIN`` and ``NET_RAW`` capabilities**: If your cluster
   enforces pod security policies, they must allow injected pods to add
   the ``NET_ADMIN`` and ``NET_RAW`` capabilities. If you use the `Istio
   CNI Plugin </docs/setup/additional-setup/cni/>`_, this requirement
   no longer applies. To learn more about the ``NET_ADMIN`` and
   ``NET_RAW`` capabilities, see `Required pod
   capabilities <#required-pod-capabilities>`_, below.

-  **Deployments with app and version labels**: We recommend adding an
   explicit ``app`` label and ``version`` label to deployments. Add the
   labels to the deployment specification of pods deployed using the
   Kubernetes ``Deployment``. The ``app`` and ``version`` labels add
   contextual information to the metrics and telemetry Istio collects.

   -  The ``app`` label: Each deployment specification should have a
      distinct ``app`` label with a meaningful value. The ``app`` label
      is used to add contextual information in distributed tracing.

   -  The ``version`` label: This label indicates the version of the
      application corresponding to the particular deployment.

-  **Named service ports**: Service ports may optionally be named to
   explicitly specify a protocol. See `Protocol
   Selection </docs/ops/configuration/traffic-management/protocol-selection/>`_
   for more details.

Ports used by Istio
-------------------

The following ports and protocols are used by Istio.

+-----------------+-----------------+-----------------+-----------------+
| Port            | Protocol        | Used by         | Description     |
+=================+=================+=================+=================+
| 15000           | TCP             | Envoy           | Envoy admin     |
|                 |                 |                 | port            |
|                 |                 |                 | (commands/diagn |
|                 |                 |                 | ostics)         |
+-----------------+-----------------+-----------------+-----------------+
| 15001           | TCP             | Envoy           | Envoy Outbound  |
+-----------------+-----------------+-----------------+-----------------+
| 15006           | TCP             | Envoy           | Envoy Inbound   |
+-----------------+-----------------+-----------------+-----------------+
| 15020           | HTTP            | Envoy           | Health checks   |
+-----------------+-----------------+-----------------+-----------------+
| 15090           | HTTP            | Envoy           | Prometheus      |
|                 |                 |                 | telemetry       |
+-----------------+-----------------+-----------------+-----------------+
| 15010           | GRPC            | Istiod          | XDS and CA      |
|                 |                 |                 | services        |
|                 |                 |                 | (plaintext)     |
+-----------------+-----------------+-----------------+-----------------+
| 15011           | GRPC            | Istiod          | XDS and CA      |
|                 |                 |                 | services (TLS,  |
|                 |                 |                 | legacy)         |
+-----------------+-----------------+-----------------+-----------------+
| 15012           | GRPC            | Istiod          | XDS and CA      |
|                 |                 |                 | services (TLS)  |
+-----------------+-----------------+-----------------+-----------------+
| 8080            | HTTP            | Istiod          | Debug interface |
+-----------------+-----------------+-----------------+-----------------+
| 443             | HTTPS           | Istiod          | Webhooks        |
+-----------------+-----------------+-----------------+-----------------+
| 15014           | HTTP            | Citadel,        | Control plane   |
|                 |                 | Citadel agent,  | monitoring      |
|                 |                 | Galley, Mixer,  |                 |
|                 |                 | Istiod, Sidecar |                 |
|                 |                 | Injector        |                 |
+-----------------+-----------------+-----------------+-----------------+
| 15029           | HTTP            | Kiali           | Kiali User      |
|                 |                 |                 | Interface       |
+-----------------+-----------------+-----------------+-----------------+
| 15030           | HTTP            | Prometheus      | Prometheus User |
|                 |                 |                 | Interface       |
+-----------------+-----------------+-----------------+-----------------+
| 15031           | HTTP            | Grafana         | Grafana User    |
|                 |                 |                 | Interface       |
+-----------------+-----------------+-----------------+-----------------+
| 15032           | HTTP            | Tracing         | Tracing User    |
|                 |                 |                 | Interface       |
+-----------------+-----------------+-----------------+-----------------+
| 15443           | TLS             | Ingress and     | SNI             |
|                 |                 | Egress Gateways |                 |
+-----------------+-----------------+-----------------+-----------------+
| 9090            | HTTP            | Prometheus      | Prometheus      |
+-----------------+-----------------+-----------------+-----------------+
| 42422           | TCP             | Mixer           | Telemetry -     |
|                 |                 |                 | Prometheus      |
+-----------------+-----------------+-----------------+-----------------+
| 15004           | HTTP            | Mixer, Pilot    | Policy/Telemetr |
|                 |                 |                 | y               |
|                 |                 |                 | - ``mTLS``      |
+-----------------+-----------------+-----------------+-----------------+
| 9091            | HTTP            | Mixer           | Policy/Telemetr |
|                 |                 |                 | y               |
+-----------------+-----------------+-----------------+-----------------+
| 8060            | HTTP            | Citadel         | GRPC server     |
+-----------------+-----------------+-----------------+-----------------+
| 9876            | HTTP            | Citadel,        | ControlZ user   |
|                 |                 | Citadel agent   | interface       |
+-----------------+-----------------+-----------------+-----------------+
| 9901            | GRPC            | Galley          | Mesh            |
|                 |                 |                 | Configuration   |
|                 |                 |                 | Protocol        |
+-----------------+-----------------+-----------------+-----------------+

Required pod capabilities
-------------------------

If `pod security
policies <https://kubernetes.io/docs/concepts/policy/pod-security-policy/>`_
are
`enforced <https://kubernetes.io/docs/concepts/policy/pod-security-policy/#enabling-pod-security-policies>`_
in your cluster and unless you use the Istio CNI Plugin, your pods must
have the ``NET_ADMIN`` and ``NET_RAW`` capabilities allowed. The
initialization containers of the Envoy proxies require these
capabilities.

To check if the ``NET_ADMIN`` and ``NET_RAW`` capabilities are allowed
for your pods, you need to check if their `service
account <https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/>`_
can use a pod security policy that allows the ``NET_ADMIN`` and
``NET_RAW`` capabilities. If you haven’t specified a service account in
your pods’ deployment, the pods run using the ``default`` service
account in their deployment’s namespace.

To list the capabilities for a service account, replace
``<your namespace>`` and ``<your service account>`` with your values in
the following command:

.. code:: sh

      $ for psp in $(kubectl get psp -o jsonpath=“{range
.items[*]}{@.metadata.name}{‘:raw-latex:`\n`’}{end}”); do if [
:math:`(kubectl auth can-i use psp/`\ psp –as=system:serviceaccount::) =
yes ]; then kubectl get psp/$psp –no-headers
-o=custom-columns=NAME:.metadata.name,CAPS:.spec.allowedCapabilities;
fi; done

For example, to check for the ``default`` service account in the
``default`` namespace, run the following command:

.. code:: sh

      $ for psp in $(kubectl get psp -o jsonpath=“{range
.items[*]}{@.metadata.name}{‘:raw-latex:`\n`’}{end}”); do if [
:math:`(kubectl auth can-i use psp/`\ psp
–as=system:serviceaccount:default:default) = yes ]; then kubectl get
psp/$psp –no-headers
-o=custom-columns=NAME:.metadata.name,CAPS:.spec.allowedCapabilities;
fi; done

If you see ``NET_ADMIN`` and ``NET_ADMIN`` or ``*`` in the list of
capabilities of one of the allowed policies for your service account,
your pods have permission to run the Istio init containers. Otherwise,
you will need to `provide the
permission <https://kubernetes.io/docs/concepts/policy/pod-security-policy/#authorizing-policies>`_.
