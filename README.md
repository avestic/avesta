<img src="https://github.com/user-attachments/assets/c7f00d98-eb0f-449e-bb31-cf9f0c2aa100" alt="Avesta's logo" width="200" />

# Avesta: Next-generation, all-in-one, cloud-native back-end framework.

Avesta encompasses and fully centers itself around sophisticated architectural ideas such as **DDD** (domain-driven design), **CQRS**, **event sourcing** — the famous DDD-CQRS-ES trifecta — as well as an event-driven architecture, and unformly adhering to [the hexagonal architecture](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)) (a.k.a. ports & adapters, onion architecture, clean architecture — all the same). The most important high-level ideals Avesta aims to facilitate are **correctness**, **scalability**, and **performance**.

From a technical standpoint, Avesta is exclusively built on top of open-source software; its "opinions" in terms of infrastructure, also known as "stack", are the following trio:
1. [**NATS**](https://nats.io/) (global across the system) as the connective backbone, the "central nervous system" of the distributed organism — it's the middleware used for asynchronous messaging, synchronous (blocking) request-reply, distributed locking and coordination, work queue mechanism, load balancing, and configuration management. (See [Why NATS](#why-nats))
2. [**MongoDB**](https://www.mongodb.com/) (per-service — with sharding and replication support) as the source-of-truth/system-of-record event store (i.e. write model storage). (See [Why MongoDB](#why-mongodb))
3. [**Redis**](https://redis.io/open-source/) (per-service — with sharding and replication support) as the query engine and storage for read models, and general key value storage. (See [Why Redis](#why-redis))

And that's it. No other out-of-process dependencies.

Other opinions include:
- **GraphQL** as the facade for the outside world and client applications (See [Why GraphQL]())

Avesta is fully written in, and uses modern C#/.NET as its foundational platform.
It makes heavy use of bleeding-edge C# features, including but not limited to the following:
- Static interface members
- Immutability-oriented features (such as record types, the `with` keyword, etc.)
- Source generators & analyzers
- Interceptors
- Extension everything

The C# of even 5 years ago would have been too inexpressive of a language for Avesta's patterns.

Language features that Avesta still feels a dire need for in C#:
- Discriminated unions
- [Associated types](https://github.com/dotnet/csharplang/discussions/8710)
- Higher-kinded types
- [Covariance/contravariance for structs and classes](https://github.com/dotnet/csharplang/discussions/2498)
- [Negative generic constraints](https://github.com/dotnet/csharplang/discussions/707)

### Why NATS:

NATS is Avesta's single most important piece of infrastructure. It alone powers and fulfills most of its infrastructure needs. I claim that it is the greatest modern pub/sub software that's been built.
- Minimal latency, lightweight, simple
- Unparalleled versatility
- Profoundly powerful primitives (e.g. subjects), offering deduplication, strong consistency

### Why MongoDB:
- Native watch feature
- Document database is most appropriate for an event store because events are highly polymorphic — and MongoDB is the most well-known, well-supported document-oriented database
- No impedance mismatch/complex ORM overhead — the MongoDB driver is sufficient and supports complex querying

### Why Redis:
- Minimal latency — Redis trades performance for durability guarantees, given its in-memory nature, which is perfect for Avesta's views since they are pure derivations and can be reconstructed on-demand
- Immediate consistency after adding/updating documents in RediSearch (something that Elasticsearch, for example, expressly doesn't have and would be a deal breaker for Avesta's needs)
- Reliable pub/sub in the form of Redis Streams that can be used for real-time notifications (again, something that Elasticsearch doesn't support)

## The result:
Through its carefully made tradeoffs and its design axioms that were taken to be non-negotiable from day one, Avesta manages to achieve numerous highly desired properties in back-end systems:
- Transparent, out-of-the-box **near-linear horizontal scalability** — achieved by workload partitioning, elegant distributed coordination, load-balanced workqueue support, etc. <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- **Immediate auto-failover** and rebalancing when an Avestic app's nodes are shut down or spun up — by extension making the system **fault-tolerant** and **highly-available** <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Automatic **load-balancing** <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Full **location transparency** — there is no need for URL sharing (not only for messages, and request-replies) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- End-to-end **type-safety** (across service boundaries, with versioning) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Purely **event-driven**/everything's real-time — "polling" is a sin in Avesta; everything, from read model updates, to the publishing of messages from the outbox happens "reactively", in an event-driven fashion, yielding near real-time synchronization across the entire system. <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Fully **event-sourced**, the only approach to persistence of business data that guarantees no data loss and thereby yielding a maximally-rich data source, providing verifiably _exhaustive_ auditability.
- Unforgiving, robust **type system** with Roslyn analyzers sprinkled on top, catching and eliminating entire classes of bugs at compile-time
- Event-sourced aggregate **snapshot** support for efficient loading of aggregate instances
- **Single executable** — no other deployment artifacts. Avestic applications always ship as one runnable program, minimizing operational complexity, both during development and also in production; we don't have multiple executable projects (e.g. one for each background worker, etc.) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Powerful primitives and abstractions for **domain modeling** (e.g. aggregate roots, aggregate members, events, reactions, etc.) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- **Exactly-once** message processing (as a result of scrupulous outbox & inbox pattern implementations) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- **Sequence-preserving** message consumption guarantees <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Near real-time synchronized, composable **view (read model) system** with **O(1) time complexity** for most queries <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Support for modeling **the "future"** (and possible futures), by scheduling/cancellation of **deferred events** <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Granular and comprehensive tracking of **cause and effect** with regards to state transitions, across and within bounded contexts. <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Comprehensive, in-built primitives to enable **set-based validation** (e.g. uniqueness) — a notoriously nontrivial problem to solve in DDD-CQRS-ES-based systems. <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Built-in, tunable **backpressure** in the context of distributed stream processing workloads
- Support for periodic **background jobs** <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Meticulous and bullet-proof handling of **concurrency** at all levels, with _automatic retries_ wherever applicable <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Efficient **MessagePack-based** (de)serialization — isolated serialization per service <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Uniform use of **value objects** (a.k.a. branded types, newtypes, fresh types), enhancing type-safety, and making "invalid states unrepresentable". <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Deep, built-in **extensibility** support, through a wide variety of _hooks_ <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Request-reply **resilience** capabilities
- Targeted, conditional **configuration mechanism** with **hot-reload**
- Utilizes **Rx primitives** (specifically, the modern [R3 implementation](https://github.com/Cysharp/R3)) to express all **stream processing** workloads, in order to achieve optimal throughout and correctness
- Fine-grained, expressive **authorization instruments**
- Queries (read requests) **fulfilled at the edge** — no roundtrips to upstream services (who have more important shit to do — i.e. commands), no request-replh definition with lots of boilerplate and repetitive work
- No need for a cache — views already have minimal latency and performance cost to retrieve, while guaranteeing eventual consistency & real-time updates
- Faciliates the elimination of synchronous/blocking service-to-service communication — in a typical Avestic system, there is exactly **zero temporal coupling** between services (which is the most essential best practice with regards to microservices)
- Full **observability** support (all-encompassing traces, rich metrics, and extensive logs) <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Sophisticated **healthcheck mechanism**, taking into account view up-to-date-ness, etc. <sup>[Tell me more](https://avestic.dev/docs/horizontal-scalability)</sup>
- Built for **zero-downtime**, blue-green deployments
- By virtue of being DDD-based and as such defining explicit consistency boundaries and being free from direct loosely-decoupled relationships, an Avestic system lends itself exceptionally well to sharding for its underlying source-of-truth data store — which is the only real solution to distributing your source-of-truth database — and this property is exceedingly difficult to achieve for systems not built on these principles.
- Mock-free ([fakes](https://testing.googleblog.com/2013/06/testing-on-toilet-fake-your-way-to.html?m=1)-[only](https://www.reddit.com/r/programming/s/kUhdqnw6AW)) testing framework/blueprint for writing fully in-process (read 'fast'), deterministic, I/O-free unit tests against the service's public API, along with an ability to programmatically advance time, capture and verify side effects, etc. — built on top of [TUnit](https://github.com/thomhurst/TUnit)
- TDD-aligned testing blueprint that achieves maximally [high-fidelity tests](https://www.reddit.com/r/programming/s/kUhdqnw6AW), acting as live specifications of the system, closely matching the high-level behaviors of real clients of the service
- Meticulous snapshot-testing to document and record changes (and catch bugs) in internal state transitions
- Unit tests against the public API of the service (read 'requests'), decoupled from lower-level inner workings (including entities, etc.) and treating them as implementation details — tests that actually fulfill the promise of remaining robust against refactorings
- Dummy-data-free/seeding-free tests. The tests are written in the form of a series of interactions with the public contract of the service in particular ways; **making invalid internal states unrepresentable**. This is in contrast to typical testing practices where you generate or hand-write data in a "manual" way to bring the SUT into a desired state before attempting to exercise its functionalities, which is an approach that effectively bypasses invariants. In Avestic tests, the desired state is brought about in precisely the same way it is in the equivalent production setting (i.e. through a series of interactions by a user/client with the system); this completely "seals" the SUT and treats it as a black box, preventing potentially-invalid and boring-to-write contrived states from sneaking into it that could lead to the very worst kind of software development nightmare: wrong tests.

- User sessions resolved lazily and at the service level so that even if the gateway is somehow bypassed, auth checks still hold.

From a more abstract standpoint, Avesta aims to approximate the ideals of:
- Performance — performance is a function of architecture; Avesta doesn't obsess over being maximally efficient at the low-level and instead prioritizes convenience and elegance over that, but it makes the right trade-offs at the high-level in terms of architecture to achieve practically perfect performance.
- Type-safety — compile-time checks over runtime exceptions
- Explicitness over implicitness
- Developer ergonomics
- Code "aesthetics"
- Principle of least surprise (i.e. it just works)

There is a single major tradeoff Avesta makes, and that is the loss of language-agnosticism. Avesta expects your system of microservices to be fully Avestic (and by extension, based on C#/.NET). That being said, it is, in principle, entirely possible to make Avesta polyglot, by introducing a language-agnostic contract definition layer (since the infrastructure-level dependencies are all language-agnostic); but that, of course, would need doing.

Avesta's relationship with other prominent technologies in the .NET ecosystem — Avesta:

**Builds on top of:**
- .NET
- ASP.NET Core

**Can be coupled with:**
- Aspire
- EF Core
- (everything else)

**Aims to rival:**
- Microsoft Orleans
- Akka .NET
- Dapr
- Temporal.io
- Axon (JVM)
- Prooph (PHP)
- Commanded (Elixir)

What Avesta frees you from:
- DTOs, DTOs everywhere, mappings, oh god mapping code...
- Dealing with ORM, impedance mismatch, embedded SQL
- Most importantly, Avesta frees you from itself — it guides toward the domain

When To Use Avesta:
Use Avesta when correctness is paramount.

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

# Licensing:
MIT-licensed.
@aradalvand's magnum opus.
