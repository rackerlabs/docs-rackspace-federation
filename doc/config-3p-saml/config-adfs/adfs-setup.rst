.. _adfs-setup-ug:

================
Configuring ADFS
================

These are the steps to setup an ADFS relying party trust with Rackspace.

1. Click on the 'Add Relying Party Trust' in your ADFS Management Console.

.. image:: adfs_1_add_trust.png

|

2. You can import the rackspace metadata via the url
`https://login.rackspace.com/federate/sp.xml
<https:login.rackspace.com/federate/sp.xml>`_ or by downloading the
``sp.xml`` metadata file locally and uploading it in ADFS.

.. image:: ADFS_step_2.png

|

3. Once you have created the rackspace relying party trust, edit the claim
rules for that trust.

.. image:: ADFS_Step4_edited.png

|

4. Create a claim rule for email and use the User-Principle-Name as an outgoing
claim type.

.. image:: email.png

|

5. Create outgoing claim of type Name ID.

.. image:: name_id.png
