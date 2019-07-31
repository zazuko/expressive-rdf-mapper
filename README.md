# A friendly language for mappings to RDF

Allows you to express data mappings to RDF in a friendly domain specific language (DSL) and generates output in [R2RML](http://www.w3.org/TR/r2rml/) and [RML](http://rml.io/).

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

*Note:* Language and generators are work-in-progress and subject to change in the future.


## Installation instructions

**TODO:** 1) get eclipse  2) install DSL from update-site


## Mapping examples

For complete examples, have a look at the projects in the [mapping-examples](mapping-examples) folder.


## Documentation

* **TODO:** Link to blogpost (motivation)
* [The Mapping Language](documentation/mapping-language.md) describes the elements of the language


## Support

Please report issues and feature requests on Github.

Pull requests are very welcome.