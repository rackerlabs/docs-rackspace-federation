.. required-mapping-ug:

=============
Required Attributes
=============

.. Define |product name| in conf.py

Your |amp| is **required** to contain:

- a minimum of one **local** rule
- static or dynamically populated values for the following fields:

.. list-table::
   :widths: 30 50 30 30
   :header-rows: 1

   * - Field
     - Description
     - Format
     - Suggested Value
   * - **domain** 
     - The Identity/Account domain that the |idp| is authorized to log users into. The domain
       is listed on the Identity Provider details page for your |idp|. 
     - Alphanumeric string
     - MUST be set to your Identity domain.
   * - **name**
     - The username of your user as provided by your identity system. 
     - Alphanumeric string
     - INSERT THE SAML STANDARD NAME FIELD PATH HERE
   * - **email**
     - The email address of your user as provided by your identity system. 
     - RFC valid email address
     - INSERT THE SAML STANDARD EMAIL FIELD PATH HERE
   * - **roles**
     - 
     - 
     - 
   * - **expires**
     - 
     - 
     - 



Example Policy with Required Attributes





