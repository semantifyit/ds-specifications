# Error List for the domain-specific verification

The following errors are generated during the compliance check of a schema.org annotation/RDF-Graph based on the constraints defined by a Domain Specification.

## Error Codes for the DS verification (start with 5)

| ErrorCode | Name | Severity | Description |
| :---: | :---: | :---: | :--- |
| 500 | Generic compliance verification error | Any |Can be used as super-type for any error regarding the compliance verification |
| 501 | Non-conform target @type | Error | Annotation has a @type not specified by the DS - Applies to the @type of the root object |
| 502 | Non-conform property | Warning or Error | Annotation has a property that is not specified by the DS. The occurrence and severity of this error depends on the use of `sh:closed`: false -> no error. true -> error. not used -> warning |
| 503 | Missing property | Error | Annotation has a missing property that is defined as required by the DS |
| 504 | Non-conform cardinality | Error | Annotation has a property with a cardinality (amount of values), that is non-conform to the DS |
| 505 | Non-conform range | Error | Annotation has a property with a  @type/datatype that is non-conform to the DS |
| 506 | Non-conform enumeration value | Error | Property has an enumeration value that is non-conform to the DS |
| 511 | Non-conform sh:maxLength | Error | String value has length that is greater than allowed |
| 512 | Non-conform sh:minLength | Error | String value has length that is smaller than allowed |
| 513 | Non-conform sh:pattern | Error | String value does not match the specified Regex Pattern |
| 514 | Non-conform sh:languageIn | Error | String value does not use any of the specified language tags |
| 515 | Non-conform sh:uniqueLang | Error | Property has string values that use the same language tag |
| 521 | Non-conform sh:minExclusive | Error | Numeric value is smaller than allowed |
| 522 | Non-conform sh:minInclusive | Error | Numeric value is smaller than allowed |
| 523 | Non-conform sh:maxExclusive | Error | Numeric value is greater than allowed |
| 524 | Non-conform sh:maxInclusive | Error | Numeric value is greater than allowed |
| 531 | Non-conform sh:equals | Error | Value of property is not equal to the value of the other property specified |
| 532 | Non-conform sh:disjoint | Error | Value of property is equal to the value of the other property specified |
| 533 | Non-conform sh:lessThan | Error | Value of property is not smaller than the value of the other property specified |
| 534 | Non-conform sh:lessThanOrEquals | Error | Value of property is not smaller or equal than the value of the other property specified |
| 535 | Non-conform sh:in | Error | Value of property does not match any value of the given list |
| 536 | Non-conform sh:hasValue | Error | Value of property does not contain the given value |
| 537 | Non-conform ds:hasLanguage | Error | Value of property does not use the given language |

## Semantics for Class Matching

In the following our understanding of the class-subclass relationship is explained.This relationship is important to determine valid matches for `sh:targetClass` / `sh:class` and `ds:subDSOf`. 

A set of classes 'SubDS' is a **valid match** for another set of classes 'SuperDS' if all elements in 'SuperDS' are also present in a set that contains all elements from 'SubDS' along with all their superclasses:

`classes(SuperDS) ⊆ superclasses(classes(SubDS))`

The following table gives examples for this.

|                 | Domain Specification      | Data                             | Match |
|-----------------|---------------------------|----------------------------------------|-------|
|                 | sh:targetClass / sh:class | @type                                  |       |
| exact match     | LodgingBusiness           | LodgingBusiness                        | yes   |
| additional type | LodgingBusiness           | LodgingBusiness, Product               | yes   |
| sub-type        | LodgingBusiness           | Motel                                  | yes   |
| no relation     | LodgingBusiness           | CreativeWork                           | no    |
| exact match     | LodgingBusiness, Product  | LodgingBusiness, Product               | yes   |
| additional type | LodgingBusiness, Product  | LodgingBusiness, Product, CreativeWork | yes   |
| sub-type        | LodgingBusiness, Product  | Hotel, Product                         | yes   |
| sub-type | Organization, Place  | Restaurant                         | yes   |
| not all types   | LodgingBusiness, Product  | LodgingBusiness                        | no    |
| no relation     | LodgingBusiness, Product  | CreativeWork                           | no    |
