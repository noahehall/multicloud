# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).

## links

- [landing page](https://tinkerpop.apache.org/)

### practical gremlin

- [AAA: landing page](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html)
- [meta properties](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html#metaprop)

### Ref

- [AAA: docs landing page](https://tinkerpop.apache.org/docs/current/reference/)
- [sessions](https://tinkerpop.apache.org/docs/current/reference/#console-sessions)

## Gremlin

- a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph. Every Gremlin traversal is composed of a sequence of (potentially nested) steps.

## Property Graph

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data
- labels: attached to vertices and edges

## quick ref

- FYI: this quick ref is geared towards neptunes gremlin implementation

```ts
////////////////////////////////// examples
// bi-directional relationship
g.v.hasLabel("person").both("friends");

// unidirectional relationship: in/out
g.v.hasLabel("person").in("friends");

// creating datasets
g.addV("person").property(id, "myname").V('myname').addE('somerelation').to(V('otherVertexId')).property('someProp', someVal').toList;

// conditional update
g.V('person1').hasLabel('Person').coalesce(has('creditScore'), property('creditScore', 'AAA+'))

// conditional update: globally unique SSN
g.V().has('ssn', 123456789).fold()
  .coalesce(__.unfold(),
            __.addV('Person').property('name', 'John Doe').property('ssn', 123456789'))

// conditional update: based on another property
g.V('person1').hasLabel('Person').has('level', 1)
 .property('level2Score', 0)
 .property(Cardinality.single, 'level', 2)

// replace a property
g.V('person1').hasLabel('Person')
 .sideEffect(properties('creditScore').drop())
 .property('creditScore', 'BBB')

////////////////////////////////// api
// g -> graph traversel source


// modification
// addV -> create verticies
// addE -> create edges

// Cardinality.single is optional, else it uses Cardinality.set and appends the value
// property(Cardinality.single?, 'key', 'val') -> inserts a property with the given key and value


// traversel
// otherV -> navigate to specific verticies
// in -> follow INcoming edges to vertices
// out -> follow OUTgoing edges to vertices
// outE -> reference to outgoing edges

// both -> bidirectional
// bothE -> bidirectional

// control structs
// until
// repeat
// emit -> yield a traversel for each step in a repeat()
// coalesce(ifThisFails, doThis) -> executes the first argument, on failure executes the second

// filters
// has
// or
// between
// is
// until
// where
// lte
// hasLabel


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
// to(V('abc'))
// sideEffect
```
