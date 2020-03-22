Constraints and properties
==============================

.. warning::

   The constraints and properties in the RBAC policy are
deprecated by the conditions in the ``AuthorizationPolicy``. Please use
the conditions in ``AuthorizationPolicy`` resources, this page is for
reference only and will be removed in the future.

This section contains the supported keys and value formats you can use
as constraints and properties in the service roles and service role
bindings configuration objects. Constraints and properties are extra
conditions you can add as fields in configuration objects with a
``kind:`` value of ``ServiceRole`` and ``ServiceRoleBinding`` to specify
detailed access control requirements.

Specifically, you can use constraints to specify extra conditions in the
access rule field of a service role. You can use properties to specify
extra conditions in the subject field of a service role binding. Istio
supports all keys listed on this page for the HTTP protocol but supports
only some for the plain TCP protocol.

.. warning::

   Unsupported keys and values will be ignored silently.


For more information, refer to the `authorization concept
page </docs/concepts/security/#authorization>`_.

Supported constraints
---------------------

The following table lists the currently supported keys for the
``constraints`` field:

+----+-----------+-------------------------+-----------+--------------+
| Na | Descripti | Supported for TCP       | Key       | Values       |
| me | on        | services                | Example   | Example      |
+====+===========+=========================+===========+==============+
| `` | Destinati | YES                     | ``destina | ``["10.1.2.3 |
| de | on        |                         | tion.ip`` | ", "10.2.0.0 |
| st | workload  |                         |           | /16"]``      |
| in | instance  |                         |           |              |
| at | IP        |                         |           |              |
| io | address,  |                         |           |              |
| n. | supports  |                         |           |              |
| ip | single IP |                         |           |              |
| `` | or CIDR   |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | The       | YES                     | ``destina | ``["80", "44 |
| de | recipient |                         | tion.port | 3"]``        |
| st | port on   |                         | ``        |              |
| in | the       |                         |           |              |
| at | server IP |                         |           |              |
| io | address,  |                         |           |              |
| n. | must be   |                         |           |              |
| po | in the    |                         |           |              |
| rt | range [0, |                         |           |              |
| `` | 65535]    |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | A map of  | YES                     | ``destina | ``["v1", "v2 |
| de | key-value |                         | tion.labe | "]``         |
| st | pairs     |                         | ls[versio |              |
| in | attached  |                         | n]``      |              |
| at | to the    |                         |           |              |
| io | server    |                         |           |              |
| n. | instance  |                         |           |              |
| la |           |                         |           |              |
| be |           |                         |           |              |
| ls |           |                         |           |              |
| `` |           |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | Destinati | YES                     | ``destina | ``["default" |
| de | on        |                         | tion.name | ]``          |
| st | workload  |                         | space``   |              |
| in | instance  |                         |           |              |
| at | namespace |                         |           |              |
| io |           |                         |           |              |
| n. |           |                         |           |              |
| na |           |                         |           |              |
| me |           |                         |           |              |
| sp |           |                         |           |              |
| ac |           |                         |           |              |
| e` |           |                         |           |              |
| `  |           |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | The       | YES                     | ``destina | ``["bookinfo |
| de | identity  |                         | tion.user | -productpage |
| st | of the    |                         | ``        | "]``         |
| in | destinati |                         |           |              |
| at | on        |                         |           |              |
| io | workload  |                         |           |              |
| n. |           |                         |           |              |
| us |           |                         |           |              |
| er |           |                         |           |              |
| `` |           |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | Experimen | YES                     | ``experim | ``["[update] |
| ex | tal       |                         | ental.env | "]``         |
| pe | metadata  |                         | oy.filter |              |
| ri | matching  |                         | s.network |              |
| me | for       |                         | .mysql_pr |              |
| nt | filters,  |                         | oxy[db.ta |              |
| al | values    |                         | ble]``    |              |
| .e | wrapped   |                         |           |              |
| nv | in ``[]`` |                         |           |              |
| oy | are       |                         |           |              |
| .f | matched   |                         |           |              |
| il | as a list |                         |           |              |
| te |           |                         |           |              |
| rs |           |                         |           |              |
| .* |           |                         |           |              |
| `` |           |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+
| `` | HTTP      | NO                      | ``request | ``["abc123"] |
| re | request   |                         | .headers[ | ``           |
| qu | headers,  |                         | X-Custom- |              |
| es | The       |                         | Token]``  |              |
| t. | actual    |                         |           |              |
| he | header    |                         |           |              |
| ad | name is   |                         |           |              |
| er | surrounde |                         |           |              |
| s` | d         |                         |           |              |
| `  | by        |                         |           |              |
|    | brackets  |                         |           |              |
+----+-----------+-------------------------+-----------+--------------+

.. warning::

   Note that no backward compatibility is guaranteed for
the ``experimental.*`` keys. They may be removed at any time, and
customers are advised to use them at their own risk.

Supported properties
--------------------

The following table lists the currently supported keys for the
``properties`` field:

+----+-----------+-------------------------+-----------+-------------+
| Na | Descripti | Supported for TCP       | Key       | Value       |
| me | on        | services                | Example   | Example     |
+====+===========+=========================+===========+=============+
| `` | Source    | YES                     | ``source. | ``"10.1.2.3 |
| so | workload  |                         | ip``      | "``         |
| ur | instance  |                         |           |             |
| ce | IP        |                         |           |             |
| .i | address,  |                         |           |             |
| p` | supports  |                         |           |             |
| `  | single IP |                         |           |             |
|    | or CIDR   |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | Source    | YES                     | ``source. | ``"default" |
| so | workload  |                         | namespace | ``          |
| ur | instance  |                         | ``        |             |
| ce | namespace |                         |           |             |
| .n |           |                         |           |             |
| am |           |                         |           |             |
| es |           |                         |           |             |
| pa |           |                         |           |             |
| ce |           |                         |           |             |
| `` |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | The       | YES                     | ``source. | ``"cluster. |
| so | identity  |                         | principal | local/ns/de |
| ur | of the    |                         | ``        | fault/sa/pr |
| ce | source    |                         |           | oductpage"` |
| .p | workload  |                         |           | `           |
| ri |           |                         |           |             |
| nc |           |                         |           |             |
| ip |           |                         |           |             |
| al |           |                         |           |             |
| `` |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | HTTP      | NO                      | ``request | ``"Mozilla/ |
| re | request   |                         | .headers[ | *"``        |
| qu | headers.  |                         | User-Agen |             |
| es | The       |                         | t]``      |             |
| t. | actual    |                         |           |             |
| he | header    |                         |           |             |
| ad | name is   |                         |           |             |
| er | surrounde |                         |           |             |
| s` | d         |                         |           |             |
| `  | by        |                         |           |             |
|    | brackets  |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | The       | NO                      | ``request | ``"accounts |
| re | authentic |                         | .auth.pri | .my-svc.com |
| qu | ated      |                         | ncipal``  | /1049585606 |
| es | principal |                         |           | 06"``       |
| t. | of the    |                         |           |             |
| au | request.  |                         |           |             |
| th |           |                         |           |             |
| .p |           |                         |           |             |
| ri |           |                         |           |             |
| nc |           |                         |           |             |
| ip |           |                         |           |             |
| al |           |                         |           |             |
| `` |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | The       | NO                      | ``request | ``"my-svc.c |
| re | intended  |                         | .auth.aud | om"``       |
| qu | audience( |                         | iences``  |             |
| es | s)        |                         |           |             |
| t. | for this  |                         |           |             |
| au | authentic |                         |           |             |
| th | ation     |                         |           |             |
| .a | informati |                         |           |             |
| ud | on        |                         |           |             |
| ie |           |                         |           |             |
| nc |           |                         |           |             |
| es |           |                         |           |             |
| `` |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | The       | NO                      | ``request | ``"12345678 |
| re | authorize |                         | .auth.pre | 9012.my-svc |
| qu | d         |                         | senter``  | .com"``     |
| es | presenter |                         |           |             |
| t. | of the    |                         |           |             |
| au | credentia |                         |           |             |
| th | l         |                         |           |             |
| .p |           |                         |           |             |
| re |           |                         |           |             |
| se |           |                         |           |             |
| nt |           |                         |           |             |
| er |           |                         |           |             |
| `` |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
| `` | Claims    | NO                      | ``request | ``"*@foo.co |
| re | from the  |                         | .auth.cla | m"``        |
| qu | origin    |                         | ims[iss]` |             |
| es | JWT. The  |                         | `         |             |
| t. | actual    |                         |           |             |
| au | claim     |                         |           |             |
| th | name is   |                         |           |             |
| .c | surrounde |                         |           |             |
| la | d         |                         |           |             |
| im | by        |                         |           |             |
| s` | brackets  |                         |           |             |
| `  |           |                         |           |             |
+----+-----------+-------------------------+-----------+-------------+
