.. _okta-attribmapping-ug:

==========================
Attribute Mapping for Okta
==========================

In the SAML attributes you will need to include the groups users belong to that you want to map into Rackspace permissions. You can do this while configuring the SAML application in the "Group Attribute Statements" or edit an existing application by going to your Admin panel and modifying the "Group Attribute Statements"


For example, if you want to include all groups a user belongs to that have the word "rackspace" in your SAML, add a field with an appropriate name like "groups" and select a regex filter with the value `*rackspace*`

.. <insert screenshot 5> is this needed?

1. ref to task 1 sample

   Result of task 1.

2. ref to task 2 sample

   Result of task 2.

   For an example of a topic that uses this template, see

