# Data Type Node

A Data Type Node is used as a potential range for a [Property Node](./Property.md).

A Data Type Node specifies the constraints for a literal value. A literal value has always a specific data type (`string` is the default). 

Domain Specifications make use of the [W3C XML Schema Definition Language (XSD) Data Type definitions](https://www.w3.org/TR/xmlschema11-2/#built-in-datatypes) to express data types. This is done because Domain Specifications use the SHACL term `sh:datatype` ([which relies on XSD](https://www.w3.org/TR/shacl/#DatatypeConstraintComponent)) to define data type constraints. However, since the Domain Specifications were built with the schema.org vocabulary (SDO) in mind, there must be a mapping between [schema.org data types](https://schema.org/DataType) and XSD data types. This mapping is presented at the end of this document.

**DS-V7** includes many optional constraints for data types. Most of them can only be used on certain data types, and their values also depends on the data type they are restricting. A detailed explanation can be found in the **semantics** section below.

## 1. Example

```json
{
  "sh:datatype": "xsd:double"
}
```

```json
{
  "sh:datatype": "xsd:string",
  "sh:defaultValue": "Innsbruck",
  "sh:minLength": "3",
  "sh:maxLength": "40",
  "sh:in": [
    "Innsbruck",
    "Salzburg",
    "Vienna",
    "Linz"
  ]
}
```

## 2. Key-value table

The following table lists all possible terms that can be used by a Data Type Node. The order in the table reflects the recommended order of these terms within a Data Type Node (optional).

| key | required | value type | description |  related error |
| :---: | :---: | :---: | :--- | :---: |
| `sh:datatype` | true | _IRI_ | The XSD-IRI representing the datatype the value must have. Below are the mapping functions between schema.org data types and XSD data types | Non-conform range |
| `sh:defaultValue` | false | _Literal_ | The standard value for this DataType (must be of the respective datatype). This is only a representational key-property (non-validating) |
| `ds:defaultLanguage` | false | _Language-Tag_ | The standard language tag for this DataType (must be a language-tagged string). This is only a representational key-property (non-validating) |
| `sh:minExclusive` | false | same as constrained Data Type | The minimum exclusive value that the value must have |  Non-conform sh:minExclusive |
| `sh:minInclusive` | false | same as constrained Data Type | The minimum inclusive value that the value must have | Non-conform sh:minInclusive |
| `sh:maxExclusive` | false | same as constrained Data Type |  The maximum exclusive value that the value must have | Non-conform sh:maxExclusive |
| `sh:maxInclusive` | false | same as constrained Data Type |  The maximum inclusive value that the value must have | Non-conform sh:maxInclusive |
| `sh:maxLength` | false |  _Integer_ | The maximum allowed string length of a literal (String or URL) |Non-conform sh:maxLength |
| `sh:minLength` | false |  _Integer_ | The minimum allowed string length of a literal (String or URL) | Non-conform sh:minLength |
| `sh:pattern` | false |  List of _Regex_ |  A regular expression that the literal must match (String or URL) | Non-conform sh:pattern |
| `sh:flag` | false |  _string_ | Optional regex flags for sh:pattern | Non-conform sh:pattern |
| `sh:languageIn` | false | List of _Language-Tag_ |  The literal must use a language tag given in the list of language tags | Non-conform sh:languageIn |
| `sh:uniqueLang` | false | _Boolean_ | The property must not use the same language tag more than one time | Non-conform sh:uniqueLang |
| `ds:hasLanguage` | false | List of _Language-Tag_  | The property must use all language tags in the given list | Non-conform ds:hasLanguage |
| `sh:in` | false | same as constrained Data Type | Specifies the condition that each value node is a member of a provided list | Non-conform sh:in |
| `sh:hasValue ` | false | List of _same as constrained Data Type_ | Specifies the condition that at least one value node is equal to the given value | Non-conform sh:hasValue |

## 3. Semantics

### 3.1. Datatype Mapping

The following table shows the mapping between **schema.org** and **XSD** data types:

| schema.org | XSD |
| :---: | :---: |
| `schema:Text` | `xsd:string` or `rdf:langString` |
| `schema:Boolean` | `xsd:boolean` |
| `schema:Date` | `xsd:date` |
| `schema:DateTime` | `xsd:dateTime` |
| `schema:Time` | `xsd:time` |
| `schema:Number` | `xsd:double` |
| `schema:Integer` | `xsd:integer` |
| `schema:Float` | `xsd:float` |
| `schema:URL` | `xsd:anyURI` |

In **DS-V7** a new datatype is introduced: `rdf:langString`

Until now, we have used `xsd:string` and allowed it to have language tags, which is not possible in theory. With **DS-V7** we do NOT allow `xsd:string` to have language tags any more (that means this datatype MUST NOT have language-related constraints), and `rdf:langString` is introduced to express a string that MUST have a language tag (the only datatype that CAN have language-related constraints). By language tag, we understand a string representing a language (see the [language tag specification](https://www.rfc-editor.org/rfc/bcp/bcp47.txt)).

For the datatype-mapping, this means that `schema:Text` is mapped to `xsd:string` if no language tag is allowed, or to `rdf:langString` if a language tag MUST be used. In theory, both of these datatype options could be added for `schema:Text` (e.g. to make language tags optional). For the verification, the division of these two data types must be clear.

Example in a Property node. The value for description MUST be a language tagged string because the data type is `rdf:langString`, further, the allowed language tags are restricted by `sh:languageIn`:

```json
{
  "@type": "sh:PropertyShape",
  "sh:path": "schema:description",
  "sh:or": [
    {
      "sh:datatype": "rdf:langString",
      "sh:languageIn": [
        "en",
        "de",
        "es"
      ]
    }
  ]
}
```

### 3.2. Advanced Constraints

Advanced constraints can be used in DataType nodes for specific data types. Some of these constraints can have multiple entries; if that is the case, then an array is used to wrap the values.

#### 3.2.1 Value Range Constraint Components

See [SHACL specification](https://www.w3.org/TR/shacl/#core-components-range)

These constraint components dictate a value range that the value of a literal value must have. 

The datatype for this value must be the same as the datatype of the constrained literal. 

Data types that are allowed to have these constraints are: Date, DateTime, Time, Number, Float, and Integer.


##### 3.2.1.1. sh:minExclusive

See [SHACL specification](https://www.w3.org/TR/shacl/#MinExclusiveConstraintComponent).

Specifies the minimum exclusive value that the literal must have.

The value for this constraint must have the same data type as the constrained literal.

A datatype node can have at most one value for `sh:minExclusive`.

Example:

```JSON
{
  "sh:datatype": "xsd:integer",
  "sh:minExclusive": 3
}
```

##### 3.2.1.2. sh:minInclusive

See [SHACL specification](https://www.w3.org/TR/shacl/#MinInclusiveConstraintComponent).

Specifies the minimum inclusive value that the literal must have.

The value for this constraint must have the same data type as the constrained literal.

A datatype node can have at most one value for `sh:minInclusive`.

Example:

```JSON
{
  "sh:datatype": "xsd:date",
  "sh:minInclusive": "2010-10-10"
}
```

##### 3.2.1.3. sh:maxExclusive

See [SHACL specification](https://www.w3.org/TR/shacl/#MaxExclusiveConstraintComponent).

Specifies the maximum exclusive value that the literal must have.

The value for this constraint must have the same data type as the constrained literal.

A datatype node can have at most one value for `sh:maxExclusive`.

Example:

```JSON
{
  "sh:datatype": "xsd:double",
  "sh:maxExclusive": 10.0
}
```

##### 3.2.1.4. sh:maxInclusive

See [SHACL specification](https://www.w3.org/TR/shacl/#MaxInclusiveConstraintComponent).

Specifies the maximum inclusive value that the literal must have.

The value for this constraint must have the same data type as the constrained literal.

A datatype node can have at most one value for `sh:maxInclusive`.

Example:

```JSON
{
  "sh:datatype": "xsd:date",
  "sh:maxInclusive": "2022-12-31"
}
```

#### 3.2.2 String based Constraint Components

See [SHACL specification](https://www.w3.org/TR/shacl/#core-components-string).

These constraint components are meant for literals having the datatype string (or URL in some cases).

##### 3.2.2.1. sh:minLength

See [SHACL specification](https://www.w3.org/TR/shacl/#MinLengthConstraintComponent).

Specifies the minimum inclusive length that the literal (string or URL) must have.

The value for this constraint must be an integer.

A data type node can have at most one value for `sh:minLength`.

Example:

```JSON
{
  "sh:datatype": "xsd:string",
  "sh:minLength": "4"
}
```

##### 3.2.2.2. sh:maxLength

See [SHACL specification](https://www.w3.org/TR/shacl/#MaxInclusiveConstraintComponent).

Specifies the maximum inclusive length that the literal (string or URL) must have.

The value for this constraint must be an integer.

A data type node can have at most one value for `sh:maxLength`.

Example:

```JSON
{
  "sh:datatype": "xsd:anyURI",
  "sh:minLength": "128"
}
```

##### 3.2.2.3. sh:pattern

See [SHACL specification](https://www.w3.org/TR/shacl/#PatternConstraintComponent).

`sh:pattern` specifies regular expressions that each literal (string or URL) must match.

The value(s) for this constraint must be a string.

The optional term `sh:flags` can be used to specify the regex flags used (value is a string).

Example of a DataType Node as range for `schema:telephone`:

```JSON
{
  "sh:datatype": "xsd:string",
  "sh:pattern": [
    "^\\s*\\+?\\s*([0-9][\\s-]*){9,}$"
  ],
  "sh:flag": "i"
}        
```

##### 3.2.2.4. sh:languageIn

See [SHACL specification](https://www.w3.org/TR/shacl/#LanguageInConstraintComponent).

`sh:languageIn` specifies that the literal must use a [language tag](https://tools.ietf.org/html/bcp47) that is present in the given list.

This constraint can only be used for language-tagged strings (data type is `rdf:langString`).

Example:

```JSON
{
  "sh:datatype": "rdf:langString",
  "sh:languageIn": [
    "en",
    "de"
  ]
}
```

##### 3.2.2.5. sh:uniqueLang

See [SHACL specification](https://www.w3.org/TR/shacl/#UniqueLangConstraintComponent).

`sh:languageIn` specifies that the multiple values for this data type must have different [language tags](https://tools.ietf.org/html/bcp47).

This constraint can only be used for language-tagged strings (data type is `rdf:langString`).

Example:

```JSON
{
  "sh:datatype": "rdf:langString",
  "sh:uniqueLang": true
}
```

##### 3.2.2.6. ds:hasLanguage

`ds:hasLanguage` has a list of [language tags](https://tools.ietf.org/html/bcp47) as value.

This constraint specifies that the literal values for the given data type must use language tags that cover at least one time each language tag specified in the given list.

This constraint can only be used for language-tagged strings (data type is `rdf:langString`).

```JSON
{
  "sh:datatype": "rdf:langString",
  "ds:hasLanguage": [
    "en",
    "de"
  ]
}
```

### 3.2.3. Other Constraint Components

See [SHACL specification](https://www.w3.org/TR/shacl/#core-components-others).

#### 3.2.3.1. sh:in

See [SHACL specification](https://www.w3.org/TR/shacl/#InConstraintComponent).

The value for `sh:in` is a list of values that have the same data type as the constrained literal (usable for all data types).

This constraint specifies that the literal must be one of the listed values.

Example:

```JSON
{
  "sh:path": "schema:City",
  "sh:or": [
    {
      "sh:datatype": "xsd:string",
      "sh:in": [
        "Innsbruck",
        "Wien",
        "Salzburg"
      ]
    }
  ]
}
```

### 3.2.3.2. sh:hasValue

See [SHACL specification](https://www.w3.org/TR/shacl/#HasValueConstraintComponent).

`sh:hasValue` specifies the condition that each value in the given list is present as literal for the given data type.

The values in the list given by `sh:hasValue` have the same data type as the constrained literal (usable for all data types).

Example:

```JSON
{
  "sh:path": "schema:City",
  "sh:or":[
    {
      "sh:datatype": "xsd:string",
      "sh:hasValue": [
        "Innsbruck"
      ]
    }
  ]
}
```


### 3.3. Metadata

Following terms represent metadata about the given data type. These terms do not have any effects on the verification result; They have only informational character.

#### 3.3.1. sh:defaultValue

`sh:defaultValue` defines the standard value for the given data type.

The default value must have the respective datatype. 

```JSON
{
  "sh:datatype": "xsd:string",
  "sh:defaultValue": "Innsbruck"
}
```

```JSON
{
  "sh:datatype": "xsd:Integer",
  "sh:defaultValue": 42
}
```

#### 3.3.2. ds:defaultLanguage

`ds:defaultLanguage` defines the standard language tag for a given DataType node with `"sh:datatype": "rdf:langString"`.

The default language must be a valid [language tag](https://tools.ietf.org/html/bcp47).

```JSON
{
  "sh:datatype": "rdf:langString",
  "sh:defaultValue": "Vienna",
  "ds:defaultLanguage": "en"
}
```