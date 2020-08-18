.. _required-mapping-ug:

Required SAML attributes and mapping example
--------------------------------------------


Required values
~~~~~~~~~~~~~~~

Your |amp| must contain:

- a minimum of one local rule
- static or dynamically populated values for the following fields:

.. list-table::
   :widths: 40 20 30 30
   :header-rows: 1

   * - Field
     - Description
     - Format
     - Common values
   * - **domain**
     - The Identity or Account Domain that the |idp| is authorized to log users
       in to.
     - Alphanumeric string
     - Must be set to your Identity Domain. The
       domain appears on the **Details** page for your
       |idp|.
   * - **name**
     - The username of your user as provided by your identity system.
     - Alphanumeric string
     - | SAML attributes:
       |
       |  **NameID** (persistent type preferred)
       |  **urn:oid:1.3.6.1.4.1.5923.1.1.1.6**
       |  **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
   * - **email**
     - The email address of your user as provided by your identity system.
     - RFC-valid email address
     - | SAML Attributes:
       |
       |  **email**
       |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
       |  **urn:oid:1.2.840.113549.1.9.1.1**
       |  **0.9.2342.19200300100.1.3"**
   * - **roles**
     - The product RBAC (role-based access control) roles that you want
       to assign to the user.
     - YAML array of alphanumeric strings
     - | **Example:**
       |
       | ``roles:``
       |     ``- "nova:admin"``
       |     ``- "lbaas:observer"``
   * - **expires**
     - The amount of time after which users must reauthenticate with your
       identity system.
     - ISO format time values
     - | **Example:** ``"PT12H`` (12 hours)
       |
       | *or*
       |
       | SAML attributes
       |
       |   **SessionNotOnOrAfter**
       |   **NotOnOrAfter**

Setting values with Attribute Mapping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can set values either explicitly or by using |amp| language features such
as inline substitutions or XPath.

The following example syntax uses in-line substitutions in the local rule to
concisely retrieve values and simplify the policy. There are additional ways to
accomplish the same (or more complex) scenarios.  |ampref|

.. list-table::
   :header-rows: 1

   * - Method
     - Description
     - Example
   * - Default
     - Retrieves the value by looking for common locations or labels for the
       field. Note that at this time, only an attribute with the same name as
       the field is matched. For example, ``name: "{D}"`` matches the
       attribute with the name ``name``.
     - ``name: "{D}"``
   * - Explicit
     - Directly input the values into the |amp| fields. This is most useful for
       values that do not change for any federated user logging in, because
       they are applied to **all** federated users for this |idp|.
     - ``expire: "PT12H"``
   * - Attribute matching
     - Uses XPath to match a SAML attribute in your SAML assertion by name,
       returning one or more values.
     - | Single value return (``At``): ``email: "{At(urn:oid:1.2.840.113549.1.9.1.1)}"``
       |
       | Multi value return (``Ats``):
       |   ``groups:``
       |          ``multiValue: true``
       |               ``value: "{Ats(http://schemas.xmlsoap.org/claims/Group)}"``
   * - Path matching
     - Uses XPath to match the path to a value in your SAML assertion by using
       the XML hierarchy or schema.
     - | ``"{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"``
       |
       | Retrieves the value of ``NotOnOrAfter``




Example policy with required attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following |amp| example uses explicit and SAML-provided values for mapping
the required fields. Note that this is a basic example, and more customization
might be required in some cases. For considerations for specific third-party
SAML providers, see :ref:`index-configuring-3p-saml-ug`.

|ampref|

XML Example:

.. code-block:: xml


      1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      2 <rules>
      3 <rule>
      4 <local>
      5 <user>
      6 <domain value="{D}"/>
      7 <name value="{D}"/>
      8 <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
      9 <roles value="nova:observer admin" multiValue="true"/>
      10 <expire value="{D}"/>
      11 </user>
      12 </local>
      13 </rule>
      14 </rules>
      15 </mapping>

   .. code-block:: yaml

    mapping:
     version: "RAX-1"
      # Comments are allowed in YAML
     rules:
     - local:
        user:
           domain: "{D}"
           # Domain must be set to your Identity Domain
           name: "{D}"
           #  Username is set from the element named "name" value in your SAML
           email: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress)}"
           #  Locates the attribute with the above URL as the claim type or name
           roles:
           - "nova:observer"
           - "admin"
           #  Assigns the roles explicitly listed above
           expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"
           #  Retrieves the NotOnOrAfter value by using the SAML path and XPath
