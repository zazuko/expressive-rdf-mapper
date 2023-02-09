# Changelog of Expressive RDF Mapper (XRM)

## Version 1.3.0 (2023-02-09)

* JSON mapping: Support `ql:JSONPath` in (CA)RML output


## Version 1.2.0 (2022-07-13)

* Optional specification of target named graph
* A class, property or datatype from a vocabulary can also be used as constant value
* CSV mapping: Support declaring null value on table level (with CSVW output)
* Validation errors for duplicated (ambiguous) names (source, vocabulary, map, etc.)
* Explicit typing of generated (R2)RML mappings with `rr:TriplesMap` improves compatibility with more mapping processors
* Show templates in code-assist also if template is declared in another file
* Show descriptions from vocabularies in code-assist (only works in Eclipse runtime)
* Fix: no doublequotes on a string constant containing an IRI
* Fix: syntax highlighting in vscode
* Fix: improved code-assist in vscode
* Fix: stricter syntax compliance of generated turtle files
* Lifecycle: Requires Java 11 or newer
* Lifecycle: Upgrade to xtext 2.26. This includes support for running on Java 17


## Version 1.0.0 (2020-05-08)

* CSV mapping: Generate CSV on the Web (CSVW) output
* XML mapping: Support CARML extensions (`carml:Stream`, `carml:multiReference`, `carml:declaresNamespace`)
* SQL query as source (R2RML view `rr:sqlQuery`)
* Nesting structures (`rr:parentTriplesMap`)
* Optional `rr:termType` assignments
* Mapping to a constant object value
* Quickfix for missing properties
* Code formatter (`Ctrl + Shift + F`)
* Source types are now part of the grammar
* Datatype definitions moved into vocabulary
* Mandatory LINE_ENDS (semicolon) for PredicateObjectMappings in `map`
* Template re-use possibility replaces `link`


## Version 0.9.0 (2019-08-05)

* The first public version
* Support for R2RML and RML output
