.. terminology-gs-ug:

===========
Terminology
===========

Before setting up |product name|, understanding some basic terminology
can helpful.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Term
     - Description
   * - Identity Provider
     - An Identity Provider is a third party identity system being
       integrated with Rackspace.
   * - SAML
     - Security Assertion Markup Language, the protocol used to communicate
       between an Identity Provider and Rackspace
   * - Attribute Mapping
     - During the login process, the Identity Provider and Rackspace exchange
       SAML messages containing attributes about the user who is
       authentication. These SAML Attributes (also called *assertions*) can be
       interpreted by an Attribute Mapping Policy to set Rackspace roles and
       permissions during login.
   * - Provisioned user
     - A user created directly in the Rackspace control panels. Provisioned
       users use the username and credentials created with Rackspace.
   * - Federated user
     - A user logging in to Rackspace using Identity Federation. Federated
       users use the credentials provided and managed by their own identity
       system.
