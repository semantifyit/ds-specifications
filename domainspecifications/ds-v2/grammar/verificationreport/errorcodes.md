# Error codes for Verification Reports

Error codes are inspired by HTML status codes.

## 100 - JSON related errors

The errors in this category are based on the JSON specification, and on requirements how the input JSON is expected. These errors can apply on Annotations during the Annotation Validation, or on Domain Specifications during the Meta Validation.

| Code | Error field | Name | Severity | Description |
| :--- | :--- | :--- | :--- | :--- |
| 100 | JSON | JSON related Error | Critical | Generic JSON related error. |
| 101 | Lexical - JSON | No JSON | Critical | The input is not valid JSON. - string that can not be represented as json |
| 102 | Lexical - JSON | No Data | Critical | The input is empty - null, empty object, "", undefined, \[\] |
| 103 | Syntax - JSON | No Object | Critical | The input is not a JSON object - input should be an JSON object, and not an array of annotations, or something else |

## 200 - JSON-LD related errors

The errors in this category are based on the JSON-LD specification, and on requirements how the input JSON-LD is expected. These errors can apply on Annotations during the Annotation Validation.

| Code | Error field | Name | Severity | Description |
| :--- | :--- | :--- | :--- | :--- |
| 200 | JSON-LD | JSON-LD related Error | Error | Generic JSON-LD related error. |
| 201 | Syntax - JSON-LD | No @context | Critical | The input has no @context |
| 202 | Semantic - JSON-LD | Bad @context | Error | The input has an invalid @context. Must be something in different syntax possibilities, only one @context in whole document expected. |
| 203 | Syntax - JSON-LD | No @type | Critical | Object misses a @type property. |
| 204 | Semantic - JSON-LD | Bad @type | Error | Object has an invalid @type - must be string or array of strings |
| 205 | Sintax - JSON-LD | Double Nested Array | Error | The input contains a double nested array, which is not conform to the JSON-LD Specification. |
| 206 | Semantic - JSON-LD | Usage of null | Warning | Usage of null as value - The use of the null value within JSON-LD is used to ignore or reset values. null has the same meaning as if the dictionary member was not defined. |
| 207 | Semantic - JSON-LD | Usage of undefined | Error | Usage of undefined as value - Not valid in JSON-LD |

## 300 - Schema.org related errors

The errors in this category are based on the Schema.org vocabulary. These errors can apply on Annotations during the Annotation Validation.

| Code | Error field | Name | Severity | Description |
| :--- | :--- | :--- | :--- | :--- |
| 300 | SDO | Schema.org related Error | Error | Generic Schema.org related error. |
| 301 | Semantic - SDO | Nonconform @context | Error | Used @context must be schema.org |
| 302 | Semantic - SDO | Nonconform @type | Error | Used @type is nonconform to Schema.org |
| 303 | Semantic - SDO | Nonconform property | Error | Used property in nonconform to Schema.org |
| 304 | Semantic - SDO | Bad action property | Error | Used action property \(input- output-\) has a value that is not a string. |
| 305 | Semantic - SDO | Nonconform domain | Error | The input has a property that it is not allowed to use for the @type |
| 306 | Semantic - SDO | Nonconform range | Error | The input has a property with a value data type that is not conform to Schema.org \(also for datatypes\) |
| 307 | Semantic - SDO | Unexpected string value | Warning | Property uses a string as value, instead of a datatype stated as range. |
| 308 | Semantic - SDO | Bad enumeration | Warning | The input has an enumeration value that is not conform to schema.org - must be an url stated as enumeration value |

## 400 - Domain Specification related Errors

The errors in this category are based on the Domain Specification vocabulary/data model. These errors can apply on Domain Specifications during the Meta Validation.

| Code | Error field | Name | Severity | Description |
| :--- | :--- | :--- | :--- | :--- |
| 400 | DomainSpecification | Domain Specification related Error | Error | Generic Domain Specification related error. |
| 401 | Syntax - DomainSpecification | No $type | Error | Specification Node has no $type property. |
| 402 | Semantic - DomainSpecification | Bad $type | Error | Specification Node has invalid $type value \(must be string from allowed types\) |
| 403 | Syntax - DomainSpecification | Nonconform node range | Error | Specification Node has a $type that is a not conform range according to the DS grammar. |
| 404 | Syntax - DomainSpecification | Missing property | Error | Required property not found. Specification Node misses a property that is required. |
| 405 | Syntax - DomainSpecification | Unknown property | Warning | Specification Node has an unknown property \(not assigned to an extension\) |
| 406 | Syntax - DomainSpecification | Nonconform range | Error | Property has a value \(data type or value\) that is not conform to the DS grammar. |
| 407 | Semantic - DomainSpecification | Bad $SDOversion | Error | URI of schema.org vocabulary is invalid. |
| 408 | Semantic - DomainSpecification | Bad $schema | Error | URI of the DomainSpecification grammar is invalid. |
| 409 | Semantic - DomainSpecification | Bad $ref | Error | Reference object has an invalid or undefinied value \(JSON pointer must start with "/definitions/" and exist in the DomainSpecification\) |
| 410 | Semantic - DomainSpecification | Unknown Schema.org class | Error | Invalid Schema.org Class reference |
| 411 | Semantic - DomainSpecification | Unknown Schema.org property | Error | Invalid Schema.org Property reference |
| 412 | Semantic - DomainSpecification | Unknown Schema.org data type | Error | Invalid Schema.org DataType reference |
| 413 | Semantic - DomainSpecification | Unknown Schema.org enumeration | Error | Invalid Schema.org Enumeration reference |
| 414 | Semantic - DomainSpecification | Unknown Schema.org enumeration value | Error | Invalid Schema.org EnumerationValue reference |
| 415 | Semantic - DomainSpecification | Unknown $rule type | Error | Unknown $rule type - rule name is unknown |
| 416 | Semantic - DomainSpecification | Nonconform $rule type | Error | $rule value is not valid for the $type of the rule |
| 417 | Semantic - DomainSpecification | Nonconform parameter | Error | The parameter has an invalid data type for the $rule |

## 500 - Compliance related Errors

The errors in this category are based on the Schema.org vocabulary. These errors can apply on Annotations during the Annotation Validation.

| Code | Error field | Name | Severity | Description |
| :--- | :--- | :--- | :--- | :--- |
| 500 | Domain specific | DS Compliance related Error | Error | Generic Compliance related error. |
| 501 | Syntax - Domain specific | Nonconform @type | Error | Annotation has a @type not specified by the DS. - Applies to the @type of the root object |
| 502 | Syntax - Domain specific | Nonconform property | Warning | Annotation has a property that is not specified by the DS. |
| 503 | Syntax - Domain specific | Missing property | Error | Annotation misses a property that is required by the DS. |
| 504 | Syntax - Domain specific | Nonconform property cardinality | Error | Annotation has a property with a cardinality \(amount of values\), that is nonconform to the DS. |
| 505 | Syntax - Domain specific | Nonconform range | Error | Property has a value with a @type/DataType that is nonconform to the DS. |
| 506 | Syntax - Domain specific | Nonconform property uniqueness | Error | Property has values that are not unique. - \("uniqueValues": true\) |
| 507 | Syntax - Domain specific | Nonconform enumeration value | Error | Property has an enumeration value that is nonconform to the DS. |
| 550 | Semantic - Domain specific | DS Rule Compliance related Error | Error | Generic Compliance of semantic rule related error. |
| 551 | Semantic - Domain specific | Nonexistent $path | Informal | A $path object for a rule can not be resolved. Rule can not be checked. |
| 552 | Semantic - Domain specific | TextRule violation | Error | A TextRule was violated. |
| 553 | Semantic - Domain specific | BooleanRule violation | Error | A BooleanRule was violated. |
| 554 | Semantic - Domain specific | DateRule violation | Error | A DateRule was violated. |
| 555 | Semantic - Domain specific | TimeRule violation | Error | A TimeRule was violated. |
| 556 | Semantic - Domain specific | DateTimeRule violation | Error | A DateTimeRule was violated. |
| 557 | Semantic - Domain specific | NumberRule violation | Error | A NumberRule was violated. |
| 558 | Semantic - Domain specific | ComplexRule violation | Error | A ComplexRule was violated. |

