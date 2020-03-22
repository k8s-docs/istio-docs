Installation Configuration Profiles
=========================================

This page describes the built-in configuration profiles that can be used
when `installing Istio </docs/setup/install/istioctl/>`_. The profiles
provide customization of the Istio control plane and of the sidecars for
the Istio data plane. You can start with one of Istio’s built-in
configuration profiles and then further customize the configuration for
your specific needs. The following built-in configuration profiles are
currently available:

1. **default**: enables components according to the default settings of
   the `IstioOperator API </docs/reference/config/istio.operator.v1alpha1/>`_ (recommend
   for production deployments). You can display the default setting by
   running the command ``istioctl profile dump``.

2. **demo**: configuration designed to showcase Istio functionality with
   modest resource requirements. It is suitable to run the
   `Bookinfo </docs/examples/bookinfo/>`_ application and associated
   tasks. This is the configuration that is installed with the `quick
   start </docs/setup/getting-started/>`_ instructions, but you can
   later `customize the
   configuration </docs/setup/install/istioctl/#customizing-the-configuration>`_
   to enable additional features if you wish to explore more advanced
   tasks.

   .. warning::

   This profile enables high levels of tracing and
   access logging so it is not suitable for performance tests. {{<
   /warning >}}

3. **minimal**: the minimal set of components necessary to use Istio’s
   `traffic management </docs/tasks/traffic-management/>`_ features.

4. **sds**: similar to the **default** profile, but also enables Istio’s
   `SDS (secret discovery
   service) </docs/tasks/security/citadel-config/auth-sds>`_. This
   profile comes with additional authentication features enabled by
   default (Strict Mutual TLS).

5. **remote**: used for configuring remote clusters of a `multicluster
   mesh </docs/ops/deployment/deployment-models/#multiple-clusters>`_
   with a `shared control
   plane </docs/setup/install/multicluster/shared-vpn/>`_
   configuration.

The components marked as **X** are installed within each profile:

================================ ======= ==== ======= === ======
\                                default demo minimal sds remote
================================ ======= ==== ======= === ======
Core components
      ``istio-citadel``          X       X            X   X
      ``istio-egressgateway``            X
      ``istio-galley``           X       X            X
      ``istio-ingressgateway``   X       X            X
      ``istio-nodeagent``                             X
      ``istio-pilot``            X       X    X       X
      ``istio-policy``           X       X            X
      ``istio-sidecar-injector`` X       X            X   X
      ``istio-telemetry``        X       X            X
Addons
      ``grafana``                        X
      ``istio-tracing``                  X
      ``kiali``                          X
      ``prometheus``             X       X            X
================================ ======= ==== ======= === ======

To further customize Istio and install addons, you can add one or more
``--set <key>=<value>`` options in the ``istioctl manifest`` command
that you use when installing Istio. Refer to `customizing the
configuration </docs/setup/install/istioctl/#customizing-the-configuration>`_
for details.
