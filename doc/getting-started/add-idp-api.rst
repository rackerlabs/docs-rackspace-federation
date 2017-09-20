.. _add-idp-api-gs-ug:

===========
Adding an Identity Provider via API
===========


It is recommended to use the Control Panel to add Identity Providers 
(see :ref:`add-idp-cp-gs-ug`), but advanced users may choose to use the
Rackspace Identity API instead.

To use the API to create and manage your Identity Providers, see the
`Identity Providers <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/identity-provider-operations/>`_
section of the `Rackspace Identity API Reference <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/>`_.

Some important things to consider when creating an |idp| via the API:

- The |idp| must first be created using a metadata XML file
- After the |idp| is created, you can then update the **Description** and **Name** values.
- The **Name** value is the equivalent of the **Login Domain** referenced in :ref:`add-idp-cp-gs-ug`.    
