Bookinfo Application
==========================

This example deploys a sample application composed of four separate
microservices used to demonstrate various Istio features.

.. note::

   If you installed Istio using the `Getting
Started </docs/setup/getting-started/>`_ instructions, you already have
Bookinfo installed and you can skip these steps.

The application displays information about a book, similar to a single
catalog entry of an online book store. Displayed on the page is a
description of the book, book details (ISBN, number of pages, and so
on), and a few book reviews.

The Bookinfo application is broken into four separate microservices:

-  ``productpage``. The ``productpage`` microservice calls the
   ``details`` and ``reviews`` microservices to populate the page.
-  ``details``. The ``details`` microservice contains book information.
-  ``reviews``. The ``reviews`` microservice contains book reviews. It
   also calls the ``ratings`` microservice.
-  ``ratings``. The ``ratings`` microservice contains book ranking
   information that accompanies a book review.

There are 3 versions of the ``reviews`` microservice:

-  Version v1 doesn’t call the ``ratings`` service.
-  Version v2 calls the ``ratings`` service, and displays each rating as
   1 to 5 black stars.
-  Version v3 calls the ``ratings`` service, and displays each rating as
   1 to 5 red stars.

The end-to-end architecture of the application is shown below.

.. image::/noistio.svg
   :alt:
   :caption:Bookinfo Application without Istio
   :width: 80%

This application is polyglot, i.e., the microservices are written in
different languages. It’s worth noting that these services have no
dependencies on Istio, but make an interesting service mesh example,
particularly because of the multitude of services, languages and
versions for the ``reviews`` service.

Before you begin
----------------

If you haven’t already done so, setup Istio by following the
instructions in the `installation guide </docs/setup/>`_.

Deploying the application
-------------------------

To run the sample with Istio requires no changes to the application
itself. Instead, you simply need to configure and run the services in an
Istio-enabled environment, with Envoy sidecars injected along side each
service. The resulting deployment will look like this:

.. image::./withistio.svg
   :alt:
   :caption:Bookinfo Application
   :width: 80%

All of the microservices will be packaged with an Envoy sidecar that
intercepts incoming and outgoing calls for the services, providing the
hooks needed to externally control, via the Istio control plane,
routing, telemetry collection, and policy enforcement for the
application as a whole.

Start the application services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

   If you use GKE, please ensure your cluster has at least 4
standard GKE nodes. If you use Minikube, please ensure you have at least
4GB RAM.

1. Change directory to the root of the Istio installation.

2. The default Istio installation uses `automatic sidecar
   injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_.
   Label the namespace that will host the application with
   ``istio-injection=enabled``:

   .. code:: sh

      $ kubectl label namespace default
   istio-injection=enabled

   .. warning::

   If you use OpenShift, make sure to give appropriate
   permissions to service accounts on the namespace as described in
   `OpenShift setup
   page </docs/setup/platform-setup/openshift/#privileged-security-context-constraints-for-application-sidecars>`_.


3. Deploy your application using the ``kubectl`` command:

   .. code:: sh

      $ kubectl apply -f
   @samples/bookinfo/platform/kube/bookinfo.yaml@

   .. warning::

   If you disabled automatic sidecar injection during
   installation and rely on [manual sidecar injection]
   (/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection),
   use the
   `istioctl kube-inject </docs/reference/commands/istioctl/#istioctl-kube-inject>`_
   command to modify the ``bookinfo.yaml`` file before deploying your
   application.

   .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f
   @samples/bookinfo/platform/kube/bookinfo.yaml@)



   The command launches all four services shown in the ``bookinfo``
   application architecture diagram. All 3 versions of the reviews
   service, v1, v2, and v3, are started.

   .. note::

   In a realistic deployment, new versions of a microservice
   are deployed over time instead of deploying all versions
   simultaneously.

4. Confirm all services and pods are correctly defined and running:

   .. code:: sh

      $ kubectl get services NAME CLUSTER-IP EXTERNAL-IP
   PORT(S) AGE details 10.0.0.31 9080/TCP 6m kubernetes 10.0.0.1 443/TCP
   7d productpage 10.0.0.120 9080/TCP 6m ratings 10.0.0.15 9080/TCP 6m
   reviews 10.0.0.170 9080/TCP 6m

   and

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE
   details-v1-1520924117-48z17 2/2 Running 0 6m
   productpage-v1-560495357-jk1lz 2/2 Running 0 6m
   ratings-v1-734492171-rnr5l 2/2 Running 0 6m
   reviews-v1-874083890-f0qf0 2/2 Running 0 6m
   reviews-v2-1343845940-b34q5 2/2 Running 0 6m
   reviews-v3-1813607990-8ch52 2/2 Running 0 6m

5. To confirm that the Bookinfo application is running, send a request
   to it by a ``curl`` command from some pod, for example from
   ``ratings``:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l app=ratings
   -o jsonpath=‘{.items[0].metadata.name}’) -c ratings – curl
   productpage:9080/productpage \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   "

   .. raw:: html

      <title>

   Simple Bookstore App

   .. raw:: html

      </title>



Determine the ingress IP and port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that the Bookinfo services are up and running, you need to make the
application accessible from outside of your Kubernetes cluster, e.g.,
from a browser. An `Istio
Gateway </docs/concepts/traffic-management/#gateways>`_ is used for
this purpose.

1. Define the ingress gateway for the application:

   .. code:: sh

      $ kubectl apply -f
   @samples/bookinfo/networking/bookinfo-gateway.yaml@

2. Confirm the gateway has been created:

   .. code:: sh

      $ kubectl get gateway NAME AGE bookinfo-gateway 32s


3. Follow `these
   instructions </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_
   to set the ``INGRESS_HOST`` and ``INGRESS_PORT`` variables for
   accessing the gateway. Return here, when they are set.

4. Set ``GATEWAY_URL``:

   .. code:: sh

      $ export
   GATEWAY_URL=\ :math:`INGRESS_HOST:`\ INGRESS_PORT

Confirm the app is accessible from outside the cluster
------------------------------------------------------

To confirm that the Bookinfo application is accessible from outside the
cluster, run the following ``curl`` command:

.. code:: sh

      $ curl -s http://${GATEWAY_URL}/productpage \| grep -o
"

.. raw:: html

   <title>

.\*

.. raw:: html

   </title>

"

.. raw:: html

   <title>

Simple Bookstore App

.. raw:: html

   </title>



You can also point your browser to ``http://$GATEWAY_URL/productpage``
to view the Bookinfo web page. If you refresh the page several times,
you should see different versions of reviews shown in ``productpage``,
presented in a round robin style (red stars, black stars, no stars),
since we haven’t yet used Istio to control the version routing.

Apply default destination rules
-------------------------------

Before you can use Istio to control the Bookinfo version routing, you
need to define the available versions, called *subsets*, in `destination
rules </docs/concepts/traffic-management/#destination-rules>`_.

Run the following command to create default destination rules for the
Bookinfo services:

-  If you did **not** enable mutual TLS, execute this command:

   .. note::

   Choose this option if you are new to Istio and are using
   the ``demo`` `configuration
   profile </docs/setup/additional-setup/config-profiles/>`_. {{< /tip
   >}}

   .. code:: sh

      $ kubectl apply -f
   @samples/bookinfo/networking/destination-rule-all.yaml@

-  If you **did** enable mutual TLS, execute this command:

   .. code:: sh

      $ kubectl apply -f
   @samples/bookinfo/networking/destination-rule-all-mtls.yaml@

Wait a few seconds for the destination rules to propagate.

You can display the destination rules with the following command:

.. code:: sh

      $ kubectl get destinationrules -o yaml

What’s next
-----------

You can now use this sample to experiment with Istio’s features for
traffic routing, fault injection, rate limiting, etc. To proceed, refer
to one or more of the `Istio Tasks </docs/tasks>`_, depending on your
interest. `Configuring Request
Routing </docs/tasks/traffic-management/request-routing/>`_ is a good
place to start for beginners.

Cleanup
-------

When you’re finished experimenting with the Bookinfo sample, uninstall
and clean it up using the following instructions:

1. Delete the routing rules and terminate the application pods

   .. code:: sh

      $ @samples/bookinfo/platform/kube/cleanup.sh@

2. Confirm shutdown

   .. code:: sh

      $ kubectl get virtualservices #– there should be no
   virtual services $ kubectl get destinationrules #– there should be no
   destination rules $ kubectl get gateway #– there should be no gateway
   $ kubectl get pods #– the Bookinfo pods should be deleted {{< /text
   >}}
