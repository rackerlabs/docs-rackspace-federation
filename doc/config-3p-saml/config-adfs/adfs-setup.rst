.. _adfs-setup-ug:

================
Configuring ADFS
================

These are the steps to setup an ADFS relying party trust with Rackspace.

1. Click on the 'Add Relying Party Trust' in your ADFS Management Console.

.. image:: adfs_1_add_trust.png

|

2. You can import the Rackspace metadata by using the url
`https://login.rackspace.com/federate/sp.xml
<https:login.rackspace.com/federate/sp.xml>`_ or by downloading the
`https://login.rackspace.com/federate/sp.xml
<https:login.rackspace.com/federate/sp.xml>`_ metadata file locally and then
uploading it in ADFS.

.. image:: ADFS_step_2.png

|

3. Once you have created the Rackspace relying party trust, edit the claim
rules for that trust.

.. image:: ADFS_Step4_edited.png

|

4. Create an "Issuance Transform Rules" claim rule for email, and use the
User-Principle-Name as an outgoing claim type.

.. image:: email.png

|

5. Create outgoing claim of type Name ID.

.. image:: name_id.png
