# Vocabularies

## Creating Vocabularies

Tips and guidelines on how to create vocabularies.

### Tips ###

* STI2 publishes vocabularies at http://vocab.sti2.at/
* Use popular formats, e.g. Turtle, JSON-LD.
* Reuse as much as possible (from the most popular vocabularies if possible), e.g. rdf, rdfs, xsd.
* For JSON-LD: Make clever use of the @context to make your vocabulary more readable.
* A vocabulary should have a context/namespace with an IRI, that is also a real URL. It should show the latest version.
* A vocabulary should have an @id with an IRI, that is also a real URL, for the vocabulary version.

### Links ###

* http://vocab.sti2.at/
* https://www.w3.org/TR/rdf-schema/
* https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/
* https://www.w3.org/TR/2014/REC-rdf11-mt-20140225/
* https://www.w3.org/TR/rdf11-primer/
* https://www.w3.org/TR/json-ld/
* https://json-ld.org/playground/
* https://terms.tdwg.org/wiki/W3C_RDF_Vocabulary_Guidelines
* https://www.w3.org/egov/wiki/Tutorial/RDF_Vocabularies#Creating_Vocabularies

### Relation to Schema.org

* Syntax used by Schema.org

* Enumeration modeling in Schema.org

## General Information about schema.org enumerations ##

As we know schema.org is not free of flaws. One of its biggest downsides is the missing conformance regarding enumerations, their data model within schema.org, and how to use them in annotations. In the following we list our findings and how we should proceed with enumerations in our tools.

### General ###

[Interesting blogpost about enumerations]([https://blog.eyas.sh/2019/05/schema-org-enumerations-in-typescript/)

### Data Model ###

There are 3 different data models for enumerations in schema.org:

1. Define an enumeration as such (using https://schema.org/Enumeration as (direct or indirect) parent type), and defining all corresponding enumeration members as such (using the @id of the corresponding enumeration as @type for the enumeration members). e.g. https://schema.org/DayOfWeek with all days of the week defined as enumeration members.
2. Link to an external enumeration in the property description where the enumeration is supposed to be used, but without specifying the enumeration as such. In that case, the more general data type "Text" is used as a range for the property. e.g. the property https://schema.org/addressCountry states that two-letter country codes from the external list ISO 3166-1 alpha-241 can be used as a value.
3. Define an enumeration as such (using https://schema.org/Enumeration as (direct or indirect) parent type), but without defining the enumeration members of that enumeration as part of Schema.org. Instead, link in the description to an external list or vocabulary containing the possible enumeration members. e.g. https://schema.org/PaymentMethod is an enumeration without enumeration members. However, the description lists a set of commonly used values for this enumeration (which are not part of Schema.org, and leaving open if there are more possible values), e.g. http://purl.org/goodrelations/v1#DirectDebit

For enumerations of the third type there could also be additional properties!
Consider that "Enumeration" is a sub-type of "Thing", and therefore it could be considered as type like any other (see schema:CreditCard).

Clearly, schema.org should fix this data model, having enumerations only of the first type.

### How enumerations are used ###

Enumerations are used in annotations through references (URIs) of their instances. In practice they are given as **"URI String"**:

`"paymentStatus": "http://schema.org/PaymentComplete"`

or as "**String**":

`"paymentStatus": "PaymentComplete"`

But the range of an enumeration property isn't Text but an enumeration class. Thus to be correct (based on the specifications used in schema.org) you'd have to use an instance of that Enumeration class when using an enum property, eg:

```json
"paymentStatus": {
    "@id": "PaymentComplete",
    "@type": "PaymentStatusType",
    "rdfs:comment": "The payment has been received and processed.",
    "rdfs:label": "PaymentComplete"
}
```

But enumeration values don't have to be specified that way in practice, since the referencing of those backed-in enumeration values suffice. This is done through the use of @id:

```json
"paymentStatus": {
    "@id": "PaymentComplete"
}
```

To make the use of plain strings as values work the following is expected: the annotation includes the @context of schema.org, and therefore any agent "understanding" the annotation fetches this context and resolves (e.g. JSON-LD context resolving) the strings for enumeration values to "**URIs**"

This would be in a perfect world, but hardly any tool really fetches the schema.org context, nor is this context "correct" (missing @type: @id for example). Most tools/actors think that enumeration values are just plain strings, this is at least what seems to be expected from Google in their guidelines.

### How we use enumerations ###

We should be consistent in our usage of enumerations throughout all our interfaces. We "assume" that the schema.org context is correct and expect enumerations to be  **"URI String"** (e.g. `"http://schema.org/PaymentComplete"`) in the best case. But we have different use cases:

#### Domain Specification ####

Here we want to be as compliant to SHACL and schema.org as possible, therefore we leave the information that these enumeration values are URIs (so they affected by JSON-LD transformations and @context definitions, instead of being plain strings):

```json
"sh:in": [
    {
      "@id": "schema:Wednesday"
    }
]
```

#### Annotation Generation ####

For this we generate enumeration values given as **"URI String"**, since it is the most usual format in the wild:

```json
"paymentStatus": "http://schema.org/PaymentComplete"
```

#### Annotation Verification ####

The verification should accept the different types of writing enumeration values that are possible:

As "**String**":

`"paymentStatus": "PaymentComplete"`

As **"URI String"** with http(s):

`"paymentStatus": "http://schema.org/PaymentComplete"`

`"paymentStatus": "https://schema.org/PaymentComplete"`

As **"URI"** with different context variations resulting in absolute URIs with http(s)://schema.org/, e.g.:

```json
//these different possibilities could all be correct, depending on the used @context
"paymentStatus": {
   "@id": "Wednesday"
},
"schema:paymentStatus": {
   "@id": "schema:Wednesday"
},
"paymentStatus": {
   "@id": "http://schema.org/PaymentComplete"
},
"paymentStatus": {
   "@id": "https://schema.org/PaymentComplete"
},
"http://schema.org/paymentStatus": {
   "@id": "http://schema.org/PaymentComplete"
}
```