.. _config-am-policy-gs-ug:

=========
Configuring Attribute Mapping
=========

The |amp| is a YAML-formatted policy for managing the mapping of SAML attributes
to Rackspace required roles and permissions. 

A default |amp| is provided when your |idp| is created. This policy
shows the default attributes that are required for users logging in to Rackspace.

**Default Attribute Mapping Policy**

  .. code::
    
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



The default policy *must* be customized to specific values
before your users log in or are able to use Rackspace
products and services. For more information on Attribute Mapping 
see :ref:`attribmapping-basics-ug`, or :ref:`index-configuring-3p-saml-ug`
for examples for specific 3rd party providers. 
