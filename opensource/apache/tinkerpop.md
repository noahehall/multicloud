# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- bookmark
  - 2.2. The Gremlin console

## TLDR!

```sh
# start a container
docker run -it tinkerpop/gremlin-console:latest gremlin

# create a graph, and then create a "traversal source" that uses that graph
graph = TinkerFactory.createModern()
g = traversal().withEmbedded(graph)

# run some queries then exit
g.V().valueMap(true)
:exit

```

## links

- [landing page](https://tinkerpop.apache.org/)
- [janusgraph](https://janusgraph.org/)
- [gremlin: getting started](https://tinkerpop.apache.org/docs/current/tutorials/getting-started/)

### practical gremlin

- [AAA: landing page](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html)
- [AAA: github](https://github.com/krlawrence/graph)
- [sample data](https://github.com/krlawrence/graph/tree/master/sample-data)
- [sample code](https://github.com/krlawrence/graph/tree/master/sample-code)
- [sample apps](https://github.com/krlawrence/graph/tree/master/demos)

### docker

- [gremlin console](https://hub.docker.com/r/tinkerpop/gremlin-console/)
- [gremlin server](https://hub.docker.com/r/tinkerpop/gremlin-server)
- [blog: setup console in docker](https://emmer.dev/blog/creating-a-gremlin-playground/)

### Ref

- [AAA: console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console)
- [AAA: docs landing page](https://tinkerpop.apache.org/docs/current/reference/)
- [AAA: recipes](https://tinkerpop.apache.org/docs/current/recipes/)
- [AAA: server](https://tinkerpop.apache.org/docs/current/reference/#gremlin-server)
- [AAA: tinkergraph](http://tinkerpop.apache.org/docs/current/reference/#tinkergraph-gremlin)
- [meta properties](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html#metaprop)
- [sessions](https://tinkerpop.apache.org/docs/current/reference/#console-sessions)

## basics

- a graph computing framework and top level project hosted by the Apache Software Foundation.

## related

- shiz that can be used with gremlin

### janusgraph

- Distributed, open source, massively scalable graph database

## Gremlin

- graph traversal (query) language
- a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph. Every Gremlin traversal is composed of a sequence of (potentially nested) steps.

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data
- labels: attached to vertices and edges

### console

- an interactive terminal or REPL that can be used to traverse local/remote graphs and interact with the data that they contain.
- the most common method for performing ad hoc graph analysis, small to medium sized data loading projects and other exploratory functions.
- hosts the Gremlin-Groovy language.

### server

- Allows hosting of graphs remotely via an HTTP/Web Sockets connection.
- among other things, Provides a method for non-JVM languages which may not have a Gremlin Traversal Machine (e.g. Python, Javascript, Go, etc.) to communicate with the TinkerPop stack on the JVM.

### quick ref

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
// drop

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

## tinkergraph

- in-memory (with optional persistence), non-transactional graph engine that provides both OLTP and OLAP functionality.
- great for learning
