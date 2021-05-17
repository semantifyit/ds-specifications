# VerificationReport

Verification Reports are given in JSON format and there are 3 different types of verification reports:

*   (Annotation Verification) General Verification of an Annotation based on following specifications:
    *   JSON
    *   JSON-LD
    *   Schema.org vocabulary
*   (Meta Verification) Verification of a Domain Specification based on following specifications:
    *   JSON
    *   Schema.org vocabulary
    *   Domain Specifications vocabulary
*   (Domain specific Verification) Domain specific Verification of a Schema.org annotation: Checks if a given Annotation is in compliance with all constraints defined in a given Domain Specification.

Errors for these verifications are specified in the following files:

* [Error List for the basic verification](./Basic-Verification.md)
* [Error List for the DS verification](./DS-Verification.md)
* [Error List for the DS-meta verification](./DS-Meta-Verification.md)
* [Error List and semantics for the schema.org verification](./SDO-Verification.md)

A Verification Report is used as a result document for any of these Verification types. It is also possible to create a Verification report for multiple types of Verifications at the same time, e.g. before a domain specific Verification can be executed, it should be checked if the input annotation is a valid JSON-LD document (If there is a lexical JSON error, the file can not be interpreted as an annotation, and consequently not be analysed during the domain specific Verification.)

Errors detected during any kind of Verification produce a corresponding error object. There is an error type for each Verification type:

Verification type | error @type |  description
---|----|---
Annotation Verification | AnnotationError | Error detected during the general Verification of the Annotation
Meta Verification | MetaError | Error detected during the Verification of the Domain Specification
Domain specific Verification | ComplianceError | Error detected during the Verification of the Annotation based on the Domain Specification

## Grammar

The grammar of Verification Reports consists of only 2 different node types. A **Verification Report Node**, that holds meta information about the overall verification outcome, and an **Error Entry Node**, that holds information about a single irregularity found. In the following the attributes (properties) for these grammar nodes are explained.

### Verification Report Node

#### validationResult (Required)

Has always a string as value. This string must be either `Valid`, `Invalid`, or `ValidWithWarnings`, depending on the outcome of the verification: The outcome is `Valid` if no errors were found. The outcome is `ValidWithWarnings` if only errors with the severity `Warning` were found. The outcome is `Invalid` if at least one error with the severity `Error` or `Critical` was found.

#### name (Optional)

Has always a string as value. The name/title for this verification report.

#### description (Optional)

Has always a string as value. The description for this verification report.

#### errors (Required)

Has always an Array of `Error Entry Nodes` as value. This array is empty if no errors were found.

### Error Entry Node

#### @type (Optional)

Has always a string as value. The value is either `ComplianceError`,`AnnotationError`, or `MetaError`, depending on the current verification process type (see table above).

#### severity (Required)

Has always a string as value. The value is either `Informational`, `Warning`, `Error`, or `Critical`, depending on the severity of the error.

#### errorCode (Required)

Has always an integer with the length of 3 as value. Error codes reflect specific categories for errors. Read more in the error lists linked above.

#### name (Required)

Has always a string as value. The name/title for this error.

#### description (Required)

Has always a string as value. The description for this error.

#### dsPath (Optional)

Has always a string as value. The path in the DS where an error was found (meta verification) or where the constraint for a found error was specified (compliance verification). The path value is a JSON-Path-like construct that points to a specific part within the Domain Specification.

#### annotationPath (Optional)

Has always a string as value. The path in the annotation where an error was found (compliance verification and schema.org verification). The path value is a JSON-Path-like construct that points to a specific part within the Domain Specification.
