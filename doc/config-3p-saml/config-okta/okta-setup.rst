.. _okta-setup-ug:

Prerequisites
-------------

- Administrator access to your organization's Okta account.
- :ref:`Rackspace Federation configuration details<generic-3p-saml-ug>`.
- These instructions are using the **Classic UI** setting in Okta.

Configure Okta
--------------
Follow these steps to set up a Security Assertion Markup Language (SAML)
integration with Okta to work with |service|:


1. Log in to your organization's Okta account using your organization's sign-in
   page.

2. Click **Applications**, located on the top ribbon.

3. On the next screen, click the **Add Application** button.

4. Next, click the **Create New Application** button.

5. From within the **Configure a New Application Integration** popup window,
   select **Web** from the Platform options and **SAML 2.0** from the
   Sign on method options.

.. image:: ../../_images/Config-okta/create_app_1.png

6. On the **General Settings** page, fill in the **App name** with whatever you
   want users to see when they use the application and then click **Next**.

7. Fill in the requested SAML information with the :ref:`Rackspace Federation
   configuration details<generic-3p-saml-ug>`.

The metadata file contains the latest certificate for signing SAML assertions.

The default values can be retrieved programmatically from the Rackspace service
provider metadata file at: `https://login.rackspace.com/federate/sp.xml
<https://login.rackspace.com/federate/sp.xml>`_ and are shown in the following
list:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Attribute
     - Value
   * - EntityID ("Audience")
     - https://login.rackspace.com
   * - Assertion Consumer Service
       ("Single Sign On URL")
     - https://login.rackspace.com/federate/acs


Instructions on setting up SAML applications in Okta can be found at:
https://developer.okta.com/standards/SAML/setting_up_a_saml_application_in_okta


Download your Okta |idp| metadata by going to the new SAML applications
settings and going to the **Sign On** section. Click the **Identity Provider
metadata** link to download the XML file that you will use to configure your
|idp| with Rackspace.

.. image:: ../../_images/Config-okta/idp_metadata.png
