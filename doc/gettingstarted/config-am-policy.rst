.. _config-am-policy-gs-ug:

======================================
Configure the Attribute Mapping Policy
======================================

The |amp| is a XML or YAML-formatted policy for managing the mapping of SAML
attributes to Rackspace required roles and permissions.

A default |amp| is provided when your |idp| is created. This policy shows the
default attributes that are required for users logging in to Rackspace, as
shown in the following example.

**Default Attribute Mapping Policy**

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

    mapping:
      rules:
        - local:
            user:
              domain: "{D}"
              name: "{D}"
              email: "{D}"
              roles: "{D}"
              expire: "{D}"
      version: "RAX-1"


The default |amp| **must** be customized to specific values before your users
log in or are able to use Rackspace products and services. For more information
on Attribute Mapping, see :ref:`config-attribute-mapping-ug`. To see examples
for specific third-party providers, see :ref:`index-configuring-3p-saml-ug`.

|ampref|
