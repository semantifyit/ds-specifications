# Enumeration Node

An Enumeration Node is used as a potential range for a [Property Node](./Property.md).

An Enumeration Node specifies the constraints for an enumeration. Note that this kind of node has similarities to a [Class Node](./Class.md), because in theory an Enumeration is just Class for which pre-defined instances exist in the vocabulary. These pre-defined instances are called "enumeration members" in the following, and in most cases they are referenced by their IRI.

An Enumeration Node is represented by an object with `"@type": "sh:NodeShape"` that is wrapped by the term `sh:node`. This NodeShape is used to express all the characteristics and constraints of the Enumeration Node. The most important constraint is `sh:class`, which constraints the expected enumeration type (e.g. `schema:DayOfWeek`). Although, only a single Enumeration is expected for `sh:class`, it should be wrapped by an array for consistence (other uses of `sh:class` in the DS specification). Note that since a normal class also uses `sh:class`, the software interpreting the DS must be able to differentiate between classes and enumerations.

The `@id` of an Enumeration Node is used to reference it in other parts of the Domain Specification, and so reuse already defined constraints for a specific Enumeration. Details below.


## 1. Example

```json
{
  "sh:node": {
    "@id": "https://semantify.it/ds/OBbzsh4_B#6hrhus",
    "@type": "sh:NodeShape",
    "sh:class": [
      "schema:DayOfWeek"
    ],
    "sh:in": [
      {
        "@id": "schema:Monday"
      },
      {
        "@id": "schema:Tuesday"
      },
      {
        "@id": "schema:Wednesday"
      },
      {
        "@id": "schema:Thursday"
      },
      {
        "@id": "schema:Friday"
      }
    ]
  }
}
```

## 2. Key-value Table

The following table lists all possible terms that can be used by an Enumeration Node. The order in the table reflects the recommended order of these terms within an Enumeration Node (optional).

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| `@id` | true | *IRI* | The IRI of the Enumeration Node, which is based on the DS IRI it is in |
| `@type` | true | `"sh:NodeShape"` | The fixed type for an Enumeration Node |
| `sh:class` | true | [ *IRI* ] | The IRI of the expected Enumeration - it should be only one | Non-conform range |
| `sh:in `| false | [ *IRI* ]  | The set of URIs (enumeration members) that are valid values for this Enumeration, these are given as `@id` wrapped strings (see example) | Non-conform enumeration value |

## 3. Semantics

### 3.1. sh:in

The constraint `sh:in` CAN be used to specify the set of allowed enumeration members for an enumeration type (e.g. `schema:Sunday`, `schema:Monday`, etc.). If `sh:in` is not given, then, based on schema.org's problematic enumeration model, any string could be allowed (should be an IRI). Checking if enumeration members are "valid" in general should be a task for the schema.org verification rather than a DS verification. If a user wants a specific set of allowed enumeration members, then he/she should use `sh:in` to define them.  Note that the IRIs given in sh:in are wrapped by `@id`, we can not change this in the `@context` since `sh:in` is also used for [DataType Nodes](./DataType.md) to express a set of valid values.

In theory, it is also possible to just create a new entity for an enumeration (e.g. a new `schema:DayOfWeek`), this would not be an IRI, but a class object. Nevertheless, in context of domain specifications we keep the use of IRIs to reference pre-defined instances from the vocabulary as the expected behaviour for enumerations.

Example:

```json
{
  "sh:in": [
    {
      "@id": "schema:Monday"
    },
    {
      "@id": "schema:Tuesday"
    },
    {
      "@id": "schema:Wednesday"
    }
  ]
}
```

### 3.2. Use of Internal references

Please check `3.3. Use of Internal references` in [Class.md](./Class.md).

### 3.3. Accepted representations

The correct way to reference an enumeration member is by using an `@id` to mark the IRI value as a referenced entity:

```json
 {
  "@type": "OpeningHoursSpecification",
  "closes": "17:00:00",
  "dayOfWeek": {
    "@id": "https://schema.org/Sunday"
  },
  "opens": "09:00:00"
}
```

However, in the schema.org context file the properties that have an enumeration as range are specified as always having an `@id` as value (this is imperfect: what about properties that can have additional ranges, no only enumerations?), therefore it is usual to see annotations having the IRI directly as value, as if it was a string/URL:

```json
 {
  "@type": "OpeningHoursSpecification",
  "closes": "17:00:00",
  "dayOfWeek": "https://schema.org/Sunday",
  "opens": "09:00:00"
}
```

Both variants should be seen as valid (even if the schema.org context file is not loaded), for compatibility reasons. However, it should be clear that an `@id` wrapping the referenced IRI is the correct variant.