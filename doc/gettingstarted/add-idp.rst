.. _add-idp-gs-ug:

========================
Add an Identity Provider
========================

The first step to use Federation is to add an Identity Provider, which is the
authentication system that you want to use to authenticate with Rackspace.
For example, if you want the employees of your company, Aeronautics-R-Us, to
authenticate to Rackspace products and services by using your Aeronautics-R-Us
credentials, you need to update your Rackspace account with some information.

Click one of the following links to learn how to add an |idp|:

- Cloud customers, add an |idp| by using the
  :ref:`Rackspace Control Panel<add-idp-cp-gs-ug>`.
- Dedicated customers, add an |idp| by using the
  :ref:`MyRack portal <add-idp-mr-gs-ug>`.
- All customers, add an |idp| by using the
  :ref:`Identity API<add-idp-api-gs-ug>`.


Create and upload the SAML configuration file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, you need to upload an XML file that contains the required metadata to
complete the setup of your |idp|. Most identity systems have a method for
generating the metadata file either automatically, or after you've completed
some basic configuration.

For general and provider-specific guidance about configuring your identity
system and retrieving your metadata XML file, see the section
:ref:`index-configuring-3p-saml-ug`.

Create the Identity Provider
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After your XML file is attached, click **Create Identity Provider**.
