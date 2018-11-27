.. _add-idp-cp-gs-ug:

=============================================
Add an Identity Provider in the Control Panel
=============================================

Use the following steps to add an |idp|:

1. Log in to the `MyRackspace Portal <https://login.rackspace.com>`_.

2. In the upper right area of the navigation bar, select
   **Account > User Management** from the drop-down menu. Alternately, browse
   directly to the `User Management <https://account.rackspace.com/users>`_
   page in the Rackspace Account Management Control Panel.

3. In the subnavigation bar, select **Federation**.

4. Click **Add Identity Provider**.

Basic information
~~~~~~~~~~~~~~~~~

Each |idp| should have a unique **Description** and **Email Domain**. The
following table provides descriptions of these parameters:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Item
     - Description
   * - Description
     - Provide a description for your |idp|. This description appears in
       lists and in other areas in the Control Panel.
   * - Email Domains
     - Provide a valid email domain, such as *mycompany.com*. Users provide
       their email address during the federated login process, and their email
       domain is used to identify the |idp| to which they are redirected to
       complete the login process.

Upload SAML metadata
~~~~~~~~~~~~~~~~~~~~~~~

Next, you need to upload an XML file that contains the required metadata to
complete the setup of your |idp|. Most identity systems have a method for
generating the metadata file either automatically, or after you have completed
some basic configuration.

For general and provider-specific guidance about configuring your identity
system and retrieving your metadata XML file, see the section
:ref:`index-configuring-3p-saml-ug`.

After your XML file is attached, click **Create Identity Provider**.
