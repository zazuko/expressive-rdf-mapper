# The Mapping Language

## Introduction

The mapping language is the syntax used to describe a concrete mapping model. The model is stored in text files with the file extension `.xrm`. Files with other extensions are ignored by the tooling.

The model can be in a single file or split over multiple files. Unless the model is very small, it generally makes sense to split the model at least into three separate files for source, target and the mapping.

The elements of the language belong either to the source, the target or the mapping in between.

| source | target | mapping |
| -------- | -------- | -------- |
| `logical-source`, `source-group`, `dialect`, `xml-namespace-extension`, `iterator` | `vocabulary`, `language-tags`     | `output`, `map`, `template`     |

XRM is [grammar driven](https://eclipse.dev/Xtext/documentation/301_grammarlanguage.html), built with Xtext. Have a look at the [XRM language definition](https://github.com/zazuko/xrm/blob/main/com.zazuko.rdfmapping.dsl.parent/com.zazuko.rdfmapping.dsl/src/com/zazuko/rdfmapping/dsl/RdfMapping.xtext) if you are interested to know the details of the syntax.


## Source


### logical-source

`logical-source` points to a source that contains the data to be mapped, for example a CSV file or a database table.

```
logical-source airport {
	type csv
	source "http://www.example.com/Airport.csv"
	iterator "$."
	
	referenceables
		id
		stop
		latitude
		longitude
}
```

* `type`: the type of the data source: *rdb*, *csv*, *json* or *xml*
* `source`: the data source to be mapped. For example a filename or tablename
* `iterator`: the logical iterator defines the iteration loop used to map the data of the input source (optional in case of CSV, required for JSON and XML)
* `referenceables`: the referenceable elements within the data source. For example the columns of a table or CSV file

A `logical-source` can stand on its own or be contained in a `source-group`

#### Aliasing reference names

Certain characters are not allowed in the identifier of `referenceables`. In CSV we sometimes find column headers with whitespace and/or special characters. Also many [XPath and JSONPath expressions](#xpath-and-jsonpath-usage) contain special characters. XRM will show you errors on those lines.

In order to handle these cases, put the reference name (or expression) in quotes and prepend it with a valid alias identifier for referencing the element inside XRM.

```
logical-source airport {
	type csv
	source "http://www.example.com/Airport.csv"
	
	referenceables
		id
		stop
		latitude
		longitude
		specRow "Spec. Row"
}
```

In this example the column name `Spec. Row` from the CSV is aliased and will be referenced using the identifier `specRow` instead.


#### XPath and JSONPath usage

XPath expressions are used to refer to the elements of an XML data source.

JSONPath expressions are used to refer to the elements of a JSON data source.

In XRM, these types of expressions are found in two places:
- `iterator`: The repetition pattern that emits elements/objects for mapping
- `referenceables`: The reference for extracting a data value for a given logical iteration


The following sample snippet shows a `logical-source` definition for a JSON data source, using some JSONPath expressions:

```
logical-source personen_desc {
  type json
  source "stdin"
  iterator "$..[?(@.thesaurus == 'Personenbegriffe')]"

  referenceables
    identifier
    title
    description
    recordId "records[*].id"
    descriptorId "related_descriptor_set[*].to_descriptor.identifier"
}
```

Please note the aliasing of the two expression for the `recordId` and `descriptorId` referenceables.

For a more complete JSON sample, have a look at:
- https://github.com/zazuko/xrm-xml-workflow/blob/main/mappings/sources.xrm
- https://github.com/zazuko/xrm/issues/129

For a more complete XML sample, have a look at:
- https://github.com/zazuko/xrm-xml-workflow/blob/main/mappings/sources.xrm
- https://github.com/zazuko/expressive-rdf-mapper/blob/main/mapping-examples/characters-mapping/characters.xrm



#### SQL query as source

[R2RML views](https://www.w3.org/TR/r2rml/#r2rml-views) are useful in cases where differing mappings have to be applied, depending on conditions in the source data. Instead of a table name, the source is a SQL query: 

```
logical-source foo42 {
  source query "SELECT * FROM foo WHERE res = 42"
  ...
}

```


#### Missing value indicators in CSV files

When dealing with CSV files, sometimes a column that contains numbers will contain special values, such as "X", when a value is unknown. CSV on the Web (`csvw`) allows to specify values that are being used to indicate missing values. In the following example, the latitude column might usually be numeric but hold an "X" if there is no latitude value:

```	
	referenceables
		id
		latitude null "X"
```


In order to apply a `null` value for all columns in a table, it can also be declared on the `logical-source` or `source-group` level:

```
logical-source airport {
	type csv
	source "airport"
	null "X"

	referenceables
		id
		stop
		latitude
		longitude
}
```


### source-group

Optionally, several `logical-source`s can be grouped together. Information that is identical for all the `logical-source`s within the group can be declared on group level, in order to avoid repetition. Like their `type` and `source`, if applicable.

```
source-group permits {
	type rdb

	logical-source t_agency {
		source "t_agency"

		referenceables
			AGENCY_ID
			AGENCY_NAME_DE
	}
    
	logical-source t_permit {
		source "t_permit"
    	
		referenceables
			PERMIT_ID
			PERMIT_DESC_DE
	}
}
```

For CSV sources, also `null` (the missing value indicator) and `dialect` can be declared on group level.

For XML sources and with *carml* output, also `xml-namespace-extension` can be declared on group level.


### dialect

CSV on the Web (`csvw`) supports [dialect descriptions](https://www.w3.org/TR/2015/REC-tabular-metadata-20151217/#dialect-descriptions)   to provide hints to parsers about how to parse a CSV file.

A complete dialect description looks as follows:

```
dialect MyCsvDialect {
	delimiter ","
	commentPrefix "#"
	doubleQuote true
	encoding "utf-8"
	header true
	headerRowCount 1
	lineTerminators "\\r\\n"
	quoteChar '\\"'
	skipBlankRows false
	skipColumns 0
	skipInitialSpace false
	skipRows 0
	trim false
}
```

Using dialects is optional and partial descriptions are possible as well:

```
dialect MyCsvDialect {
	delimiter ";"
}
```

In order to use a `dialect`, it has to be referenced from the respective `logical-source` or `source-group`:

```
logical-source airport {
	type csv
	source "http://www.example.com/Airport.csv"
	dialect MyCsvDialect
```


### xml-namespace-extension

When working with XML documents, it is often necessary to specify namespaces in order to identify a node's qualified name in XPath expressions. [CARML](https://github.com/carml/carml) includes a [XML namespace extension](https://github.com/carml/carml#xml-namespace-extension) to convey these namespaces to the processing engine.

One or more namespaces are defined within a `xml-namespace-extension`. In order to use a `xml-namespace-extension`, it has to be referenced from the respective `logical-source` or `source-group`: 

```
xml-namespace-extension SomeXmlNsExtension {
	prefix "exa" "http://www.example.com/audios/1.0/"
	prefix "exb" "http://www.example.com/books/1.0/"
}

logical-source foo {
	type xml
	source "http://example.org/example.xml"
	xml-namespace-extension SomeXmlNsExtension

	...
}
```

Note that `xml-namespace-extension` has to appear _after_ any `source` definition, it will yield an error otherwise.


## Target

### vocabulary

A `vocabulary` lists `classes`, `properties` and `datatypes` available for mapping.

```
vocabulary schema {
	prefix "schema" "https://schema.org/"
	
	classes	
		GovernmentPermit
		GovernmentOrganization
	
	properties
		name
		alternateName
		description
		
	datatypes
		Text
}
```

The following is an example for refering to the class *schema.GovernmentOrganization* in a `map`:

`types schema.GovernmentOrganization`


#### Aliasing elements that have special characters in their name

Certain characters are not allowed in the identifier of `classes`, `properties` and `datatypes`. To handle these cases, put the name in quotes and prepend it with a valid alias identifier for referencing the element inside XRM.

```
vocabulary ric {
	prefix "ric" "http://www.ica.org/standards/RiC/ontology#"
    	
	properties
		characteristicPhysical "characteristic-Physical"
}
```

In this example the property `characteristic-Physical` is aliased and will be referenced using the identifier `characteristicPhysical` instead.


### language-tags

The `language-tags` available for mapping.

```
language-tags {
	de
	en
	fr
	it
}
```

The following is an example for refering to the language *en* in a `map`:

`schema.name from AGENCY_NAME_EN with language-tag en;`


## Mapping

### output

The `output` header is used to choose the format of the generated mapping file.

The supported output formats are:

* `r2rml`: [R2RML](http://www.w3.org/TR/r2rml/)
* `rml`: [RML](https://rml.io/specs/rml/)
* `carml`: [CARML](https://github.com/carml/carml)
* `csvw`: [CSV on the Web](https://w3c.github.io/csvw/primer/)

```
output r2rml

map AirportMapping from airport {
	subject template "http://airport.example.com/{0}" with id;
}
```

### map

A `map` describes how to translate source data `from` one `logical-source` into RDF triples.

```
map AirportMapping from airport {
	subject template "http://airport.example.com/{0}" with id;
	
	graphs
		template "http://airport.example.com/graph/stop/{0}" with id;
		constant "http://www.w3.org/ns/r2rml#defaultGraph";
	
	types transit.Stop
	
	properties
		transit.route from stop with datatype xsd.integer;
		wgs84_pos.lat from latitude;
		wgs84_pos.long from longitude;
}
```



* `subject`: (mandatory) the subject IRI described as `template`
* `graphs`: (optional) one or more IRIs of the target [named graph](https://rml.io/specs/rml/#named-graph). For details on this, see https://github.com/zazuko/xrm/issues/135#issuecomment-1840964288
* `types`: (optional) one or more `classes` from a `vocabulary`
* `properties`: (optional) one or more `properties` from a `vocabulary` and the source for their value

```
map Permit from permits.t_permit {
	subject template "https://permits.example.org/permits/{0}" with PERMIT_ID;
	
	properties
		schema.description from PERMIT_DESC_FR with language-tag fr;
		schema.description from PERMIT_DESC_EN with language-tag en;

		ex.status template "https://permits.example.org/statuses/{0}" with PERMIT_STATUS;
		
		ex.foo constant "bar";
}
```

The property value (the object of the triple):

* Can also be `with datatype` or `with languag-tag`
* Can be described as `template`
* Can be a `constant` value. Either a string or a `class`, `property` or `datatype` from a `vocabulary`
* Can be a join to another source, using `parent-map`. For details on this, see https://github.com/zazuko/xrm/issues/31#issuecomment-1853921584


Note regarding the use of `multi-reference`: `multi-reference` was specifically added to support the CARML MultiTermMap extension in XRM. Be aware that this extension [got deprecated](https://github.com/carml/carml/issues/52#issuecomment-690999094) with CARML release v0.3.0 (2020-09-11). 


The following block shows some common combinations of input data value(s), XRM object map and resulting RDF term: (examples validated with JSON input, carml output and carml processor)

```
// foo = hello
from foo ;  // => "hello"^^xsd:string
	
// foo = 42
from foo with datatype xsd.integer ;  // => "42"^^xsd:integer

// foo = hello
from foo with language-tag en ;  // => "hello"@en
	
// foo = hello
template "https://data.example.org/{0}" with foo ;  // => <https://data.example.org/hello>
	
// foo = hello, bar = world
template "{0} {1}!" with foo bar as Literal ;  // => "hello world!"^^xsd:string
	
// foo = hello, bar = world
template "https://data.example.org/{0}/{1}" with foo bar ;  // => <https://data.example.org/hello/world>
	
// --------------------------------------------

// foo = https://www.wikidata.org/wiki/Q1424987
from foo with datatype xsd.anyURI ;  // => "https://www.wikidata.org/wiki/Q1424987"^^xsd:anyURI
	
// foo = http://www.wikidata.org/entity/Q31465098
from foo as IRI ;  // => <http://www.wikidata.org/entity/Q31465098>
```


#### map: template

A `template` can have multiple variables:

`subject template "http://venue.example.com/{0},{1}" with latitude longitude;`


[By default](https://www.w3.org/TR/r2rml/#from-template), `template` generates IRIs. If a literal is to be created instead, then the term type has to be set explicitly:

```
properties
	ex.foobaz template "{0}-{1}" with FOO BAZ as Literal;
```

Templates can be re-used, this is especially useful for IRI templates. See the [template](#template) section below.


#### map: other hints

There may be more than one `map` for one and the same `logical-source`.

For dealing with XML or JSON sources, see also the section about [XPath and JSONPath usage](#xpath-and-jsonpath-usage). These path expressions are defined as `referenceables` inside the `logical-source`.


### template

A `template` can be declared on the top-level, outside of a `map`, and subsequently be referred to from multiple mappings.
Doing so avoids repetition of IRI templates when multiple mappings involve the same resource.

In larger projects, we found it to be practical to have a dedicated file for all the templates. Having all IRI templates defined in one place makes it easier to refactor them.

```
template airportIri "http://airport.example.com/{0}"
template airlineIri "http://airline.example.com/{0}"

map AirportMapping from airport {
	subject template airportIri with id;

	properties
		wgs84_pos.lat from latitude;
		wgs84_pos.long from longitude;
}

map AirportOwnership from airportowners {
	subject template airportIri with id;

	properties
		ex.owner from ownership;
}

map AirlineAtAirport from airlineairport {
	subject template airlineIri with id;

	properties
		ex.airportServed template airportIri with airportId;
}
```

In this example the `airportIri` template is used three times, twice for the subject map and once to create a link.


## Special stuff

### Handling invalid identifiers

Aliasing elements that have special characters in their name.

Certain characters are not allowed as part of the identifiers used in XRM. Aliasing is used to handle these cases, when `referenceables`, `classes`, `properties` or `datatypes` have special characters in their name that
make them invalid identifiers in XRM. To resolve this, add quotes around the name and prepend it with a valid alias for referencing the element inside XRM.

The alias identifier is used for references inside the XRM mapping. The generated output on the other hand contains the quoted string value instead of the alias.

```
vocabulary ric {
	prefix "ric" "http://www.ica.org/standards/RiC/ontology#"
    	
	properties
		characteristicPhysical "characteristic-Physical"
}
```

In this example the property `characteristic-Physical` is aliased and will be referenced using the identifier `characteristicPhysical` instead.


### Escaping keywords

Identifiers that conflict with keywords of the mapping language need to be escaped with the `^` character. 

The following example shows how *map* is escaped and used as an identifier:

```
map FooMapping from foo {
	subject template "http://foo.example.com/{0}" with id;

	properties
		ex.^map from bar;
}

vocabulary ex {
	prefix "ex" "http://example.org/"

	properties
		^map
}
```


### Semicolons

Inside the `map` element, ending lines with a semicolon is mandatory for the subject IRI, the named graph IRI and for the predicate object mappings.
There is no line end for subject type mappings.
