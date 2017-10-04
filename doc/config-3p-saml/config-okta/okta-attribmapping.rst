.. _okta-attribmapping-ug:

==========================
Attribute Mapping for Okta
==========================

In the SAML attributes you will need to include the groups users belong to that you want to map into Rackspace permissions. You can do this while configuring the SAML application in the "Group Attribute Statements" or edit an existing application by going to your Admin panel and modifying the "Group Attribute Statements"


For example, if you want to include all groups a user belongs to that have the word "rackspace" in your SAML, add a field with an appropriate name like "groups" and select a regex filter with the value `*rackspace*`

.. <insert screenshot 5> is this needed?


The following is an example .yml policy you can use when you configure your attribute mapping policy with rackspace. This assumes you have a group named "rackspace-billing" with users you want to access rackspace billing services using the 'billing:admin' rackspace role.

.. code:: yaml
    ---
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
              domain: "your_account_number_goes_here"
              email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
              expire: PT4H # this would configure a maximum session duration of 4 hours
              name: "{D}"
              roles:
                - "{0}"
          remote:
            -
              multiValue: true
              path: |
                  (
                    if (mapping:get-attributes('groups')='rackspace-billing') then    'billing:admin' else ()
                  )
      version: RAX-1
