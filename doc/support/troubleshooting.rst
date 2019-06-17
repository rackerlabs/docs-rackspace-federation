.. _troubleshooting-ug:

===============
Troubleshooting
===============

If you encounter issues when working with |service|, use the information
in this section to help you troubleshoot.


Need to save a SAML response in the Chrome browser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes your support technician might ask you to send them the SAML response
so that they can do additional troubleshooting for you.

Perform the following actions to save a SAML response in Chrome:

1. If you don't already have the SAML Chrome Panel, go to
   `Chrome Webstore - SAML Chrome Panel
   <https://chrome.google.com/webstore/detail/saml-chrome-panel/paijfdbeoenhembfhkhllainmocckace>`_
   and click **Add to Chrome** to get and install the plug in.

2. In Chrome, press **F12** to start the Developer Console.

3. Select the **Network** tab and click **Preserve Log** to add a check mark.

4. Select the SAML tab.

5. Reproduce the issue.

6. Look for any unique SAML posts in the **SAML Response** section. Select that
   row, and then copy the XML-formatted SAML response.

7. Paste the response to send to the support technician.

Problems creating an Identity Provider
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check the following items:

- Was your metadata XML file downloaded correctly and not corrupted in
  any way?
- Is your Login Domain unique? For example, `mycompany.com` can't be used
  for more than one |idp|, even if the domains are in different Rackspace
  accounts.


Problems logging in
~~~~~~~~~~~~~~~~~~~

After entering your email address into `login.rackspace.com/federate
<https://login.rackspace.com/federate>`_, were you not redirected to your
identity system login or were you not redirected back to Rackspace after a
successful credential entry?

If not, consider the following:

- Check the Login Domain for your |idp| to ensure that you have set the
  correct one and that it is a valid email address domain. (For example,
  ``mycompany.com``, not ``user@mycompany.com``).
- Try updating your |idp| metadata by re-downloading the metadata file from
  your third-party SAML provider and updating it in the |idp| details
  page.
- Verify the Rackspace federation details that you entered into your
  third-party SAML provider. For examples, see
  :ref:`index-configuring-3p-saml-ug`.


Problems with roles or access
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After successfully logging in, were you able to access the products or
services that you expected to find?

Consider the following items:

- Review your |amp| to ensure you are assigning values to the `roles`
  parameter. Look at examples in :ref:`index-configuring-3p-saml-ug`, and
  :ref:`config-attribute-mapping-ug`.
- If you are using Fanatical Support for AWS, review the **Fanatical Support
  for AWS** section in :ref:`faws-mapping-ug`. If things look correct, contact
  your **Fanatical Support for AWS** account manager or support team for
  further guidance.


Other issues or questions
~~~~~~~~~~~~~~~~~~~~~~~~~

Contact Rackspace Support as covered in :ref:`getting-support-ug`.

Have the following information available when contacting support:

- Account number for which you set up the |idp|
- The third-party SAML provider being used
- The Login Domain that you set up for your |idp|
- Browser or browsers that you use to access the Control Panel and Login
  portals
- Any |amp| changes or versions that you have created
- Any error messages that you encountered during |idp| set up or when logging
  in
