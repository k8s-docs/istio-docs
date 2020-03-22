message-format
================

The ``istioctl analyze`` command provides messages in the format:

{{< text plain >}} [] ()

The ``<affected-resource>`` field expands to:

{{< text plain >}} .

For example:

{{< text plain >}} Error [IST0101] (VirtualService httpbin.default)
Referenced gateway not found: “httpbin-gateway-bogus”

The ``<message-details>`` field contains a detailed description that may
contain further information to help resolve the problem. The namespace
suffix is omitted for cluster-scoped resources, for example
``namespace``.
