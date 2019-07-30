
.. table:: Attributes mapped to XPaths

   +-----------+----------------------------------------------------------------------------------------------------------------------+
   | Attribute | SAML Assertion Location (XPath)                                                                                      |
   +===========+======================================================================================================================+
   | domain    | /saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue[1]    |
   +-----------+----------------------------------------------------------------------------------------------------------------------+
   | name      | /saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID                                                          |
   +-----------+----------------------------------------------------------------------------------------------------------------------+
   | email     | /saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue[1]     |
   +-----------+----------------------------------------------------------------------------------------------------------------------+
   | roles     | /saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue        |
   +-----------+----------------------------------------------------------------------------------------------------------------------+
   | expire    | /saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter |
   +-----------+----------------------------------------------------------------------------------------------------------------------+
