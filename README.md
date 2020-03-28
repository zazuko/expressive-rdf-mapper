# A friendly language for mappings to RDF

Allows you to express data mappings to RDF in a friendly domain specific language (DSL) and generates output in
[R2RML](http://www.w3.org/TR/r2rml/), [RML](http://rml.io/), [CARML](https://github.com/carml/carml) and [CSV on the Web](https://w3c.github.io/csvw/primer/) format.

```
output r2rml

map TriplesMap1 from EMPLOYEE {
  subject template "http://data.example.com/employee/{0}" with EMPNO;
  
  types ex.Employee
  
  properties
    ex.name from ENAME;
}
```


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

If you receive an error message about missing dependencies, then make sure that the option *"Contact all update sites during install to find required software"* is selected in the install dialog.

If you try installing in an older version of Eclipse and receive an error message about missing dependency
"org.eclipse.xtext.xbase.lib 2.14.0" or similar, then follow instructions on https://www.eclipse.org/Xtext/download.html to install the Xtext plugins for Eclipse manually, before installating the DSL extension. If that doesn't solve the problem, then please install the recommended Eclipse version mentioned in step 1).

## Mapping examples

For complete examples, have a look at the projects in the [mapping-examples](mapping-examples) folder.
Once you installed the extension in Eclipse a good way to start is to start playing with them.

1. Clone this repository.
2. In Eclipse, click _File > Import_.
3. In the Import wizard: Expand General and then click _Projects from Folder or Archive_ . Click Next . Click _Directory_ and select the cloned repository. Click Finish to add it to your Eclipse.
4. Open one of the samples like `employee-mapping` and try to modify the current mapping.
5. Xtext constantly watches the `.xrm` files in your Eclipse project and validates the syntax (ie. *on save*). When the syntax is valid, it automatically writes the generated mapping file(s) to `./src-gen`.

## Documentation

* The blogpost [RDF and Domain Specific Languages (DSL) - A Perfect Match](https://zazuko.com/blog/rdf-and-dsl-a-perfect-match) describes our motivation and some ideas for future developments
* [The Mapping Language](documentation/mapping-language.md) describes the elements of the language
* [Frequently Asked Questions](FAQ.md)


## License and Support

The included [End-User License Agreement](EULA.md) covers personal, non-commercial use.

For commercial use, including commercial support, please get in [contact with us](mailto:info@zazuko.com?subject=RDF%20Mapping%20DSL) 

Please report issues and feature requests on Github.