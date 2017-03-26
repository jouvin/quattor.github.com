---
layout: article
title: Pan Configuration Language
redirect_from: /documentation/2012/06/19/documentation-pan-book.html
menu: Pan Language
---

The pan configuration language allows system administrators to
describe (complete or partial) machine configurations.  These
descriptions are then compiled into a machine-friendly format (usually
XML or JSON), which is then used by downstream Quattor tools to affect
the necessary changes to the machine configuration.

The pan language is a "Domain Specific Language" (DSL) with a simple,
human-friendly syntax.  Unlike other languages, pan allows the
simultaneous definitions of the configuration schema, the
configuration itself, and contraints for validation.

Pan Language Reference
----------------------

The core syntax and functionality of pan have been stable for a long
time.  However, each new release of the pan compiler brings fixes and
improvements, some of which affect the pan language itself.

Each release of the pan compiler is accompanied by an updated "Pan
Book".  It contains an introduction to the language, detailed
reference, and instructions for installation and use.

Look at the [release page][panc] in GitHub to find the
version of the book you're looking for. It is availabe in both PDF and
EPUB formats.

Feedback
--------

Comments, bug reports, and feature requests are welcome.  Please
register in GitHub to [raise issue][panc-issues]. You are also encourage to subscribe
to the Quattor discussion list to get support from the community.

[panc]: https://github.com/quattor/pan/releases
[panc-issues]: https://github.com/quattor/pan/issues

