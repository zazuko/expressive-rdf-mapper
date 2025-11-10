# Expressive RDF Mapper (XRM)

Expressive RDF Mapper (XRM) is an RDF mapping language for humans. It greatly facilitates the mapping of non-RDF resources to RDF, offering several features such as autocomplete, code lookup, and a type-safe experience.

XRM is available as an Eclipse and Visual Studio Code Plugin.

Expressive RDF Mapper allows you to express data mappings to RDF in a friendly domain specific language (DSL) and generates output in
[R2RML](http://www.w3.org/TR/r2rml/), [RML](https://rml.io/specs/rml/), [CARML](https://github.com/carml/carml) and [CSV on the Web](https://w3c.github.io/csvw/primer/) format.

Expressive RDF Mapper is free software, see the [MIT License](LICENSE.txt) and our [product page](https://zazuko.com/products/expressive-rdf-mapper/) at zazuko.com for more details. The software is available under the MIT license, source code can be found [here](https://github.com/zazuko/xrm). We offer commercial support for it, get in [contact](mailto:info@zazuko.com?subject=XRM) with us.

```
output r2rml

map Employee from EMPLOYEE {
  subject template "http://data.example.com/employee/{0}" with EMPNO;

  types ex.Employee

  properties
    ex.name from ENAME;
}
```


## Installation instructions

Expressive RDF Mapper is using Java, a Java runtime environment (JRE) is required to run the mapper in Eclipse or Visual Studio Code. We recommend [Adoptium](https://adoptium.net/) for those who do not have a JRE installed yet.

We've tested the plugin with:

* OpenJDK 11
* OpenJDK 17
* OpenJDK 21
* OpenJDK 25

### Using Eclipse

1) Download and install Eclipse "Eclipse IDE 2023-12" (or newer): https://www.eclipse.org/downloads/packages/
   * Package: *Eclipse IDE for Java and DSL Developers*
   * Hint: The Eclipse Installer now also includes a JRE. A Java 11 or newer JRE/JDK is required.

2) Start Eclipse and install the extension:
   * Help > Install New Software
   * Click `Add..` to add a new repository and fill in the following details:
     * Name: *Expressive RDF Mapper (XRM) Updates*
     * Location: https://download.zazukoians.org/rdf-mapping-dsl/updates/
   * Select *Expressive RDF Mapper (XRM)* from the list and click `Next>`
   * Confirm the security warning about unsigned content by selecting the "com.zazuko.rdfmapping.dsl" bundles and features and clicking on `Trust Selected`

If you receive an error message about missing dependencies, then make sure that the option *"Contact all update sites during install to find required software"* is selected in the install dialog.

The plugin will not work properly with older Eclipse releases! Please install the recommended Eclipse version mentioned in step 1).

### Using Visual Studio Code

1. Download the extension: <https://download.zazukoians.org/expressive-rdf-mapper/expressive-rdf-mapper-1.6.0.vsix>.
1. Open Visual Studio Code and navigate to the "Extensions" (select View > Extensions from the menu).
1. Top-right corner of the "Extensions" pane, use the `...` button and choose "Install from VSIX…".
1. Select the `.vsix` file you downloaded, click *Install*.

The extension will detect and handle `.xrm` mapping files.

Details of editor issues are shown in the problems view. Use `View > Problems (Ctrl-Shift-M)` to activate it.

## Mapping examples

For complete examples, have a look at the projects in the [mapping-examples](mapping-examples) folder.
Once you installed the extension a good way to start is to start playing with them.

1. Open one of the samples like `employee-mapping` and try to modify the current mapping.
2. Xtext constantly watches the `.xrm` files in your project and validates the syntax (ie. *on save*). When the syntax is valid, it automatically writes the generated mapping file(s) to `./src-gen`.

## Tutorials & Documentation

* Video Tutorial [Mapping non-RDF data to a graph: Hands-on](https://youtu.be/gl2qzXmJypw)
* Template for creating automated [CSV to RDF pipelines](https://github.com/zazuko/xrm-csv-workflow), using R2RML, DuckDB and Ontop 
* Template for creating automated [CSV to RDF pipelines](https://github.com/zazuko/xrm-csvw-workflow), using CSV on the Web
* Template for creating automated [relational database (RDB) to RDF pipelines](https://github.com/zazuko/xrm-r2rml-workflow)
* Template for creating automated [JSON or XML to RDF pipelines](https://github.com/zazuko/xrm-xml-workflow)
* Try out mapping and transforming a MySQL database into a graph using our [Gitpod sample workspace](https://github.com/zazuko/gitpod-example) (in the cloud, without requiring a local toolchain installation)

* [The Mapping Language](documentation/mapping-language.md) describes the elements of the language.
* [Frequently Asked Questions](FAQ.md)
* Consult our [product page](https://zazuko.com/products/expressive-rdf-mapper/) for general information.
* The blogpost [RDF and Domain Specific Languages (DSL) - A Perfect Match](https://zazuko.com/blog/rdf-and-dsl-a-perfect-match) describes our motivation and some ideas for future developments. Note that this was written for the first release of XRM, some of the ideas are already supported now.

## License and Support

Expressive RDF Mapper is free software, released under the [MIT License](LICENSE.txt)

For commercial support, consult our [product page](https://zazuko.com/products/expressive-rdf-mapper/) at zazuko.com for more details.

Please report issues and feature requests on Github. If you have other questions please post a message in the [RDF.community discussion forum](https://github.com/rdf-community/discussions/discussions).
