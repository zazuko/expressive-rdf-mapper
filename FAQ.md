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