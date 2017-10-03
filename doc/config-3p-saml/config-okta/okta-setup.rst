.. _okta-setup-ug:

================
Configuring Okta
================

These are the steps to setup a SAML integration to do federation with
Rackspace.


1. Configure a new application integration and select 'SAML 2.0'


(insert first screenshot here)


Instructions on setting up SAML applications in Okta can be found here:
https://developer.okta.com/standards/SAML/setting_up_a_saml_application_in_okta


2. Fill in the SAML information from Rackspaces metadata which is available
here: https://login.rackspace.com/federate/sp.xml


3. Download your Okta Idp metadata by going to the new SAML applications
settings and going to the "Sign On" section. Click the "Identity Provider
metadata" link to download the xml file that you will use to configure your
identity provider with Rackspace.

(see last screenshot in this PR pointing to a link, thinking about inserting it
here for clarity)
