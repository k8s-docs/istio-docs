expression language
============================

This page describes how to use the Mixer configuration expression
language (CEXL).

Background
----------

Mixer configuration uses an expression language (CEXL) to specify match
expressions and `mapping
expressions </docs/reference/config/policy-and-telemetry/mixer-overview/#attribute-expressions>`_.
CEXL expressions map a set of typed
`attributes </docs/reference/config/policy-and-telemetry/mixer-overview/#attributes>`_
and constants to a typed
`value <https://github.com/istio/api/blob/%7B%7B%3C%20source_branch_name%20%3E%7D%7D/policy/v1beta1/value_type.proto>`_.

Syntax
------

CEXL accepts a subset of `Go
expressions <https://golang.org/ref/spec#Expressions>`_, which defines
the syntax. CEXL implements a subset of the Go operators that constrains
the set of accepted Go expressions. CEXL also supports arbitrary
parenthesization.

Functions
---------

CEXL supports the following functions.

+-------------------------+---------------+----------+----------------+
| Operator/Function       | Definition    | Example  | Description    |
+=========================+===============+==========+================+
| ``==``                  | Equals        | ``reques |                |
|                         |               | t.size = |                |
|                         |               | = 200``  |                |
+-------------------------+---------------+----------+----------------+
| ``!=``                  | Not Equals    | ``reques |                |
|                         |               | t.auth.p |                |
|                         |               | rincipal |                |
|                         |               |  != "adm |                |
|                         |               | in"``    |                |
+-------------------------+---------------+----------+----------------+
| \|\|                    | Logical OR    | ``(reque |                |
|                         |               | st.size  |                |
|                         |               | == 200)` |                |
|                         |               | `        |                |
|                         |               | \|\|     |                |
|                         |               | ``(reque |                |
|                         |               | st.auth. |                |
|                         |               | principa |                |
|                         |               | l == "ad |                |
|                         |               | min")``  |                |
+-------------------------+---------------+----------+----------------+
| ``&&``                  | Logical AND   | ``(reque |                |
|                         |               | st.size  |                |
|                         |               | == 200)  |                |
|                         |               | && (requ |                |
|                         |               | est.auth |                |
|                         |               | .princip |                |
|                         |               | al == "a |                |
|                         |               | dmin")`` |                |
+-------------------------+---------------+----------+----------------+
| ``[ ]``                 | Map Access    | ``reques |                |
|                         |               | t.header |                |
|                         |               | s["x-req |                |
|                         |               | uest-id" |                |
|                         |               | ]``      |                |
+-------------------------+---------------+----------+----------------+
| ``+``                   | Add           | ``reques |                |
|                         |               | t.host + |                |
|                         |               |  request |                |
|                         |               | .path``  |                |
+-------------------------+---------------+----------+----------------+
| ``>``                   | Greater Than  | ``respon |                |
|                         |               | se.code  |                |
|                         |               | > 200``  |                |
+-------------------------+---------------+----------+----------------+
| ``>=``                  | Greater Than  | ``reques |                |
|                         | or Equal to   | t.size > |                |
|                         |               | = 100``  |                |
+-------------------------+---------------+----------+----------------+
| ``<``                   | Less Than     | ``respon |                |
|                         |               | se.code  |                |
|                         |               | < 500``  |                |
+-------------------------+---------------+----------+----------------+
| ``<=``                  | Less Than or  | ``reques |                |
|                         | Equal to      | t.size < |                |
|                         |               | = 100``  |                |
+-------------------------+---------------+----------+----------------+
| \|                      | First non     | ``source |                |
|                         | empty         | .labels[ |                |
|                         |               | "app"]`` |                |
|                         |               | \|       |                |
|                         |               | ``source |                |
|                         |               | .labels[ |                |
|                         |               | "svc"]`` |                |
|                         |               | \|       |                |
|                         |               | ``"unkno |                |
|                         |               | wn"``    |                |
+-------------------------+---------------+----------+----------------+
| ``match``               | Glob match    | ``match( | Matches prefix |
|                         |               | destinat | or suffix      |
|                         |               | ion.serv | based on the   |
|                         |               | ice, "*. | location of    |
|                         |               | ns1.svc. | ``*``          |
|                         |               | cluster. |                |
|                         |               | local")` |                |
|                         |               | `        |                |
+-------------------------+---------------+----------+----------------+
| ``email``               | Convert a     | ``email( | Use the        |
|                         | textual       | "awesome | ``email``      |
|                         | e-mail into   | @istio.i | function to    |
|                         | the           | o")``    | create an      |
|                         | ``EMAIL_ADDRE |          | ``EMAIL_ADDRES |
|                         | SS``          |          | S``            |
|                         | type          |          | literal.       |
+-------------------------+---------------+----------+----------------+
| ``dnsName``             | Convert a     | ``dnsNam | Use the        |
|                         | textual DNS   | e("www.i | ``dnsName``    |
|                         | name into the | stio.io" | function to    |
|                         | ``DNS_NAME``  | )``      | create a       |
|                         | type          |          | ``DNS_NAME``   |
|                         |               |          | literal.       |
+-------------------------+---------------+----------+----------------+
| ``ip``                  | Convert a     | ``source | Use the ``ip`` |
|                         | textual IPv4  | .ip == i | function to    |
|                         | address into  | p("10.11 | create an      |
|                         | the           | .12.13") | ``IP_ADDRESS`` |
|                         | ``IP_ADDRESS` | ``       | literal.       |
|                         | `             |          |                |
|                         | type          |          |                |
+-------------------------+---------------+----------+----------------+
| ``timestamp``           | Convert a     | ``timest | Use the        |
|                         | textual       | amp("201 | ``timestamp``  |
|                         | timestamp in  | 5-01-02T | function to    |
|                         | RFC 3339      | 15:04:35 | create a       |
|                         | format into   | Z")``    | ``TIMESTAMP``  |
|                         | the           |          | literal.       |
|                         | ``TIMESTAMP`` |          |                |
|                         | type          |          |                |
+-------------------------+---------------+----------+----------------+
| ``uri``                 | Convert a     | ``uri("h | Use the        |
|                         | textual URI   | ttp://is | ``uri``        |
|                         | into the      | tio.io") | function to    |
|                         | ``URI`` type  | ``       | create a       |
|                         |               |          | ``URI``        |
|                         |               |          | literal.       |
+-------------------------+---------------+----------+----------------+
| ``.matches``            | Regular       | ``"svc.* | Matches        |
|                         | expression    | ".matche | ``destination. |
|                         | match         | s(destin | service``      |
|                         |               | ation.se | against        |
|                         |               | rvice)`` | regular        |
|                         |               |          | expression     |
|                         |               |          | pattern        |
|                         |               |          | ``"svc.*"``.   |
+-------------------------+---------------+----------+----------------+
| ``.startsWith``         | string prefix | ``destin | Checks whether |
|                         | match         | ation.se | ``destination. |
|                         |               | rvice.st | service``      |
|                         |               | artsWith | starts with    |
|                         |               | ("acme") | ``"acme"``.    |
|                         |               | ``       |                |
+-------------------------+---------------+----------+----------------+
| ``.endsWith``           | string        | ``destin | Checks whether |
|                         | postfix match | ation.se | ``destination. |
|                         |               | rvice.en | service``      |
|                         |               | dsWith(" | ends with      |
|                         |               | acme")`` | ``"acme"``.    |
+-------------------------+---------------+----------+----------------+
| ``emptyStringMap``      | Create an     | ``reques | Use            |
|                         | empty string  | t.header | ``emptyStringM |
|                         | map           | s``      | ap``           |
|                         |               | \|       | to create an   |
|                         |               | ``emptyS | empty string   |
|                         |               | tringMap | map for        |
|                         |               | ()``     | default value  |
|                         |               |          | of             |
|                         |               |          | ``request.head |
|                         |               |          | ers``.         |
+-------------------------+---------------+----------+----------------+
| ``conditional``         | Simulate      | ``condit | Returns        |
|                         | ternary       | ional((c | ``"client"``   |
|                         | operator      | ontext.r | if report kind |
|                         |               | eporter. | is             |
|                         |               | kind``   | ``outbound``   |
|                         |               | \|       | otherwise      |
|                         |               | ``"inbou | returns        |
|                         |               | nd") ==  | ``"server"``.  |
|                         |               | "outboun |                |
|                         |               | d", "cli |                |
|                         |               | ent", "s |                |
|                         |               | erver")` |                |
|                         |               | `        |                |
+-------------------------+---------------+----------+----------------+
| ``toLower``             | Convert a     | ``toLowe | Returns        |
|                         | string to     | r("User- | ``"user-agent" |
|                         | lowercase     | Agent")` | ``.            |
|                         | letters       | `        |                |
+-------------------------+---------------+----------+----------------+
| ``size``                | Length of a   | ``size(" | Returns 5      |
|                         | string        | admin")` |                |
|                         |               | `        |                |
+-------------------------+---------------+----------+----------------+

Type checking
-------------

CEXL variables are attributes from the typed `attribute
vocabulary </docs/reference/config/policy-and-telemetry/attribute-vocabulary/>`_,
constants are implicitly typed and, functions are explicitly typed.

Mixer validates a CEXL expression and resolves it to a type during
configuration validation. Selectors must resolve to a boolean value and
mapping expressions must resolve to the type they are mapping into.
Configuration validation fails if a selector fails to resolve to a
boolean or if a mapping expression resolves to an incorrect type.

For example, if an operator specifies a *string* label as
``request.size | 200``, validation fails because the expression resolves
to an integer.

Missing attributes
------------------

If an expression uses an attribute that is not available during request
processing, the expression evaluation fails. Use the ``|`` operator to
provide a default value if an attribute may be missing.

For example, the expression ``request.auth.principal == "user1"`` fails
evaluation if the ``request.auth.principal`` attribute is missing. The
``|`` (OR) operator addresses the problem:
``(request.auth.principal | "nobody" ) == "user1"``.

Examples
--------

+----------------------+------------------------+----------------------+
| Expression           | Return Type            | Description          |
+======================+========================+======================+
| ``request.size`` \|  | **int**                | ``request.size`` if  |
| 200                  |                        | available, otherwise |
|                      |                        | 200.                 |
+----------------------+------------------------+----------------------+
| ``request.headers["x | **boolean**            |                      |
| -forwarded-host"] == |                        |                      |
|  "myhost"``          |                        |                      |
+----------------------+------------------------+----------------------+
| ``(request.headers[" | **boolean**            | True if the user is  |
| x-user-group"] == "a |                        | admin or in the      |
| dmin")``             |                        | admin group.         |
| \|\|                 |                        |                      |
| ``(request.auth.prin |                        |                      |
| cipal == "admin")``  |                        |                      |
+----------------------+------------------------+----------------------+
| ``(request.auth.prin | **boolean**            | True if              |
| cipal``              |                        | ``request.auth.princ |
| \|                   |                        | ipal``               |
| ``"nobody" ) == "use |                        | is “user1”, The      |
| r1"``                |                        | expression will not  |
|                      |                        | error out if         |
|                      |                        | ``request.auth.princ |
|                      |                        | ipal``               |
|                      |                        | is missing.          |
+----------------------+------------------------+----------------------+
| ``source.labels["app | **boolean**            | True if app label is |
| "]=="reviews" && sou |                        | reviews and version  |
| rce.labels["version" |                        | label is v3, false   |
| ]=="v3"``            |                        | otherwise.           |
+----------------------+------------------------+----------------------+
