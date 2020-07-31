.. _index-manage-idp:

=========================
Manage Identity Providers
=========================

.. note::

    Rackspace recommends that you use the Control Panel to mange Identity
    Providers, but advanced users might choose to use the Identity
    API instead.

    To use the API to create and manage your Identity Providers, see the
    `Identity Providers <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/identity-provider-operations/>`_
    section of the `Identity API Reference
    <https://developer.rackspace.com/docs/cloud-identity/v2/api-reference/>`_.


Basic tasks
~~~~~~~~~~~

Manage your Identity Providers by using the Rackspace Control Panel at
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
     - Enables a currently disabled |idp|. Users will be able to log in through
       |product name| to use products and services.
   * - **Disable**
     - Disables a currently enabled |idp|. Users will NOT be able to log in
       through |product name|, and any currently logged in users will be
       logged out at the next opportunity.
   * - **Remove**
     - Removes an |idp|. Users will NOT be able to login using this |idp|,
       and any currently logged in users will be logged out at the next
       opportunity.

Update metadata and certificates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To update key details about your |idp|, click **Update Metadata** in the
Control Panel. The following items are extracted and updated from your
metadata:

- the security certificate
- the issuer value

Updating your metadata does not change the **Login Domain** or **Description**
that you have provided.

Update the |amp|
~~~~~~~~~~~~~~~~

To update the |amp| for your |idp|, upload a new XML or YAML file by using the
**Update Policy File** link in the |idp| details page.

The file must be valid XML or YAML, and the file extensions should be ``.xml`` or ``.yaml``. To validate your XML or YAML, you can use any XML or YAML validation library or
website.
