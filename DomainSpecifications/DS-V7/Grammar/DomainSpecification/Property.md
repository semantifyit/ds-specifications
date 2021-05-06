# Property Node

Property Nodes express the constraints for a given property (identified by its IRI in `sh:path`). The term `sh:or` holds the possible ranges and their constraints for the property. If there is only 1 possible range, then it is possible to express all range constraints in the property node itself instead of using `sh:or` in theory, but in order to keep the same syntax for all Property Shapes `sh:or` should always be used.

## 1. Example

```JSON
{
  "@type": "sh:PropertyShape",
  "sh:order": 12,
  "sh:path": "schema:address",
  "sh:minCount": 1,
  "sh:maxCount": 1,
  "rdfs:comment": [
    {
      "@language": "en",
      "@value": "This Property is required. Its value must be either a String or a PostalAddress entity."
    }
  ],
  "sh:or": [
    ...
  ]
}
```

## 2. Key-value table

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@type` | true | `sh:PropertyShape` | The fixed type for a Property Node |
| `sh:order` | false | _Integer_ | The position of this property in comparison with other properties for the same Subject (only representation purpose) |
| `sh:path` | true | _IRI_ | The IRI of the property that is restricted by this node |
| `sh:minCount` | false | _Integer_ | The minimum cardinality (amount of values) for the property | Missing Property, Non-conform cardinality |
| `sh:maxCount` | false | _Integer_ | The maximum cardinality (amount of values) for the property | Non-conform cardinality |
| `rdfs:comment` | false | List of *Language tagged String* |  The Description/Justification for this Property and/or its ranges |
| `sh:or` | true | List of **Range Node** | List of Range Nodes. Every value of this property must match at least one of the listed range nodes | Non-conform range |
| `sh:equals`  | false | List of _IRI_  | The property must have the same values as the property specified in `sh:equals` | Non-conform sh:equals |
| `sh:disjoint`  | false | List of _IRI_  | The property must NOT have the same values as the property specified in `sh:disjoint`) |Non-conform sh:disjoint |
| `sh:lessThan`  | false | List of _IRI_ | The property must have values that are less than the values of the property specified in `sh:lessThan` |Non-conform sh:lessThan |
| `sh:lessThanOrEquals`  | false | List of _IRI_  | The property must have values that are less than or equal to the values of the property specified in `sh:lessThanOrEquals` | Non-conform sh:lessThanOrEquals |

## 3. Semantics

### 3.1. Property-pair constraints

See [SHACL specification](https://www.w3.org/TR/shacl/#core-components-property-pairs).

The following property-pair constraints allow to compare the literal values of different properties (on the same level = properties of the same subject). It makes sense to define them at property level, rather than at range level.

### 3.1.1. sh:equals

Specifies the condition that the set of all value nodes is equal to the set of objects of the triples that have the focus node as subject and the value of sh:equals as predicate. (The property shape having `sh:equals` must have the same values as the property shape with the `sh:path` specified with `sh:equals`) | Non-conform sh:equals

### 3.1.2. sh:disjoint

Specifies the condition that the set of value nodes is disjoint with the set of objects of the triples that have the focus node as subject and the value of `sh:disjoint` as predicate. (The property shape having `sh:disjoint` must NOT have the same values as the property shape with the `sh:path` specified with `sh:disjoint`) |Non-conform sh:disjoint

### 3.1.3. sh:lessThan

Specifies the condition that each value node is smaller than all the objects of the triples that have the focus node as subject and the value of `sh:lessThan` as predicate. (The property shape having `sh:lessThan` must have the values that are less than the property shape with the `sh:path` specified with `sh:lessThan`). The comparison should be possible for all datatypes except boolean and URL.

### 3.1.4. sh:lessThanOrEquals

Specifies the condition that each value node is smaller than or equal to all the objects of the triples that have the focus node as subject and the value of `sh:lessThanOrEquals` as predicate. (like `sh:lessThan`, but equal values are also allowed). The comparison should be possible for all datatypes except boolean and URL.




Regarding the Error generation based on these property-pair constraints, tests were done trying https://shacl.org/playground/:

* sh:equals produces an error if the specified property to compare does not exist.
* sh:disjoint does NOT produce an error if the specified property to compare does not exist.
* sh:lessThan does NOT produce an error if the specified property to compare does not exist.
* sh:lessThanOrEquals does NOT produce an error if the specified property to compare does not exist.

Textual definition of error generation in the SHACL specification:

* https://www.w3.org/TR/shacl/#EqualsConstraintComponent
* https://www.w3.org/TR/shacl/#DisjointConstraintComponent
* https://www.w3.org/TR/shacl/#LessThanConstraintComponent
* https://www.w3.org/TR/shacl/#LessThanOrEqualsConstraintComponent


Regarding the implementation, have a look at https://www.w3.org/TR/rdf-sparql-query/

### Example

```JSON
{
    "@type": "sh:PropertyShape",
    "sh:path": "schema:name",
    "rdfs:comment": "The full name of a Person. It should not be the same as the 'givenName', which is only the first name of the Person.",
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
    "rdfs:comment": "The first name of a person.",
    "sh:or": [
       {
          "sh:datatype": "xsd:string"       
       }
    ]
}
```
