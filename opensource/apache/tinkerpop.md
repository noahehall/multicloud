# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- intended for those interested in AWS neptune but prefer to rampup via tinkerpop
- bookmark
  - 3.8. Working with IDs

## TLDR!

- [run this container](https://github.com/nirv-ai/dbs/tree/main/graph#tinkerpop)

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
- gephi: open source tool for visualizing graph data

## basics

- a graph computing framework and top level project hosted by the Apache Software Foundation.
- common file formats
  - GraphML: widely recognized by TinkerPop
  - GraphSON: JSON defined by Apache TinkerPop and heavily used in that environment
  - CSV

### Data Model

- vertices: entity nodes
- edges: relationships connecting nodes
- properties: represent and store data for both edges and vertices
- labels: attached to vertices and edges
- ids: every vertex and every edge in a graph has a unique ID
  - can be auto generated or user provided
- Every vertex, every edge and even every property in a graph has a unique ID that can be used to reference it individually or as part of a group

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

- tested with tinkergraph and the practical gremlin air routes data file
- check the TLDR up top

```ts

////////////////////////////////// common steps
////////////////// Graph object
graph // e.g. graph = TinkerGraph.open()
  .features() // what tinkerpop features are supported
  .toString() // basic stats about the graph

////////////////// graph traversel source object
g = graph.traversal()
  .V() // all vertices
  .E() // all edges



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


////////////////// filters
between()
has() // e.g. has(['label',] 'prop', 'value')
hasLabel()
hasNot() // shorthand for not(has('prop'))
hasNext() // check if an edge exists between two vertices, eg.
is()
lte()
not()
or()
where()


////////////////// terminal steps: stops traversal and generates a result set
next() // returns object by default, pass an int to return the next(10) elements as an array
toList() // array
toSet() // math set
bulkSet() // weight set; the count of each element in the set
// dunno if these are classified as terminal steps
fold() // converts results to an array
unfold() // unbundles a collection



////////////////// limiting results
limit() // first X results, e.g. limit(10)
tail() // last X results
range() // between X and Y, start inclusiv, 0 indexed, e.g. range(0, 20) first 20, range(10, -1) until end
skip() // the first X and return remaining, shorthand for range(X, -1)
timeLimit() // limit query to X milliseconds, e.g. timeLinit(10)
until()
dedup() // remove duplicates, aka unique?; can provide references to limit specific steps


////////////////// modulators: alter the behavior of the steps that they are associated with.
as() // create a reference so you can refer to it later
// processed in a round robin fashion in cases where there are more elements that modulators specified
// ^ e.g. if path() returns [el1, el2, el3] and you only specify 1 by(), its executed for each
// ^ use a by modulator with no param if the element is not a vertex/edge
by() // for each element returned, extract this prop/run this fn
from()
to()
with() // e.g. with(WithOptions.tokens,WithOptions.labels, WithOptions.ids)
path() // get a summary of the path walked; more expensive (memory, cpu) than select + as


////////////////// CRUD: READ
values() // for 0/more keys, e.g. values('a', 'b', ...)
// valueMap(true): return ID an label of element; should use the with modulator starting from 3.4
// ^ e.g. valueMap().with(WithOptions.tokens)
valueMap() // all/specific properties as an array of key=[value] pairs; value is always an array
// vertices: returns id + label and the first value in a list as a primitive for each property
// edges: id + label + the id & label of in + outgoing verticies as well
elementMap() // enhanced valueMap
label()
// the keywords are used in a query when as() is used to supply multiple labels to the same el
// ^ e.g. g.V(1).as('a').V(2).as('a').select(first,'a')
select() // extract values/references; e.g. .select([first|last|all,]'propOrReferenceName')
project() // shorthand for as and select steps; creates projection of results


////////////////// CRUD: modification
// addV -> create verticies
// addE -> create edges
// drop
// Cardinality.single is optional, else it uses Cardinality.set and appends the value
// property(Cardinality.single?, 'key', 'val') -> inserts a property with the given key and value



////////////////// useful steps
count()
join()
order()

////////////////// aggregates
groupCount()
by()
group() // collect things into a group using some filter


////////////////// control structs
repeat()
emit() // yield a traversel for each step in a repeat()
coalesce(ifThisFails, doThis) // executes the first argument, on failure executes the second




////////////////// other steps
getMethods() // list of all the methods and their supported types on the current element
getClass() // get element class


// other stuff
// sideEffect()



////////////////// enums
label //
local //
first|last|all //

```

#### useful examples

- all based on the practice gremlin air routes dataset

```ts
////////////////////////////////// examples
// total airports by country
g.V().hasLabel("airport").groupCount().by("country");
// all paths from  AUS to AGR with exactly two stops
g.V().has("code", "AUS").out().out().out().has("code", "AGR").path().by("code");
// same as above but using repeat fn
g.V()
  .has("code", "AUS")
  .repeat(out())
  .times(3)
  .has("code", "AGR")
  .path()
  .by("code");
// how many routes leaving airports
g.V().hasLabel("airport").outE("route").count();
// How many of each type of vertex/edge are there? all return the same thing
g.V().groupCount().by(label); // can replace g.V() with g.E()
g.V().label().groupCount();
g.V().group().by(label).by(count());
// How many airports are there in each country?
g.V().hasLabel("airport").groupCount().by("country");
// same as above but looks at country first
g.V().hasLabel("country").group().by("code").by(out().count());
// How many airports are there in France, Greece and Belgium respectively?
g.V().hasLabel("airport").groupCount().by("country").select("FR", "GR", "BE");
// for each route, return both vertices and the edge that connects them.
g.V().has("airport", "code", "LCY").outE().inV().path();
// modulates the previous query
g.V().has("airport", "code", "LCY").outE().inV().path().by("code").by("dist");
// five routes that start in austin, modulated by an anonymous traversal
g.V(3).out().limit(5).path().by(values("code", "city").fold());
// similar to above, but uses AS and FROM to ignore the starting vertex
g.V()
  .has("airport", "code", "AUS")
  .out()
  .as("a")
  .out()
  .as("b")
  .path()
  .by("code")
  .from("a")
  .to("b")
  .limit(10);
// check if an edge exists between two verticies
g.V().has("code", "AUS").out("route").has("code", "DFW").hasNext();
// path vs select & as, both return the same thing but path is less verbose
g.V()
  .has("code", "DFW")
  .as("from")
  .out()
  .has("region", "US-CA")
  .as("to")
  .select("from", "to")
  .by("code");
g.V().has("code", "DFW").out().has("region", "US-CA").path().by("code");
// get the code, region and total routes of the first ten airports
// AS enables getting the same element 3 times, SELECT + BY assigns properties to each
g.V()
  .has("type", "airport")
  .limit(10)
  .as("a", "b", "c")
  .select("a", "b", "c")
  .by("code")
  .by("region")
  .by(out().count());
// same as above but uses PROJECT
g.V()
  .has("type", "airport")
  .limit(10)
  .project("a", "b", "c")
  .by("code")
  .by("region")
  .by(out().count());
// return every other stop
g.V()
  .has("code", "LAX")
  .out()
  .as("stop")
  .out()
  .out()
  .as("stop")
  .out()
  .out()
  .as("stop") // multiple AS returns an array
  .limit(1)
  .select(all, "stop")
  .unfold() // thats why we need to unfold the array
  .values("code")
  .fold(); // so we can operate on each element
// select the edge connecting MIA and DFW verticies
g.V().has("code", "MIA").outE().as("e").inV().has("code", "DFW").select("e");
// same as above but uses inE and outV
g.V().has("code", "MIA").inE().as("e").outV().has("code", "DFW").select("e");
// modulate how dedupe is applied to results
// return 1 airport for each unique number of runways
g.V()
  .has("region", "GB-ENG")
  .dedup()
  .by("runways")
  .values("code", "runways")
  .fold();
// unwrap a valuemap via an extra by(unfold()) step so all key=value are returned as strings
g.V().has("code", "SFO").valueMap().by(unfold()).unfold();
// examine an edge with elementMap
g.V(3).outE().limit(1).elementMap();
// same as above but with valueMap
g.E(5161)
  .project("v", "IN", "OUT")
  .by(valueMap(true))
  .by(inV().union(id(), label()).fold())
  .by(outV().union(id(), label()).fold());





////////////////////////////////// dunno where these came from (aws docs?)
// should be updated to use the air routes data or deleted
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

#### groovy: just enough

- just enough for debugging in the console

```groovy

// assign query results to a var; must end with a terminal step
// then print the results
aus=g.V().has('code','AUS').valueMap().next()
println "The AUS airport is located in " + aus['city'][0] // need [0] because value is always a list

// store the result of a query into prexisting variable
// same as calling toList/toSet
a = [] // or a = [] as Set
g.V().has('airport','region','US-TX').values('runways').fill(a)


// working with collections
size()
uniqueSize()
asBulk() // converts a bulkSet to a key=value map where key = element and value = occurrence
```
