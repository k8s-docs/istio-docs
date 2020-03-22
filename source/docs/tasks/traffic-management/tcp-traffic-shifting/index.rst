TCP Traffic Shifting
==========================

This task shows you how to gradually migrate TCP traffic from one
version of a microservice to another. For example, you might migrate TCP
traffic from an older version to a new version.

A common use case is to migrate TCP traffic gradually from one version
of a microservice to another. In Istio, you accomplish this goal by
configuring a sequence of rules that route a percentage of TCP traffic
to one service or another. In this task, you will send 100% of the TCP
traffic to ``tcp-echo:v1``. Then, you will route 20% of the TCP traffic
to ``tcp-echo:v2`` using Istio’s weighted routing feature.

Before you begin
----------------

-  Setup Istio by following the instructions in the `Installation guide </docs/setup/>`_.

-  Review the `Traffic Management </docs/concepts/traffic-management>`_ concepts doc.

Apply weight-based TCP routing
------------------------------

1. To get started, deploy the ``v1`` version of the ``tcp-echo`` microservice.

   -  First, create a namespace for testing TCP traffic shifting

      .. code:: sh

         $ kubectl create namespace istio-io-tcp-traffic-shifting

   -  If you are using `manual sidecar injection </docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection>`_,
      use the following command

      .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f @samples/tcp-echo/tcp-echo-services.yaml@) -n istio-io-tcp-traffic-shifting

      The
      `istioctl kube-inject </docs/reference/commands/istioctl/#istioctl-kube-inject>`_
      command is used to manually modify the ``tcp-echo-services.yaml``
      file before creating the deployments.

   -  If you are using a cluster with `automatic sidecar injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_
      enabled, label the ``istio-io-tcp-traffic-shifting`` namespace
      with ``istio-injection=enabled``

      .. code:: sh

         $ kubectl label namespace istio-io-tcp-traffic-shifting istio-injection=enabled

      Then simply deploy the services using ``kubectl``

      .. code:: sh

         $ kubectl apply -f @samples/tcp-echo/tcp-echo-services.yaml@ -n istio-io-tcp-traffic-shifting

2. Next, route all TCP traffic to the ``v1`` version of the ``tcp-echo`` microservice.

   .. code:: sh

      $ kubectl apply -f @samples/tcp-echo/tcp-echo-all-v1.yaml@ -n istio-io-tcp-traffic-shifting

3. Confirm that the ``tcp-echo`` service is up and running.

   The ``$INGRESS_HOST`` variable below is the External IP address of
   the ingress, as explained in the `Ingress Gateways </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_
   doc. To obtain the ``$INGRESS_PORT`` value, use the following command.

   .. code:: sh

      $ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath=‘{.spec.ports[?(@.name==“tcp”)].port}’)

   Send some TCP traffic to the ``tcp-echo`` microservice.

   .. code:: sh

      $ for i in {1..10}; do
      docker run -e
      INGRESS_HOST=\ :math:`INGRESS_HOST -e INGRESS_PORT=`\ INGRESS_PORT
      -it –rm busybox sh -c “(date; sleep 1) \| nc $INGRESS_HOST
      $INGRESS_PORT”;
      done one Mon Nov 12 23:24:57 UTC 2018 one Mon Nov 12 23:25:00 UTC
      2018 one Mon Nov 12 23:25:02 UTC 2018 one Mon Nov 12 23:25:05 UTC
      2018 one Mon Nov 12 23:25:07 UTC 2018 one Mon Nov 12 23:25:10 UTC
      2018 one Mon Nov 12 23:25:12 UTC 2018 one Mon Nov 12 23:25:15 UTC
      2018 one Mon Nov 12 23:25:17 UTC 2018 one Mon Nov 12 23:25:19 UTC
      2018

   .. warning::

   The ``docker`` command may require using ``sudo`` depending on your Docker installation.

   You should notice that all the timestamps have a prefix of *one*,
   which means that all traffic was routed to the ``v1`` version of the
   ``tcp-echo`` service.

4. Transfer 20% of the traffic from ``tcp-echo:v1`` to ``tcp-echo:v2``
   with the following command:

   .. code:: sh

      $ kubectl apply -f @samples/tcp-echo/tcp-echo-20-v2.yaml@ -n istio-io-tcp-traffic-shifting

   Wait a few seconds for the new rules to propagate.

5. Confirm that the rule was replaced:

   .. code:: bash

       $ kubectl get virtualservice tcp-echo -o yaml -n istio-io-tcp-traffic-shifting apiVersion:
         networking.istio.io/v1alpha3 kind: VirtualService metadata: name:
         tcp-echo … spec: … tcp:

   -  match:

      -  port: 31400 route:
      -  destination: host: tcp-echo port: number: 9000 subset: v1
         weight: 80
      -  destination: host: tcp-echo port: number: 9000 subset: v2
         weight: 20

6. Send some more TCP traffic to the ``tcp-echo`` microservice.

   .. code:: sh

      $ for i in {1..10}; do
      docker run -e
     INGRESS_HOST=\ :math:`INGRESS_HOST -e INGRESS_PORT=`\ INGRESS_PORT
     -it –rm busybox sh -c “(date; sleep 1) \| nc $INGRESS_HOST
     $INGRESS_PORT”;
      done one Mon Nov 12 23:38:45 UTC 2018 two Mon Nov 12 23:38:47 UTC
     2018 one Mon Nov 12 23:38:50 UTC 2018 one Mon Nov 12 23:38:52 UTC
     2018 one Mon Nov 12 23:38:55 UTC 2018 two Mon Nov 12 23:38:57 UTC
     2018 one Mon Nov 12 23:39:00 UTC 2018 one Mon Nov 12 23:39:02 UTC
     2018 one Mon Nov 12 23:39:05 UTC 2018 one Mon Nov 12 23:39:07 UTC
     2018

   .. warning::

   The ``docker`` command may require using ``sudo``
   depending on your Docker installation.

   You should now notice that about 20% of the timestamps have a prefix
   of *two*, which means that 80% of the TCP traffic was routed to the
   ``v1`` version of the ``tcp-echo`` service, while 20% was routed to
   ``v2``.

Understanding what happened
---------------------------

In this task you partially migrated TCP traffic from an old to new
version of the ``tcp-echo`` service using Istio’s weighted routing
feature. Note that this is very different than doing version migration
using the deployment features of container orchestration platforms,
which use instance scaling to manage the traffic.

With Istio, you can allow the two versions of the ``tcp-echo`` service
to scale up and down independently, without affecting the traffic
distribution between them.

For more information about version routing with autoscaling, check out
the blog article `Canary Deployments using Istio </blog/2017/0.1-canary/>`_.

Cleanup
-------

1. Remove the ``tcp-echo`` application and routing rules:

   .. code:: sh

      $ kubectl delete -f @samples/tcp-echo/tcp-echo-all-v1.yaml@ -n istio-io-tcp-traffic-shifting
      $ kubectl delete -f @samples/tcp-echo/tcp-echo-services.yaml@ -n istio-io-tcp-traffic-shifting
      $ kubectl delete namespace istio-io-tcp-traffic-shifting
