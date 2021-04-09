# SDO Enumeration

## JSON Syntax of DS-V1

A **dsv:RestrictedEnumeration** node appears as value for the **dsv:expectedType** property of a **dsv:Property** node. The enumeration consists of the **dsv:RestrictedEnumeration** object that specifies the enumeration class, and the valid enumeration members that are listed in the array of **dsv:expectedEnumerationValue**.

```javascript
{
  "@type": "dsv:Property",
  "dsv:baseProperty": {
    "@id": "schema:dayOfWeek"
  },
  "dsv:expectedType": [
    {
      "@type": "dsv:RestrictedEnumeration",
      "dsv:baseEnumeration": {
        "@id": "schema:DayOfWeek"
      },
      "dsv:expectedEnumerationValue": [
        {
          "@type": "dsv:EnumerationValue",
          "@id": "schema:Sunday"
        },
        {
          "@type": "dsv:EnumerationValue",
          "@id": "schema:Thursday"
        }
      ]
    }
  ]
}
```

## SHACL JSON-LD Syntax of DS-V3

**sh:class** is used to specify the expected enumeration type \(e.g. **schema:DayOfWeek**\). Note that since a normal type also uses **sh:class**, the software interpreting the DS must be able to differentiate types from enumerations. **sh:in** CAN be used to specify the set of allowed enumeration members for that enumeration type \(e.g. **schema:Sunday**, **schema:Monday**, etc.\). If **sh:in** it not given, then any enumeration member of the target enumeration type is allowed \(see second example\). If the enumeration has no enumeration members \(happens in Schema.org ...\) the enumeration value is valid, as long as it is a URI.

_Example uses @context of **context.md**_

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:dayOfWeek",
  "sh:class": "schema:DayOfWeek",
  "sh:in": {
    "@list": [
      "schema:Sunday",
      "schema:Thursday"
    ]
  }
}
```

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:dayOfWeek",
  "sh:class": "schema:DayOfWeek"
}
```

## Target Annotation structure \(JSON-LD\)

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

## Expected Annotation structure \(JSON-LD\)

This annotation shows a simplified syntax \(how it is used in the wild\) for the previous example Schema.org annotation. This syntax is usually not enough to be processed by a SHACL validator for the previous enumeration constraint example, however our validation tool should be able to interpret/validate it.

```javascript
{
  "@context": "http://schema.org/",
  "@type": "OpeningHoursSpecification",
  "dayOfWeek": "http://schema.org/Thursday"
}
```

