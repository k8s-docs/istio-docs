announcing-0.4
========================

This release has only got a few weeks’ worth of changes, as we stabilize
our monthly release process. In addition to the usual pile of bug fixes
and performance improvements, this release includes the items below.

{{< relnote >}}

General
-------

-  **Cloud Foundry**. Added minimum Pilot support for the `Cloud
   Foundry <https://www.cloudfoundry.org>`_ platform, making it
   possible for Pilot to discover CF services and service instances.

-  **Circonus**. Mixer now includes an adapter for the
   `Circonus <https://www.circonus.com>`_ analytics and monitoring
   platform.

-  **Pilot Metrics**. Pilot now collects metrics for diagnostics.

-  **Helm Charts**. We now provide Helm charts to install Istio.

-  **Enhanced Attribute Expressions**. Mixer’s expression language
   gained a few new functions to make it easier to write policy rules.
   `Learn
   more </docs/reference/config/policy-and-telemetry/expression-language/>`_

If you’re into the nitty-gritty details, you can see our more detailed
low-level release notes
`here <https://github.com/istio/istio/wiki/v0.4.0>`_.
