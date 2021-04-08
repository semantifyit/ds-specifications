# DS-Vocabulary

The DS Vocabulary is an RDF vocabulary that introduces terms that are needed to create RDF documents related to the concept of Domain Specifications, such as **Domain Specifications**, **Lists**, and **Verification Reports**.

The vocabulary file is provided as JSON-LD, but of course it can be translated in any other RDF formats. The terms used to describe this vocabulary are the same used by Schema.org for their vocabulary.

The namespace for the vocabulary is `http://vocab.sti2.at/ds/` and the vocabulary file should be hosted at that URL (also with content negotiation for different RDF formats if possible).

## DS-Vocabulary Versions

* [DS-Vocab-V1](./DS-Vocab-V1/DS-Vocab-V1.md)

## Motivation (WIP)

Why does this file exist.

## Convention (WIP)

Basic rules for the Vocabulary that should be followed in all versions.

### Naming Convention

##### Plural or singular property names?

Thibault - some properties in plural form could be singular: (not sure on that one, schema.org also has some plural properties)   ds:usedVocabularies -> ds:usedVocabulary  ds:errors -> ds:error

Omar - Agree with Thib, we should be more consequent with using singular terms instead of plurals (singular is the standard for RDF, since every piece of data is seen as a triple on its own)

##### rdfs:label -> Reuse the ID as SDO does, or write a real label? e.g. @id = ds:VerificationError, rdfs:label = "VerificationError" or "Verification Error"

Omar - I am for the human readable version. IMHO if you get the label by just removing the "ds:" part before, you don't need an extra property (rdfs:label) for that.
