
# Vocabulary for Domain Specifications

## Introduction

Domain Specifications are defined in an RDF-conform format (JSON-LD). Therefore, we introduce a vocabulary that contains new terms (where we can't reuse existing terms) needed for DomainSpecifications and its related uses (e.g. DS-Lists, verification results). The vocabulary file should be available at its namespace http://vocab.sti2.at/ds/ (content negotiation enabled). The vocabulary file can also be found [on this Wiki](./domain-specification-vocabulary.jsonld), please check that out for details.

## Uses

This vocabulary should cover the introduced terms needed for following document types:

* **Domain Specification** - A document that specifies constraints for a Semantic Annotation or entity in a Knowledge Graph (KG). Examples can be found at [https://semantify.it/domainspecifications/](https://semantify.it/domainspecifications/).

* **List** - A document that contains a set of Vocabularies, or a set of DomainSpecifications. Examples can be found at [https://semantify.it/lists/](https://semantify.it/lists/).

* **Verification Results** - With [Verigraph](https://semantify.it/verigraph/) we introduced RDF-conform verification results, in order to be align with SHACL's [Validation Reports](https://www.w3.org/TR/shacl/#validation-report). Verigraph is able to output the verification results in SHACL and in our own format (a JSON-LD version of the JSON we used before). This means that we should write a more detailed specification on how this verification results look like; it should cover the verification of **semantic annotations** as well as the verification of **knowledge graphs**; it should cover all types of verification (**General Ver**. -> Annotation vs. Schema.org | **Compliance Ver**. -> Annotation vs. DS | **Meta Ver.** -> DS vs. Schema.org and DS-Specification).

## Terms

First, we have a List of the new terms introduced. Then a mapping between these terms and the previously mentioned document types that use those terms. The following @context is used for abbreviations:

```json
{
  "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
  "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
  "xsd": "http://www.w3.org/2001/XMLSchema#",
  "owl": "http://www.w3.org/2002/07/owl#",
  "schema": "http://schema.org/",
  "sh": "http://www.w3.org/ns/shacl#",
  "ds": "http://vocab.sti2.at/ds/"
}
```

### New Terms

* **ds:DomainSpecification** - A Class representing a Domain Specification.
* **ds:Vocabulary** - A Class representing an RDF Vocabulary.
* **ds:usedVocabularies** - A property holding the Vocabularies used in a Domain Specification besides schema.org.
* **ds:VerificationReport** - A general Class representing a Verification Report of any kind (General, Compliance, Meta) or target (Annotation, KG). 
* **ds:verificationResult** - A property that holds the overall verification outcome of a ds:VerificationReport as a String. 

todo: Add the other terms related to the verification report.

### Mapping

| Document Type | Used new Term | 
|--------|---------------------------|
| Domain Specification | ds:DomainSpecification |
| | ds:usedVocabularies |
| DS-List | ds:DomainSpecification |
| Vocabulary-List | ds:Vocabulary |
| Verification Result | ds:VerificationReport |
|  | ds:verificationResult |

todo: Add the other terms related to the verification report.
                                                                                                  
## Discussion

As an overview about verification and the ds specc we have following Doc: https://docs.google.com/document/d/1UPVY57CffPQJezwvjCBHL4d3wngI05F0nHFIuBz_FZg/edit#

### General

Labels, how do they work? Should we use language tags everywhere it is possible? e.g. {"@language": "en", "@value":"Meta Error"}

Plural or singular property names?

### ds:DomainSpecification         

Can/should we make ds:DomainSpecification a subclass of sh:NodeShape? The idea would be to substitute the current @type ["sh:NodeShape","schema:CreativeWork"] with "ds:DomainSpecification". We couldn't put the DS document as such in a SHACL validator anymore (But we don't care?). Else we have to use @type ["sh:NodeShape","ds:DomainSpecification"].                                                                  
### ds:VerificationReport

There should be more specific sub-classes to this, for every verification type/target ? That allows to enforce different properties for different verification types (e.g. an @id to a DS is not needed/allowed for a general verification based on the schema.org vocab).
 
### sh:value

We use this property like SHACL, but shouldn't we introduce something like ds:value so we have control over the definition and use of that property?    

original from https://github.com/w3c/data-shapes/blob/gh-pages/shacl/shacl.ttl  has no rangeIncludes:

```
sh:value  a rdf:Property ;                                                  
    rdfs:label "value"@en ;
    rdfs:comment "An RDF node that has caused the result."@en ;          
    rdfs:domain sh:AbstractResult ;
    rdfs:isDefinedBy sh: .
```

### ds:dsPath

 -> @param {string | null} dsPath - string indicating the path within the DS where the error occurred
 
->  @param {[string] | null} dataPath - array pointing to the place where the error occurred

(Paths are arrays to easier travers over them and differentiate between "steps" which may include MTEs
e.g. https://docs.google.com/spreadsheets/d/144iAPlBpjFS4WF1-czwmiIo9IFQNtqH2JPrxiJdKuFM/edit#gid=0 )
 
 
What is "schema:rangeIncludes" in this case? can we say a rdf:List ? since the order matters

note: maybe we change this property with a JSON-Path, ergo a single string is the range

todo: Add the other terms related to the verification report.