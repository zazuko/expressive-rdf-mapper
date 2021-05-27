# Frequently Asked Questions

## I changed something and saved the file, but the generated mapping doesn't automatically reflect my change. What can I do?

Xtext constantly watches the `.xrm` files in your Eclipse project and validates the syntax (ie. *on save*). When the syntax is valid, it automatically writes the generated mapping files to `./src-gen`. The build is incremental.

The build can also be forced to regenerate all mapping files in the Project: _Project > Clean_


## I want to keep the generated mapping file under version control. Why are generated files added to `.gitignore` automatically?

The git provider in Eclipse defaults to automatically ignore derived resources.
Auto ignore derived resources can be disabled in Eclipse preferences: 

1. _Window > Preferences_
2. _Team > Git > Projects > [ ] Automatically ignore derived resources ..._


## Is there a code formatter for the mapping language?

Yes. To format a file, open it and hit `Ctrl-Shift-F`


## Is a government organization considered Non-Commercial use?

Governmental or intergovernmental organizations are not covered by the Non-Commercial License. [Contact us](mailto:info@zazuko.com?subject=XRM) for assistance in identifying your license needs.


## I try to map a CSV file that has column headers with whitespace and/or special characters in the column names. The editor shows errors on those lines. How can I resolve this?

Certain characters are not allowed as part of the identifiers used in XRM. Aliasing is used to handle these cases. Add quotes around the name and prepend it with a valid alias for referencing the element inside XRM.

[Read more about this](documentation/mapping-language.md#escaping-special-chars)


## The editor shows errors on some of the elements in the vocabulary I'm using. How can I resolve this?

Certain characters are not allowed as part of the identifiers used in XRM. Aliasing is used to handle these cases. Add quotes around the name and prepend it with a valid alias for referencing the element inside XRM.

[Read more about this](documentation/mapping-language.md#handling-invalid-identifiers)