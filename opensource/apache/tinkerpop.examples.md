# gremlin useful examples

- all based on air routes dataset & copypasta from the practice gremlin book

```ts
////////////////////////////////// exploration
// FYI: a good graph db usually has an API to explore the graph state
// 100 unique vertex and edge labels
g.V().label().dedup().limit(100)
g.E().label().dedup().limit(100)
// properties associated with a specific vertex/edge label
g.V().hasLabel('airport').limit(1).next().keys()
g.E().hasLabel('route').limit(1).next().keys()
// How many of each type of vertex/edge are there
g.V().groupCount().by(label); // use g.E() for edges
// ^ different ways to get the same thing
g.V().label().groupCount();
g.V().group().by(label).by(count());


////////////////////////////////// DELETE
// vertex
g.V().has('code','XYZ').drop()
// edge: note how we examine both ends of the edge before dropping it
// ^ and do it in both directions
g.V().has('code','AUS').outE().as('e').inV().has('code','LHR').select('e').drop()
g.V().has('code','LHR').outE().as('e').inV().has('code','AUS').select('e').drop()
// or remove all edges, or everything from the graph
g.E().drop()
g.V().drop() // drops everything
// property: a specific, or all properties
g.V().has('code','AUS').properties().hasValue('ABIA').drop() // a specific value from a property list
g.V().has('code','SFO').properties('desc').drop() // the property and ALL its values
g.V().has('code','SFO').properties().drop() // all properties


////////////////////////////////// CREATE
// airport with a code of 'XYZ' and connect it to DFW
g.addV('airport').property('code','XYZ').
                        property('icao','KXYZ').
                        property('desc','This is not a real airport').next()
// edge TO the created vertex
g.V().has('code','DFW').addE('route').to(V().has('code','XYZ'))
// edge FROM the created vertex
g.V().has('code','DFW').addE('route').from(xyz)
// ^ using as
g.V().has('code','XYZ').as('a').V().has('code','DFW').addE('route').to('a')
// use an existing elements label for a new vertex
g.addV(V().has('code','AUS').label()).property('code','XYZ')
// ^ same but for an edge
g.V(53768).addE(V().has('code','AUS').outE().limit(1).label()).
           to(V().has('code','AUS'))
// ^ same but for a property; sets [places:[[a, b, ...c]]]
g.V().has('code','AUS').property('places',out().values('code').fold())
// using inject to create multiple vertices with custom IDs
g.inject(99997L,99998L).addV().property(id,identity())
// same as above
g.addV().property(id,99999L)
// add multiple properties to an existing vertex
g.V().has('code','AUS').
      property(list,'desc','Austin Airport'). // we need LIST else it replaces
      property(list,'desc','Bergstrom')
// ^ or as a set, so it doesnt add duplicates
g.V(3).property(set,'hw','hello').property(set,'hw','world')

// everything above can be changed in one long script
graph=TinkerGraph.open()
g=graph.traversal()
g.addV('airport').property('code','AUS').as('aus').
  addV('airport').property('code','DFW').as('dfw').
  addE('route').from('aus').to('dfw').
  addE('route').from('aus').to('atl')

////////////////////////////////// UPSERT
// if unfold() returns nil it creates a new vertex
g.V().has('code','XYZ').fold().coalesce(unfold(),addV().property('code','XYZ'))
// if the property doesnt exist it adds it
g.V().has('code','XYZ').coalesce(has('creditScore'), property('creditScore', 'AAA+'))
// if vertex with ID exist it updates the property, else creates a new vertex
g.V(3).fold().
       coalesce(unfold().property('runways',3),
       addV('airport').property('runways',3))
// create a new vertex & property based on some information
g.V().has('code','DFW').group().by().by(outE().count()).as('ct').
      addV('dfwcount').
      property('current_count',select('ct').unfold().select(values))

////////////////////////////////// READ
// total airports by country
g.V().hasLabel("airport").groupCount().by("country");
// all paths from  AUS to AGR with exactly two stops
g.V().has("code", "AUS").out().out().out().has("code", "AGR").path().by("code");
// same as above but using repeat fn
g.V().
  has("code", "AUS").
  repeat(out()).
  times(3).
  has("code", "AGR").
  path().
  by("code");
// how many routes leaving airports
g.V().hasLabel("airport").outE("route").count();
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
  .as("stop") // multiple AS to same label returns an array
  .limit(1)
  .select(all, "stop")
  .unfold() // thats why we need to unfold the array
  .values("code")
  .fold(); // so we can operate on each element
// select the edge connecting MIA and DFW verticies
g.V().has("code", "MIA").outE().as("e").inV().has("code", "DFW").select("e");
// ^ same as above but uses inE and outV
g.V().has("code", "MIA").inE().as("e").outV().has("code", "DFW").select("e");
// modulate how dedupe is applied to results
// return 1 airport for each unique number of runways
g.V()
  .has("region", "GB-ENG")
  .dedup()
  .by("runways")
  .values("code", "runways")
  .fold();
// unwrap a valuemap via an extra by(unfold()) step so all key=[value] are returned as strings
g.V().has("code", "SFO").valueMap().by(unfold()).unfold();
// examine an edge with elementMap
g.V(3).outE().limit(1).elementMap();
// ^ same as above but with valueMap
g.E(5161)
  .project("v", "IN", "OUT")
  .by(valueMap(true))
  .by(inV().union(id(), label()).fold())
  .by(outV().union(id(), label()).fold());
// using where & is
g.V().where(label().is(eq('airport'))).count()
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
// case insensitive text search via OR
g.V().hasLabel('airport').
      or(has('city',startingWith('dal')),
         has('city',startingWith('Dal'))).
      dedup().by('city').
      count()
// List the 10 airports with the longest runways in decreasing order.
g.V().hasLabel('airport').order().by('longest',desc).valueMap().
      select('code','longest').limit(10)
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
// probably should spend some time playing with MATCH
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
// ^ FYI out() refers to the traversal immediately before the union() step
g.V().union(has('airport','code','DFW'),
            has('airport','code','DFW').
               out().count()).fold()
// ^ same as above but but using SELECT and AS
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
// all the places you can go to FROM austing with ONE stop
// ^ aggregate tracks all the places you can go to austing NON stop
// ^ the where removes the NON stop vertices then dedupes
g.V().has('code','AUS').out().aggregate('nonstop').
     out().where(without('nonstop')).dedup().count()
// using inject with a useless value in order to execute some other query
// ^ CHOOSE must be executed on an object, and not directly on g
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
// get the element associated with a property
g.V().properties().hasId(583).next().element()
// get the graph an element belongs to
g.V().has('airport','code','DFW').properties('city').next().graph()
// retrieve the ID and total edges
 g.V().has('code','DFW').project('dfw','route_count').
       by().by(outE().count())
// all airports in countries flying to ireland
g.V().has('airport','country','IE').aggregate('ireland').
      out().where(without('ireland')).
      values('country').
      dedup().fold().order(local)
// extract the keys and values from a map (output of group()) separately
g.V().hasLabel('airport').limit(5).group().by('code').by('city').select(keys)
g.V().hasLabel('airport').limit(5).group().by('code').by('city').select(values)
// retrieve a property from vertices
g.V().hasLabel('airport').limit(10).map(properties('city'))
// ^ same as above, but without map
g.V().hasLabel('airport').limit(10).values('city')
// ^ same as above, but converts the result to a map via group and orders by values
g.V().hasLabel('airport').limit(10).
      map(properties('city').group().by(key()).by(value())).
      unfold().order().by(values,asc)
// map vs flatMap
g.V().has('code','SAF').map(out()) // returns a single vertex
g.V().has('code','SAF').flatMap(out()) // returns all vertices
// find all the paths from AUS to any other vertex and return the city
// ^ returns results like [Austin,e[3712][3-route->43],Tucson]
g.V().has('code','AUS').outE().inV().
      path().by('city').by().limit(3)
// ^ same as above, but this time use flatMap to remove the route and only return connecting cities
// ^ returns results like [Austin,Tucson]
g.V().has('code','AUS').flatMap(outE().inV()).
      path().by('city').limit(3)
// ^ lol same thing but without map ;)~
g.V().has('code','AUS').out().path().by('city').limit(3)


////////////////////////////////// maths
// Pick 20 airports at random with an evenly biased coin (50% chance).
g.V().hasLabel('airport').coin(0.5).limit(20).values('code')
// pick 20 random airports; FYI sample isnt as random as you may think
// ^ i.e. its skewed to lower bounded values
g.V().hasLabel('airport').sample(20).values('code')
// sort in a random order
g.V().hasLabel('airport').limit(20).values('code').order().by(shuffle).fold()
// calculate the avg using math
g.V().hasLabel('airport').limit(10).
      group().by('code').by(values('runways')).
      unfold().
      select(values).
      sum().
      math('_ / 10')
// ^ same as above but using project instead of hardcoding 10
g.V().hasLabel('airport').limit(10).
      group().by('code').by(values('runways')).
      project('total','number').
        by(select(values).unfold().sum()).
        by(count(local)).
      math('total / number')
// calculate the ratio between two traversals in one query
g.V().has('region','US-NM').
      group().by('code').by(values('runways')).
      select(values).
      unfold().
      sum().
      store('a').
      V().has('region','US-AZ').
      group().by('code').by(values('runways')).
      select(values).
      unfold().
      sum().
      store('b').
      project('first','second').
        by(select('a').unfold()).
        by(select('b').unfold()).
      math('first / second')
// routes from SAF with 1 between stop, distance per stop + total distance
g.withSack(0).
  V().has('code','SAF').
      repeat(outE().sack(sum).by('dist').inV()).times(2).limit(10). // the sack SUMs the distance
      order().by(sack()).
      sack().path().
      by('code').by('dist').by('code').by('dist').by('code').by() // the last by() is the sack
// ^ same as above but without repeat for clarity
g.withSack(0).
  V().has('code','SAF').
      outE().limit(10).sack(sum).by('dist').inV(). // sum SAF to first stop
      outE().limit(10).sack(sum).by('dist').inV(). // sum first top to destination
      sack().path().
      by('code').by('dist').by('code').by('dist').by('code').by()
// ^ same as above but with PROJECT, COALESCE and CONSTANT
g.V().has('code','SAF').
      repeat(outE().inV().simplePath()).times(2).limit(10).
        project('path','total').
          by(path().by('code').by('dist')).
          by(path().unfold().coalesce(values('dist'),constant(0)).sum()).
          order().by(select('total')).
          select(values)
// sum the total hops between two vertices
g.withSack(0).V().
  has('code','AUS').
  repeat(out().simplePath().sack(sum).by(constant(1))).
    until(has('code','WLG')).
  limit(10).
  local(union(path().by('code'),sack()).fold())


////////////////////////////////// using local effectively
// average routes per airport (wrong, the query is re-written before mean() is invoked)
g.V().hasLabel('airport').out('route').count().mean()
// ^ the correct way to calculate the average via local
g.V().hasLabel('airport').local(out('route').count()).mean()
// order by STRING and locally group results,
// ^ returns [[A, zzz], [B, zzz], [C, zzz]] <- local groups its item into a sub array
// ^ instead of [A,zzz, B,zzz, C,zzz]  <- is just a flat list
g.V().has('region','GB-SCT').order().by('code').
      local(values('code','city').fold())
// Query and order by airport code (the key)
g.V().hasLabel('airport').limit(5).
      group().by('code').by('runways').
      order(local).by(keys,asc) // local required for order to execute appropriately
// info about 10 random airports; notice the use of local
g.V().hasLabel("airport").sample(10).
      local(union(id(),values("code","city")).fold()) // [id, airport code, airport city]
// get the first/last 2 elements from a list
g.V().has('region','US-TX').values('code').fold().limit(local,2)
g.V().has('region','US-TX').values('code').fold().tail(local,2)
// create a map of labels:[codes] where codes is limited to 10 max for each label
g.V().has('code','AUS').
      both().
      group().
        by(label).
        by(values('code').fold().dedup(local).order(local).limit(local,10))
// create a map ordered by its values
g.V().hasLabel('airport').limit(10).
      group().by('code').by(values('runways')).
      order(local).by(values)
// calculate the sum of a maps values
// ^ e.g. in [FLL:2,AUS:2,DCA:3,BWI:3,ANC:3,BNA:4,IAD:4,ATL:5,BOS:6,DFW:7]
g.V().hasLabel('airport').limit(10).
      group().by('code').by(values('runways')).
      local(select(values).sum(local))



////////////////////////////////// reducing barrier steps
// expectation: this returns [a: vertex, b: theCount]
// ^ actual: nothing is returned, A cannot be retrieved because it was thrown away due to B's count step
g.V().has('code','DFW').as('a').outE().count().as('b').select('a','b')
// ^ the correct way to write this query
g.V().has('code','DFW').group().by().by(outE().count())
// expectation: the vertex is returned
// ^ actual: nothing is returned, A cannot be retrieved because the group reduced it to a map
g.V().has('code','DFW').as('a').group().by().by(outE().count()).select('a')
// ^ the correct way to retrieve A; without UNFOLD you get [v[someId]] instead of v[someId]
g.V().has('code','DFW').group().by().by(outE().count()).unfold().select(keys)



////////////////////////////////// side effects
// store and output a reference to a traversal
// ^ notice the use of STORE instead of AS
g.V(3).sideEffect(out().count().store('a')).
       out().out().count().as('b').select('a','b')
// output a traversal as a set so duplicates are removed
g.withSideEffect('s', [] as Set).
  V().hasLabel('airport').limit(20).values('runways').
      store('s').cap('s').order(local)
// ^ similar to above but uses BY with STORE instead of VALUES
g.V().has('region','US-TX').store('r').by('runways').cap('r')
// ^ similar to above but with AGGREGATE
g.V().has('airport','country','IE').aggregate('ireland').cap('ireland')
// total runways for each airport that SAF goes to
// ^ note how we initialize sack to 0
g.withSack(0).V().has('code','SAF').out().values('runways').fold()
// ^ similar to above, but this time uses SUM operator
// ^ the final sack() call returns the current sack
g.withSack(1).V().has('code','SAF').out().values('runways').sack(sum).sack().fold()
// ^ same as above, but this time uses ASSIGN to initialize the sack
// ^ this form enables initializing the sack anywhere in the query
// ^ instead of using CONSTANT, you can use anything, e.g. another traversal
g.V().sack(assign).by(constant(1)).has('code','SAF').
      out().values('runways').sack(sum).sack().fold()
// using BY to assign values to a sack
g.V().has('code','AUS').sack(assign).by('runways').
  V().has('code','SAF').out().
      sack(sum).by('runways').sack().fold()
// subtract the sack’s initial value from the other values
g.V().sack(assign).by(constant(-1)).has('code','SAF').
      out().values('runways').sack(sum).sack().fold()
// subtract the other values from the sacks initial value
g.V().has('code','AUS').sack(assign).by('runways').
  V().has('code','SAF').out().
      sack(minus).by('runways').sack().fold()
// return the minimum distance for each route
g.V().sack(assign).by(constant(400)).has('code','SAF').
      outE().sack(min).by('dist').sack().fold()
// add arbitrary parts of a query to a sack list
g.withSack([]).V().has('code','SAF').out().
               values('runways').fold().sack(addAll).
               V().has('code','AUS').out().values('runways').fold().
               sack(addAll).sack()
// use the value of a sack in a comparison
g.withSack(0).V().
  has('code','SAF').
  repeat(outE().
         sack(sum).by('dist').
         inV()).
  until(sack().is(gt(10000))).
  limit(5).
  path().
    by('code').
    by('dist')



////////////////////////////////// dunno where these came from (aws docs?)
// should all be updated to use the air routes data or deleted
// conditional update: based on another property
g.V('person1').hasLabel('Person').has('level', 1)
 .property('level2Score', 0)
 .property(Cardinality.single, 'level', 2)

// replace a property
g.V('person1').hasLabel('Person')
 .sideEffect(properties('creditScore').drop())
 .property('creditScore', 'BBB')

```

### copypasta from docs

```ts
// the age of vadas and marko
g.V().has("person", "name", within("vadas", "marko")).values("age");
```

## groovy: just enough

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


// read els at index 2 through 3
g.V().has('code','AUS').values('places').next()[2..4]

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

// add new verticies ina loop; note the call to next()
vertices = [["WYZ","KWYZ"],["XYZ","KXYZ"]]
for (a in vertices) {g.addV("airport").property("code",a[0],"iata",a[1]).next()}


// inspect the key types for a label
pkeys=g.V().hasLabel('airport').limit(1).next().keys()
pkeys.each {
  printf("%10s : %s\n" , it,
          g.V().hasLabel('airport').limit(1).
            values(it).next().class)};[]
// ^ returns something like
//  country : class java.lang.String
//     code : class java.lang.String
//  longest : class java.lang.Integer
//     city : class java.lang.String

```

## typescript: bun.sh + gremlin-javascript

- [typescript DSL](https://github.com/nirv-ai/dbs/blob/main/graph/tinkerpop/bun/src/lib/groovy/dsl.ts)
- [practical gremlin examples in typescript](https://github.com/nirv-ai/dbs/blob/main/graph/tinkerpop/bun/src/test/airRoutes/examples.ts)
- FYI
  - using typescript to submit parameterized scripts:
    - should be copypasta
  - using typescript gremlin API
    - its generally not going to be a copypasta from gremlin to typescript
      - but shouldnt require much modification
      - using the docker compose in the link above you can connect gremlin console & a bun server to the same gremlin server in two separate terminals
        - practice getting identical results
        - take note of the difference in Results format

```ts
////////////////////////////////// not supported in typescript
// FYI: the apache docs are old and the latest gremlin npm package may support these
// next(n): next() is supported
// tryNext()
// toSet()
// toBulkSet()
// fill(collection)

////////////////////////////////// bestfriends in typescript land
// append to basically any example in this file and it should work
hasNext(): Promise<boolean>;
iterate(): Promise<void>; // execute a traversal for its side effects
next(): Promise<IteratorResult<any>>;
toList(): Promise<Traverser[]>;
toString(): string;

////////////////////////////////// important gremlin.process members
Statics;
WithOptions;
P; // predicates
TextP;
Traversal;
cardinality;
column; // e.g. keys, values
Direction;
Operator;
order;
pick;
pop;
scope;
t; // enums that are available in the console, e.g. id, key, label, value, etc
GraphTraversal;
GraphTraversalSource;
statics; // i.e. __, allows the cration of anonymous traversals, eg. contains out(), count(), etc and bunches of other traversal steps

```
