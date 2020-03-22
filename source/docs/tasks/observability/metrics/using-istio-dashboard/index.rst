using-istio-dashboard
==========================

This task shows you how to setup and use the Istio Dashboard to monitor
mesh traffic. As part of this task, you will use the Grafana Istio
add-on and the web-based interface for viewing service mesh traffic
data.

The `Bookinfo </docs/examples/bookinfo/>`_ sample application is used
as the example application throughout this task.

Before you begin
----------------

-  `Install Istio </docs/setup>`_ in your cluster. If not enabled in
   your chosen configuration profile, enable the Grafana addon
   ``--set values.grafana.enabled=true``
   `option </docs/reference/config/installation-options/>`_.
-  Deploy `Bookinfo </docs/examples/bookinfo/>`_ application.

Viewing the Istio dashboard
---------------------------

1. Verify that the ``prometheus`` service is running in your cluster.

   In Kubernetes environments, execute the following command:

   .. code:: sh

      $ kubectl -n istio-system get svc prometheus NAME
   CLUSTER-IP EXTERNAL-IP PORT(S) AGE prometheus 10.59.241.54 9090/TCP
   2m

2. Verify that the Grafana service is running in your cluster.

   In Kubernetes environments, execute the following command:

   .. code:: sh

      $ kubectl -n istio-system get svc grafana NAME
   CLUSTER-IP EXTERNAL-IP PORT(S) AGE grafana 10.59.247.103 3000/TCP 2m


3. Open the Istio Dashboard via the Grafana UI.

   In Kubernetes environments, execute the following command:

   .. code:: sh

      $ kubectl -n istio-system port-forward $(kubectl -n
   istio-system get pod -l app=grafana -o
   jsonpath=‘{.items[0].metadata.name}’) 3000:3000 &

   Visit http://localhost:3000/dashboard/db/istio-mesh-dashboard in your
   web browser.

   The Istio Dashboard will look similar to:

.. image::./grafana-istio-dashboard.png
   :alt:
   :caption:Istio    Dashboard
   :width: 80%


4. Send traffic to the mesh.

   For the Bookinfo sample, visit ``http://$GATEWAY_URL/productpage`` in
   your web browser or issue the following command:

   .. code:: sh

      $ curl http://$GATEWAY_URL/productpage {{< /text
   >}}

   .. note::

   ``$GATEWAY_URL`` is the value set in the
   `Bookinfo </docs/examples/bookinfo/>`_ example.

   Refresh the page a few times (or send the command a few times) to
   generate a small amount of traffic.

   Look at the Istio Dashboard again. It should reflect the traffic that
   was generated. It will look similar to:

.. image::./dashboard-with-traffic.png
   :alt:
   :caption:Istio Dashboard With Traffic
   :width: 80%

   This gives the global view of the Mesh along with services and
   workloads in the mesh. You can get more details about services and
   workloads by navigating to their specific dashboards as explained
   below.

5. Visualize Service Dashboards.

   From the Grafana dashboard’s left hand corner navigation menu, you
   can navigate to Istio Service Dashboard or visit
   http://localhost:3000/dashboard/db/istio-service-dashboard in your
   web browser.

   .. note::

   You may need to select a service in the Service dropdown.


   The Istio Service Dashboard will look similar to:

.. image::./istio-service-dashboard.png
   :alt:
   :caption:Istio Service Dashboard
   :width: 80%

   This gives details about metrics for the service and then client
   workloads (workloads that are calling this service) and service
   workloads (workloads that are providing this service) for that
   service.

6. Visualize Workload Dashboards.

   From the Grafana dashboard’s left hand corner navigation menu, you
   can navigate to Istio Workload Dashboard or visit
   http://localhost:3000/dashboard/db/istio-workload-dashboard in your
   web browser.

   The Istio Workload Dashboard will look similar to:

.. image::./istio-workload-dashboard.png
   :alt:
   :caption:Istio Workload Dashboard
   :width: 80%

   This gives details about metrics for each workload and then inbound
   workloads (workloads that are sending request to this workload) and
   outbound services (services to which this workload send requests) for
   that workload.

About the Grafana addon
~~~~~~~~~~~~~~~~~~~~~~~

The Grafana addon is a preconfigured instance of Grafana. The base image
(```grafana/grafana:5.2.3`` <https://hub.docker.com/r/grafana/grafana/>`_)
has been modified to start with both a Prometheus data source and the
Istio Dashboard installed. The base install files for Istio, and Mixer
in particular, ship with a default configuration of global (used for
every service) metrics. The Istio Dashboard is built to be used in
conjunction with the default Istio metrics configuration and a
Prometheus backend.

The Istio Dashboard consists of three main sections:

1. A Mesh Summary View. This section provides Global Summary view of the
   Mesh and shows HTTP/gRPC and TCP workloads in the Mesh.

2. Individual Services View. This section provides metrics about
   requests and responses for each individual service within the mesh
   (HTTP/gRPC and TCP). This also provides metrics about client and
   service workloads for this service.

3. Individual Workloads View: This section provides metrics about
   requests and responses for each individual workload within the mesh
   (HTTP/gRPC and TCP). This also provides metrics about inbound
   workloads and outbound services for this workload.

For more on how to create, configure, and edit dashboards, please see
the `Grafana documentation <https://docs.grafana.org/>`_.

Cleanup
-------

-  Remove any ``kubectl port-forward`` processes that may be running:

   .. code:: sh

      $ killall kubectl

-  If you are not planning to explore any follow-on tasks, refer to the
   `Bookinfo cleanup </docs/examples/bookinfo/#cleanup>`_ instructions
   to shutdown the application.
