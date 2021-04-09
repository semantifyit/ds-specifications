# Enumeration Node

Am Enumeration Node specifies the constraints for an enumeration. Note that this kind of node has similarities to a [Class Node](class.md).

`sh:class` is used to specify the expected enumeration type \(e.g. `schema:DayOfWeek`\). Note that since a normal class also uses `sh:class`, the software interpreting the DS must be able to differentiate between classes and enumerations.

`sh:in` CAN be used to specify the set of allowed enumeration members for an enumeration type \(e.g. `schema:Sunday`, `schema:Monday`, etc.\). If `sh:in` is not given, then, based on schema.org's problematic enumeration model, any string could be allowed \(should be a URI\). \(Checking if enumeration members are "valid" in general should be a task for the schema.org verification rather than a DS verification. If a user wants a specific set of allowed enumeration members, then he should use `sh:in` to define them.\)

## Key-value table

| key | required | value type | description | related error |
| :---: | :---: | :---: | :--- | :---: |
| sh:class | true | IRI | The IRI of the Enumeration that the value \(value must be an entity with @ type\) must have | Non-conform range |
| sh:in | false | \[IRI\] | The set of URIs \(enumeration members\) that are valid values for this Enumeration, these are given as @id wrapped strings \(see example\) | Non-conform enumeration value |

## Example

The first Enumeration Node restricts the enumeration to a set of enemeration members \(the value must be one of those enumeration members\).

The second Enumeration Node does not constraint the enumeration members, in theory any IRI that points to an instance that is a `schema:DayOfWeek` would be a valid value, in practice one would only allow IRIs that are specified as enumeration members for this enumeration by the used vocabulary \(schema.org\).

```javascript
{
  "sh:class": "schema:DayOfWeek",
  "sh:in": [
      { "@id": "schema:Sunday"},
      { "@id": "schema:Thursday"}
  ]
},
{
  "sh:class": "schema:DayOfWeek"
}
```

### Target Annotation structure \(JSON-LD\)

This is a Schema.org annotation that is valid for the previous enumeration constraint. This example uses a "correct and complete" JSON-LD syntax.

```javascript
{
  "@context": {
    "@vocab": "http://schema.org/"
  },
  "@type": "OpeningHoursSpecification",
  "dayOfWeek": {
    "@id": "http://schema.org/Thursday",
    "@type": "http://schema.org/DayOfWeek"
  }
}
```

### Expected Annotation structure \(JSON-LD\)

This annotation shows a simplified syntax \(how it is used in the wild\) for the previous example Schema.org annotation. This syntax is usually not enough to be processed by a SHACL validator for the previous enumeration constraint example, however, a verification tool should be able to interpret/verify it.

```javascript
{
  "@context": "http://schema.org/",
  "@type": "OpeningHoursSpecification",
  "dayOfWeek": "http://schema.org/Thursday"
}
```

