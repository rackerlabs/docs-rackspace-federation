.. generic-3p-saml-ug:

====================
Other SAML Providers
====================

|service| is designed to be compatible with any SAML 2.0 based identity
provider. The information below provides basic settings needed to
configure a third party SAML provider.

SAML Configuration Items
~~~~~~~~~~~~~~~~~~~~~~~~

SAML providers will require one or more of the following links or URLS to
configure how to connect to Rackspace and redirect during login sessions.

The default values are listed below, but can also be retrieved programmatically
from the Rackspace service provider metadata file at:
`https://login.rackspace.com/federate/sp.xml
<https:login.rackspace.com/federate/sp.xml>`_

The metadata file will also always contain the latest certificate for signing
SAML assertions.

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
   * - Single Logout Service
     - https://login.rackspace.com/federate/sls


SAML Attribute Mapping
~~~~~~~~~~~~~~~~~~~~~~

You will need to set up an |amp| to ensure that the SAML attributes your
identity provider sends during the SAML login process are mapped to the
required/desired values for Rackspace.

An overview of the required values and example mappings can be found at
:ref:`required-mapping-ug`.
