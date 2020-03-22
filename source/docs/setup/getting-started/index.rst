Getting Started
==================

This guide lets you quickly evaluate Istio.
If you are already familiar with Istio or interested in installing other configuration profiles
or advanced `deployment models </docs/ops/deployment/deployment-models/>`_,
see `Customizable Install with istioctl </docs/setup/install/istioctl/>`_ instead.

These steps require you to have a {{< gloss >}}cluster{{< /gloss >}}
running a compatible version of Kubernetes. You can use any supported
platform, for example
`Minikube <https://kubernetes.io/docs/tasks/tools/install-minikube/>`_
or others specified by the `platform-specific setup
instructions </docs/setup/platform-setup/>`_.

Follow these steps to get started with Istio:

1. `Download and install Istio <#download>`_
2. `Deploy the sample application <#bookinfo>`_
3. `Open the application to outside traffic <#ip>`_
4. `View the dashboard <#dashboard>`_

.. _download:

Download Istio
--------------

1. Go to the `Istio release <%7B%7B%3C%20istio_release_url%20%3E%7D%7D>`_ page to
   download the installation file for your OS, or download and extract
   the latest release automatically (Linux or macOS):

   .. code:: sh

      $ curl -L https://istio.io/downloadIstio \| sh -


   .. note::

      The command above downloads the latest release
      (numerically) of Istio. To download a specific version, you can add a
      variable on the command line. For example to download Istio 1.4.3,
      you would run
      ``curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.4.3 sh -``


2. Move to the Istio package directory. For example, if the package is ``istio-{{< istio_full_version >}}``:

   .. code:: sh

      $ cd istio-{{< istio_full_version >}}

   The installation directory contains:

   -  Installation YAML files for Kubernetes in ``install/kubernetes``
   -  Sample applications in ``samples/``
   -  The `istioctl </docs/reference/commands/istioctl>`_ client binary in the ``bin/`` directory.

3. Add the ``istioctl`` client to your path (Linux or macOS):

   .. code:: sh

      $ export PATH=\ :math:`PWD/bin:`\ PATH

.. _install:

Install Istio
-------------

1. For this installation, we use the ``demo`` `configuration profile </docs/setup/additional-setup/config-profiles/>`_.
   It’s selected to have a good set of defaults for testing, but there are other profiles for production or performance testing.

   .. code:: sh

      $ istioctl manifest apply –set profile=demo

   Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT

   -  Applying manifest for component Base… ✔

      Finished applying manifest for component Base.

   -  Applying manifest for component Pilot… ✔

      Finished applying manifest for component Pilot. Waiting for resources to become ready…

   -  Applying manifest for component EgressGateways…
   -  Applying manifest for component IngressGateways…
   -  Applying manifest for component AddonComponents… ✔

      Finished applying manifest for component EgressGateways. ✔

      Finished applying manifest for component IngressGateways. ✔

      Finished applying manifest for component AddonComponents.

   ✔ Installation complete

2. Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later:

   .. code:: sh

      $ kubectl label namespace default istio-injection=enabled namespace/default labeled

.. _bookinfo:

Deploy the sample application
-----------------------------

1. Deploy the `Bookinfo sample application </docs/examples/bookinfo/>`_:

   .. code:: sh

      $ kubectl apply -f @samples/bookinfo/platform/kube/bookinfo.yaml@service/details

         created serviceaccount/bookinfo-details created
         deployment.apps/details-v1 created service/ratings created
         serviceaccount/bookinfo-ratings created deployment.apps/ratings-v1
         created service/reviews created serviceaccount/bookinfo-reviews
         created deployment.apps/reviews-v1 created deployment.apps/reviews-v2
         created deployment.apps/reviews-v3 created service/productpage
         created serviceaccount/bookinfo-productpage created
         deployment.apps/productpage-v1 created

2. The application will start. As each pod becomes ready, the Istio sidecar will deploy along with it.

   .. code:: sh

      $ kubectl get services NAME TYPE CLUSTER-IP

         EXTERNAL-IP PORT(S) AGE details ClusterIP 10.0.0.212 9080/TCP 29s
         kubernetes ClusterIP 10.0.0.1 443/TCP 25m productpage ClusterIP
         10.0.0.57 9080/TCP 28s ratings ClusterIP 10.0.0.33 9080/TCP 29s
         reviews ClusterIP 10.0.0.28 9080/TCP 29s

   and

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE

         details-v1-78d78fbddf-tj56d 0/2 PodInitializing 0 2m30s
         productpage-v1-85b9bf9cd7-zg7tr 0/2 PodInitializing 0 2m29s
         ratings-v1-6c9dbf6b45-5djtx 0/2 PodInitializing 0 2m29s
         reviews-v1-564b97f875-dzdt5 0/2 PodInitializing 0 2m30s
         reviews-v2-568c7c9d8f-p5wrj 1/2 Running 0 2m29s
         reviews-v3-67b4988599-7nhwz 0/2 PodInitializing 0 2m29s

   .. note::

      Re-run the previous command and wait until all pods
      report READY 2 / 2 and STATUS Running before you go to the next step.
      This might take a few minutes depending on your platform.

3. Verify everything is working correctly up to this point.
   Run this command to see if the app is running inside the cluster and serving
   HTML pages by checking for the page title in the response:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l app=ratings

         -o jsonpath=‘{.items[0].metadata.name}’) -c ratings – curl
         productpage:9080/productpage \| grep -o "

.. _ip:

Open the application to outside traffic
---------------------------------------

The Bookinfo application is deployed but not accessible from the
outside. To make it accessible, you need to create an `Istio Ingress
Gateway </docs/concepts/traffic-management/#gateways>`_, which maps a
path to a route at the edge of your mesh.

1. Associate this application with the Istio gateway:

   .. code:: sh

      $ kubectl apply -f

         @samples/bookinfo/networking/bookinfo-gateway.yaml@
         gateway.networking.istio.io/bookinfo-gateway created
         virtualservice.networking.istio.io/bookinfo created

2. Confirm the gateway has been created:

   .. code:: sh

      $ kubectl get gateway NAME AGE bookinfo-gateway 32s


Determining the ingress IP and ports
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow these instructions to set the ``INGRESS_HOST`` and
``INGRESS_PORT`` variables for accessing the gateway. Use the tabs to
choose the instructions for your chosen platform:

Set the ingress ports:

.. code:: sh

      $ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o
         jsonpath=‘{.spec.ports[?(@.name==“http2”)].nodePort}’) $ export
         SECURE_INGRESS_PORT=$(kubectl -n istio-system get service
         istio-ingressgateway -o
         jsonpath=‘{.spec.ports[?(@.name==“https”)].nodePort}’)

Ensure a port was successfully assigned to each environment variable:

.. code:: sh

    $ echo $INGRESS_PORT 32194

.. code:: sh

   $ echo $SECURE_INGRESS_PORT 31632

Set the ingress IP:

.. code:: sh

   $ export INGRESS_HOST=$(minikube ip)

Ensure an IP address was successfully assigned to the environment variable:

.. code:: sh

   $ echo $INGRESS_HOST 192.168.4.102

Run this command in a new terminal window to start a Minikube tunnel that sends traffic to your Istio Ingress Gateway:

.. code:: sh

   $ minikube tunnel

Execute the following command to determine if your Kubernetes cluster is running in an environment that supports external load balancers:

.. code:: sh

      $ kubectl get svc istio-ingressgateway -n istio-system

         NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE istio-ingressgateway
         LoadBalancer 172.21.109.129 130.211.10.121
         80:31380/TCP,443:31390/TCP,31400:31400/TCP 17h

If the ``EXTERNAL-IP`` value is set, your environment has an external
load balancer that you can use for the ingress gateway. If the
``EXTERNAL-IP`` value is ``<none>`` (or perpetually ``<pending>``), your
environment does not provide an external load balancer for the ingress
gateway. In this case, you can access the gateway using the service’s
`node port <https://kubernetes.io/docs/concepts/services-networking/service/#nodeport>`_.

Choose the instructions corresponding to your environment:

**Follow these instructions if you have determined that your environment has an external load balancer.**

Set the ingress IP and ports:

.. code:: sh

   $ export INGRESS_HOST=$(kubectl -n istio-system get
      service istio-ingressgateway -o
      jsonpath=‘{.status.loadBalancer.ingress[0].ip}’) $ export
      INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway
      -o jsonpath=‘{.spec.ports[?(@.name==“http2”)].port}’) $ export
      SECURE_INGRESS_PORT=$(kubectl -n istio-system get service
      istio-ingressgateway -o
      jsonpath=‘{.spec.ports[?(@.name==“https”)].port}’)

.. warning::

   In certain environments, the load balancer may be
   exposed using a host name, instead of an IP address. In this case, the
   ingress gateway’s ``EXTERNAL-IP`` value will not be an IP address, but
   rather a host name, and the above command will have failed to set the
   ``INGRESS_HOST`` environment variable. Use the following command to
   correct the ``INGRESS_HOST`` value:

   .. code:: sh

      $ export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath=‘{.status.loadBalancer.ingress[0].hostname}’)



**Follow these instructions if your environment does not have an external load balancer and choose a node port instead.**

Set the ingress ports:

.. code:: sh

   $ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o

      jsonpath=‘{.spec.ports[?(@.name==“http2”)].nodePort}’) $ export
      SECURE_INGRESS_PORT=$(kubectl -n istio-system get service
      istio-ingressgateway -o
      jsonpath=‘{.spec.ports[?(@.name==“https”)].nodePort}’)

*GKE:*

.. code:: sh

   $ export INGRESS_HOST=

You need to create firewall rules to allow the TCP traffic to the ``ingressgateway`` service’s ports.
Run the following commands to allow the traffic for the HTTP port, the secure port (HTTPS) or both:

.. code:: sh

   $ gcloud compute firewall-rules create
      allow-gateway-http –allow tcp:$INGRESS_PORT $ gcloud compute
      firewall-rules create allow-gateway-https –allow
      tcp:$SECURE_INGRESS_PORT

*Docker For Desktop:*

.. code:: sh

   $ export INGRESS_HOST=127.0.0.1

*Other environments (e.g., IBM Cloud Private, etc.):*

.. code:: sh

   $ export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath=‘{.items[0].status.hostIP}’)

1. Set ``GATEWAY_URL``:

   .. code:: sh

      $ export GATEWAY_URL=\ :math:`INGRESS_HOST:`\ INGRESS_PORT

2. Ensure an IP address and port were successfully assigned to the environment variable:

   .. code:: sh

      $ echo $GATEWAY_URL 192.168.99.100:32194

.. _confirm:

Verify external access
~~~~~~~~~~~~~~~~~~~~~~

Confirm that the Bookinfo application is accessible from outside.
Copy the output of this command and paste into your browser:

.. code:: sh

   $ echo http://$GATEWAY_URL/productpage

.. _dashboard:

View the dashboard
------------------

Istio has several optional dashboards installed by the ``demo``
installation. The Kiali dashboard helps you understand the structure of
your service mesh by displaying the topology and indicates the health of
your mesh.

1. Access the Kiali dashboard. The default user name is ``admin`` and default password is ``admin``.

   .. code:: sh

      $ istioctl dashboard kiali

2. In the left navigation menu, select *Graph* and in the *Namespace* drop down, select *default*.

   The Kiali dashboard shows an overview of your mesh with the
   relationships between the services in the ``Bookinfo`` sample
   application. It also provides filters to visualize the traffic flow.

   .. image::./kiali-example2.png
      :alt:
      :caption:Kiali Dashboard

Next steps
----------

Congratulations on completing the evaluation installation!

These tasks are a great place for beginners to further evaluate Istio’s
features using this ``demo`` installation:

-  `Request routing </docs/tasks/traffic-management/request-routing/>`_
-  `Fault injection </docs/tasks/traffic-management/fault-injection/>`_
-  `Traffic shifting </docs/tasks/traffic-management/traffic-shifting/>`_
-  `Querying metrics </docs/tasks/observability/metrics/querying-metrics/>`_
-  `Visualizing metrics </docs/tasks/observability/metrics/using-istio-dashboard/>`_
-  `Rate limiting </docs/tasks/policy-enforcement/rate-limiting/>`_
-  `Accessing external services </docs/tasks/traffic-management/egress/egress-control/>`_
-  `Visualizing your mesh </docs/tasks/observability/kiali/>`_

Before you customize Istio for production use, see these resources:

-  `Deployment models </docs/ops/deployment/deployment-models/>`_
-  `Deployment best practices </docs/ops/best-practices/deployment/>`_
-  `Pod requirements </docs/ops/deployment/requirements/>`_
-  `General installation instructions </docs/setup/>`_

Join the Istio community
------------------------

We welcome you to ask questions and give us feedback by joining the `Istio community </about/community/join/>`_.

Uninstall
---------

The uninstall deletes the RBAC permissions, the ``istio-system``
namespace, and all resources hierarchically under it. It is safe to
ignore errors for non-existent resources because they may have been
deleted hierarchically.

.. code:: sh

   $ istioctl manifest generate –set profile=demo \| kubectl delete -f -

To delete the ``Bookinfo`` sample application and its configuration, see `Bookinfo cleanup </docs/examples/bookinfo/#cleanup>`_.
