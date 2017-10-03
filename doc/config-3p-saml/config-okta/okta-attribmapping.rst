.. _okta-attribmapping-ug:

==========================
Attribute Mapping for Okta
==========================

In the SAML attributes/assertions sent to Rackspace during login, you may need
to customize the mappings for information that Okta provides to properly align
to Rackspace values.

Be sure to validate the following items in your |amp|:

- The Okta groups users belong to that you want to map to specific
  Rackspace permissions. You can do this while configuring the SAML application
  in the **"Group Attribute Statements"**, or edit an existing application by
  going to your Okta Admin panel and modifying the **"Group Attribute
  Statements"**.
- The ``expire`` value/path
- The ``email`` value/path

For example, if you want to include all groups a user belongs to that have the
word ``rackspace`` in your SAML assertions, add a field with an appropriate
name like ``groups`` and select a regex filter with the value ``*rackspace*``.

.. image:: create_app_5.png

TODO: Gabe to REVISE this


An example |amp| that should work with Okta defaults is below.

Notes:

- Remember to update *at a minimum* the ``domain`` value to your Identity
  Domain from the |idp| details page.
- Validate that any values being mapped to ``email`` and ``expire`` are
  properly specified for your specific SAML attributes/assertions. For example,
  in policy below, ``email`` is being set using the ``path``/``"{Pt}`` syntax
  in the |amp| language to point to the ``NameID`` attribute in the SAML
  assertion.

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


