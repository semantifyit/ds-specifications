### Error Entry Node

An Error Entry Node represents the information about a found error during a verification process.

## 1. Example

```json
{
  "@type": "ds:ComplianceError",
  "ds:severity": "ds:ErrorSeverity",
  "ds:errorCode": 511,
  "schema:name": "Non-conform sh:maxLength",
  "schema:description": "The string value has a length (31) that is greater than allowed (20) by the domain specification.",
  "sh:value": "this is the value from the data",
  "ds:dsPath": ["https://example.com/entity1234", "https://schema.org/name"],
  "ds:dataPath": "$.schema:name"
}
```

## 2. Key-value table

The following table lists all possible terms that can be used by an Error Entry Node. The order in the table reflects the recommended order of these terms within an Error Entry Node (optional).

| key | required | value type | description |
| :---: | :---: | :---: | :--- |
| `@type` | true | IRI for the error type | The type of the error. `ds:Error` is the default type, but there are also more specific types (see semantics below) |

## 3. Semantics

### 3.1. Error type

There is a `@type` for an Error Entry Node, which is `ds:Error`. There are several sub-classes for `ds:Error`, which can be used to further categorize the errors depending on their characteristics:

* `ds:AnnotationError` - Error is related to the verification based on the schema.org vocabulary.
* `ds:ComplianceError` - Error is related to the domain-specific verification.
* `ds:MetaError` - Error is related to the meta-verification of a Domain Specification (missing compliance to the DS specification).
* `ds:ExecutionError` - Error is related to unexpected behaviour during a verification process.
* `ds:JsonError` - Error is related to the missing compliance to the JSON specification.
* `ds:JsonLdError` - Error is related to the missing compliance to the JSON-LD specification.

### 3.2. Error code

`ds:errorCode` always an integer with the length of 3 as value. Error codes reflect specific categories for errors (like HTTP status codes). The existing error codes available for the specific verification processes are listed in the corresponding documents, see details in [README.md](./README.md).

### 3.3. Error severity

`ds:severity` has always an IRI as value. This IRI represents the severity of the found error. In the following the possible IRIs are listed:

* `ds:CriticalSeverity` - The error entry is severe (e.g. if it results in the verification being halted to avoid a process error).
* `ds:ErrorSeverity` - The standard error severity.
* `ds:WarningSeverity` - The error entry reflects a light error (e.g. if another variation is recommended).
* `ds:InformationalSeverity` - The error entry is not so much an error but a noticeable finding that is being reported (e.g. operational tweaks needed for the verification of the given data instance).

### 3.4. Error pointer

`ds:dsPath` and `ds:dataPath` represent pointers to the source of the found error in the DS and/or the data, respectively. The following semantics can be changed based on the verification use case and available data formats.

`ds:dsPath` has a string as value. The path in the DS where an error was found (meta verification) or where the constraint for a found error was specified (compliance verification). The path value is a JSON-Path-like construct that points to a specific part within the Domain Specification. For DS-V7, the used DS-Path syntax is presented in [a dedicated document](./../DsPath/README.md).

`ds:dataPath` has a string as value. The path in the given data where an error was found (compliance verification and schema.org verification). The path value is a JSON-Path-like construct that points to a specific part within the given data.
