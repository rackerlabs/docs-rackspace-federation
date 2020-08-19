.. _okta-attribmapping-ug:

Attribute mapping for Okta
--------------------------

Mapping users at Okta to permissions at Rackspace is necessary to ensure
your users have access to the applications and permissions that they need.

Mapping Okta groups to Rackspace
--------------------------------

Assigning permissions based on groups is an efficient way to ensure you assign
permissions properly to multiple users. You can use existing Okta groups or
create Rackspace specific groups at Okta.

To send Okta group information to Rackspace, you can configure
the **Group Attribute Statements** in your SAML application by completing the
following steps:

1. Log in to your organization's Okta account by using your organization's
   sign-in page.

2. Click **Applications** located on the top ribbon.

3. Click your Rackspace Federation application. If you have not yet set
this up, see instructions in the :ref:`Configure Rackspace Federation at
Okta<okta-setup-ug>` section.

4. Click the **General** tab for the application.

5. Scroll down to **SAML settings** section and click the **Edit** button. Skip
the first page of the SAML wizard by clicking **Next**.

6. In the section **Group Attribute Statements (Optional)**, enter a
name for the group attribute statement in the **Name** field.

7. Leave **Name format** set to **Unspecified**.

8. Choose a **Filter** option and enter the necessary details. For
example, if you want to include all the user's groups that have the
word ``rackspace`` in your SAML assertions, add a field with an
appropriate name like ``groups``, and select a regex filter with the
value ``.*rackspace.*``.

9. Click **Next** then click **Finish**.

Mapping Rackspace permissions to Okta groups
--------------------------------------------

This section details how to map Okta groups to specific Rackspace attribute
mapping policies. Attribute mapping policies determine the Rackspace roles and
permissions assigned to Okta groups.

Update your Rackspace XML (``.xml``) attribute mapping policy by using the
following steps:

1. Log in to the `Rackspace Customer Portal <https://login.rackspace.com>`_.

2. In the upper right area of the navigation bar, select
   **Account > User Management** from the drop-down menu. Alternately, browse
   directly to the `User Management <https://account.rackspace.com/users>`_
   page in the Rackspace Account Management Control Panel.

3. In the sub-navigation bar, select **Identity Federation**.

4. Edit your Identity provider by clicking the name of the provider.

5. Scroll down to the **Attribute Policy Mapping** section.

6. Download the ``.yml`` file and make any applicable edits. See the next
section for an attribute policy mapping example ``.yml`` configuration.

7. Click the **Update** button and upload the updated ``.yml`` file.

Attribute policy mapping example
--------------------------------

The following example shows both Rackspace XML (``.xml``) and YAML (``.yml``)
attribute mapping policy that you can use when you configure your
identity provider with Rackspace. This example assumes that you have a group
named ``rackspace-billing`` with users that you want to access Rackspace
billing services by using the ``billing:admin`` Rackspace role. See
:ref:`Rackspace Cloud roles<required-mapping-ug>` for a full list of all
Rackspace roles.

Notes:

- Change the ``groups`` specified in the example to match your
  configured Okta groups.
- Any XML or YAML group name must match your Okta group name exactly.
- At a minimum, remember to update the example's ``domain`` value to your
  Identity domain on the |idp| details page.
- Validate that any values mapped to ``email`` and ``expire`` are
  properly specified for your specific SAML attributes or assertions. For
  example, in the following example policy, ``email`` is set by using the
  *path* (``"{Pt}"``) syntax in the |amp| language to point to the ``NameID``
  attribute in the SAML assertion.

XML Example:

.. code-block:: xml

      1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      2 <rules>
      3 <rule>
      4 <local>
      5 <user>
      6 <domain value="your_domain_id_goes_here"/>
      7 <name value="{D}"/>
      8 <email value="{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"/>
      9 <roles value="{0}"/>
      10 <expire value="PT4H"/>
      11 </user>
      12 <faws xsi:type="LocalAttributeGroup"> <groups value="{Ats(groups)}"multiValue="true" xsi:type="LocalAttribute"/>
      13 </faws>
      14 </local>
      15 <remote>
      16 <attribute path="(if (mapping:get-attributes('groups')='rackspace-billing') then 'billing:admin' else ())" multiValue="true"/>
      17 </remote>
      18 </rule>
      19 </rules>
      20 </mapping>

YAML Example :

.. code-block:: yaml

    mapping:
      version: RAX-1
      rules:
        - local:
            faws:
              groups:
                multiValue: true
                value:
                  - "{Ats(groups)}"
            user:
              domain: "your_domain_id_goes_here"
              # Update to your Identity Domain from the Identity Provider details page
              email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
              expire: PT4H
              # This would configure a maximum session duration of four hours,
              # you may wish to set this to a SAML provided value
              name: "{D}"
              # This value matches to the SAML attribute "name" by default.
              roles:
                - "{0}"
          remote:
            - multiValue: true
              path: |
                  (
                    if (mapping:get-attributes('groups')='rackspace-billing')
                    then    'billing:admin' else ()
                  )
              # Substitute these example groups with your own groups.

See :ref:`Required SAML attributes<required-mapping-ug>` for a detailed
breakdown of each section of the XML configuration.

Be sure to validate and modify the following items in your policy |amp|:

- The Okta groups that users belong to and to which you want to map
  specific Rackspace permissions
- The ``expire`` value/path
- The ``email`` value/path

|ampref|
