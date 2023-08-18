# Distributed systems

- by Roberto Vitillo
  - reading: done
  - copying: paage 72 > raft leader election
- a group of nodes that cooperate by exchanging messages over communication links to achieve some task
- the emergence of an application requiring high availability and resilience against single node failures

## links

- [law of leaky abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)
- [cubic TCP algorithm](https://en.wikipedia.org/wiki/CUBIC_TCP)
- [grpc](https://grpc.io/)

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
- linearizable
- compare-and-swap operation: atomically updates the value of a key if and only if the process attempting to update the value correctly identifies the current value

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

#### Raft leader election

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
  - it votes for itself and sends a request to all processes in the system to vote for it, stamping tyhe request with the current term election
  - the process remains in the candidate state until
    - the candidate wins the election
    - another process wins the eletion
    - a period of time goes by with no winner
      - i.e. a split vote: the candidate state will eventually time out and start a new election and repeat until a process wins
