========================
Attribute mapping basics
========================

Attribute mapping policies describe a means of extracting a set of
well known identity attributes from a signed SAML assertion produced
by an |idp|.  This section examines a sample SAML assertion in detail,
describes the attributes that need to be extracted from the assertion in order
for |service| to work, and walks through the construction of a
mapping policy that extracts those attributes from the assertion.

The SAML assertion
==================

When an |idp| successfully authenticates a user it
presents Rackspace Identity with a SAML assertion, much like the following:

.. code-block:: XML
   :linenos:

   <?xml version="1.0" encoding="UTF-8"?>
   <saml2p:Response xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
                 xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 ID="_7fcd6173-e6e0-45a4-a2fd-74a4ef85bf30"
                 IssueInstant="2017-11-15T16:19:06.310Z"
                 Version="2.0">
     <saml2:Issuer>http://test.rackspace.com</saml2:Issuer>
     <ds:Signature>
      <ds:SignedInfo>
         <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
         <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
         <ds:Reference URI="#_1105c5b8-28d4-45e5-a6e4-bc4179f5a111">
            <ds:Transforms>
               <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
               <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            </ds:Transforms>
            <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
            <ds:DigestValue>JaeeD+wkw297KPF+BKpdacRtaPmQKFD+DqXQ3JE3Aus=</ds:DigestValue>
         </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>kDBwO3xYaxITVS7ZZIy9AGOlk16Y5E0BlqU12QlRpWGfiwjtgok4p5q3YQS+N/Pxh84JUIjd7i+n0to/2yJyaCfoSA2SIUUf448lTtHNzVmjiC4WiUmUTRGaxUpsdcYUkjFAVAS40yGDBLXMYn/JYS4cbRV52/RTJ5smCCpqBMjgzhVaeAqJif/gXGjvMLl4RFN8JGvHZGzpjCb14UdKhVqfP0ZumLo4cLIWd3Ch49zRBQBgchbFqEJbTdPPLTJ4SMIEYm5RwX4PtQ2Ce94u8IGXkIhYf32H43l+955a35XGh37hcZMLZEzjk4FBMqSScupKqDej1c0m34MkeRGMlQ==</ds:SignatureValue>
      <ds:KeyInfo>
         <ds:X509Data>
            <ds:X509Certificate>MIIC6jCCAdKgAwIBAgIQE+gZKcmH841I4gYjUiHCSDANBgkqhkiG9w0BAQsFADAxMS8wLQYDVQQDEyZBREZTIFNpZ25pbmcgLSBhZGZzLmNvbnRvc293aWRnZXRzLmNvbTAeFw0xNjA2MTYwMDUyNTZaFw0xNzA2MTYwMDUyNTZaMDExLzAtBgNVBAMTJkFERlMgU2lnbmluZyAtIGFkZnMuY29udG9zb3dpZGdldHMuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAna30lllMTaivaXPCjrW7VcRI6BsPs0iVxV559I9UONENSldX9pYTPlqLzxTP1RAVzfbGNoSvNelXrc0cb6jslgi+0Ya0jxrj1CsxQDgLtZeZchwWUYnJgsvk/HHfXiQBrWPLaZbImPNVvzG1zlYoQyQHTe1Nvr1m5Lv9foruSnw4My2LP4M27ZLPGL7rLaqpBg0E9sMX0iIrucNNN6AdyyPsR8oAUtV//QB49pCk+/rb3UtSDyGrdFFD+sJBDiAXYjTGzzYxYnMjckBZQfPKMWRntGwe7lM1KkX7mtUr9pNSvX1mQS/PHxhIcvO7aWKc15FJKzFmtdAEM2mvZjnCtwIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBj5cSPBQGyICJZHsMXA2KddWxSUqtYSBDPWYVsW9gJSMYiJBjdEnR1aGpw5K6iYei7KCACH717VQNfEF64qnBCbOvWc7FmZ3V0n4plfyZYuexbbZqp7RTi+J1q2xPsdb8MB7138YhXCc3Uf1p0oEuw+hKZ5rt4srcfgxuEauKXhnaI/UAOWOgDslzTuku+ogPsHBkc7wfH2CS9UqA3JUVJksR42yMg/Y47DUTp0Ma02RoVIfjFh+y3lX01O7B3ccCCdiKaSxcnLQ8n/ypn7LBhUUWDWZVIBj1flioohFMc5gU2Jl2Ueki72yxOwKVehqYgBHLPZBCUQUJDnUsbJJgb</ds:X509Certificate>
         </ds:X509Data>
      </ds:KeyInfo>
     </ds:Signature>
     <saml2p:Status>
      <saml2p:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
     </saml2p:Status>
     <saml2:Assertion ID="_406fb7fe-a519-4919-a42c-f67794a670a5"
                    IssueInstant="2017-11-15T16:19:06.310Z"
                    Version="2.0">
      <saml2:Issuer>http://my.rackspace.com</saml2:Issuer>
      <ds:Signature>
         <ds:SignedInfo>
            <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <ds:Reference URI="#_a8a6920c-d4eb-467f-85df-6fa2767ae63d">
               <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
               </ds:Transforms>
               <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
               <ds:DigestValue>uQmSKQT03SRunafqzpb6v159a+jMVvTqmBCFYn17e7s=</ds:DigestValue>
            </ds:Reference>
         </ds:SignedInfo>
         <ds:SignatureValue>cYKqfE92hWqaPylJIcl89U9TKNzJXcFIPO0fvohg70zLB4JWnYlIKOz7S9XFUvS24mN47XS1T8DeR0IGITBMhqA/GCM624SOW0QjIRhQ9gh6/ONlyuAxGbVDo5tYb82sICFa9sMWI2Vr5ZH2LeTqyvsBRnlWBkZIw4hS2PBDHbhcnILUGX9uUDRcOrONAEMimnB7cNmZxSwQgdPfupyS39oedrUAiORa7GMII8GglWoj6Jy8SX0fQKXfsXD+wC5XFw76WAKJjSCuEkrXfxMQia/2H1tE24zNgZd6Y+uQ2Nh8YlUvO+DaMoj7mTKZUBqlxQt6It4kGH0+hfqvWx1MHQ==</ds:SignatureValue>
         <ds:KeyInfo>
            <ds:X509Data>
               <ds:X509Certificate>MIIC6jCCAdKgAwIBAgIQE+gZKcmH841I4gYjUiHCSDANBgkqhkiG9w0BAQsFADAxMS8wLQYDVQQDEyZBREZTIFNpZ25pbmcgLSBhZGZzLmNvbnRvc293aWRnZXRzLmNvbTAeFw0xNjA2MTYwMDUyNTZaFw0xNzA2MTYwMDUyNTZaMDExLzAtBgNVBAMTJkFERlMgU2lnbmluZyAtIGFkZnMuY29udG9zb3dpZGdldHMuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAna30lllMTaivaXPCjrW7VcRI6BsPs0iVxV559I9UONENSldX9pYTPlqLzxTP1RAVzfbGNoSvNelXrc0cb6jslgi+0Ya0jxrj1CsxQDgLtZeZchwWUYnJgsvk/HHfXiQBrWPLaZbImPNVvzG1zlYoQyQHTe1Nvr1m5Lv9foruSnw4My2LP4M27ZLPGL7rLaqpBg0E9sMX0iIrucNNN6AdyyPsR8oAUtV//QB49pCk+/rb3UtSDyGrdFFD+sJBDiAXYjTGzzYxYnMjckBZQfPKMWRntGwe7lM1KkX7mtUr9pNSvX1mQS/PHxhIcvO7aWKc15FJKzFmtdAEM2mvZjnCtwIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBj5cSPBQGyICJZHsMXA2KddWxSUqtYSBDPWYVsW9gJSMYiJBjdEnR1aGpw5K6iYei7KCACH717VQNfEF64qnBCbOvWc7FmZ3V0n4plfyZYuexbbZqp7RTi+J1q2xPsdb8MB7138YhXCc3Uf1p0oEuw+hKZ5rt4srcfgxuEauKXhnaI/UAOWOgDslzTuku+ogPsHBkc7wfH2CS9UqA3JUVJksR42yMg/Y47DUTp0Ma02RoVIfjFh+y3lX01O7B3ccCCdiKaSxcnLQ8n/ypn7LBhUUWDWZVIBj1flioohFMc5gU2Jl2Ueki72yxOwKVehqYgBHLPZBCUQUJDnUsbJJgb</ds:X509Certificate>
            </ds:X509Data>
         </ds:KeyInfo>
      </ds:Signature>
      <saml2:Subject>
         <saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">john.doe</saml2:NameID>
         <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml2:SubjectConfirmationData NotOnOrAfter="2017-11-17T16:19:06.298Z"/>
         </saml2:SubjectConfirmation>
      </saml2:Subject>
      <saml2:AuthnStatement AuthnInstant="2017-11-15T16:19:04.055Z">
         <saml2:AuthnContext>
            <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
            </saml2:AuthnContextClassRef>
         </saml2:AuthnContext>
      </saml2:AuthnStatement>
      <saml2:AttributeStatement>
         <saml2:Attribute Name="roles">
            <saml2:AttributeValue xsi:type="xs:string">nova:admin</saml2:AttributeValue>
         </saml2:Attribute>
         <saml2:Attribute Name="domain">
            <saml2:AttributeValue xsi:type="xs:string">323676</saml2:AttributeValue>
         </saml2:Attribute>
         <saml2:Attribute Name="email">
            <saml2:AttributeValue xsi:type="xs:string">john.doe@rackspace.com</saml2:AttributeValue>
         </saml2:Attribute>
         <saml2:Attribute Name="groups">
            <saml2:AttributeValue xsi:type="xs:string">group1</saml2:AttributeValue>
            <saml2:AttributeValue xsi:type="xs:string">group2</saml2:AttributeValue>
            <saml2:AttributeValue xsi:type="xs:string">group3</saml2:AttributeValue>
         </saml2:Attribute>
         <saml2:Attribute Name="FirstName">
            <saml2:AttributeValue xsi:type="xs:string">John</saml2:AttributeValue>
         </saml2:Attribute>
         <saml2:Attribute Name="LastName">
            <saml2:AttributeValue xsi:type="xs:string">Doe</saml2:AttributeValue>
         </saml2:Attribute>
      </saml2:AttributeStatement>
     </saml2:Assertion>
   </saml2p:Response>


The assertion describes a view of the user that has successfully
logged in. It contains within it all of the information deemed by the
|idp| to be relevant to the Service Provider, which is Rackspace.

.. note:: For help configuring Third Party identity providers (such as Active
   Directory Federation Services, Okta, and others) please refer to
   :ref:`Configure Third-Party SAML providers<index-configuring-3p-saml-ug>`.


