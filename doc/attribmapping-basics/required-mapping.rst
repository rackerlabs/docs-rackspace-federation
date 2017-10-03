.. _required-mapping-ug:

============================================
Required SAML Attributes and Mapping Example
============================================

.. Define |product name| in conf.py

Required Values
~~~~~~~~~~~~~~~

Your |amp| is **required** to contain:

- a minimum of one **local** rule
- static or dynamically populated values for the following fields:

.. list-table::
   :widths: 40 20 30 30
   :header-rows: 1

   * - Field
     - Description
     - Format
     - Common Values
   * - **domain**
     - The Identity/Account Domain that the |idp| is authorized to log users
       into.
     - Alphanumeric string
     - **MUST** be set to your Identity Domain. The
       domain is listed on the Identity Provider details page for your
       |idp|.
   * - **name**
     - The username of your user as provided by your identity system.
     - Alphanumeric string
     - | SAML Attributes:
       |
       |  **NameID** (persistent type preferred)
       |  **urn:oid:1.3.6.1.4.1.5923.1.1.1.6**
       |  **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
   * - **email**
     - The email address of your user as provided by your identity system.
     - RFC valid email address
     - | SAML Attributes:
       |
       |  **email**
       |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
       |  **urn:oid:1.2.840.113549.1.9.1.1**
       |  **0.9.2342.19200300100.1.3"**
   * - **roles**
     - The product RBAC (role based access control) roles you want the user
       to be assigned. (Supported for Rackspace Cloud products only at this
       time.)
     - YAML array of alphanumeric strings
     - | **Example:**
       |
       | ``roles:``
       |     ``- "nova:admin"``
       |     ``- "lbaas:observer"``
   * - **expires**
     - The timeout before users must reauthenticate with your identity
       system.
     - ISO format time values
     - | **Example:** ``"PT12H`` (12 hours)
       |
       | *or*
       |
       | SAML Attributes
       |
       |   **SessionNotOnOrAfter**
       |   **NotOnOrAfter**

Setting Values with Attribute Mapping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are a number of ways to set values either explicitly or by using
|amp| language features such as *substitions* or XPath.

The example syntax below uses in-line substitutions in the **local** rule to
concisely retrieve values and simplify our policy. There are multiple
additional ways to accomplish the same (or more complex) scenarios.  |ampref|

.. list-table::
   :header-rows: 1

   * - Method
     - Description
     - Example
   * - Default
     - Retrieves the value by looking for common locations/labels for the
       field. Note that at this time, only an attribute with the same name as
       the field will be matched: i.e. ``name: "{D}"`` will match the attribute
       with the name ``name``.
     - ``name: "{D}"``
   * - Explicit
     - Directly input the values into the |amp| fields. This is most useful for
       values that will not change for any federated user logging in, as they
       will be applied to **all** federated users for this |idp|.
     - ``expire: "PT12H"``
   * - Attribute Matching
     - Uses XPath to match a SAML attribute in your SAML assertion by name,
       returning one or more values.
     - | Single value return (``At``): ``email: "{At(urn:oid:1.2.840.113549.1.9.1.1)}"``
       |
       | Multi value return (``Ats``):
       |   ``groups:``
       |          ``multiValue: true``
       |               ``value: "{Ats(http://schemas.xmlsoap.org/claims/Group)}"``
   * - Path Matching
     - Uses XPath to match the path to a value in your SAML assertion using the
       XML hierarchy/schema.
     - | ``"{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"``
       |
       | Retrieves the value of ``NotOnOrAfter``




Example Policy with Required Attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is an example |amp| using explicit and SAML-provided values for mapping
the required fields. Note that this is a basic example, and more customization
may be required in some cases. For considerations for specific third party SAML
providers, see :ref:`index-configuring-3p-saml-ug`.

|ampref|

   .. code-block:: yaml

      mapping:
      version: "RAX-1"
          # Comments are allowed in YAML
          rules:
              local:
                  user:
                      domain: 636462353
                      # Domain must be set to your Identity Domain
                      name: {D}
                      #  Username will be set from element named "name" value in your SAML
                      email: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress)}"
                      #  Locates the attribute with the above URL as the claim type/name
                      roles:
                          - "nova:observer"
                          - "lbaas:admin"
                      #  Assigns the roles explicitly listed above
                      expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"
                      #  Retrieves the NotOnOrAfter value by using the SAML path and XPath

