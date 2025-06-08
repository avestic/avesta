<img src="https://github.com/user-attachments/assets/c7f00d98-eb0f-449e-bb31-cf9f0c2aa100" alt="Avesta's logo" width="200" />

# Avesta: Next-generation, all-in-one back-end framework.

Avesta encompasses and fully centers itself around sophisticated architectural ideas such as **DDD** (domain-driven design), **CQRS**, **event sourcing**.

From a technical standpoint, Avesta is exclusively built on top of open-source software; its "opinions" in terms of infrastructure, also known as "stack", are the following trio:
1. [**NATS**](https://nats.io/) as the connective backbone, the "central nervous system" of the distributed system — it's the middleware used for asynchronous messaging, synchronous (blocking) request-reply, distributed locking, key-value storage, and configuration management. (See [Why NATS]())
2. [**MongoDB**](https://www.mongodb.com/) as the source-of-truth event store (i.e. write model storage). (See [Why MongoDB]())
3. [**Redis**](https://redis.io/open-source/) as the query engine and storage for read models, and general key value storage. (See [Why Redis]())

And that's it. No other out-of-process dependencies.

Other opinions include:
- **GraphQL** as the facade for the outside world and client applications (See [Why GraphQL]())

Avesta is fully written in, and uses modern C#/.NET as its foundational platform.
It makes heavy use of bleeding-edge C# features, including but not limited to the following:
- Static interface members
- Record types (more generally, features around immutability such as the `with` keyword)
- Source generators & analyazers
- Interceptors
- Nested interface definitions

The C# of even 5 years ago would have been too inexpressive of a language for Avesta's patterns.

Language features that Avesta still feels a dire need for in C#:
- [Associated types](https://github.com/dotnet/csharplang/discussions/8710)
- [Covariance/contravariance for structs and classes](https://github.com/dotnet/csharplang/discussions/2498)
- Discriminated unions
- Static extensions & extension properties
- [Negative generic constraints](https://github.com/dotnet/csharplang/discussions/707)

# Why NATS:
NATS is Avesta's single most important piece of infrastructure. It alone powers and fulfills most of its infrastructure needs.

## The result:
Through its carefully made tradeoffs and its design axioms that were taken to be non-negotiable from day one, Avesta manages to achieve numerous highly desired properties in back-end systems:
- Native, out-of-the-box horizontal scalability (with elegant distributed coordination, workqueue support, etc.)
- Automatic load-balancing
- Full location transparency
- End-to-end type-safety (across service boundaries, with versioning)
- Purely event-driven/everything's real-time — "polling" is a sin in Avesta; everything, from read model updates, to the publishing of messages from the outbox happens "reactively", in an event-driven fashion, yielding near real-time synchronization across the entire system.
- Single executable — no other deployment artifacts. Avesta applications always ship as one runnable program, minimizing operational complexity, both during development and also in production; we don't have multiple executable projects (e.g. one for each background worker, etc.)
- Exactly-once message processing (thanks to solid outbox+inbox patterns)
- Maximally-performant, maximally-flexible view (read model) system
- Support for modeling the "future", scheduling deferred events
- Periodic background jobs
- Powerful primitives and abstractions for domain modeling (e.g. aggregate roots, aggregate members, events, reactions, etc.)
- Meticulous handling of concurrency
- Efficient, MessagePack-based (de)serialization
- Full observability support (all-encompassing traces, rich metrics, and extensive logs)
- Sophisticated healthcheck mechanism, taking into account view up-to-date-ness, etc.

From a more abstract standpoint, Avesta aims to approximate the ideals of:
- Type-safety — compile-time checks over runtime exceptions
- Explicitness over implicitness
- Developer ergonomics
- Code "aesthetics"
- Principle of least surprise (i.e. it just works)

TODO:
## Event sourcing — the only approach for an exhaustive, lossless source of truth
## Functional "bias" and its powerful implications:
Avesta is loosely inspired by the "Functional Core, Imperative Shell" philosophy.
## There is no such thing as a "service".
At best, it's an insufficiently-descriptive choice of naming; and at worst, it's indicative of something more fundamentally wrong, either tight coupling, mixing of concerns, or otherwise just bad design.
## If you're not building microservices, Avesta still gives you, out of the box, what you'd have to try hard to get on your own: seamless horizontal scalability for your monolith.

# FAQ:
### "masterfully-crafted"? Cringe.
One man's cringe is another's profundity. Give it some time and you too will be drawn into the cringe.

### What is "Avestic"?
Avestic is an adjective; and here, it means an application built on top of Avesta, the framework. It's short for "Avesta-based".
