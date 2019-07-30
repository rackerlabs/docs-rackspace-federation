============================================
Appendix: Attribute Mapping Policy Reference
============================================

.. This documentation uses an RST directive called attribmap.  This
   directive allows us to easily bring in core mapping test examples
   (which are under test) into the docs -- the examples show the
   result of executing attributeMapping on a SAML Response.  The
   attribute map directive allows one to quickly show the SAML
   Response, the policy, and the resulting attributes

   Simple example:

   .. attribmap:: defaults
      :saml: sample_assert.xml
      :map: defaults.yaml

   Here the samples are brought in from the test called defaults which
   you can find in core/src/test/resources/tests/mapping-tests.  Most
   tests refer to multiple SAML responses and mapping policies so
   you'll have to state which one you want to show.  Notice you don't
   specify anything about the result of the mapping, the directive
   will show the resulting attributes.

   You can specify a caption on the SAML response, the policy file,
   and the results. You can also emphasize lines in the examples:

   .. attribmap:: defaults
      :saml: sample_assert.xml
      :saml-caption: Default Assertion
      :saml-emphasize-lines: 17, 19, 29, 32, 35
      :map: defaults.yaml
      :map-caption: Default Mapping Policy
      :map-emphasize-lines: 3-7
      :results-caption: Resultning attributes

   Finally, you can suppress the output of the SAML response, the
   policy file or the results.  For example, if you only want to show
   just the mapping policy you could do this:

   .. attribmap:: defaults
      :saml: sample_assert.xml
      :saml-show: false
      :results-show: false
      :map: defaults.yaml

   A simpler way to achive this is to use the map directive:

   .. map:: defaults/defaults.yaml

   Notice you list the name of the test and the yaml result in a
   single argument seperated by a slash. You can add a caption and
   emphasize lines but you don't need to prepend the field with map-.

   .. map:: defaults/defaults.yaml
      :caption: Default Mapping Policy
      :emphasize-lines: 3-7

   The saml directive works the same way

   .. saml:: defaults/sample_assert.xml
      :caption: Default Assertion
      :emphasize-lines: 17, 19, 29, 32, 35

version: |version|

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   intro
   map
   examples

