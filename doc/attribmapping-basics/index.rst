.. _attribmapping-basics-ug:

========================
Attribute mapping basics
========================

Attribute Mapping policies are YAML-formatted files that provide a means
to map SAML attributes to Rackspace-required fields, such as roles and account
permissions.

An |amp| is required for every |idp| that you create.

Attribute Mapping policies are composed of one or more rules. These rules
assign local values, which are attached to the user once they log in to
Rackspace, based on explicit or remote values in the SAML exchange from your
third-party provider.

Following are some common use cases for an |amp|:

- Assigning roles to a user based on a SAML attribute (such as *groups*
  provided by Active Directory Federation Services)
- Identifying Rackspace-required values like *email address* when it is not
  stored in the standard SAML attribute location
- Setting the expiration time that will require users to re-authenticate

To customize your |amp|, review the sections below for required and
product-specific guidance. |ampref|

.. toctree::
   :maxdepth: 1

   required-mapping.rst
   rscloud-mapping.rst
   faws-mapping.rst
   full-roles.rst
..   mgcp-mapping.rst

