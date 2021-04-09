# DS-Vocabulary

The [DS Vocabulary](https://github.com/semantifyit/ds-specifications/tree/e8b181d53fd50820e71e2b30ade2fa0c20d71b85/DS-Vocabulary/Input/DS-Vocab-V1/Examples/DS-Vocabulary.jsonld) is an RDF vocabulary that introduces terms that are needed to create RDF documents related to the concept of Domain Specifications, such as **Domain Specifications**, **Lists**, and **Verification Reports**.

The vocabulary file is provided as JSON-LD, but of course it can be translated in any other RDF formats. The terms used to describe this vocabulary are the same used by Schema.org for their vocabulary.

The namespace for the vocabulary is [http://vocab.sti2.at/ds/](http://vocab.sti2.at/ds/) and it should be hosted at that URL \(also with content negotiation for different RDF formats\).

**This vocabulary is still under development.**

## Discussion

As an overview about verification and the ds spec we have following Doc: [https://docs.google.com/document/d/1UPVY57CffPQJezwvjCBHL4d3wngI05F0nHFIuBz\_FZg/edit\#](https://docs.google.com/document/d/1UPVY57CffPQJezwvjCBHL4d3wngI05F0nHFIuBz_FZg/edit#)

### Multi-language

Should we use language tags everywhere it is possible? e.g. {"@language": "en", "@value":"Meta Error"}

Omar - In general a nice idea, but at this stage having a single \(english\) string is fine.

### Naming Convention

#### Plural or singular property names?

Thibault - some properties in plural form could be singular: \(not sure on that one, schema.org also has some plural properties\) ds:usedVocabularies -&gt; ds:usedVocabulary ds:errors -&gt; ds:error

Omar - Agree with Thib, we should be more consequent with using singular terms instead of plurals \(singular is the standard for RDF, since every piece of data is seen as a triple on its own\)

#### rdfs:label -&gt; Reuse the ID as SDO does, or write a real label? e.g. @id = ds:VerificationError, rdfs:label = "VerificationError" or "Verification Error"

Omar - I am for the human readable version. IMHO if you get the label by just removing the "ds:" part before, you don't need an extra property \(rdfs:label\) for that.

### ds:DomainSpecification

Can/should we make ds:DomainSpecification a subclass of sh:NodeShape? The idea would be to substitute the current @type \["sh:NodeShape","schema:CreativeWork"\] with "ds:DomainSpecification". We couldn't put the DS document as such in a SHACL validator anymore \(But we don't care?\). Else we have to use @type \["sh:NodeShape","ds:DomainSpecification"\].

Omar - As we saw with the official test suite of SHACL, our approach is not meant to be "just another" SHACL validator. In that regard the ability of a DS to be used by SHACL validators is not that important.

### ds:VerificationReport

There should be more specific sub-classes to this, for every verification type/target ? That allows to enforce different properties for different verification types \(e.g. an @id to a DS is not needed/allowed for a general verification based on the schema.org vocab\).

### sh:value

We use this property like SHACL, but shouldn't we introduce something like ds:value so we have control over the definition and use of that property?

original from [https://github.com/w3c/data-shapes/blob/gh-pages/shacl/shacl.ttl](https://github.com/w3c/data-shapes/blob/gh-pages/shacl/shacl.ttl) has no rangeIncludes:

```text
 sh:value  a rdf:Property ;                                                  
     rdfs:label "value"@en ;
     rdfs:comment "An RDF node that has caused the result."@en ;          
     rdfs:domain sh:AbstractResult ;
     rdfs:isDefinedBy sh: .
```

### ds:dsPath

-&gt; @param {string \| null} dsPath - string indicating the path within the DS where the error occurred

-&gt; @param {\[string\] \| null} dataPath - array pointing to the place where the error occurred

\(Paths are arrays to easier travers over them and differentiate between "steps" which may include MTEs e.g. [https://docs.google.com/spreadsheets/d/144iAPlBpjFS4WF1-czwmiIo9IFQNtqH2JPrxiJdKuFM/edit\#gid=0](https://docs.google.com/spreadsheets/d/144iAPlBpjFS4WF1-czwmiIo9IFQNtqH2JPrxiJdKuFM/edit#gid=0) \)

What is "schema:rangeIncludes" in this case? can we say a rdf:List ? since the order matters

note: maybe we change this property with a JSON-Path, ergo a single string is the range

todo: Add the other terms related to the verification report.

## Term notes

In the following the terms of the vocabulary are listed and put into context: why they exist and what they do. If they are not final yet a \(WIP\)-Tag is set next to their title.

### ds:DomainSpecification \(WIP\)

This Class represents a Domain Specification. It is a subclass of schema:CreativeWork and of sh:NodeShape \(the old types\). It is used as the @type of DS-documents, and as @type for the items included in DS-List-documents.

### ds:Vocabulary

This Class represents an RDF Vocabulary. It is used as range for "used vocabularies" of Domain Specifications, and as @type for the items included in Vocabulary-List-documents.

### ds:usedVocabulary

This property allows a Domain Specification to state the RDF Vocabulary/ies that are used for the content of the Domain Specification.

