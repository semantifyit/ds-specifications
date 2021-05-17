# Error List for the Meta verification of DS (unfinished)

The Meta verification checks if a given DS complies to the DS Specification itself.

The meta-verification has had a low priority so far because there is no implementation, or a plan for it in the near future. The list of error types is very shallow for this reason.

If someone wants to create a DS-Meta-Verification tool, they can use the error code 400 for any error type that is not in this list yet.

## Error Codes for the Meta verification of DS (start with 4)

| ErrorCode | Name | Severity | Description |
| :---: | :---: | :---: | :--- |
| 400 | Generic meta verification error | Can be used for any error regarding the meta verification |
| 401 | Unknown term | Warning | The current Node uses a term that is not expected |
| 402 | Missing term | Error | The current Node misses a term that is mandatory |
| 403 | Invalid term value (Syntax) | Error | The current Node uses a term with a value data type that is invalid for that term |
| 404 | Invalid term value (Semantic) | Error | The current Node uses a term with a value that is semantically invalid for that term |

Error 404 could be e.g. sh:minCount is higher than the value of sh:maxCount

