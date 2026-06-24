# Mid Backend Developer

## Description

What a mid-level backend developer should know — designing APIs, database optimization, caching, testing, and operational readiness.

## Prerequisites

- [Junior Backend Developer](../junior/backend.md)

## Learning Path

### Language & Runtime Deep Dive

- `[must-know]` Your primary language in depth — concurrency, error handling, standard library
- `[must-know]` Dependency injection — patterns, frameworks (if applicable)
- `[good-to-know]` Type safety — TypeScript for Node, typed Python, strict Go
- `[good-to-know]` Profiling and debugging — pprof, Pyflame, Node Inspector

### API Design

- `[must-know]` RESTful design principles — HATEOAS, versioning, pagination
- `[must-know]` Input validation — schema validation, whitelisting
- `[must-know]` Error handling — structured error responses, error codes
- `[good-to-know]` API documentation — OpenAPI / Swagger
- `[good-to-know]` Rate limiting, throttling, backpressure

### Databases

- `[must-know]` Advanced SQL — subqueries, window functions, CTEs, indexes
- `[must-know]` ORM usage — Prisma, TypeORM, SQLAlchemy, GORM
- `[must-know]` Migrations — versioning database schema changes
- `[good-to-know]` NoSQL — MongoDB, Redis data models
- `[good-to-know]` Query optimization — EXPLAIN, analyzing slow queries
- `[nice-to-have]` Replication and read replicas

### Caching

- `[must-know]` HTTP caching — Cache-Control, ETag, conditional requests
- `[must-know]` Application caching — Redis, in-memory caches
- `[good-to-know]` Cache invalidation strategies — TTL, write-through, write-behind

### Message Queues & Async

- `[good-to-know]` Message queues — RabbitMQ, Kafka, Redis Streams
- `[good-to-know]` Background jobs — Bull, Celery, Sidekiq
- `[nice-to-have]` Event-driven architecture basics

### Testing

- `[must-know]` Unit and integration tests — mocking databases, HTTP clients
- `[must-know]` API contract testing
- `[good-to-know]` Load testing basics — k6, Artillery
- `[nice-to-have]` Chaos engineering concepts

### Security

- `[must-know]` Authentication — JWT, OAuth2, session-based auth
- `[must-know]` Authorization — RBAC, ABAC, permission models
- `[must-know]` Input sanitization — preventing injection (SQL, NoSQL, command)
- `[good-to-know]` Rate limiting and brute-force protection
- `[good-to-know]` Secrets management — env vars, vaults

### Observability

- `[good-to-know]` Structured logging — JSON logs, log levels
- `[good-to-know]` Metrics — Prometheus, Grafana basics
- `[nice-to-have]` Distributed tracing

### Soft Skills

- `[must-know]` Estimating technical effort accurately
- `[must-know]` Code review — spotting logic errors, security issues
- `[good-to-know]` Incident response — identifying, triaging, fixing production issues

## Next Steps

- [Senior Backend Developer](../senior/backend.md) — system design, scalability, mentoring
