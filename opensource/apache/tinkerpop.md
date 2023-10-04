# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- intended for those interested in AWS neptune but prefer to rampup via tinkerpop
- bookmark
  - 3.3.4. Does an edge exist between two vertices?

## TLDR!

```sh
# download some sample data; all examples in this file use this dataset
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

- graph traversal and query language for working with property graphs
- a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph. Every Gremlin traversal is composed of a sequence of (potentially nested) steps.
- traversal: aka query; traverses a graph from point A to point B using one/more steps chained together
  - adjacent: One vertex is considered to be adjacent to another vertex if there is an edge connecting them
  - incident: A vertex and an edge are considered incident if they are connected to each other.
  - anonymous: traversals that do not start with a `g.V/E()`
- steps: aka methods;
  - terminal steps: ends the graph traversal and returns a concrete object that you can work with further
- walking the graph: describe moving from one vertex to another vertex via an edge; moving through the graph from one place to one or more other places
  - circular walk: walking the graph but ending up back where you started
  - path: the journey you took on the walk

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data for both edges and vertices
- labels: attached to vertices and edges
- ids: every vertex and every edge in a graph has a unique ID
  - can be auto generated or user provided

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
- keywords: label

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
// same as above but using repeat fn
g.V().has('code','AUS').repeat(out()).times(3).has('code','AGR').path().by('code')
// how many routes leaving airports
g.V().hasLabel('airport').outE('route').count()
// How many of each type of vertex/edge are there? all return the same thing
g.V().groupCount().by(label) // can replace g.V() with g.E()
g.V().label().groupCount()
g.V().group().by(label).by(count())
// How many airports are there in each country?
g.V().hasLabel('airport').groupCount().by('country')
// How many airports are there in each country? (look at country first)
g.V().hasLabel('country').group().by('code').by(out().count())
// How many airports are there in France, Greece and Belgium respectively?
g.V().hasLabel('airport').groupCount().by('country').select('FR','GR','BE')
// for each route, return both vertices and the edge that connects them.
g.V().has('airport','code','LCY').outE().inV().path()
// modulates the previous query
g.V().has('airport','code','LCY').outE().inV().path().by('code').by('dist')
// five routes that start in austin, modulated by an anonymous traversal
g.V(3).out().limit(5).path().by(values('code','city').fold())
// similar to above, but uses AS and FROM to ignore the starting vertex
g.V().has('airport','code','AUS').out().as('a').out().as('b').path().by('code').from('a').to('b').limit(10)



////////////////////////////////// common steps
g = graph.traversal() // graph traversel source object for a loaded graph
g.V() // all vertices
g.E() // all edges


////////////////// terminal steps:
next() // turns the result of the traversal into an object we can work with further.
fold() // converts results to an array
toList() // dunno, suppose to return an array
path() // get a summary of the path walked
limit() // amount of elements returned
dedup() // remove duplicates

////////////////// traversal steps;
// all accept edge labels as filters
both() // Both incoming and outgoing adjacent vertices.
bothE() // Both outgoing and incoming incident edges.
in() // Incoming adjacent vertices.
inE() // Incoming incident edges.
otherV() // The vertex that was not the vertex we came from.
out() // Outgoing adjacent vertices
outE() // Outgoing incident edges; examine the outgoing edges from a given vertex
// dont accept edge labels as filters
outV() // Outgoing vertex.
inV() // Incoming vertex.


////////////////// filters: will return back the ID of matching element(s)
between()
has(['label',] 'prop', 'value')
hasLabel('prop')
hasNot('prop') // shorthand for not(has('prop'))
is()
lte()
not()
or()
where()



////////////////// CRUD: READ
values('key1', 'key2', ...) // retrieve values for 0/more keys
label() // read
select() // extract specific values


////////////////// CRUD: modification
// addV -> create verticies
// addE -> create edges
// drop
// Cardinality.single is optional, else it uses Cardinality.set and appends the value
// property(Cardinality.single?, 'key', 'val') -> inserts a property with the given key and value




////////////////// modulators: alter the behavior of the steps that they are associated with.
as() // create a reference so you can refer to it later
// processed in a round robin fashion in cases where there are more elements that modulators specified
// ^ e.g. if path() returns [el1, el2, el3] and you only specify 1 by(), its executed for each
// ^ use a by modulator with no param if the element is not a vertex/edge
by() // for each element returned, extract this prop/run this fn
from()
to()


////////////////// useful steps
count()


////////////////// aggregates
groupCount()
by()
group() // collect things into a group using some filter



////////////////// other steps
getMethods() // list of all the methods and their supported types



////////////////// control structs
// until
// repeat
// emit -> yield a traversel for each step in a repeat()
// coalesce(ifThisFails, doThis) -> executes the first argument, on failure executes the second



// other stuff
// project() -> create projection of results
// valueMap()
// V()
// sideEffect()




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

```
