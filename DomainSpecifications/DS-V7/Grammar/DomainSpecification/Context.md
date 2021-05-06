# Standard @Context

In the past, we have tried to use a standard `@context` for Domain Specifications that would work in all tools and use-cases. In **DS-V7** a minimal `@context` is provided, which (1) omits the explicit `@id` for specified terms and (2) omits the terms `sh:property` and `sh:node`. If a tool needs further changes to the @context, they should be done in a pre-processing step for that tool (most tools only have to 'read' Domain Specifications).

Additionally, some new term entries are introduced. Keep in mind that external vocabularies could introduce vocabulary namespaces to the `@context`.

```json
{
  "@context": {
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "schema": "https://schema.org/",
    "sh": "http://www.w3.org/ns/shacl#",
    "ds": "http://vocab.sti2.at/ds/",
    "sh:targetClass": {
      "@type": "@id"
    },
    "sh:targetObjectsOf": {
      "@type": "@id"
    },
    "sh:path": {
      "@type": "@id"
    },
    "sh:datatype": {
      "@type": "@id"
    },
    "sh:class": {
      "@type": "@id"
    },
    "sh:or": {
      "@container": "@list"
    },
    "sh:in": {
      "@container": "@list"
    },
    "sh:languageIn": {
       "@container":"@list"
    },
    "sh:equals": {
       "@type": "@id"
    },
    "sh:disjoint": {
       "@type": "@id"
    },
    "sh:lessThan": {
       "@type": "@id"
    },
    "sh:lessThanOrEquals": {
       "@type": "@id"
    },
    "ds:usedVocabulary": {
      "@type": "@id"
    },
    "ds:subDSOf": {
      "@type": "@id"
    }
  }
}
```
