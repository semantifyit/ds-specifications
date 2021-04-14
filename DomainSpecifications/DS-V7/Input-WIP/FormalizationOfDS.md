# Introduction

We propose a new approach to domain-specifications to solve two issues:

- to solve the ambiguities regarding target selection (an event organizer and an NGO may be both instances of Organization type)
- to support hierarchical Domain Specifications
- to improve the maintenance of Domain Specifications (there can be multiple Domain Specifications based on subtypes of Place. A change in the Place DS should be automatically reflected on its sub-Domain Specifications). see also: [Formalization of inheritance](Formalization%20of%20Inheritance),

This new formalism reflects the syntax and semantics of MSKR (see out KG book)

# Challenges

- ensure the consistency of hierarchical domain specifications
- change the verification algorithm

# Approach

We treat Domain Specifications as types. An instance of a schema.org type can also be a type of domain specification. This way, the verification process turns into instance checking under CWA (i.e., is the instance an instance of the specified domain specification?).

This approach also fits with the previous conceptualization as we treat SHACL shapes more class centric (a node shape is defined with its property shapes). We address the first challenge by allowing the explicit definition of DS as instance types (i.e. target class for each instance is clearly defined.)

# Syntax

We use SHACL syntax with the restrictions of the previous DS concept. (e.g., property shapes are only defined on node shapes, and they are not allowed to have target classes). We introduce a new property, `ds:subDSOf`, to define the hierarchy of domain specifications. 

```
hasDomain(subDSOf, NodeShape)
hasRange(subDSOf, NodeShape)
```

SHACL `sh:shapesGraph` property is used to make instance assertions with DS as types.

```
hasDomain(shapesGraph, Thing)
hasRange(shapesGraph, NodeShape)
```

# Semantics

1. A subDS inherits all property shapes from its superDS
2. if subDSOf(ds1,ds2) and shapesGraph(i,ds1) then shapesGraph(i, ds2)
3. targetClass(ds1,t1) and targetClass(ds2,t2) and subDSOf(ds1, ds2) then isA(t1,t2) 
4. a subDS can introduce new property shapes.
5. the constraints defined on a subDS must be as restrictive as the superDS or more restrictive. (also related to rule 2. instances that fit a subDS must be a subset of those that fit a superDS). That is important for consistency.  
6. Multiple sh:shapesGraph assertions are treated as a conjunction (an instance must fit all DS defined on it)

# Verification Example

Data:

```json
{
    "@type": "Event",
    "sh:shapesGraph": "dzt:Event"
    "name": "Fasching",
    "startDate": "2022-02-07",
    "endDate": "2022-02-09",
    "organizer": {
        "@type": "Organization"
        "name": "Stadt Köln"
    },
    "funder": {
        "@id": "https://koelsch.com"
    }
}
```

_Note: we assume there is an Organization instance identified with https://koelsch.com exists in the graph._

DS:

```turtle
dzt:Event a sh:NodeShape;
    sh:targetClass schema:Event;
    sh:property [
        sh:path schema:name;
        sh:minCount 1;
        sh:datatype xsd:string
    ];
    sh:property [
        sh:path startDate;
        sh:minCount 1;
        sh:maxCount 1;
        sh:datatype xsd:datetime .
    ];
    sh:property [
        sh:path endDate;
        sh:minCount 0;
    ];
    sh:property [
        sh:path organizer;
        sh:minCount 1;
        sh:class schema:Organization;
        sh:node [
            sh:property [
                sh:path schema:name;
                sh:minCount 1;
            ]
        ]
    ];
    sh:property [
        sh:path funder;
        sh:minCount 1;
        sh:class Organization;
        sh:node dzt:Organization .
    ]
```

_Note: we assume there is a dzt:Organization shape_

Unlike the current verification approach, we start with the instances. Each instance that has a sh:shapesGraph assertion is verified separately. Verification happens similar to the current method. Starting from the node with sh:shapesGraph assertion, all property shapes are checked against the graph. So here, an event must have a name, start date, organizer and a funder. Here the value of the `organizer` property is defined within the dzt:Event shape.
In contrast, the value of the funder is linked to another shape (dzt:Organization). Note that the sh:node dzt:Organization constraint can be translated into a sh:shapesGraph assertion on funder's value. Meaning, the koelsch.com instance must fit dzt:Organization. If koelsch.com instance has another ds:shapesGraph attached to it, then it should fit both shapes. If not, then the instance checking fails for the corresponding type.



## Verification with DS inheritance

**TODO: make an example DS as subDS of dzt:Event**

Assume that there is a dzt:MusicEvent shape. It is defined as subDS of dzt:Event and adds a performer property shape. Note that adding new property shapes sounds like extending a type, but the extension shrinks the set of instances that fit the shape. Events with a performer is a subset of all events.

An event with dzt:MusicEvent as sh:shapesGraph value must first fit dzt:Event, then dzt:MusicEvent. If it does not fit the superDS but fits the subDS, we can infer an inconsistency between hierarchical DS (i.e., they are not in the same hierarchy.). 

### Conclusion

- target selection ambiguity is solved with sh:shapesGraph
- inheritance comes out of the box. (also addresses the use case of join.)
- if a domain expert wants to make a LocalBusiness DS but not based on an existing Place DS, she should omit the ds:subDSOf assertion.