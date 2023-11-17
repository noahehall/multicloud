# quick ref

## links

- [withOptions](https://tinkerpop.apache.org/javadocs/current/full/org/apache/tinkerpop/gremlin/process/traversal/step/util/WithOptions.html)

## FYI

- tested with tinkergraph and the practical gremlin air routes data file
- most steps
  - that accept 1 param, generally accept more than 1 param
  - accept a static/dynamic value for filtering
- the double underscore `__` can be used similarly to the `idenity()` step
  - `__` === the current element
- many steps generate collections: group, groupCount, fold, union, project, etc
- Both aggregate and store can be followed by a by modulator to specify more precisely what should be collected
- keys & values steps can be used with maps (duh!)
- understanding the local keyword is crucial for ordering, counting and running calculations on collections
- pay attention to whats returned from a traversal
  - if its not a primitive (e.g. string, int, etc) you likely cant operate on it
  - use the UNFOLD step to extract things that are returned as a collection
- side effect steps: can extract & store/operate on values during a traversal but has no affect on what is passed to the next step
  - sack, sideEffect, withSideEffect
- Avoiding unnecessary use of closures is a Gremlin best practice.
- path, simplePath and as steps can be memory intensive
  - prefer sack > as > simplePath > path
- barrier steps: eagerly evaluates its traversers and outputs a result
  - collecting barriers: outputs a collection
    - order, sample, aggregate, barrier
  - reducing barriers: outputs a single value; the path history leading up to a reducing barrier step is destroyed and cant be referred back to
    - fold, count, sum, max, min
  - supplying barriers: evaluates all traversers, and you supply which to emit
    - cap
  - TraverserVertexProgram (TODO)
- scopes: either local or global
- watch out for barriers!
  - generally moving the barrier step into a an anonymous traversals unblocks you
- watch out for lists!
  - if you need to work with a previous retrieved vertex/edge make sure you unfold it

## console

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

## tinkergraph

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

## server

```sh
# start the gremlin server passing in the conf file
gremlin-server.sh conf/gremlin-server/gremlin-server.yaml
# ^ or in the bg
export GREMLIN_YAML='conf/gremlin-server/gremlin-server.yaml'
bin/gremlin-server.sh start


```

## gremlin cheatsheet

- theres a million ways to do something in gremlin, pick the simplest/most efficient
- examples copypastad from the book is useful for learning, perhaps not so much as production code
  - the author repeatedly states this!
- iterators vs terminal steps
  - iterators: dont execute the underlying traversal (unless in console magicland)
    - e.g. `g.V()` returns an iterator, in typescript land you need to invoke something like `toList()`
  - terminal steps: execute all steps within an iterator and return the results

```ts
////////////////// Graph object
// holds a reference to the underlying data in a graph
graph = TinkerGraph.open()
graph
  .features() // what tinkerpop features are supported
  .toString() // basic stats about the graph
  .createIndex('userId', Vertex.class) // index userId label on vertices
  .variables()
    .set('maintainer','Kelvin') // set a variable
    .keys() // retrieve all graph variables
    .asMap()
    .get('someProp')
    .remove('someProp')
  .io(graphml()).writeGraph('my-graph.graphml') // persist to disk
  // each vertex and all of its edges as a single JSON object, one per line
  // better for ingesting large amounts of data back into tinkerpop
  .io(graphson()).writeGraph("my-graph.json") // persist as unwrapped json


// create via graph
// ^  best practice to use the g object when adding, updating or deleting vertices and edges
addVertex()
addEdge()
 ////////////////// data I/O
// persist as wrapped json; i.e. wrapped adjacency list
// all vertices and edges stored in a single large JSON oject inside of an enclosing vertices object.
fos = new FileOutputStream("my-graph.json")
GraphSONWriter.build().wrapAdjacencyList(true).create().writeGraph(fos,graph)

// persist data using an older graphson version (the default is 3.0)
fos = new FileOutputStream("my-graph.json")
mapper = graph.io(IoCore.graphson()).
         mapper().version(GraphSONVersion.V1_0).create() // specify the version here
graph.io(IoCore.graphson()).
    writer().mapper(mapper).
    create().writeGraph(fos, graph)

// load a graphml file
graph.io(IoCore.graphml()).readGraph('my-graph.graphml')

// Load a grapSON file
graph.io(IoCore.graphson()).readGraph('my-graph.json')

// view query results as json
json_mapper = GraphSONMapper.
                build().
                version(GraphSONVersion.V3_0). // V{1,2}_0,
                create().
                createMapper()
lax = g.V().has('code','LAX').next()
json_mapper.writeValueAsString(lax)


////////////////// GraphTraversalSource
// instantiate `g` once and reuse it
g = traversal()
    .withEmbedded(graph) // restricted to languages using a JVM
// set configuration options for a specific traversal
// always g.withSomeConfigOption
g.withStrategies(SubgraphStrategy.build().vertices(hasLabel('person')).create()).
  V().has('name','marko').out().values('name')
g.withSack(1.0f).V().sack()
g.withComputer().V().pageRank()

////////////////// GraphTraversal: steps;
///////// start steps
// create an element that starts a traversal
addV()
addE()
// retrieve an element that starts a traversal
E()
V()
// insert arbitrary objects to start a traversal
inject()

///////// terminal steps: stops traversal and generates a result set
bulkSet() // weighted set; the count of each element in the set
fill() // put all results in the provided collection and return the collection when complete
hasNext() // whether there are available results
iterate() // generates side effects without returning the actual result
next() // returns next result, pass an int to return the next(10) elements as an array
toList() // array
toSet() // math set
tryNext() // return an Optional and thus, is a composite of hasNext()/next()

////////////////// side effects
// collect all the objects at a particular point of traversal into a Collection; can be passed to by modulator
// ^ (local, 'x') uses lazy evaluation
// ^ (global, 'x') === ('x') uses eager evaluation; use when the result is requird in full for the next step
aggregate() // stores the collection in string you provide, e.g. aggregate('x')


////////////////// predicates: P objects
eq() // equal
neq() // not equal
within() // Must match at least one of the values provided. Can be a range or a list
without() // Must not match any of the values provided. Can be a range or a list

////////////////// predicates: P numbers
lt()
lte()
gt()
gte()
between() // Between two values incl/excl e.g. hasId(between(1,6))
inside() // Inside a lower and upper bound, neither bound is included.
outside() // Outside a lower and upper bound, neither bound is included.


////////////////// predicates: TextP strings
startingWith()
endingWith()
containing()
notStartingWith()
notEndingWith()
notContaining()
regex()
notRegex()

////////////////// collections
fold() // converts results to an array
unfold() // unbundles a collection

////////////////// maths
mean()
coin() // % of picking a value from a set, e.g. coin(.75)
sample() // randomly pick exactly X form a set, e.g. sample(20)
math() // run arbitrary calculations
count() //
max() // numbers/strings
min() // numbers/strings
sum() //

////////////////// READ
get() // the current traversed object
loops() // number of times the traverser has been here
bulk() // number of objects in the traverser
// initial a sack with runways: g.V().has('code','AUS').sack(assign).by('runways').
sack() // local data structure of the traverser, use sack(sum|addAll|mult|minus|etc)
sideEffects() // associated with the traverser
value() // value of a property
values() // for 0/more properties, e.g. values('a', 'b', ...)
// valueMap(true): return ID an label of element; prefer the WITH modulator starting from 3.4
// ^ e.g. valueMap().with(WithOptions.tokens)
valueMap() // all/specific properties as a map of key=[value] pairs; value is always an array
// vertices: returns id + label and the first value in a list as a primitive for each property
// edges: id + label + the id & label of in + outgoing verticies as well
elementMap() // enhanced valueMap, see above comments
propertyMap() // enhanced valueMap that includes the vertex property objects for each property.
label() // retrieves the label
// the keywords are used in a query when as() is used to supply multiple labels to the same el
// ^ e.g. g.V(1).as('a').V(2).as('a').select(first,'a')
// ^ you often need to select when multiple references are created, else only the last is returned
// ^ to select nested properties: select('outer').select('inner')
select() // extract values/references; e.g. .select([first|last|all,]'propOrReferenceName')
project() // shorthand for as and select steps; creates projection of results
properties() // all or a specific set
id() // of current element
identity() // the input element of the current step, i.e. the el that was output from the previous step
key() // key of a property
keys()
element() // retrieves the element
graph() // retrieves the graph associated with a thing


////////////////// UPDATE
// also check start steps for other CRUD steps
// FYI:
// ^ vertex creation: appends unless single passed as first prop, e.g. property(Cardinality.single,'code','ABIA')
// ^ modification: replaces unless list pass as first prop, e.g. property(list,'code','ABIA')
property() // overloaded step enabling create/read/updating properties
drop() // a vertex and all connected edges, or a specific edge/property


///////// main steps: all others are based on these and you should reach for those first
// if it receives multiple inputs, it only passes the first one received to the next step
// transform the incoming object to another object (S → E).
map()
// if it receives multiple inputs, it passes them all on to the next step
// transform the incoming traverser’s object to an iterator of other objects (S → E*).
flatMap()
filter() // exactly what you think
sideEffect() // execute some process on incoming, but pass it through unchanged
branch() // split incoming and process them separately


///////// Verticies and Edges
// all accept labels as filters
both() // Both incoming and outgoing adjacent vertices.
bothE() // Both outgoing and incoming incident edges.
bothV() // vertices at both ends of an edge

///////// undirectional
in() // Incoming adjacent vertices; need to invoke as __.in() within gremlin console
inE() // Incoming incident edges.
otherV() // vertex at the other end of an edge, relative to wherever you started
out() // Outgoing adjacent vertices, when you dont care about the edge and just want the vertex, e.g. out('label')
outE() // Outgoing incident edges, when you need to inspect the edge

// dont accept edge labels as filters
outV() // Outgoing vertex.
inV() // Incoming vertex. e.g. g.V(1).outE('knows').inV()


////////////////// filters: basic
has() // e.g. has(['label',] 'prop', 'value')
hasId() // shorthand for has(id,12345)
hasLabel() // e.g. hasLabel('this', 'or', 'that')
hasNext()
hasNot() // shorthand for not(has('prop'))
hasValue()
where()


////////////////// filters: logical
and()
not()
or()
is()
test() // requires closures, dunno if supported in neptune, skipped


////////////////// filters: text search case sensitive
// available since 3.4; on the TextP class
startingWith()
endingWith()
containing()
notStartingWith()
notEndingWith()
notContaining()



////////////////// limiting results
limit() // first X results, e.g. limit(10), limit has a global scope and not local to any specific step
tail() // last X results
range() // between X and Y, start inclusiv, 0 indexed, e.g. range(0, 20) first 20, range(10, -1) until end
skip() // the first X and return remaining, shorthand for range(X, -1)
timeLimit() // limit query to X milliseconds, e.g. timeLinit(10)
dedup() // remove duplicates; can provide references to limit specific steps


////////////////// modulators: alter the behavior of the steps that they are associated with.
// provide a label to the current step for retrieving during a later step
// used with select, match, etc
as()
cap() // iterates and emits the sideEffect referenced by the provided key(s).
// will only add things to its collection as they are seen
store() // same as aggregate, but is lazy
// will block and immediately gather up everything from the prior traversal
aggregate() // similar to store, but is greedy, tracks the results of some traversal in a temporary variable
// specify how items are added to the collection:
// ^ e.g. addition, multiplication, subtraction, division
// ^ e.g. minimum or maximum value of a pair of values
sack() // like aggregate & store, but way more powerful
withSack() // initializes a sack variable, initialize iwht something like [:] or []
// determines the property to group on or fn to run
// remember its passed into the previous step
// used with bunches of steps, check the docs for how by is consumed which is not always apparent
// if it doesnt produce a result, its deemed unproductive and is handled differently by each step
// processed in a round robin fashion in cases where there are more elements that modulators specified
// ^ e.g. if path() returns [el1, el2, el3] and you only specify 1 by(), its executed for each
// ^ use a by modulator with no param if the element is not a vertex/edge
by() // some steps only accept 1 by, others an arbitrary amount
from()
to()
with() // e.g. with(WithOptions.tokens, ...)
path() // get a summary of the path walked
simplePath() // shortest + unique path between two entities;
// execute steps based on the current state of a traversal
// ^ this is an extremely important concept to master
local() // e.g. calculate the count before taking the avg local(out('edges').count()).mean()
option() // combine with choose to create case statement; e.g. option(test, truthy), test can be a static/dynamic value


////////////////// aggregates
groupCount()
group() // collect things into a map of key:value pairs
union() // collect multiple traversal results


////////////////// control structs
coalesce() // returns the first truthy result, e.g. coalesce(x, y, ...z)
choose() // if/else? e.g. choose(test, truthy[, falsy])
match() // pattern matching, e.g. match(x, y, ...z) each being a traversal
optional() // return optional, else the previous step, e.g. returnThis().optional(ifThisFalsy())
// loops
emit() // yield the result for each step in a traversal
times() // abcd
loops() // abcd
repeat() // make sure you have an ending step, e.g. times, emit, loops or until
until() // abcd


////////////////// other steps
getMethods() // list of all the methods and their supported types on the current element
getClass() // get element class

////////////////// profiling
// TimeUtil class: timing queries
// executes a query one/more times and returns the avg in MS
// ^ always executes a warm up pass; so human perceived time is double whats reported
// ^ swing back to this, requires a closure so we skipped it
clock()
clockWithResult()
// ProfileStep class: profiling queries
profile() // MS per step in a query

////////////////// useful steps
withSideEffect() // dunno, check the docs
join()
order() // default ascending, else use order(desc)
constant() // return a constant value e.g. constant('this value')
explain() // instead of executing a query




////////////////// enums/constants/keywords/etc
// mixed: returns a list if 2/more items exist, else a list
Pop.{first,last,all,mixed} // selecting an item from a collection
T.{label,id,key,value} // for valueMap/properties
Column.{keys,values} // for map datatype
Scope.{local,global} // global is default
// both incr and decr deprecated 3.4; removed 3.5
// shuffle as in random
Order.{desc,asc,shuffle} // generally used like order().by(desc/asc/shuffle)
withOptions.{tokens, labels, ids, keys, all} // , ...
Cardinality.{single, set, list} // how a property value should be treated when added to a vertex
// sack operators: defined on the java Operator enum
// minus: subtracted from the sacks initialize value
// mult multiply
Operator.{sum,minus,mult,div,assign,min,max,addAll,and,or}
// used in associated with edge directions
Direction.{IN,OUT,BOTH} // e.g.  myEdge.vertices(Direction.IN)
// used in association with the branch, choose and option steps.
Pick.{none,any}
```
