enable-istio-all-microservices
============================================

Previously, you enabled Istio on a single microservice, ``productpage``.
You can proceed to enable Istio on the microservices incrementally to
get the Istio functionality for more microservices. For the purpose of
this tutorial, you will enable Istio on all the remaining microservices
in one step.

1. For the purpose of this tutorial, scale the deployments of the
   microservices down to 1:

   .. code:: sh

      $ kubectl scale deployments –all –replicas 1

2. Redeploy the Bookinfo application, Istio-enabled. The service
   ``productpage`` will not be redeployed since it already has Istio
   injected, and its pods will not be changed. This time you will use
   only a single replica of a microservice.

   .. code:: sh

      $ curl -s {{< github_file
   >}}/samples/bookinfo/platform/kube/bookinfo.yaml \| istioctl
   kube-inject -f - \| kubectl apply -l app!=reviews -f - $ curl -s {{<
   github_file >}}/samples/bookinfo/platform/kube/bookinfo.yaml \|
   istioctl kube-inject -f - \| kubectl apply -l app=reviews,version=v2
   -f - service “details” unchanged deployment “details-v1” configured
   service “ratings” unchanged deployment “ratings-v1” configured
   service “productpage” unchanged deployment “productpage-v1”
   configured deployment “reviews-v2” configured

3. Access the application’s webpage several times. Note that Istio was
   added **transparently**, the original application did not change. It
   was added on the fly, without the need to undeploy and redeploy the
   whole application.

4. Check the application pods and verify that now each pod has two
   containers. One container is the microservice itself, the other is
   the sidecar proxy attached to it:

   .. code:: sh

      $ kubectl get pods details-v1-58c68b9ff-kz9lf 2/2
   Running 0 2m productpage-v1-59b4f9f8d5-d4prx 2/2 Running 0 2m
   ratings-v1-b7b7fbbc9-sggxf 2/2 Running 0 2m
   reviews-v2-dfbcf859c-27dvk 2/2 Running 0 2m sleep-88ddbcfdd-cc85s 1/1
   Running 0 7h

5. Access the Istio dashboard at
   `http://my-istio-dashboard.io/dashboard/db/istio-mesh-dashboard <http://my-istio-dashboard.io/dashboard/db/istio-mesh-dashboard>`_.
   In the top left drop-down menu, select *Istio Mesh Dashboard*. Note
   that now all the services from your namespace appear in the list of
   services.

.. image::dashboard-mesh-all.png
   :alt:
   :caption:Istio Mesh Dashboard
   :width: 80%

6. Check some other microservice in *Istio Service Dashboard*,
   e.g. \ ``ratings`` :

.. image::dashboard-ratings.png
   :alt:
   :caption:Istio Service Dashboard
   :width: 80%

7. Visualize your application’s topology by using the
   `Kiali <https://www.kiali.io>`_ console, which is not a part of
   Istio. Access
   `http://my-kiali.io/kiali/console <http://my-kiali.io/kiali/console>`_.
   (The ``my-kiali.io`` URL should be in your /etc/hosts file, you set
   it
   `previously </docs/examples/microservices-istio/bookinfo-kubernetes/#update-your-etc-hosts-configuration-file>`_).
   If you installed Kiali as part of the `getting
   started </docs/setup/getting-started/>`_ instructions, your Kiali
   console user name is ``admin`` and the password is ``admin``.

   Click on the Graph tab and select your namespace in the *Namespace*
   drop-down menu in the top level corner. In the *Display* drop-down
   menu mark the *Traffic Animation* check box to see some cool traffic
   animation.

   .. image::kiali-display-menu.png
      :alt:
      :caption:Kiali Graph Tab, display drop-down menu
      :width: 80%

   Try different options in the *Edge Labels* drop-down menu. Hover with
   the mouse over the nodes and edges of the graph. Notice the traffic
   metrics on the right.

   .. image::kiali-edge-labels-menu.png
      :alt:
      :caption:Kiali Graph Tab, edge labels drop-down menu
      :width: 80%

   .. image::kiali-display-menu.png
      :alt:
      :caption:kiali-initial.png” caption=“Kiali Graph Tab
      :width: 80%

You are ready to `configure the Istio Ingress Gateway </docs/examples/microservices-istio/istio-ingress-gateway>`_.
