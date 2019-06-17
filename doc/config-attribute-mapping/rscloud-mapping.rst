.. _rscloud-mapping-ug:

Assigning Rackspace permissions
-------------------------------

This section provides examples of assigning Rackspace permissions.

Basic example
~~~~~~~~~~~~~

All Rackspace permissions for federated users are granted through roles
that you assign in the |amp|.

The following code shows a basic example of an |amp| for
Rackspace Cloud:

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
assigned to any users who log in by using this |idp| and |amp|.

For basic Identity Federation setups, this basic setup may be sufficient.

For a full list of Rackspace Cloud product roles, see :ref:`full-roles-ug`.

For detailed information about Rackspace roles for Dedicated Hosting accounts,
use the following steps to access the MyRackspace Customer Portal Permissions
Guide:

 1. Log in to the `MyRackspace Portal <https://login.rackspace.com>`_.
 2. In the subnavigation bar, select **Account > Permissions**.
 3. On the **Permissions** page, click **Permissions Guide** in the top-right
    corner.

Permissions by groups
~~~~~~~~~~~~~~~~~~~~~

For more complex scenarios, especially where access to Rackspace
products is governed by roles or groups defined in your corporate identity
system, the |amp| language provides more flexible control.

Permissions by groups example - Cloud
-------------------------------------

The following code shows a complex example of an |amp| for Rackspace Cloud:

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

Permissions by groups example - Dedicated Hosting
-------------------------------------------------

The following code shows a complex example of an |amp| for Dedicated
Hosting:

.. code:: yaml

    mapping:
    rules:
    -
      local:
        user:
          domain: "{D}"
          email: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
          expire: PT12H
          firstName: "{D}"
          groups: "{0}"
          lastName: "{D}"
          name: "{D}"
      remote:
        -
          multiValue: true
          path: |
              (
                if (mapping:get-attributes('groups')='admin_group') then ('user-group-admin') else (),
                if (mapping:get-attributes('groups')='user_group') then ('user-group-user') else (),
                if (mapping:get-attributes('groups')='low_group') then ('user-group-low') else ()
              )
    version: RAX-1
