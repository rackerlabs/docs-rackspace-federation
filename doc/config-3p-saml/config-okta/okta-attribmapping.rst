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

TODO: Gabe to insert Attrib map policy example here WITH GROUPS

