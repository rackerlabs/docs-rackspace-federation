.. _add-idp-cp-gs-ug:

===========
Adding an Identity Provider in the Control Panel
===========


To add an |idp|, log in to your Rackspace control panel by browsing to 
https://login.rackspace.com. 

Click on your username in the upper right area of the navigation bar, and select
**User Management** from the dropdown menu. 

*Alternate approach START*

To add an |idp|, browse to the `Account Management <https://account.rackspace.com/>`_ page
in the Rackspace Control Panel.

*Alternate approach END*


|ss|

Click the **Add Identity Provider** button. 

|ss|

Basic Information
~~~~~~~~~~~~~~~~~

Each |idp| should have a unique **Description** and **Login Domain**. 

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Item
     - Description
   * - Description
     - Provide a description for your |idp|. This is the description will show
       in lists and other areas in the Control Panel.
   * - Login Domain
     - Provide a valid email domain, such as *mycompany.com*. Users will provide
       their email address during the federated login process, and their email domain
       will be used to identify which |idp| they should be redirected to to complete logging in. 

SAML Metadata
~~~~~~~~~~~~~~

Next, you will need to upload an XML file containing required metadata to complete the 
setup of your |idp|. Most identity systems have a method for generating this metadata file
automatically, or after some basic configuration has been completed. 

See the section **LINK TO WHEREVER WE LIST IDP SETUP GUIDES** for general
and provider specific guidance on configuring your identity system and 
retrieving your metadata XML file. 

|ss|

When your XML file is attached, click the **Create Identity Provider** button to complete
the process. 

