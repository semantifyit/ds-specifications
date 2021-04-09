# Terms used in Domain Specifications

This is a list containing the term URIs used for Domain Specifications in DS-V5. The standard `@context` for DS is assumed \(see [Context.md](context.md)\).

## Meta Data describing Components

Following components give additional non-validating/constraining information about the DS.

**schema:name** - `String` - The Name of the DS.

**schema:description** - `String` - The Description of the DS.

**schema:author** - `schema:Person` - The Author of the DS.

**schema:version** - `String` - The version of this particular DS.

**schema:schemaVersion** - `String` - The used Schema.org version of the DS as URI.

**rdfs:comment** - `String` - The Description/Justification for a Property node.

**sh:order** - `Integer` - The position of a Property in comparison with other properties for the same Subject \(only representation purpose\).

## External vocabularies

**ds:usedVocabularies** - `[IRI]` - The used external vocabularies \(besides schema.org\) for this DS. The values are IRIs of those vocabularies \(it is assumed that the vocabulary file can be retrieved with the given IRI\).

## Value Type Constraint Components

**sh:class** - `IRI` \| `[IRI]` - The Target value must have the specified IRI\(s\) as Type\(s\) or have be a valid instance for the specified IRI \(Enumerations\).

**sh:datatype** - `IRI` - The Target Literal must have the specified IRI as Datatype.

## Other structure-related components

**sh:path** - `IRI` - A property node must have `sh:path` to signalize the property it is constraining.

**sh:targetClass** - `IRI` \| `[IRI]` - This is the entry-point for the verification, the link between the root node of a DS and the root node of the data to verify.

## Cardinality Constraint Components

**sh:minCount** - `Integer` - The minimum cardinality \(amount of values\) for a property.

**sh:maxCount** - `Integer` - The maximum cardinality \(amount of values\) for a property.

## Shape-based Constraint Components

**sh:node** - `node shape` - The value node conforms to the given Node Shape.

**sh:property** - `property shape` - The value node conforms to the given Property Shape.

## Logical Constraint Components

**sh:or** - `[node shape]` - The value node conforms at least one of the given Shapes. This is used only for ranges of properties.

## Advanced Constraint Components

These are advanced constraints, their use depends on the position/datatype of the target. See [https://docs.google.com/spreadsheets/d/1AFD21lB\_zQkns\_3aeDLIFW1E-C36eJ3DogcnOb2vD7g/edit\#gid=1009977856](https://docs.google.com/spreadsheets/d/1AFD21lB_zQkns_3aeDLIFW1E-C36eJ3DogcnOb2vD7g/edit#gid=1009977856)

### Value Range Constraint Components

SHACL reference: [https://www.w3.org/TR/shacl/\#core-components-range](https://www.w3.org/TR/shacl/#core-components-range)

These constraint components dictate a value range that the value of a literal value must have. The datatype for this value must be the samea s the datatype of the constrained literal. Datatypes that are allowed to have these constraints are: Date, DateTime, Time, Number, Float, and Integer.

**sh:minExclusive** - `same as constrained Data Type Node` - The minimum exclusive value that the value must have.

**sh:minInclusive** - `same as constrained Data Type Node` - The minimum inclusive value that the value must have.

**sh:maxExclusive** - `same as constrained Data Type Node` - The maximum exclusive value that the value must have.

**sh:maxInclusive** - `same as constrained Data Type Node` - The maximum inclusive value that the value must have.

### String based Constraint Components

SHACL reference: [https://www.w3.org/TR/shacl/\#core-components-string](https://www.w3.org/TR/shacl/#core-components-string)

These constraint components are meant for literals having the datatype string \(and their sub-datatypes in some cases\).

**sh:maxLength** - `Integer` - The maximum allowed string length of a literal \(String or URL\)

**sh:minLength** - `Integer` - The minimum allowed string length of a literal \(String or URL\)

**sh:pattern** - `[Regex]` - A regular expression that the literal must match \(String or URL\)

**sh:languageIn** - `[Language-Tag]` - The literal must use a language tag given in the list of language tags \([https://tools.ietf.org/html/bcp47](https://tools.ietf.org/html/bcp47)\) . Only for String values \(in theory the exact datatype of the target literal is `rdf:langString`, but due to our Schema.org &lt;-&gt; XSD mapping we use xsd:string and allow a language tag for it\)

**sh:uniqueLang** - `Boolean` - The property `sh:uniqueLang` can be set to true to specify that no pair of value nodes may use the same language tag. \(Array of values must have unique language tags\). Only for String values \(in theory the exact datatype of the target literal is `rdf:langString`, but due to our Schema.org &lt;-&gt; XSD mapping we use xsd:string and allow a language tag for it\)

## Property Value Pair Constraint Components

SHACL reference: [https://www.w3.org/TR/shacl/\#core-components-property-pairs](https://www.w3.org/TR/shacl/#core-components-property-pairs)

The constraint components in this section specify conditions on the sets of value nodes in relation to other properties. **These constraint components can only be used by property shapes**. A verification algorithm should be able to treat certain datatypes as equal \(e.g. inside String-based datatypes \(Text, URL\), or inside Number-based datatypes \(Number, Float, Integer\)\).

**sh:equals** - `[IRI]` - specifies the condition that the set of all value nodes is equal to the set of objects of the triples that have the focus node as subject and the value of sh:equals as predicate. \(The property shape having sh:equals must have the same values as the property shape with the sh:path specified with sh:equals\). Available for all datatypes.

**sh:disjoint** - `[IRI]` - specifies the condition that the set of value nodes is disjoint with the set of objects of the triples that have the focus node as subject and the value of sh:disjoint as predicate. \(The property shape having sh:disjoint must NOT have the same values as the property shape with the sh:path specified with sh:disjoint\). Available for all datatypes.

**sh:lessThan** - `[IRI]` - specifies the condition that each value node is smaller than all the objects of the triples that have the focus node as subject and the value of sh:lessThan as predicate. \(The property shape having sh:lessThan must have the values that are less than the property shape with the sh:path specified with sh:lessThan\). Available for all datatypes excluding Boolean.

**sh:lessThanOrEquals** - `[IRI]` - specifies the condition that each value node is smaller than or equal to all the objects of the triples that have the focus node as subject and the value of sh:lessThanOrEquals as predicate. \(like sh:lessThan, but equal values are also allowed\). Available for all datatypes excluding Boolean.

## Other Constraint Components

SHACL reference: [https://www.w3.org/TR/shacl/\#core-components-others](https://www.w3.org/TR/shacl/#core-components-others)

These constraints could be used on range- or property-level, but to have a clearer structure \(that eases the UI options for interfaces working with DS\) we put these constraints on the range-level.

Our algorithm should be able to treat certain datatypes as equal \(e.g. inside String-based datatypes \(Text, URL\), or inside Number-based datatypes \(Number, Float, Integer\)\).

We also use sh:in to define valid enumeration values for an enumeration, those values are wrapped by an `@id` object, signalizing that they are URIs.

**sh:in** - `[IRI | Literal]` - sh:in specifies the condition that each value node is a member of a provided SHACL list \(IRI or Literal\).

**sh:hasValue** - `[IRI | Literal]` - sh:hasValue specifies the condition that at least one value node is equal to the given RDF term \(IRI or Literal\) value.

