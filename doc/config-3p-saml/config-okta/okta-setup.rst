.. _okta-setup-ug:

Configure Okta
--------------

Follow these steps to set up a SAML integration with Okta to work with
|service|:


1. Configure a new application integration and select **SAML 2.0**.

.. image:: ../../_images/Config-okta/create_app_1.png


2. Fill in the requested SAML information with Rackspace details.

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


3. Download your Okta |idp| metadata by going to the new SAML applications
settings and going to the **Sign On** section. Click the **Identity Provider
metadata** link to download the XML file that you will use to configure your
|idp| with Rackspace.

.. image:: ../../_images/Config-okta/idp_metadata.png
