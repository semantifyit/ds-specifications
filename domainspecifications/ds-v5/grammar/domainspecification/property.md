# Property Node

Property Nodes express the constraints for a given Property. `sh:or` holds the possible ranges and their constraints for the property. If there is only 1 possible range, then it is possible to express all range constraints in the property node itself instead of using `sh:or` in theory. Consequently, any property-key of a RangeNode may be part of a Property Node. However, it is recommended to always use `sh:or` to wrap the range\(s\), since within semantify.it the code expects this structure.

## Key-value table

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `sh:path` | true | _IRI_ | The IRI of the Property that is restricted by this node |  |
| `sh:or` | true | List of **RangeNode** | List of RangeNode. Every value of this property must match at least 1 of the listed range nodes | Non-conform range |
| `@type` | false | `sh:PropertyShape` | Declares the type of this node |  |
| `sh:minCount` | false | _Integer_ | The minimum cardinality \(amount of values\) for the property | Missing Property, Non-conform cardinality |
| `sh:maxCount` | false | _Integer_ | The maximum cardinality \(amount of values\) for the property | Non-conform cardinality |
| `sh:order` | false | _Integer_ | The position of this Property in comparison with other properties for the same Subject \(only representation purpose\) |  |
| `rdfs:comment` | false | _String_ | The Description/Justification for this Property and/or its ranges\) |  |

## Example

Tthe last 2 examples express the same constraints, it is recommeded to use the variant with `sh:or`:

```javascript
{
    "@type": "sh:PropertyShape",
    "sh:path": "schema:address",
    "sh:minCount": 1,   
    "sh:maxCount": 1,   
    "sh:order": 12,
    "rdfs:comment": "This Property is required. Its value must be either a String or a PostalAddress entity.",
    "sh:or": [
         ...RangeNodes...
    ]
},
{
    "@type": "sh:PropertyShape",
    "sh:path": "schema:name",
    "sh:or": [
       {
          "sh:datatype": "xsd:string",
          "sh:minLength": 3
       }
    ]
},
{
    "@type": "sh:PropertyShape",
    "sh:path": "schema:name",
    "sh:datatype": "xsd:string",
    "sh:minLength": 3
}
```

### Property-pair constraints

The following property-pair constraints allow to compare the literal values of different properties. It makes sense to define them at property level, rather than at range level.

| key | value type | description | related error |
| :---: | :---: | :--- | :---: |
| `sh:equals` | \[ _IRI_ \] | Specifies the condition that the set of all value nodes is equal to the set of objects of the triples that have the focus node as subject and the value of sh:equals as predicate. \(The property shape having `sh:equals` must have the same values as the property shape with the `sh:path` specified with `sh:equals`\) | Non-conform sh:equals |
| `sh:disjoint` | \[ _IRI_ \] | Specifies the condition that the set of value nodes is disjoint with the set of objects of the triples that have the focus node as subject and the value of `sh:disjoint` as predicate. \(The property shape having `sh:disjoint` must NOT have the same values as the property shape with the `sh:path` specified with `sh:disjoint`\) | Non-conform sh:disjoint |
| `sh:lessThan` | \[ _IRI_ \] | Specifies the condition that each value node is smaller than all the objects of the triples that have the focus node as subject and the value of `sh:lessThan` as predicate. \(The property shape having `sh:lessThan` must have the values that are less than the property shape with the `sh:path` specified with `sh:lessThan`\). The comparison should be possible for all datatypes except boolean and URL. | Non-conform sh:lessThan |
| `sh:lessThanOrEquals` | \[ _IRI_ \] | Specifies the condition that each value node is smaller than or equal to all the objects of the triples that have the focus node as subject and the value of `sh:lessThanOrEquals` as predicate. \(like `sh:lessThan`, but equal values are also allowed\). The comparison should be possible for all datatypes except boolean and URL. | Non-conform sh:lessThanOrEquals |

Regarding the Error generation based on these property-pair constraints, tests were done trying [https://shacl.org/playground/](https://shacl.org/playground/):

* sh:equals produces an error if the specified property to compare does not exist.
* sh:disjoint does NOT produce an error if the specified property to compare does not exist.
* sh:lessThan does NOT produce an error if the specified property to compare does not exist.
* sh:lessThanOrEquals does NOT produce an error if the specified property to compare does not exist.

Textual definition of error generation in the SHACL specification:

* [https://www.w3.org/TR/shacl/\#EqualsConstraintComponent](https://www.w3.org/TR/shacl/#EqualsConstraintComponent)
* [https://www.w3.org/TR/shacl/\#DisjointConstraintComponent](https://www.w3.org/TR/shacl/#DisjointConstraintComponent)
* [https://www.w3.org/TR/shacl/\#LessThanConstraintComponent](https://www.w3.org/TR/shacl/#LessThanConstraintComponent)
* [https://www.w3.org/TR/shacl/\#LessThanOrEqualsConstraintComponent](https://www.w3.org/TR/shacl/#LessThanOrEqualsConstraintComponent)

Regarding the implementation, have a look at [https://www.w3.org/TR/rdf-sparql-query/](https://www.w3.org/TR/rdf-sparql-query/)

### Example

```javascript
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

