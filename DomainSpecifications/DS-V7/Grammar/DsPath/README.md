# DS-V7 Path Syntax

A DS-Path Syntax is a recommended format on how to point to a certain node within a Domain Specification. In the
following the **DS-Path Syntax** for **DS-V7** is introduced.

A DS-Path is a string composed of different tokens that lead from one DS node to another, until all tokens are consumed.
All tokens that use IRIs make use of the context in the DS (that means they must be denoted in compact IRI form).


## Symbols

|                 Name                |         Syntax         |                                                                                  Description                                                                                  |                Full example               |
|:-----------------------------------:|:----------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------:|
|              Root Node              |            `$`           | Points to the root node of the DS (found directly in the @graph array).                                                                                                       |                     `$`                     |
|    Internal Reference Definition    |     ` #{fragmentId} `    | Points to an internal reference definition (found directly in the @graph array). The fragment ID is the uid after the # of the @id.                                           |                   `#tMMiT`                 |
|    External Reference Definition    |         `{DsUID}`        | Points to an external reference definition, which is found directly in the @graph array ONLY IF the DS has been populated and the external DS is included in the current one. |                 `gsaTefLCP`                 |
|    Internal node External Reference Definition    |         `{DsUID}#{fragmentId}`        | Points to an internal node of an external reference definition, which is found directly in the @graph array ONLY IF the DS has been populated and the external DS is included in the current one (together with its internal reference node). |                 `gsaTefLCP#lwioY`                 |
|               Context               |        `@context`        | Special DS-Path that consists only of this token. Points to the Context of the DS                                                                                             |                  `@context`                 |
|               Property              |     `.{propertyIRI}`     | The property IRI of the node (sh:path) is added with a preceding dot.                                                                                                         |               `$.schema:name`               |
|               DataType              |     `/{datatypeIRI}`     | The data type IRI of the node (sh:datatype) is added with a preceding slash.                                                                                                  |         `$.schema:name/xsd:string`          |
|          Class/Enumeration          |      `/{classIRIs}`      | The class(es) IRIs of the node (sh:class) are added with a preceding slash. Multiple classes are divided by a comma.                                                          | `$.schema:offer/schema:Product,schema:Room` |
|         Root node Reference         |           `/@$`          | Uses the same $-symbol as the root node, but with a preceding slash and @-symbol.                                                                                             |             `$.schema:offer/@$`             |
|          Internal Reference         |     `/@#{fragmentId}`    | Uses the same syntax as the internal reference definition, but with a preceding slash and @-symbol.                                                                           |           `$.schema:offer/@#tMMiT`          |
|          External Reference         |        `/@{DsUID}`       | The UID of the external DS (part of their @id) is used with a preceding slash and @-symbol.                                                                                   |         `$.schema:offer/@yFV-LM7MP`         |
| Internal node of External Reference | `/@{DsUID}#{fragmentId}` | The UID of the external DS (part of their @id) and the fragment ID of the internal node are used with a preceding slash and @-symbol.                                         |      `$.schema:offer/@gsaTefLCP#lwioY`      |

## Semantics 

There is only one special DS-Path, which is `@context`. In any other case, a DS-Path always starts with an initial token, and can be followed by alternating property tokens and range tokens. A property token can only be added if the previous token was a Class (or a reference to a Class).

A DS-Path can serve multiple purposes. In general, it can point to a specific part of an unpopulated or a populated DS (e.g. DS-Browser). But it can also mark a specific path from the root node to any reachable node, including referenced nodes (e.g. for the dsPath entry of a verification error). To be fitting for both use-cases following rules are introduced:

* A DS-Path pointing to a reference node does exactly that: pointing to the REFERENCE entry, not to the referenced node. This holds if the dsPath string ends with the reference token. E.g. `$.schema:organizer/@#Int12`
* If a referenced node should be pointed to, the dsPath should have the corresponding initial token (`#{fragmentId}` for internal reference definition or `{DsUID}` for external reference definition in a populated DS). E.g. `#Int12`
* If a DS-Path includes referenced nodes, but does not end with those, the algorithm traverses the reference node following the properties of its referenced node. E.g. `$.schema:organizer/@#Int12.schema:name`

## Example

**DS-Path:**

`$.schema:organizer/schema:Person.schema:name/xsd:string`

**Path steps:**

|       Token      |                 Description                |
|:-----------------:|:------------------------------------:|
|         `$`         |               Start of path: Root Node              |
| `.schema:organizer` | Property Node for “schema:organizer” |
|   `/schema:Person`  |    Class Node for “schema:Person”    |
|    `.schema:name`   |    Property Node for “schema:name”   |
|    `/xsd:string`    |    DataType Node for “xsd:string”. End of path   |

This DS-Path points to a DataType Node for xsd:string.