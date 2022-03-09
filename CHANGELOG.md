# Changelog of rdf-mapping-dsl

## next

* Optional specification of target named graph
* A class, property or datatype from a vocabulary can also be used as constant value
* CSV mapping: Support declaring null value on table level (with CSVW output)
* Validation errors for duplicated (ambiguous) names (source, vocabulary, map, etc.)
* Explicit typing of generated (R2)RML mappings with `rr:TriplesMap` improves compatibility with more mapping processors
* Show templates in code-assist also if template is declared in another file
* Show descriptions from vocabularies in code-assist (only works in Eclipse runtime)
* Fix: no doublequotes on a string constant containing an IRI

## Version 1.0.0

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


## Version 0.9.0

* The first public version
* Support for R2RML and RML output
