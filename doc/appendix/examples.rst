==========================
Attribute Mapping Examples
==========================

The examples in this section provide insight into more complex attribute
mapping scenarios.

Working with defaults
=====================

.. code-block:: yaml

   1 <?xml version="1.0" encoding="UTF-8"?>
   2 <saml2p:Response xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
   3                  xmlns:xs="http://www.w3.org/2001/XMLSchema"
   4                  ID="_7fcd6173-e6e0-45a4-a2fd-74a4ef85bf30"
   5                  IssueInstant="2017-11-15T16:19:06.310Z"
   6                  Version="2.0">
   7     <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">http://test.rackspace.com</saml2:Issuer>
   8     <saml2p:Status>
   9         <saml2p:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
  10     </saml2p:Status>
  11     <saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion"
  12                     ID="_406fb7fe-a519-4919-a42c-f67794a670a5"
  13                     IssueInstant="2017-11-15T16:19:06.310Z"
  14                     Version="2.0">
  15       <saml2:Issuer>http://my.rackspace.com</saml2:Issuer>
  16       <saml2:Subject>
  17         <saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">john.doe</saml2:NameID>
  18         <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
  19             <saml2:SubjectConfirmationData NotOnOrAfter="2017-11-17T16:19:06.298Z"/>
  20         </saml2:SubjectConfirmation>
  21       </saml2:Subject>
  22       <saml2:AuthnStatement AuthnInstant="2017-11-15T16:19:04.055Z">
  23         <saml2:AuthnContext>
  24             <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  25             </saml2:AuthnContextClassRef>
  26         </saml2:AuthnContext>
  27       </saml2:AuthnStatement>
  28       <saml2:AttributeStatement>
  29         <saml2:Attribute Name="roles">
  30             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">nova:admin</saml2:AttributeValue>
  31         </saml2:Attribute>
  32         <saml2:Attribute Name="domain">
  33             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">323676</saml2:AttributeValue>
  34         </saml2:Attribute>
  35         <saml2:Attribute Name="email">
  36             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">no-reply@rackspace.com</saml2:AttributeValue>
  37         </saml2:Attribute>
  38         <saml2:Attribute Name="bar">
  39             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">BAR!</saml2:AttributeValue>
  40         </saml2:Attribute>
  41         <saml2:Attribute Name="FirstName">
  42             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">John</saml2:AttributeValue>
  43         </saml2:Attribute>
  44         <saml2:Attribute Name="LastName">
  45             <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string">Doe</saml2:AttributeValue>
  46         </saml2:Attribute>
  47       </saml2:AttributeStatement>
  48    </saml2:Assertion>
  49 </saml2p:Response>



Default mapping:

XML Example:

.. code-block:: xml

  1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  2 <rules>
  3 <rule>
  4 <local>
  5 <user>
  6 <domain value="{D}"/>
  7 <name value="{D}"/>
  8 <email value="{D}"/>
  9 <roles value="{D}"/>
  10 <expire value="{D}"/>
  11 </user>
  12 </local>
  13 </rule>
  14 </rules>
  15 </mapping>

YAML Example:

.. code-block:: yaml

   1 mapping:
   2   version: RAX-1
   3   rules:
   4   - local:
   5       user:
   6         domain: "{D}"
   7         name:   "{D}"
   8         email:  "{D}"
   9         roles:  "{D}"
  10         expire: "{D}"


Resulting attributes:

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

XML Example:

.. code-block:: xml

  1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  2 <rules>
  3 <rule>
  4 <local>
  5 <user>
  6 <domain value="{D}"/>
  7 <name value="{D}"/>
  8 <email value="{D(name)}@rackspace.com"/>
  9 <roles value="{D}"/>
  10 <expire value="{D}"/>
  11 </user>
  12 </local>
  13 </rule>
  14 </rules>
  15</mapping>

YAML Example:

.. code-block:: yaml

   1 mapping:
   2   version: RAX-1
   3   rules:
   4   - local:
   5       user:
   6         domain: "{D}"
   7         name: "{D}"
   8         email: "{D(name)}@rackspace.com"
   9         roles: "{D}"
  10         expire: "{D}"

Resulting attributes:

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

XML Example :

.. code-block:: xml

  1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  2 <rules>
  3 <rule>
  4 <local>
  5 <user>
  6 <domain value="{D}"/>
  7 <name value="{D}"/>
  8 <email value="{D(name)} <{D(name)}@{D(domain)}.rackspace.com>"/>
  9 <roles value="{D}"/>
  10 <expire value="{D}"/>
  11 </user>
  12 </local>
  13 </rule>
  14 </rules>
  15 </mapping>

YAML Example :

.. code-block:: yaml

   1 mapping:
   2   version: RAX-1
   3   rules:
   4   - local:
   5       user:
   6         domain: "{D}"
   7         name: "{D}"
   8         email: "{D(name)} <{D(name)}@{D(domain)}.rackspace.com>"
   9         roles: "{D}"
  10         expire: "{D}"

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

XML Example:

.. code-block:: xml

  1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  2 <rules>
  3 <rule>
  4 <local>
  5 <user>
  6 <domain value="{D}"/>
  7 <name value="{D}"/>
  8 <email value="{At(FirstName)} {At(LastName)}<{D(name)}@{D(domain)}.rackspace.com>"/>
  9 <roles value="{D}"/>
  10 <expire value="{D}"/>
  11 </user>
  12 </local>
  13 </rule>
  14 </rules>
  15 </mapping>

YAML Example:

.. code-block:: yaml

   1 mapping:
   2   version: RAX-1
   3   rules:
   4   - local:
   5       user:
   6         domain: "{D}"
   7         name: "{D}"
   8         email: "{At(FirstName)} {At(LastName)} <{D(name)}@{D(domain)}.rackspace.com>"
   9         roles: "{D}"
  10         expire: "{D}"


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
