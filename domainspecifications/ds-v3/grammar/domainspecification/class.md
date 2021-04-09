# SDO Class

## JSON Syntax of DS-V1

A **dsv:RestrictedClass** node specifies the use of a SDO Class \(referenced with **dsv:baseClass**\) with a limited amount of properties. Each property is represented by its own **dsv:Property** node.

```javascript
{
  "@type": "dsv:RestrictedClass",
  "dsv:baseClass": {
    "@id": "schema:Airport"
  },
  "schema:name": "Airport",
  "dsv:property": [
    {
      "@type": "dsv:Property",
      ...
    },
    ...
  ]
}
```

## SHACL JSON-LD Syntax of DS-V3

Basically a straightforward mapping. If a **sh:NodeShape** specifies a root node for verification, then **sh:targetClass** is used instead of **sh:class**. In SHACL there is supposed to be only one **sh:targetClass** that links the restriction definition with the target document. The **"@ type": "sh:NodeShape"** type definition is optional. For **sh:class** arrays are allowed, these are supposed to represent multi-typed entities \(MTE\) and our verification tool matches this MTEs only if the target entity has all the listed classes \(see the second example\). The verification tool is also expected to accept subclasses of the class\(es\) defined in **sh:class**.

_Example uses `@context` of **context.md**_

```javascript
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

## Target Annotation structure \(JSON-LD\)

```javascript
{
  "@context": "http://schema.org/",
  "@type": "Airport",
  ...
}
```

This MTE can have properties of **schema:HotelRoom** and of **schema:Product**.

```javascript
{
  "@context": "http://schema.org/",
  "@type": ["HotelRoom", "Product"],
  ...
}
```

