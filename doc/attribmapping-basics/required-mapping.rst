.. _required-mapping-ug:

============================================
Required SAML Attributes and Mapping Example
============================================

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
     - Example Value
   * - **domain**
     - The Identity/Account domain that the |idp| is authorized to log users
       into. The domain is listed on the Identity Provider details page for
       your |idp|.
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
     - The product RBAC (role based access control) roles you want the user
       to be assigned.
     - Alphanumeric string, comma delimited
     - "nova:observer, lbaas:admin"
   * - **expires**
     - The timeout before users must reauthenticate with your identity
       system.
     - SOME VALID FORMAT HERE
     - INSERT SAML STANDARD NOTONORAFTERHERE

**Example Policy with Required Attributes**

This is an example |amp| using explicit and SAML-provided
values for mapping the required fields.

   .. code-block:: yaml

      mapping:
          rules:
              local:
                  user:
                      domain: 636462353
                      name: {D}  #  Username will be set from common "name" value in your SAML
                      email: "{2}"  # References the second element of the "remote" rule, i.e. "emailaddress"
                      roles: "nova:observer, lbaas:admin"
                      expire: {1}  #  References the first element of the "remote" rule, i.e. NotOnOrAfter
             remote:
                  - path: "/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1]"
                          # This value is specified as a path in the SAML XML
                  - name: "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
                          # This value is specified as a SAML schema element
          version: "RAX-1"


For information on adding additional rules, or using XPath to interact
with your SAML and |amp|, see LINK TO REFERENCE HERE. OR MORE EXAMPLES?
