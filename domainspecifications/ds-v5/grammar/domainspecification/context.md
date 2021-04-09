# @Context

DS-V5 uses almost the same **@context** as the previous version DS-V4 with some additions and changes.

## Additions and changes

Additions to the **@context** are:

* sh:languageIn
* sh:equals
* sh:disjoint
* sh:lessThan
* sh:lessThanOrEquals
* ds \(this is a namespace/vocabulary for DS related specifications, which is also part of DS-V5\)
* ds:usedVocabularies

Since we want to use `sh:in` to constraints the possible values for literals too \(we used it only for enumeration values before\), we have to remove the corresponding `"@type": "@id"` from the @context. This means, that enumeration values are now wrapped with `@id`:

**in DS-V4**

```javascript
@context:
...
"sh:in": {
    "@id": "sh:in",
    "@type": "@id",
    "@container": "@list"
}
...
@graph:
...
{
    "sh:class": "schema:DayOfWeek",
    "sh:in": [
        "schema:Wednesday",
        "schema:Saturday",
        "schema:Thursday"
    ]
}
...
```

**in DS-V5**

```javascript
@context:
...
"sh:in": {
    "@id": "sh:in",
    "@container": "@list"
}
...
@graph:
...
{
    "sh:class": "schema:DayOfWeek",
    "sh:in": [
        {
            "@id": "schema:Wednesday"
        },
        {
           "@id": "schema:Saturday"
        },
        {
           "@id": "schema:Thursday"
        }
    ]
}
...
{
    "sh:path": "schema:City",
    "sh:or": [
        {
            "sh:datatype": "xsd:string",
            "sh:in": [
                "Innsbruck",
                "Mayrhofen",
                "Seefeld"
            ]
        }
    ]
}
```

To enable a listing of allowed language tags without having a wrapping `@list`, we need to add the following to the context:

```javascript
"sh:languageIn": {
    "@id": "sh:languageIn",
    "@container":"@list"
}
```

Value for property pair constraints have always an URI as values, so it wise to add that type in the context:

```javascript
"sh:equals": {
    "@id": "sh:equals",
    "@type": "@id"
}
```

## @context for DS-V5

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
      "@container": "@list"
    },
    "sh:languageIn": {
       "@id": "sh:languageIn",
       "@container":"@list"
    },
    "sh:equals": {
       "@id": "sh:equals",
       "@type": "@id"
    },
    "sh:disjoint": {
       "@id": "sh:disjoint",
       "@type": "@id"
    },
    "sh:lessThan": {
       "@id": "sh:lessThan",
       "@type": "@id"
    },
    "sh:lessThanOrEquals": {
       "@id": "sh:lessThanOrEquals",
       "@type": "@id"
    },
    "ds": "http://vocab.sti2.at/ds/",
    "ds:usedVocabularies": {
      "@id": "ds:usedVocabularies",
      "@type": "@id"
    }
  }
}
```

