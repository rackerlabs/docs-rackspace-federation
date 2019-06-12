.. _config-am-policy-gs-ug:

===========================
Configure attribute mapping
===========================

The |amp| is a YAML-formatted policy for managing the mapping of SAML
attributes to Rackspace required roles and permissions.

A default |amp| is provided when your |idp| is created. This policy shows the
default attributes that are required for users logging in to Rackspace, as
shown in the following example.

**Default Attribute Mapping Policy**

   .. code-block:: yaml

      mapping:
          rules:
              local:
                  user:
                      domain: {D}
                      name: {D}
                      email: {D}
                      roles: {D}
                      expire: {D}
          version: "RAX-1"


The default |amp| **must** be customized to specific values before your users
log in or are able to use Rackspace products and services. For more information
on attribute mapping, see :ref:`config-attribute-mapping-ug`. To see examples for
specific third-party providers, see :ref:`index-configuring-3p-saml-ug`.

|ampref|
