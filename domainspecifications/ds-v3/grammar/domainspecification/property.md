# SDO Property

## JSON Syntax of DS-V1

A **dsv:RestrictedProperty** node specifies the use of a SDO property \(referenced with **dsv:baseProperty**\) with a limited amount of ranges. Each range is represented by its own in the **dsv:expectedType** property. The ranges may be from following @ type: **schema:DataType**, **dsv:RestrictedClass**, or **dsv:RestrictedEnumeration**. Cardinality is expressed through the properties **dsv:isOptional** and **dsv:multipleValuesAllowed**.

```javascript
{
  "@type": "dsv:Property",
  "dsv:baseProperty": {
    "@id": "schema:address"
  },
  "schema:name": "address",
  "dsv:isOptional": false,
  "dsv:multipleValuesAllowed": false,
  "dsv:justification": "The address of an event. To indicate the place of the event.",
  "dsv:expectedType": [
    {
      "@type": "schema:DataType",
      ...
    },
    {
      "@type": "dsv:RestrictedClass",
      ...
    }
  ]
}
```

## SHACL JSON-LD Syntax of DS-V3

In SHACL Syntax the referencing of the property is done with **sh:path** instead of **dsv:baseProperty**. The cardinality is expressed with **sh:minCount** and **sh:maxCount**. The **"@ type": "sh:PropertyShape"** type definition is optional, but recommended \(In our case we want to use it, in order to make the DS more readable\). The SHACL property **sh:closed** is expected as standard \(other properties as those listed are not allowed\) and has not to be listed in the DS. The property ordering can now be defined with **sh:order** \(int starting from 0, optional property\). The property may have **rdfs:comment** to justify the usage of the property and its ranges \(string, optional\). The definition of a custom description for this property is also possible with "sh:description" \(string, optional\), this allows to override the default property description from schema.org.

The value range\(s\) for the property can be defined in different manner, but we should always use "sh:or" to define the range\(s\), even if there is only 1 possible range. \(see explanation below\)

_Example uses @context of **context.md**_

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:minCount": 1,
  "sh:maxCount": 1,
  "sh:order": 5,
  "rdfs:comment": "The address is required to enable location based features of applications. The value should express the city, country and postal code of the business.",
  "sh:or": {
    "@list": [
      {
        "sh:class": "schema:PostalAddress",
        "sh:node": {
          "@type": "sh:NodeShape",
          ...
        }
      },
      {
        "sh:datatype": "xsd:string"
      }
    ]
  }
}
```

The definition of ranges is more tricky in SHACL: Depending on the amount and type of the ranges different SHACL properties can/must be used. In the following are the different possibilities listed for a single range type, which is defined in the same level as the property itself. Note that we should always use the "sh:or" wrapper to make our lives easier while working with DS.

### Range: datatype

If the range is a basic datatype of SDO, then **sh:datatype** must be used. \(see mapping of SHACL&lt;-&gt;SDO datatypes in **datatype.md**\)

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:datatype": "xsd:string"
}
```

### Range: enumeration

If the range is an enumeration of SDO, then **sh:class** is used to specify the expected enumeration type \(e.g. **schema:DayOfWeek**\) and **sh:in** CAN used to specify a set of allowed enumeration members for that enumeration type \(e.g. **schema:Sunday**, **schema:Monday**, etc.\). See **enumeration.md** for more examples.

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

### Range: standard type

If the range is a class of SDO without any other restrictions, then **sh:class** is used to specify the expected SDO class that is used for the property. As long as the value of the property has the specified @ type, the constraint is fulfilled \(the entity may have any properties allowed for that class\).

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:class": "schema:PostalAddress"
}
```

### Range: restricted type

If the range is a restricted class of SDO, then **sh:node** is used \(additionally to **sh:class** to express the target class\) to wrap the extended constraints, that are defined with a **sh:NodeShape** node \(see **type.md**\).

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:class": "schema:PostalAddress",
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

### Multiple ranges

If there are multiple valid ranges for a property then all possible ranges are wrapped with the **sh:or** property.

```javascript
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:author",
  "sh:or": {
    "@list": [
      {
        "sh:class": "schema:Organization"
      },
      {
        "sh:class": "schema:Person",
        "sh:node": {
          "@type": "sh:NodeShape",
          "sh:property": [
             ...
        }
      },
      {
        "sh:datatype": "xsd:string"
      }
    ]
  }
}
```

