# Domain Specification Node

A Domain Specification is a JSON object that has 2 parts: a `@context` and a `@graph`.

The `@graph` is an array that usually has 1 object. That object is seen as the root node of the Domain Specification (which is called the **DS node**, from a grammar point of view). It contains meta-information about the DS, along with data-matching conditions (`sh:targetClass` or `sh:targetObjectsOf`) and its properties (`sh:property`). It also contains an `@id` with an IRI identifying the DS, and in the best case, it should also be a valid URL that shows the content of the DS.

There is a standard `@context` for Domain Specifications (See [Context.md](./Context.md) for details).
If a DS uses other vocabularies besides schema.org, their namespaces are mentioned in the `@context` and their IRIs (should also work as URL to get the vocabulary file) are explicitly listed with `ds:usedVocabulary`.

## 1. Example of a DS node

```JSON
{
  "@id": "https://semantify.it/ds/OBbzsh4_B",
  "@type": "ds:DomainSpecification",
  "sh:targetClass": [
    "schema:Hotel"
  ],
  "sh:class": [
    "schema:Hotel"
  ],
  "schema:name": [
    {
      "@language": "en",
      "@value": "Hotel Specification"
    }
  ],
  "schema:description": [
    {
      "@language": "en",
      "@value": "Example DS for our application"
    }
  ],
  "schema:author": {
    "@type": "schema:Person",
    "schema:name": "John Doe",
    "schema:memberOf": {
      "@type": "schema:Organisation",
      "schema:name": "Johns Company"
    }
  },
  "ds:version": "7.0",
  "schema:version": 1.02,
  "schema:schemaVersion": "11.0",
  "ds:usedVocabulary": [
    "https://semantify.it/voc/KEl6e6F0U"
  ],
  "sh:closed": false,
  "sh:property": [
    ...
  ]
}
```

## 2. Key-value Table

The following table lists all possible terms that can be used by a DS Node. The order in the table reflects the recommended order of these terms within a DS Node (optional).

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@id` | true | *IRI* | The IRI of the DS. Should be a valid URL to get the DS |
| `@type` | true | `"ds:DomainSpecification"` | The fixed type for a Domain Specification |
| `ds:subDSOf` | false | *IRI* | The Super-DS of this DS. Constraints from the Super-DS are inherited |
| `sh:targetClass` | false | List of *IRI* | The class(es) that the target entities for this Domain Specification must have (for DS matching)| |
| `sh:targetObjectsOf` | false | *IRI* | The property whose entity values are targets for this Domain Specification (for DS matching)  | |
| `sh:class` | false | List of *IRI* | The class(es) that the target entities for this Domain Specification must have (as a constraint for verification) | Non-conform @type |
| `schema:name` | false | List of *Language tagged String* | The name of the Domain Specification |
| `schema:description` | false | List of *Language tagged String* | The description of the Domain Specification |
| `schema:author` | false | *Object* | A `schema:Person` object, that holds the name of the author and optionally their organisation |
| `ds:version` | true | *String* | The DS specification version used. This eases the handling of different DS versions for tools. The range of `ds:version` is a string that specifies the used version number with one decimal place, e.g. if the specification is titled "DS-V7", the value is `"7.0"`|
| `schema:version` | false |*Float* | The version of this Domain Specification. Starts at `1.00`, small patches increase the decimal by 1 -> `1.01`, bigger patches/vocabulary version updates increase the integer by 1 -> `2.00`|
| `schema:schemaVersion` | true | *String* | The used schema.org version as string, e.g. `"11.0"` |
| `ds:usedVocabulary` | false | List of *IRI* | The used external vocabularies (besides schema.org) for this DS. The values are IRIs of those vocabularies |
| `sh:closed` | false | *Boolean* | Specifies if additional properties are allowed or not | Non-conform property |
| `sh:property` | true | List of **PropertyNode** | A list of property nodes that apply to the target entity | Missing Property, Non-conform Property |


## 3. Semantics

### 3.1. Data Matching

There are different possibilities to find a/the matching Domain Specification for a data instance. Keep in mind that the data-matching has no influence on the verification outcome (it does not generate errors), but is only a help to manage the connection between a DS and corresponding data that should be compliant. In the following, the different variations are presented.

#### 3.1.1. sh:targetClass

See [SHACL specification](https://www.w3.org/TR/shacl/#targetClass).

`sh:targetClass` lists the class(es) that a target entity for this DS must match. This term is used for data-matching only (see `sh:class` below for more details). Domain Specifications use [custom semantics for class matching](./../VerificationReport/DS-Verification.md).

Examples:

```JSON
"sh:targetClass": [
  "schema:LodgingBusiness",
  "schema:Restaurant
],
```

```JSON
"sh:targetClass": [
  "schema:Article"
],
```

#### 3.1.2. sh:class

See [SHACL specification](https://www.w3.org/TR/shacl/#ClassConstraintComponent).

A DS node can include `sh:class` specifying the class(es) that the verified entity **MUST** match. The difference to `sh:targetClass` is that if the `sh:class` constraint is not fulfilled a corresponding error is produced during the verification. Domain Specifications use [custom semantics for class matching](./../VerificationReport/DS-Verification.md).

Examples:

```JSON
"sh:class": [
  "schema:LodgingBusiness",
  "schema:Restaurant
]
```

```JSON
"sh:class": [
  "schema:Article"
]
```

#### 3.1.3. sh:targetObjectsOf

See [SHACL specification](https://www.w3.org/TR/shacl/#targetObjectsOf).

`sh:targetObjectsOf` specifies the IRI of a property whose values (objects) are entities that are restricted by this Domain Specification. This property is used for data matching. 

In the following example, any entity that is a range of the property `schema:address` counts as a target for the Domain Specification:

```JSON
"sh:targetObjectsOf": "schema:address"
```

#### 3.1.4. sh:shapesGraph

See [SHACL specification](https://www.w3.org/TR/shacl/#sh-shapes-graph).

`sh:shapesGraph` is a term used for data matching, but is not part of a Domain Specification. Instead, it CAN be used on data entities to specify the Domain Specification(s) to which they comply. The range of the property is the `@id` of the corresponding DS. Multiple `sh:shapesGraph` assertions are treated as a conjunction (an instance MUST fit all DS defined on it).

Example for a JSON-LD annotation:

```json
{
  "@context": {
    "@vocab":"https://schema.org/",
    "sh": "http://www.w3.org/ns/shacl#"
  },
  "@type": "Person",
  "sh:shapesGraph": {
    "@id": "https://semantify.it/ds/-fYx5D34d"
  },
  "name": "Jane Doe",
  "jobTitle": "Professor",
  "telephone": "(123) 123-4567",
  "url": "http://www.janedoe.com"
}
```

### 3.2. Used Vocabularies

#### 3.2.1. schema:schemaVersion

With this term, the used [schema.org vocabulary version](https://schema.org/docs/releases.html) is specified. The used version is now given as a string (only the version number).

Example:

```json
"schema:schemaVersion": "11.01"
```

#### 3.2.2. ds:usedVocabulary

The term `ds:usedVocabulary` lists vocabularies (besides schema.org) that are being used for the content of the DS. Vocabularies are identified by their `@id`. Keep in mind that external vocabularies could introduce vocabulary namespaces to the `@context`.

Example:

```json
"ds:usedVocabulary": [
  "https://semantify.it/voc/KEl6e6F0U"
]
```

### 3.3. Properties

#### 3.3.1. sh:property

See [SHACL specification](https://www.w3.org/TR/shacl/#PropertyConstraintComponent).

The mandatory term `sh:property` lists the [Property Nodes](./Property.md) that the target entity must comply with.

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

#### 3.3.2. sh:closed

See [SHACL specification](https://www.w3.org/TR/shacl/#ClosedConstraintComponent).

The term `sh:closed` can be used to specify if additional properties (other than the properties allowed by `sh:property`) are allowed or not. In the past, Domain Specifications had `"sh:closed": true` implicitly. Now the DS creator should specify the wished behaviour.

Example:

```json
"sh:closed": true
```

### 3.4. Other metadata

#### 3.4.1. ds:version

The term `ds:version` is used to **specify the DS Specification version** that was used to create the Domain Specification. This information is important for software parsing Domain Specifications to handle different versions of DS (expected terms, grammar, etc.).

The range of `ds:version` is a string that specifies the used version number with one decimal place, e.g. if the specification is titled "DS-V7", the `ds:version` is `"7.0"`. The decimal place allows the release of minor upgrades to the specification (if wished in the future).

Example:

```json
"ds:version": "7.0"
```

#### 3.4.2. schema:version

The term `schema:version` specifies the version of the DS itself. The value is a float number that should be increased every time the content of the DS is changed. The value starts at `1.00`, small patches increase the decimal place, e.g. `1.00` -> `1.01`, bigger patches/vocabulary version updates increase the integer place, e.g. `1.00` -> `2.00`.

Example:

```json
"schema:version": "1.46"
```

#### 3.4.3. schema:name

The name of the Domain Specification. The value(s) must be a language-tagged string.

Example:

```json
"schema:name": [
  {
    "@language": "en",
    "@value": "My example DS"
  },
  {
    "@language": "es",
    "@value": "Mi ejemplo DS"
  }
]
```

#### 3.4.4. schema:description

The description of the Domain Specification. The value(s) must be a language-tagged string.

Example:

```json
"schema:description": [
  {
    "@language": "en",
    "@value": "Example DS for our application."
  },
  {
    "@language": "es",
    "@value": "Ejemplo DS para nuestra aplicacion."
  }
]
```

#### 3.4.5. schema:author

The author of the Domain Specification. The value must be a `schema:Person` that should have at least a `schema:name` property. On semantify.it the author always has a `schema:memberOf` property with a `schema:Organisation` as value.

Example:

```json
"schema:author": [
  "@type": "schema:Person",
  "schema:name": "John Doe",
  "schema:memberOf": {
    "@type": "schema:Organisation",
    "schema:name": "Johns Company"
  }
]
```

### 3.5. DS hierarchy

#### 3.5.1. ds:subDSOf

The term `ds:subDSOf` specifies that the Domain Specification is a Sub-DS of the referenced DS (by its `@id`). A Sub-DS inherits all the constraints from its Super-DS. A Domain Specification can have only one Super-DS but could have multiple Sub-DS.

Example:

```json
{
  "@type": "ds:DomainSpecification",
  "sh:targetClass": ["schema:Hotel"],
  "sh:class": ["schema:Hotel"],
  "ds:subDSOf": "https://semantify.it/ds/fBhz5h78s",
  ...
}
```

**Semantics**

* The constraints defined in a Sub-DS MUST be as restrictive as the Super-DS or more restrictive. In order to be in compliance with a DS that has a Super-DS, a data instance MUST also be compliant to that Super-DS (instances that fit a Sub-DS MUST be a subset of those that fit its Super-DS).
* A Sub-DS can introduce new constraints. This is also possible for already defined constraints, but they MUST be as restrictive as the Super-DS or more restrictive. This is a delicate challenge, e.g.
  * Adding a new PropertyShape makes a Sub-DS more restrictive.
  * Adding a new range to an inherited PropertyShape makes a Sub-DS less restrictive.
  * Adding a cardinality constraint (e.g. `sh:maxCount`) to an inherited PropertyShape makes a Sub-DS more restrictive.
  * Increasing the value of an inherited `sh:maxCount` constraint makes a Sub-DS less restrictive.
  * Increasing the value of an inherited `sh:minCount` constraint makes a Sub-DS more restrictive.
  * The `sh:targetClass` of a Sub-DS MUST be the same or a sub-class of the `sh:targetClass` of its Super-DS.
  * Adding additional target classes (MTE) would make a Sub-DS invalid (due to our semantics of `sh:targetClass`).

**Implications for the implementation**

The most challenging part of this new term is to ensure the consistency between hierarchical DS when creating/editing/deleting Domain Specifications. Tools must aid users to not take actions that result in an invalid `ds:subDSOf` definition.

Tools presenting Domain Specifications should show/link the Super-DS in a prominent way, and/or show the total resulting constraints of a DS and its Super-DS (and recursively their Super-DS). For the verification the total resulting constraints are important. All these tools that read a DS assume that the `ds:subDSOf` link is valid (that the target DS exists and that the semantic rules are followed).