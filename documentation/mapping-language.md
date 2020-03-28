# The Mapping Language

## Introduction

The mapping language is the syntax used to describe a concrete mapping model. The model is stored in text files with the file extension `.xrm`. Files with other extensions are ignored by the tooling.

The model can be in a single file or split over multiple files. Unless the model is very small, it generally makes sense to split the model at least into three separate files for source, target and the mapping.

The elements of the language belong either to the source, the target or the mapping in between.

| source | target | mapping |
| -------- | -------- | -------- |
| `logical-source`, `source-group`, `dialect`, `xml-namespace-extension`     | `vocabulary`, `language-tags`     | `output`, `map`, `template`     |


## Source


### logical-source

`logical-source` points to a source that contains the data to be mapped, for example a CSV file or a database table.

```
logical-source airport {
	type csv
	source "http://www.example.com/Airport.csv"
	
	referenceables
		id
		stop
		latitude
		longitude
}
```

* `type`: the type of the data source: *rdb*, *csv* or *xml*
* `source`: the data source to be mapped. For example a filename or tablename
* `referenceables`: the referenceable elements within the data source. For example the columns of a table or CSV file

A `logical-source` can stand on its own or be contained in a `source-group`


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





### source-group

Several `logical-source`s can be grouped together. Information that is identical for the `logical-source`s within the group can be declared on group level, like their `type` or the `source`, in order to avoid repetition.

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
	xml-namespace-extension SomeXmlNsExtension

	...
}
```



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

A `map` describes how to translate source data from one `logical-source` into RDF triples.

```
map AirportMapping from airport {
	subject template "http://airport.example.com/{0}" with id;
	
	types transit.Stop
	
	properties
		transit.route from stop with datatype xsd.integer;
		wgs84_pos.lat from latitude;
		wgs84_pos.long from longitude;
}
```



* `subject`: (mandatory) the subject IRI described as `template`
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

* can also be `with datatype` or `with languag-tag`
* can be described as `template`
* can be a `constant` value
* can be multiple terms, resulting from a `multi-reference` (see CARML [MultiTermMap extension](https://github.com/carml/carml#multitermmap-extension) )

#### map: template

A `template` can have multiple variables:

`subject template "http://venue.example.com/{0},{1}" with latitude longitude;`


[By default](https://www.w3.org/TR/r2rml/#from-template), `template` generates IRIs. If a literal is to be created instead, then the term type has to be set explicitly:

```
properties
		ex.foobaz template "{0}-{1}" with FOO BAZ as Literal;
```


#### map: other hints

There may be more than one `map` for one and the same `logical-source`.


### template

A `template` can be declared on the top-level, outside of a `map`, and subsequently be referred to from multiple mappings.
Doing so avoids repetition of IRI templates when multiple mappings involve the same resource.

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

## Special stuff

### Handling invalid identifiers

Naming rules for the identifiers are strict and certain characters are not allowed as part of the identifier. Therefore it's possible to define an additional string value next to the identifier of `referenceables`, `classes`, `properties` and `datatypes`. The identifier is always used for references inside the mapping model, but the generator will use the string value instead of the identifier.

```
vocabulary ric {
	prefix "ric" "http://www.ica.org/standards/RiC/ontology#"
    	
	properties
		title
		characteristicPhysical "characteristic-Physical"
}
```


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

Inside the `map` element, ending lines with a semicolon is mandatory for the subject IRI and for predicate object mappings.
There is no line end for subject type mappings.
