# DataType Node

A DataType Node specifies the constraints for a literal value.

## Key-value table

In the following the basic attributes that a DataType Node can have are listed:

| key | required | value type | description |  related error |
| :---: | :---: | :---: | :--- | :---: |
| `sh:datatype` | true | _IRI_ | The XSD-IRI representing the datatype the value must have. Below are the mapping functions between schema.org datatypes and XSD datatypes | Non-conform range |
| `sh:defaultValue` | false | _Literal_ | The standard value for this DataType (must be of the respective datatype). This is only a representational key-property (non-validating) |

## Example

```json
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:or": [
    {
      "sh:datatype": "xsd:string",
      "sh:defaultValue": "Innsbruck 6020"
    }
  ]  
}
```

## Advanced Constraints

Advanced constraints are introduced with this DS version. They can be used in DataType nodes for specific datatypes. Some of these constraints can have multiple entries; if that is the case, then an array is used to wrap the values.

See https://docs.google.com/spreadsheets/d/1AFD21lB_zQkns_3aeDLIFW1E-C36eJ3DogcnOb2vD7g/edit#gid=1009977856

### Value Range Constraint Components

SHACL reference: https://www.w3.org/TR/shacl/#core-components-range

These constraint components dictate a value range that the value of a literal value must have. The datatype for this value must be the same as the datatype of the constrained literal. Datatypes that are allowed to have these constraints are: Date, DateTime, Time, Number, Float, and Integer.

| key | value type | description | related error |
| :---: | :---: | :--- | :---: |
| `sh:minExclusive` |  same as constrained Data Type | The minimum exclusive value that the value must have|  Non-conform sh:minExclusive |
| `sh:minInclusive` |  same as constrained Data Type |The minimum inclusive value that the value must have| Non-conform sh:minInclusive |
| `sh:maxExclusive` |  same as constrained Data Type |  The maximum exclusive value that the value must have| Non-conform sh:maxExclusive |
| `sh:maxInclusive` | same as constrained Data Type |  The maximum inclusive value that the value must have | Non-conform sh:maxInclusive |

Example:

```JSON
{
    "sh:datatype": "xsd:integer",
    "sh:minExclusive": 0,
    "sh:maxInclusive": 10
}
```

```JSON
{
    "sh:datatype": "xsd:date",
    "sh:minExclusive": "2010-10-10"
}
```

### String based Constraint Components

SHACL reference: https://www.w3.org/TR/shacl/#core-components-string

These constraint components are meant for literals having the datatype string (and their sub-datatypes in some cases).

| key | value type | description | related error |
| :---: | :---: | :--- | :---: |
| `sh:maxLength` |  Integer | The maximum allowed string length of a literal (String or URL) |Non-conform sh:maxLength |
| `sh:minLength` |  Integer | The minimum allowed string length of a literal (String or URL) | Non-conform sh:minLength |
| `sh:pattern` |  [Regex] |  A regular expression that the literal must match (String or URL) | Non-conform sh:pattern |
| `sh:languageIn` | [Language-Tag] |  The literal must use a language tag given in the list of language tags (https://tools.ietf.org/html/bcp47) . Only for String values (in theory the exact datatype of the target literal is `rdf:langString`, but due to our Schema.org <-> XSD mapping we use `xsd:string` and allow a language tag for it) | Non-conform sh:languageIn |
| `sh:uniqueLang` | Boolean | The property `sh:uniqueLang` can be set to true to specify that no pair of value nodes may use the same language tag. (Array of values must have unique language tags). Only for String values (in theory the exact datatype of the target literal is `rdf:langString`, but due to our Schema.org <-> XSD mapping we use `xsd:string` and allow a language tag for it)| Non-conform sh:uniqueLang |

Example of a DataType Node as range for `schema:description`:

```JSON
{
    "sh:datatype": "xsd:string",
    "sh:minLength": 20,
    "sh:maxLength": 200,
    "sh:languageIn": [
      "en",
      "de"
    ],
    "sh:uniqueLang": true
}
```

Example of a DataType Node as range for `schema:telephone`:

```JSON
{
  "sh:datatype": "xsd:string",
  "sh:pattern": [
    "^\\s*\\+?\\s*([0-9][\\s-]*){9,}$"
  ]
}        
```
### Property Value Pair Constraint Components

SHACL reference: https://www.w3.org/TR/shacl/#core-components-property-pairs

These constraints are explained in [Property.md](./Property.md) since they are expressed on a property-level, and not on a data-type-level.

### Other Constraint Components

SHACL reference: https://www.w3.org/TR/shacl/#core-components-others

These constraints can be used on any data type.

| key | value type | description | related error |
| :---: | :---: | :--- | :---: |
| `sh:in` | same as constrained Data Type | Specifies the condition that each value node is a member of a provided SHACL list | Non-conform sh:in |
| `sh:hasValue ` | same as constrained Data Type | Specifies the condition that at least one value node is equal to the given value | Non-conform sh:hasValue |

Example:

```JSON
{
  "sh:path": "schema:City",
  "sh:or":[
    {
      "sh:datatype": "xsd:string",
      "sh:in": [
        "Innsbruck",
        "Wien",
        "Salzburg"
      ],
      "sh:hasValue": [
        "Innsbruck"
      ]
    }
  ]
}
```

## Datatype Mapping

Mapping Functions between XSD datatypes and schema.org datatypes:

(Other datatypes are ignored for now, since they are not used in practice)

```javascript
function schemaToXsd(dataType) {
    switch (dataType) {
        case "schema:Text":
            return "xsd:string";
        case "schema:Boolean":
            return "xsd:boolean";
        case "schema:Date":
            return "xsd:date";
        case "schema:DateTime":
            return "xsd:dateTime";
        case "schema:Time":
            return "xsd:time";
        case "schema:Number":
            return "xsd:double";
        case "schema:Float":
            return "xsd:float";
        case "schema:Integer":
            return "xsd:integer";
        case "schema:URL":
            return "xsd:anyURI";
    }
}
function xsdToSchema(dataType) {
    switch (dataType) {
        case "xsd:string":
            return "schema:Text";
        case "xsd:boolean" :
            return "schema:Boolean";
        case "xsd:date" :
            return "schema:Date";
        case "xsd:dateTime":
            return "schema:DateTime";
        case "xsd:time":
            return "schema:Time";
        case "xsd:double":
            return "schema:Number";
        case "xsd:float":
            return "schema:Float";
        case "xsd:integer":
            return "schema:Integer";
        case "xsd:anyURI":
            return "schema:URL";
    }
}
```
