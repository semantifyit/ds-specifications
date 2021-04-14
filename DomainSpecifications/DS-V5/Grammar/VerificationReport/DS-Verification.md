# Error List for the DS-based verification

Error codes have been introduced for the new advanced constraint components in DS-V5 (error codes 511 - 536).

## Error Codes for the DS verification (start with 5)

| ErrorCode | Name | Severity | Description |
| :---: | :---: | :---: | :--- |
| 500 | Generic compliance verification error | Error |Can be used as super-type for any error regarding the compliance verification |
| 501 | Non-conform target @type | Critical | Annotation has a @type not specified by the DS - Applies to the @type of the root object |
| 502 | Non-conform property | Warning | Annotation has a property that is not specified by the DS |
| 503 | Missing property | Error | Annotation has a missing property that is defined as required by the DS |
| 504 | Non-conform cardinality | Error | Annotation has a property with a cardinality (amount of values), that is non-conform to the DS |
| 505 | Non-conform range | Error | Annotation has a property with a  @type/datatype that is non-conform to the DS |
| 506 | Non-conform enumeration value | Error | Property has an enumeration value that is non-conform to the DS |
| 511 | Non-conform sh:maxLength | Error | String value has length that is bigger than allowed |
| 512 | Non-conform sh:minLength | Error | String value has length that is smaller than allowed |
| 513 | Non-conform sh:pattern | Error | String value does not match the specified Regex Pattern |
| 514 | Non-conform sh:languageIn | Error | String value does not use any of the specified language tags |
| 515 | Non-conform sh:uniqueLang | Error | Property has string values that use the same language tag |
| 521 | Non-conform sh:minExclusive | Error | Numeric value is smaller than allowed |
| 522 | Non-conform sh:minInclusive | Error | Numeric value is smaller than allowed |
| 523 | Non-conform sh:maxExclusive | Error | Numeric value is bigger than allowed |
| 524 | Non-conform sh:maxInclusive | Error | Numeric value is bigger than allowed |
| 531 | Non-conform sh:equals | Error | Value of property is not equal to the value of the other property specified |
| 532 | Non-conform sh:disjoint | Error | Value of property is equal to the value of the other property specified |
| 533 | Non-conform sh:lessThan | Error | Value of property is not smaller than the value of the other property specified |
| 534 | Non-conform sh:lessThanOrEquals | Error | Value of property is not smaller or equal than the value of the other property specified |
| 535 | Non-conform sh:in | Error | Value of property does not match any value of the given list |
| 536 | Non-conform sh:hasValue | Error | Value of property does not contain the given value |

## Semantics for Class Matching

Regarding the matching of sh:targetClass/sh:class and the @type of an entity:

|                 | Domain Specification      | Annotation                             | Match |
|-----------------|---------------------------|----------------------------------------|-------|
|                 | sh:targetClass / sh:class | @type                                  |       |
| exact match     | LodgingBusiness           | LodgingBusiness                        | yes   |
| additional type | LodgingBusiness           | LodgingBusiness, Product               | no    |
| sub-type        | LodgingBusiness           | Motel                                  | yes   |
| no relation     | LodgingBusiness           | CreativeWork                           | no    |
| exact match     | LodgingBusiness, Product  | LodgingBusiness, Product               | yes   |
| additional type | LodgingBusiness, Product  | LodgingBusiness, Product, CreativeWork | no    |
| sub-type        | LodgingBusiness, Product  | Hotel, Product                         | yes   |
| not all types   | LodgingBusiness, Product  | LodgingBusiness                        | no    |
| no relation     | LodgingBusiness, Product  | CreativeWork                           | no    |