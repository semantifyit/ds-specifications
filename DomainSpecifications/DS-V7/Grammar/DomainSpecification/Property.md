# Property Node

Property Nodes express the constraints for a given property (identified by its IRI in `sh:path`). The term `sh:or` holds the possible ranges and their constraints for the property. If there is only 1 possible range, then it is possible to express all range constraints in the property node itself instead of using `sh:or` in theory, but in order to keep the same syntax for all Property Shapes `sh:or` should always be used.

## 1. Example

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "rdfs:label": [
    {
      "@language": "en",
      "@value": "Postal address"
    },
    {
      "@language": "de",
      "@value": "Adresse"
    }
  ],
  "rdfs:comment": [
    {
      "@language": "en",
      "@value": "This Property is required. Its value must be either a String or a PostalAddress entity."
    }
  ],
  "sh:minCount": 1,
  "sh:maxCount": 1,
  "sh:or": [
    ...
  ]
}
```

## 2. Key-value table

The following table lists all possible terms that can be used by a Property Node. The order in the table reflects the recommended order of these terms within a Property Node (optional).

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@type` | true | `sh:PropertyShape` | The fixed type for a Property Node |
| `sh:order` | false | _Integer_ | The position of this property in comparison with other properties for the same Subject (only representation purpose) - deprecated in favor of `ds:propertyDisplayOrder` |
| `sh:path` | true | _IRI_ | The IRI of the property that is restricted by this node |
| `rdfs:label` | false | List of *Language tagged String* |  The label for this Property |
| `rdfs:comment` | false | List of *Language tagged String* |  The description/justification for this Property and/or its ranges |
| `sh:minCount` | false | _Integer_ | The minimum cardinality (amount of values) for the property | Missing Property, Non-conform cardinality |
| `sh:maxCount` | false | _Integer_ | The maximum cardinality (amount of values) for the property | Non-conform cardinality |
| `sh:equals`  | false | List of _IRI_  | The property must have the same values as the property specified in `sh:equals` | Non-conform sh:equals |
| `sh:disjoint`  | false | List of _IRI_  | The property must NOT have the same values as the property specified in `sh:disjoint`) |Non-conform sh:disjoint |
| `sh:lessThan`  | false | List of _IRI_ | The property must have values that are less than the values of the property specified in `sh:lessThan` |Non-conform sh:lessThan |
| `sh:lessThanOrEquals`  | false | List of _IRI_  | The property must have values that are less than or equal to the values of the property specified in `sh:lessThanOrEquals` | Non-conform sh:lessThanOrEquals |
| `sh:or` | true | List of **Range Node** | List of Range Nodes. Every value of this property must match at least one of the listed range nodes | Non-conform range |

## 3. Semantics

### 3.1. Property match

The term `sh:path` specifies the IRI of the property to which this Property Node belongs.

Example:

```JSON
"sh:path": "schema:name"
```  

### 3.2. Property Range

The term `sh:or` contains all possible range specifications that the property can have. The value of the property must be compliant with at least one of the specified ranges in order to be compliant with the Property Node.

From a grammar point of view, following Node types are possible ranges (Range Nodes):

* [Class Node](./Class.md)
* [Enumeration Node](./Enumeration.md)
* [Data Type Node](./DataType.md)

In the following example the property `schema:address` must have values that are either a string (`"sh:datatype": "xsd:string"`), or an entity with the class 'PostalAddress' (`"sh:class":["schema:PostalAddress"]`).

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:or": [
    {
      "sh:datatype": "xsd:string"
    },
    {
      "sh:node": {
        "@id": "https://semantify.it/ds/Oadk498#hgrdH",
        "@type": "sh:NodeShape",
        "sh:class": [
          "schema:PostalAddress"
        ],
        ...
      }
    }
  ]
}
```  

### 3.3. Property-pair constraints

See [SHACL specification](https://www.w3.org/TR/shacl/#core-components-property-pairs).

The following property-pair constraints allow comparing the literal values of different properties (on the same level = properties of the same subject). It makes sense to define them at the property level, rather than at the range level. The values of the property-pair constraints terms are IRIs (multiple value possible).

In the following example the constraint `sh:disjoint` specifies that the property `schema:name` MUST NOT have the same value as the property `schema:givenName`.

```JSON
[
  {
    "@type": "sh:PropertyShape",
    "sh:path": "schema:name",
    "rdfs:comment": [
      {
        "@language": "en",
        "@value": "The full name of a Person. It should not be the same as the 'givenName'."
      }
    ],
    "sh:disjoint": [
      "schema:givenName"
    ],
    "sh:or": [
      {
        "sh:datatype": "xsd:string"
      }
    ]
  },
  {
    "@type": "sh:PropertyShape",
    "sh:path": "schema:givenName",
    "rdfs:comment": [
      {
        "@language": "en",
        "@value": "The first name of a person."
      }
    ],
    "sh:or": [
      {
        "sh:datatype": "xsd:string"
      }
    ]
  }
]
```

### 3.3.1. sh:equals

See [SHACL specification](https://www.w3.org/TR/shacl/#EqualsConstraintComponent).

Specifies the condition that the set of all value nodes is equal to the set of objects of the triples that have the focus node as subject and the value of `sh:equals` as predicate. (The property shape having `sh:equals` must have the same values as the property shape with the `sh:path` specified with `sh:equals`).

### 3.3.2. sh:disjoint

See [SHACL specification](https://www.w3.org/TR/shacl/#DisjointConstraintComponent).

Specifies the condition that the set of value nodes is disjoint with the set of objects of the triples that have the focus node as subject and the value of `sh:disjoint` as predicate. (The property shape having `sh:disjoint` must NOT have the same values as the property shape with the `sh:path` specified with `sh:disjoint`).

### 3.3.3. sh:lessThan

See [SHACL specification](https://www.w3.org/TR/shacl/#LessThanConstraintComponent).

Specifies the condition that each value node is smaller than all the objects of the triples that have the focus node as subject and the value of `sh:lessThan` as predicate. (The property shape having `sh:lessThan` must have the values that are less than the property shape with the `sh:path` specified with `sh:lessThan`). The comparison should be possible for all data types except boolean and URL.

### 3.3.4. sh:lessThanOrEquals

See [SHACL specification](https://www.w3.org/TR/shacl/#LessThanOrEqualsConstraintComponent).

Specifies the condition that each value node is smaller than or equal to all the objects of the triples that have the focus node as subject and the value of `sh:lessThanOrEquals` as predicate. (like `sh:lessThan`, but equal values are also allowed). The comparison should be possible for all data types except boolean and URL.


Regarding the Error generation based on these property-pair constraints, tests were done trying https://shacl.org/playground/:

* sh:equals produces an error if the specified property to compare does not exist.
* sh:disjoint does NOT produce an error if the specified property to compare does not exist.
* sh:lessThan does NOT produce an error if the specified property to compare does not exist.
* sh:lessThanOrEquals does NOT produce an error if the specified property to compare does not exist.



### 3.4. Metadata

Following terms represent metadata about the given property. These terms do not have any effects on the verification result; They have only informational character.

#### 3.4.1. sh:order (**deprecated**)

This term is **deprecated** in favor of `ds:propertyDisplayOrder`. Details can be found in [DomainSpecification.md](./DomainSpecification.md).

The term `sh:order` CAN be used to indicate a specific order for a list of properties. This can be used e.g. by a GUI to render the list of properties in the wished order. The value for this term must be an integer, starting with `0`.

Example:

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:order": 0,
  "sh:path": "schema:givenName",
  ...
},
{
  "@type": "sh:PropertyShape",
  "sh:order": 1,
  "sh:path": "schema:familyName",
  ...
},
```

#### 3.4.2. rdfs:comment

The term `rdfs:comment` CAN be used to describe and/or justify the property and its expected value types (in different languages). The value for this term is a language-tagged string. The standard description for a property term is usually provided by its vocabulary itself, `rdfs:comment` can be used to overwrite that standard description.

Example:

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:name",
  "rdfs:comment": [
    {
      "@language": "en",
      "@value":"Please provide only your first name and your family name, with a blank space between."
    }
  ],
  "sh:or": [
    {
      "sh:datatype": "xsd:string"
    }
  ]
}
```

#### 3.4.3. rdfs:label

The term `rdfs:label` CAN be used to give the property a label (in different languages). The value for this term is a language-tagged string. The standard label for a property term is usually provided by its vocabulary itself, `rdfs:label` can be used to overwrite that standard label.

Example:

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:postalAddress",
  "rdfs:label": [
    {
      "@language": "en",
      "@value":"Postal address"
    },
    {
      "@language": "de",
      "@value": "Wohnaddresse"
    }
  ],
  ...
}
```

### 3.5. Cardinality

The terms `sh:minCount` and `sh:maxCount` CAN be used to specify the cardinality for the values of the property (the amount of values that the property can have). The value for these terms must be a positive integer. In order to avoid contradictory constraints, the value of `sh:maxCount` MUST NOT be smaller than the value of `sh:minCount`.

The following example Property Node specifies that the property `schema:email` must have at least two values, and at most 3 values.

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:email",
  "sh:minCount": 2,
  "sh:maxCount": 3,
  ...
}
```

The following example Property Node specifies that the property `schema:name` must have at least one value. There is no upper limit for the amount of values.

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:name",
  "sh:minCount": 1,
  ...
}
```