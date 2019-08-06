# A friendly language for mappings to RDF

Allows you to express data mappings to RDF in a friendly domain specific language (DSL) and generates output in [R2RML](http://www.w3.org/TR/r2rml/) and [RML](http://rml.io/).

```
map TriplesMap1 from EMPLOYEE {
  subject template "http://data.example.com/employee/{0}" with EMPNO;
  
  types ex.Employee;
  
  properties
    ex.name from ENAME;
}
```

*Note:* Language and generators are work-in-progress and subject to change in the future.
Only a subset of R2RML and RML is supported. RDB and CSV support already works well in practice, whereas JSON and XML support is still missing some parts to be of practical use.


## Installation instructions

1) Download and install Eclipse: https://www.eclipse.org/downloads/packages/
   * Recommended package: *Eclipse IDE for Java Developers*
   * Hint: You need a [Java runtime environment (JRE)](https://wiki.eclipse.org/Eclipse/Installation#Install_a_JVM) to use Eclipse (Java SE 8 or greater).

2) Start Eclipse and install the extension:
   * Help > Install New Software
   * Click `Add..` to add a new repository and fill in the following details:
     * Name: *Zazuko RDF Mapping DSL Updates*
     * Location: https://download.zazukoians.org/rdf-mapping-dsl/updates/
   * Select *RDF Mapping DSL* from the list and click `Next>`
   * Confirm the security warning about unsigned content by clicking on `Install anyway`


## Mapping examples

For complete examples, have a look at the projects in the [mapping-examples](mapping-examples) folder.
Once you installed the extension in Eclipse a good way to start is to start playing with them.

1. Clone this repository.
2. In Eclipse, click _File > Import_.
3. In the Import wizard: Expand General and then click _Projects from Folder or Archive_ . Click Next . Click _Directory_ and select the cloned repository. Click Finish to add it to your Eclipse.
4. Open one of the samples like `employee-mapping` and try to clone the current mapping.
5. If everything was valid, you can find the generated R2RML file in the `src-gen` folder.

## Documentation

* **TODO:** Link - This blogpost mentions our motivation and some ideas around future development
* [The Mapping Language](documentation/mapping-language.md) describes the elements of the language


## Support

Please report issues and feature requests on Github.

Zazuko offers commercial support for this tool, get in [contact with us](mailto:info@zazuko.com?subject=RDF Mapping DSL) if you would like to know more.
