.. _faws-mapping-ug:

Assigning Fanatical Support for AWS Permissions
-----------------------------------------------

Fanatical Support for AWS Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to features within Rackspace's Fanatical Support for AWS Control Panel. User permissions can be set to ``observer``, ``admin``, or omitted from the mapping policy. Users with ``observer`` permissions have read-only access to the control panel. Users with ``admin`` permissions have read and write access to the control panel. These permissions are assigned by granting them as roles in a mapping policy. The following mapping policy assigns the ``admin`` role to all federated users.

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

It will be much more common to assign roles conditionally based on a user's group membership. The following mapping policy assigns roles conditionally based upon a user's group membership. Users who belong to the ``mycompany.global.admin`` group are granted the ``admin`` role . Users who belong to the ``mycompany.global.observer`` group are granted the ``observer`` role.

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


AWS Console and API Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These permissions control access to the Amazon Web Services APIs and to features within the AWS Web Console. The following mapping policy assigns all users the "ViewOnly" IAM policy for all AWS accounts. It also assigns the "Administrator" IAM policy to a single AWS account.

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

As with Fanatical Support for AWS permissions, it is much more common to assign roles conditionally based on a user's group membership. The following mapping policy assigns the following permissions:

* Users in the ``mycompany.global.admin`` group are assigned the ``Administrator`` IAM policy on all AWS accounts
* Users in the ``mycompany.global.observer`` group are assigned the ``ViewOnly`` IAM policy on all AWS accounts.
* Users in the ``mycompany.12345678012.admin`` group are assigned the ``Administrator`` IAM policy to a single AWS account, 123456789012.

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
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess')
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
              )
            multiValue: true
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
              )
            multiValue: true

Complete Mapping Policy Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following example combines both Fanatical Support for AWS permissions and Fanatical Support for AWS permissions into a single mapping policy.

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
          aws:
            iamPolicies:*:
              - "{1}"
            iamPolicies:123456789012:
              - "{2}"
        remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('admin') else (),
                if (mapping :get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('observer') else ()
              )
            multiValue: true
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess')
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.global.observer') then ('arn:aws:iam::aws:policy/job-function/ViewOnlyAccess') else ()
              )
            multiValue: true
          - path: |
              (
                if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.123456789012.admin') then ('arn:aws:iam::aws:policy/AdministratorAccess') else ()
              )
            multiValue: true
