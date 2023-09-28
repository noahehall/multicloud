# Neptune Property graph

- Apache tinkerpop
- gremlin traversal language

## best practicies

- design for querability: work backwards from the questions your data must answer

## basics

## data model

- the supsert of paths traversed by your queries
  - should offer the most efficient routes for your queries
- entity vs attribute, vertex vs property
  - does it have an identity or is it a value type?
  - is it a complex type with multiple fields?
  - are there any structural relationships between values
  - prefer properties > vertices/edges until the needs arise

### vertices:

- entities: things with identities, complex value types
- properties: entity attributes + metadata
- labels: roles, grouping, tagging
- intermediate vertex: required since you cant `attribute` an edge with a vertex
  - i.e. you can go from vertex A to edge B that connects vertex C & D
  - you need an intermediate vertex that defines the relationship from A to C & D
    - this generally occurs when youve used an edge property when in fact you need a new vertex

### edges

- relationships between entities, variable structure
  - mult-edges: there can be multiple routes between two vertices
  - self-edges: a vertice can route to itself
- properties: relationship strength, weight or property + metadata
- labels: relationship semantics

## tests

### unit tests

- create & execute tests via sagemaker jupiter notebooks, gremlin console, cloud9 ide

## gremlin

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


// filters
// has
// or
// between
// until
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
// in -> following INcoming edges to vertices
// out -> following OUTgoing edges to vertices
// fold
// repeat
// count
// emit -> yield a traversel for each step in a repeat()
```
