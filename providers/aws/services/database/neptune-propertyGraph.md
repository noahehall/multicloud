# Neptune Property graph

- create and query data using Apache tinkerpop's gremlin traversal language

## links

- [knowledge graphs on AWS](https://aws.amazon.com/neptune/knowledge-graphs-on-aws/)

## best practicies

- design for querability: work backwards from the questions your data must answer; whereby you iterate the model and the queries on a use-case-by-use-case basis.

## basics

## data model

- the supsert of paths traversed by your queries
  - should offer the most efficient routes for your queries
- entity vs attribute, vertex vs property
  - does it have an identity or is it a value type?
  - is it a complex type with multiple fields?
  - are there any structural relationships between values
  - prefer properties > vertices/edges until the needs arise

### IDs:

- every vertex and edge must have a unique ID
- you can supply one on creation, else it will be auto assigned
- a vertex and edge can share the same ID, but not two vertex/edges

### Properties

- edges and vertices can have optional properties
- generally used for metadata, ACLs, etc
- edge properties: e.g. strength, weight or quality of te relationship
- vertex properties: e.g. entity attributes/metadata

### Labels

- edges and vertex are required to have labels
  - edge: exactly one
  - labels: 0/more (lol, not really required ;)~)
- vertex labels: tag, type and group vertices
- edge labels: the semantics of the relationship represented by the edge

### vertices

- entities: things with identities, complex value types
- intermediate vertex: required since you cant `attribute` an edge with a vertex
  - i.e. you can go from vertex A to edge B that connects vertex C & D
  - you need an intermediate vertex that defines the relationship from A to C & D
    - this generally occurs when youve used an edge property when in fact you need a new vertex

### edges

- relationships between entities, variable structure
  - mult-edges: there can be multiple routes between two vertices
  - self-edges: a vertice can route to itself
- gotchas
  - Every edge must have a name, or label, and a direction (i.e. no dangling edges)

## tests

### unit tests

- create & execute tests via sagemaker jupiter notebooks, gremlin console, cloud9 ide
- Create test fixtures that install a sample dataset in a known state, and write unit tests for your queries that assert query results based on a fixed set of inputs.

## gremlin

- check the tinkerpop file
