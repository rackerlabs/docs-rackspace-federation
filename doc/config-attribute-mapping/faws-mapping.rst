.. _faws-mapping-ug:

===============================================
Assigning Fanatical Support for AWS Permissions
===============================================

**Note:** Identity Federation for Fanatical Support for AWS is available as
part of a **public beta**. It is available to all customers, but phone support
is not available for federated users. Federated users need to submit all
support requests through the
`Rackspace ticketing portal <https://portal.rackspace.com/tickets>`_.

Fanatical Support for AWS Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to features within Rackspace's Fanatical
Support for AWS Control Panel. User permissions can be set to ``observer``,
``admin``, or omitted from the mapping policy. Users with ``observer``
permissions have read-only access to the Control Panel. Users with ``admin``
permissions have read and write access to the Control Panel. These permissions
are assigned by granting them as roles in a mapping policy. The following
mapping policy assigns the ``admin`` role to all federated users:

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

It's much more common to assign roles conditionally based on a user's group
membership. In the following mapping policy, users who belong to the
``mycompany.global.admin`` group are granted the ``admin`` role, and users who
belong to the ``mycompany.global.observer`` group are granted the ``observer``
role:

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

``admin`` and ``observer`` can also be scoped to specific AWS accounts. In this
policy, members of the ``mycompany.scoped.admin`` group are granted the FAWS
``admin`` role on multiple AWS accounts, and members of
``mycompany.scoped.observer`` are granted ``observer`` on the single account
``12345678012`` :

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

For more information about Fanatical Support for AWS permissions, visit the
`User Management and Permissions <https://manage.rackspace.com/aws/docs/product-guide/access_and_permissions/user_management_and_permissions.html>`_
section of the Fanatical Support for AWS product guide.

AWS Console and API Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to the Amazon Web Services APIs and to
features within the AWS Web Console. The following mapping policy assigns all
users the "ViewOnlyAccess" IAM policy for all AWS accounts. It also assigns the
"AdministratorAccess" IAM policy to all users for a single AWS account.

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
policy below assigns permissions as follows:

* Users in the ``mycompany.global.admin`` group are assigned the
  ``AdministratorAccess`` IAM policy on all AWS accounts.
* Users in the ``mycompany.global.observer`` group are assigned the
  ``ViewOnlyAccess`` IAM policy on all AWS accounts.
* Users in the ``mycompany.12345678012.admin`` group are only assigned the
  ``AdministratorAccess`` IAM policy for AWS account 123456789012.

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
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else (),
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
              )
            multiValue: true
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
              )
            multiValue: true

Many customers create their own
`customer managed policies <https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#customer-managed-policies>`_
that are the same across many AWS accounts. Policy ARNs can omit the account ID
section, which makes it easier to assign these policies. For example, if a
policy named ``MyCompany.Audit`` exists on every AWS account, you can assign
this policy by using ``arn:aws:iam:::policy/MyCompany.Audit`` in your mapping
policy.

AWS Account Creator Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This permission controls whether or not a user can create new AWS accounts
through the Fanatical Support for AWS Control Panel. The following mapping
policy grants users in the ``mycompany.global.admin`` group permission to
create new AWS accounts:

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

Complete Mapping Policy Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following example combines both Fanatical Support for AWS permissions and
AWS Console and API permissions into a single mapping policy:

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

