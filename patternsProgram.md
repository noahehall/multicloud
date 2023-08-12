# Program Design Patterns

## Links

- [towards a human-centered workshop design](https://www.slideshare.net/TobiasBlum/innovating-the-api-economy-towards-a-humancentered-workshop-design)
  - master thesis by tobias blum; creator of the human-centered api methdology
- [human-centered api design](https://medium.com/api-product-management/design-apis-human-centered-to-build-successful-api-products-ffe35015cee5)
- [api product ideation and validation](https://medium.com/api-product-management/api-product-ideation-and-validation-aef140db00b)
- [best practices for REST api design (stackoverflow.blog)](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
- [best practices in API Design (swagger)](https://swagger.io/resources/articles/best-practices-in-api-design/)
- [RESTful web API design (microsoft)](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [REST API: concepts, best practices and benefits (altexsoft)](https://www.altexsoft.com/blog/rest-api-design/)
- [HATEOAS driven REST APIs](https://restfulapi.net/hateoas/)
- [best practices for designing pragmatic RESTful apis (enchant)](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
- [REST (roy fielding)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
- [REST (wikipedia)](https://en.m.wikipedia.org/wiki/Representational_state_transfer)
- [roy fielding](https://roy.gbiv.com/)
- [selecting a rapid prototyping process](https://engineeringproductdesign.com/rapid-prototyping-process-selection-key-factors/)
- [rapid prototyping](https://engineeringproductdesign.com/knowledge-base/rapid-prototyping-techniques/)
- [CRUD](https://en.m.wikipedia.org/wiki/Create,_read,_update_and_delete)
- [api versioning](https://stackoverflow.com/questions/389169/best-practices-for-api-versioning)
- [api versioning (stripe)](https://stripe.com/docs/api/pagination/auto)
- [api as a product](https://api-as-a-product.com/articles/case-study-human-centered-api-design/)
- [rethinking service blueprints for agile delivery](https://wiprodigital.com/2018/08/30/rethinking-service-blueprints-for-agile-delivery/)
- [how to mke effective service blueprints](https://miro.com/guides/service-blueprints/)

## basics

- specific to the internals of a single application, restricted by the application architecture
  - the structural composition of the software program
  - the interactions between software elements
  - during the process of writing software code, edevelopers encounter similar problems multiple times within a roject/compony
    - design patterns give engineers a reusable way to solve recurring problems
- bad design:
  - rigid: hard to change, e.g. due to dependency changes
  - fragile: easy to break, e.g. when making a change bugs cascade
  - immobile: difficult to reuse
- OOP concepts not always relevant to good design principles
  - enheritance: reuse features & behaviors
  - encapsulation: hide & protect data
  - polymorphism: code that works by behavior, and works with types/subtypes with a similar interface
  - abstraction: hide implementation details, and depend instead on declarative interfaces

### design principles

- sets of practices that form the basis of design patterns
- favor composition over inheritance: dont overuse inheritance, composition should also be used to extend behavior
  - identify application components that vary, and separate them from what stays the same
  - HAS-A (a dog has an owner) is better than IS-A (a dog is a animal)
  - objects get new behaviors by consuming (instead of inheriting from at compile time) objects that supply that behavior at runtime
- encapsulate what varies: anything expected to change often should be encapsulated behind some sort of interface
  - its all about letting one part of a system vary independently of others, e.g. moving the brancing logic into a separate module that exports an interface
  - all patterns implement this principle in some form or another
- program to interfaces: program to the most abstract behaviors as possible, allowing implementers of the interface the opportunity to change the specific behaviors as needed
  - one class should not rely on a specific instance of another class
  - objects should rely on interfaces, and shouldnt care which class implements that interface
- loose coupling between objects that interact
  - objects should have little knowledge about implementation details of other objects
  - changes to one should not impact another, if it does, you've got some tight coupling
  - reduces the dependency between components
- SOLID: by Martin Robert Martin
  - S: single responsibility: all about limiting the impact of change
    - an object should only have one reason to change
    - each additional responsibility adds a reason for modifications to be required to the object as the nature of that responsibilty changes in the future
  - O: classes should be open for extension, but closed for modification
    - objects should provide default behavior, but enable consumers to override behaviors when needed
  - L: Liskov substitution: subtypes should be substitutable for their base types
    - i.e. subtypes should adhere to the interfaces of their base types
    - i.e. subtypes behave like their base types
  - I: interface segregation: interfaces should be small and cohesive, and shouldnt include methods they dont use
    - polluted interface: continuing to add new functionality to an interface as it evolves
      - causes unwanted dependencies between consumers of the the interfaces
    - cohesion: how related a class/interfaces methods are to themselves
      - if all consumers of a class/interface use the class/interface the same way, it generally means there is high cohesion
      - else you should subtype the class/interface into additional subtypes, that only implements the behavior with high cohesion
  - D: dependency inversion: high level modules should not depend on low level modules; both should depend on abstractions
    - instead of coupling high level component -> low level component
    - ^ you should instead program to an interface, and have both the high and low components depend on the same interface abstraction
    - ^ the abstraction should not depend on details (implementation), but the implementation (low level component) should depend on the interface
- design by contract: specify preconditions, postconditions & invariants; treat inputs and outputs the same way across implementations

## APIs

### REST

#### Best Practices

- before you do anything
  - start with a stable data model before releasing a public API
- structure & design: an API is a user interface for a developer
  - use plural NOUNS (things) and not ACTIONS (http methods) in your endpoint URLs
    - wtf is the difference between goose & geese?
  - use web standards
  - should be explorable via a browser address bar
    - this should also include common search queries
      - package up common sets of conditions into easily accessible endpoints
      - e.g. recently closed could be `/tickets/recentlyclosed` versus a long as fkn filter query param
  - have a single source of documentation
    - available without logging in
    - include copypasta examples
  - use restful URLs and actions
    - structure your API into logical resources that are manipulated using HTTP methods (CRUD)
      - dont map to your data model 1 to 1
        - this is likely not effective from an API consumer perspective
        - a security risk by revealing the structure to your data modal
        - brittle in the event your data model changes
      - hierarchy
        1. if resource Y is always a child of X, then `/v1/x/:id/y
        2. if resource Y is independent but associated: include an identifier with X where Y can be retrieved with a second API call
        3. if resource Y is independent but always requested with X: see #1 or embed the resource within the call to retrieve X
           - this avoid the second call with approach #2
      - http methods have meaning: e.g. a single `/users` endpoint can receive GET, PUT, POST, PUT, PATCH, etc without needing 5 different URIs
        - GET: retrieve thing(s)
        - POST: create thing(s)
        - PUT: update thing(s)
        - PATCH: partially update thing(s)
          - arguable if PATCH should ever be used
          - however PATCH can be used to make an ACTION on a resource appear as a FIELD ona resource
            - e.g. ACTIVATE action could be a PATCH on a resource, even tho the backend data model supports this via other logic (and not an activate field)
        - DELETE: delete thing(s)
  - return useful confirmations from POST, PATCH & PUT requests
  - only/always use
    - JSON syntax (fk xml) for all HTTP methods
      - some HTTP clients wont be supported, but fk them (unless their paying u)
    - camelCase (fk snake)
    - compression (e.g. gzip) unless in test mode
    - pretty print unless in unless in prod prod
  - effectively use HTTP status codes
  - define a consumable error payload
- versioning
  - have a predictable and publicly available versioning scheme and update schedule
    - CHANGE is coming, everyone knows it, versioning helps manage it
  - version via the URL
    - prevents invalid requests from hitting updated endpoints
    - smooth transition to newer versions while sunsetting legacy endpoints
    - ensures browser explorability across versions
    - provides structural stability
  - version via HEADER fields
    - useful for specifying minor/patch versions of a major versions
      - e.g. field deprecedations, endpoint changes, etc
- features
  - HATEOAS
  - many features require extended set of options
    - filtering
      - use a unique query param for each field e.g. `poop?field1=yes&field2=no`
    - sorting
      - use a generic queyr param for sorting rules e.g. poop?sort=-field1&field2
        - unary operators -/+ indicate DESC & ASC
    - searching:
      - /users?search=field1...:
        - some resources require SEARCH as a mechanism to filtering and retrieving matches
      - /search?...:
        - a distinct endpoint for searching all resources
    - pagination
      - via link headers
      - via query params
  - ability to toggle/specify specific features
    - toggle
      - response envelopes: required by JSONP and other limited http clients
    - specify
      - version: major version in the URL, minor/patch versions via HEADER fields (see stripe/enchant)
      - returned fields: do you need every user field every time?
  - autoloading
  - http-method-override
    - never on GET requests
  - caching
    - via response headers
- security
  - use SSL everywhere
    - encrypt communication between parties
    - inhibit eavesdropping/impersonation if authentication credentials are hijacked
    - enables use of access tokens instead of having to sign each API request
  - token based authentication
  - oauth2 in case delegation is required
  - use HARD errors
    - e.g. a client requests a non secure version of an API endpoint
      - an automatic redirect to the SSL version could leak request params over the unencrypted endpoint

## patterns

- specific strategies for resolving common architecture problems
- adapter
- aggregator
- builder
- chain of responsibility
- decorator
- domain driven development
- facade
- factory: abstracts implementation details of instantiating entities
  - simple factory: encapsulating a complex if statement/similar into a separate module
- interface oriented design
- observer
- repository: abstract implementation details on how to retrieve data; logic for retrieving, validating and translating responses from a data service
- singleton
- strategy
- tryparse: pattern for trying some logic and returning the response, but on error returning the given substitute instead of throwing the exception

## event sourcing

- storing events instead of state, enabling you to rehydrate/replay timelines

## command query responsibility segregation (CQRS)

- specific to data problems
- a distinct service for reading from data sources
- a distinct service for writing to data sources

## CQRS and event sourcing combined

- useful for operational & yielding problems
