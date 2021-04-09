# DS-V6

This specification version never passed the draft-phase. It included proposals for improvements to not only the DS specification (including verification results), but also Lists, the verification process, and the DS vocabulary. 

Following proposed improvements are the result of an analysis we did on the DS specification and its usage (verification on different data sources, e.g. annotations vs. knowledge graph).

* Use JSON-Path for the paths given in the errors based on the preprocessed JSON-LD to verify (instead of our own formats - we have different ones for Annotation and KG) - need to check this more in detail. If needed we introduce different path-related vocabulary terms in the DS-Vocabulary for Annotation and KG.
  
* Update specifications for DS and Verification at least with the stuff we added on-the-fly because we needed it fast (e.g. shacl:targetOfProperty, use of shacl:nodeKind for URL literals).
  
* Create and host our DS/List-Vocabulary, and use it as the format/vocabulary for all JSON-LD verification outcomes. Front-end tools would need to be adapted to understand the new vocabulary (For the most part this is not a big issue).
  
* Unify verification algorithms for Annotations and KG Data. This can be done by preprocessing both input formats into JSON-LD and handling that preprocessed JSON-LD as the “semantic data we understood'' (Google does that too). Anomalies originating from each input type (Annotations/KG) can be handled during the preprocessing so that the resulting JSON-LD has a valid/understandable syntax, but reflects those anomalies which may be identified as errors during the verification (e.g. KG data can have data type in XSD that we map to schema). The big gain here is that we have only 1 verification algorithm afterwards (now we have 1 for annotations and 1 for kg, although the latter is based on the first), and a standard format for the “preprocessed” annotation, that can be displayed easier in the UI (if it is parsed with UI elements). Have to check in detail if this is possible.