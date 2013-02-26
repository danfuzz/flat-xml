Flat-XML
========

Because the world can never have too many incomplete implementations
of XML parsing.

Introduction
------------

The `flat-xml` utility script is an XML "flattener" which takes
XML input and produces a flattened form that is much easier for
other shell scripts to digest. The output is meant to be maximally
unambiguous, if a bit verbose.

The impetus for this utility was a desire to write shells scripts that
could gracefully handle the output from Amazon's Route53, while
requiring neither any compilation nor anything preinstalled beyond
baseline "standardish POSIX" tools (with the Bash shell considered
"standardish" enough).

This program was inspired by several others that fill similar niches
in the ecosystem, namely:

* xml2 &mdash; <http://www.ofb.net/~egnor/xml2/>
* xmlparse.awk &mdash; <http://awk.info/?doc/tools/xmlparse.html>
* Pyxie &mdash; <http://www.xml.com/pub/a/2000/03/15/feature/>

And many details for this program were derived from the official
XML specification at:

* <http://www.w3.org/TR/REC-xml/>


Installation
------------

Copy the file `bin/flat-xml` anywhere convenient.


Usage
-----

The program parses arguments in the usual way, interpreting all arguments
beginning with a `-` to be options, up until an argument that does not
start with a `-` or until an argument of exactly `--`. Subsequent arguments
are taken to be the names of files to process. A file named `-` is taken
to mean the standard input.

Each file to be processed is first read to completion (so, don't
expect it to operate on an unterminated input stream). After that,
it is parsed as XML, and each element of the file is emitted to
standard output as one or more lines.

The program does *not* parse all of XML. Limitations are noted below.

The program does *not* validate XML against a DTD, but it *does* make
a good faith effort to reject syntactically incorrect XML, with some
leniency.

Note that any *known* leniency is documented (under "Known
Limitations," below), and that for the most part such leniency exists
because the author is unaware of practical problems with it.

All that said, the author is happy to accept patches which improve
coverage of syntax and/or strictness of parsing, as long as such
patches sacrifice neither the performance (such as it is), nor
readability, nor maintainability of the code. See
[CONTRIBUTING.md](CONTRIBUTING.md) for more details.


Options
-------

### --help

Emits a short help message and exits.

### --version

Emits version and copyright information, and exits.


Output
------

Each output line is a two- or three- field line, where a single
space separates each field, and where the third field (if present)
consists of everything after the field-separating space until the
end of the line (including, e.g. and in particular other spaces).

The first field is a "path" to an element, starting with a slash `/`
and with a slash between each component. Each component of a path is
one of:

* a tag &mdash; represented with the same text as the tag in the original
  document.

* an attribute &mdash; represented with the same text as the attribute
  name in the original document, but prefixed with an at-sign `@`.

* an XML declaration &mdash; represented with the component `?xml`.

* a processing instruction &mdash; represented with the same text as
  the instruction name, but prefixed with a question mark `?`.

The second field is an "action," one of:

* `{` &mdash; indicates that the path is being "opened." For a tag,
  this indicates the start of a tag, *before* any attributes are
  parsed.

* `}` &mdash; indicates that the path is being "closed." This indicates
  that there is no more content under the given path.

* `+` &mdash; indicates newline-terminated content / data at the given
  path. This is either data directly under a tag or quoted content
  inside an attribute value.

* `-` &mdash; indicates the end of content / data at the given
  path, the value of which is *not* newline-terminated. Any time there
  is data at all, it will be terminated by a possibly-empty `-` line.

The third field is the data associated with a `+` or `-` action, if any.
The data is output with entities translated, so if the XML is `blort
&amp; fizmo` the output value will be `blort & fizmo`.

The output is produced in the same order that elements appear in the
original XML document.

See [the examples directory](examples/) or [the tests](tests/)
for more details.


Known Limitations
-----------------

* Doesn't limit the `<?xml ... ?>` declaration to have just the valid
  attribute names, values, or order.

* Doesn't reject processing instructions that look like `<?xml`
  declarations.

* Only accepts the ASCII-7 subset of valid attribute name characters.

* Only recognizes the five standard predefined entities: `&amp;`
  `&apos;` `&gt;` `&lt;` `&quot;`

* Doesn't handle CDATA at all.

* Doesn't handle directives `<! ... !>` at all.

* Gets the processing instruction `<? ... ?>` syntax woefully wrong.

* Doesn't attempt to honor a character encoding declaration, if present.

* Only handles UTF-8 (the assumed default character encoding) as well
  as the underlying system's awk implementation does. Recent versions
  of Gnu Awk (`gawk`) seem to do a reasonable job at this. Other versions
  do not.


Unknown Limitations
-------------------

* Probably a lot.


Contributing
------------

See [CONTRIBUTING.md](CONTRIBUTING.md).


Author and License
------------------

Copyright 2013 Dan Bornstein, <danfuzz@milk.com>.

Licensed under the Apache License, Version 2.0. See the top-level
file `LICENSE.txt` and (http://www.apache.org/licenses/LICENSE-2.0).

With thanks to my employers [NAME REDACTED] for their support.
