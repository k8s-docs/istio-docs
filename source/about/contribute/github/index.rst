github
====================================

The Istio documentation follows the standard `GitHub collaboration
flow <https://guides.github.com/introduction/flow/>`_ for Pull Requests
(PRs). This well-established collaboration model helps open source
projects manage the following types of contributions:

-  `Add </about/contribute/add-content>`_ new files to the repository.
-  `Edit <#quick-edit>`_ existing files.
-  `Review </about/contribute/review>`_ the added or modified files.
-  Manage multiple release or development
   `branches <#branching-strategy>`_.

The contribution guides assume you can complete the following tasks:

-  Fork the `Istio documentation
   repository <https://github.com/istio/istio.io>`_.
-  Create a branch for your changes.
-  Add commits to that branch.
-  Open a PR to share your contribution.

Before you begin
----------------

To contribute to the Istio documentation, you need to:

1. Create a `GitHub account <https://github.com>`_.

2. Sign the `Contributor License
   Agreement <https://github.com/istio/community/blob/master/CONTRIBUTING.md#contributor-license-agreements>`_.

3. Install `Docker <https://www.docker.com/get-started>`_ on your
   authoring system to preview and test your changes.

The Istio documentation is published under the `Apache
2.0 <https://github.com/istio/community/blob/master/LICENSE>`_ license.

.. _quick-edit:

Perform quick edits
-------------------

Anyone with a GitHub account who signs the CLA can contribute a quick
edit to any page on the Istio website. The process is very simple:

1. Visit the page you wish to edit.
2. Add ``preliminary`` to the beginning of the URL. For example, to edit
   ``https://istio.io/about``, the new URL should be
   ``https://preliminary.istio.io/about``
3. Click the pencil icon in the lower right corner.
4. Perform your edits on the GitHub UI.
5. Submit a Pull Request with your changes.

Please see our guides on how to `contribute new
content </about/contribute/add-content>`_ or `review
content </about/contribute/review>`_ to learn more about submitting
more substantial changes.

Branching strategy
------------------

Active content development takes place using the master branch of the
``istio/istio.io`` repository. On the day of an Istio release, we create
a release branch of master for that release. The following button takes
you to the repository on GitHub:

Browse this site’s source code

The Istio documentation repository uses multiple branches to publish
documentation for all Istio releases. Each Istio release has a
corresponding documentation branch. For example, there are branches
called ``release-1.0``, ``release-1.1``, ``release-1.2`` and so forth.
These branches were created on the day of the corresponding release. To
view the documentation for a specific release, see the `archive
page <https://archive.istio.io/>`_.

This branching strategy allows us to provide the following Istio online
resources:

-  The `public site </docs/>`_ shows the content from the current
   release branch.

-  The preliminary site at ``https://preliminary.istio.io`` shows the
   content from the master branch.

-  The `archive site <https://archive.istio.io>`_ shows the content
   from all prior release branches.

Given how branching works, if you submit a change into the master
branch, that change won’t appear on ``istio.io`` until the next major
Istio release happens. If your documentation change is relevant to the
current Istio release, then it’s probably worth also applying your
change to the current release branch. You can do this easily and
automatically by using the special cherry-pick labels on your
documentation PR. For example, if you introduce a correction in a PR to
the master branch, you can apply the ``cherrypick/release-1.4`` label in
order to merge this change to the ``release-1.4`` branch.

When your initial PR is merged, automation creates a new PR in the
release branch which includes your changes. You may need to add a
comment to the PR that reads ``@googlebot I consent`` in order to
satisfy the ``CLA`` bot that we use.

On rare occasions, automatic cherry picks don’t work. When that happens,
the automation leaves a note in the original PR indicating it failed.
When that happens, you must manually create the cherry pick and deal
with the merge issues that prevented the process from working
automatically.

Note that we only ever cherry pick changes into the current release
branch, and never to older branches. Older branches are considered to be
archived and generally no longer receive any changes.

Istio community roles
---------------------

Depending on your contributions and responsibilities, there are several
roles you can assume.

Visit our `role summary
page <https://github.com/istio/community/blob/master/ROLES.md#role-summary>`_
to learn about the roles, the related requirements and responsibilities,
and the privileges associated with the roles.

Visit our `community page <https://github.com/istio/community>`_ to
learn more about the Istio community in general.
