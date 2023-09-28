# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).

## links

- [landing page](https://tinkerpop.apache.org/)
- [tinkerpop: reference](http://tinkerpop.apache.org/docs/current/reference/)
- [gremlin: with neptune](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin.html)
- [gremlin: online book](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html)

## Gremlin

- a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph. Every Gremlin traversal is composed of a sequence of (potentially nested) steps.

## Property Graph

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data
- labels: attached to vertices and edges

## quick ref

```ts
////////////////////////////////// examples
// bi-directional relationship
g.v.hasLabel("person").both("friends");

// unidirectional relationship: in/out
g.v.hasLabel("person").in("friends");

// creating datasets
g.addV("person").property(id, "myname").V('myname').addE('somerelation').to(V('otherVertexId')).property('someProp', someVal').toList;

////////////////////////////////// long list
// g -> graph traversel source


// vertices
// addV -> create verticies
// otherV -> navigate to specific verticies

// edges
// addE -> create edges
// outE -> reference to outgoing edges


// traversel
// in -> follow INcoming edges to vertices
// out -> follow OUTgoing edges to vertices
// both ->
// bothE ->

// loops
// until
// repeat
// emit -> yield a traversel for each step in a repeat()


// filters
// has
// or
// between
// is
// until
// where
// lte


// terminal methods
// toList -> returns array


// other stuff
// as -> create a reference so you can refer to it later
// project -> create projection of results
// by -> select by property value
// select
// values
// valueMap
// fold
// count
```
