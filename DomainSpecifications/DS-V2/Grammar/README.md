# DS-V2 - Grammar

The grammar for DS-V2 is given in [BNF](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form). The grammar consists of different node-types, that are used to construct a Domain Specification. A Domain Specification is a tree-shaped JSON, like the annotations it should represent.

## Content

* Node-types of this grammar (below)
* [The grammar and semantics of Domain Specifications](DomainSpecification/README.md)
  * [Examples for grammar nodes](./DomainSpecification/Examples)
  * [The grammar and semantics of Rules](Rules/README.md)  
  * [The grammar and semantics of Numeric Patterns](NumericPattern/README.md)  
* [The grammar and semantics of Verification Reports](VerificationReport/README.md)
    * [The grammar and semantics of Errors](./VerificationReport/Error.md)
    * [List of Error Codes](./VerificationReport/ErrorCodes.md)


## Node types of the DS grammar

Each node object must have a $type indicating the type of the Node. This helps to know which node is handled AND improves the readability of the structure, little trade-off is the extra property on each node.

Possible Node types:

*   DomainSpecification - Root node of a DS
*   RestrictedClass - A type(class) from SDO with syntactic restrictions (properties are restricted)
*   Class - A type(class) from SDO without syntactic restrictions
*   RestrictedProperty - A property from SDO with syntactic restrictions (range is restricted)
*   Property - A property from SDO without syntactic restrictions
*   DataType - A data type from SDO
*   RestrictedEnumeration - An enumeration from SDO with syntactic restrictions (possible values are restricted)
*   Enumeration - An enumeration from SDO without syntactic restrictions
*   EnumerationValue - An enumeration value from SDO (must be stated in the vocabulary as an enumeration value)
*   CustomEnumerationValue - A custom enumeration, that is not stated in the vocabulary as an enumeration value

CustomEnumerationValue is a new concept that should bridge the broken enumeration modelling of Schema.org. It should help to define valid enumeration values, although they are not part of the Schema.org vocabulary.
This is NECESSARY since the goodrelations features were included in the SDO vocabulary, but didn't match the enumeration modelling. Most notably: the expected/allowed enumeration values are not stated as such in the vocabulary, making it hard to list/validate the expected values for that enumeration (ironically, that is what enumerations are for). Possible enumeration values are sometimes stated as an example in the description of the enumeration, at most. See https://schema.org/PaymentMethod

Rules have also their own types:

*   TextRule, applies only to text values according to SDO data types (Text, URL)
*   BooleanRule, applies only to boolean values according to SDO data types (Boolean)
*   DateRule, applies only to date values according to SDO data types (Date)
*   TimeRule, applies only to Time values according to SDO data types (Time)
*   DateTimeRule, applies only to DateTime values according to SDO data types (DateTime)
*   NumberRule, applies only to numeric values according to SDO data types (Number, Integer, Float)
*   ComplexRule, applies as a logical connector between rules/checks




