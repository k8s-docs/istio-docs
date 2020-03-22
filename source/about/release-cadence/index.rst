release cadence
====================================

We produce new builds of Istio for each commit. Around once a quarter or
so, we build a Long Term Support (LTS) release, and run through a bunch
more tests and release qualification. Finally, if we find something
wrong with an LTS release, we issue patches.

The different types represent different product quality levels and
different levels of support from the Istio team. In this context,
*support* means that we will produce patch releases for critical issues
and offer technical assistance. Separately, 3rd parties and partners may
offer longer-term support solutions.

+-----------+---------------------------------------+------------------+
| Type      | Support Level                         | Quality and      |
|           |                                       | Recommended Use  |
+===========+=======================================+==================+
| Developme | No support                            | Dangerous, may   |
| nt        |                                       | not be fully     |
| Build     |                                       | reliable. Useful |
|           |                                       | to experiment    |
|           |                                       | with.            |
+-----------+---------------------------------------+------------------+
| LTS       | Support is provided until 3 months    | Safe to deploy   |
| Release   | after the next LTS                    | in production.   |
|           |                                       | Users are        |
|           |                                       | encouraged to    |
|           |                                       | upgrade to these |
|           |                                       | releases as soon |
|           |                                       | as possible.     |
+-----------+---------------------------------------+------------------+
| Patches   | Same as the corresponding             | Users are        |
|           | Snapshot/LTS release                  | encouraged to    |
|           |                                       | adopt patch      |
|           |                                       | releases as soon |
|           |                                       | as they are      |
|           |                                       | available for a  |
|           |                                       | given release.   |
+-----------+---------------------------------------+------------------+

You can find available releases on the `releases
page <https://github.com/istio/istio/releases>`_, and if you’re the
adventurous type, you can learn about our development builds on the
`development builds
wiki <https://github.com/istio/istio/wiki/Dev%20Builds>`_. You can find
high-level releases notes for each LTS release `here </news>`_.

Naming scheme
-------------

Our naming scheme for LTS releases is:

{{< text plain >}} ..

where ``<minor>`` is increased for every LTS release, and
``<LTS patch level>`` counts the number of patches for the current LTS
release. A patch is usually a small change relative to the LTS.

For snapshot releases, our naming scheme is:

{{< text plain >}} .-alpha.

where ``<major>.<minor>`` represent the next LTS, and ``<sha>``
represents the git commit the release is built from.
