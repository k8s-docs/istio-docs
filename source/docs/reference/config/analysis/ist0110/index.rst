ist0110
===========

This message occurs when more than one Sidecar resource in a namespace
selects the same workload instance. This can lead to undefined behavior.
See the reference for the
`Sidecar </docs/reference/config/networking/sidecar/>`_ resource for
more information.

To fix, ensure that the set of workload instances (e.g. pods) selected
by each Sidecar workload selector in a namespace do not overlap.
