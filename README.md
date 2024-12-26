# Zeus: Next-generation, all-in-one back-end framework.

Zeus encompasses and fully centers itself around sophisticated architectural ideas such as DDD (domain-driven design), CQRS, event sourcing.

From a technical standpoint, Strata is exclusively built on top of open-source software; its "opinions" in terms of infrastructure, also known as "stack", include:
- NATS as the connective backbone — middleware used for asynchronous messaging, synchronous (blocking) request-reply, distributed locking, key-value storage, and configuration management. (See [Why NATS]())
- MongoDB as the source-of-truth event store (i.e. write model storage). (See [Why MongoDB]())
- Elasticsearch as the query engine and storage for read models. (See [Why Elasticsearch]())
- GraphQL as the facade for the outside world and client applications (See [Why GraphQL]())

Zeus is fully written in, and uses modern C#/.NET as its foundational platform.

# Why NATS:
NATS is Strata's single most important piece of infrastructure. It alone powers and fulfills most of its infrastructure needs.

## The result:
Through its carefully made tradeoffs and its design axioms that were taken to be non-negotiable from day one, Strata manages to achieve numerous highly desired properties in back-end systems:
- Native, out-of-the-box horizontal scalability
- Automatic load-balancing
- Full location transparency
- End-to-end type-safety (across service boundaries, with versioning)
- Purely event-driven/everything's real-time (Strata never "polls" anything; everything, from read model updates, to publishing of messages from the outbox, happen "reactively", in an event-driven fashion, yielding near real-time synchronization across the entire system.

TODO:
## Functional "bias" and its powerful implications:
## There is no such thing as a "service".
At best, it's an insufficiently-descriptive choice of naming; and at worst, it's indicative of something more fundamentally wrong, either tight coupling, mixing of concerns, or otherwise just bad design.
