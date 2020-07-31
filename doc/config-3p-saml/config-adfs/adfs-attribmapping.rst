.. _adfs-attribmapping-ug:

Attribute mapping for ADFS
--------------------------

The normal method for mapping ADFS users to Rackspace roles or permissions is
to use ADFS Groups. This guide gives an example of setting up your |amp| to
send both the ADFS Groups to which users belong and user information as SAML
assertions for proper mapping.


Use the following steps for ADFS attribute mapping:

1. Go to the **Claim rules** for the Rackspace relying-party trust that you
   set up, as shown in the following image:

.. image:: ../../_images/Config-ADFS/ADFS_Step4_edited.png

|

2. Add a new rule for the **LDAP Attribute** named
   **Token-Groups - Unqualified Names** with an **Outgoing Claim Type** of
   **Group**, as shown in the following image:

|

.. image:: ../../_images/Config-ADFS/claims_groups_7.png

|

To learn more about customizing how you include Active Directory group
membership in your SAML attributes, see
`https://msdn.microsoft.com/en-us/library/ff359101.aspx
<https://msdn.microsoft.com/en-us/library/ff359101.aspx>`_

The following example shows both Rackspace XML (``.xml``) as well as YAML (``.yml``) Attribute Mapping
Policy that you can use when you configure your Identity Provider with
Rackspace. This example assumes that you have a group named
``rackspace-billing`` with users who you want to access Rackspace billing
services by using the ``billing:admin`` Rackspace role.

More information
~~~~~~~~~~~~~~~~

When you map ADFS users to Rackspace roles or permissions, ensure that you
perform the following tasks:

- Change the ``groups`` specified in the example to match your
  configured outgoing claim type for the ADFS groups.
- At a minimum, remember to update the example's ``domain`` value to your
  Identity Domain, which is found on the **Details** page for the |idp|.
- Validate that any values that are mapped to ``email`` and ``expire`` are
  properly specified for your specific SAML attributes or assertions. For
  example, in the following example policy, ``email`` is set by using the
  *path* (``"{Pt}"``) syntax in the |amp| language to point to the ``NameID``
  attribute in the SAML assertion, as shown in the following example:

XML Example:

.. code-block:: XML

  <?xml version="1.0" encoding="UTF-8"?>
  <mapping xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:xs="http://www.w3.org/2001/XMLSchema"
          xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules"
          version="RAX-1">
    <rules>
        <rule>
        <local>
              <user>
                <domain value="your_domain_id_goes_here"/>
                <name value="{D}"/>
                <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
                <roles value="{0}" multiValue="true"/>
                <expire value="PT4H"/>
              </user>
              <faws xsi:type="LocalAttributeGroup">
                <groups value="{Ats(http://schemas.xmlsoap.org/claims/Group)}"
                        multiValue="true"
                        xsi:type="LocalAttribute"/>
              </faws>
        </local>
          <remote>
              <attribute 
                    path="(
                        if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='rackspace-billing')then    'billing:admin' else ()
                        )"
                        multiValue="true"/>
          </remote>
        </rule>
    </rules>
  </mapping>

YAML Example:

.. code-block:: yaml

    mapping:
      rules:
        - local:
            faws:
              groups:
                multiValue: true
                value: "{Ats(http://schemas.xmlsoap.org/claims/Group)}"
            user:
              domain: "your_domain_id_goes_here"
              # Update to your Identity Domain from the Identity Provider details page
              email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
              expire: PT4H
              # This would configure a maximum session duration of 4 hours, you might want to set this to a SAML-provided value
              name: "{D}"
              # This value matches to the SAML attribute "name" by default.
              roles:
                - "{0}"
              # This substitution states to take the value of the return from the first element of the remote role.
          remote:
            - multiValue: true
              path: |
                  (
                    if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='rackspace-billing')then    'billing:admin' else ()
                  )
              # The groups specified here are examples. You should substitute your own groups.
      version: RAX-1
- Ensure that you validate and modify the following items in your own |amp|:

  - The ADFS groups that users belong to and to which you want to
    map specific Rackspace permissions
  - The ``expire`` value or path
  - The ``email`` value or path

|ampref|
