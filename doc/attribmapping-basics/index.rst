.. _attribmapping-basics-ug:

========================
Attribute Mapping Basics
========================

.. Define |product name| in conf.py

Attribute Mapping Policies are YAML-formatted files that provide a means
for SAML attributes to be mapped to Rackspace required fields such as roles
and account permissions.

An |amp| is required for every |idp| you create.

Attribute Mapping Policies are composed of one or more **rules**. These
**rules** assign **local** values (attached to the user once they log in to
Rackspace), based on explicit or **remote** values (in the SAML exchange from
your third party provider).

Common use cases for an |amp| include:

- Assigning roles to a user based on a SAML attribute (such as *groups*
  provided by Active Directory Federation Services)
- Identifying Rackspace required values like *email address* when it is not
  stored in the standard SAML attribute location
- Setting the expiration time that will require users to re-authenticate

To customize your |amp|, review the sections below for **required** and
**product specific** guidance. You can also consult the complete TODO LINK TO ATTRIBUTE MAPPING REFERENCE HERE for more detailed functionality and examples.

.. toctree::
   :maxdepth: 1

   required-mapping.rst
   rscloud-mapping.rst
   faws-mapping.rst
   full-roles.rst
..   mgcp-mapping.rst

