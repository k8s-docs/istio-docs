add-content
====================================

To contribute new documentation to Istio, just follow these steps:

1. Identify the audience and intended use for the information.
2. Choose the `type of content <#content-types>`_ you wish to
   contribute.
3. `Choose a title <#choosing-a-title>`_.
4. Write your contribution following our `documentation contribution
   guides </about/contribute>`_.
5. Submit your contribution to our `GitHub
   repository <https://github.com/istio/istio.io>`_.
6. Follow our `review process </about/contribute/review>`_ until your
   contribution is merged.

Identify the audience and intended use
--------------------------------------

The best documentation starts by knowing the intended readers, their
knowledge, and what you expect them to do with the information.
Otherwise, you cannot determine the appropriate scope and depth of
information to provide, its ideal structure, or the necessary supporting
information. The following examples show this principle in action:

-  The reader needs to perform a specific task: Tell them how to
   recognize when the task is necessary and provide the task itself as a
   list of numbered steps, don’t simply describe the task in general
   terms.

-  The reader must understand a concept before they can perform a task:
   Before the task, tell them about the prerequisite information and
   provide a link to it.

-  The reader needs to make a decision: Provide the conceptual
   information necessary to know when to make the decision, the
   available options, and when to choose one option instead of the
   other.

-  The reader is an administrator but not a SWE: Provide a script, not a
   link to a code sample in a developer’s guide.

-  The reader needs to extend the features of the product: Provide an
   example of how to extend the feature, using a simplified scenario for
   illustration purposes.

-  The reader needs to understand complex feature relationships: Provide
   a diagram showing the relationships, rather than writing multiple
   pages of content that is tedious to read and understand.

The most important thing to avoid is the common mistake of simply giving
readers all the information you have, because you are unsure about what
information they need.

If you need help identifying the audience for you content, we are happy
to help and answer all your questions during the `Docs Working
Group <https://github.com/istio/community/blob/master/WORKING-GROUPS.md#istio-working-groups>`_
biweekly meetings.

Content types
-------------

When you understand the audience and the intended use for the
information you provide, you can choose content type that best addresses
their needs. To make it easy for you to choose, the following table
shows the supported content types, their intended audiences, and the
goals each type strives to achieve:

.. raw:: html

   <table>

.. raw:: html

   <thead>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Content type

.. raw:: html

   </th>

.. raw:: html

   <th>

Goals

.. raw:: html

   </th>

.. raw:: html

   <th>

Audiences

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   </thead>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Concepts

.. raw:: html

   </td>

.. raw:: html

   <td>

Explain some significant aspect of Istio. For example, a concept page
describes the configuration model of a feature and explains its
functionality. Concept pages don’t include sequences of steps. Instead,
provide links to corresponding tasks.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers that want to understand how features work with only basic
knowledge of the project.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Reference pages

.. raw:: html

   </td>

.. raw:: html

   <td>

Provide exhaustive and detailed technical information. Common examples
include API parameters, command-line options, configuration settings,
and advanced procedures. Reference content is generated from the Istio
code base and tested for accuracy.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers with advanced and deep technical knowledge of the project that
needs specific bits of information to complete advanced tasks.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Examples

.. raw:: html

   </td>

.. raw:: html

   <td>

Describe a working and stand-alone example that highlights a set of
features, an integration of Istio with other projects, or an end-to-end
solution for a use case. Examples must use an existing Istio setup as a
starting point. Examples must include an automated test since they are
maintained for technical accuracy.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers that want to quickly run the example themselves and experiment.
Ideally, readers should be able to easily change the example to produce
their own solutions.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Tasks

.. raw:: html

   </td>

.. raw:: html

   <td>

Shows how to achieve a single goal using Istio features. Tasks contain
procedures written as a sequence of steps. Tasks provide minimal
explanation of the features, but include links to the concepts that
provide the related background and knowledge. Tasks must include
automated tests since they are tested and maintained for technical
accuracy.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers that want to use Istio features.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Setup pages

.. raw:: html

   </td>

.. raw:: html

   <td>

Focus on the installation steps needed to complete an Istio deployment.
Setup pages must include automated tests since they are tested and
maintained for technical accuracy.

.. raw:: html

   </td>

.. raw:: html

   <td>

New and existing Istio users that want to complete a deployment.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Blog posts

.. raw:: html

   </td>

.. raw:: html

   <td>

Focus on Istio or products and technologies related to it. Blog posts
fall in one of the following three categories:

.. raw:: html

   <ul>

.. raw:: html

   <li>

Posts detailing the author’s experience using and configuring Istio,
especially those that articulate a novel experience or perspective.

.. raw:: html

   </li>

.. raw:: html

   <li>

Posts highlighting Istio features.

.. raw:: html

   </li>

.. raw:: html

   <li>

Posts detailing how to accomplish a task or fulfill a specific use case
using Istio. Unlike Tasks and Examples, the technical accuracy of blog
posts is not maintained and tested after publication.

.. raw:: html

   </li>

.. raw:: html

   </ul>

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers with a basic understanding of the project who want to learn
about it in an anecdotal, experiential, and more informal way.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

News entries

.. raw:: html

   </td>

.. raw:: html

   <td>

Focus on timely information about Istio and related events. News entries
typically announce new releases or upcoming events.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers that want to quickly learn what’s new and what’s happening in
the Istio community.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

FAQ entries

.. raw:: html

   </td>

.. raw:: html

   <td>

Provide quick answers to common questions. Answers don’t introduce any
concepts. Instead, they provide practical advice or insights. Answers
must link to tasks, concepts, or examples in the documentation for
readers to learn more.

.. raw:: html

   </td>

.. raw:: html

   <td>

Readers with specific questions who are looking for brief answers and
resources to learn more.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

.. raw:: html

   <td>

Operation guides

.. raw:: html

   </td>

.. raw:: html

   <td>

Focus on practical solutions that address specific problems encountered
while running Istio in a real-world setting.

.. raw:: html

   </td>

.. raw:: html

   <td>

Service mesh operators that want to fix problems or implement solutions
for running Istio deployments.

.. raw:: html

   </td>

.. raw:: html

   </tr>

.. raw:: html

   </table>

Choosing a title
----------------

Choose a title for your topic that has the keywords you want search
engines to find. All content files in Istio are named ``index.md``, but
each content file is within a folder that uses the keywords in the
topic’s title, separated by hyphens, all in lowercase. Keep folder names
as short as possible to make cross-references easier to create and
maintain.

Submit your contribution to GitHub
----------------------------------

If you are not familiar with GitHub, see our `working with GitHub
guide </about/contribute/github>`_ to learn how to submit documentation
changes.

If you want to learn more about how and when your contributions are
published, see the `section on
branching </about/contribute/github#branching-strategy>`_ to understand
how we use branches and cherry picking to publish our content.
