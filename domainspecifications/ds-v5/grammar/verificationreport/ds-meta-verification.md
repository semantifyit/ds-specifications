# Error List for the Meta verification of DS \(unfinished\)

The Meta verification checks if a given DS complies to the DS Specification itself.

This List of errors is unfinished, since new error types are needed to express irregularities regarding new introduced terms for Domain Specifications in DS-V5. This has not been done yet because the meta-verification has had a low priority so far \(there is no implementation of a DS-Meta-Verification so far\).

If someone wants to create a DS-Meta-Verification tool, they can use the error code 400 for any error type that is not in this list yet.

## Error Codes for the Meta verification of DS \(start with 4\)

| ErrorCode | Name | Severity | Description |
| :---: | :---: | :---: | :--- |
| 400 | Generic meta verification error | Can be used as super-type for any error regarding the meta verification |  |
| 401 | Missing DS @type | Critical | DS has a missing @type definition |
| 402 | Missing PropertyShape @type | Warning | PropertyShape has a missing @type definition |
| 403 | Missing NodeShape @type | Warning | NodeShape has a missing @type definition |
| 404 | Missing @id | Error | Node has a missing @id definition |
| 405 | Missing sh:targetClass | Error | DS has a missing sh:targetClass definition |
| 406 | Missing sh:property | Error | Node has a missing sh:property definition |
| 407 | Missing sh:path | Error | PropertyShape has a missing sh:path definition |
| 408 | Missing sh:minCount | Error | PropertyShape has a missing sh:minCount definition |
| 409 | Missing sh:maxCount | Error | PropertyShape has a missing sh:maxCount definition |
| 410 | Missing property range | Error | PropertyShape has no property range definitions |
| 411 | Missing sh:class | Error | Node has a missing sh:class definition |
| 412 | Missing sh:datatype | Error | Node has a missing sh:datatype definition |
| 413 | Missing enumeration | Error | Node has a missing enumeration definition |
| 414 | Missing enumeration value | Error | Node has a missing enumeration value definition |
| 415 | Missing sh:node | Error | Node has a missing sh:node definition |
| 421 | Invalid DS @type | Critical | DS has an invalid @type definition |
| 422 | Invalid PropertyShape @type | Error | PropertyShape has an invalid @type definition |
| 423 | Invalid NodeShape @type | Error | NodeShape has an invalid @type definition |
| 424 | Invalid @id | Error | Node has an invalid @id definition |
| 425 | Invalid sh:targetClass | Error | DS has an invalid sh:targetClass definition |
| 426 | Invalid sh:property | Error | Node has an invalid sh:property definition |
| 427 | Invalid sh:path | Error | PropertyShape has an invalid sh:path definition |
| 428 | Invalid sh:minCount | Error | PropertyShape has an invalid sh:minCount definition |
| 429 | Invalid sh:maxCount | Error | PropertyShape has an invalid sh:maxCount definition |
| 430 | Invalid property range | Error | PropertyShape has invalid property range definitions |
| 431 | Invalid sh:class | Error | Node has an invalid sh:class definition |
| 432 | Invalid sh:datatype | Error | Node has an invalid sh:datatype definition |
| 433 | Invalid enumeration | Error | Node has an invalid enumeration definition |
| 434 | Invalid enumeration value | Error | Node has an invalid enumeration value definition |
| 435 | Invalid sh:node | Error | Node has an invalid sh:node definition |

