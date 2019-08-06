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
Once you installed the extension in Eclipse a good way to start is to start playing with them.

1. Clone this repository.
2. In Eclipse, click _File > Import_.
3. In the Import wizard: Expand General and then click _Projects from Folder or Archive_ . Click Next . Click _Directory_ and select the cloned repository. Click Finish to add it to your Eclipse.
4. Open one of the samples like `employee-mapping` and try to clone the current mapping.
5. If everything was valid, you can find the generated R2RML file in the `src-gen` folder.

## Documentation

* **TODO:** Link to blogpost (motivation)
* [The Mapping Language](documentation/mapping-language.md) describes the elements of the language


## Support

Please report issues and feature requests on Github.

Zazuko offers commercial support for this tool, get in [contact with us](mailto:info@zazuko.com?subject=RDF Mapping DSL) if you would like to know more.
