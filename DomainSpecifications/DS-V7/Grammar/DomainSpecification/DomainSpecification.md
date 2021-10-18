# Domain Specification Node

A Domain Specification is a JSON object that has 2 parts: a `@context` and a `@graph`.

The `@graph` is an array that usually has 1 object. That object is seen as the root node of the Domain Specification (
which is called the **DS node**, from a grammar point of view). It contains meta-information about the DS, along with
data-matching conditions (e.g. `sh:targetClass`, `sh:targetObjectsOf`, etc.) and its properties (`sh:property`). It also
contains an `@id` with an IRI identifying the DS, and in the best case, it should also be a valid URL that shows the
content of the DS.

There is a standard `@context` for Domain Specifications (See [Context.md](./Context.md) for details). If a DS uses
other vocabularies besides schema.org, their namespaces are mentioned in the `@context` and their IRIs (should also work
as URL to get the vocabulary file) are explicitly listed with `ds:usedVocabulary`.

## 1. Example

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
  "schema:version": "1.02",
  "schema:schemaVersion": "11.0",
  "ds:usedVocabulary": [
    "https://semantify.it/voc/KEl6e6F0U"
  ],
  "sh:closed": false,
  "ds:propertyDisplayOrder": [
    "schema:name",
    "schema:description",
    "schema:address",
    "schema:url",
    "schema:image"
  ],
  "sh:property": [
    ...
  ]
}
```

## 2. Key-value Table

The following table lists all possible terms that can be used by a DS Node. The order in the table reflects the
recommended order of these terms within a DS Node (optional).

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@id` | true | *IRI* | The IRI of the DS. Should be a valid URL to get the DS |
| `@type` | true | `"ds:DomainSpecification"` | The fixed type for a Domain Specification |
| `ds:subDSOf` | false | *IRI* | The Super-DS of this DS. Constraints from the Super-DS are inherited |
| `sh:targetClass` | false | List of *IRI* | The class(es) that the target entities for this Domain Specification must have (for DS matching)| |
| `sh:targetObjectsOf` | false | *IRI* | The property whose object entities are targets for this Domain Specification (for DS matching)  | |
| `sh:targetSubjectsOf` | false | *IRI* | The property whose subject entities are targets for this Domain Specification (for DS matching)  | |
| `sh:class` | false | List of *IRI* | The class(es) that the target entities for this Domain Specification must have (as a constraint for verification) | Non-conform @type |
| `schema:name` | false | List of *Language tagged String* | The name of the Domain Specification |
| `schema:description` | false | List of *Language tagged String* | The description of the Domain Specification |
| `schema:author` | false | *Object* | A `schema:Person` object, that holds the name of the author and optionally their organisation |
| `ds:version` | true | *String* | The DS specification version used. This eases the handling of different DS versions for tools. The range of `ds:version` is a string that specifies the used version number with one decimal place, e.g. if the specification is titled "DS-V7", the value is `"7.0"`|
| `schema:version` | false |*String* | The version of this Domain Specification instance. |
| `schema:schemaVersion` | true | *String* | The used schema.org version as string, e.g. `"11.0"` |
| `ds:usedVocabulary` | false | List of *IRI* | The used external vocabularies (besides schema.org) for this DS. The values are IRIs of those vocabularies |
| `sh:closed` | false | *Boolean* | Specifies if additional properties are allowed or not | Non-conform property |
| `ds:propertyDisplayOrder` | false | List of **IRI** | A list of property IRIs that reflect the order of the properties for this DS, including those inherited from its Super-DS. If this property is used, it replaces the order given by `sh:order` of the property nodes in question. | |
| `sh:property` | true | List of **PropertyNode** | A list of property nodes that apply to the target entity | Missing Property, Non-conform Property |

## 3. Semantics

### 3.1. Data Matching

There are different possibilities to find a/the matching Domain Specification for a data instance. Keep in mind that the
data-matching has no influence on the verification outcome (it does not generate errors), but is only a help to manage
the connection between a DS and corresponding data that should be compliant. In the following, the different variations
are presented.

#### 3.1.1. sh:targetClass

See [SHACL specification](https://www.w3.org/TR/shacl/#targetClass).

`sh:targetClass` lists the class(es) that a target entity for this DS must match. This term is used for data-matching
only (see `sh:class` below for more details). Domain Specifications
use [custom semantics for class matching](./../VerificationReport/DS-Verification.md).

Examples:

```JSON
"sh:targetClass": [
"schema:LodgingBusiness",
"schema:Restaurant"
],
```

```JSON
"sh:targetClass": [
"schema:Article"
],
```

#### 3.1.2. sh:class

See [SHACL specification](https://www.w3.org/TR/shacl/#ClassConstraintComponent).

A DS node can include `sh:class` specifying the class(es) that the verified entity **SHOULD** match. The difference
to `sh:targetClass` is that if the `sh:class` constraint is not fulfilled a corresponding error is produced during the
verification. Domain Specifications
use [custom semantics for class matching](./../VerificationReport/DS-Verification.md).

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

#### 3.1.3. sh:targetObjectsOf

See [SHACL specification](https://www.w3.org/TR/shacl/#targetObjectsOf).

`sh:targetObjectsOf` specifies the IRI of a property whose values (objects) are entities that are restricted by this
Domain Specification. This property is used for data matching.

In the following example, any entity that is a range of the property `schema:address` counts as a target for the Domain
Specification:

```JSON
"sh:targetObjectsOf": "schema:address"
```

#### 3.1.4. sh:targetSubjectsOf

See [SHACL specification](https://www.w3.org/TR/shacl/#targetSubjectsOf).

`sh:targetSubjectsOf` specifies the IRI of a property whose subjects are entities that are restricted by this Domain
Specification. This property is used for data matching.

In the following example, any entity that uses the property `schema:address` counts as a target for the Domain
Specification:

```JSON
"sh:targetSubjectsOf": "schema:address"
```

#### 3.1.5. ds:compliesWith

`ds:compliesWith` is a term used for data matching, but is not part of a Domain Specification. Instead, it CAN be used
on data entities to specify the Domain Specification(s) to which they comply. The range of the property is the `@id` of
the corresponding DS. Multiple `ds:compliesWith` assertions are treated as a conjunction (an instance MUST fit all DS
defined on it).

Example for a JSON-LD annotation:

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "ds": "https://vocab.sti2.at/ds/"
  },
  "@type": "Person",
  "ds:compliesWith": {
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

With this term, the used [schema.org vocabulary version](https://schema.org/docs/releases.html) is specified. The used
version is now given as a string (only the version number).

Example:

```json
"schema:schemaVersion": "11.01"
```

#### 3.2.2. ds:usedVocabulary

The term `ds:usedVocabulary` lists vocabularies (besides schema.org) that are being used for the content of the DS.
Vocabularies are identified by their `@id`. Keep in mind that external vocabularies could introduce vocabulary
namespaces to the `@context`.

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

The term `sh:closed` can be used to specify if additional properties (other than the properties allowed
by `sh:property`) are allowed or not. In the past, Domain Specifications had `"sh:closed": true` implicitly. Now the DS
creator should specify the wished behaviour.

Example:

```json
"sh:closed": true
```

### 3.4. Other metadata

#### 3.4.1. ds:version

The term `ds:version` is used to **specify the DS Specification version** that was used to create the Domain
Specification. This information is important for software parsing Domain Specifications to handle different versions of
DS (expected terms, grammar, etc.).

The range of `ds:version` is a string that specifies the used version number with one decimal place, e.g. if the
specification is titled "DS-V7", the `ds:version` is `"7.0"`. The decimal place allows the release of minor upgrades to
the specification (if wished in the future).

Example:

```json
"ds:version": "7.0"
```

#### 3.4.2. schema:version

The term `schema:version` specifies the version of the DS itself. The value type for this term is a string. In
semantify.it this string represents a float number that should be increased every time the content of the DS is changed.
The value starts at `1.00`, small patches increase the decimal place, e.g. `1.00` -> `1.01`, bigger patches/vocabulary
version updates increase the integer place, e.g. `1.00` -> `2.00`.

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

The author of the Domain Specification. The value must be a `schema:Person` that should have at least a `schema:name`
property. On semantify.it the author always has a `schema:memberOf` property with a `schema:Organisation` as value.

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

The term `ds:subDSOf` specifies that the Domain Specification is a Sub-DS of the referenced DS (by its `@id` - the range
type is fixed in the standard `@context`). A Sub-DS inherits all the constraints from its Super-DS. A Domain
Specification can have only one Super-DS, but could have multiple Sub-DS.

Example:

```json
{
  "@type": "ds:DomainSpecification",
  "ds:subDSOf": "https://semantify.it/ds/fBhz5h78s",
  "sh:targetClass": [
    "schema:Hotel"
  ],
  "sh:class": [
    "schema:Hotel"
  ],
  ...
}
```

**Semantics**

* The constraints defined in a Sub-DS MUST be as restrictive as the Super-DS or more restrictive. In order to be in
  compliance with a DS that has a Super-DS, a data instance MUST also be compliant to that Super-DS (instances that fit
  a Sub-DS MUST be a subset of those that fit its Super-DS).
* A Sub-DS can introduce new constraints. This is also possible for already defined constraints, but they MUST be as
  restrictive as the Super-DS or more restrictive. This is a delicate challenge, e.g.
  * Adding a new PropertyShape makes a Sub-DS more restrictive, but the Super-DS must NOT have `sh:closed true`, in
    order to allow the Sub-DS to introduce new properties.
  * Adding a new range to an inherited PropertyShape makes a Sub-DS less restrictive.
  * Adding a cardinality constraint (e.g. `sh:maxCount`) to an inherited PropertyShape makes a Sub-DS more restrictive.
  * Increasing the value of an inherited `sh:maxCount` constraint makes a Sub-DS less restrictive.
  * Increasing the value of an inherited `sh:minCount` constraint makes a Sub-DS more restrictive.
  * The `sh:targetClass` of a Sub-DS MUST be the same or a sub-class of the `sh:targetClass` of its Super-DS.
  * Adding additional target classes (MTE) would make a Sub-DS more restrictive.

**Implications for the implementation**

The most challenging part of this new term is to ensure the consistency between hierarchical DS when
creating/editing/deleting Domain Specifications. Tools must aid users to not take actions that result in an
invalid `ds:subDSOf` definition.

Tools presenting Domain Specifications should show/link the Super-DS in a prominent way, and/or show the total resulting
constraints of a DS and its Super-DS (and recursively their Super-DS). For the verification the total resulting
constraints are important. All these tools that read a DS assume that the `ds:subDSOf` link is valid (that the target DS
exists and that the semantic rules are followed).

#### 3.5.1.1. ds:propertyDisplayOrder

`ds:propertyDisplayOrder` is used in the root node of a DS to provide a list of property IRIs that reflect the order of the properties for this DS, including those inherited from its Super-DS (this is important to display the populated version of the DS). If this property is used, it replaces the order given by `sh:order` (which is deprecated now) of the property nodes in question. Both terms can coexist (but it is recommended to use `ds:propertyDisplayOrder` instead of `sh:order`), e.g. `sh:order` is taken for the property order for the unpopulated version, and `ds:propertyDisplayOrder` is taken for the populated version.

To be correct and complete, the list in `ds:propertyDisplayOrder` must contain all properties of the Sub-DS and all properties inherited from the Super-DS. It is up to the implementation to handle invalid lists, but as a recommendation: If there are missing or additional properties, those can be skipped or rather displayed at the end of the list.

For convenience, following entry is included in the standard `@context`:

```json
"ds:propertyDisplayOrder": {
  "@container": "@list",
  "@type": "@id"
}
```

The following example contains 4 property IRIs defining their order when resolving the population of the DS with its Super-DS (defined with `ds:subDSOf`). Imagine that the Super-DS provides the properties `schema:address` and `schema:name`, and the DS provides the properties `schema:description` and `schema:location`.

```json
{
  "@type": "ds:DomainSpecification",
  "ds:subDSOf": "https://semantify.it/ds/fBhz5h78s",
  ...
  "ds:propertyDisplayOrder": [
    "schema:name",
    "schema:description",
    "schema:address",
    "schema:location"
  ],
  ...
}
```

#### 3.5.2. Internal and external references

It is possible to reuse NodeShapes by linking to them (references). For the range of a property it is possible to link
to a NodeShape inside the Domain Specification (internal reference), but also to link to another Domain
Specifications (external references). In the latter case the external domain specification is understood as a Node
Shape, with its class and property restrictions. In both cases the reference is established by using only the `@id` of
the referenced NodeShape / Domain Specification in the `sh:node` object (see examples below).

The intention of this feature is to allow the reuse of an already defined property range (e.g. the same range definition is used for multiple properties), allow circular constructs (e.g. a Domain Specification can reference itself as a range of its properties), and manage Class-based definitions at one place (e.g. having a standard Domain Specification for `schema:PostalAddress` that is referenced, and therefore always up to date, in multiple other Domain Specifications).

##### 3.5.2.1 Internal references

An internal reference links to an already existing NodeShape within the Domain Specification. In order to make the use of internal references more convenient, we introduce the following rules:

* Every NodeShape inside a DS receives an IRI, even if it hasn't been referenced yet.
* The referenced NodeShape contains `"@type": "sh:NodeShape"` and all the constraints needed.
* The internal references contain only the `@id` entry, pointing to the referenced NodeShape. They can not add additional constraints.
* NodeShapes that are referenced are "relocated" into the `@graph` array of the Domain Specification, so that they are easy to find and to identify as internal references.
* Only valid matches can be used as a reference for a range (from an ontology point of view). For this, the `sh:class` constraint of the target NodeShape is checked. If the target class of a NodeShape is `schema:Hotel`, but the property in question can not have this class as a valid range, then that NodeShape can **not** be referenced.
* It is possible to create circular Domain Specifications (the referenced NodeShape is the DomainSpecification root node).
* It is possible to create circular NodeShapes (the NodeShape references itself as a range for one of its properties).

Example:

The first node in the `@graph` array is the root node of the Domain Specification. The second node in the `@graph` array is
an internally referenced node. It is referenced as the range for the `schema:address` Property Shape (only the `@id` of the referenced node must be given).
Note that the `@id` of NodeShapes within a Domain Specification have the same BaseIRI as the DS in which they are in, with the addition of a **fragment id**.

```json
"@graph": [
  {
    "@id": "https://semantify.it/ds/rsFn_FabM",
    "@type": "ds:DomainSpecification",
    ...
    "sh:property": [
      {
        "@type": "sh:PropertyShape",
        "sh:path": "schema:address",
        ...
        "sh:or": [
          {
            "sh:node": {
              "@id": "https://semantify.it/ds/rsFn_FabM#1D5bj"
            }
          }
        ]
      }
    ]
  },
  {
    "@id": "https://semantify.it/ds/rsFn_FabM#1D5bj",
    "@type": "sh:NodeShape",
    "sh:class": [
      "schema:PostalAddress"
    ],
    ...
  }
]
```

##### 3.5.2.2 External references

An external reference links to another Domain Specification. The root node of the referenced DS is understood as a NodeShape (and therefore as a Class Node). The important properties from the referenced DS are those that are also expected from a Class Node, namely `sh:class`, `sh:closed`, and `sh:property`.

Example:

In the following DS the `schema:address` Property Shape defines another Domain Specification as the range for the property (only the `@id` of the referenced Domain Specification must be given).
(We assume that `https://semantify.it/ds/rsDL46Kfs` is the IRI of an existing DS that defines a Class which is a valid range for `schema:address`) 

```json
"@graph": [
  {
    "@id": "https://semantify.it/ds/rsFn_FabM",
    "@type": "ds:DomainSpecification",
    ...
    "sh:property": [
      {
        "@type": "sh:PropertyShape",
        "sh:path": "schema:address",
        ...
        "sh:or": [
          {
            "sh:node": {
              "@id": "https://semantify.it/ds/rsDL46Kfs"
            }
          }
        ]
      }
    ]
  }
]
```

#### 3.5.3. Populate Domain Specifications

The process of resolving constraints from external references and from the Super-Domain Specification is defined as **Populate**, a term inspired by [Mongoose](https://mongoosejs.com/docs/populate.html). After this process, the resulting Domain Specification is called **Populated Domain Specification** and can be seen as an independent document, since all external constraints have been consolidated during the population.

Examples for the population are provided in the examples file [Examples file](./../../Examples/README.md).

#### 3.5.3.1. Populate a Super-DS

Details about the semantics of `ds:subDSOf` are given in chapter **3.5.1.** of this document.

In order to populate the constraints defined in a Super-DS the following steps have to be considered:

* **Consolidate the used** `@context`: any additions/differences in the `@context` of the Super-DS must be taken into account when adapting the `@context` (and subsequently the content) of the Sub-DS. Since Domain Specifications are expected to use the same standard `@context,` this step becomes relevant when additional namespaces are stated (e.g. external vocabularies, extensions).
* **Consolidate the root nodes:** the constraints defined in the Super-DS root node must be consolidated into the Sub-DS root node. That means that constraints are copied to the Sub-DS UNLESS those constraints are redefined in the Sub-DS. This concerns:
  * Target-matching constraints like `sh:targetClass`.
  * `ds:usedVocabulary`: any external vocabularies used in the Super-DS must be included in the external vocabularies of the Sub-DS.
  * `sh:class` and `sh:closed` (if the Super-DS has `"sh:closed": true` then the Sub-DS is not allowed to add any new properties, this would go against the semantics of `ds:subDSOf`).
  * for `sh:property`, every Property Node is compared separately. The resulting set of Property Nodes consists of:
    * Property Nodes from the Super-DS that are not redefined in the Sub-DS (the `sh:path` constraint is used for comparison).
    * Property Nodes from the Sub-DS.
* **Consolidate internal references:** If the Super-DS had additional NodeShapes in the `@graph`, those must also be taken over in the `@graph` of the Sub-DS.
* Take into account that population is recursive. In order to populate a Super-DS into a Sub-DS, the Super-DS must be populated first (if it has its own Super-DS, and/or external references).
  
#### 3.5.3.2. Populate external references

Details about the semantics of references are given in chapter **3.5.2.** of this document.

In order to populate the constraints defined in an external reference the following steps have to be considered:

* **Consolidate the used** `@context`: any additions/differences in the `@context` of the referenced DS must be taken into account when adapting the `@context` (and subsequently the content) of the populated DS. Since Domain Specifications are expected to use the same standard `@context,` this step becomes relevant when additional namespaces are stated (e.g. external vocabularies, extensions).
* `ds:usedVocabulary`: any external vocabularies used in the referenced DS must be included in the external vocabularies of the populated DS.
* **Covert root node of referenced DS:** The root node of the referenced DS must be converted into an internal referenced NodeShape and added to the `@graph` of the populated DS. The important properties from the referenced DS are those that are also expected from a Class Node, namely `sh:class`, `sh:closed`, and `sh:property`.
* **Consolidate internal references:** If the referenced DS had additional NodeShapes in the `@graph`, those must also be taken over in the `@graph` of the populated DS.
* Take into account that population is recursive. In order to populate an external reference into a DS, the referenced DS must be populated first (if it has a Super-DS, and/or external references).