.. _faws-mapping-ug:

Assigning Fanatical Support for AWS permissions
-----------------------------------------------

After configuring your |idp| and any basic Attribute Mapping that needs
to occur, you need to complete additional steps to assign permissions for
accounts managed by **Fanatical Support for AWS**.

Update Attribute Mapping Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you need to ensure that you have added a ``rules`` section to your
|amp| that indicates that any ``groups`` provided in your SAML response will be
applied to your AWS account permissions.

The following example shows a basic policy with the required rules included:

.. code-block:: yaml

    mapping:
      rules:
        -
          local:
            user:
              domain: "{D}"
              email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
              expire: "{D}"
              name: "{D}"
            faws:
              groups:
                multiValue: true
                value: "{Ats(http://schemas.xmlsoap.org/claims/Group)}"
      version: RAX-1

The lines below ``faws`` indicate that any value associated with the SAML
schema attribute ``http://schemas.xmlsoap.org/claims/Group`` will be assigned
to the ``faws/groups`` local value.

.. note::
    If you use a different SAML attribute to provide a ``groups`` value, or
    something similar, substitute that attribute instead.


Cloud Tenant Role Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following example shows a basic policy with cloud tenant roles to configure rackspace identity permissions for specific AWS accounts:


.. code-block:: yaml


    mapping:
      rules:
        -
          local:
            faws:
              groups:
                multiValue: true
                value:
                  - "{Ats(groups)}"
            user:
              domain: "{D}"
              email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
              expire: "PT12H"
              name: "{D}"
              roles:
                - "{0}"
          remote:
            -
              multiValue: true
              path: |
                  (
                    if (mapping:get-attributes('groups')='your_sso_ldap_group') then ('role_name/insert_tenant_id_here')     else ()
                  )
      version: RAX-1

.. note::
    If you use a different SAML attribute to provide a ``groups`` value, or
    something similar, substitute that attribute instead.


Contact Fanatical AWS support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After your |amp| has the correct section added, contact your **Fanatical
Support for AWS** support or account team, and they will help you further
configure the specific AWS permissions and roles needed for controlling
federated user access.
