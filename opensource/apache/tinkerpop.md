# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- [tinkerpop quickref with code](./tinkerpop.quickref.md)
- TODO:
  - finish the bookmark section at the bottom
  - review and categorize the sections better
    - maybe put the gremlin examples in a separate file

## TLDR!

- [run this container](https://github.com/nirv-ai/dbs/tree/main/graph/tinkerpop)

## links

- [landing page](https://tinkerpop.apache.org/)
- [gremlin: tutorials](https://tinkerpop.apache.org/docs/current/tutorials/getting-started/)
- [gremlin: anatomy tutorial (deck)](https://www.slideshare.net/StephenMallette/gremlins-anatomy-88713465)
- [gremlin: console tutorial](https://tinkerpop.apache.org/docs/current/tutorials/the-gremlin-console/)
- [tinkerpop providers](https://tinkerpop.apache.org/providers.html)
- [gremlin query converter](https://www.gremlator.com/)
- [gremlin: google group to ask questions](https://groups.google.com/g/gremlin-users)

### MISC

- [dates: neptune gremlin datetime (groovy)](https://docs.aws.amazon.com/neptune/latest/userguide/best-practices-gremlin-datetime.html)
- [dates: neptune gremlin datetime (not groovy)](https://docs.aws.amazon.com/neptune/latest/userguide/best-practices-gremlin-datetime-glv.html)

### practical gremlin

- [AAA: bunch of query examples](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html#msc)
- [AAA: github](https://github.com/krlawrence/graph)
- [AAA: landing page](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html)
- [sample: apps](https://github.com/krlawrence/graph/tree/master/demos)
- [sample: code](https://github.com/krlawrence/graph/tree/master/sample-code)
- [sample: create graph from CSV](https://github.com/krlawrence/graph/blob/main/sample-code/GraphFromCSV.java)
- [sample: data startup script](https://github.com/krlawrence/graph/blob/main/sample-data/load-air-routes-graph-34.groovy)
- [sample: data](https://github.com/krlawrence/graph/tree/master/sample-data)

### docker

- [gremlin console](https://hub.docker.com/r/tinkerpop/gremlin-console/)
- [gremlin server](https://hub.docker.com/r/tinkerpop/gremlin-server)
- [blog: setup console in docker](https://emmer.dev/blog/creating-a-gremlin-playground/)

### Ref

- [AAA: console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console)
- [AAA: docs landing page](https://tinkerpop.apache.org/docs/current/reference/)
- [AAA: getting started tutorials](http://tinkerpop.apache.org/docs/current/tutorials/getting-started/)
- [AAA: recipes](https://tinkerpop.apache.org/docs/current/recipes/)
- [AAA: tinkerpop javadocs](https://tinkerpop.apache.org/javadocs/current/full/)
- [AAA: language variants](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-drivers-variants)
- [AAA: gremlin anatomy](https://tinkerpop.apache.org/docs/3.7.0/tutorials/gremlins-anatomy/)
- [meta properties](https://kelvinlawrence.net/book/Gremlin-Graph-Guide.html#metaprop)
- [server](https://tinkerpop.apache.org/docs/current/reference/#gremlin-server)
- [sessions](https://tinkerpop.apache.org/docs/current/reference/#console-sessions)
- [tinkergraph](http://tinkerpop.apache.org/docs/current/reference/#tinkergraph-gremlin)
- [javascript: imports](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-javascript-imports)
- [traversal steps](https://tinkerpop.apache.org/docs/3.7.0/reference/#graph-traversal-steps)
- [mergeV](https://tinkerpop.apache.org/docs/current/reference/#mergevertex-step)
- [mergeE](https://tinkerpop.apache.org/docs/current/reference/#mergeedge-step)
- [vertex properties (multi, meta, etc)](https://tinkerpop.apache.org/docs/3.7.0/reference/#vertex-properties)
- [clone vertex program: bulk loading](https://tinkerpop.apache.org/docs/3.7.0/reference/#clonevertexprogram)

### related

- [janusgraph: graph db](https://janusgraph.org/)
- gephi: open source tool for visualizing graph data

## best practices

- all of the tinkerpop documentation uses singlar (e.g. person) vs plural (e.g. people) for Vertex labels, unlike in relational dbs where its common to use the plural form for table names
- always include a label when filtering based on props
  - e.g. instead of `g.V().has(k,v)` use `g.V().has('this label only', k,v)`
- always use indices: `g.V()` and etc has to iterate over all the vertices and edges

### IMO

- a vertex should not have multiple adjacent vertices with the same incident edge
  - i.e. each V -> e -> V should have distinct edge labels to reduce the traversal complexity

## basics

- a graph computing framework and top level project hosted by the Apache Software Foundation.
- tinkerpop: a graph abstraction layer over different graph databases and different graph processors
- TinkerPop-enabled: a graph provider that implements tinkerpop's core API
  - enables consumers to code in an agnostic fashion and generally utilize any provider tinkerpop-enabled provider with minimal overhead

### common file formats

- GraphML: XML; supported by tinkerpop
  - can represent entire graphs
  - industry standard and widely adopted
  - does not support all of the data types and structures used by TinkerPop
- GraphSON: JSON; defined by Apache TinkerPop
  - can represent entire graphs
  - can represent the results of gremlin queries
  - important for gremlin server but not widely adopted outside of tinkerpop
  - fully supported by tinkerpop
- CSV: generally requires more than 1 CSV
  - vertex csv: all vertex data
  - edge csv: all edge data

### Graph Computing

- graph: a data structure composed of vertices and edges
- property graph: a directed, binary, attributed multi-graph
  - supports labels and properties on both vertices and edges

#### Structure (graph) API

- structure: the underlying verticies and edges, and the labels and properties applied to them
  - the topology formed by the explicit references between its vertices, edges, and properties.
  - generally relevant for graph providers, developers should use the traversal API
- primary components
  - Graph: e.g. `graph = TinkerGraph.open()`; maintains a set of vertices and edges, and access to database functions such as transactions.
  - Element: maintains a collection of properties and a string label denoting the element type.
    - Vertex: extends Element and maintains a set of incoming and outgoing edges.
    - Edge: extends Element and maintains an incoming and outgoing vertex.
  - Property<V>: a string key associated with a V value.
  - VertexProperty<V>: a string key associated with a V value as well as a collection of Property<U> properties (vertices only)

#### process (traversal) API

- traversal: the process of navigating the graph structure, i.e. querying the graph
- primary components
  - GraphTraversalSource: e.g. `g = traversal().with(graph)`; a generator of traversals for a particular graph, domain specific language (DSL), and execution engine.
    - maintains the configuration options `e.g. g.withSomeConfig().startStep()`
    - spawns GraphTraversals instances
  - Traversal<S,E>: a functional data flow process transforming objects of type S into object of type E.
    - GraphTraversal: a traversal DSL that is oriented towards the semantics of the raw graph (i.e. vertices, edges, etc.).
      - i.e. the steps that make up the gremlin language
      - GraphTraversal instances (e.g. `g.V()`) are created via start steps (i.e. the `V()`) and begin the traversal
      - the instances contain the steps that make up the Gremlin language
      - Each step returns a GraphTraversal so that the steps can be chained together in a fluent fashion
      - Components: e.g. `has(), outE()` etc
        - receives the output of the previous start step/component
      - step modulators: e.g. `by()` are NOT STEPS!!! they modify the behavior of the previous step
      - anonymous traversals: a traversal that is not bound to a GraphTraversalSource
        - e.g. `g.V().groupCount().by(label())` label is not bound to g
        - abcd
      - terminal steps: return a result and DO NOT return a GraphTraversal instance
      - expressions: anything thats not a step, step modulator or anonymous traversal
        - e.g. predicates like `within()`, string tokens, enum values, and classes with static methods that might spawn certain required values.
  - GraphComputer: a system that processes the graph in parallel and potentially, distributed over a multi-machine cluster.
    - VertexProgram: code executed at all vertices in a logically parallel manner with intercommunication via message passing.
    - MapReduce: a computation that analyzes all vertices in the graph in parallel and yields a single reduced result.

#### Data Model

- vertices: entity nodes
  - A vertex is adjacent to another vertex if they share an incident edge
- edges: relationships connecting nodes
  - A vertex has incident edges
- properties: represent and store data for both edges and vertices
  - key/value pair, where the key is always a character String
  - is attached to an element and an element has a set of properties
  - data types:
    - edges: String, Integer
    - vertices: edge types + Set, List
  - VertexProperty: interface for vertexes, extends Property
    - are immutable. When you update a vertex property, a new property object is created with a new ID and replaces the previous one
  - Property: interface for edge properties
- labels: group vertices and edges into classes or types
  - not all graph database engines provide support for indexing labels
    - its recommended to use a vertex or edge property, that can be indexed, as a surrogate for the label.
- ids: Every vertex, every edge and even every property in a graph has a unique ID that can be used to reference it individually or as part of a group
  - can be auto generated or user provided
  - Don’t rely on the graph preserving the ID values you provide. How IDs are managed will be graph database implementation dependent.
  - using IDs is typically very efficient; many of your queries will involve collecting one or more IDs and then passing those on to other queries or parts of the same query

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
- hosts the Gremlin-Groovy language; you can enter valid Groovy code directly into the console
- the most common method for performing ad hoc graph analysis, small to medium sized data loading projects and other exploratory functions.
- common files (docker perspective) located in /opt/gremlin-console
  - bin/
    - gremlin.sh: start script
  - conf/
    - remote.yaml: connection + serialization version for a remote/local gremlin server
- tldr
  - ending a cmd with `;[]` hides the console output
  - end a line with period, comma, etc or backslash like in shell for continuation

```sh
############### TLDR
# console cmds, always prefixed with `:`

############### quickies
# connect to a gremlin server
:remote connect tinkerpop.server conf/remote.yaml

# toggle/stop remote/local mode
:remote console # toggle
:remote close # a remote connection

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
:submit # a gremlin script to a gremlin server
:show imports

# any maven lib, e.g. :install com.datastax.cassandra cassandra-driver-core 2.1.9
# you should google to see whats available, theres bunches of plugins you can add to the console
:un/install


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
graph = TinkerGraph.open() # empty graph
# ^ TinkerFactory can be used to create a non empty graph
  # ^ .createClassic(): tinkerpop v2
  # ^ .createModern(): tinkerpop v3
  # ^ .createTheCrew() tinkerpop v3 latest (meta properties, multi properties, etc)
  # ^ .createGratefulDead() v3 with alot of data
graph.io(graphml()).readGraph('air-routes.graphml')

```

### server

- Allows hosting of graphs remotely via an HTTP/Web Sockets connection.
  - gremlin console -> gremlin server -> some graph db
    - console & server must be configured to use the same version of various classes
      - both contain yaml files to set this up
- among other things, Provides a method for non-JVM languages which may not have a Gremlin Traversal Machine (e.g. Python, Javascript, Go, etc.) to communicate with the TinkerPop stack on the JVM.
- main configuration files located in conf/gremlin-server
  - gremlin-server.yaml
  - bunch of properties files
  - empty-sample.groovy: sample file that you should modify/copypasta

```sh
# start the gremlin server passing in the conf file
gremlin-server.sh conf/gremlin-server/gremlin-server.yaml
# ^ or in the bg
export GREMLIN_YAML='conf/gremlin-server/gremlin-server.yaml'
bin/gremlin-server.sh start


```

# bookmark

- docs/3.7.0/reference
  - [anatomy tutorial deck](https://www.slideshare.net/StephenMallette/gremlins-anatomy-88713465)
  - [the graph process](https://tinkerpop.apache.org/docs/3.7.0/reference/#the-graph-process)
  - [typescript](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-javascript)
    - skipped: should eventually get to these one day
      - [configuration](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-javascript-configuration)
      - [transactions](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-javascript-transactions)
      - [lambdas](https://tinkerpop.apache.org/docs/3.7.0/reference/#gremlin-javascript-lambda)
- [recipes](https://tinkerpop.apache.org/docs/3.7.0/recipes/)
- practical gremlin
  - chapter 3: basics
    - 3.27.3. Limiting the results at each depth
      - last section is 3.31
  - we can probably skip these (for now) and focus on migration
    - chapter 5: misc queries
    - chapter 4: shiz we skipped
      - author said the API is lame and hasnt been updated
        - 4.16. Turning graphs into trees
      - not supported by neptune: closures, lambdas, meta properties, graph object, gremlin variables
        - 4.7.10. Adding properties to other properties (meta properties)
        - 4.7.11. Using unfold and WithOptions with Meta Properties
        - 4.12. Using Lambda functions
        - 4.12.2. Using lambdas with sack steps
        - 4.15. Using graph variables to associate metadata with a graph
          - 4.17. Creating a sub graph
        - 4.19.3. Introducing TinkerGraph indexes
        - 4.20.1. Introducing the TinkerPop Graph Computer
        - 4.20.2. Experiments with Page Rank
        - requires closures
          - 4.12.4. Using regular expressions to do fuzzy searches
          - 4.13.1. Creating a regular expression predicate
          - 4.13. Creating custom tests (predicates)
          - 4.19.1. Timing a query - introducing clock and clockWithResult
            - should probably return to this
