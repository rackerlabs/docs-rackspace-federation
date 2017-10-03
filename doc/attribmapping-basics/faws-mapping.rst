.. _faws-mapping-ug:

===============================================
Assigning Fanatical Support for AWS Permissions
===============================================

After configuring your |idp|, and any basic Attribute Mapping that needs
to occur, you will need to complete additional steps to assign permissions for
accounts managed by Fanatical Support for AWS.

Update Attribute Mapping Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you will need to ensure that you have added a rule section to your
|amp| that indicates that any ``groups`` provided in your SAML response will be
applied to your AWS account permissions.

This example shows a basic policy with the required rule included:

.. code:: yaml

    mapping:
    version: RAX-1
    rules:
    - local:
        user:
            domain: '99199991999'
            email: "{At(urn:oid:1.2.840.113549.1.9.1.1)}"
            expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"
            name: "{D}"
            roles:
                - "nova:admin"
        faws:
            groups:
                multiValue: true
                value: "{Ats(http://schemas.xmlsoap.org/claims/Group)}"

The lines below ``faws`` indicate that any value associated with the SAML
schema attribute ``http://schemas.xmlsoap.org/claims/Group`` will be assigned
to the ``faws/groups`` local value. If you use a different SAML attribute to
provide a ``groups`` value, or similar, substitute that attribute instead.

Contact Fanatical AWS Support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once your |amp| has the correct section added, contact your Fanatical Support
for AWS support or account team, and they will help you further configure the
specific AWS permissions and roles needed for controlling federated user
access.
