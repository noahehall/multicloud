# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- intended for those interested in AWS neptune but prefer to rampup via tinkerpop
- bookmark
  - 2.8. A word about indexes and schemas

## TLDR!

```sh
# download some sample data
# https://github.com/krlawrence/graph/blob/main/sample-data/air-routes-latest.graphml

# start a container and copy the sample data into it
docker run -it --rm --name gremlin tinkerpop/gremlin-console:latest gremlin
docker cp ~/Downloads/air-routes-latest.graphml gremlin:sampledata.graphml

# enable the sugar plugin for shorthand forms of queries
# dunno if this transfers to neptune tho
:plugin use tinkerpop.sugar

# create a graph, load the data file and then create a "traversal source"
graph = TinkerGraph.open()
graph.io(graphml()).readGraph('/sampledata.graphml')
g = traversal().withEmbedded(graph)

# run some queries then exit
graph.toString()
g.V().count().next()
g.V().valueMap(true)
g.V().has('code','AUS').valueMap(true).unfold()
:exit

```

## links

- [landing page](https://tinkerpop.apache.org/)
- [janusgraph](https://janusgraph.org/)
- [gremlin: tutorials](https://tinkerpop.apache.org/docs/current/tutorials/getting-started/)
- [gremlin: console tutorial](https://tinkerpop.apache.org/docs/current/tutorials/the-gremlin-console/)

### practical gremlin

- [AAA: github](https://github.com/krlawrence/graph)
- [AAA: landing page](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html)
- [AAAA: bunch of query examples](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html#msc)
- [sample: apps](https://github.com/krlawrence/graph/tree/master/demos)
- [sample: code](https://github.com/krlawrence/graph/tree/master/sample-code)
- [sample: data startup script](https://github.com/krlawrence/graph/blob/main/sample-data/load-air-routes-graph-34.groovy)
- [sample: data](https://github.com/krlawrence/graph/tree/master/sample-data)

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

### related

- janusgraph: Distributed, open source, massively scalable graph database
- gephi: source tool for visualizing graph data

## basics

- a graph computing framework and top level project hosted by the Apache Software Foundation.
- common file formats
  - GraphML: widely recognized by TinkerPop
  - GraphSON: JSON defined by Apache TinkerPop and heavily used in that environment
  - CSV

## Gremlin

- graph traversal and query language
- a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph. Every Gremlin traversal is composed of a sequence of (potentially nested) steps.

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data for both edges and vertices
- labels: attached to vertices and edges

### console

- an interactive terminal or REPL that can be used to traverse local/remote graphs and interact with the data that they contain.
- the most common method for performing ad hoc graph analysis, small to medium sized data loading projects and other exploratory functions.
- hosts the Gremlin-Groovy language; you can enter valid Groovy code directly into the console
- tldr
  - ending a cmd with `;[]` hides the console output

```sh
### console cmds, always prefixed with `:`
# important
:help # list or specific cmd
:exit # the shell
:cls # clear the screen
:load # a file/url into buffer, e.g. a groovy startup script
:record start toThisFile.log .... :record stop
:history # of shiz u did
:plugin # manage console plugins
:remote # define a connection

# others
:quit # via :exit
:clear # the buffer & reset prompt counter
:inspect # var/last result in browser
:purge # maybe everything
:save # buffer to file
:record # current session to file
:grab # add a dependency to the shell env
:set # or list preferences
:un/install # a maven lib
:submit # a gremlin script to a gremlin server
:show imports


### known classes
Gremlin
  .version()
```

### tinkergraph

- in-memory (with optional persistence), non-transactional graph engine that provides both OLTP and OLAP functionality.
- great for learning

```sh
### enable tinkergraph if its not enabled
:plugin use tinkerpop.tinkergraph

### create a new graph and load some data from a file
graph = TinkerGraph.open()
graph.io(graphml()).readGraph('air-routes.graphml')

```

### server

- Allows hosting of graphs remotely via an HTTP/Web Sockets connection.
- among other things, Provides a method for non-JVM languages which may not have a Gremlin Traversal Machine (e.g. Python, Javascript, Go, etc.) to communicate with the TinkerPop stack on the JVM.

### quick ref

- tested with tinkergraph and the practical grammer air routes data file
- check the TLDR up top

```ts
// Graph object
graph // e.g. graph = TinkerGraph.open()
  .features() // what tinkerpop features are supported
  .toString() // basic stats about the graph

////////////////////////////////// examples
// total airports by country
g.V().hasLabel('airport').groupCount().by('country')

// all paths from  AUS to AGR with exactly two stops
g.V().has('code','AUS').out().out().out().has('code','AGR').path().by('code')
// same as above
g.V().has('code','AUS').repeat(out()).times(3).has('code','AGR').path().by('code')




////////////////////////////////// dunno where these came from (aws docs?) but should be updated to use the air routes data
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

////////////////////////////////// wouldnt trust anything here
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
