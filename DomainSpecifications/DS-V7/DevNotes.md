# Developer Notes

The following points are not part of the DS specification, but implementation guidelines to ease the life of developers and readers of the raw JSON-LD format. These are nice-to-have features, but not mandatory.

## 1. Use of arrays for convenience

Some terms expect an RDF-list as value, those terms have an entry in the `@context` that indicates that:

```json
"sh:or": {
  "@container": "@list"
}
```

These terms will always have an array as a value, even if that array has only one value (also after a JSON-LD transformation):

```json
"sh:or": [
  {
    "sh:datatype": "xsd:string"
  }
]
```

This approach is convenient since the structure (having an array) stays the same, which is easier to read and code with. We have other terms where this behaviour is wished (most terms that CAN have an array of values), but it does not come out of the box, which means that after a JSON-LD transformation the value for these terms must be wrapped in an array if that is not yet the case. This "convenient" version of a JSON-LD is the one that is saved in the semantify.it MongoDB and shown in [SHACL format](https://semantify.it/ds/_1hRVOT8Q?format=shacl). We do this already with some terms, like `sh:property` and `ds:usedVocabulary`. Further, if there are no values in the array, the term should not be used at all. 

See the following list of example terms that should have this behaviour (any term that CAN have multiple values should have such an array):

* sh:targetClass
* sh:class
* ds:usedVocabulary
* sh:property
* schema:name
* schema:description

All terms that have an RDF-list as value (see `@context`) are also expected to always have an array as value.

## 2. Order of terms

In JSON-LD the order of properties is not important (in theory). In practice having an order increases the readability for humans of such documents. We could further improve the "convenient" version of a JSON-LD so that there is at least some ordering for the important terms, e.g. `sh:targetClass` is one of the most important terms for the DS node, but it is currently found only at the end of the document. The reordering of terms can go hand in hand with the previous step of array normalization before a DS is saved/updated in MongoDB.

In general, we want to have the terms sorted by importance and alphabetical order. Terms that have recursive content should come at the end (e.g. `sh:property`).

Priority List:

1. Terms starting with @ (e.g. `@id`, `@type`)
   
2. Terms indicating the actual path (e.g. `sh:targetClass`, `sh:class`, `sh:datatype`, `sh:path`)
   
3. Terms for metadata (e.g. `schema:name`, `sh:order`, `rdfs:comment`)
   
4. Terms for constraints (e.g. `sh:minCount`, `sh:minLength`)
   
5. Terms that have recursive content (e.g. `sh:property`, `sh:or`, `sh:node`)

## 3. Generating IDs for inner NodeShape

IRIs for inner Nodes of a DS need an ID that goes after the # part of the IRI. There should not be many IRIs needed within a DS, therefore we can choose a small length and a small alphabet set for these IDs.

* The npm library [nanoid](https://www.npmjs.com/package/nanoid) helps us to achieve this ID generation.
* Collision probability can be tested with the [Nano ID Collision Calculator](https://zelark.github.io/nano-id-cc/)
* Proposed length: `5`
* Proposed alphabet: `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`

Example Class node (for DS with `"@id": "https://semantify.it/ds/OBbzsh4_B"`):

```json
{
  "sh:node": {
    "@id": "https://semantify.it/ds/OBbzsh4_B#DjToD",
    "@type": "sh:NodeShape",
    "sh:class": ["schema:PostalAddress"],
    "sh:property": [
      ...
    ]
  }
}
```

## 4. Recommended Libraries

The use of the following libraries can ease your life as a developer when you work on Domain Specifications.

* [SDO Adapter](https://www.npmjs.com/package/schema-org-adapter) - A library created by semantify.it to handle vocabulary data (like the schema.org vocabulary). This library offers an API that abstracts the whole vocabulary handling and reasoning.
* [JSON-LD](https://www.npmjs.com/package/jsonld) - This library is an implementation of the [JSON-LD specification](https://json-ld.org/spec/latest/) in JavaScript.
* [Nano Id](https://www.npmjs.com/package/nanoid) - Helps to create random IDs for IRIs.