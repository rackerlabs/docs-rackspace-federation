.. _terminology-gs-ug:

===========
Terminology
===========

Before setting up |product name|, understanding some basic terminology
can helpful. The following table provides descriptions for some of the terms
that are associated with |product name|:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Term
     - Description
   * - Identity Provider
     - An Identity Provider is a third-party identity system that is being
       integrated with Rackspace.
   * - SAML
     - SAML (Security Assertion Markup Language) is the protocol that is used
       to communicate between an Identity Provider and Rackspace
   * - Attribute Mapping
     - During the login process, the Identity Provider and Rackspace exchange
       SAML messages containing attributes about the user who is
       authenticating. These SAML attributes (also called *assertions*) can be
       interpreted by an Attribute Mapping Policy to set Rackspace roles and
       permissions during login.
   * - Provisioned user
     - A provisioned user is a user created directly in the Rackspace control
       panels. Provisioned users use the username and credentials that are
       created with Rackspace.
   * - Federated user
     - A federated user is a user who logs in to Rackspace using Identity
       Federation. Federated users use the credentials provided and managed by
       their own identity system.
