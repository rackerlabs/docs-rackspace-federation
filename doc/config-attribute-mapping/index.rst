.. _config-attribute-mapping-ug:

===========================
Configure Attribute Mapping
===========================

Attribute Mapping Policies are YAML or XML-formatted files that are used
to map SAML attributes to Rackspace-required fields, such as
roles and account permissions.

An |amp| is required for every |idp| that you create.

Attribute Mapping Policies are composed of one or more rules. These rules
assign local values that are attached to the user when they log in to
Rackspace, based on explicit or remote values in the SAML exchange from your
third-party provider.

An |amp| has the following common use cases:

- Assign roles to a user based on a SAML attribute (such as *groups*
  provided by Active Directory Federation Services (ADFS))
- Identify Rackspace-required values like *email address* when they are not
  stored in the standard SAML attribute location
- Set the expiration time after which users are required to re-authenticate

The deep-dive |amp| reference guide provides detailed functionality and
examples for the |amp| language. Use the reference guide to construct policies
that use features like conditional matching, substitutions, and other
scenarios.

The |amp| reference can be found at
`<https://rackerlabs.github.io/attributeMapping/docs-1.3.0/>`_.

To customize your |amp|, review the following sections for required and
product-specific guidance. |ampref|

.. toctree::
   :maxdepth: 1

   required-mapping.rst
   rscloud-mapping.rst
   faws-mapping.rst
   full-roles.rst
..   mgcp-mapping.rst
