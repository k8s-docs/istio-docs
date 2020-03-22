Conditions
===================

This page describes the supported keys and value formats you can use as
conditions in the ``when`` field of an `authorization policy
rule </docs/reference/config/security/authorization-policy/#Rule>`_.

.. warning::

   Unsupported keys and values are silently ignored. {{<
/warning >}}

For more information, refer to the `authorization concept
page </docs/concepts/security/#authorization>`_.

Supported Conditions
--------------------

+--------+------------------+-----------------------------+------------+
| Name   | Description      | Supported Protocols         | Example    |
+========+==================+=============================+============+
| ``requ | HTTP request     | HTTP only                   | ``key: req |
| est.he | headers. The     |                             | uest.heade |
| aders` | actual header    |                             | rs[User-Ag |
| `      | name is          |                             | ent]``\ \  |
|        | surrounded by    |                             | ``values:  |
|        | brackets         |                             | ["Mozilla/ |
|        |                  |                             | *"]``      |
+--------+------------------+-----------------------------+------------+
| ``sour | Source workload  | HTTP and TCP                | ``key: sou |
| ce.ip` | instance IP      |                             | rce.ip``\  |
| `      | address,         |                             | \ ``values |
|        | supports single  |                             | : ["10.1.2 |
|        | IP or CIDR       |                             | .3"]``     |
+--------+------------------+-----------------------------+------------+
| ``sour | Source workload  | HTTP and TCP                | ``key: sou |
| ce.nam | instance         |                             | rce.namesp |
| espace | namespace,       |                             | ace``\ \ ` |
| ``     | requires mutual  |                             | `values: [ |
|        | TLS enabled      |                             | "default"] |
|        |                  |                             | ``         |
+--------+------------------+-----------------------------+------------+
| ``sour | The identity of  | HTTP and TCP                | ``key: sou |
| ce.pri | the source       |                             | rce.princi |
| ncipal | workload,        |                             | pal``\ \ ` |
| ``     | requires mutual  |                             | `values: [ |
|        | TLS enabled      |                             | "cluster.l |
|        |                  |                             | ocal/ns/de |
|        |                  |                             | fault/sa/p |
|        |                  |                             | roductpage |
|        |                  |                             | "]``       |
+--------+------------------+-----------------------------+------------+
| ``requ | The              | HTTP only                   | ``key: req |
| est.au | authenticated    |                             | uest.auth. |
| th.pri | principal of the |                             | principal` |
| ncipal | request.         |                             | `\ \ ``val |
| ``     |                  |                             | ues: ["acc |
|        |                  |                             | ounts.my-s |
|        |                  |                             | vc.com/104 |
|        |                  |                             | 958560606" |
|        |                  |                             | ]``        |
+--------+------------------+-----------------------------+------------+
| ``requ | The intended     | HTTP only                   | ``key: req |
| est.au | audience(s) for  |                             | uest.auth. |
| th.aud | this             |                             | audiences` |
| iences | authentication   |                             | `\ \ ``val |
| ``     | information      |                             | ues: ["my- |
|        |                  |                             | svc.com"]` |
|        |                  |                             | `          |
+--------+------------------+-----------------------------+------------+
| ``requ | The authorized   | HTTP only                   | ``key: req |
| est.au | presenter of the |                             | uest.auth. |
| th.pre | credential       |                             | presenter` |
| senter |                  |                             | `\ \ ``val |
| ``     |                  |                             | ues: ["123 |
|        |                  |                             | 456789012. |
|        |                  |                             | my-svc.com |
|        |                  |                             | "]``       |
+--------+------------------+-----------------------------+------------+
| ``requ | Claims from the  | HTTP only                   | ``key: req |
| est.au | origin JWT. The  |                             | uest.auth. |
| th.cla | actual claim     |                             | claims[iss |
| ims``  | name is          |                             | ]``\ \ ``v |
|        | surrounded by    |                             | alues: ["* |
|        | brackets         |                             | @foo.com"] |
|        |                  |                             | ``         |
+--------+------------------+-----------------------------+------------+
| ``dest | Destination      | HTTP and TCP                | ``key: des |
| inatio | workload         |                             | tination.i |
| n.ip`` | instance IP      |                             | p``\ \ ``v |
|        | address,         |                             | alues: ["1 |
|        | supports single  |                             | 0.1.2.3",  |
|        | IP or CIDR       |                             | "10.2.0.0/ |
|        |                  |                             | 16"]``     |
+--------+------------------+-----------------------------+------------+
| ``dest | The recipient    | HTTP and TCP                | ``key: des |
| inatio | port on the      |                             | tination.p |
| n.port | server IP        |                             | ort``\ \ ` |
| ``     | address, must be |                             | `values: [ |
|        | in the range [0, |                             | "80", "443 |
|        | 65535]           |                             | "]``       |
+--------+------------------+-----------------------------+------------+
| ``conn | The server name  | HTTP and TCP                | ``key: con |
| ection | indication,      |                             | nection.sn |
| .sni`` | requires mutual  |                             | i``\ \ ``v |
|        | TLS enabled      |                             | alues: ["w |
|        |                  |                             | ww.example |
|        |                  |                             | .com"]``   |
+--------+------------------+-----------------------------+------------+
| ``expe | Experimental     | HTTP and TCP                | ``key: exp |
| riment | metadata         |                             | erimental. |
| al.env | matching for     |                             | envoy.filt |
| oy.fil | filters, values  |                             | ers.networ |
| ters.* | wrapped in       |                             | k.mysql_pr |
| ``     | ``[]`` are       |                             | oxy[db.tab |
|        | matched as a     |                             | le]``\ \ ` |
|        | list             |                             | `values: [ |
|        |                  |                             | "[update]" |
|        |                  |                             | ]``        |
+--------+------------------+-----------------------------+------------+

.. warning::

   No backward compatibility is guaranteed for the
``experimental.*`` keys. They may be removed at any time, and customers
are advised to use them at their own risk.
