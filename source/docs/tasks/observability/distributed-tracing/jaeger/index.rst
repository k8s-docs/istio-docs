jaeger
============================

After completing this task, you understand how to have your application
participate in tracing with `Jaeger <https://www.jaegertracing.io/>`_,
regardless of the language, framework, or platform you use to build your
application.

This task uses the `Bookinfo </docs/examples/bookinfo/>`_ sample as the
example application.

To learn how Istio handles tracing, visit this task’s
`overview <../overview/>`_.

Before you begin
----------------

1. To set up Istio, follow the instructions in the `Installation
   guide </docs/setup/install/istioctl>`_ and then configure:

   a) a demo/test environment by setting the
      ``--set values.tracing.enabled=true`` install option to enable
      tracing “out of the box”

   b) a production environment by referencing an existing Jaeger
      instance, e.g. created with the
      `operator <https://github.com/jaegertracing/jaeger-operator>`_,
      and then setting the
      ``--set values.global.tracer.zipkin.address=<jaeger-collector-service>.<jaeger-collector-namespace>:9411``
      install option.

   .. warning::

   When you enable tracing, you can set the sampling
   rate that Istio uses for tracing. Use the
   ``values.pilot.traceSampling`` option to set the sampling rate. The
   default sampling rate is 1%.

2. Deploy the
   `Bookinfo </docs/examples/bookinfo/#deploying-the-application>`_
   sample application.

Accessing the dashboard
-----------------------

`Remotely Accessing Telemetry
Addons </docs/tasks/observability/gateways>`_ details how to configure
access to the Istio addons through a gateway. Alternatively, to use a
Kubernetes ingress, specify the option
``--set values.tracing.ingress.enabled=true`` during install.

For testing (and temporary access), you may also use port-forwarding.
Use the following, assuming you’ve deployed Jaeger to the
``istio-system`` namespace:

.. code:: sh

      $ istioctl dashboard jaeger

Generating traces using the Bookinfo sample
-------------------------------------------

1. When the Bookinfo application is up and running, access
   ``http://$GATEWAY_URL/productpage`` one or more times to generate
   trace information.

   {{< boilerplate trace-generation >}}

2. From the left-hand pane of the dashboard, select
   ``productpage.default`` from the **Service** drop-down list and click
   **Find Traces**:

.. image::./istio-tracing-list.png
   :alt:
   :caption:Tracing Dashboard
   :width: 80%

3. Click on the most recent trace at the top to see the details
   corresponding to the latest request to the ``/productpage``:

.. image::./istio-tracing-details.png
   :alt:
   :caption:Detailed Trace View
   :width: 80%

4. The trace is comprised of a set of spans, where each span corresponds
   to a Bookinfo service, invoked during the execution of a
   ``/productpage`` request, or internal Istio component, for example:
   ``istio-ingressgateway``.

Cleanup
-------

1. Remove any ``istioctl`` processes that may still be running using
   control-C or:

   .. code:: sh

      $ killall istioctl

2. If you are not planning to explore any follow-on tasks, refer to the
   `Bookinfo cleanup </docs/examples/bookinfo/#cleanup>`_ instructions
   to shutdown the application.
