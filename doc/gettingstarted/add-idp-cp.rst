.. _add-idp-cp-gs-ug:

===================================================
Add an Identity Provider by using the Control Panel
===================================================

Each |idp| should have a unique **Description** and **Email Domain**. The
following table provides descriptions of these parameters:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Item
     - Description
   * - Description
     - Provides a description for your |idp|. This description appears in
       lists and in other areas in the Control Panel.
   * - Email Domains
     - Provides a valid email domain, such as *mycompany.com*. Users provide
       their email address during the federated login process, and their email
       domain is used to identify the |idp| to which they are redirected to
       complete the login process.

Add an |idp|
------------

Cloud customers, use the following steps to add an |idp|:

1. Log in to the `Rackspace Customer Portal <https://login.rackspace.com>`_.

2. In the upper right area of the navigation bar, select
   **Account > User Management** from the drop-down menu. Alternately, browse
   directly to the `User Management <https://account.rackspace.com/users>`_
   page in the Rackspace Account Management Control Panel.

3. In the sub-navigation bar, select **Identity Federation**.

4. Click **Add Identity Provider**.

5. Provide a short description of the |idp| for organizational purposes.The
   The name used here is displayed to users when logging in using federation.

6. Click **Add Domain** and enter the email domain users should authenticate
   with.Click **Add**.

7. Within the **SAML Metadata** section locate and click the **No file chosen**
   button. Choose the metadata file you downloaded from your |idp|. The
   :ref:`Okta metadata file<okta-metadata>` is an example of this file.

After your |idp| metadata is successfully uploaded, you are now ready to begin
:ref:`mapping policy attributes<config-attribute-mapping-ug>`.
