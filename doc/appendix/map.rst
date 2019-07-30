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
   :emphasize-lines: 2,34,58,64,70

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

Parts of the SAML Assertion
---------------------------

This section describes the relevant parts of the previous SAML assertion.
According to the SAML protocol, the assertion itself is wrapped in a
``<saml2p:Response />`` element which begins on line 2.  The actual assertion
(``<saml2p:Assertion />``) begins on line 34.

- **Issuer (37)**: The issuer is the system that generated (or issued) the
  assertion. This is identified as a URI.

- **Signature (38 - 57)**: The XML signature of the assertion part of the
  request. The signature is used to verify that that the assertion was indeed
  produced by the issuer.

- **Subject (58 - 63)**: The subject is used to identify the identity (or user)
  that the assertion is about.

- **AuthnStatement (64 - 69)**: The AuthnStatement contains details on how the
  subject is authenticated.

- **AttributeStatement (70 - 91)**: This section contains a list of arbitrary
  attributes associated with the subject. Each attribute in the list is a
  name/value pair. The values are of a type identified by the ``xsi:type``
  XML attribute. In this case, they are all strings. Attributes can have
  multiple values.  The groups attribute defined in lines 80 - 84, for example,
  contain 3 separate values (group1, group2, and group3).

Signing SAML Assertions
-----------------------

Both the SAML Response on line 2 and the SAML Assertion on line 34 might be
signed. Rackspace Identity might verify both signatures. However, you should
note that while signing the SAML Response is optional, signing the
SAML Assertion is strictly required. This means that a message that
contains a single signature at the SAML Response level will be rejected.

It is possible for a SAML Response to contain multiple assertions. In
this case, all assertions must be signed, and they must all be issued
by the same issuer.  Rackspace Identity typically examines only the
first assertion for authorization data, but this behavior can be easily
overwritten with a mapping policy.

Required Attributes
===================

Following are descriptions of the attributes that Rackspace Identity requires
in order to successfully authenticate a user including line references to the
preceding SAML Response example.

Domain
------

Rackspace Identity keeps information about users, roles, and other
entitlements in a domain. When a user federates into Rackspace, the
user is placed in a single identity domain. Each domain is accessed
by using a unique alpha-numeric ID, which Rackspace usually sets to be the
same the user's account ID. The domain ID is required when a federated user
requests access. This is especially important because a customer is
allowed to create multiple domains and Rackspace Identity needs to
place the federated user in the correct one.

In the preceding SAML Assertion, the domain is passed as a SAML attribute
in lines 74 - 76. This implies that the identity provider was
pre-configured to emit the correct value. It is not strictly required
that the |idp| do this because most federated users target a single domain, and
the domain value can be easily hard coded in an attribute mapping policy.

Name
----

This is the username of the federated user.  Rackspace Identity
assumes that each user is identified with a unique username, and
that the same user has the same username from one federated
login to the next.

In the preceding SAML Assertion, the username is identified by the
``NameID`` in the *Subject* section of the assertion (line 59). That
said, an IDP can return a stable username as an attribute in the
*AttributeStatement* section.

Email
-----

This is the email of the federated user.  In the SAML assertion, it is
identified as an attribute in the *AttributeStatement* section (lines
77 - 79). Some Identity Providers make no distinction between a username and
an email, in which case the email is located in the *Subject* section.

Roles
-----

Rackspace Identity expects an attribute with the list of roles that should
be assigned to the federated user.  Rackspace Identity
only allows roles that it recognizes to be assigned.

In the SAML Assertion above the list of roles is specified in an
attribute named ``roles`` on lines 71 - 73.  Note that this is a
good use case for a multi-value attribute, but in this case, we only
assign the ``nova:admin`` role.

The example assigns ``nova:admin`` at the domain level. This
means that the user will have the role on all accounts (sometimes
referred to as tenants) that are associated with the domain.  For
example, if accounts ``12873`` and ``33987`` are associated with the
domain, the user will have the ``nova:admin`` role on both of those
accounts.

It is possible to restrict the role to a specific account.  For
example, to grant ``nova:admin`` on account ``12873`` and
``nova:observer`` on account ``33987``, you should specify the roles
as ``nova:admin/33987`` and ``nova:observer/33987``.

Expire
------

Finally, Rackspace identity needs to understand the amount of time
that a federated user should be allowed on Rackspace systems before
the user is forced to re-authenticate.  This attribute can be
expressed in two different formats. First, you can use an `ISO 8601`_
timestamp, which should include a time zone designator.
For example, the timestamp ``2017-10-04T16:20:57Z`` signifies that the
user should be forced to re-authenticate after October 4th 2017 at
16:20:57 UTC.  Second, you can use an `ISO 8601`_ duration.
For example, ``PT1H2M`` signifies that the user should be forced to
re-authenticate one hour and two minuets after successfully logging
in.

In the preceding SAML Assertion example, an expire timestamp is specified
in the ``NotOnOrAfter`` attribute of the SubjectConfirmationData on line 61.
In SAML, this attribute denotes the time after which the
SAML Assertion should no longer be considered valid. While this
timestamp does not fit semantically with the expire attribute that
Rackspace Identity expects, it still works as a reasonable default.

Other Attributes
----------------

The attributes described in the previous sections (domain, name,
email, roles, and expire) are expected in every federated login. Some
Rackspace products may expect additional optional attributes. Please
consult this document for details on these attributes.

Mapping Attributes
==================

The SAML Assertion attributes can be found on the following lines of the
in the preceding example:

.. include:: tables/loc.rst

The values at those locations are listed here:

+--------+--------------------------+
| domain | 323676                   |
+--------+--------------------------+
| name   | john.doe                 |
+--------+--------------------------+
| email  | john.doe@rackspace.com   |
+--------+--------------------------+
| roles  | - nova:admin             |
+--------+--------------------------+
| expire | 2017-11-17T16:19:06.298Z |
+--------+--------------------------+

In a sense, this table represents an attribute mapping. It maps
data located in the SAML Assertion into attributes that
Rackspace Identity requires to log in a federated user.  This is a
silly mapping, however, because mapping attributes by referring to
line numbers is extremely unpractical, inexact, and brittle. Using
XPath, on the other hand, is a more stable and practical way of
pinpointing the exact location of the data that we need. After all,
XPath was designed specifically to pinpoint and extract data form XML
documents [#j1]_.

Mapping attributes with XPath
-----------------------------

The following table replaces line numbers with XPaths into the SAML
Assertion.

.. include:: tables/xpath.rst


You can turn this table into an attribute mapping policy, as shown in the
following example:

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

   mapping:
     version: RAX-1
     description: |-
       Simple policy where we select required attributes via an XPath.
     rules:
     - local:
         user:
           domain: "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue[1])}"
           name:   "{Pts(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
           email:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue[1])}"
           roles:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
           expire: "{Pts(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

The following section walks through the preceding policy in detail and
examines how the code uses XPath to extract the attribute values.

Parts of the mapping policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The mapping policy is YAML document that contains instructions to
retrieve (or derive) identity attributes from a SAML Assertion.
You can think of it as a simple script that executes every
time a SAML Assertion is presented to Rackspace Identity. This
section breaks the preceding mapping policy into its relevant parts.

- **mapping (2)**: The mapping policy is always contained in a single
  top-level ``mapping`` object.

- **version (3)**: The ``version`` key identifies the version of the mapping
  policy language.  This required attribute should always have the
    value of ``RAX-1``. The mapping policy language described here is
    based on the `Mapping Combinations`_ language by the OpenStack
    Keystone and the version name is used to differentiate a Rackspace
    Identity mapping policy from a Keystone mapping policy.

- **description (4)**: The ``description`` key provides a human readable
  description of the mapping policy. This description is optional.

- **rules (6)**: A mapping policy is made up of a collection of rules. These
  rules are encapsulated by the ``rules`` array.  A policy must contain at
  least one rule.

- **rule (7 - 13)**: These lines contain a rule that drives the policy. A
  rule may contain  a ``local`` and a ``remote`` section.  Both ``local``
  and ``remote`` sections are optional (in this case, we don't need a
  ``remote``), but there should be at least one rule with a ``local`` section.

  It's important to note that things are local or remote from the
  perspective of Rackspace Identity. For example, the ``local``
  section contains statements about the user within Rackspace Identity
  (the local user).  The remote section contains statements about the
  user as its presented by the |idp| (the remote user).

  Lines 8 - 13 describe what the local user should look like. In other
  words, they describe the attributes of the local user. Here, you
  specify each of the required identity attributes and describe how
  they can be obtained from an XPath.

Using XPath in the mapping policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Notice that the XPaths in the mapping policy are contained within
something that looks like this ``{Pts()}``. This is known as an
XPath substitution. There are various types of substitutions in the
mapping policy language each of which is encapsulated by curly braces
``{}``. Substitutions are only allowed in the local part of the
mapping policy. They help set values for local attributes in that they
are always replaced (or substituted) by other content. In the case of
the XPath substitution, the value is replaced by the result of the XPath
within the parenthesis when executed against the SAML Assertion.

It is important to note that spaces matter in a substitution.  In
order to be interpreted correctly there should be no spaces between
the prologue of the substitution ``{Pts(`` and the epilogue of the
substitution ``)}``.

.. include:: tables/pts.rst


Note that the XPath substitution refers to elements in
certain namespaces (``saml2p``, ``saml2``). These namespace prefixes,
which are common to SAML, are always predefined. The following table
describes how namespace prefixes are mapped by default:

.. include:: tables/namespaces.rst


A SAML Assertion can refer to elements in extended
namespaces not listed here. You can define additional prefixes by
adding a ``namespaces`` key to the mapping policy. In XPath, it's the
namespace URI that uniquely identifies an element. The prefix is
simply a short hand for this URI.  The following example replaces the
``saml2p`` prefix with ``foo``. Because the namespace URI bound to
element is the same as the previous example, the two mapping policies
produce the exact same result.

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

   mapping:
     version: RAX-1
     description: |-
       Simple policy where we select required attributes via an XPath.
     namespaces:
       foo: urn:oasis:names:tc:SAML:2.0:protocol
     rules:
     - local:
         user:
           domain: "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue[1])}"
           name:   "{Pts(/foo:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
           email:  "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue[1])}"
           roles:  "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
           expire: "{Pts(/foo:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Using the {Pt()} substitution
-----------------------------

Notice that the XPath for retrieving the domain ends with
``saml2:AttributeValue[1]``, while the XPath for retrieving the list of
roles ends with ``saml2:AttributeValue``.  This is because we want a single
value for a domain, but we want a list of roles.  Without the ``[1]`` at the
end, the XPath would return every ``AttributeValue`` in a SAML Assertion
for a domain. The ``[1]`` signifies that you are only interested
in the first value [#j2]_.

Because it is common to want to retrieve a single value, there's an
alternative XPath substitution (``{Pt()}``) that always returns the
first value of an XPath result. This is useful in cases where you
expect a single value, and you want to automatically protect against the
off chance that the operation might return multiple values in a SAML assertion.

Given this new substitution, you can rewrite the mapping policy as
follows:

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

   mapping:
     version: RAX-1
     description: |-
       Simple policy where we select required attributes via an XPath.
       We use {Pt()} instead of {Pts()} in single value attributes to
       avoid having to select the first attribute value in XPath.
     rules:
     - local:
         user:
           domain: "{Pt(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue)}"
           name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
           email:  "{Pt(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue)}"
           roles:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
           expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"


Using the mapping:get-attributes call
-------------------------------------

At this point, the XPaths for retrieving a domain, email, and roles
all look very similar. They all retrieve the list of values for an attribute
in the ``AttributeStatement`` of the assertion by name.  Because this is a
common case, there is a predefined XPath function called
``mapping:get-attributes`` that takes the name of a SAML attribute as
a string and returns the attribute values associated with that name.

Thus, you could rewrite the mapping policy using this function as follows:

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

   mapping:
     version: RAX-1
     description: |-
       Simple policy where we select required attributes via an
       XPath. Here we use the mapping:get-attributes call to return
       attribute values.
     rules:
     - local:
        user:
           domain: "{Pt(mapping:get-attributes('domain'))}"
           name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
           email:  "{Pt(mapping:get-attributes('email'))}"
           roles:  "{Pts(mapping:get-attributes('roles'))}"
           expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Using the {Ats()} and {At()} substitutions
------------------------------------------

Having an XPath function that retrieves SAML attribute values is handy
when you want to process those values in some way before returning a
result.  In some cases, however, you simply want to return the value
(or values) of a SAML attribute as is. The ``{Ats()}`` and ``{At()}``
substitutions, do just that. These are known as attribute
substitutions. As with their XPath counterparts, the ``{Ats()}``
substitution returns all values for a specific attribute name, and the
``{At()}`` returns just the first value.

Given these substitutions we can rewrite the policy as follows:

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

   mapping:
     version: RAX-1
     description: |-
       Simple policy where we select required attributes. We use At
       instead of Pts as a simple means of accessing an name SAML
       attribute.
     rules:
     - local:
         user:
           domain: "{At(domain)}"
           name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
           email:  "{At(email)}"
           roles:  "{Ats(roles)}"
           expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Notice that in this case the name of the attribute is not in single
quotes. We are no longer directly using XPath with attribute
substitutions, but under the hood, these substitutions are also
interpreted as XPath statements.

Using the {D} substitution
--------------------------

There are certain default places where a mapping policy looks for a
particular identity attribute value. The default substitution
(``{D}``) is always replaced with the value at the default
location. Unlike other substitutions, the default substitution is
interpreted differently depending on where the substitution is
placed. For example, if the substitution is placed as a value of the
``name`` attribute, it replaces itself with the default value for
name. Likewise, if ``{D}`` is placed in the ``email`` attribute, it
is replaced with the default value for email and so on.

What are the default locations in a SAML assertion for the five
attributes Rackspace Identity expects?  It turns out that the SAML
assertion in this example has all of the values in the default places!
So it's possible, in this case, to write a mapping policy that looks
like this:

.. highlight:: yaml
   :linenothreshold: 5
   :linenos:

    mapping:
     version: RAX-1
     description: |-
       The default policy.  All attributes are in the expected location
       in the SAML assertion.
     rules:
     - local:
         user:
           domain: "{D}"
           name:   "{D}"
           email:  "{D}"
           roles:  "{D}"
           expire: "{D}"

Next Steps
==========

This section explored the SAML Assertions, how to use
XPath and attribute names, and the expected defaults to write mapping
polices.  All of the policy examples, however, have described
simple direct mappings. The following Examples section, looks at
writing polices in cases where things don't align so perfectly.

.. References:

.. _rackspace identity federation user guide: http://developer.rackspace.com/docs/rackspace-federation

.. _list of allowed roles: https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/full-roles/

.. _iso 8601: https://en.wikipedia.org/wiki/ISO_8601

.. _mapping combinations: https://docs.openstack.org/keystone/latest/advanced-topics/federation/mapping_combinations.html

.. Footnotes:

.. [#j1] Later versions of XPath allow extracting data from JSON
   documents as well!

.. [#j2] The first index in XPath is 1 not 0.  This logical and makes
   sense unless you're a software developer.

