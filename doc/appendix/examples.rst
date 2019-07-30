==========================
Attribute Mapping Examples
==========================

.. See index.rst for info on attribmap, saml, and map directives.

Working with defaults
=====================


.. highlight:: xml
   :linenothreshold: 5


.. literalinclude::  ../../../../core/target/map-results/defaults/sample_assert.xml/sample_assert.xml
   :linenos: 


.. highlight:: yaml
   :linenothreshold: 5


.. literalinclude::  ../../../../core/src/test/resources/tests/mapping-tests/defaults/maps/defaults.yaml
   :linenos: 

Resulting Attributes:

+--------+--------------------------+
| domain | 323676                   |
+--------+--------------------------+
| name   | john.doe                 |
+--------+--------------------------+
| email  | no-reply@rackspace.com   |
+--------+--------------------------+
| roles  | - nova:admin             |
+--------+--------------------------+
| expire | 2017-11-17T16:19:06.298Z |
+--------+--------------------------+

Accessing default from a different field:
-----------------------------------------


.. highlight:: yaml
   :linenothreshold: 5


.. literalinclude::  ../../../../core/src/test/resources/tests/mapping-tests/defaults2/maps/defaults2.yaml
   :linenos: 

Resulting Attributes:

+--------+--------------------------+
| domain | 323676                   |
+--------+--------------------------+
| name   | john.doe                 |
+--------+--------------------------+
| email  | john.doe@rackspace.com   |
+--------+--------------------------+
| roles  | - nova:admin             |
+--------+--------------------------+
| expire | 2017-11-17T16:19:06.298Z |
+--------+--------------------------+

More complex example with multiple substitutions
------------------------------------------------


.. highlight:: yaml
   :linenothreshold: 5


.. literalinclude::  ../../../../core/src/test/resources/tests/mapping-tests/defaults3/maps/defaults3.yaml
   :linenos: 

Resulting Attributes:

+--------+------------------------------------------+
| domain | 323676                                   |
+--------+------------------------------------------+
| name   | john.doe                                 |
+--------+------------------------------------------+
| email  | john.doe <john.doe@323676.rackspace.com> |
+--------+------------------------------------------+
| roles  | - nova:admin                             |
+--------+------------------------------------------+
| expire | 2017-11-17T16:19:06.298Z                 |
+--------+------------------------------------------+

Mixing in non-default attributes
--------------------------------


.. highlight:: yaml
   :linenothreshold: 5


.. literalinclude::  ../../../../core/src/test/resources/tests/mapping-tests/defaults4/maps/defaults4-s.yaml
   :linenos: 

Resulting Attributes:

+--------+------------------------------------------+
| domain | 323676                                   |
+--------+------------------------------------------+
| name   | john.doe                                 |
+--------+------------------------------------------+
| email  | John Doe <john.doe@323676.rackspace.com> |
+--------+------------------------------------------+
| roles  | - nova:admin                             |
+--------+------------------------------------------+
| expire | 2017-11-17T16:19:06.298Z                 |
+--------+------------------------------------------+

Working with expiration
=======================

Working with lists
==================

Black lists
-----------

White lists
-----------
