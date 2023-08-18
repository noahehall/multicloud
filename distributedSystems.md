# Distributed systems

- by Roberto Vitillo
  - reading: done
  - copying: 90 > chain replication
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

## hallmarks of distributed systems

- communication
  - how are requests & responses represented on the wire
  - what happens in the event of temproary network outages
  - how to guard `against snooping
- coordination
  - especially in the presence of failures
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

- a group of processes has to decide a value so that
  - every non-faulty process eventually agrees on a value
  - the final decision of every non-faulty process is the same everywhere
  - the value that has been agreed on has been proposed by a process

## Replication

- increases availability, scalability and performance by redundantly storing data and operating services across multiple nodes

### State Machine Replication

- the main idea is that a single process (the leader) broadcasts operations that changes it's state to other processes (the followers)
- if each follower executes the same sequence of operations as the leader, then each follower will end up in the same state as the leader
- a large part of state machine replication is didicated to fault-tolerance
  - any process can fail at any time
  - broadcast messages can be lost due to network failures
- all operations must be deterministic so that all followers end up in the same state
- goals
  - a deterministic way to solve the problem of consensus

### Chain Replication

- deals with the coordination tax paid in leader-based replication by moving coordination operations off a systems critical path

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
  - a client can send two reads that are resolved by different followers each having different views of the state
- the only guarantee this provides is that all followers will eventual converge on the same state

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

## Implementations

### Paxos

### Raft

- leader-based replication

#### leader election

- implemented as a state machine in which any process is in one of three states
  - follower: the process recognizes another one as the leader
  - candidate: the process starts a new election proposing itself as a leader
  - leader: the process is the leader
- time: is divided into election terms of arbitrary length that are numbered in consecutive integers (i.e. logical timestamps)
  - a term begins with a new election, during which one/more candidates attempt to become the leader
- Process
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

#### Replication

- uses state machine replication
- general process
  - when the system starts up, a leader is elected using Rafts leader election algorithm
  - the leader is the only process that can change the rpelicated state
    - it does so by storing the sequence of operations that alter the state into a local log, which it replicaes to the followers
  - when the leader wants to change state
    - it first appends a new entry for the operation to its log and NOT to the state
    - it broadcasts the same AppendEntry operation to all followers who also appends it to their local log (and NOT to their local state)
    - once the leader receives an acknowledgement response from a majority of followers it considers the AppendEntry log to be committed and executes the entry against its local state
    - once the leaders local state is updated it sends another broadcast message to followers to update their state
- impact on leader election
  - a follower cant vote for a leader if the candidates log is less up-to-date than its own log
    - this way only the most up to date candidate can become the leader
