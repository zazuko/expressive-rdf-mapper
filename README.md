# Expressive RDF Mapper XRM

Expressive RDF Mapper (XRM) is an RDF mapping language for humans. It greatly facilitates the mapping of non-RDF resources to RDF.

XRM is available as an Eclipse and Visual Studio Code Plugin.

Expressive RDF Mapper allows you to express data mappings to RDF in a friendly domain specific language (DSL) and generates output in
[R2RML](http://www.w3.org/TR/r2rml/), [RML](https://rml.io/specs/rml/), [CARML](https://github.com/carml/carml) and [CSV on the Web](https://w3c.github.io/csvw/primer/) format. 

Expressive RDF Mapper is a commercial product, see the [End-User License Agreement](EULA.md) and our [product page](https://zazuko.com/products/expressive-rdf-mapper/) at zazuko.com for more details.

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

Expressive RDF Mapper is using Java, a Java runtime environment (JRE) is required to run the mapper in Eclipse or Visual Studio Code. We recommend [AdoptOpenJdk](https://adoptopenjdk.net/) for those who do not have a JRE installed yet. If you have a recent Oracle JDK or JRE installed, you should be good to go.

We've tested the plugin with:

* OpenJDK 8 (LTS)
* OpenJDK 11 (LTS)
* OpenJDK 14 (Latest)
* Java SE Development Kit 8

### Using Eclipse

If you are installing the plugin into an existing Eclipse installation please make sure that it is a recent Eclipse release. The plugin does not work properly with older Eclipse releases. If you get errors during the installation process about unresolved dependencies, your Eclipse is most probably too old.

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

### Using Visual Studio Code

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

For commercial use, including commercial support, consult our [product page](https://zazuko.com/products/expressive-rdf-mapper/) at zazuko.com for more details.

Please report issues and feature requests on Github. If you have other questions please post a message in the [RDF.community discussion forum](https://discuss.rdf.community/).
