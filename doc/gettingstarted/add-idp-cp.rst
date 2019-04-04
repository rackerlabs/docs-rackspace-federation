.. _add-idp-cp-gs-ug:

=============================================
Add an Identity Provider in the Control Panel
=============================================

To add an |idp|, log in to the Rackspace Control Panel by browsing to
`https://login.rackspace.com <https://login.rackspace.com>`_.

1. In the upper right area of the navigation bar select **Account** then
   **User Management** from the dropdown menu. Alternately, browse
   directly to the `User Management <https://account.rackspace.com/users>`_ page
   in the Rackspace Account Management Control Panel.

2. Choose the **Identity Federation** tab at the top of the page.

3. Click the **Add Identity Provider** button.

Basic information
~~~~~~~~~~~~~~~~~

Each |idp| should have a unique **Description** and **Login Domain**.  The
following table provides descriptions of these items:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Item
     - Description
   * - Description
     - Provide a description for your |idp|. This description is displayed in
       lists and in other areas in the Control Panel.
   * - Login Domain
     - Provide a valid email domain, such as *mycompany.com*. Users provide
       their email address during the federated login process, and their email
       domain is used to identify the |idp| to which they are redirected to
       complete logging in.

Uploading SAML metadata
~~~~~~~~~~~~~~~~~~~~~~~

Next, you need to upload an XML file containing the required metadata to
complete the setup of your |idp|. Most identity systems have a method for
generating the metadata file either automatically or after some basic
configuration has been completed.

For general and provider-specific guidance on configuring your identity system
and retrieving your metadata XML file, see the section
:ref:`index-configuring-3p-saml-ug`.

After your XML file is attached, click **Create Identity Provider** to complete
the process.
