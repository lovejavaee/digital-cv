---
layout: post
title: LRPC - Lightweight RPC
---



## LRPC - Lightweight RPC

Lightweight RPCs are the special case of RPCs where calling process and the called process are on the same machine. Remember the two forms of communication of a distributed system – explicit (passing data) and implicit (sharing memory). You can think of not using a RPC system for the special case that both processes are on the same machine but using a shared piece memory. The optimization is to construct the message as a buffer and simply write to the shared memory region. When client and server both are two processes on the same machine and you make RPC calls between two components on the same machine, following are the things which can make it better over the traditional RPC.

> Lightweight Remote Procedure Call (LRPC) is a communication facility designed and optimized for communication between protection domains on the same machine. In contemporary small-kernel operating systems, existing RPC systems incur an unnecessarily high cost when used for the type of communication that predominates-between protection domains on the same machine. This cost leads system designers to coalesce weakly related subsystems into the same protection domain, trading safety for performance. By reducing the overhead of same-machine communication, LRPC encourages both safety and performance.

## Overview/Main Points

- The Case for Lightweight RPC (LRPC):

  - Monolithic kernels are insulated from user programs, but few protection boundaries exist within the OS itself, which makes it difficult to modify, debug, and validate.
  - Capability systems consist of fine-grained objects sharing an address space but with their own protection domains. These offer flexibility, modularity, and protection.
  - The common case for communication is between domains on the same machine as opposed to across machines.
  - Most communication is simple, involving few arguments and little data, since complex data is often hidden behind abstractions (Both this and the above point were backed with some questionably representative examples).
  - While RPC is robust enough to handle both local and remote calls, it has high overhead. A lighter weight solution is necessary.

  

- Lightweight RPC features:

  - **Simple control transfer:** Conventional RPC involves multiple threads that must send signals and switch context. In LRPC, the kernel changes the address space for the caller's thread and lets it continue to run in the server domain (at the same priority?).
  - **Simple data transfer:**Shared buffers are pre-allocated (when the caller imports the LRPC module) for the communication of arguments and results. The caller copies the data onto the A-stack, after which no other copies are required.
  - **Simple stubs:** RPC has general stubs, but many of its features are infrequently needed. LRPC uses a stub generator that produces simple stubs in Modula2+ assembly language.
  - **Design for concurrency:**Idle processors in multi-processor machines cache domain contexts to reduce context-switch overhead. Counters are used to keep the highest activity LRPC domains active on idle processors.

---

- Performance:

  - 3X speedup for single processor.
  - Additional speedups for multiprocessor machines.

---

There are several use cases for local RPC, but a very straightforward example is when a server has both remote and local clients.

Let us consider for example an RPC-based print server:

- You may have clients located on remote hosts (e.g. for a networked/sharing print server);
- You may also have also clients located on the server's host itself (so that local applications can print too).

Obviously, you don't want to write both an print-server for remote clients and a separate print-server for local clients. Thus, it is far better if the architecture or middleware allows for designing a print server that can be used indifferently by remote clients (*remote RPC*) and local clients (*local RPC*).

At this point, the architecture or middleware ensures a common interface both for local clients and for remote clients: how inter-process communication is achieved in practice must be made fully transparent to the application developer.

However, using the same inter-process communication technology both for remote clients and for local clients may be inefficient. Thus, it is fairly common for the RPC architecture to implement some kind of optimization so as to optimize performance when the server and the client are located on the same host. In spirit, this optimization is very similar to the fact that local network communication use the local loop rather than going back and forth between the host and the network card.

*Lightweight RPC* is one such solution (it is not the only one) allowing for optimizing RPC performance for local clients. When this optimization is implemented into the RPC architecture:

- The same RPC interface can be made available both for local and for remote clients:
- Application developers do not have to handle the issue that some clients may be local when other clients may be remote;
- The RPC architecture optimizes under the hood calls to local servers, so that local clients are not penalized by some remote IPC technique that would be sub-optimal for local communication.

---
