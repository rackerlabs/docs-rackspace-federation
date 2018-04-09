.. _okta-attribmapping-ug:

Attribute Mapping for Okta
--------------------------

If you want the user's groups to appear in the SAML attributes and assertions
sent to Rackspace (so that they can be mapped into permissions), you might need
to customize the group attribute statements that Okta uses to include group
membership. You can do this while configuring the SAML application in the
**Group Attribute Statements** or while editing an existing application by
going to the Admin panel and modifying the **Group Attribute Statements**.

For example, if you want to include all the user's groups that have the
word ``rackspace`` in your SAML assertions, add a field with an appropriate
name like ``groups``, and select a regex filter with the value
``.*rackspace.*``.

.. image:: ../../_images/Config-okta/create_app_5.png

|

The following example shows a Rackspace YAML (``.yml``) attribute mapping
policy that you can use when you configure your identity provider with
Rackspace. This example assumes that you have a group named
``rackspace-billing`` with users that you want to access Rackspace billing
services by using the ``billing:admin`` Rackspace role.

Notes:

- You should change the ``groups`` specified in the example to match your
  configured Okta groups.
- At a minimum, remember to update the example's ``domain`` value to your
  Identity Domain, which is found on the |idp| details page.
- Validate that any values that are mapped to ``email`` and ``expire`` are
  properly specified for your specific SAML attributes or assertions. For
  example, in the following example policy, ``email`` is set by using the
  *path* (``"{Pt}"``) syntax in the |amp| language to point to the ``NameID``
  attribute in the SAML assertion.


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
             # This would configure a maximum session duration of 4 hours, you may wish to set this to a SAML provided value
             name: "{D}"
             # This value will match to the SAML attribute "name" by default.
             roles:
               - "{0}"
         remote:
           - multiValue: true
             path: |
                 (
                   if (mapping:get-attributes('groups')='rackspace-billing')then    'billing:admin' else ()
                 )
             # The groups specified here are examples. You should substitute your own groups


Be sure to validate and modify the following items in your own policy |amp|:

- The Okta groups users that users belong to and to which you want to map
  specific Rackspace permissions.
- The ``expire`` value/path
- The ``email`` value/path

|ampref|
