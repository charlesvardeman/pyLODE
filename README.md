![image](https://rawcdn.githack.com/RDFLib/pyLODE/b1ff1b1e19262cdc21ee28c7362b1690ca18e30b/img/pyLODE-250.png)

[![image](https://badge.fury.io/py/pyLODE.svg)](https://badge.fury.io/py/pyLODE)

# pyLODE

An OWL ontology documentation tool using Python, based on LODE.

In addition to making web page, human-readable forms of ontologies,
pyLODE encourages ontology annotation *best practice* by only producing
good results for well documented inputs! pyLODE defines what it
considers \'well documented\' in sections below, e.g. [What pyLODE
understands](#what-pylode-understands).

------------------------------------------------------------------------

**A note on the v 3.x change**

This is pyLODE version 3.0.1 and it\'s vastly different from pyLODE 2.x.
It doesn\'t yet handle all the various \"profiles\" that pyLODE 2.13.2
does, such as SKOS \'vocabularies\' & Profiles Vocabulary \'profiles\',
it only handles OWL \'ontologies\', nor all the special data types, such
as JSON literals, BUT, it generates HTML in a much more straightforward
manner and the code is both more efficient and much more maintainable,
which is why it\'s been made.

v 3.x will eventually catch up to all of v 2.13.2\'s features.

To access v 2.13.2 of pyLODE, either [download it from
PyPI](https://pypi.org/project/pyLODE/2.13.2/) , [check it out from
GitHub](https://github.com/RDFLib/pyLODE/releases/tag/2.13.2) or access
it via the [online service](http://pylode.surroundaustralia.com/) .

------------------------------------------------------------------------

## Contents

1.  [Quick Intro](#quick-intro)
2.  [Use](#use)
3.  [What pyLODE understands](#what-pylode-understands)
4.  [Examples](#examples)
5.  [Installation](#installation)
6.  [Differences from LODE](#differences-from-lode)
7.  [Releases](#releases)
8.  [License](#license)
9.  [Citation](#citation)
10. [Collaboration](#collaboration)
11. [Contacts](#contacts)

## Quick Intro

The Live OWL Documentation Environment tool
([LODE](https://github.com/essepuntato/LODE)) is a well-known (in
Semantic Web circles) Java & XSLT-based tool used to generate
human-readable HTML documents for OWL and RDF ontologies. That tool is
now a bit dated (old-style HTML, use of older technologies like XSLT)
and it\'s ([online version](https://www.essepuntato.it/lode)) is not
always online.

This tool is a complete re-implementation of LODE\'s functionality using
Python and Python\'s RDF manipulation module,
[rdflib](https://pypi.org/project/rdflib/). An ontology to be documented
is parsed and inspected using rdflib and HTML is generated directly,
using Python\'s [dominate](https://pypi.org/project/dominate/) package.

## Use

The tool can be used in multiple ways:

-   

    BASH command line script

    :   -   pyLODE.sh in bin/

-   

    Windows EXE

    :   -   pyLODE.exe in bin/

-   

    Mac executable

    :   -   pyLODE in bin/

-   

    Python Script

    :   -   cli.py or module

-   

    as-a-service locally

    :   -   via the popular [Falcon
            framework](https://falconframework.org/).
        -   see server.py in the main folder

-   

    as-a-service online

    :   -   hosted at <https://pylode.surroundaustralia.com>

### Command line arguments

The BASH, Windows EXE and Python Script methods all use the same command
line arguments:

    usage: cli.py [-h] [-v] [-o OUTPUTFILE] [-c {true,false}] input

    positional arguments:
        input                 Input file location or URL

    optional arguments:
        -h, --help          show this help message and exit
        -v, --version       show program's version number and exit
        -o OUTPUTFILE,
        --outputfile OUTPUTFILE
                            A name you wish to assign to the output file. Will be
                            postfixed with .html if not already added. If no
                            output file is given, output will be printed to screen
        -c {true,false},
        --css {true,false}
                            Whether (true) or not (false) to include CSS within an
                            output HTML file.

#### Basic Use

-   as a Python script
-   executed in this directory

```{=html}
<!-- -->
```
    python pylode examples/minimal.ttl -o minimal.html

This will produce the file `minimal.html` in this directory which should
match exactly the file `examples/minimal.html`.

## Examples

The [examples/
directory](https://github.com/RDFLib/pyLODE/tree/master/examples)
contains multiple pairs of RDF & HTML files generated from them using
this version of pyLODE.

You can also see rendered versions of these example files online too:

-   [minimal.html](https://rdflib.dev/pyLODE/examples/ontdoc/minimal.html)
-   [agift.html](https://rdflib.dev/pyLODE/examples/ontdoc/agrif.html)
-   [alternates.html](https://rdflib.dev/pyLODE/examples/ontdoc/alternates.html)
-   [asgs.html](https://rdflib.dev/pyLODE/examples/ontdoc/asgs.html)

## What pyLODE understands

pyLODE knows about definitional ontologies (`owl:Ontology`) and the
major elements usually found in them, such as classes (`owl:Class` or
`rdf:Class) and properties (`rdf:Property`&`owl:ObjectProperty`etc.).  To see what properties for ontology, class and RDF property documentation pyLODE currently supports, just look in the`rdf_elements.py`` file. All elements' properties supported are given in property lists there.  pyLODES won't just translate everything that you can describe in RDF into HTML! This is a conscious design choice to ensure that a certain conventional style of documented ontology is produced. However, support for new properties and ontology patterns can be made - just create an Issue on `this project's Issue tracker <https://github.com/RDFLib/pyLODE/issues>`__.  While it *does* know about instance data, such as Named Individuals, it's not really designed to document large ontologies containing class instances.  Notes on Agents --------------- pyLODE can understand both simple and complex Agent objects. You can use simple string properties like ``dc:contributor
\"Nicholas J.
Car\"`too if you really must but better would be to take advantage of real Linked Data representation, e.g. complex Agent objects with web addresses, emails, affiliations, ORCIDs and so on, e.g.:  ::      <ontology_x>         dct:creator [             sdo:name "Nicholas J. Car" ;             sdo:identifier <http://orcid.org/0000-0002-8742-7730> ;             sdo:affiliation [                 sdo:name "SURROUND Australia Pty Ldt." ;                 sdo:url "https://surroundaustralia.com"^^xsd:anyURI ;             ] ;         ] ;  See all the properties in`rdf_elements.py:AGENT_PROPS`` for a list of all the Agent properties pyLODE can handle.  Installation ============  pyLODE is `on PyPI <https://pypi.org/project/pyLODE/>`_, so you can install it using `pip <https://pypi.org/project/pip/>`_ as normal:  ::      pip install pylode   Differences from LODE ===================== -  command line access     -  you can use this on your own desktop so you don't need me to       maintain a live service for use  -  use of modern simple HTML     - no JavaScript: pyLODE generates static HTML pages  -  catering for a wider range of ontology options such as:     -  schema.org ``domainIncludes`&`rangeIncludes\`\`
for properties

-   better Agent representation
    -   see the [Notes on Agents](#notes-on-agents) section above
-   smarter CURIES
    -   pyLODE caches and looks up well-known prefixes to make
        more/better CURIES
    -   it tries to be smart with CURIE presentation by CURIE-ising all
        URIs it finds, rather than printing them
-   reference ontologies property labels
    -   pyLODE caches \~ 10 well-known ontologies (RDFS, SKOS etc),
        properties from which people often use for their ontology
        documentation. Where these properties are used, the background
        ontology\'s labels are use
-   **active development**
    -   pyLODE has been under active development since mid-2019 and is
        still very much actively developed - it\'s not just staying
        still
    -   it will be improved in foreseeable to cater for more and more
        things
    -   recent ontology documentation initiatives such as the [MOD
        Ontology](https://github.com/sifrproject/MOD-Ontology) will be
        handled, if requested

## Releases

pyLODE is under continual and constant development. The current
developers have a roadmap for enhancements in mind, which is given here,
however, since this is an open source project, new developers may join
the pyLODE dev community and change/add development priorities.

### Current Release

The current release, as of May, 2022, is **3.0.3**.

### Release Schedule

  -----------------------------------------------------------------------
  Version             Date         Description
  ------------------- ------------ --------------------------------------
  **3.0.3**           **24 May     Use of Poetry
                      2022**       

  **3.0.2**           **24 May     Support for preformatted skos:example
                      2022**       literals

  3.0.1               6 Jan 2022   Direct HTML generation using dominate;
                                   easier to maintain and extend

  2.13.2              21 December  Updated RDFlib to 6.1.1, improved test
                      2021         to properly use pytest

  2.10.0              24 May 2021  Update Windows EXE build process,
                                   simplified versioning

  2.9.1               28 Apr 2021  Support for ASCIIDOC format (OntDoc
                                   profile only)

  2.8.11              28 Apr 2021  Further changes for PyPI only

  2.8.10              27 Apr 2021  Further changes for PyPI only

  2.8.9               27 Apr 2021  PyPI enhancements only

  2.8.8               27 Apr 2021  Several small bugs fixed,
                                   auto-generation of version no. from
                                   Git tag

  2.8.6               23 Feb 20201 Fixing char encoding issues, updated
                                   examples, new test files style - per
                                   issue

  2.8.5               5 Jan 20201  Small enhancements to the Falcon
                                   server deployment option

  2.8.3               3 July 2020  Packaging bugfixes only

  2.7                 1 July 2020  Much refactoring for new profile
                                   creation ease

  2.6                 June 2020    Supports PROF profiles as well as
                                   taxonomies & ontologies

  2.4                 27 May 2020  Small improvements over 2.0

  2.0                 18 Apr 2020  Includes multiple profiles - OWP &
                                   vocpub

  1.0                 15 Dec 2019  Initial working release
  -----------------------------------------------------------------------

  : **pyLODE Release Schedule**

## License

This code is licensed using the BSD 3-Clause licence. See the [LICENSE
file](LICENSE) for the deed. Note *Citation* below though for
attribution.

## Citation

If you use pyLODE, please leave the pyLODE logo with a hyperlink back
here in the top left of published HTML pages.

## Collaboration

The maintainers welcome any collaboration.

If you have suggestions, please email the contacts below or leave Issues
in this repository\'s [Issue
tracker](https://github.com/rdflib/pyLODE/issues).

But the very best thing you could do is create a Pull Request for us to
action!

## Contacts

| *Author*:
| **Nicholas Car**
| *Data Architect*
| [Kurrawong AI](https://kurrawong.net)
| <nick@kurrawong.net>
