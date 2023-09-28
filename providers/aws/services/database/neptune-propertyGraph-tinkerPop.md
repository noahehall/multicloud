# Neptune Property graph

- create and query data using Apache tinkerpop's gremlin traversal language
- check the apache/tinkerpop file for ramping up on gremlin

## links

- [knowledge graphs on AWS](https://aws.amazon.com/neptune/knowledge-graphs-on-aws/)

### gremlin

- [AAA: accessing neptune](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin.html)
- [AAA: getting started](https://docs.aws.amazon.com/neptune/latest/userguide/get-started-graph-gremlin.html)
- [neptune compliance](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin-differences.html)
- [sessions](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin-sessions.html)
- [with neptune](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin.html)
- [transactions](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin-transactions.html)
- [statements](https://docs.aws.amazon.com/neptune/latest/userguide/gremlin-explain-background-statements.html)
- [api status endpoint](https://docs.aws.amazon.com/neptune/latest/userguide/gremlin-api-status.html)

## best practicies

- design for querability: work backwards from the questions your data must answer; whereby you iterate the model and the queries on a use-case-by-use-case basis.
- Try to limit each vertex to having just one label, but exceptions can exist if entities play multiple roles
  - avoid using labels as flags or enumerated tags that group entities of a particular type
    - use a property to perform this partitioning

### Anti patterns

- check the neptune compliance docs if running into any issues with gremlin copypasta

## basics

- specifications by example: illustrating a small, representative example of the graph, with specific vertices, labels, properties and edges showing how instances of things in the application domain are attributed and connected to one another

## data model

- the supsert of paths traversed by your queries
  - should offer the most efficient routes for your queries
- hard questions
  - What types of vertices do you have in your graph, as represented by vertex labels?
  - What properties are attached to each type of vertex?
  - How are different vertices connected?
  - What edge labels do you use to represent different types of edges?
  - What properties are attached to these edges?

### Constraints

- AWS
  - edge & vertex id uniqueness
  - Every edge must have a name, or label, and a direction (i.e. no dangling edges)
- abcd

### IDs

- every vertex and edge must have a unique ID
- you can supply one on creation, else it will be auto assigned
  - user-defined IDS are required when bulk uploading
- a vertex and edge can share the same ID, but not two vertex/edges
- user supplied ids
  - use the property step with the id keyword:` g.addV().property(id, 'customid')`

### Properties

- edges and vertices can have optional properties
- generally used for metadata, ACLs, etc
- edge properties: e.g. strength, weight or quality of te relationship
  - further filter which edges a traversal follows –
    - e.g. following only KNOWS edges in a social graph whose strength property is greater than 5
    - e.g. store metadata such as a version number, last updated timestamp, or access control list.
- vertex properties: e.g. entity attributes, application metadata, etc
- updating properties: by default it uses set cardinality, which always appends when upserting
  - you must specify `single` to replace all existing values
  - e.g. `g.V('exampleid01').property(single, 'age', 25)`

### Labels

- edges and vertex are required to have labels
  - edge: exactly one
  - vertex: 0/more (lol, not really required ;)~)
- vertex labels: tag, type and group vertices
- edge labels: the semantics of the relationship represented by the edge
- When you create a label, you can specify multiple labels by separating them with ::.
  - `g.addV("Label1::Label2::Label3")` adds a vertex with three different labels.
  - `::` is reserved only for adding labels function

### vertices

- entities: things with identities, complex value types
- intermediate vertex: required since you cant `attribute` an edge with a vertex
  - i.e. you can go from vertex A to edge B that connects vertex C & D
  - you need an intermediate vertex that defines the relationship from A to C & D
    - this generally occurs when youve used an edge property when in fact you need a new vertex
- vertex property vs label
  - label to type a vertex or to describe the role a vertex plays in your dataset.
  - properties to capture the instance-based attributes of a thing in your domain
- vertex attribute vs vertex: should you convert an attribute to its own vertex?
  - prefer properties > vertices until the need arises, e.g. whenever the attribute has/is/will be
    - an identity or is a complex value type?
    - part of a value structure, such as a hierarchy?
    - used to relate entities at query time?
- complex value types
  - values with more than one field, e.g. an Address
  - subgraph structures: e.g. a classification hierarchy: A and B are both types of C
  - connected data queries: used to create paths through the network that relate entities at query time
    - i.e. starting at entity X find other entities based on some unknown attribute Y
      - attribute Y should be a vertex and not an attribute if its significant concept

### edges

- relationships between entities, variable structure
  - mult-edges: there can be multiple routes between two vertices
  - self-edges: a vertice can route to itself

#### Performance

- performance of a graph query depends on how much of the graph the query has to 'touch' in order to generate a set of results
- ensure your queries touch the minimum amount of data by naming edges in a way that allows the query engine to follow only those relationships relevant to the query being executed.
- Derive your edge labels from your use cases. Doing so helps structure and partition your data so that queries ignore vertices and edges that have no bearing on the working set necessary to satisfy the query.
  - fine grained labels: queries need only find relationships with a particular name drawn from a family of names
    - e.g. of all addresses I need the work address -> create an adge for each address type
  - general labels: queries need to find all relationships belonging to a particular family
    - all addresses, regardless of type -> edge label + property for type
    - when you do need just a particular type of address, it will require more runtime work but satisfies both general and fine grained
- design your model so that your performance-critical queries follow mostly outgoing edges.
  - If a query has to traverse an incoming edge, always specify the edge label as part of the query, even if there is only one type of edge label that the traversal could possibly follow.
    - e.g. using in('CREATED') and inE('CREATED') rather than in() and inE() to traverse those edges.

## Patterns

- design an application graph data model that is expressive of your domain,
- easy to query on behalf of your most important use cases,
- easy to evolve as you discover new use cases and introduce new features into your application

### Hub and Spoke

- a central vertex (HUB) connected to several neighbouring vertices (SPOKES).
  - hub: often represents a fact or event
  - spokes: contextual information that helps explain or enrich our understanding of the hub
  - e.g. Purchase hub vertex, representing a purchasing event, connected to the User who made the purchase, the several Product items in the user's shopping basket, and the Shop where the items were bought.
- similar to the star schema or facts and dimensions model employed in data warehousing
- use cases
  - needing more expressive structuring of entities and relationships.
  - n-ary relationships: multi-dimensional relationships that bind together several things and concepts.
  - if you ever find yourself:
    - wanting to add an edge to an edge – to annotate one relationship with another
    - transforming a noun into a verb:
      - e.g. instead of saying X SENT an Email TO Y, we might verb the noun, and say X EMAILED Y.
- issues
  - you'll end up increasing storage overheads and query latencies (more data to store, fetch and traverse)

## tests

### unit tests

- create & execute tests via sagemaker jupiter notebooks, gremlin console, cloud9 ide
- Create test fixtures that install a sample dataset in a known state, and write unit tests for your queries that assert query results based on a fixed set of inputs.

## gremlin

- check the tinkerpop file for gremlin specific shiz

### API compliance

- neptune doesnt support;
  - Groovy commands that don't start with g
  - Lambda Steps.
  - metaproperties
  - list cardinality for vertex properties (only single & set)
  - Gremlin variables or parameterization in scripts
    - because neptune uses the ANTLR grammar in its query processing engine rather than the older GremlinGroovyScriptEngine
  - fully qualified class names for enumeration values
    - e.g. use `single` and not `org.apache.tinkerpop.gremlin.structure.VertexProperty.Cardinality.single`
      - fk java!
  - gremlin `io-step` is only partially supported
  - does not expose the graph object.
    - i.e. a bunch of graph features arent supported, check the docs!
- optimizations
  - neptune is optimized for
    - traversing outgoing edges
    - datasets containing a relatively small number of unique predicates – in the order of several thousand at most.
      - e.g. A dataset containing 100,000 User vertices, each with 5 properties, and 1 million FOLLOWS edges has 6 unique predicates (5 vertex properties and 1 edge label).
- Variables and parameters in scripts
  - neptune:
    - pre-bound variables: the traversal object g is Pre-bound in Neptune, and the graph object is not supported.
- java code! fk java!
  - Neptune does not support calls to methods defined by arbitrary Java or Java library calls other than supported Gremlin APIs.
  - e.g. java.lang.\*, Date(), and g.V().tryNext().orElseGet() are not allowed.
- Script execution
  - neptune
    - All queries must begin with g, the traversal object.
    - multiple traversals can be issued separated by a semicolon (;) or a newline character (\n)
      - To be executed, every statement other than the last must end with an .iterate() step.
      - Only the final traversal data is returned.
      - this does not apply to GLV ByteCode query submissions.
- sessions
  - in Neptune are limited to only 10 minutes in duration.
- transactions
  - Neptune opens a new transaction at the beginning of each Gremlin traversal
    - closes the transaction upon the successful completion of the traversal.
    - rolled back when there is an error.
  - Multiple statements separated by a semicolon (;) or a newline character (\n) are included in a single transaction.
    - Every statement other than the last must end with a next() step to be executed.
    - Only the final traversal data is returned.
- when you send the Gremlin query as a text string, the following are not supported
  - Manual transaction logic using tx.commit() and tx.rollback()
  - a bunch of gremlin methods, lol check the docs!
