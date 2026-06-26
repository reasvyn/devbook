# Mid Backend Developer

## Description

What a mid-level backend developer should know — designing APIs, database optimization, caching, testing, and operational readiness.

## Prerequisites

- [Junior Backend Developer](../junior/backend-developer.md)

## Learning Path

### Language & Runtime Deep Dive

- `🔴 CRITICAL` Your primary language in depth — concurrency, error handling, standard library
- `🔴 CRITICAL` Dependency injection — patterns, frameworks (if applicable)
- `🟠 HIGH` Type safety — TypeScript for Node, typed Python, strict Go
- `🟠 HIGH` Profiling and debugging — pprof, Pyflame, Node Inspector

### API Design

- `🔴 CRITICAL` RESTful design principles — HATEOAS, versioning, pagination
- `🔴 CRITICAL` Input validation — schema validation, whitelisting
- `🔴 CRITICAL` Error handling — structured error responses, error codes
- `🟠 HIGH` API documentation — OpenAPI / Swagger
- `🟠 HIGH` Rate limiting, throttling, backpressure

### Databases

- `🔴 CRITICAL` Advanced SQL — subqueries, window functions, CTEs, indexes
- `🔴 CRITICAL` ORM usage — Prisma, TypeORM, SQLAlchemy, GORM
- `🔴 CRITICAL` Migrations — versioning database schema changes
- `🟠 HIGH` NoSQL — MongoDB, Redis data models
- `🟠 HIGH` Query optimization — EXPLAIN, analyzing slow queries
- `🟢 LOW` Replication and read replicas

### Caching

- `🔴 CRITICAL` HTTP caching — Cache-Control, ETag, conditional requests
- `🔴 CRITICAL` Application caching — Redis, in-memory caches
- `🟠 HIGH` Cache invalidation strategies — TTL, write-through, write-behind

### Message Queues & Async

- `🟠 HIGH` Message queues — RabbitMQ, Kafka, Redis Streams
- `🟠 HIGH` Background jobs — Bull, Celery, Sidekiq
- `🟢 LOW` Event-driven architecture basics

### Testing

- `🔴 CRITICAL` Unit and integration tests — mocking databases, HTTP clients
- `🔴 CRITICAL` API contract testing
- `🟠 HIGH` Load testing basics — k6, Artillery
- `🟢 LOW` Chaos engineering concepts

### Security

- `🔴 CRITICAL` Authentication — JWT, OAuth2, session-based auth
- `🔴 CRITICAL` Authorization — RBAC, ABAC, permission models
- `🔴 CRITICAL` Input sanitization — preventing injection (SQL, NoSQL, command)
- `🟠 HIGH` Rate limiting and brute-force protection
- `🟠 HIGH` Secrets management — env vars, vaults

### Observability

- `🟠 HIGH` Structured logging — JSON logs, log levels
- `🟠 HIGH` Metrics — Prometheus, Grafana basics
- `🟢 LOW` Distributed tracing

### Soft Skills

- `🔴 CRITICAL` Estimating technical effort accurately
- `🔴 CRITICAL` Code review — spotting logic errors, security issues
- `🟠 HIGH` Incident response — identifying, triaging, fixing production issues

## Next Steps

- [Senior Backend Developer](../senior/backend-developer.md) — system design, scalability, mentoring
