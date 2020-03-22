controlling-what-the-sidecar-reports
======================================

It is sometimes useful to exclude access to specific URLs from being
reported. For example, you might wish to exclude some health-checking
URLs. You can skip telemetry reports specific to certain URLs by
excluding them using a ``match`` clause of the telemetry configuration
such as:

.. code:: yaml

    match: source.name != “health”
