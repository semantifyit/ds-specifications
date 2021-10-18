# DS-V7

The specification version **DS-V7** introduces improvements and new features based on version **DS-V5**, which include:

* Adaptions to the DS and Data matching
* Adaptions and fixes for some terms, and the DS structure
* Adaptions to the `@context`
* Introduction of new terms
* Support for multilingual metadata
* Improved handling of language-tagged strings
* Introduction of Sub-DS and references (internal and external)

A detailed listing of all changes to the previous specification version can be found in the [Changelog](Changelog.md).

A detailed listing of patches for this specification version can be found below (patches are changes to the specification that happen after the specification version has been published).

## Content

* [Standard @context](./Grammar/DomainSpecification/Context.md) - The standard `@context` that is used for Domain Specifications and Verification Reports.
* [Grammar](./Grammar/README.md) - Formal specification of the components (node types, terms, etc.) of Domain Specifications and Verification Reports.
* [Changelog](Changelog.md) - A detailed listing of all changes and additions of **DS-V7** in comparison to **DS-V5**.
* [Developer Notes](DevNotes.md) - Guidelines for developers building software around Domain Specifications.
* [Examples](Examples/README.md) - Example files for Domain Specifications and Verification Reports.

## Patches

* **[2021-10-18]**
  * `ds:propertyDisplayOrder` is introduced as a new property for the DS root node and Class nodes. It replaces `sh:order` in property nodes, which is deprecated now. Details at [Grammar/DomainSpecification/DomainSpecification](./Grammar/DomainSpecification/DomainSpecification.md). A corresponding entry in the standard `@context` is added. 
  * [Examples](Examples/README.md) have been updated to use the new property instead of `sh:order`.

* **[2021-09-28]**
  * `rdf:HTML` as introduced as a new datatype. Added chapter 3.1.2. and adapted mapping-table in chapter 3.1. in [Grammar/DomainSpecification/DataType](./Grammar/DomainSpecification/DataType.md).
  * The DS-Path Syntax is introduced at [Grammar/DsPath/README.md](./Grammar/DsPath/README.md). This syntax can be used for the verification report or for any application that needs pointer to a specific node of a DS.

* **[2021-07-07]** 
  * Removed chapter 3.3. about internal references from [Grammar/DomainSpecification/Class](./Grammar/DomainSpecification/Class.md).
  * All changes and details regarding internal and external references, Super-DS, and the resolving of these relations (population) have been added to chapter 3.5. of [Grammar/DomainSpecification/DomainSpecification](./Grammar/DomainSpecification/DomainSpecification.md).
  * Examples for the population process have been added to [Examples](Examples/README.md).