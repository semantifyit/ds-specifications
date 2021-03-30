# DS-V2

This specification version was created due to following reasons:

*   More features
    *   from "future implementation" wishlist of the first version
    *   from STI2 requests
    *   from our own feedback
    *   from related work research
    * Specification for the verification outcome
*   Easier to work with

The new grammar is defined in JSON, which is easier to read AND work with than JSON-LD (At this time we had no practical use of DS in RDF-format, so it would be handier to strap off the JSON-LD layer and work only with JSON). However, the grammar is created having the possibility to translate a DS from JSON into JSON-LD, and vice versa. 

For the semantic verification of values, "Rules" are introduced. We had only a conceptual version of this feature for a paper, but it never made it into the DS grammar. These rules should allow for **local** (tied to a single property of an object) and **global** (tied to multiple properties from an entity) semantic constraints.

The idea right now is that local constraints can be checked with Rules that are based on the data type of the checked value instance. Global constraints are applied on objects with the same rules on objects(classes), which bring together the properties/values to verify, and the checks (reuse of the other rules). This is done with JSON Pointers.

There are properties in the grammar, starting with the "$" symbol. Most of these properties stand for special properties containing short strings which represent URIs (mostly from Schema.org).

## Content:

* [Examples](./Examples/Examples.md) (for DS and Verification Report)
* [Grammar](./Grammar/Grammar.md)
