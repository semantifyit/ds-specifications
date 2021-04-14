# Lists

(WIP)

Lists are collections of specific items that can be created and managed by users on semantify.it on an organisation-scope.
These specific items can be (so far): Domain Specifications, or Vocabularies (semantify hosted).
**Lists can have only 1 kind of item, therefore it is important to identify/differentiate their content (e.g. sub-types of List?) on all software levels (front-end, API routes, MongoDB, JSON-LD representation).**

Lists are supposed to be "public", the same way DS and Vocabularies do: having an @id that is a real URL, e.g. https://semantify.it/list/:HASH:
The semantify.it organisation "STI2" will manage different lists that will be used in our code for certain collections, e.g. DS for verification, DS for Instant Annotations, DS displayed on ds.sti2.org, Vocabularies open to any user for the DS Editor, etc.  
Further ideas/comments can be found in the corresponding trello card (see links).

Since we want to publish them as JSON-LD, we need a structure/vocabulary for it. If possible we should not introduce new terms. There is a guideline in this Wiki for us how to make/use vocabularies (see links).

### Links

* [Trello Card](https://trello.com/c/4AgBMDKt/1054-roadmap-ds-voc-redesign)
* [Creating Vocabularies](../CreatingVocabularies.md)

### Discussion

We may introduce new kind of lists in the future, would be good if we would not have to add a lot of stuff (infrastructure, specification) to make that possible. At the same time we want to easily identify which kind of "list" we are dealing with. Suggestion:

#### MongoDB-Model (mongoose schema)

We can have a single model for any list-type, distinguishing them by "itemType":

```json
{
    itemType: String, //we will have Enumerations for this e.g. "DomainSpecification", "Vocabulary"
    content: Object,  //the actual JSON-LD
    hash: {           //random HASH, also used for the @id inside the JSON-LD
        type: String, 
        unique: true
    },    
    organisation: [{type: mongoose.Schema.Types.ObjectId, ref: 'Organisation'}], //since lists are managed on an organisation level
    revisionHistory: [{   //maybe we want this?
        _id: false,
        user: {type: mongoose.Schema.Types.ObjectId, ref: 'User'},
        updated: {type: Date, default: Date.now},
    }]
},
{
    timestamps: {createdAt: 'created', updatedAt: 'updated'}
}
```

the "name" should be taken from the content instead of saving it in the outer model.

#### API Routes

If possible use already the api v2 infrastructure.

##### Routes for the front-end

We will have a view to manage lists -> get all lists for a user (based on his organisations), that info is given by the Auth token. We will have views where we interact with certain type of lists (e.g. add DS to a DS list in the DS overview UI)

* GET: /api/lists ?itemType=:itemType: ?populate=(true/false)
* POST /api/lists
* GET:  /api/lists/:listID: ?populate=(true/false)
* PATCH/DELETE:  /api/lists/:listID:

?itemType filters lists with a specific type of items.

?populate = true adds meta information to the items, depending on their type. (see example below)

##### Route for the @id

* semantify.it/list/:HASH: = smtfy.it/lst/:HASH:

(I dont think we need to display the type of the items in that IRI)
I think this should return always the populated items.

##### JSON-LD

This is the json object saved in mongo DB:

```json
{
  "@context": {
    "schema": "http://schema.org/"
  },
  "@id": "https://semantify.it/list/Ab34get3",
  "@type": "DataSet",
  "name" : "Public Domain Specifications",
  "description" : "A list of well-documented Domain Specifications published by STI Innsbruck. The human-readable version of these Domain Specifications can be found at http://ds.sti2.org/",
  "author": {
    "@type": "schema:Person",
    "name": "oleksandra",
    "memberOf": {
      "@type": "schema:Organization",
      "name": "STI Innsbruck"
    }
  },
  "hasPart": [
    {
      "@type": "DataDownload",
      "contentUrl": "https://semantify.it/ds/_1hRVOT8Q"
    },
    {
      "@type": "DataDownload",
      "contentUrl": "https://semantify.it/ds/KXacVldjU"
    } 
  ]
}
```

This is the same List with populated = true:

```json
{
  "@context": {
    "@vocab": "http://schema.org/",
    "shacl": "http://www.w3.org/ns/shacl#",
    "sh:targetClass": {
      "@type": "@id"
    }
  },
  "@id": "https://semantify.it/list/Ab34get3",
  "@type": "DataSet",
  "name" : "Public Domain Specifications",
  "description" : "A list of well-documented Domain Specifications published by STI Innsbruck. The human-readable version of these Domain Specifications can be found at http://ds.sti2.org/",
  "author": {
    "@type": "schema:Person",
    "name": "oleksandra",
    "memberOf": {
      "@type": "schema:Organization",
      "name": "STI Innsbruck"
    }
  },
  "hasPart": [
    {
      "@type": "DataDownload",
      "contentUrl": "https://semantify.it/ds/_1hRVOT8Q",
      "sh:targetClass": "http://schema.org/Airport",
      "name": "Airport",
      "description": "Airport Domain Specification is a domain-specific pattern for annotating airports using schema.org vocabulary."   
    },
    {
      "@type": "DataDownload",
      "contentUrl": "https://semantify.it/ds/KXacVldjU",
      "sh:targetClass": "http://schema.org/BarOrPub",
      "name": "BarOrPub",
      "description": "BarOrPub Domain Specification is a domain-specific pattern for annotating bars and pubs using schema.org vocabulary."
    } 
  ]
}
```

## Discussion:

* Add "url" with the same value as "@id"?
* Take other type than "DataDownload"? (since we have to create a vocab for DS anyway, we can introduce a type for DomainSpecifications and use it here?)
* ?populate could have multiple options, like including the whole document instead of only meta data?
* Which properties of DS/Vocabulary to put in the populated version?