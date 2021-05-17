# Elections And Group Communication

## Elections

- Many algorithms used in distributed systems require a coordinator that performs functions needed by other processes in the system
- Election algorithms choose a process from a group of processors to act as a coordinator
- Election algorithm assumes that every active process in the system has a unique priority number
- The process with highest priority will be chosen as a new coordinator
- Election algorithms
  - Ring-based algorithm
  - Bully algorithm

### Election Algorithms

- But leader may fail (crash)
- Some process should detect this
- Then what?

#### Assumptions And Requirements

- Any process can call for an election
- A process can call for at most one election at a time
- The result of an election should not depend on which process calls for it
- The non-faulty process with the best (highest) election attribute value (e.g., highest ID or address, or fastest CPU, etc.) is elected
- The highest number is sent to every active process in the distributed system

### The Ring Algorithm

- This algorithm applies to systems organised as a ring (logically or physically)
- In this algorithm we assume that the link between the processes are unidirectional
- And every process can message to the process on it's right only

#### Ring Algorithm: Example

- We start with 6 processes, connected in a logical ring.
- Process 6 is the leader, as it has the highest number
- If process 6 fails ...
  - The process to the left of 6 notices that 6 does not respond
  - It starts an election, sending a message containing its ID to the next node in the ring
  - Then all consecutive nodes also add their IDs to the message
  - When the process that started the election receives the message back, it knows the message has gone around the ring, as it's own ID is in the list
  - Picking the highest ID in the list, it starts the coordinator message "5 is the leader" around the ring

## Group Communication

- Collection of processes can communicate reliably over one-to-one channels
- Processes are members of groups - destinations of messages sent with the multicast operation - a process belongs to a single group
- `multicast(g, m)`: sends message `m` to all members of group `g`
- `deliver(m)`: delivers message `m` sent by multicast to the process
- A multicast message can be queued before the `deliver(m)` operation is called
- Every message `m` carries the unique identifier of the sender and the unique destination group identifier

### Broadcast Protocols

- Broadcast (multicast) protocols: algorithms for delivering one message to multiple recipients
- Broadcast (multicast) is group communication:
  - One node sends message, all nodes in group deliver it
  - Set of group members may be fixed (static) or dynamic
  - If one node is faulty, remaining group members carry on
  - Note: concept is more general than IP multicast (we build upon point-to-point messaging)

### Application/Broadcast Layering

- Node A -> Application (broadcasts) -> Broadcast algorithm (sends) -> Network -> Broadcast algorithm (receives) -> Application (delivers) -> Node B
- Assume network provides point-to-point send/receive
- After broadcast algorithm receives message from network, it may buffer/queue it before delivering to the application

### Forms Of Reliable Broadcast

- Example protocols include:
  - FIFO broadcast
  - Causal broadcast
  - Total order broadcast

#### FIFO Broadcast

- FIFO order: if a process broadcasts a message `m1` before it broadcasts a message `m2`, then no correct process delivers `m2`, unless it has previously delivered `m1`
- Messages sent by the same sender are delivered in the order they were broadcast
- Messages sent by different nodes can be delivered in any order

#### Causal Broadcast

- Causal Order: if the broadcast of message `m1` causally precedes the broadcast of message `m2`, then no correct process delivers `m2` unless it has previously delivered `m1`
- Causally related messages must be delivered in causal order
- Example:
  - Causal dependencies: `broadcast(m1) -> broadcast(m2)` and `broadcast(m1) -> broadcast(m3)`
- Causal broadcast does not impose any order on those messages that are not causally related

#### Total Order Broadcast

- Total order: if correct, processes `r` and `s` both deliver messages `m1` and `m2`, then `r` delivers `m1` before `m2` if and only if `s` delivers `m1` before `m2` (messages sent concurrently are delivered in identical order to the selected destinations)
- All nodes must deliver messages in same order

## What Is ZooKeeper

- An open-source, high-performance coordination service for distributed applications
  - Coordination: an act that multiple nodes must perform together
  - Exposes common services in simple interface
  - Getting node coordination correct is very hard
- ZooKeeper can be used for:
  - Configuration management (e.g, load configuration from a centralised source)
  - Group membership (e.g, Node join/leave)
  - Leader election
  - Synchonisation (e.g, locks, queues, barriers, ...)
  - Highly reliable data registry - Availability of data even when one or a few nodes are down
  - Naming: e.g find a machine in a cluster of many servers
- ZooKeeper is used heavily in the real world by many distributed systems
  - Apache kafka, hadoop, flume, hbase, twitter for service discovery

### ZooKeeper - Architecture: Replicated Mode

- Ensemble: Group of Zookeeper servers
- Leader: Server node that is elected on service startup
- Server:
  - One of the nodes in our ZooKeeper ensemble
  - Provides all the services to clients
  - The servers that make up the ZooKeeper service must all know about each other
  - Keep in-memory image of state
  - Transactions logs and snapshots - persistent
- Client:
  - Connect to a single ZooKeeper server
  - The client maintains a TCP connection
  - It sends requests, gets responses, gets watch events
  - Sends heart beats
  - If the TCP connection to the server breaks, the client will connect to a different server
- ZooKeeper service is replicated over a set of machines
- All machines store a copy of the data (in memory)
- A leader is elected on service startup
- Clients only connect to a single ZooKeeper server and maintains a TCP connection
- Client can read from any ZooKeeper server
- Writes go through the leader and needs majority consensus

### ZooKeeper - API

- Simple interface that supports only these operations:
  - Create : creates a node at a location in the tree
  - Delete : deletes a node
  - Exists : tests if a node exists at a location
  - Get data : reads the data from a node
  - Set data : writes data to a node
  - Get children : retrieves a list of children of a node
  - Sync : waits for data to be propagated

### ZooKeeper - Data Model/Hierarchical Namespace

- Hierarchical namespace:
  - Is a file system used for memory representation
  - Root zNode separated by `/` (root node is called `/`)
  - Each zNode can store some bytes of data
- zNode: data node with a `Stat` structure that includes version numbers for data changes and timestamps
- ZooKeeper internal data structure is like a tree
- Each node is called a zNode
- Each zNode has a path
- zNode can be persistent or ephemeral
- Each node can store data
- Each zNode can be watched for changes
