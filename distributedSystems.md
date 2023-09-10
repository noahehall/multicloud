# Distributed systems

- by Roberto Vitillo
  - reading: done
  - copying: 145 > http caching
- a group of nodes that cooperate by exchanging messages over communication links to achieve some task
- the emergence of an application requiring high availability and resilience against single node failures

## links

- [law of leaky abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)
- [cubic TCP algorithm](https://en.wikipedia.org/wiki/CUBIC_TCP)
- [grpc](htTitleps://grpc.io/)
- [building distirbuted locks with dynamodb](https://aws.amazon.com/blogs/database/building-distributed-locks-with-the-dynamodb-lock-client/)
- [paxos whitepaper by microsoft PDF](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/paxos-simple-Copy.pdf)
- [raft consensus algorithm](https://raft.github.io/)
- [finite state machines](https://en.wikipedia.org/wiki/Finite-state_machine)
- [consensus algorithms](<https://en.wikipedia.org/wiki/Consensus_(computer_science)>)
- [CAP thereom white paper PDF](https://groups.csail.mit.edu/tds/papers/Gilbert/Brewer2.pdf)

## basics

- throughput: number of requests processed per second
- response time: the time elapsed in seconds between sending a request and receiving a response
- capacity: the maximum load a service can handle
- availability: the percentage of time a service is available for use, generally expressed in Nines
  - one nine: 90%; 2.4 hours
  - 2 nines: 99%; 14.40 minutes
  - 3 nines: 99.9%; 1.44 minutes
  - 4 nines; 8.64 seconds
  - 5 nines: 864 milliseconds
- fault-tolerant data stores
- linearizable: aka stong consistency: the strongest consistency guarantee a system can provide for single object requests
- compare-and-swap operation: atomically updates the value of a key if and only if the process attempting to update the value correctly identifies the current value
- write-once register: WOR; a thread-safe and linearizable register that can onlyh be written once but can be read many times

### put somewhere else

- read-modify-write: aka optimistic concurrency control; application-level design pattern;
  - updating an item but ONLY if it hasnt changed since you last read it
    - requires that all processes knows to increment the version number for specific items
  - read: an application retrieves the item and store its version number in memory
  - modify: prepare the update and increment the version number (this will be the new version you'll check against in the future write)
  - write: execute the update with a conditional expression that fails if the version number has changed since you last read it
    - indicates some other process has changed the data your interested in
- scatter gather technique: funny name for a regular-ole thread-safe data chunking algorithm
  - the base thread determines the number of chunks primary key for each chunk
  - threads are created to read/write chunks to/from dynamodb

## hallmarks of distributed systems

- communication
  - how are requests & responses represented on the wire
  - what happens in the event of temproary network outages
  - how to guard `against snooping
- coordination
  - especially in the presence of failures
    - and failures are unavoidable
  - is always expensive to implement
- scalability
  - in terms of efficiently handling load, i.e. the consumption of system resources
  - an increase in load sholuld not degrade a service's performance
- resiliency
  - system able to work in the face of failures
- maintainability
  - easy to modify, extend and operate

## communication

- IPC: inter-process communication
- internet protocol suite
- TCP: reliability and stability at the price of lower bandwidth and higher latencies
  - connection lifecycle: opening > established > closing
  - flow control: backoff mechanisms implemented to private the sender from overwhelming the receiver; similar to rate-limiting
  - congestion window: the total number of outstanding segments that can be sent without an acknowledgement from the other side
    - a way to guard against flooding the undelrying network
- UDP: connectionless transport layer protocol for higher bandwith utilization by not providing any reliability, flow control, or congestion control
- securing messages with TLS
  - servers authenticated with digital certificates provided by some root certificate authority
  - maintaining message integrity by calculating message digests
- service discovery
  - DNS: the phonebook of the internet lol ;)
    - setting the right TTL for optimial DNS caching
      - too long: clients wont see DNS changes
      - too short: increase the load on name servers as cache entries expire requiring clients to fully resolve hostnames
- APIs
  - direct: all processes are up and running for the communication to succeed
    - request-response pattern
  - indirect: e.g. messaging patterns where processes communicate via a broker and not directly to each other
  - synchronous: sender waits for a response by blocking the current thread
  - asynchronous: senders doesnt wait, or provides a callback
  - effective API evolution strategies
    - endpoint level: changeing /x to /x2
    - message level: changing the schema of expected request/response messages

### models

- fair-loss: assumes that messages may be lost and duplicated; but if the sender keeps resending a message, eventually it will be delivered to the destination
- reliable link: assumes that a message is deliverd exactly once, without loss or duplication
  - a reliable link can be implmeneted on top of a fair loss one by de-duplicating messages at the receiving end
- authenticated reliable link: makes the same assumptions as reliable link but additionally assumes that the receiver can authenticate the sender

### message serialization

- JSON: human readable; self-describing
- protobuff: binary

### IPC inter-process communication

- HTTP: generally used for public APIs
  - REST
  - openApi (swagger)
  - using the correct Http methods
    - safe methods: cachable requests that dont have any visible side effects
      - GET
    - idempotent methods: can be executed multiple times and the end result will be the same as if it was executed just a single time
      - PUT/DELETE/GET
    - POST is neither safe nor idempotent
      - but can be made idempotent by adding a some ID to each request
      - the server can store this UID and validate if its a duplicate or not
      - if the IDs are stored in the same DB as api resources then you can guarantee atomicity with ACID transactions
  - responding with the correct http status codes
- gRPC: open source universal RPC framework
  - generally used for internal APIs

### broadcast protocols

- multicast: enables delivery of messages to groups of processes
  - unlike TCP which is point-to-point, or unicast
- guarantees the message is delivered to all non fault processes in a group
  - best-effort broadcast: only if the sender doesnt crash
  - reliable broadcast: even if the sender crashes before the message has been fully delivered
  - eager reliable broadcast: reliable broadcast + each process retransmits the message to the rest of the gorup the first time its delivered
  - gossip broadtcast protcol: eager reliable broadcast + only a subset of processes retransmit the message
  - total order broadcast: reliable broadcast + guarantees that messages are delivered in the same order to all processes

## Coordination

- a group of processes that gives its users the illusion they are interacting with one coherent node

### failure detection

- when a client sends a request to a server - how does it know if its successful?
  - how long to wait for a response?
  - how long to wait before triggering a timeout?
- ping: a periodic request that a process sends to another to check whether its available
  - the sender expects a response within a specific timeframe
- heartbeat: a message that a process periodically sends to another
  - if the destination doesnt receive a heartbeat within a specific time frame, ikt triggers a timeout and considers the process unavailable

### behavior models

- arbitrary fault: assumes that a process can deviate from its algorithm in arbitrary ways, leading to crashes or unexpewcted behaviors caued by bugs/malicious activity
  - aka byzantine model
  - used to model safety-critical systems like airplange engines, nuclear power plants, cryptocurrencies
- crash recovery: assumes that a process doesnt deviate fromm its algorithm but can crash and restart at any time, losing its in memory state
- crash stop: assumes that a process doesnt deviate from its algorithm but doesnt come back online if it crashes
  - generally used to model unrecoverable hardware faults

### timing models

- synchronous: assumes that sending a message or executing an operation never takes more than a certain amount of time
- asynchronous: assumes that sending a mesage or executing an operation on a process can take an unbounded amount of time
- partial asynchronous: assumes that the system behaviors synchronously most of the time
  - generally representative enough of real world systems

### Understanding Time

- there is no shared global clock that all processes agree on that can be used to order operations
- it all boils down to knowing if one operation happened before another across nodes

#### physical clocks

- every process has access toa physical wall-time clock
- generally based on a vibrating quartz crystal; measurements are affected by manufacturing differences and external temperature
  - quartz clocks periodically synchronize their time with atomic clocks
- clock drift: the rate at which a clock runs faster/slower
- clock skew: the difference between two clocks at a specific point in time
- atomic clocks measure time based on quantum mechanical properties of atoms
- monotonic clocks: measures the number of secondes elapsed since an arbitary point in time and can only move forward
  - useful for measuring elapsed time between two timestamps on the same node

#### logical clocks

- measures the passing of time in terms of logical opeartions, not wall-clock time
- counter: incremented before an opeartion is executed
- logical timestamp: if two operations execute on the same process, then one must come before the other
- synchronization point: when one process sends a message to another
  - the operations executed by the sender before the message was sent
    - MUST HAVE HAPPENED BEFORE
  - the operations that the recediver executed after receiving it
- lamport clock: a logical clock based on the idea of synchronization points
  - each process in the system must have a local counter that follows specific rules
    - that i wont capture here, google it
  - assumes a crash-stop model

#### vector clocks

- a logical clock that guarantees that if a logical timestamp is less than another, then the former must have happened-before the latter
- implemneted with an array/dictionary of counters, one for each process in the system, and each process has a local copy of all counters

### Leader Election

- a specific process with special powers relative to others
  - accessing shared resources
  - assigning work to others
- algorithm requirements
  - safety: only one leader at any given time
  - liveness: an election eventually completes even in the presence of failures
- best practices
  - minimize the amount of work the leader performs
  - account for failed elections or instances when more than one leader exists
  - the data store that tracks leases must be fault-tolerant
- positives
  - having a leader can simplify the design of a system as it eleminates concurrency
- negatives
  - scalability bottleneck if the number of operations performed by the leader increases tot he point where it can longer keep up
  - the leader is a single point of failure with a large blast radius
    - if the election process/leader node fails it can bring down the entire system

### consensus

- general consensus: a group of processes has to decide a value so that
  - every non-faulty process eventually agrees on a value
  - the final decision of every non-faulty process is the same everywhere
  - the value that has been agreed on has been proposed by a process
- uniform consensus: when all processes have to a agree on a value, even fault ones
  - e.g. when atomically commiting a distributed transaction

## Replication

- increases availability, scalability and performance by redundantly storing data and operating services across multiple nodes

### algorithm comparisons

- chain vs leader-based replication
  - chain: simpler and more performant:
    - reads: higher througputs and lower response times
      - can be served immediately without contacting othe replicas
      - can be distributed across replicas while still guaranteeing linearizability
    - writes: higher potential latency but cant be pipelined to improve throughput
      - updates need to go through all processes in the chain before it can be considered committed
      - a single slow replica can slow down all writes or take down the entire chain
  - leader-based
    - writes: more resilient to transient degredations
      - the leader only has to wait for a quorom before a write is considered commited
      - if any particular process fails, it doesnt matter as long as a majority are still up

### State Machine (leader-based) Replication

- the main idea is that a single process (the leader) broadcasts operations that changes it's state to other processes (the followers)
- if each follower executes the same sequence of operations as the leader, then each follower will end up in the same state as the leader
- a large part of state machine replication is didicated to fault-tolerance
  - any process can fail at any time
  - broadcast messages can be lost due to network failures
- all operations must be deterministic so that all followers end up in the same state
  - a deterministic way to solve the problem of consensus
- main components
  - broadcast protocol: guarantees every replica receives the same updates in the same order even in the presence of faults
    - i.e. fault-tolerant total order broadcast (equivelent to consensus)
  - update function: a deterministic function that handles updates on each replica

### Chain Replication

- deals with the coordination tax paid in leader-based replication by moving coordination operations off of the systems data plane into a separate control plane
  - control plane: can tolerate up to C/2 failures, where C = number of replicas in the control plane
  - data plane: can tolerate up to N-1 failures, where N = number of processes

#### Data Plane

- the systems critical path responsible for responding to client requests
  - doesnt require a state machine (i.e. a leader) because its not responsible for fault tolerance
  - its sole focus is throughput and effiency
- processes are arranged in a chain
  - head: the leftest/first process
    - handles all writes, updates its local state and forwards the update to the next process until it reaches the tail
    - the tail updates its local state and responds back up the chain
    - whe the head receives the acknowledgment it responds to the client the write succeeded
  - tail: the rightest/last process
    - handles all reads
    - responsible for acknowledging writes received by the head and replicated down the chain
- consistency: strong
  - all writes and reads ar eproceeded one at a time by the tail
- failure modes
  - the head can fail
    - the control plan removes it and promotes a successer process to be the new head and notifies clients
  - the tail can fail
    - the control plan removes it and makes a predecessor the new tail
  - an intermediate process can fail
    - the control plan removes it and updates the link to between the failed nodes predecessor and the fail nodes successor
      - i.e. if A - B - C and B fails, the new link becomes A - C

#### control plane

- the configuration manager responsible for
  - the data planes health
  - fault tolerance in the system (replacing faulty processes)
  - ensures theres a single view of the chain's topology and that every process agrees with it
- the control plane itself needs to be fault tolerant
  - requires some state machine replication like Raft

### consistency

- the type of guarantee a distributed system provides to readers of distributed data that a read request matches the most recent write request
- modeled on a spectrum
  - strong consistency (linearizability)
  - sequential consistency
  - eventual consistency
- the issue being
  - a write request takes time to process and replicate to all other nodes
  - a read request can be received by the node after a request request is sent, but before its committed and replicated to all other nodes
  - while a write request will only go to the leader in a distributed system, a read request can go to:
    - the leader
    - any follower
    - a combination of leader and follower nodes
  - since a read request can be handled by any node
    - observers of the system can have different views of the systems state causing inconsistent system behavior
- when picking a consistency type you must decide between
  - how consistent the observers views of the system are
  - the systems performance and availability

#### Strong Consistency

- the only model providing real-time guarantees
  - even tho it doesnt happen in real time
- writes and reads appear to take place atomically at any specific point in time as if there was a single copy of data
  - e.g. if clients sends writes & reads exclusively to the leader node
- the side-effects of an operation are visible to all observers once its completes
- before a request can be resolved, the service must confirm that its copy of the data is up to date with a quorom of nodes
  - this confirmation step considerable increases the time required to serve a request

#### Causal Consistency

- might be another term for Sequential consistency
- is stronger than eventual consistency but weaker than strong consistency
  - strong consistency imposes a GLOBAL order that all processes must agree with
- solves the happened-before order problem of eventual consistency
- benefits
  - for many applications: causal consistnecy is `consistent enough` and easier to work with than eventual consistency
  - is provably the strongest consistency model that enables building systems that are also available and partition tolerant
    - CAP boomshakalaka
- algorithm imposes a partial order on operations
  - requires that processes agree on the order of causally related operations
  - processes can disagree on on the order of unreleated operations

#### Sequential Consistency

- ensures operations occur in the same order for all observers but doesnt provide any real-time gurantees about when an operations side effects becomes visible
- the lack of real-time guarantees is what differentiates this from strong consistency
- all reads must go to followers and not leaders, and clients are pinned to specific followers
  - considerably increases read throughput compared to strong consistency
  - by pinning clients to specific followers, reduces availability in the event a follower goes down, then their clients lose access to the data store
- examples
  - pub/sub systems synchronized with a queue
    - a producer writes items to the queue, which a consumer reads
    - the producer and consumer see the items in the same order, but the consumer lags behind the producer

#### Eventual Consistency

- allows clients to read from any follower to increase availability but sacrifices consistency
  - the happened before problem
    - a client can send two reads that are resolved by different followers each having different views of the state
- the only guarantee this provides is that all followers will eventual converge on the same state
- requirments
  - eventual delivery: guarantee that every update applied at a replica is eventually applied to all replicas
  - convergence: guarantee that replicas that have applied the same updates eventually reach the same state
    - particularly important as replicas may receive writes in totally different orders

#### Strong Eventual Consistency

- stronger guarantees relative to plain eventual consistency
  - eventual deivery: the same guarantee as in eventual consistency
  - strong convergence: guarantee that replicas that have executed the same updates have the same state
- uses various kinds of algorithms that provide a detreminstic outcome for any potential write conflict without requiring consensus between nodes
- enables building systems that are highly availble, strongly eventual consistent, and also partial tolerant
  - i.e. almost all 3 of the CAP theoreom

### CAP Theorem

- a system can guarantee TWO of THREE
  - C: strong Consistency
  - A: Availability
  - P: network Partition tolerenace
- the idea that in the event of a network partition, whereby parts of a system become disconnected from each other, two choices are available
  - remain available by allowing clients to query followers that are reachable but not consistent with each other
  - guarantee strong consistency by failing reads that cant reach consistent nodes
- FYI on the Availability in CAP
  - requires that every request eventually receives a response
  - this is impossible in the real world
  - in fact, extgremely slow responses are entirely worthless
- FYI on the P in CAP
  - network partitions can happen, but are rare in data centers
  - its better to think about this in terms of Latency/performance relative to strong consistency
    - the more strongly consistent a system is, the more latency will be inherint

#### PACELC Theorem

- an extension to CAP
- in general
  - in case of networking partioning (P)
    - the system must choose between availability (A) and consistency (C)
  - else (E) even in the absense of partitions
    - the system must choose between latency (L) and consistency (C)

#### CAP vs PACELC

- each represent consistency, availability and partitions as binary choices, but really its a spectrum
- its really a choice between the amount of consistency and performance required by a system, relative to the required availability

### CALM Theorem

- states that a program has a consistent, coordination-free distribution implemnetation if an only if it is monotonic

#### monotonic:

- when new inputs further refine the output and cant tae back any prior output
- e.g. a program that computes the union of a set
  - once an element (input) is added to the set (output) it cant be rmeoved
  - CRDTs are monotonic
- monotonic programs TRUMP the CAP thereom by being all three a the same time
  - consistent: lol not the same C in CAP (lineariability)
    - consistency in CALM focuses on the programs output (application level)
      - a consistent program is one that produces the same output:
        - no matter in which order the imputs are processed
        - despite any conflicts
    - consistency in CAP focuses on reads and writes (storage level)
  - available
  - partition tolerant

#### non-monotonic:

- when a new input can retract a prior output
- e.g. variable assignment is a non-monotonic operation since it overwrites the variables prior value

### Data types

- Conflict-free replicated data type: CRDT; a data type with specific requirements for when
  - a client sends an update or query operation to any replica
    - it keeps a localy copy of the request
    - it broadcasts the request to others
    - it merges broadcast requests it receives
  - how each replica converges to the same state
    - its local state is a semilattice (partially ordered)
    - its merge operation produces a state that is idempotent, commutative and associative
  - guarantees a lower form of consistency but doesnt have to pay the cost of expensive coordination
- examples
  - registers
  - counters
  - sets
  - dictionaries
  - graphs

#### Registers

- last writer wins: LWW; associates a timestamp with every update to make them totally ordewrable
  - issues:
    - conflicting updates resolve to the update with the greatest timestamp which may not logically make sense from a business perspective
- multi value: MV; tracks all concurrent updates and returns conflicts to the client application for resolution
  - issues:
    - the client application must contain logic to resolve any conflicts

## Transactions

- provide the illusion that either all the operations within a group complete successfully or none of them do
  - i.e. as if the group of operations were atomic
- transactions are straight forward within a single DB, but complicated with distributed DBs

### ACID

#### Atomic

- guarantees that partial failures arent possible
- either all operations in the transactions succeed or none do
  - partial failures require guaranteeing that the successes are rolled back

##### Write Ahead Logs (WAL)

- a log thats persisted to disk before changing state in the actual data store
- each log entry records the:
  - identifier of the transaction making the change
  - identifier of the object being modified
  - both the old and new values of the object
- if a transaction is aborted/system crashes, the WAL contains enough info to redo changes
  - ensures atomicity and durability within a single data store
    - the WAL isnt shared across processes

##### Two Phase commit (2PC)

- synchronous protocol used to implement atomic transaction commits across multiple processes
  - often combined with a blocking concurrency control protocol like 2PL to provide isolation
    - i.e. participants are holding locks while waiting for the coordinator, blocking other transactions acessing the same objects from making progress
- it requires
  - cooridnator process: orchestrates the actions of other processes
    - when the coordinator wants to commit a transaction it sends a `prepare` request to the participants
    - if all processes are prepared, it then sends a `commit` request ordering them to do so
    - if any process is unprepared/doesnt reply, it sends a abort request
  - participants: the other processes
- assumptions
  - the coordinator and the participants are always available
  - the duration of any transaction is short-lived
- issues with 2PC
  - requires multiple round trips for a transaction to complete
  - if the coordinator/any participant fails, transactions are blocked

#### Consistency

- guarantees that application level invariants must always be trust
- i.e. transactions can only transition a DB from one correct state to another correct state
- this shiz is funny because this isnt a DB-level requirement, but a developer-level requirement
  - Joe Hellerstein said he through the C in to make the acronym work!
  - has nothing to do with the C (consistency) in other acronyms in this file

#### Isolation

- guarantees that a transaction appears to run in isolation as if no othe transactions are executing
- i.e. like a synchronous operation that isnt susceptible to race conditions
- easiest way to guarantee isolation is to execute transactions serially one after the other using a global lock

##### race conditions: issues with concurrent transactions

- dirty write: when a transaction overwrites the value written by another transaction that hasnt comitted
- dirty read: when a transaction observes a write form a transaction that hasnt completed yet
- fuzzy read: when a transaction reads an objects value twice but sees a different value in each read
  - because another transaction updated the value between the two reads
- phantom read: when a transaction reads a group of objects matching a specific condition while another transaction concurrently adds/updates/deletes objects matching the same condition

##### race conditions: solutions via isolation levels

- protects against one/more types of race conditions
  - there are more isolation levels that the ones listed below
- serializable: forbids phantom reads and everything below it
  - the only isolation level that protects against al possible race conditions
  - strict serializability: adds a real-time requirement on the order of transactions
    - combines serializable guarnatees with lineraizability guarantees
    - when a transaction completes, its side effects become immediately visible to all future transaction
- repeatable read: forbids fuzzy read
- read comitted: forbids dirty reads
  - e.g. postgres' default isolation level
- read uncommitted: forbid dirty writes

##### Concurrency Control Protocols

- Serializability: i.e. the best isolation level (see above somewhere)
- the challenge becomes maximizing concurrency while still preserving the appearance of serial execution
- pessimistic concurrency control protocol
  - uses locks to block other transactions from accessing an object
  - 2PL: two-phase locking: has two types of LOGICAL locks
    - READ locks: can be shared by multiple transactions that acess the object in read only mode
    - WRITE locks: can be held by a single transactions only
    - issues with 2PL
      - deadlocks: when two transactions are waiting for a lock the other transaction holds
      - read only transactions might wait for a long time to acquire a shared lock
- optimistic concurrency control protocol
  - executes a transaction without blocking based on the assumption that conflicts are rare and transactions are short lived
  - OCC: Optimistic Concurrency Control: lol a protocol that uses the type of protocol as the name for the protocol
    - a transaction writes to a local workspace without modifying the actual data store
    - when the transaction wants to commit
      - the data store compares the transctions workspace to see whether any conflicts exist with its state
      - if no conflicts exist (some serializability algorithm) the local workspace is committed to the datastores workspace
      - if conflicts do exist: the transaction is thrown away
      - issues
        - when conflicts exist transactions are wasted
        - ready only transactions may be aborted because the value that was rad has been overwritten
    - uses physical locks: i.e. locks that exist at the storage level and not the DB-program level
      - this might not be the best summarization of the book
- optimistic vs pessimistic protocols
  - optimistic: better for read-heavy workloads
    - avoid the overhead pessimistic protocols: i.e. no need to acquire or manage logical locks
  - pessimistic: more efficient for conflict-heavy workloads since they avoid wasting work
- multi version concurrency control: MVCC; maintains an older version of the data store to overcome the issues with 2PL and OCC
  - optimizes for read-only transactions by ensuring it always reads an immutable and consistent snapshot of the data store without blocking or aborting due to a conflict with a write transaction
    - can never block because of awaiting a lock
    - conflict because the previous value was overwritten by a concurrent write
  - for write tranactions it falls back to either 2PL or OCC

#### Durability

- guarantees that once the DB commits the transaction, the changes are persisted on durable storage s othat db doesnt lose the cnages if it subsequently crashes

### Asynchronous Transactions

- whenever you need to update apply transactions across multiple data stores
- outbox pattern:
  - the first data store sends its sucessfully committed local transactions to some temporary store
  - the temporary store is monitored by some relay service and replays those transactions in the other data stores
  - conceptually similar to state machine replication
- Saga pattern:
  - each saga is a distributed transaction composed of a set of local transactions
  - each local transaction is paired with an `undo` transaction
  - if all transactions in a saga succeed, then continue, else call the undo transactions to rollback
  - its generally a good idea for a saga to be implemented with
    - an orchestrator: manages the execution of the local transactions across the processes involved
    - semantic locks: any data the saga modifies is marked with a dirty flag, which is only cleared at the end of the transaction

## Scalability

- the ability for a system to perform its purpose without performance degradations as load increases
- vertical scaling: scaling up: making a system use its resources more efficiently, or providing more resources to be consumed
- key strategies for scalability
  - functional decomposition
  - partitioning
  - replication

### functional decomposition

- breaking a system into distinct components with well defined boundaries and responsibilities
- then each component can be scaled individually

### partitioning

- splitting data into partitions and distributing them among nodes
- then no single node takes the full force of load

### replication (horizontal scaling)

- replicating functionality or data across nodes, i.e. horizontal scaling
- the only long-term solution to increase a systems capacity

## Implementations

- of various things discussed in this file

### Paxos

- state machine replication protocol

### Raft

- leader-based replication

#### leader election

- implemented as a state machine in which any process is in one of three states
  - follower: the process recognizes another one as the leader
  - candidate: the process starts a new election proposing itself as a leader
  - leader: the process is the leader
- time: is divided into election terms of arbitrary length that are numbered in consecutive integers (i.e. logical timestamps)
  - a term begins with a new election, during which one/more candidates attempt to become the leader
- leader election process
  - when the system starts up, all processes are in the follower state
  - if a follower does not receive a heartbeat from the leader the process presumes the leader is dead
  - the follower starts a new election by incrementing the current term and transitioning to the candidate state
  - it votes for itself and sends a request to all processes in the system to vote for it, stamping the request with the current term election
  - the process remains in the candidate state until
    - the candidate wins the election
    - another process wins the eletion
    - a period of time goes by with no winner
      - i.e. a split vote: the candidate state will eventually time out and start a new election and repeat until a process wins
  - the idea is that each competing process tries to acquire a lease by creating a new key with compare-and-swap
    - the first process to succeed becomes the leader and remains such until it stops renewing the lease
- replication process
  - when the system starts up, a leader is elected using Rafts leader election algorithm
  - the leader is the only process that can change the replicated state
    - it does so by storing the sequence of operations that alter the state into a local log, which it replicaes to the followers
  - when the leader wants to change state
    - it first appends a new entry for the operation to its log and NOT to the state
    - it broadcasts the same AppendEntry operation to all followers who also appends it to their local log (and NOT to their local state)
    - once the leader receives an acknowledgement response from a majority of followers it considers the AppendEntry log to be committed and executes the entry against its local state
    - once the leaders local state is updated it sends another broadcast message to followers to update their state
- impact on leader election
  - a follower cant vote for a leader if the candidates log is less up-to-date than its own log
    - this way only the most up to date candidate can become the leader

### Dynamo-style Data stores

- in the author's opinion: the best known design of an eventually consistent and highly available key-value store
- many other key-value stores have been inspired by it: e.g. cassandra and Riak Kv
- novelty
  - every replica can accept write and read requests
  - write quorum: when a clients sents a write entry to the data store:
    - it sends the request to N replicas in parallel
    - waits for an acknowledgment from only W replicas
  - read quorum: when a client wants to read an entry from the data store:
    - it sends the request to all replicas
    - waits for just R replies
    - returns the most recent entry
  - to resolve conflicts
    - entries behave like LWW or MV registers depending on the implementatoin behavior
  - when W + R > N: the write quorum and the read quorum must intersect with each other
    - i.e. at least one read will return the latest version
    - read heavy workloads: benefit form a smaller R;
      - i.e. less read-replicas need to agree on what the most recent entry is
  - when W + R < N: for maximum performance at the expense of consistency
- read repair: a mechanism that clients implement to help bring replicas back in sync whenever they perform a read
  - basically a client that sends a read request, but fails to get a read quorom
  - it resends its latest write request to bring out-of-sync replicas back in sync
- replica synchronization: continous backgorund mechanism that runs on every replica and periodically communicates with others to identify and repair inconsisistencies

### Google Spanner

- arguable one of the most sucessful NewSQL data stores
- breaks key-value pairs into partitions in order to scale
- each partition is replicated across a group of nodes in different data centers using the Paxos protocol
- uses 2PC to support transactions that span multiple partitions
- uses MVCC combined with 2PL to guarantee isolation between transactions
