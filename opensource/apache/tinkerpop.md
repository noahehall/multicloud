# TinkerPop

- a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP).
- intended for those interested in AWS neptune but prefer to rampup via tinkerpop
- bookmark
  - chapter 3: basics
    - 3.27.3. Limiting the results at each depth
      - last section of chapter 3 === 3.31
  - chapter 4: beyond the basics
    - 4.5.2. Using a traversal to determine a new label name
  - chapter 8: common graph serialization formats
  - we can probably skip these and focus on migrating from postgres to neptune
    - chapter 5: misc queries
    - chapter 6: beyond gremlin console & tinkergraph
    - chapter 7: introducing gremlin server

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
- the most common method for performing ad hoc graph analysis, small to medium sized data loading projects and other exploratory functions.
- hosts the Gremlin-Groovy language; you can enter valid Groovy code directly into the console
- tldr
  - ending a cmd with `;[]` hides the console output
  - end a line with period, comma, etc or backslash like in shell for continuation

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
- FYI
  - most steps
    - that accept 1 param, generally accept more than 1 param
    - accept a static/dynamic value for filtering
  - the double underscore `__` can be used similarly to the `idenity()` step
    - `__` === the current element

#### gremlin quick ref

```ts
////////////////// Graph object
graph = TinkerGraph.open()
graph
  .features() // what tinkerpop features are supported
  .toString() // basic stats about the graph

////////////////// graph traversel source object
// FYI most things below are on this object
g = graph.traversal()
  .V() // all vertices, or V(1234) or V(12,34,56) V(varContaingAPreviousResult)
  .E() // all edges, can also limit by passing some 1/more ids



////////////////// traversal steps;
// all accept labels as filters
// bidrectional
both() // Both incoming and outgoing adjacent vertices.
bothE() // Both outgoing and incoming incident edges.
bothV() // vertices at both ends of an edge
// undirectional
in() // Incoming adjacent vertices; need to invoke as __.in() within gremlin console
inE() // Incoming incident edges.
otherV() // vertex at the other end of an edge, relative to wherever you started
out() // Outgoing adjacent vertices
outE() // Outgoing incident edges

// dont accept edge labels as filters
outV() // Outgoing vertex.
inV() // Incoming vertex.


////////////////// filters: basic
has() // e.g. has(['label',] 'prop', 'value')
hasId() // shorthand for has(id,12345)
hasLabel() // e.g. hasLabel('this', 'or', 'that')
hasNext() // check if an edge exists between two vertices, eg.
hasNot() // shorthand for not(has('prop'))
filter()

////////////////// filters: comparisons
eq() // equal
gt()
gte()
lt()
lte()
neq() // not equal

////////////////// filters: logical
and()
not()
or()
is()
where()

////////////////// filters: ranges
between() // Between two values inclusive/exclusive (upper bound is excluded) e.g. hasId(between(1,6))
inside() // Inside a lower and upper bound, neither bound is included.
outside() // Outside a lower and upper bound, neither bound is included.
within() // Must match at least one of the values provided. Can be a range or a list
without() // Must not match any of the values provided. Can be a range or a list


////////////////// filters: text search case sensitive
startingWith()
endingWith()
containing()
notStartingWith()
notEndingWith()
notContaining()

////////////////// terminal steps: stops traversal and generates a result set
next() // returns object by default, pass an int to return the next(10) elements as an array
toList() // array
toSet() // math set
bulkSet() // weight set; the count of each element in the set
// dunno if these are classified as terminal steps
fold() // converts results to an array
unfold() // unbundles a collection


////////////////// limiting results
limit() // first X results, e.g. limit(10), limit has a global scope and not local to any specific step
tail() // last X results
range() // between X and Y, start inclusiv, 0 indexed, e.g. range(0, 20) first 20, range(10, -1) until end
skip() // the first X and return remaining, shorthand for range(X, -1)
timeLimit() // limit query to X milliseconds, e.g. timeLinit(10)
dedup() // remove duplicates, aka unique?; can provide references to limit specific steps


////////////////// modulators: alter the behavior of the steps that they are associated with.
as() // create a reference so you can refer to it later
// processed in a round robin fashion in cases where there are more elements that modulators specified
// ^ e.g. if path() returns [el1, el2, el3] and you only specify 1 by(), its executed for each
// ^ use a by modulator with no param if the element is not a vertex/edge
store() // same as AS, but works with sideEffect? dunno find it in the apache docs
aggregate() // same as AS, but tracks the results of some traversal in a temporary variable
by() // for each element returned, extract this prop/run this fn
from()
to()
with() // e.g. with(WithOptions.tokens,WithOptions.labels, WithOptions.ids)
path() // get a summary of the path walked; more expensive (memory, cpu) than select + as
simplePath() // shortest + unique path between two entities;
// execute steps based on the current state of a traversal
// ^ this is an extremely important concept to master
local() // e.g. calculate the count before taking the avg local(out('edges').count()).mean()

////////////////// CRUD: READ
values() // for 0/more keys, e.g. values('a', 'b', ...)
// valueMap(true): return ID an label of element; prefer the WITH modulator starting from 3.4
// ^ e.g. valueMap().with(WithOptions.tokens)
valueMap() // all/specific properties as an array of key=[value] pairs; value is always an array
// vertices: returns id + label and the first value in a list as a primitive for each property
// edges: id + label + the id & label of in + outgoing verticies as well
elementMap() // enhanced valueMap
label()
// the keywords are used in a query when as() is used to supply multiple labels to the same el
// ^ e.g. g.V(1).as('a').V(2).as('a').select(first,'a')
// ^ you often need to select when multiple references are created, else only the last is returned
select() // extract values/references; e.g. .select([first|last|all,]'propOrReferenceName')
project() // shorthand for as and select steps; creates projection of results
id() // of current element
identity() // the input element of the current step, i.e. the el that was output from the previous step

////////////////// CRUD: modification
// via the graph object
// ^  best practice to use the g object when adding, updating or deleting vertices and edges
addVertex()
// via the traversal source object
addV()
addE()
addEdge()
property() // e.g. ([Cardinality.single?,] 'key', 'val') <-- else it uses Cardinality.set and appends the value
// drop




////////////////// maths
count()
mean()
sum()
max() // numbers/strings
min() // numbers/strings
coin() // % of picking a value from a set, e.g. coin(.75)
sample() // randomly pick exactly X form a set, e.g. sample(20)


////////////////// aggregates
groupCount()
by()
group() // collect things into a map of key:value pairs
union() // collect multiple traversal results


////////////////// control structs
coalesce() // returns the first truthy result, e.g. coalesce(x, y, ...z)
choose() // if/else? e.g. choose(test, truthy[, falsy])
option() // combine with choose to create case statement; e.g. option(test, truthy), test can be a static/dynamic value
match() // pattern matching, e.g. match(x, y, ...z) each being a traversal
optional() // return optional, else the previous step, e.g. returnThis().optional(ifThisFalsy())
// loops
emit() // yield the result for each step in a traversal
times()
loops()
repeat() // make sure you have an ending step, e.g. times, emit, loops or until
until()

////////////////// other steps
getMethods() // list of all the methods and their supported types on the current element
getClass() // get element class


////////////////// useful steps
sideEffect() // execute steps without changing what gets passed on to the next stage of the query
join()
order() // default ascending, else use order(desc)
constant() // return a constant value e.g. constant('this value')
inject() // some values in the result set




////////////////// enums/constants/keywords
label //
first|last|all //
id //
asc // ascending
incr // increasing; prefer asc
desc // descending order
decr // decreasing; prefer desc
shuffle // as in random
values // the values without the keys, e.g. select(values)
keys // the keys without the values, e.g. select(keys)
withOptions.{x,y,z} // e.g. tokens, labels, ids, ...}
```

#### gremlin useful examples

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
// using where & is
g.V().where(label().is(eq('airport'))).count()
// average routes per airport (wrong, the query is re-written before mean() is invoked)
g.V().hasLabel('airport').out('route').count().mean()
// ^ the correct way to calculate the average via local
g.V().hasLabel('airport').local(out('route').count()).mean()
// order by STRING and locally group results,
// ^ returns [[A, zzz], [B, zzz], [C, zzz]] <- local groups its item into a sub array
// ^ instead of [A,zzz, B,zzz, C,zzz]  <- is just a flat list
g.V().has('region','GB-SCT').order().by('code').
      local(values('code','city').fold())
// airports with at least 5 runways
g.V().has('runways',gte(5)).values('code','runways').fold()
// airports with exactly 3 runways, can use has without eq(1)
g.V().has('runways',3)
// using between to simulate a startsWith search
g.V().hasLabel('airport').
      has('city',between('Dal','Dam')). // i.e. starts specifically with Dal as it Dam is excluded
      values('city')
// ^ or a string that starts with a single character
g.V().has('airport','code',between('X','Xa')). // specifically X
      values('code').fold()
// Flights from Austin to countries outside the US and Canada
g.V().has('code','AUS').out().has('country',without('US','CA')).values('city')
// Pick 20 airports at random with an evenly biased coin (50% chance).
g.V().hasLabel('airport').coin(0.5).limit(20).values('code')
// pick 20 random airports; FYI sample isnt as random as you may think
// ^ i.e. its skewed to lower bounded values
g.V().hasLabel('airport').sample(20).values('code')
// case insensitive text search via OR
g.V().hasLabel('airport').
      or(has('city',startingWith('dal')),
         has('city',startingWith('Dal'))).
      dedup().by('city').
      count()
// sort in a random order
g.V().hasLabel('airport').limit(20).values('code').order().by(shuffle).fold()
// List the 10 airports with the longest runways in decreasing order.
g.V().hasLabel('airport').order().by('longest',desc).valueMap().
      select('code','longest').limit(10)
// Query and order by airport code (the key)
g.V().hasLabel('airport').limit(5).
      group().by('code').by('runways').
      order(local).by(keys,asc) // local required for order to execute appropriately
// all airports FROM austin that have the same number of runways AS austin
// pay attention to chaining WHERE and BY modulator
 g.V().has('code','AUS').as('a').out().
       where(eq('a')).by('runways').valueMap('code','runways')
// ^ same as above but without WHERE BY
g.V().has('code','AUS').as('a').out().as('b').
      filter(select('a','b').by('runways').where('a',eq('b'))).
      valueMap('code','runways')
// all airports FROM austin that have fewer runways THAN austin
// again, pay attention to chaining WHERE and BY
g.V().has('airport','code','AUS').as('a').out().as('b').
      where('a',gt('b')).by('runways').valueMap('code','runways')
// If an airport has a runway > 12,000 feet return its code else return its description
g.V().has('region','US-TX').
      choose(values('longest').is(gt(12000)),
             values('code'),
             values('desc')).
      limit(5)
// choose with constant & by modulator
g.V().hasLabel('airport').sample(10).as('a').
      choose(out('route').count().is(gt(50)),
             constant('lots'),
             constant('not so many')).as('b').
      select('a','b').by('code').by()
// using CHOOSE and OPTION to create case statements
// none case is optional
g.V().hasLabel('airport').
      choose(values('code')).
        option('DFW',values('desc')).
        option('AUS',values('region')).
        option('LAX',values('runways')).
        option(none, constant('not DFW, AUS, or LAX'))
// ^ same as above but supplying an additional test for each option
g.V().hasLabel('airport').
      groupCount().
        by(choose(values('elev')).
             option(gt(5000),constant('high')).
             option(gt(3000),constant('medium')).
             option(none,constant('low')))
// any airport you can fly from non stop, but cant return from non stop
// probably should spend some time getting use to MATCH
g.V().hasLabel('airport').
      match(__.as('s').out().as('d'),
            __.not(__.as('d').out().as('s'))).
      select('s','d').by('code')
// ^ same query but without using match
g.V().hasLabel('airport').as('s').
      out().as('d').
      where(__.not(out().as('s'))).
      select('s','d').by('code')
// retrieve a specific airport and the total outgoing routes
// ^ FYI out() refers to the traversal immediately before the union() step, i.e. as('a')
g.V().union(has('airport','code','DFW'),
            has('airport','code','DFW').
               out().count()).fold()
// ^ same as above but without duplication
g.V().has('airport','code','DFW').as('a').
      union(select('a'),out().count()).fold()
// ^ same as above but using identity instead of SELECT and AS
g.V().has('airport','code','DFW').
      union(identity(),out().count()).fold()
// ^ same as above but using GROUP and BY to produce a map instead of a list
g.V().has('airport','code','DFW').
      group().by().by(out().count())
// interesting use of UNION and CONSTANT
g.V(3).union(constant("Hello"),
             constant("There"),
             identity()).fold() // returns [hello, there, v[3]]
// info about 10 random airports; notice the use of local
g.V().hasLabel("airport").sample(10).
      local(union(id(),values("code","city")).fold()) // [id, airport code, airport city]
// store and output a reference to a traversal
// ^ notice the use of STORE instead of AS
g.V(3).sideEffect(out().count().store('a')).
       out().out().count().as('b').select('a','b')
// all the places you can go to FROM austing with ONE stop
// ^ aggregate tracks all the places you can go to austing NON stop
// ^ the where removes the NON stop vertices then dedupes
g.V().has('code','AUS').out().aggregate('nonstop').
     out().where(without('nonstop')).dedup().count()
// using inject with a useless value in order to execute some other query
// ^ CHOOSE must be executed on an object, and not directly on g.choose e.g.
// ^ i.e. this is better than using g.V().choose()
g.inject(1).choose(V().hasLabel('XYZ').count().is(0),constant("None found"))
// returns the previous step if optional returns nil
g.V().has('code','AUS').optional(out().has('code','doesnt exist')).values('city')
// ^ same as above, but with COALESCE and IDENTITY
g.V().has('code','AUS').coalesce(out().has('code','doesnt exist'),identity()).values('city')
// shortest, unqiue paths between two verticies
g.V().has('code','AUS').
      repeat(out().simplePath()).
        until(has('code','AGR')).
        path().by('code').limit(10)
// unique paths with 1 stop between two verticies
g.V().has('code','AUS').repeat(out()).times(2).has('code','SYD').path().by('code')
// looping: times vs until; the following do the same thing
g.V(3).repeat(out()).until(loops().is(2)).count()
g.V(3).repeat(out()).times(2).count()
// times with + without emit
// ^ exactly 3 hops between two verticies limit 5: notice no EMIT
g.V(3).repeat(out()).times(3).has('code','MIA').
       limit(5).path().by('code')
// ^ at most 3 hops, limit 5: notice the EMIT before times
g.V(3).repeat(out().simplePath()).emit().times(3).has('code','MIA').
       limit(5).path().by('code')
// ^ same as above but using OR
g.V(3).repeat(out().simplePath()).
         until(has('code','MIA').or().loops().is(3)).
       has('code','MIA').
       path().by('code').limit(5)
// nested repeat + loops require naming each
g.V().has('code','SAF').
      repeat('r1',out().simplePath()).
        until(loops('r1').is(3).or().has('code','MAN')).
      path().by('city').
      limit(3).
      toList()
// Add an imaginary airport with a code of 'XYZ' and connect it to DFW
g.addV('airport').property('code','XYZ').
                        property('icao','KXYZ').
                        property('desc','This is not a real airport').next()
g.V().has('code','DFW').addE('route').to(V().has('code','XYZ'))
// Add the return route back to DFW
g.V().has('code','DFW').addE('route').from(xyz)
// ^ using as
g.V().has('code','XYZ').as('a').V().has('code','DFW').addE('route').to('a')










////////////////////////////////// dunno where these came from (aws docs?)
// should be updated to use the air routes data or deleted

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
- builds upon everything in the quickref, as the gremin console is a Groovy repl

```groovy

// FYIs
__.in() // in is a reserved word in groovy, but a fn in gremlin

// assign query results to a var; must end with a terminal step
// then print the results (make sure to call next())
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



// A simple function to return the distance between two airports
// dist(g,'AUS','MEX')
def dist(g,from,to) {
  d=g.V().has('code',from).outE().as('a').inV().has('code',to)
         .select('a').values('dist').next()
  return d }


// Using a Groovy for() loop to iterate over a list returned by Gremlin
x=g.V().hasLabel('airport').limit(10).toList()
for (a in x) {println(a.values('code').next()+" "+a.values('icao').next()+" "+a.values('desc').next())}


// working with maps
a=g.V().group().by(label).by('code').next()
println(a["country"].size())
println(a["country"][5])
println(a["airport"][2])


```
