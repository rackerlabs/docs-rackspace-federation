========================
Attribute mapping basics
========================

Attribute mapping policies describe a means of extracting a set of
well-known identity attributes from a signed SAML assertion produced
by an |idp|.  This section provides the following information:

-  Examines a sample SAML assertion in detail.

-  Describes the attributes that need to be extracted from the assertion
   for |service| to work.
   
-  Walks through the construction of a mapping policy that extracts those attributes
   from the assertion.

The SAML assertion
==================

When an |idp| successfully authenticates a user, it
presents Rackspace Identity with a SAML assertion, much like the following:

.. code-block:: XML

    1  <?xml version="1.0" encoding="UTF-8"?>
    2  <saml2p:Response xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
    3                xmlns:xs="http://www.w3.org/2001/XMLSchema"
    4                xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
    5                xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion"
    6                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    7                ID="_7fcd6173-e6e0-45a4-a2fd-74a4ef85bf30"
    8                IssueInstant="2017-11-15T16:19:06.310Z"
    9                Version="2.0">
   10    <saml2:Issuer>http://test.rackspace.com</saml2:Issuer>
   11    <ds:Signature>
   12     <ds:SignedInfo>
   13        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
   14        <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
   15        <ds:Reference URI="#_1105c5b8-28d4-45e5-a6e4-bc4179f5a111">
   16           <ds:Transforms>
   17              <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
   18              <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
   19           </ds:Transforms>
   20           <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
   21           <ds:DigestValue>JaeeD+wkw297KPF+BKpdacRtaPmQKFD+DqXQ3JE3Aus=</ds:DigestValue>
   22        </ds:Reference>
   23     </ds:SignedInfo>
   24     <ds:SignatureValue>kDBwO3xYaxITVS7ZZIy9AGOlk16Y5E0BlqU12QlRpWGfiwjtgok4p5q3YQS+N/Pxh84JUIjd7i+n0to/2yJyaCfoSA2SIUUf448lTtHNzVmjiC4WiUmUTRGaxUpsdcYUkjFAVAS40yGDBLXMYn/JYS4cbRV52/RTJ5smCCpqBMjgzhVaeAqJif/gXGjvMLl4RFN8JGvHZGzpjCb14UdKhVqfP0ZumLo4cLIWd3Ch49zRBQBgchbFqEJbTdPPLTJ4SMIEYm5RwX4PtQ2Ce94u8IGXkIhYf32H43l+955a35XGh37hcZMLZEzjk4FBMqSScupKqDej1c0m34MkeRGMlQ==</ds:SignatureValue>
   25     <ds:KeyInfo>
   26        <ds:X509Data>
   27           <ds:X509Certificate>MIIC6jCCAdKgAwIBAgIQE+gZKcmH841I4gYjUiHCSDANBgkqhkiG9w0BAQsFADAxMS8wLQYDVQQDEyZBREZTIFNpZ25pbmcgLSBhZGZzLmNvbnRvc293aWRnZXRzLmNvbTAeFw0xNjA2MTYwMDUyNTZaFw0xNzA2MTYwMDUyNTZaMDExLzAtBgNVBAMTJkFERlMgU2lnbmluZyAtIGFkZnMuY29udG9zb3dpZGdldHMuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAna30lllMTaivaXPCjrW7VcRI6BsPs0iVxV559I9UONENSldX9pYTPlqLzxTP1RAVzfbGNoSvNelXrc0cb6jslgi+0Ya0jxrj1CsxQDgLtZeZchwWUYnJgsvk/HHfXiQBrWPLaZbImPNVvzG1zlYoQyQHTe1Nvr1m5Lv9foruSnw4My2LP4M27ZLPGL7rLaqpBg0E9sMX0iIrucNNN6AdyyPsR8oAUtV//QB49pCk+/rb3UtSDyGrdFFD+sJBDiAXYjTGzzYxYnMjckBZQfPKMWRntGwe7lM1KkX7mtUr9pNSvX1mQS/PHxhIcvO7aWKc15FJKzFmtdAEM2mvZjnCtwIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBj5cSPBQGyICJZHsMXA2KddWxSUqtYSBDPWYVsW9gJSMYiJBjdEnR1aGpw5K6iYei7KCACH717VQNfEF64qnBCbOvWc7FmZ3V0n4plfyZYuexbbZqp7RTi+J1q2xPsdb8MB7138YhXCc3Uf1p0oEuw+hKZ5rt4srcfgxuEauKXhnaI/UAOWOgDslzTuku+ogPsHBkc7wfH2CS9UqA3JUVJksR42yMg/Y47DUTp0Ma02RoVIfjFh+y3lX01O7B3ccCCdiKaSxcnLQ8n/ypn7LBhUUWDWZVIBj1flioohFMc5gU2Jl2Ueki72yxOwKVehqYgBHLPZBCUQUJDnUsbJJgb</ds:X509Certificate>
   28        </ds:X509Data>
   29     </ds:KeyInfo>
   30    </ds:Signature>
   31    <saml2p:Status>
   32     <saml2p:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
   33    </saml2p:Status>
   34    <saml2:Assertion ID="_406fb7fe-a519-4919-a42c-f67794a670a5"
   35                   IssueInstant="2017-11-15T16:19:06.310Z"
   36                   Version="2.0">
   37     <saml2:Issuer>http://my.rackspace.com</saml2:Issuer>
   38     <ds:Signature>
   39        <ds:SignedInfo>
   40           <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
   41           <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
   42           <ds:Reference URI="#_a8a6920c-d4eb-467f-85df-6fa2767ae63d">
   43              <ds:Transforms>
   44                 <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
   45                 <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
   46              </ds:Transforms>
   47              <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
   48              <ds:DigestValue>uQmSKQT03SRunafqzpb6v159a+jMVvTqmBCFYn17e7s=</ds:DigestValue>
   49           </ds:Reference>
   50        </ds:SignedInfo>
   51        <ds:SignatureValue>cYKqfE92hWqaPylJIcl89U9TKNzJXcFIPO0fvohg70zLB4JWnYlIKOz7S9XFUvS24mN47XS1T8DeR0IGITBMhqA/GCM624SOW0QjIRhQ9gh6/ONlyuAxGbVDo5tYb82sICFa9sMWI2Vr5ZH2LeTqyvsBRnlWBkZIw4hS2PBDHbhcnILUGX9uUDRcOrONAEMimnB7cNmZxSwQgdPfupyS39oedrUAiORa7GMII8GglWoj6Jy8SX0fQKXfsXD+wC5XFw76WAKJjSCuEkrXfxMQia/2H1tE24zNgZd6Y+uQ2Nh8YlUvO+DaMoj7mTKZUBqlxQt6It4kGH0+hfqvWx1MHQ==</ds:SignatureValue>
   52        <ds:KeyInfo>
   53           <ds:X509Data>
   54              <ds:X509Certificate>MIIC6jCCAdKgAwIBAgIQE+gZKcmH841I4gYjUiHCSDANBgkqhkiG9w0BAQsFADAxMS8wLQYDVQQDEyZBREZTIFNpZ25pbmcgLSBhZGZzLmNvbnRvc293aWRnZXRzLmNvbTAeFw0xNjA2MTYwMDUyNTZaFw0xNzA2MTYwMDUyNTZaMDExLzAtBgNVBAMTJkFERlMgU2lnbmluZyAtIGFkZnMuY29udG9zb3dpZGdldHMuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAna30lllMTaivaXPCjrW7VcRI6BsPs0iVxV559I9UONENSldX9pYTPlqLzxTP1RAVzfbGNoSvNelXrc0cb6jslgi+0Ya0jxrj1CsxQDgLtZeZchwWUYnJgsvk/HHfXiQBrWPLaZbImPNVvzG1zlYoQyQHTe1Nvr1m5Lv9foruSnw4My2LP4M27ZLPGL7rLaqpBg0E9sMX0iIrucNNN6AdyyPsR8oAUtV//QB49pCk+/rb3UtSDyGrdFFD+sJBDiAXYjTGzzYxYnMjckBZQfPKMWRntGwe7lM1KkX7mtUr9pNSvX1mQS/PHxhIcvO7aWKc15FJKzFmtdAEM2mvZjnCtwIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBj5cSPBQGyICJZHsMXA2KddWxSUqtYSBDPWYVsW9gJSMYiJBjdEnR1aGpw5K6iYei7KCACH717VQNfEF64qnBCbOvWc7FmZ3V0n4plfyZYuexbbZqp7RTi+J1q2xPsdb8MB7138YhXCc3Uf1p0oEuw+hKZ5rt4srcfgxuEauKXhnaI/UAOWOgDslzTuku+ogPsHBkc7wfH2CS9UqA3JUVJksR42yMg/Y47DUTp0Ma02RoVIfjFh+y3lX01O7B3ccCCdiKaSxcnLQ8n/ypn7LBhUUWDWZVIBj1flioohFMc5gU2Jl2Ueki72yxOwKVehqYgBHLPZBCUQUJDnUsbJJgb</ds:X509Certificate>
   55           </ds:X509Data>
   56        </ds:KeyInfo>
   57     </ds:Signature>
   58     <saml2:Subject>
   59        <saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">john.doe</saml2:NameID>
   60        <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
   61           <saml2:SubjectConfirmationData NotOnOrAfter="2017-11-17T16:19:06.298Z"/>
   62        </saml2:SubjectConfirmation>
   63     </saml2:Subject>
   64     <saml2:AuthnStatement AuthnInstant="2017-11-15T16:19:04.055Z">
   65        <saml2:AuthnContext>
   66           <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
   67           </saml2:AuthnContextClassRef>
   68        </saml2:AuthnContext>
   69     </saml2:AuthnStatement>
   70     <saml2:AttributeStatement>
   71        <saml2:Attribute Name="roles">
   72           <saml2:AttributeValue xsi:type="xs:string">nova:admin</saml2:AttributeValue>
   73        </saml2:Attribute>
   74        <saml2:Attribute Name="domain">
   75           <saml2:AttributeValue xsi:type="xs:string">323676</saml2:AttributeValue>
   76        </saml2:Attribute>
   77        <saml2:Attribute Name="email">
   78           <saml2:AttributeValue xsi:type="xs:string">john.doe@rackspace.com</saml2:AttributeValue>
   79        </saml2:Attribute>
   80        <saml2:Attribute Name="groups">
   81           <saml2:AttributeValue xsi:type="xs:string">group1</saml2:AttributeValue>
   82           <saml2:AttributeValue xsi:type="xs:string">group2</saml2:AttributeValue>
   83           <saml2:AttributeValue xsi:type="xs:string">group3</saml2:AttributeValue>
   84        </saml2:Attribute>
   85        <saml2:Attribute Name="FirstName">
   86           <saml2:AttributeValue xsi:type="xs:string">John</saml2:AttributeValue>
   87        </saml2:Attribute>
   88        <saml2:Attribute Name="LastName">
   89           <saml2:AttributeValue xsi:type="xs:string">Doe</saml2:AttributeValue>
   90        </saml2:Attribute>
   91      </saml2:AttributeStatement>
   92    </saml2:Assertion>
   93  </saml2p:Response>

The assertion describes a view of the user that has successfully
logged in. It contains within it all of the information deemed by the
|idp| to be relevant to the service provider, which is Rackspace.

.. note:: For help configuring third-party identity providers (such as Active
   Directory&reg; Federation Services, Okta&reg;, and others), refer to
   :ref:`Configure Third-Party SAML providers<index-configuring-3p-saml-ug>`.

Parts of the SAML assertion
---------------------------

This section describes the relevant parts of the previous SAML assertion.
According to the SAML protocol, a ``<saml2p:Response />`` element that
begins on line 2 wraps the assertion itself. The actual assertion
(``<saml2p:Assertion />``) begins on line 34.

- **Issuer (37)**: The issuer is the system that generated (or issued) the
  assertion. This is identified as a URI.

- **Signature (38 - 57)**: The XML signature of the assertion part of the
  request. The signature verifies that that the issuer indeed produces
  the assertion.

- **Subject (58 - 63)**: The subject identifies the identity (or user)
  that the assertion is about.

- **AuthnStatement (64 - 69)**: The ``AuthnStatement`` contains details
  about the authentication of the subject.

- **AttributeStatement (70 - 91)**: This section contains a list of arbitrary
  attributes associated with the subject. Each attribute in the list is a
  name/value pair. The values are of a type identified by the ``xsi:type``
  XML attribute. In this case, they are all strings. Attributes can have
  multiple values. The groups attribute defined in lines 80 - 84, for example,
  contain 3 separate values (group1, group2, and group3).

Signing SAML assertions
-----------------------

Both the SAML response on line 2 and the SAML assertion on line 34 might be
signed. Rackspace Identity might verify both signatures. However, you should
note that while signing the SAML response is optional, signing the
SAML assertion is strictly required. This means that a message that
contains a single signature at the SAML response level is rejected.

A SAML response can contain multiple assertions. In
this case, all assertions must be signed, and the same issue must issue
them all. Rackspace Identity typically examines only the
first assertion for authorization data, but a mapping policy can easily
overwrite this behavior.

Required attributes
===================

The following are descriptions of the attributes that Rackspace Identity requires
to successfully authenticate a user, including line references to the
preceding SAML response example.

Domain
------

Rackspace Identity keeps information about users, roles, and other
entitlements in a domain. When a user federates into Rackspace,
Rackspace Identity places the user in a single identity domain. Each domain
is accessed by using a unique alphanumeric ID, which Rackspace usually
sets to be the same as the user's account ID. The domain ID is required
when a federated user
requests access. This is especially important because a customer is
allowed to create multiple domains, and Rackspace Identity needs to
place the federated user in the correct one.

In the preceding SAML assertion, the domain is passed as a SAML attribute
in lines 74 - 76. This implies that the identity provider is
preconfigured to emit the correct value. It is not strictly required
that the |idp| do this, because most federated users target a single domain,
and you can easily hard code the domain value in an attribute mapping policy.

Name
----

This value is the username of the federated user.  Rackspace Identity
assumes that each user has a unique username, and
that the same user has the same username from one federated
login to the next.

In the preceding SAML assertion, the username is identified by the
``NameID`` in the ``Subject`` section of the assertion (line 59). That
said, an IDP can return a stable username as an attribute in the
``AttributeStatement`` section.

Email
-----

This value is the email of the federated user. In the SAML assertion, it is
an attribute in the``AttributeStatement`` section (lines
77 - 79). Some identity providers make no distinction between a username and
an email, in which case the email is in the ``Subject`` section.

Roles
-----

Rackspace Identity expects an attribute with the list of roles to
assign to the federated user. Rackspace Identity
only allows the assignment of roles that it recognizes.

In the SAML assertion above, the list of roles is in an
attribute named ``roles`` on lines 71 - 73.  Note that this is a
good use case for a multivalue attribute, but in this case,
you assign only the ``nova:admin`` role.

The example assigns ``nova:admin`` at the domain level, which
means that the user has the role on all accounts (sometimes
referred to as tenants) that are associated with the domain.  For
example, if you associate accounts ``12873`` and ``33987`` with the
domain, the user has the ``nova:admin`` role on both of those
accounts.

You can restrict the role to a specific account. For
example, to grant ``nova:admin`` on account ``12873`` and
``nova:observer`` on account ``33987``, you should specify the roles
as ``nova:admin/33987`` and ``nova:observer/33987``.

.. _Expire:

Expire
------

Finally, Rackspace Identity needs to understand the amount of time
allowed to a federated user on Rackspace systems before forcing
the user to reauthenticate.  You can use two different formats 
to provide this attribute.

-  You can use an `ISO 8601`_ timestamp, which should include a time zone designator.
   For example, the timestamp ``2017-10-04T16:20:57Z`` signifies that the
   user is forced to reauthenticate after October 4th 2017 at
   16:20:57 UTC.  
   
-  You can use an `ISO 8601`_ duration.
   For example, ``PT1H2M`` signifies that the user is forced to
   reauthenticate one hour and two minutes after successfully logging
   in.

In the preceding SAML assertion example, an expire timestamp is specified
in the ``NotOnOrAfter`` attribute of the ``SubjectConfirmationData`` on line 61.
In SAML, this attribute denotes the time after which the
SAML assertion is no longer considered valid. While this
timestamp does not fit semantically with the ``expire`` attribute that
Rackspace Identity expects, it still works as a reasonable default.

Other attributes
----------------

Rackspace Identity expects the attributes described in the previous
sections (``domain``, ``name``,``email``, ``roles``, and ``expire``)
in every federated login. Some Rackspace products might expect additional
optional attributes. Consult this document for details on these attributes.

Mapping attributes
==================

You can find the SAML assertion attributes on the following lines of the
preceding example:

.. include:: tables/loc.rst

The following table gives the values at those locations:

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
data located in the SAML assertion into attributes that
Rackspace Identity requires to log in a federated user. However,
this is a silly mapping because mapping attributes by referring to
line numbers is extremely unpractical, inexact, and brittle. Using
XPath, on the other hand, is a more stable and practical way of
pinpointing the exact location of the data that you need. After all,
XPath is designed specifically to pinpoint and extract data from XML
documents [#j1]_.

Mapping attributes with XPath
-----------------------------

The following table replaces line numbers with XPaths into the SAML
assertion:

.. include:: tables/xpath.rst


You can turn this table into an attribute mapping policy, as shown in the
following example:

.. code-block:: yaml

    1 mapping:
    2   version: RAX-1
    3   description: |-
    4     Simple policy where we select required attributes via an XPath.
    5   rules:
    6   - local:
    7       user:
    8         domain: "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue[1])}"
    9         name:   "{Pts(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
   10         email:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue[1])}"
   11         roles:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
   12         expire: "{Pts(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

The following section walks through the preceding policy in detail and
examines how the code uses XPath to extract the attribute values.

Parts of the mapping policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The mapping policy is a YAML document that contains instructions to
retrieve (or derive) identity attributes from a SAML assertion.
You can think of it as a simple script that executes every
time a SAML assertion is presented to Rackspace Identity. This
section breaks the preceding mapping policy into its relevant parts.

- **mapping (1)**:  A single top-level ``mapping`` object always contains
  the mapping policy.

- **version (2)**: The ``version`` key identifies the version of the mapping
  policy language.  This required attribute should always have the
  value of ``RAX-1``. The mapping policy language described here is
  based on the `Mapping Combinations`_ language in OpenStack
  Keystone. It uses the version name to differentiate a Rackspace
  Identity mapping policy from an OpenStack Keystone mapping policy.

- **description (3)**: The ``description`` key provides a human-readable
  description of the mapping policy. This description is optional.

- **rules (5)**: A collection of rules makes up a mapping policy. The
  ``rules`` array encapsulates these rules. A policy must contain at
  least one rule.

- **rule (6 - 12)**: These lines contain a rule that drives the policy. A
  rule may contain  a ``local`` and a ``remote`` section.  Both ``local``
  and ``remote`` sections are optional (in this case, you don't need a
  ``remote``), but you must have at least one rule with a ``local`` section.

  It's important to note that things are local or remote from the
  perspective of Rackspace Identity. For example, the ``local``
  section contains statements about the user within Rackspace Identity
  (the local user). The remote section contains statements about the
  user as it's presented by the |idp| (the remote user).

  Lines 7 - 12 describe what the local user should look like. In other
  words, they describe the attributes of the local user. Here, you
  specify each of the required identity attributes and describe how
  you can obtain them from an XPath.

Using XPath in the mapping policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Something that looks like the following structure contains the XPaths
in the mapping policy:

 ``{Pts()}``
 
This is an
XPath substitution. Various types of substitutions are in the
mapping policy language, each of which is encapsulated by curly braces
``{}``. Substitutions are only allowed in the local part of the
mapping policy. They help set values for local attributes because they
are always replaced (or substituted) by other content. For
XPath substitution, the result of the XPath
within the parenthesis, when executed against the SAML assertion, replaces
the value.

.. note::
   Spaces matter in a substitution.  For correct interpretation, use
   no spaces between the prologue of the substitution denoted as ``{Pts(``
   and the epilogue of the substitution denoted as ``)}``.

.. include:: tables/pts.rst


Note that the XPath substitution refers to elements in
certain namespaces (``saml2p``, ``saml2``). These namespace prefixes,
which are common to SAML, are always predefined. The following table
describes how namespace prefixes are mapped by default:

.. include:: tables/namespaces.rst


A SAML assertion can refer to elements in extended
namespaces that are not listed. You can define additional prefixes by
adding a ``namespaces`` key to the mapping policy. In XPath, it's the
namespace URI that uniquely identifies an element. The prefix is
simply short hand for this URI.  The following example replaces the
``saml2p`` prefix with ``foo``. Because the namespace URI bound to
element is the same as in the previous example, the two mapping policies
produce the exact same result.

.. code-block:: yaml

    1 mapping:
    2   version: RAX-1
    3   description: |-
    4     Simple policy where we select required attributes via an XPath.
    5   namespaces:
    6     foo: urn:oasis:names:tc:SAML:2.0:protocol
    7   rules:
    8   - local:
    9       user:
   10         domain: "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue[1])}"
   11         name:   "{Pts(/foo:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
   12         email:  "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue[1])}"
   13         roles:  "{Pts(/foo:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
   14         expire: "{Pts(/foo:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Using the {Pt()} substitution
-----------------------------

Notice that the XPath for retrieving the domain ends with
``saml2:AttributeValue[1]``, while the XPath for retrieving the list of
roles ends with ``saml2:AttributeValue``.  This structure is because
you want a single
value for a domain, but you want a list of roles.  Without the ``[1]`` at the
end, the XPath returns every ``AttributeValue`` in a SAML assertion
for a domain. The ``[1]`` signifies that you are only interested
in the first value [#j2]_.

Because it's common to want to retrieve a single value, there's an
alternative XPath substitution (``{Pt()}``) that always returns the
first value of an XPath result. This alternative is useful in cases where you
expect a single value, and you want to automatically protect against the
off chance that the operation might return multiple values in a SAML assertion.

Given this new substitution, you can rewrite the mapping policy as
like the following example:

.. code-block:: yaml

    1 mapping:
    2   version: RAX-1
    3   description: |-
    4     Simple policy where we select required attributes via an XPath.
    5     We use {Pt()} instead of {Pts()} in single value attributes to
    6     avoid having to select the first attribute value in XPath.
    7   rules:
    8   - local:
    9       user:
   10         domain: "{Pt(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='domain']/saml2:AttributeValue)}"
   11         name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
   12         email:  "{Pt(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='email']/saml2:AttributeValue)}"
   13         roles:  "{Pts(/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name='roles']/saml2:AttributeValue)}"
   14         expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"


Using the mapping:get-attributes call
-------------------------------------

At this point, the XPaths for retrieving a domain, email, and roles
all look very similar. They all retrieve the list of values for an attribute
in the ``AttributeStatement`` of the assertion by name. Because this is a
common case, a predefined XPath function exists that is called
``mapping:get-attributes`` that takes the name of a SAML attribute as
a string and returns the attribute values associated with that name.

Thus, you could rewrite the mapping policy using this function as follows:

.. code-block:: yaml

    1 mapping:
    2   version: RAX-1
    3   description: |-
    4      Simple policy where we select required attributes via an
    5      XPath. Here we use the mapping:get-attributes call to return
    6      attribute values.
    7   rules:
    8   - local:
    9      user:
   10         domain: "{Pt(mapping:get-attributes('domain'))}"
   11         name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
   12         email:  "{Pt(mapping:get-attributes('email'))}"
   13         roles:  "{Pts(mapping:get-attributes('roles'))}"
   14         expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Using the {Ats()} and {At()} substitutions
------------------------------------------

Having an XPath function that retrieves SAML attribute values is handy
when you want to process those values in some way before returning a
result.  In some cases, however, you simply want to return the value
(or values) of a SAML attribute as is. The ``{Ats()}`` and ``{At()}``
substitutions, do just that. These are known as attribute
substitutions. Like their XPath counterparts, the ``{Ats()}``
substitution returns all values for a specific attribute name, and the
``{At()}`` returns just the first value.

Given these substitutions, you can rewrite the policy as follows:

.. code-block:: yaml

    1 mapping:
    2   version: RAX-1
    3   description: |-
    4     Simple policy where we select required attributes. We use At
    5     instead of Pts as a simple means of accessing an name SAML
    6     attribute.
    7   rules:
    8   - local:
    9       user:
   10         domain: "{At(domain)}"
   11         name:   "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID)}"
   12         email:  "{At(email)}"
   13         roles:  "{Ats(roles)}"
   14         expire: "{Pt(/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:SubjectConfirmation/saml2:SubjectConfirmationData/@NotOnOrAfter)}"

Notice that in this case the name of the attribute is not in single
quotes. You are no longer directly using XPath with attribute
substitutions, but under the hood, these substitutions are also
interpreted as XPath statements.

Using the {D} substitution
--------------------------

There are certain default places where a mapping policy looks for a
particular identity attribute value. The value at the default
location always replaces the default substitution (``{D}``). Unlike
other substitutions, the default substitution is
interpreted differently depending on where the substitution is
placed. For example, if the substitution is placed as the value of the
``name`` attribute, it replaces itself with the default value for the
name. Likewise, if ``{D}`` is placed in the ``email`` attribute, it
is replaced with the default value for email and so on.

What are the default locations in a SAML assertion for the five
attributes Rackspace Identity expects? It turns out that the SAML
assertion in this example has all of the values in the default places.
So it's possible, in this case, to write a mapping policy that looks
like the following one:

.. code-block:: yaml

    1  mapping:
    2   version: RAX-1
    3   description: |-
    4     The default policy.  All attributes are in the expected location
    5     in the SAML assertion.
    6   rules:
    7   - local:
    8       user:
    9         domain: "{D}"
   10         name:   "{D}"
   11         email:  "{D}"
   12         roles:  "{D}"
   13         expire: "{D}"

Next steps
==========

This section explored the SAML assertions, how to use
XPath and attribute names, and the expected defaults to write mapping
policies. All of the policy examples, however, described
simple direct mappings. The following **Examples** section looks at
writing policies in cases where things don't align so perfectly.

.. References:

.. _rackspace identity federation user guide: https://developer.rackspace.com/docs/rackspace-federation

.. _list of allowed roles: https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/full-roles/

.. _iso 8601: https://en.wikipedia.org/wiki/ISO_8601

.. _mapping combinations: https://docs.openstack.org/keystone/latest/advanced-topics/federation/mapping_combinations.html

.. Footnotes:

.. [#j1] Later versions of XPath allow extracting data from JSON
   documents as well.

.. [#j2] The first index in XPath is 1, not 0.  This is logical and makes
   sense unless you're a software developer.
