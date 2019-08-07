# The Mapping Language

## Introduction

The mapping language is the syntax used to describe a concrete mapping model. The model is stored in text files with the file extension `.xrm`. Files with other extensions are ignored by the tooling.

The model can be in a single file or split over multiple files. Unless the model is very small, it generally makes sense to split the model at least into separate files for source/target/mapping.

The elements of the language belong either to the source, the target or the mapping in between.

| source | target | mapping |
| -------- | -------- | -------- |
| `source-types`, `logical-source`, `source-group`     | `vocabulary`, `datatypes`, `language-tags`     | `map`     |


## Source

### source-types

`source-types` are the different types of data sources with their [referenceFormulation](http://rml.io/spec.html#referenceFormulation)

```
source-types {
	rdb referenceFormulation "rr:SQL2008"
	csv referenceFormulation "ql:CSV"
}
```


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

* `type`: the type of the data source, one of the defined `source-types`
* `source`: the data source to be mapped. For example a filename or tablename
* `referenceables`: the referenceable elements within the data source. For example the columns of a table or CSV file

A `logical-source` can stand on its own or be contained in a `source-group`


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


## Target

### vocabulary

### datatypes

### language-tags


## Mapping

### map

A `map` describes how to translate source data from one `logical-source` into RDF triples.

```
map AirportMapping from airport {
	subject template "http://airport.example.com/{0}" with id;
	
	types transit.Stop;
	
	properties
		transit.route from stop with datatype xsd.int;
		wgs84_pos.lat from latitude;
		wgs84_pos.long from longitude;
}
```



* `subject`: the subject IRI described as `template`
* `types`: zero or more `classes` from a `vocabulary`
* `properties`: zero or more `properties` from a `vocabulary` and the source for their value

```
map Permit from permits.t_permit {
	subject template "https://permits.example.org/permits/{0}" with PERMIT_ID;
	
	properties
		schema.description from PERMIT_DESC_FR with language-tag fr;
		schema.description from PERMIT_DESC_EN with language-tag en;

		ex.status template "https://permits.example.org/statuses/{0}" with PERMIT_STATUS;
		
		ex.agency link Agency with PERMIT_AGENCY_FK;
}
```

The property value (the object of the triple):

* can also be `with datatype` or `with languag-tag`
* can be described as `template`
* can be a `link` to another `map`

#### map: template

A `template` can have multiple variables:

`subject template "http://venue.example.com/{0},{1}" with latitude longitude;`

#### map: link

`link` is syntactic sugar of the DSL and not part of R2RML or RML. Using `link` eliminates the need for copy/pasting a template by re-using the `subject template` of the linked `map` instead. This makes refactorings easier.

The following example shows how `link` works. The two object mappings for `ex.agency` are equivalent (Note: for the sake of brevity, parts of the mapping are omitted):

```
map Agency from permits.t_agency {
    subject template "https://permits.example.org/agencies/{0}" with AGENCY_ID;
    ...
}

map Permit from permits.t_permit {
    ...

    properties
        ex.agency link Agency with PERMIT_AGENCY_FK;
//same: ex.agency template "https://permits.example.org/agencies/{0}" with PERMIT_AGENCY_FK;
}
```

#### map: other hints

There may be more than one `map` for one and the same `logical-source`.


## Special stuff

### Handling invalid identifiers

Naming rules for the identifiers are strict and certain characters are not allowed as part of the identifier. Therefore it's possible to define an additional string value next to the identifier of `referenceables`, `classes`, `properties` and `datatypes`. The identifier is always used for references inside the mapping model, but the generator will use the string value instead of the identifier.

```
vocabulary ric {
	prefix "ric:" "http://www.ica.org/standards/RiC/ontology#"
    	
	properties
		title
		characteristicPhysical "characteristic-Physical"
}
```

### Semicolons

Using semicolons in the `map` is optional but improves code-assist.
