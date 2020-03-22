adapters
============================

.. warning::

   Mixer is deprecated. The functionality provided by Mixer
is being moved into the Envoy proxies. Use of Mixer with Istio will only
be supported through the 1.7 release of Istio. {{</ warning>}}

.. note::

   To implement a new adapter for Mixer, please refer to the
`Adapter Developer’s
Guide <https://github.com/istio/istio/wiki/Mixer-Compiled-In-Adapter-Dev-Guide>`_.


Templates
---------

The table below shows the set of
`templates </docs/reference/config/policy-and-telemetry/templates>`_
that are implemented by each supported adapter.

{{< adapter_table >}}


.. toctree::
   :maxdepth: 2

   apache-skywalking/index
   apigee/index
   app-identity-access-adapter/index
   circonus/index
   cloudmonitor/index
   cloudwatch/index
   datadog/index
   denier/index
   fluentd/index
   kubernetesenv/index
   layer5/index
   list/index
   memquota/index
   newrelic/index
   opa/index
   prometheus/index
   redisquota/index
   solarwinds/index
   stackdriver/index
   statsd/index
   stdio/index
   wavefront/index
   zipkin/index
