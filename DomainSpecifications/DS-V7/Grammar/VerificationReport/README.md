# Verification Report

A Verification Report is a structured document that represents the outcome of a verification process. In the following, a formal grammar and vocabulary terms are given for the JSON-LD serialization of a verification report. However, a simplified version (JSON) of verification reports is also possible, e.g. if the semantic structure of a JSON-LD is not needed. In that case, the simplified version should contain the key-characteristics of a verification report (e.g. overall outcome, errors with their type, severity and error cause).

## 1. Grammar

From a grammar point of view, a verification report consists of a [Verification Report Node](./VerificationReport.md), which holds metadata, the overall outcome of the verification, and a list of [Error Entry Nodes](./ErrorEntry.md) with details about the found errors.

## 2. Verification processes

The content for the error entries depends on the verification process covered. There are 3 different types of verification processes, for which detailed error lists exist:

*   **Domain specific Verification** - Checks if a given Schema.org annotation/RDF-Graph is in compliance with all constraints defined in a given Domain Specification. The [error list for the DS verification](./DS-Verification.md) specifies the possible errors based on **DS-V7**.
*   **Meta Verification** - Verification of a Domain Specification based the DS specification **DS-V7** and the used vocabularies (valid content of the DS). The [error list for the DS-meta verification](./DS-Meta-Verification.md) specifies the possible errors based on **DS-V7**.
*   **Annotation Verification** - General Verification of a Schema.org annotation/RDF-Graph based on the Schema.org vocabulary. The details about this verification process are not part of the DS specification per se, but are covered in the [Verification of schema.org annotations](./../../../../SDO-Verification/README.md).

All these verification processes share certain error types, which are couple to the JSON specification, the JSON-LD specification, or generic causes (like an execution error). The checking for these basics is called [basic verification](./Basic-Verification.md).
