.. _okta-attribmapping-ug:

==========================
Attribute Mapping for Okta
==========================

In the SAML attributes/assertions sent to Rackspace during login, you may need
to include the Okta groups users belong to that you want to map to specific
Rackspace permissions. You can do this while configuring the SAML application
in the **"Group Attribute Statements"**, or edit an existing application by
going to your Admin panel and modifying the **"Group Attribute Statements"**.


For example, if you want to include all groups a user belongs to that have the
word ``rackspace`` in your SAML assertions, add a field with an appropriate
name like ``groups`` and select a regex filter with the value ``*rackspace*``.

.. image:: create_app_5.png

TODO: Gabe to REVISE this


An example |amp| that should work with Okta defaults is below. Remember to
update *at a minimum* the ``domain`` value to your Identity Domain from the
|idp| details page.

.. code-block:: yaml

    ---
    mapping:
    rules:
        -
        local:
            user:
            domain: "DOMAIN HERE"
            email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
            expire: PT12H
            name: "{D}"
            roles:
                - "{0}"
        remote:
            -
            multiValue: true
            path: |
                (
                    if (mapping:get-attributes('groups')='rax-medium-access-mycloud') then ('nova:admin', 'ticketing:admin') else (),
                    if (mapping:get-attributes('groups')='rax-observer-mycloud') then 'billing:admin' else ()
                )
    version: RAX-1


