# Error List for the DS-based verification

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