# Domain Specification

## JSON Syntax of DS-V1

The **dsv:DomainSpecification** node specifies the used context, metadata about the DS, and the possible target types for verification, which are stored within the **dsv:class** property \(e.g. target type and possible sub-types\).

```javascript
{
  "@context": {
    "dsv": "http://ontologies.sti-innsbruck.at/dsv/",
    "schema": "http://schema.org/"
  },
  "@type": "dsv:DomainSpecification",
  "schema:name": "Airport DS",
  "schema:version": "1.1",
  "schema:schemaVersion": "https://schema.org/version/3.4/",
  "schema:author": "Author",
  "schema:description": "description of the DS",
  "dsv:class": [
    {
      "@type": "dsv:RestrictedClass",
      "dsv:baseClass": {
        "@id": "schema:Airport"
      },
      ...
    },
    ...
  ]
}
```

## SHACL JSON-LD Syntax of DS-V3

For SHACL, the root node of the DS is similar to our current version, but there is only one possible target type for verification, which is specified with **sh:targetClass**. Our Verification Tool is expected to match any subclasses of the targetClass to this DS. In our Verification Tool this matching happens only for the outermost entity \(root object/entity of the validated annotation\). \(comment Umut\) **sh:targetClass** can not have an array of classes as targets, so MTE matching for the root node would not be possible in SHACL. \(comment Omar\) After further discussion we just introduced the possibility to use an Array for **sh:targetClass** because the feature of matching MTE in the root node is a Must-Have.

_Example uses @context of **context.md**_

```javascript
{
  "@id": "_:DSRootNode",
  "@type": ["sh:NodeShape", "schema:CreativeWork"],
  "schema:name": "Airport DS",
  "schema:version": "1.1",
  "schema:schemaVersion": "https://schema.org/version/3.4/",
  "schema:author": "Author",
  "schema:description": "description of the DS",
  "sh:targetClass": "schema:Airport",
  "sh:property": [
    {
      "@type": "sh:PropertyShape",
      ...
    },
    ...
  ]
}
```

## Target Annotation structure \(JSON-LD\)

This is a Schema.org annotation that uses a valid syntax for the previous DS. This example uses a "correct and complete" JSON-LD syntax. \(using @vocab results in adding the url to the IDs instead of loading the actual context of the url!! This may be wished if the target context does "strange stuff", which is the case for Schema.org!\)

```javascript
{
  "@context": {
    "@vocab": "http://schema.org/"
  },
  "@type": "Airport",
  ...
}
```

## Expected Annotation structure \(JSON-LD\)

This annotation shows a simplified syntax \(how it is used in the wild\) for the previous example annotation. This syntax is usually not enough to be processed by a SHACL Verification Tool, however, our verification tool should be able to interpret/validate it.

```javascript
{
  "@context": "http://schema.org/",
  "@type": "Airport",
  ...
}
```

