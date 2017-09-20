# Contributor guidelines

These guidelines provide the general process for maintaining source code that
builds the Rackspace ``Federation`` developer documentation.

- [Project description](#project-description)
- [Updating and adding content](#updating-and-adding-content)
- [Using writing guidelines](#using-writing-guidelines)
- [Submitting your content](#submitting-changes)
- [Previewing changes](#previewing-changes)

## Project description

This project is developed and built by using the
[Python Sphinx documentation generator](http://sphinx-doc.org/). Content is
written in [reStructuredText](http://sphinx-doc.org/rest.html), which is the
markup syntax and parser component of
[Python Docutils](http://docutils.sourceforge.net/index.html).

Source files for the Sphinx documentation project are in the ``doc``
directory. Following are the key files that define project and content
architecture:


Content | File
--- | ---
|Index page for the main content structure| [index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/index.rst)
|Common front information|[common/common-front.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/common/common-front.rst)
|Overview index|[overview/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/overview/index.rst)
|Installing index|[installing/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/docs/installing/index.rst)
|Getting started index|[getting-started/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/getting-started/index.rst)
|Using index|[using/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/tree/master/doc/using/index.rst)
|Administering index|[administering/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/administering/index.rst)
|Support index|[support/index.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/support/index.rst)
|API|[api.rst](/https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/api.rst)
|Common back information|[common/common-back.rst](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/common/common-back.rst)
|Sphinx documentation configuration file| [conf.py](https://github.com/rackerlabs/docs-rackspace-federation/blob/master/doc/conf.py)
|Linux and OS X build script|Makefile|
|Windows build script|make.bat|
|Install requirements for building locally|[requirements.txt](https://github.com/rackerlabs/docs-common/blob/master/requirements.txt)

## Updating and adding content

Contributions are submitted, reviewed, and accepted by using GitHub pull
requests (PRs), following the [GitHub workflow](GITHUBING.md) for this repository.

To update existing source files or add new ones, follow the
[GitHub workflow](GITHUBING.md) for this repository.

* Update source files by using the GitHub editor or any plain text editor.
* Format source files with
  [reStructuredText syntax](http://www.sphinx-doc.org/en/stable/rest.html).
* For quick syntax checking, try the
[Online reStructuredText editor](http://rst.ninjs.org/).

## Using writing guidelines

When you add or update content, use the writing and style guidelines
in the [Style guidelines for technical content](http://rackerlabs.github.io/docs-rackspace/style-guide/index.html).
Start with the guidelines in the [Quickstart](http://rackerlabs.github.io/docs-rackspace/style-guide/quickstart.html#quickstart)
section.

## Submitting changes

When you've completed your changes, submit a PR. Someone on the
Information Development team will review your PR.
- Minor updates and corrections get a quick review to ensure that content is
  error-free and doesn't introduce other issues.
- More complex changes or additions require both technical and editorial review.

Depending on the review feedback, you might be asked to make additional changes.

After content has been reviewed and approved, the updates can be merged to the
master branch. The merge triggers the build and deploy process. Typically, new
content is available on **developer.rackspace.com** within a minute or two after it
is merged. Larger updates might take a bit longer.

## Previewing changes

When you submit a PR, the build process creates a preview of
your changes in a staging environment. After the build process is complete, a
message is displayed in the PR comments with a link to the content in the
staging environment.

You can also build the project locally by using the [Sphinx documentation
generator](http://sphinx-doc.org/). For details, see
[Building from source](https://github.com/rackerlabs/docs-rackspace/blob/master/doc/tools/build-from-source.rst).
