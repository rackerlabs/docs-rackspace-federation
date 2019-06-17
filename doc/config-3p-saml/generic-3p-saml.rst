.. generic-3p-saml-ug:

====================
Other SAML providers
====================

|service| is designed to be compatible with any SAML 2.0-based identity
provider. The following information provides basic settings that are needed to
configure a third-party SAML provider.

SAML configuration items
~~~~~~~~~~~~~~~~~~~~~~~~

SAML providers require one or more of the following links or URLS to
configure the connection to Rackspace and to redirect during login sessions.

The metadata file contains the latest certificate for signing SAML assertions.

The default values can be retrieved programmatically from the Rackspace service
provider metadata file at `https://login.rackspace.com/federate/sp.xml
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
   * - Single Logout Service
     - https://login.rackspace.com/federate/sls


SAML attribute mapping
~~~~~~~~~~~~~~~~~~~~~~

You need to set up an |amp| to ensure that the SAML attributes that your
identity provider sends during the SAML login process are mapped to the
required or desired values for Rackspace.

You can find an overview of Attribute Mapping and example mapping policies at
:ref:`config-am-policy-gs-ug`.
