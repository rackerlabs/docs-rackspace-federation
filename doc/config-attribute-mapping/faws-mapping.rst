.. _faws-mapping-ug:

===============================================
Assigning Fanatical Support for AWS Permissions
===============================================

Fanatical support for AWS permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to features within Rackspace's Fanatical
Support for the Amazon Web Services (FAWS) Control Panel. You can assign the roles 
of ``observer`` or ``admin`` or omit them from the mapping policy. Users with 
``observer`` permissions have read-only access to the Control Panel. Users with
 ``admin`` permissions have read and write access to the Control Panel. The 
following mapping policy assigns the ``admin`` role to all federated users:

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="admin" multiValue="true"/>
                <expire value="PT12H"/>
              </user>
          </local>
        </rule>
    </rules>
  </mapping>

YAML Example: 

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
            roles:
              - "admin"


It's common to assign roles based on a user's group membership. 
The following mapping policy example grants the ``admin`` role to users who
belong to the ``mycompany.global.admin`` group, and the ``observer``
role to users who belong to the ``mycompany.global.observer`` group:

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="{0}" multiValue="true"/>
                <expire value="PT12H"/>
              </user>
          </local>
          <remote>
              <attribute path="(
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('admin') else (),
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('observer') else ()
                                )"
                        multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
            roles:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('admin') else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('observer') else ()
              )
            multiValue: true


You can limit the roles of ``admin`` and ``observer`` to specific Amazon Web 
Services® (AWS) accounts. The preceding policy example grants the FAWS ``admin`` role
to members of the ``mycompany.scoped.admin`` group on multiple
 AWS accounts, and the  ``observer`` role to members of ``mycompany.scoped.observer``
 on the single account ``12345678012``:

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="{0}" multiValue="true"/>
                <expire value="PT12H"/>
              </user>
          </local>
          <remote>
              <attribute path="(
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.admin') then (
                                'admin/faws:12345678012',    
                                'admin/faws:987654321098',
                                'admin/faws:112233445566'  ) else (),
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.observer') then ('observer/faws:12345678012') else ()
                                )"
                                multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
            roles:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.admin') then (
                  'admin/faws:12345678012',
                  'admin/faws:987654321098',
                  'admin/faws:112233445566'
                ) else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.observer') then ('observer/faws:12345678012') else ()
              )
            multiValue: true


In the preceding example, members of both the ``mycompany.scoped.admin`` group 
and the ``mycompany.scoped.observer`` group have the ``admin`` role on the 
single FAWS account ``12345678012``. 

Swapping the ``admin`` and ``observer`` groups in the next example grants 
only the ``observer`` role on that single account to any
user in both groups. This assignment occurs because the first ``if`` condition
matches, so the policy doesn't evaluate the second ``if`` condition. 

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="{0}" multiValue="true"/>
                <expire value="PT12H"/>
              </user>
          </local>
          <remote>
              <attribute path="(
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.observer') then ('observer/faws:12345678012') else ()
                                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.admin') then (
                                'admin/faws:12345678012',
                                'admin/faws:987654321098',
                                'admin/faws:112233445566'
                                ) else (),
                                )"
                        multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
            roles:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.observer') then ('observer/faws:12345678012') else ()
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.scoped.admin') then (
                  'admin/faws:12345678012',
                  'admin/faws:987654321098',
                  'admin/faws:112233445566'
                ) else (),
              )
            multiValue: true


Visit the `User Management and Permissions <https://manage.rackspace.com/aws/docs/product-guide/access_and_permissions/user_management_and_permissions.html>`_
section of the Fanatical Support for AWS product guide for further details.

AWS console and API permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to the Amazon Web Services APIs and to
features within the AWS Web Console. The following mapping policy assigns all
users the "ViewOnlyAccess" IAM policy for all AWS accounts. It also assigns the
"AdministratorAccess" IAM policy to all users for a single AWS account.

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                  <domain value="{D}"/>
                  <name value="{D}"/>
                  <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                  <expire value="PT12H"/>
              </user>
              <aws xsi:type="LocalAttributeGroup">
                  <attribute name="iamPolicies:123456789012"
                              value="arn:aws:iam::aws:policy/AdministratorAccess"
                              multiValue="true"/>
                  <attribute name="iamPolicies:*"
                              value="arn:aws:iam::aws:policy/job-function/ViewOnlyAccess"
                              multiValue="true"/>
              </aws>
          </local>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
          aws:
            iamPolicies:*:
              - "arn:aws:iam::aws:policy/job-function/ViewOnlyAccess"
            iamPolicies:123456789012:
              - "arn:aws:iam::aws:policy/AdministratorAccess"


As with Fanatical Support for AWS permissions, it's much more common to assign
IAM policies conditionally based on a user's group membership. The mapping
policy assigns permissions as follows:

* Users in the ``mycompany.global.security`` group are assigned the
  ``SecurityAudit`` IAM policy on all AWS accounts.
* Users in the ``mycompany.global.observer`` group are assigned the
  ``ViewOnlyAccess`` IAM policy on all AWS accounts.
* Users in the ``mycompany.12345678012.admin`` group are only assigned the
  ``AdministratorAccess`` IAM policy for AWS account ``123456789012``.

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <expire value="PT12H"/>
              </user>
              <aws xsi:type="LocalAttributeGroup">
                  <attribute name="iamPolicies:123456789012" value="{1}" multiValue="true"/>
                  <attribute name="iamPolicies:*" value="{0}" multiValue="true"/>
              </aws>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.security') then ('arn:aws:iam::aws:policy/SecurityAudit') else (),
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
                        )"
                        multiValue="true"/>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
                        )"
                        multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
          aws:
            iamPolicies:*:
              - "{0}"
            iamPolicies:123456789012:
              - "{1}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.security') then ('arn:aws:iam::aws:policy/SecurityAudit') else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
              )
            multiValue: true
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
              )
            multiValue: true


In the preceding example, members of the
``mycompany.global.security`` and the ``mycompany.123456789012.admin``
groups, have the``AdministratorAccess`` IAM policy. In this case, the 
``SecurityAudit`` IAM policy attaches to the user's temporary session for the 
AWS account ``123456789012``. 

Customer-managed AWS IAM policies that are the same across AWS accounts
-----------------------------------------------------------------------

Many customers create their own
`customer-managed policies <https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#customer-managed-policies>`_
that are the same across many AWS accounts. Policy ARNs can omit the account ID
section, which makes it easier to assign these policies. For example, if a
policy named ``MyCompany.Audit`` exists on every AWS account, you can assign
this policy by using ``arn:aws:iam:::policy/MyCompany.Audit`` in your mapping
policy.

AWS account creator permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This permission controls whether a user can create new AWS accounts
through the Fanatical Support for AWS Control Panel. The following mapping
policy grants users in the ``mycompany.global.admin`` group permission to
create new AWS accounts:

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <expire value="PT12H"/>
              </user>
              <aws xsi:type="LocalAttributeGroup">
                <creator value="{0}" xsi:type="LocalAttribute"/>
              </aws>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('true') else ('false')
                        )"
                        multiValue="false"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  mapping:
    version: RAX-1
    rules:
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
          aws:
            creator: "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('true') else ('false')
              )
            multiValue: false


Complete mapping policy example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following example combines both Fanatical Support for AWS permissions and
AWS Console and API permissions into a single mapping policy:

XML Example:

.. code:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
          <local>
              <user>
                <domain value="{D}"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="{0}" multiValue="true"/>
                <expire value="PT12H"/>
              </user>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('admin') else (),
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('observer') else ()
                        )"
                        multiValue="true"/>
          </remote>
        </rule>
        <rule>
          <local>
                <aws xsi:type="LocalAttributeGroup">
                  <creator value="{0}" xsi:type="LocalAttribute"/>
                </aws>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('true') else ('false')
                        )"
                        multiValue="false"/>
          </remote>
        </rule>
        <rule>
          <local>
                <aws xsi:type="LocalAttributeGroup">
                  <attribute name="iamPolicies:*" value="{0}" multiValue="true"/>
                </aws>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else (),
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
                        )"
                        multiValue="true"/>
          </remote>
        </rule>
        <rule>
          <local>
                <aws xsi:type="LocalAttributeGroup">
                  <attribute name="iamPolicies:123456789012" value="{0}" multiValue="true"/>
                </aws>
          </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
                        )"
                        multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code:: yaml

  ---
  mapping:
    version: RAX-1
    rules:
      # Map groups to user roles
      - local:
          user:
            domain: "{D}"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: "PT12H"
            name: "{D}"
            roles:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('admin') else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('observer') else ()
              )
            multiValue: true
      # Map groups to AWS account creator permissions
      - local:
          aws:
            creator: "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('true') else ('false')
              )
            multiValue: false
      # Map groups to IAM policies for all AWS accounts
      - local:
          aws:
            iamPolicies:*:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
              )
            multiValue: true
      # Map groups to IAM policies for AWS account 123456789012
      - local:
          aws:
            iamPolicies:123456789012:
              - "{0}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
              )
            multiValue: true
