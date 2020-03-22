shared-gateways
============================

Follow this guide to configure a multicluster mesh using a shared
`control
plane </docs/ops/deployment/deployment-models/#control-plane-models>`_
with gateways to connect network-isolated clusters. Istio’s
location-aware service routing feature is used to route requests to
different endpoints, depending on the location of the request source.

By following the instructions in this guide, you will setup a
two-cluster mesh as shown in the following diagram:

.. image::./diagram.svg
   :alt:
   :caption:Shared Istio control plane topology spanning multiple Kubernetes clusters using gateways
   :width: 80%

The primary cluster, ``cluster1``, runs the full set of Istio control
plane components while ``cluster2`` only runs Istio Citadel, Sidecar
Injector, and Ingress gateway. No VPN connectivity nor direct network
access between workloads in different clusters is required.

Prerequisites
-------------

-  Two or more Kubernetes clusters with versions: {{<
   supported_kubernetes_versions >}}.

-  Authority to `deploy the Istio control
   plane </docs/setup/install/istioctl/>`_

-  Two Kubernetes clusters (referred to as ``cluster1`` and
   ``cluster2``).

   .. warning::

   The Kubernetes API server of ``cluster2`` MUST be
   accessible from ``cluster1`` in order to run this configuration. {{<
   /warning >}}

{{< boilerplate kubectl-multicluster-contexts >}}

Setup the multicluster mesh
---------------------------

In this configuration you install Istio with mutual TLS enabled for both
the control plane and application pods. For the shared root CA, you
create a ``cacerts`` secret on both ``cluster1`` and ``cluster2``
clusters using the same Istio certificate from the Istio samples
directory.

The instructions, below, also set up ``cluster2`` with a selector-less
service and an endpoint for ``istio-pilot.istio-system`` that has the
address of ``cluster1`` Istio ingress gateway. This will be used to
access pilot on ``cluster1`` securely using the ingress gateway without
mutual TLS termination.

Setup cluster 1 (primary)
~~~~~~~~~~~~~~~~~~~~~~~~~

1. Deploy Istio to ``cluster1``:

   .. warning::

   When you enable the additional components necessary
   for multicluster operation, the resource footprint of the Istio
   control plane may increase beyond the capacity of the default
   Kubernetes cluster you created when completing the `Platform
   setup </docs/setup/platform-setup/>`_ steps. If the Istio services
   aren’t getting scheduled due to insufficient CPU or memory, consider
   adding more nodes to your cluster or upgrading to larger memory
   instances as necessary.

   | .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER1 ns
     istio-system $ kubectl create –context=$CTX_CLUSTER1 secret generic
     cacerts -n istio-system –from-file=samples/certs/ca-cert.pem
     –from-file=samples/certs/ca-key.pem
     –from-file=samples/certs/root-cert.pem
     –from-file=samples/certs/cert-chain.pem $ istioctl manifest apply
     –context=$CTX_CLUSTER1
   | -f
     install/kubernetes/operator/examples/multicluster/values-istio-multicluster-primary.yaml


   .. warning::

   Note that the gateway addresses are set to
   ``0.0.0.0``. These are temporary placeholder values that will later
   be updated with the public IPs of the ``cluster1`` and ``cluster2``
   gateways after they are deployed in the following section. {{<
   /warning >}}

   Wait for the Istio pods on ``cluster1`` to become ready:

   .. code:: sh

      $ kubectl get pods –context=$CTX_CLUSTER1 -n
   istio-system NAME READY STATUS RESTARTS AGE
   istio-citadel-55d8b59798-6hnx4 1/1 Running 0 83s
   istio-galley-c74b77787-lrtr5 2/2 Running 0 82s
   istio-ingressgateway-684f5df677-shzhm 1/1 Running 0 83s
   istio-pilot-5495bc8885-2rgmf 2/2 Running 0 82s
   istio-policy-69cdf5db4c-x4sct 2/2 Running 2 83s
   istio-sidecar-injector-5749cf7cfc-pgd95 1/1 Running 0 82s
   istio-telemetry-646db5ddbd-gvp6l 2/2 Running 1 83s
   prometheus-685585888b-4tvf7 1/1 Running 0 83s

2. Create an ingress gateway to access service(s) in ``cluster2``:

   .. code:: sh

      $ kubectl apply –context=$CTX_CLUSTER1 -f - <<EOF
   apiVersion: networking.istio.io/v1alpha3 kind: Gateway metadata:
   name: cluster-aware-gateway namespace: istio-system spec: selector:
   istio: ingressgateway servers:

   -  port: number: 443 name: tls protocol: TLS tls: mode:
      AUTO_PASSTHROUGH hosts:

      -  "*.local" EOF

   This ``Gateway`` configures 443 port to pass incoming traffic through
   to the target service specified in a request’s SNI header, for SNI
   values of the *local* top-level domain (i.e., the `Kubernetes DNS
   domain <https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/>`_).
   Mutual TLS connections will be used all the way from the source to
   the destination sidecar.

   Although applied to ``cluster1``, this Gateway instance will also
   affect ``cluster2`` because both clusters communicate with the same
   Pilot.

3. Determine the ingress IP and port for ``cluster1``.

   1. Set the current context of ``kubectl`` to ``CTX_CLUSTER1``

   ::

      .. code:: sh


      $ export ORIGINAL_CONTEXT=$(kubectl config current-context)
      $ kubectl config use-context $CTX_CLUSTER1


   1. Follow the instructions in `Determining the ingress IP and
      ports </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_,
      to set the ``INGRESS_HOST`` and ``SECURE_INGRESS_PORT``
      environment variables.

   2. Restore the previous ``kubectl`` context:

      .. code:: sh

      $ kubectl config use-context $ORIGINAL_CONTEXT $
      unset ORIGINAL_CONTEXT

   3. Print the values of ``INGRESS_HOST`` and ``SECURE_INGRESS_PORT``:

      .. code:: sh

      $ echo The ingress gateway of cluster1:
      address=\ :math:`INGRESS_HOST, port=`\ SECURE_INGRESS_PORT

4. Update the gateway address in the mesh network configuration. Edit
   the ``istio ConfigMap``:

   .. code:: sh

      $ kubectl edit cm -n istio-system
   –context=$CTX_CLUSTER1 istio

   Update the gateway’s address and port of ``network1`` to reflect the
   ``cluster1`` ingress host and port, respectively, then save and quit.
   Note that the address appears in two places, the second under
   ``values.yaml:``.

   Once saved, Pilot will automatically read the updated network
   configuration.

Setup cluster 2
~~~~~~~~~~~~~~~

1. Export the ``cluster1`` gateway address:

   | .. code:: sh

      $ export
     LOCAL_GW_ADDR=\ :math:`(kubectl get --context=`\ CTX_CLUSTER1 svc
     –selector=app=istio-ingressgateway
   | -n istio-system -o
     jsonpath=‘{.items[0].status.loadBalancer.ingress[0].ip}’) && echo
     ${LOCAL_GW_ADDR}

   This command sets the value to the gateway’s public IP and displays
   it.

   .. warning::

   The command fails if the load balancer configuration
   doesn’t include an IP address. The implementation of DNS name support
   is pending.

2. Deploy Istio to ``cluster2``:

   | .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER2 ns
     istio-system $ kubectl create –context=$CTX_CLUSTER2 secret generic
     cacerts -n istio-system –from-file=samples/certs/ca-cert.pem
     –from-file=samples/certs/ca-key.pem
     –from-file=samples/certs/root-cert.pem
     –from-file=samples/certs/cert-chain.pem $
     CLUSTER_NAME=\ :math:`(kubectl --context=`\ CTX_CLUSTER2 config
     view –minify=true -o jsonpath=‘{.clusters[].name}’) $ istioctl
     manifest apply –context=:math:`CTX_CLUSTER2 \
      --set profile=remote \
      --set values.gateways.enabled=true \
      --set values.security.selfSigned=false \
      --set values.global.createRemoteSvcEndpoints=true \
      --set values.global.remotePilotCreateSvcEndpoint=true \
      --set values.global.remotePilotAddress=`\ {LOCAL_GW_ADDR}
   | –set values.global.remotePolicyAddress=\ :math:`{LOCAL_GW_ADDR} \
      --set values.global.remoteTelemetryAddress=`\ {LOCAL_GW_ADDR}
   | –set
     values.gateways.istio-ingressgateway.env.ISTIO_META_NETWORK=“network2”
   | –set values.global.network=“network2”
   | –set values.global.multiCluster.clusterName=${CLUSTER_NAME}
   |

   Wait for the Istio pods on ``cluster2``, except for
   ``istio-ingressgateway``, to become ready:

   .. code:: sh

      $ kubectl get pods –context=$CTX_CLUSTER2 -n
   istio-system -l istio!=ingressgateway NAME READY STATUS RESTARTS AGE
   istio-citadel-55d8b59798-nlk2z 1/1 Running 0 26s
   istio-sidecar-injector-5749cf7cfc-s6r7p 1/1 Running 0 25s {{< /text
   >}}

   .. warning::

   ``istio-ingressgateway`` will not be ready until you
   configure the Istio control plane in ``cluster1`` to watch
   ``cluster2``. You do it in the next section.

3. Determine the ingress IP and port for ``cluster2``.

   1. Set the current context of ``kubectl`` to ``CTX_CLUSTER2``

   ::

      .. code:: sh


      $ export ORIGINAL_CONTEXT=$(kubectl config current-context)
      $ kubectl config use-context $CTX_CLUSTER2


   1. Follow the instructions in `Determining the ingress IP and
      ports </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_,
      to set the ``INGRESS_HOST`` and ``SECURE_INGRESS_PORT``
      environment variables.

   2. Restore the previous ``kubectl`` context:

      .. code:: sh

      $ kubectl config use-context $ORIGINAL_CONTEXT $
      unset ORIGINAL_CONTEXT

   3. Print the values of ``INGRESS_HOST`` and ``SECURE_INGRESS_PORT``:

      .. code:: sh

      $ echo The ingress gateway of cluster2:
      address=\ :math:`INGRESS_HOST, port=`\ SECURE_INGRESS_PORT

4. Update the gateway address in the mesh network configuration. Edit
   the ``istio ConfigMap``:

   .. code:: sh

      $ kubectl edit cm -n istio-system
   –context=$CTX_CLUSTER1 istio

   Update the gateway’s address and port of ``network2`` to reflect the
   ``cluster2`` ingress host and port, respectively, then save and quit.
   Note that the address appears in two places, the second under
   ``values.yaml:``.

   Once saved, Pilot will automatically read the updated network
   configuration.

5. Prepare environment variables for building the ``n2-k8s-config`` file
   for the service account ``istio-reader-service-account``:

   .. code:: sh

      $
   CLUSTER_NAME=\ :math:`(kubectl --context=`\ CTX_CLUSTER2 config view
   –minify=true -o jsonpath=‘{.clusters[].name}’) $
   SERVER=\ :math:`(kubectl --context=`\ CTX_CLUSTER2 config view
   –minify=true -o jsonpath=‘{.clusters[].cluster.server}’) $
   SECRET_NAME=\ :math:`(kubectl --context=`\ CTX_CLUSTER2 get sa
   istio-reader-service-account -n istio-system -o
   jsonpath=‘{.secrets[].name}’) $
   CA_DATA=\ :math:`(kubectl get --context=`\ CTX_CLUSTER2 secret
   ${SECRET_NAME} -n istio-system -o jsonpath=“{.data[‘ca.crt’]}”) $
   TOKEN=\ :math:`(kubectl get --context=`\ CTX_CLUSTER2 secret
   ${SECRET_NAME} -n istio-system -o jsonpath=“{.data[‘token’]}” \|
   base64 –decode)

   .. note::

   An alternative to ``base64 --decode`` is
   ``openssl enc -d -base64 -A`` on many systems.

6. Create the ``n2-k8s-config`` file in the working directory:

   .. code:: sh

      $ cat < n2-k8s-config apiVersion: v1 kind: Config
   clusters:

   -  cluster: certificate-authority-data: ${CA_DATA} server: ${SERVER}
      name: ${CLUSTER_NAME} contexts:
   -  context: cluster: ${CLUSTER_NAME} user: ${CLUSTER_NAME} name:
      ${CLUSTER_NAME} current-context: ${CLUSTER_NAME} users:
   -  name: ${CLUSTER_NAME} user: token: ${TOKEN} EOF

Start watching cluster 2
~~~~~~~~~~~~~~~~~~~~~~~~

1. Execute the following commands to add and label the secret of the
   ``cluster2`` Kubernetes. After executing these commands Istio Pilot
   on ``cluster1`` will begin watching ``cluster2`` for services and
   instances, just as it does for ``cluster1``.

   .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER1 secret
   generic n2-k8s-secret –from-file n2-k8s-config -n istio-system $
   kubectl label –context=$CTX_CLUSTER1 secret n2-k8s-secret
   istio/multiCluster=true -n istio-system

2. Wait for ``istio-ingressgateway`` to become ready:

   .. code:: sh

      $ kubectl get pods –context=$CTX_CLUSTER2 -n
   istio-system -l istio=ingressgateway NAME READY STATUS RESTARTS AGE
   istio-ingressgateway-5c667f4f84-bscff 1/1 Running 0 16m

Now that you have your ``cluster1`` and ``cluster2`` clusters set up,
you can deploy an example service.

Deploy example service
----------------------

As shown in the diagram, above, deploy two instances of the
``helloworld`` service, one on ``cluster1`` and one on ``cluster2``. The
difference between the two instances is the version of their
``helloworld`` image.

Deploy helloworld v2 in cluster 2
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create a ``sample`` namespace with a sidecar auto-injection label:

   .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER2 ns sample $
   kubectl label –context=$CTX_CLUSTER2 namespace sample
   istio-injection=enabled

2. Deploy ``helloworld v2``:

   .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER2 -f
   @samples/helloworld/helloworld.yaml@ -l app=helloworld -n sample $
   kubectl create –context=$CTX_CLUSTER2 -f
   @samples/helloworld/helloworld.yaml@ -l version=v2 -n sample

3. Confirm ``helloworld v2`` is running:

   .. code:: sh

      $ kubectl get po –context=$CTX_CLUSTER2 -n sample
   NAME READY STATUS RESTARTS AGE helloworld-v2-7dd57c44c4-f56gq 2/2
   Running 0 35s

Deploy helloworld v1 in cluster 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create a ``sample`` namespace with a sidecar auto-injection label:

   .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER1 ns sample $
   kubectl label –context=$CTX_CLUSTER1 namespace sample
   istio-injection=enabled

2. Deploy ``helloworld v1``:

   .. code:: sh

      $ kubectl create –context=$CTX_CLUSTER1 -f
   @samples/helloworld/helloworld.yaml@ -l app=helloworld -n sample $
   kubectl create –context=$CTX_CLUSTER1 -f
   @samples/helloworld/helloworld.yaml@ -l version=v1 -n sample

3. Confirm ``helloworld v1`` is running:

   .. code:: sh

      $ kubectl get po –context=$CTX_CLUSTER1 -n sample
   NAME READY STATUS RESTARTS AGE helloworld-v1-d4557d97b-pv2hr 2/2
   Running 0 40s

Cross-cluster routing in action
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To demonstrate how traffic to the ``helloworld`` service is distributed
across the two clusters, call the ``helloworld`` service from another
in-mesh ``sleep`` service.

1. Deploy the ``sleep`` service in both clusters:

   .. code:: sh

      $ kubectl apply –context=$CTX_CLUSTER1 -f
   @samples/sleep/sleep.yaml@ -n sample $ kubectl apply
   –context=$CTX_CLUSTER2 -f @samples/sleep/sleep.yaml@ -n sample

2. Wait for the ``sleep`` service to start in each cluster:

   .. code:: sh

      $ kubectl get po –context=$CTX_CLUSTER1 -n sample
   -l app=sleep sleep-754684654f-n6bzf 2/2 Running 0 5s

   .. code:: sh

      $ kubectl get po –context=$CTX_CLUSTER2 -n sample
   -l app=sleep sleep-754684654f-dzl9j 2/2 Running 0 5s

3. Call the ``helloworld.sample`` service several times from
   ``cluster1`` :

   .. code:: sh

      $ kubectl exec –context=$CTX_CLUSTER1 -it -n sample
   -c sleep :math:`(kubectl get pod --context=`\ CTX_CLUSTER1 -n sample
   -l app=sleep -o jsonpath=‘{.items[0].metadata.name}’) – curl
   helloworld.sample:5000/hello

4. Call the ``helloworld.sample`` service several times from
   ``cluster2`` :

   .. code:: sh

      $ kubectl exec –context=$CTX_CLUSTER2 -it -n sample
   -c sleep :math:`(kubectl get pod --context=`\ CTX_CLUSTER2 -n sample
   -l app=sleep -o jsonpath=‘{.items[0].metadata.name}’) – curl
   helloworld.sample:5000/hello

If set up correctly, the traffic to the ``helloworld.sample`` service
will be distributed between instances on ``cluster1`` and ``cluster2``
resulting in responses with either ``v1`` or ``v2`` in the body:

{{< text plain >}} Hello version: v2, instance:
helloworld-v2-758dd55874-6x4t8 Hello version: v1, instance:
helloworld-v1-86f77cd7bd-cpxhv

You can also verify the IP addresses used to access the endpoints by
printing the log of the sleep’s ``istio-proxy`` container.

.. code:: sh

      $ kubectl logs –context=$CTX_CLUSTER1 -n sample
:math:`(kubectl get pod --context=`\ CTX_CLUSTER1 -n sample -l app=sleep
-o jsonpath=‘{.items[0].metadata.name}’) istio-proxy
[2018-11-25T12:37:52.077Z] “GET /hello HTTP/1.1” 200 - 0 60 190 189 “-”
“curl/7.60.0” “6e096efe-f550-4dfa-8c8c-ba164baf4679”
“helloworld.sample:5000” “192.23.120.32:15443”
outbound|5000||helloworld.sample.svc.cluster.local - 10.20.194.146:5000
10.10.0.89:59496 - [2018-11-25T12:38:06.745Z] “GET /hello HTTP/1.1” 200
- 0 60 171 170 “-” “curl/7.60.0” “6f93c9cc-d32a-4878-b56a-086a740045d2”
“helloworld.sample:5000” “10.10.0.90:5000”
outbound|5000||helloworld.sample.svc.cluster.local - 10.20.194.146:5000
10.10.0.89:59646 -

In ``cluster1``, the gateway IP of ``cluster2``
(``192.23.120.32:15443``) is logged when v2 was called and the instance
IP in ``cluster1`` (``10.10.0.90:5000``) is logged when v1 was called.

.. code:: sh

      $ kubectl logs –context=$CTX_CLUSTER2 -n sample
:math:`(kubectl get pod --context=`\ CTX_CLUSTER2 -n sample -l app=sleep
-o jsonpath=‘{.items[0].metadata.name}’) istio-proxy
[2019-05-25T08:06:11.468Z] “GET /hello HTTP/1.1” 200 - “-” 0 60 177 176
“-” “curl/7.60.0” “58cfb92b-b217-4602-af67-7de8f63543d8”
“helloworld.sample:5000” “192.168.1.246:15443”
outbound|5000||helloworld.sample.svc.cluster.local - 10.107.117.235:5000
10.32.0.10:36840 - [2019-05-25T08:06:12.834Z] “GET /hello HTTP/1.1” 200
- “-” 0 60 181 180 “-” “curl/7.60.0”
“ce480b56-fafd-468b-9996-9fea5257cb1e” “helloworld.sample:5000”
“10.32.0.9:5000” outbound|5000||helloworld.sample.svc.cluster.local -
10.107.117.235:5000 10.32.0.10:36886 -

In ``cluster2``, the gateway IP of ``cluster1``
(``192.168.1.246:15443``) is logged when v1 was called and the gateway
IP in ``cluster2`` (``10.32.0.9:5000``) is logged when v2 was called.

Cleanup
-------

Execute the following commands to clean up the example services **and**
the Istio components.

Cleanup the ``cluster2`` cluster:

| .. code:: sh

      $ istioctl manifest generate
  –context=:math:`CTX_CLUSTER2 \
   --set profile=remote \
   --set values.gateways.enabled=true \
   --set values.security.selfSigned=false \
   --set values.global.createRemoteSvcEndpoints=true \
   --set values.global.remotePilotCreateSvcEndpoint=true \
   --set values.global.remotePilotAddress=`\ {LOCAL_GW_ADDR}
| –set values.global.remotePolicyAddress=\ :math:`{LOCAL_GW_ADDR} \
   --set values.global.remoteTelemetryAddress=`\ {LOCAL_GW_ADDR}
| –set
  values.gateways.istio-ingressgateway.env.ISTIO_META_NETWORK=“network2”
| –set values.global.network=“network2” \| kubectl
  –context=$CTX_CLUSTER2 delete -f - $ kubectl delete
  –context=$CTX_CLUSTER2 ns sample $ rm n2-k8s-config $ unset
  CTX_CLUSTER2 CLUSTER_NAME SERVER SECRET_NAME CA_DATA TOKEN
  INGRESS_HOST SECURE_INGRESS_PORT INGRESS_PORT LOCAL_GW_ADDR {{< /text
  >}}

Cleanup the ``cluster1`` cluster:

.. code:: sh

      $ istioctl manifest generate
–context=:math:`CTX_CLUSTER1 \
 -f install/kubernetes/operator/examples/multicluster/values-istio-multicluster-primary.yaml | kubectl --context=`\ CTX_CLUSTER1
delete -f - $ kubectl delete –context=$CTX_CLUSTER1 ns sample $ unset
CTX_CLUSTER1
