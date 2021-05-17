# Verification Report Node

A Verification Report is a structured document that represents the outcome of a verification process (see [details about the different verification processes](./README.md)). The corresponding grammar node is called Verification Report Node. Such a verification Report Node holds metadata about the verification outcome and a list of [Error Entry Nodes](./ErrorEntry.md) with details about the found errors.

## 1. Example

```json
{
  "@context": {
    "schema": "https://schema.org/",
    "ds": "https://vocab.sti2.at/ds/",
    "sh": "http://www.w3.org/ns/shacl#",
    "ds:verificationResult": {
      "@type": "@id"
    },
    "ds:usedDomainSpecification": {
      "@type": "@id"
    },
    "ds:severity": {
      "@type": "@id"
    }
  },
  "@type": "ds:VerificationReport",
  "ds:verificationResult": "ds:Invalid",
  "ds:usedDomainSpecification": "https://semantify.it/ds/OBbzsh4_B",
  "schema:name": "Example verification report",
  "schema:description": "This verification report reflects the outcome for a domain specific verification of an RDF-graph.",
  "ds:error": [
    ...
  ]
}
```

## 2. Key-value table

The following table lists all possible terms that can be used by a Verification Report Node. The order in the table reflects the recommended order of these terms within a Verification Report Node (optional).

| key | required | value type | description |
| :---: | :---: | :---: | :--- |
| `@type` | true | `"ds:VerificationReport"` | The fixed type for a Verification Report Node |
| `ds:verificationResult` | true | *IRI* | The overall verification outcome as IRI (there are enumerations for this) | 
| `ds:usedDomainSpecification` | false | *IRI* | The IRI of the domain specification that was used for the domain specific verification (if the verification report was created for a domain specific verification) | 
| `schema:name` | false | String | The name of the Verification Report |
| `schema:description` | false |  String | The description of the Verification Report |
| `ds:error` | true |  List of **PropertyNode** | A list of Error Entry Nodes that reflects the encountered errors during the verification |


## 3. Semantics

### 3.1. ds:verificationResult

`ds:verificationResult` has an IRI as value. This IRI represents the overall verification outcome (also called "Verification Validity"), and depends on the found errors. In the following the possible IRIs are listed:

* `ds:Valid` - The outcome is valid if no errors were found.
* `ds:ValidWithWarnings` - The outcome is `Valid with Warnings` if only errors with the severity `Warning` were found.
* `ds:Invalid` - The outcome is `Invalid` if at least one error with the severity `Error` or `Critical` was found.

### 3.2. ds:error

`ds:error` has always an array of [Error Entry Nodes](./ErrorEntry.md) as value. This array is empty if no errors were found (so that the term `ds:error` is always present).
