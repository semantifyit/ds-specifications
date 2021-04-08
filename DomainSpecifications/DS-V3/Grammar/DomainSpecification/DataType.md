# SDO Datatype

### Actual JSON Syntax

A **dsv:DataType** node appears as value for the **dsv:expectedType** property of a **dsv:Property** node. The **@ id** property specifies the corresponding datatype of SDO.

```json
{
  "@type": "dsv:DataType",
  "@id": "schema:Text",
  "schema:name": "Text"
}
```

### SHACL JSON-LD Syntax

Datatypes in SHACL are expressed using **sh:datatype**. Our mapping of SHACL (uses XSD datatypes) and SDO (has its own datatypes) showed below. A datatype definition may use **sh:defaultValue** to express a default value for the property (optional, must be of the specified datatype).

*Example uses @ context of **context.md***

```json
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:address",
  "sh:datatype": "xsd:string",
  "sh:defaultValue": "Innsbruck 6020"
}
```

### Datatype mapping

Bijective mapping of Datatypes from SDO and SHACL:

```js
switch (SDODataType["@id"]) {
    case "schema:Text":
        result["sh:datatype"] = "xsd:string";
    case "schema:Boolean":
        result["sh:datatype"] = "xsd:boolean";
    case "schema:Date":
        result["sh:datatype"] = "xsd:date";
    case "schema:DateTime":
        result["sh:datatype"] = "xsd:dateTime";
    case "schema:Time":
        result["sh:datatype"] = "xsd:time";
    case "schema:Number":
        result["sh:datatype"] = "xsd:double";
    case "schema:Float":
        result["sh:datatype"] = "xsd:float";
    case "schema:Integer":
        result["sh:datatype"] = "xsd:integer";
    case "schema:URL":
        result["sh:datatype"] = "xsd:anyURI"; 
}
```