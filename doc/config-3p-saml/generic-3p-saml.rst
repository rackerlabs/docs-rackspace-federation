.. _generic-3p-saml-ug:

====================
Other SAML providers
====================

|service| is designed to be compatible with any SAML 2.0-based identity
provider. The following information provides basic settings that you need to
configure a third-party SAML provider.

SAML configuration items
~~~~~~~~~~~~~~~~~~~~~~~~

SAML providers require one or more of the following links to
configure the connection to Rackspace and to redirect during login sessions.

The metadata file contains the latest certificate to sign SAML assertions.

You can retrieve the default values programmatically from the Rackspace
metadata file at `https://login.rackspace.com/federate/sp.xml
<https://login.rackspace.com/federate/sp.xml>`_. The following list
includes the values in the file:

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

Set up an |amp| to ensure that the SAML attributes that your
identity provider sends during the SAML login process are mapped to the
required or desired values for Rackspace.

You can find an overview of attribute mapping and example mapping policies at
:ref:`config-am-policy-gs-ug`.
