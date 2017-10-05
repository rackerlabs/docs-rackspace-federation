.. _troubleshooting-ug:

===============
Troubleshooting
===============

If you encounter issues when working with |service|, use the information
in this section to help you troubleshoot.


Problems Creating an Identity Provider
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ensure that:

- Your metadata XML file was downloaded correctly and was not corrupted in
  any way.
- Your Login Domain is unique. For example, `mycompany.com` can't be used 
  for more than one |idp|, even if the domains are in different Rackspace 
  accounts.


Problems Logging In
~~~~~~~~~~~~~~~~~~~

After entering your email address into `login.rackspace.com/federate <https://login.rackspace.com/federate>`_, 
you aren't successfully redirected to your identity system login or you 
aren't redirected back to Rackspace after a successful credential entry.

Consider the following:

- Check the Login Domain for your |idp| to ensure that you have set the
  correct one and that it is a valid email address domain. (For example,
  ``mycompany.com``, not ``user@mycompany.com``).
- Try updating your |idp| metadata by re-downloading the metadata file from
  your third party SAML provider and updating it in the |idp| details
  page.
- Verify the Rackspace federation details you entered into your third party
  SAML provider. Examples can be found in the document in
  :ref:`index-configuring-3p-saml-ug`.


Problems with Roles/Access
~~~~~~~~~~~~~~~~~~~~~~~~~~

After successfully logging in, you are unable to access the products or
services you expect.

Consider the following:

- Review your |amp| to ensure you are assigning values to the `roles` 
  parameter. Look at examples in :ref:`index-configuring-3p-saml-ug`, and
  :ref:`attribmapping-basics-ug`. |ampref|
- If using Fanatical Support for AWS, review the Fanatical Support for AWS
  section in :ref:`faws-mapping-ug`. If that looks correct, contact your
  Fanatical Support for AWS account/support team for further guidance.


Other Issues or Questions
~~~~~~~~~~~~~~~~~~~~~~~~~

Contact Rackspace Support as covered in :ref:`getting-support-ug`.
