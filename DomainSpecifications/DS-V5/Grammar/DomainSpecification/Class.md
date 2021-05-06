# Class Node

A Class Node specifies the constraints for a class. Note that this kind of node has similarities to an [Enumeration Node](./Enumeration.md).

For `sh:class` arrays are allowed, these are supposed to represent multi-typed entities (MTE) and our verifier matches this MTEs only if the target entity has all the listed types (see the second example) or subtypes of the type(s) defined in `sh:class`.

 If further restrictions are given for a type (e.g. its properties, which is always the expected case in semantify.it), then these are defined within a NodeShape. This NodeShape is wrapped by a `sh:node` attribute for the range node.

## Example

```json
{
  "sh:class": "schema:Airport",
  "sh:node": {
    "@type": "sh:NodeShape",
    "sh:property": [
      {
        "@type": "sh:PropertyShape",
        ...
      },
      ...
    ]
  }
}
,
{
  "sh:class": ["schema:HotelRoom", "schema:Product"],
  "sh:node": {
    "@type": "sh:NodeShape",
    "sh:property": [
      {
        "@type": "sh:PropertyShape",
        ...
      },
      ...
    ]
  }
}
```

## Key-value table

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `sh:class` | true | _IRI_ / [ *IRI* ] | The IRI(s) of the Class(es) that the value (value must be an entity with `@type`) must have | Non-conform range |
| `sh:node` | false | **NodeShape** | Additional constraints for the properties of the value (value must be an entity with `@type`) are possible with a node shape. The NodeShape CAN have the key-property `@type` with the string value `sh:NodeShape`, and it MUST have the key-property `sh:property` with an Array of PropertyNodes as value | Missing Property, Non-conform property |
