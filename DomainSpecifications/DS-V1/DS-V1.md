# DS-V1

This is the first concept of Domain Specifications. The main idea is to create a pattern to which schema.org annotations have to comply (primarily JSON-LD annotations found on websites). 

The file format for this DS version is JSON-LD. The namespace `"dsv": "http://ontologies.sti-innsbruck.at/dsv/"` is used to introduce new vocabulary terms needed to express the needed constraints. `dsv` stands for Domain Specification Vocabulary.

## Content:

* [Examples](./Examples/Examples.md)
* [Grammar](./Grammar/Grammar.md)

There is no further documentation on the verification algorithm, nor the resulting verification report. In short: The verification algorithm for this version consists of recursively checking every node of the tree-structured JSONLD annotation against the corresponding node in the DS. Every irregularity results in an error entry for the verification outcome, which is a simple JSON file for which no further specification is given.