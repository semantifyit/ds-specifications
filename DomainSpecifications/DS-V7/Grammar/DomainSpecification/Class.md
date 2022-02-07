# Class Node

A Class Node is used as a potential range for a [Property Node](./Property.md).

It is represented by an object with `"@type": "sh:NodeShape"` that is wrapped by the term `sh:node`. This NodeShape is used to express all the characteristics and constraints of the Class Node. The most important constraint is `sh:class`, which constraints the class(es) that the target entity must have. For `sh:class` arrays are allowed, which are supposed to represent multi-typed entities (MTE).

The `@id` of a Class Node is used to reference it in other parts of the Domain Specification, and so reuse already defined constraints for a specific Class. Details below. 

## 1. Example

```json
{
  "sh:node": {
    "@id": "https://semantify.it/ds/OBbzsh4_B#DpruH",
    "@type": "sh:NodeShape",
    "sh:class": [
      "schema:Airport"
    ],
    "rdfs:label": [
      {
        "@language": "en",
        "@value": "Airport"
      },
      {
        "@language": "de",
        "@value": "Flughafen"
      }
    ],
    "rdfs:comment": [
      {
        "@language": "en",
        "@value": "An international Airport that is identified by its ICAO identifier."
      }
    ],
    "sh:closed": true,
    "ds:propertyDisplayOrder": [
      "schema:name",
      "schema:icaoCode",
      "schema:url"
    ],
    "sh:property": [
      ...
    ]
  }
}
```

## 2. Key-value Table

The following table lists all possible terms that can be used by a Class Node. The order in the table reflects the recommended order of these terms within a Class Node (optional).

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@id` | true | *IRI* | The IRI of the Class Node, which is based on the DS IRI it is in |
| `@type` | true | `"sh:NodeShape"` | The fixed type for a Class Node |
| `sh:class` | true | [ *IRI* ] | The IRI(s) of the Class(es) that the entity must have | Non-conform range |
| `rdfs:label` | false | List of *Language tagged String* |  The label for this Class |
| `rdfs:comment` | false | List of *Language tagged String* |  The description for this Class |
| `sh:closed` | false | *Boolean* | Specifies if additional properties are allowed or not | Non-conform property |
| `ds:propertyDisplayOrder` | false | List of **IRI** | A list of property IRIs that reflect the order of the properties for this Class. If this property is used, it replaces the order given by `sh:order` of the property nodes in question. | |
| `sh:property` | false | List of **PropertyNode** | A list of property nodes that apply to the entity | Missing Property, Non-conform Property |

## 3. Semantics

### 3.1. Class-Matching

See [SHACL specification](https://www.w3.org/TR/shacl/#ClassConstraintComponent).

A Class Node includes `sh:class` to specify the class(es) that the verified entity **MUST** match. Domain Specifications use [custom semantics for class matching](./../VerificationReport/DS-Verification.md).

Examples:

```JSON
"sh:class": [
  "schema:LodgingBusiness",
  "schema:Restaurant"
]
```

```JSON
"sh:class": [
  "schema:Article"
]
```

### 3.2. Properties

The terms `sh:property` and `sh:closed` are used in a Class Node to give further restrictions on the properties of the corresponding entity. Such a Class Node with property restrictions is also called **Restricted Class Node**. A Class Node without further property restrictions is called a **Standard Class Node**.

#### 3.2.1. sh:property

See [SHACL specification](https://www.w3.org/TR/shacl/#PropertyConstraintComponent).

The term `sh:property` lists the property shapes that the target entity must comply with. For every property, there is a corresponding [Property Node](./Property.md) in this list.

Example:

```json
"sh:property": [
  {
    "@type": "sh:PropertyShape",
    "sh:order": 0,
    "sh:path": "schema:identifier"
    "sh:maxCount": 1,
    "sh:or": [
      {
        "sh:datatype": "xsd:string",
      }
    ]
  },
  {
    "@type": "sh:PropertyShape",
    "sh:order": 1,
    "sh:path": "schema:alternateName"
    "sh:maxCount": 1,
    "sh:or": [
      {
       "sh:datatype": "xsd:string",
      }
    ]
  }
]
```

#### 3.2.2. sh:closed

See [SHACL specification](https://www.w3.org/TR/shacl/#ClosedConstraintComponent).

The term `sh:closed` can be used to specify if additional properties (other than the properties allowed by `sh:property`) are allowed or not. In the past, Domain Specifications had `"sh:closed": true` implicitly in all NodeShapes. Now the DS creator should specify the wished behaviour.

Example:

```json
"sh:closed": true
```

#### 3.2.3. ds:propertyDisplayOrder

The term `ds:propertyDisplayOrder` provides a list of property IRIs that reflect a wished order of the properties for display purposes. Details about this term can be found in [DomainSpecification.md](./DomainSpecification.md).

### 3.3. Metadata

Following terms represent metadata about the given class. These terms do not have any effects on the verification result; They have only informational character.

#### 3.3.1. rdfs:label

The term `rdfs:label` CAN be used to give the class a label (in different languages). The value for this term is a language-tagged string. The standard label for a class term is usually provided by its vocabulary itself, `rdfs:label` can be used to overwrite that standard label or provide a single label for multi-class entities (a class that has multiple `sh:class` entries).

Example:

```JSON
{
  "@id": "https://semantify.it/ds/OBbzsh4_B#DpruH",
  "@type": "sh:NodeShape",
  "sh:class": [
    "schema:LodgingBusiness",
    "schema:Restaurant"
  ],
  "rdfs:label": [
    {
      "@language": "en",
      "@value":"Hotel and Restaurant"
    }
  ],
  ...
}
```

#### 3.3.2. rdfs:comment

The term `rdfs:comment` CAN be used to describe the class (in different languages). The value for this term is a language-tagged string. The standard description for a class term is usually provided by its vocabulary itself, `rdfs:comment` can be used to overwrite that standard description or provide a single description for multi-class entities (a class that has multiple `sh:class` entries).

Example:

```JSON
{
  "@id": "https://semantify.it/ds/OBbzsh4_B#DpruH",
  "@type": "sh:NodeShape",
  "sh:class": [
    "schema:LodgingBusiness",
    "schema:Restaurant"
  ],
  "rdfs:comment": [
    {
      "@language": "en",
      "@value":"A business that offers rooms and has a restaurant in the same building."
    }
  ],
  ...
}
```