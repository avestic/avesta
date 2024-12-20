# Strata — next-gen microservice framework.

Strata encompasses and fully centers itself around sophisticated architectural ideas such as DDD (domain-driven design), CQRS, event sourcing.

From a technical standpoint, Strata is exclusively built on top of open-source software; its "opinions" in terms of infrastructure include:
- NATS as the connective backbone — middleware used for asynchronous messaging, synchronous (blocking) request-reply, distributed locking, and configuration management. (See [Why NATS]())
- MongoDB as the source-of-truth event store (i.e. write model storage). (See [Why MongoDB]())
- Elasticsearch as the query engine and storage for read models. (See [Why Elasticsearch]())

Strata is fully written in, and uses modern C#/.NET as its fundamental platform.

# Why NATS:
NATS is Strata's single most important piece of infrastructure.
