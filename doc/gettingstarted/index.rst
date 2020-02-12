.. _index-gs-ug:

===============
Getting started
===============

This section provides information about getting started with |product name|.

Prerequisites
-------------

Before proceeding, verify that you have the necessary resources to
complete setting up |product name|. Review the following list of
prerequisites:

- You have administrator access to your |idp| such as Okta®.
- You have administrator access to your Rackspace Customer Portal.
- You have any necessary permissions at your company.

Summary of steps
-----------------

The following are the basic steps for using Rackspace Federation:

1. Add Rackspace Federation to your |idp|. Use the instructions listed
   for your company's |idp|:

   - :ref:`Add Rackspace Federation at your Identity Provider<add-rfi-idp-gs-ug>`.

2. Add an |idp| at Rackspace by using one of the following methods:

   - :ref:`Add an Identity Provider in the Control Panel<add-idp-cp-gs-ug>`.

   - :ref:`Add an Identity Provider in the MyRack Portal<add-idp-mr-gs-ug>`.

   - :ref:`Add an Identity Provider by using the API<add-idp-api-gs-ug>` by
     providing basic information about the |idp|.

3. :ref:`Configure the Attribute Mapping Policy<config-am-policy-gs-ug>` and
   upload that file to complete the |idp| creation.
4. :ref:`Log in<accessing-gs-ug>` and test your configuration.

Review the following sections for more information:

.. toctree::
   :maxdepth: 1

   add-rfi-idp.rst
   add-idp.rst
   config-am-policy.rst
   accessing.rst

Concepts
--------

Before you set up |product name|, make sure you understand some basic terminology.
The following table provides descriptions for some of the terms
that are associated with |product name|:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Term
     - Description
   * - Identity provider
     - An identity provider is a third-party identity system that is
       integrated with Rackspace.
   * - SAML
     - SAML (Security Assertion Markup Language) is the protocol that is used
       to communicate between an identity provider and Rackspace.
   * - Attribute mapping
     - During the login process, the identity provider and Rackspace exchange
       SAML messages containing attributes about the user who is
       authenticating. An attribute mapping policy interprets these SAML
       attributes (also called *assertions*) to set Rackspace roles and
       permissions during login.
   * - Provisioned user
     - A provisioned user is a user created directly in the Rackspace Customer
       Portal. Provisioned users use the username and credentials that are
       created with Rackspace.
   * - Federated user
     - A federated user is a user who logs in to Rackspace by using Identity
       Federation. Federated users use the credentials provided and managed by
       their own identity system.
