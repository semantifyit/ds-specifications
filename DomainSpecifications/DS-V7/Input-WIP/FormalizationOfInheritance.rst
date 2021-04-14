Introduction
============
 

Challenge
==========
 

*  a DS based on a supertype has no formal relationship with ta DS defined on a subtype
*  this makes it very challenging to maintain DSs
  
   * what happens when you have 50 DSs for subtypes of place and want to change the constraint on a Place property


*  there is no possibility to define a DS with a subset of the subtypes of a type as target
*  no formal relationship between MTEs and the types comprising the MTE. (what is the relationship between HotelMuseum DS and Hotel DS (or Museum DS)?)

Requirements

=============

We need a way to formalize the inheritance between hierarchical  DS to improve maintainability. Meanwhile, we also need to support internal use cases and prevent them from being affected by the changes happening in the DS that is higher in the hierarchy.

Technical Solution
==================


#. formalize the inheritance. if I am making a hotel DS, I should be able to select a super DS. We should formally check if the sub DS is compatible with the super DS (e.g. if the set of instance matching a sub ds are subset of the set of the instances matching super DS)

#. Global vs Local View of DS: Use lists to define a global or local view of DS.
Example scenario: Thüringen wants to make a DS for some specific POI. They use the POI DS from DZT. They should have the option to have a local copy of the POI DS to maintain themselves. (e.g., add a new property that is valid for all POIs in thüringen but not necessarily for DZT). For all internal purposes, this local POI DS is used. If thüringen wants to push data to DZT, then the POI DS from DZT is selected from verification. This has the following benefit: Thüringen can do whatever they want with their data, but they have to comply with DZT when they are pushing the data to DZT KG. High flexibility with some new complexity.


formal definitions will follow.