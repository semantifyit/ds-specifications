# Domain Specification Node

A DS is a JSON object that has 3 parts: a `@context`, a `@graph`, and a `@id`.

The `@graph` is an Array that has 1 object. That object is seen as the Root Node of the Domain Specification (from a grammar point of view), it contains meta information about the DS, along with the target type(s) for the DS (`sh:targetClass`) and its/their properties (`sh:property`).

The `@id` is an IRI identifying the DS, and in the best case it should also be a valid URL that shows the content of the DS.

A big change to the previous version is the form on how to represent the author and author organisation. This change allows us to stay in strict compliance with the schema.org vocabulary.

Before:
```json
"schema:author": "John Doe",
"schema:authorOrganisation": "John Doe Inc."
```

Now:
```json
"schema:author": {
  "@type": "schema:Person",
  "schema:name": "John Doe",
  "schema:memberOf": {
    "@type": "Organization",
    "schema:name": "John Doe Inc."
  }
}
```

If the DS uses other vocabularies beside schema.org, their namespaces are mentioned in the `@context` and they are explicitly listed with `ds:usedVocabularies` (See [Context.md](./Context.md) for details).

## Key-value Table

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@type` | true | `["sh:NodeShape", "schema:CreativeWork"]` | The fixed type for a Domain Specification |
| `sh:targetClass` | true |*IRI* / List of *IRI* | The IRI(s) of the class(es) that is/are restricted by this Domain Specification | Non-conform target @type |
| `sh:property` | true | List of **PropertyNode** | A list of PropertyNode that apply to the properties of the target Class | Missing Property, Non-conform Property |
| `schema:schemaVersion` | true | *URL* | Used schema.org version as URL, e.g. https://schema.org/version/3.9/|
| `@id` | false | *IRI* | The ID of the root node, which is a blank node. It is recommended to predefine this blank node as "_:RootNode" |
| `schema:author` | false | *Object* | a @Person object, that hold the name of the author and optionally his/her organisation |
| `schema:name` | false | *String* | The name of the Domain Specification |
| `schema:description` | false | *String* | A description about the Domain Specification |
| `schema:version` | false |*Float* | The version of this Domain Specification. Starts at 1.00, small patches increase the decimal by 1 -> 1.01, bigger patches/vocabulary version updates increase the integer by 1 -> 2.00|
| `ds:usedVocabularies` | false |List of *IRI* | The used external vocabularies (besides schema.org) for this DS. The values are IRIs of those vocabularies |

## Example

```JSON
{
    "@id": "_:RootNode",
    "@type": [
        "sh:NodeShape",
        "schema:CreativeWork"
    ],
    "schema:author": {
        "@type": "schema:Person",
        "schema:name": "John Doe",
        "schema:memberOf": {
            "@type": "Organization",
            "schema:name": "John Doe Inc."
        }
    },
    "schema:description": "Example DS for our application",
    "schema:name": "Hotel Specification",
    "schema:schemaVersion": "https://schema.org/version/3.9/",
    "schema:version": 1.02,
    "sh:targetClass": "schema:Hotel",   
    "sh:property": [
        ...PropertyNodes...
    ],
    "ds:usedVocabularies": [
        "http://semantify.it/voc/asdf234ad"
    ]  
}
```
