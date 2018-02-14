Sympa Documentation
===================

This site holds the draft of new documentation site for Sympa.

To browse entire content, start from [index](index.md) file.

See [LICENSE](LICENSE.md) file about license.

See [AUTHORS](AUTHORS.md) file about authors.

See [CONTRIBUTING](CONTRIBUTING.md) file to know how you can contribute to
documentation.

Sympa Development
===================

Here is the state of decision and consensus that was reached regarding development of Sympa 7.0

## Technology
We agreed on switching to the following main technologies:
  - [Moo](https://metacpan.org/pod/Moo) for object orientation management
  - [DBIx::Class](https://metacpan.org/pod/DBIx::Class) for database access and manipulation
  - [Dancer 2](https://metacpan.org/pod/Dancer2) for web server

## Coding style
[eiro](https://github.com/eiro) wrote a reference module, called [Sympatic](https://github.com/sympa-community/p5-sympatic), whose purpose is to:
  * include all pre-requesite modules for a Sympa module. Therefore, the line `use Sympatic;` should be enough to include Moo and all required modules,
  * serve as an example and documentation about the coding style.
This is still a proposal and the exact details should be accepted by the community.
