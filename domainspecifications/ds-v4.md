# DS-V4

This specification version is the same as DS-V3, but with improvements to the `@context` used for Domain Specifications. Those changes are explained below. Additionally, there is an [Example DS](https://github.com/semantifyit/ds-specifications/tree/e8b181d53fd50820e71e2b30ade2fa0c20d71b85/DomainSpecifications/DS-V4/Examples/DS-Airport.jsonld).

## @Context changes

This context introduces the `@container` keyword with the value `@list`. This results in a syntax for ranges of properties and possible values for enumeration to be easier to read because these containers dont need a `@list`-wrapper anymore:

With previous context:

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:dayOfWeek",
  "sh:order": 1,
  "sh:minCount": 1,
  "sh:or": {
    "@list": [
      {
        "sh:class": "schema:DayOfWeek",
        "sh:in": {
          "@list": [
            "schema:Wednesday",
            "schema:Sunday",
            "schema:PublicHolidays",
            "schema:Tuesday",
            "schema:Monday",
            "schema:Friday",
            "schema:Saturday",
            "schema:Thursday"
          ]
        }
      }
    ]
  }
}
```

with new context:

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:dayOfWeek",
  "sh:order": 1,
  "sh:minCount": 1,
  "sh:or": [
    {
      "sh:class": "schema:DayOfWeek",
      "sh:in": [
        "schema:Wednesday",
        "schema:Sunday",
        "schema:PublicHolidays",
        "schema:Monday",
        "schema:Friday",
        "schema:Tuesday",
        "schema:Saturday",
        "schema:Thursday"
      ]
    }
  ]
}
```

## Improved @context

```javascript
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
      "@id": "sh:property"
    },
    "sh:path": {
      "@id": "sh:path",
      "@type": "@id"
    },
    "sh:datatype": {
      "@id": "sh:datatype",
      "@type": "@id"
    },
    "sh:node": {
      "@id": "sh:node"
    },
    "sh:class": {
      "@id": "sh:class",
      "@type": "@id"
    },
    "sh:or": {
      "@id": "sh:or",
      "@container": "@list"
    },
    "sh:in": {
      "@id": "sh:in",
      "@type": "@id",
      "@container": "@list"
    }
  }
}
```

