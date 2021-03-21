# Distributed Systems (RPC)

- Some servers don't serve the entire page initially, e.g twitter/facebook feeds
  - Full page reloads are costly
- Allow web pages to request small chunks of data (such as HTML, XML, JSON, or plain text) and display them only when needed
- A server may also need to access multiple other servers (JSON, XML)
- API SHIT INCOMING WITH AJAX REQUESTS

## Introduction

### Hadoop Distributed File System

- Connect over the internet for distributed storage
- Scale out performance and capacity by adding servers
- Files are stored across 3 servers for greater reliability

### Remote Procedure Call

- Basic RPC operation
  - The client will ask the server to do a task for it

### Map Reduce

- Processing and generating big data sets with a parallel, distributed algorithm on a cluster
- Maps and then reduces

---

- All these services are to store data or do operations

## A Distributed System Is

- A group of computers working together as to appear as a single computer to the end-user
- These machines have a shared state, operate concurrently
- Communicate via a network to achieve a task
- Networked computers communicate and coordinate their actions only by passing messages

### Why Need Them

- A desire to share resources
- The scalability, performance and high availability of applications
  - Web search
  - Distributed information
  - Network file systems
  - Real-time process control
  - Parallel computation

### Benefits

- Computation speedup, better performance
  - Parallel computation on multiplier servers
  - Solve bigger problems
- Redundancy, reliability, fault-tolerant
  - Several machines can provide the same services, so if one is unavailable, work does not stop
- Many applications are inherently distributed

### Challenges

- The network may fail
- Processes may crash
- Writing a program to run on a single computer is comparatively easy
- How can we coordinate the processes of these machines?

### Goal

- The goal of making the programming of distributed systems look similar, if not identical, to conventional programming

## Remote Procedure Call (RPC)

- Ideally, RPC makes a call to a remote function look the same as a local function call
- In practice,
  - What if the service crashes during the function call?
  - What if a message is lost?
  - What if a message is delayed?
  - If something goes wrong, is it safe to retry?
- A process on a machine A can call a procedure on machine B
- The process on A is suspended and execution continues on B
  - It will be a blocking call
- When B returns, the return value is passed to A and A continues execution

### Local Procedure Calls

- Everything in the same address space
- Calls cannot fail (they can but programmers have full control)
  - `ret = foo(64, "string", &myStruct);`
    - `&myStruct` by reference
- Push arguments, local variables and return address in the stack
- Call foo
- Pop everything out of the stack and put ret to a register
- Continue with the next call
  - What about parameter passing?

### RPC: How Do You Pass Parameters

- Passing by value is simple (just copy the value into the network message)
- Passing by reference is hard: it makes no sense to pass an address to a remote machine
- A memory location in a process on one machine is meaningless to another process on another machine

### RPC: Client Side

- Stub function
  - The same signature as if it was called locally
  - Marshals parameters
  - Sends request message
  - Waits for a response from the server
  - Un-marshals the response and returns the appropriate data

### RPC: Server Side

- Dispatcher
  - Receives client requests
  - Identifies appropriate function to call
- Skeleton
  - Un-marshals parameters
  - Calls the local function
  - Marshals the response and sends it back to the dispatcher

### Representing Data

- In remote calls:
  - Different byte ordering
    - Big Endian - most significant byte is stored in the smallest address
    - Little Endian - least significant byte is stored in the smallest address
  - Different sizes of integers and other types
  - Different floating point representations
  - Different languages

### Interface Definition Language (IDL)

- IDL: language-independent API specification
- IDL provides language-independent type signatures of the functions that are being made available over RPC
- Only to define interface signatures (not implementation)
- Support for cross language RPC
- The IDL compiler is responsible for generating service stubs use by clients and servers
- Allow programmer to specify remote procedure interfaces (names, parameters, return values)
- Pre-compiler can use this to generate:
  - Client and server stubs
  - Marshalling code
  - Un-marshalling code
  - Network transport routines
- Data serialisation formats, various ways to convert complex objects to sequences of bits
- Map in-memory data structures to a wire-format (how datatypes use the underlying transport to encode/decode themselves) (JSON, XML, plain text, compact, binary)
- A set of rules for encoding data in a format that is both human-readable and machine-readable
