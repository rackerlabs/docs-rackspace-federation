============
Introduction
============

Attribute mapping policies enable you to integrate with |product name|
without having to make significant configuration changes to your |idp|. An
|amp| provides a declarative means of extracting and transforming
information produced by your identity system so that it may seamlessly
inter-operate with Rackspace.

Technology background
=====================

In order to write attribute mapping policies, you should have
a basic understanding of the following technologies:

- **SAML 2.0**: SAML is an OASIS Standard for defining XML-encoded assertions
  about authentication, authorization, and related attributes. This appendix
  concentrates solely on SAML Responses and Assertions, but you should have a
  basic understanding of the SAML protocol.

- **XPath 2.0**: XPath is a W3C standard expression language for extracting
  information from structured data and is designed to be embedded in a host
  language.

- **YAML 1.1**: YAML is a simple data serialization language that is designed
  to be human friendly. YAML is very similar to JSON but allows for useful
  features such as comments and the ability to easily input multi-line data.
  Attribute mapping policies are written in YAML.

- **XML 1.0**: Extensible Markup Language (XML) is a universal format,
  maintained by the W3C, used for representation and transfer of structured
  data on the web or between different applications. XML is a markup language
  that defines set of rules for encoding documents in a format that is both
  human-readable and machine-readable.

What is Attribute Mapping?
==========================

Your |idp| contains information about every user in your
organization.  For example, it maintains that Jane Doe is a
development manager whose username is ``janed``. Jane's email address
is janed@widgets.com, and as a development manager, she is a member of
the ``engineering``, ``managers``, and ``linux_user``
groups. Additionally, Jane is managing the ``widgets_ui`` project.

When Jane logs into Rackspace, your organization's identity provider
submits to Rackspace a cryptographically signed SAML response that
contains (among other things) the *attributes* previously described.
Rackspace (the service provider) needs these attributes in
order to grant Jane access to the corresponding Rackspace services.

Attribute mapping allows the extraction and transformation of the
attributes in the SAML response so that they can be processed by
Rackspace. There are three major reasons why attribute mapping is
required:

Attribute name alignment
------------------------

Rackspace expects that Jane's email is supplied in an attribute named
``email``, but your organization's identity provider might by default
submit a user's email in an attribute named ``mail``. This is not an
atypical situation because several competing standards exist for
attribute names. In this case, the attribute ``mail`` must be mapped to
the attribute ``email``.

The mapping is simple and is summarized as follows:

``mail`` → ``email``

Role and group alignment
------------------------

The groups ``engineering``, ``managers``, and ``linux_user`` are
entirely meaningless to Rackspace.  Rackspace organizes its roles
according to the services it provides. For example, the ``nova:admin``
role grants administration access to our OpenStack compute service and
the ``ticketing:observer`` role grants view-only access to Rackspace
tickets.

In this case, we want the ``managers`` group to map to the
``ticketing:admin`` role because any manager should be able to create
and edit tickets. We also want to map managers to the
``billing:observer`` role because all managers can see invoices.
Additionally, we want to map the ``linux_user`` group to
``nova:observer`` because all linux users should be able to query the
Compute API.

The mapping described above are summarized as follows:

``managers``    → ``ticketing:admin``,  ``billing:observer``

``linux_user``  → ``nova:observer``

Implementation of access policies
---------------------------------

The mapping described previously is fairly simple.  All users in the
``managers`` group will obtain the ``ticketing:admin`` and ``billing:observer``
roles. Unfortunately, things are often more complicated than this. For
example, some managers are actually contractors, and contractors
should not be allowed to view invoices. Therefore, these contractors should not
receive the ``billing:observer`` role. You can tell that a user is a
contractor by looking at membership in the ``contractors`` group.

Additionally, there are separate Rackspace accounts for each project
managed by a manager. A manager involved with the ``widgets_ui``
project should have full administrator rights (via the ``admin`` role)
to account ``777654``, which is the account associated with that
project.  The |idp| passes an attribute named ``manager_projects`` that
contains the list of all of the projects managed by a user.

There are two other projects to consider: ``widgets_moble`` is
associated with account ``887655``, and ``widgets_platform`` which
should have admin access to account ``779966``.

**Note**: By performing this mapping, you are implementing an access
policy that is executed whenever a user logs in. As long as relevant
information is provided by the identity provider in a SAML response,
you can implement similar policies for your organization.

The preceding mapping rules are summarized as follows:

``managers`` → ``ticketing:admin``,  ``billing:observer`` unless the
user is also a member of ``contractors``.

``manager_projects`` contains ``widgets_ui``    → ``admin`` on
``777654``

``manager_projects`` contains ``widgets_moble`` → ``admin`` on
``887655``

``manager_projects`` contains ``widgets_platform`` → ``admin`` on
``779956``

Mapping Policy for Widget.com
=============================

The following attribute mapping policy implements the rules described
in the previous section. The rest of this document provides a guide
for writing such polices.

XML Example:

.. code-block:: xml

     1 <mapping xmlns="http://docs.rackspace.com/identity/api/ext/MappingRules" version="RAX-1" xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
     2 <description>The following is an attribute mapping for Widgets.com.</description>
     3 <rules>
     4 <rule>
     5 <local>
     6 <user>
     7 <domain value="{D}"/>
     8 <name value="{D}"/>
     9 <email value="{At(email)}"/>
     10 <roles value="{0}"/>
     11 <expire value="{D}"/>
     12 </user>
     13 </local>
     14 <remote>
     15 <attribute path="for $group in mapping:get-attributes('groups') return if ($group = 'managers') then ('ticketing:admin',if (not(mapping:get-attributes('groups')='contractors')) then
     'billing:observer' else(),for $project in mapping:get-attributes('manager_projects') return(if ($project = 'widgets_ui') then 'admin/777654' else if ($project = 'widgets_mobile')   then 'admin/887655'
     else if ($project = 'widgets_platform') then 'admin/779956' else())) else if ($group = 'linux_user') then 'nova:observer' else()"
     multiValue="true"/>
     16 </remote>
     17 </rule>
     18 </rules>
     19 </mapping>


.. code-block:: yaml

    1  mapping:
    2    version: RAX-1
    3    description: |-
    4      The following is an attribute mapping for Widgets.com.
    5    rules:
    6    - local:
    7     user:
    8       domain: "{D}"
    9       name: "{D}"
   10       email: "{At(mail)}"
   11       roles: "{0}"
   12       expire: "{D}"
   13    remote:
   14       - multiValue: true
   15         path: |-
   16            (:
   17               The following describes the rules for assigning roles to
   18               users.
   19            :)
   20             for $group in mapping:get-attributes('groups') return
   21                 (:
   22                   If a user is a manager they get ticketing:admin,
   23                   If they are not a contractor then they also get billing:observer
   24                   Managers become admin based on the project that they are working
   25                   on
   26                 :)
   27               if ($group = 'managers') then
   28                    (
   29                     'ticketing:admin',
   30                     if (not(mapping:get-attributes('groups')='contractors')) then 'billing:observer' else
   31                     (),
   32                     for $project in mapping:get-attributes('manager_projects') return
   33                     (
   34                        if ($project = 'widgets_ui')       then 'admin/777654' else
   35                        if ($project = 'widgets_mobile')   then 'admin/887655' else
   36                        if ($project = 'widgets_platform') then 'admin/779956' else
   37                        ()
   38                     )
   39                    ) else
   40               (:
   41                  If a user is a member of the linux_user group they get the
   42                  nova:observer role.
   43               :)
   44               if ($group = 'linux_user') then 'nova:observer' else
   45               ()
