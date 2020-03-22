formatting
====================================

This page shows the formatting standards for the Istio documentation.
Istio uses Markdown to markup the content and Hugo to build the website.
To ensure consistency across our documentation, we have agreed on these
formatting standards.

Don’t use capitalization for emphasis
-------------------------------------

Only use the original capitalization found in the code or configuration
files when referencing those values directly. Use back-ticks \`\` around
the referenced value to make the connection explicit. For example, use
``IstioRoleBinding``, not ``Istio Role Binding`` or
``istio role binding``.

If you are not referencing values or code directly, use normal sentence
capitalization, for example, “The Istio role binding configuration takes
place in a YAML file.”

Use angle brackets for placeholders
-----------------------------------

Use angle brackets for placeholders in commands or code samples. Tell
the reader what the placeholder represents. For example:

{{< text markdown >}}

1. Display information about a pod:

   {{</\* text bash */>}} $ kubectl describe pod {{</* /text \*/>}}

   Where ``<pod-name>`` is the name of one of your pods.



Use **bold** to emphasize user interface elements
-------------------------------------------------

================= ===============
Do                Don’t
================= ===============
Click **Fork**.   Click “Fork”.
Select **Other**. Select ‘Other’.
================= ===============

Use *italics* to emphasize new terms
------------------------------------

+------------------------------------------------------------------+---+
| Do                                                               | D |
|                                                                  | o |
|                                                                  | n |
|                                                                  | ’ |
|                                                                  | t |
+==================================================================+===+
| A *cluster* is a set of nodes …                                  | A |
|                                                                  | “ |
|                                                                  | c |
|                                                                  | l |
|                                                                  | u |
|                                                                  | s |
|                                                                  | t |
|                                                                  | e |
|                                                                  | r |
|                                                                  | ” |
|                                                                  | i |
|                                                                  | s |
|                                                                  | a |
|                                                                  | s |
|                                                                  | e |
|                                                                  | t |
|                                                                  | o |
|                                                                  | f |
|                                                                  | n |
|                                                                  | o |
|                                                                  | d |
|                                                                  | e |
|                                                                  | s |
|                                                                  | … |
+------------------------------------------------------------------+---+
| These components form the *control plane*.                       | T |
|                                                                  | h |
|                                                                  | e |
|                                                                  | s |
|                                                                  | e |
|                                                                  | c |
|                                                                  | o |
|                                                                  | m |
|                                                                  | p |
|                                                                  | o |
|                                                                  | n |
|                                                                  | e |
|                                                                  | n |
|                                                                  | t |
|                                                                  | s |
|                                                                  | f |
|                                                                  | o |
|                                                                  | r |
|                                                                  | m |
|                                                                  | t |
|                                                                  | h |
|                                                                  | e |
|                                                                  | * |
|                                                                  | * |
|                                                                  | c |
|                                                                  | o |
|                                                                  | n |
|                                                                  | t |
|                                                                  | r |
|                                                                  | o |
|                                                                  | l |
|                                                                  | p |
|                                                                  | l |
|                                                                  | a |
|                                                                  | n |
|                                                                  | e |
|                                                                  | * |
|                                                                  | * |
|                                                                  | . |
+------------------------------------------------------------------+---+

Use the ``gloss`` shortcode and add glossary entries for new terms.

Use ``back-ticks`` around file names, directories, and paths
------------------------------------------------------------

+------------------------------------------------------------+---------+
| Do                                                         | Don’t   |
+============================================================+=========+
| Open the ``foo.yaml`` file.                                | Open    |
|                                                            | the     |
|                                                            | foo.yam |
|                                                            | l       |
|                                                            | file.   |
+------------------------------------------------------------+---------+
| Go to the ``/content/en/docs/tasks`` directory.            | Go to   |
|                                                            | the     |
|                                                            | /conten |
|                                                            | t/en/do |
|                                                            | cs/task |
|                                                            | s       |
|                                                            | directo |
|                                                            | ry.     |
+------------------------------------------------------------+---------+
| Open the ``/data/args.yaml`` file.                         | Open    |
|                                                            | the     |
|                                                            | /data/a |
|                                                            | rgs.yam |
|                                                            | l       |
|                                                            | file.   |
+------------------------------------------------------------+---------+

Use ``back-ticks`` around inline code and commands
--------------------------------------------------

+----------------------------------------------------------+-----------+
| Do                                                       | Don’t     |
+==========================================================+===========+
| The ``foo run`` command creates a ``Deployment``.        | The “foo  |
|                                                          | run”      |
|                                                          | command   |
|                                                          | creates a |
|                                                          | ``Deploym |
|                                                          | ent``.    |
+----------------------------------------------------------+-----------+
| For declarative management, use ``foo apply``.           | For       |
|                                                          | declarati |
|                                                          | ve        |
|                                                          | managemen |
|                                                          | t,        |
|                                                          | use “foo  |
|                                                          | apply”.   |
+----------------------------------------------------------+-----------+

Use code-blocks for commands you intend readers to execute. Only use
inline code and commands to mention specific labels, flags, values,
functions, objects, variables, modules, or commands.

Use ``back-ticks`` around object field names
--------------------------------------------

+----------------------------------------------------------------+-----+
| Do                                                             | Don |
|                                                                | ’t  |
+================================================================+=====+
| Set the value of the ``ports`` field in the configuration      | Set |
| file.                                                          | the |
|                                                                | val |
|                                                                | ue  |
|                                                                | of  |
|                                                                | the |
|                                                                | “po |
|                                                                | rts |
|                                                                | ”   |
|                                                                | fie |
|                                                                | ld  |
|                                                                | in  |
|                                                                | the |
|                                                                | con |
|                                                                | fig |
|                                                                | ura |
|                                                                | tio |
|                                                                | n   |
|                                                                | fil |
|                                                                | e.  |
+----------------------------------------------------------------+-----+
| The value of the ``rule`` field is a ``Rule`` object.          | The |
|                                                                | val |
|                                                                | ue  |
|                                                                | of  |
|                                                                | the |
|                                                                | “ru |
|                                                                | le” |
|                                                                | fie |
|                                                                | ld  |
|                                                                | is  |
|                                                                | a   |
|                                                                | ``R |
|                                                                | ule |
|                                                                | ``  |
|                                                                | obj |
|                                                                | ect |
|                                                                | .   |
+----------------------------------------------------------------+-----+
