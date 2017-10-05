.. _managing-idp-ug:

===========================
Managing Identity Providers
===========================

.. note::

    It is recommended to use the Control Panel to mange Identity Providers, but
    advanced users may choose to use the Rackspace Identity API instead.

    To use the API to create and manage your Identity Providers, see the
    `Identity Providers <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/identity-provider-operations/>`_
    section of the `Rackspace Identity API Reference
    <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/>`_.


Basic Tasks
~~~~~~~~~~~

Manage your Identity Providers via the Rackspace Control Panel at
`https://account.rackspace.com/users <https://account.rackspace.com/users>`_.
You can take actions to manage your |idp| through either the list of
Identity Providers on the **Identity Federation** page, or through
the **Actions** menu on the details page for any |idp|.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Action
     - Description
   * - **Enable**
     - Enable a currently disabled |idp|. Users will be able to log in through
       |product name| and use products and services.
   * - **Disable**
     - Disable a currently enabled |idp|. Users will NOT be able to log in
       through |product name|\, and any currently logged in users will
       logged out at the next opportunity.
   * - **Remove**
     - Removes an |idp|. Users will NOT be able to login using this |idp|,
       and any currently logged in users will be logged out at the next
       opportunity.

Updating Metadata and Certificates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To update key details about your |idp|, use the  **Update Metadata**
button in the Control Panel. The following will be extracted and updated
from your metadata:

- the security certificate
- issuer value

Updating your metadata will not change the **Login Domain** or **Description**
you have provided.

Updating the |amp|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To update the |amp| for your |idp|, upload a new YAML file using the **Update
Policy File** link in the |idp| details page.

The file must be valid YAML, and the file extension should be ``.yml`` or
``.yaml``. To validate your YAML, you can use one of several YAML validation
libraries or websites.

