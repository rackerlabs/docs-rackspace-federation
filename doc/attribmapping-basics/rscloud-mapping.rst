.. _rscloud-mapping-ug:

Assigning Rackspace Cloud permissions
-------------------------------------

This section provides examples of assigning Rackspace Cloud permissions.

Basic example
~~~~~~~~~~~~~

All Rackspace Cloud permissions for federated users are granted through roles
assigned in the |amp|.

A basic example of an |amp| follows:

.. code:: yaml

    mapping:
     version: RAX-1
     rules:
     - local:
        user:
           domain: '999994919999'
           email: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress)}"
           expire: "PT12H"
           name: "{D}"
           roles:
            - "admin"
            - "ticketing:admin"

In this example, the ``admin`` and ``ticketing:admin`` roles are explicitly
assigned to any users logging in using this |idp| and |amp|. (Refer to the
:ref:`full-roles-ug` for a full list of Rackspace Cloud product roles.)

For basic Identity Federation setups, this may be sufficient.

Permissions by groups example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For more complex scenarios, especially where access to Rackspace Cloud
products is governed by roles or groups defined in your corporate identity
system, the |amp| language provides more flexible control.

A complex example of an |amp| follows:

.. code:: yaml

    mapping:
     version: RAX-1
     rules:
     - local:
         user:
            domain: '9999953939'
            email: "{At(urn:oid:1.2.840.113549.1.9.1.1)}"
            expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Conditions/@NotOnOrAfter[1])}"
            name: "{D}"
            roles:
            - "{0}"
       remote:
       - path : |
           (
               if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.rackspace.admin') then ('billing:admin', 'ticketing:admin','admin') else (),
               if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.rackspace.billing') then 'billing:admin' else (),
               if (mapping:get-attributes('http://schemas.xmlsoap.org/claims/Group')='mycompany.rackspace.ticketing') then 'ticketing:admin' else ()
           )
         multiValue: true

This example uses the substitution and piping features of the |amp|, in
conjunction with XPath, to observe the SAML ``groups`` value and to assign
values to the local ``role`` value based on any matching scenarios. (The
``{0}`` indicator under ``roles`` causes the resultant value(s) of the
first ``remote`` rule to be substituted in its place.)

|ampref|
