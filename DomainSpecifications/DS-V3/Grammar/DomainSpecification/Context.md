# @Context

To simplify the JSON-LD structure we make clever use of the **@context** possibilities. Reusing the same context on all interfaces makes it easier for us to understand/reuse code that interacts with domain specifications (e.g. using the JSON-LD npm package).

```json
{
  "@context": {
    "rdf": "http://www.w3.org/1999/02/22-rdf-syntax-ns#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "sh": "http://www.w3.org/ns/shacl#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "schema": "http://schema.org/",
    "sh:targetClass": {
      "@id": "sh:targetClass",
      "@type": "@id"
    },
    "sh:property": {
      "@id": "sh:property",
      "@type": "@id"
    },
    "sh:path": {
      "@id": "sh:path",
      "@type": "@id"
    },
    "sh:nodeKind": {
      "@id": "sh:nodeKind",
      "@type": "@id"
    },
    "sh:datatype": {
      "@id": "sh:datatype",
      "@type": "@id"
    },
    "sh:node": {
      "@id": "sh:node",
      "@type": "@id"
    },
    "sh:class": {
      "@id": "sh:class",
      "@type": "@id"
    },
    "sh:or": {
      "@id": "sh:or",
      "@type": "@id"
    },
    "sh:in": {
      "@id": "sh:in",
      "@type": "@id"
    }
  }
}
```