# Perseus: Next-generation, all-in-one back-end framework.

Perseus encompasses and fully centers itself around sophisticated architectural ideas such as DDD (domain-driven design), CQRS, event sourcing.

From a technical standpoint, Zeus is exclusively built on top of open-source software; its "opinions" in terms of infrastructure, also known as "stack", include:
- **NATS** as the connective backbone — middleware used for asynchronous messaging, synchronous (blocking) request-reply, distributed locking, key-value storage, and configuration management. (See [Why NATS]())
- **MongoDB** as the source-of-truth event store (i.e. write model storage). (See [Why MongoDB]())
- **Elasticsearch** as the query engine and storage for read models. (See [Why Elasticsearch]())
- **GraphQL** as the facade for the outside world and client applications (See [Why GraphQL]())

Zeus is fully written in, and uses modern C#/.NET as its foundational platform.
It makes heavy use of bleeding-edge C# features, including but not limited to the following:
- Static interface members
- Record types (+ features around immutability like the `with` keyword)
- Source generators
- Interceptors
- Nested interface definitions

The C# of even 5 years ago would not have been a capable enough language to support expressing Perseus's patterns.

# Why NATS:
NATS is Zeus's single most important piece of infrastructure. It alone powers and fulfills most of its infrastructure needs.

## The result:
Through its carefully made tradeoffs and its design axioms that were taken to be non-negotiable from day one, Zeus manages to achieve numerous highly desired properties in back-end systems:
- Native, out-of-the-box horizontal scalability
- Automatic load-balancing
- Full location transparency
- End-to-end type-safety (across service boundaries, with versioning)
- Purely event-driven/everything's real-time (Zeus never "polls" anything; everything, from read model updates, to publishing of messages from the outbox, happen "reactively", in an event-driven fashion, yielding near real-time synchronization across the entire system.

From a more abstract standpoint, Zeus aims to approximate the ideals of:
- Type-safety — compile-time checks over runtime exceptions
- Explicitness over implicitness
- Developer ergonomics
- Code aesthetics
- Principle of least surprise (i.e. it just works)

TODO:
## Event sourcing — the only approach for an exhaustive, lossless source of truth
## Functional "bias" and its powerful implications:
## There is no such thing as a "service".
At best, it's an insufficiently-descriptive choice of naming; and at worst, it's indicative of something more fundamentally wrong, either tight coupling, mixing of concerns, or otherwise just bad design.
## If you're not building microservices, Perseus is still appropriate, because it gives you, out of the box, what you'd have to try hard to get on your own: seamless horizontal scalability for your monolith.
