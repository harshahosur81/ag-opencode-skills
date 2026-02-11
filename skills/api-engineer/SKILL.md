---
name: api-engineer
description: API Engineer
---

## 🔗 Lifecycle Triggers (Orchestration Integration)

**Incoming Dependencies (You cannot start until):**
- **From PM:** Received "PRD" with clear business goals.
- **From Design:** Received "High-Fidelity Mocks" (Phase 3 of Design).
- **From Architect:** Received "Architecture Decision Record" (if complex).

**Outgoing Handshakes (You must sync before building):**
- **To Mobile/Backend Counterpart:** "Contract Review." Agree on the JSON/API schema.
- **To QA:** "Risk Review." Tell them what is risky so they can plan tests.

**Definition of Done (You cannot merge until):**
- **Integration Check:** The Integration/Media Engineer has approved your usage of their components.
- **Visual QA:** The Designer has marked the build as "Visually Correct."
## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Data Modeling & Architecture

**BEFORE writing API endpoints:**

1.  **Schema Design**
    - Draw the Entity Relationship Diagram (ERD).
    - **Normalization:** Minimize redundancy (3NF) unless read-performance demands denormalization.
    - **Indexing:** What queries will be run most often? Index those fields now.
    - **Migrations:** How do we evolve this schema without downtime?

2.  **API Contract Design (API First)**
    - Define the interface (OpenAPI/Swagger/GraphQL) before coding.
    - **Review:** Get sign-off from Frontend/Mobile devs. "Does this JSON structure work for you?"
    - **Versioning:** Plan for `/v1/`. Breaking changes are expensive later.

3.  **Capacity Planning**
    - Estimate RPS (Requests Per Second).
    - Is this Read-heavy (Cache it?) or Write-heavy (Queue it?)
    - **Sync vs Async:** Should this be a direct response or a background job?

### Phase 1.5: Modern API Paradigms (2026)

**Beyond REST:**

1.  **Choosing the Right API Pattern**
    | Pattern | Use When | Don't Use When | Complexity |
    |---------|----------|----------------|------------|
    | **REST** | Public APIs, CRUD operations | Complex data aggregation | Low |
    | **GraphQL** | Mobile clients, flexible queries | Simple CRUD | Medium |
    | **tRPC** | TypeScript monorepos | Polyglot clients | Low |
    | **gRPC** | Microservice mesh, high throughput | Browser clients (needs proxy) | High |
    | **WebSockets** | Real-time bidirectional | Request-response pattern | Medium |
    | **SSE** | Server push, one-way streams | Bidirectional communication | Low |

2.  **GraphQL Considerations**
    - **Benefits:** Client specifies exact fields, single endpoint
    - **Challenges:** N+1 problem (use DataLoader), query complexity attacks
    - **Tools:** Apollo Server, GraphQL Yoga, Pothos (code-first)
    - **When:** Mobile apps with varying data needs

3.  **tRPC (Type-Safe RPC)**
    - **Full-stack TypeScript:** Share types between client/server
    - **No code generation:** Types inferred automatically
    - **Best for:** Next.js apps, internal tools
    ```typescript
    // Server
    const appRouter = t.router({
      getUser: t.procedure.input(z.number()).query(({ input }) => db.user.find(input))
    });
    // Client (auto-complete!)
    const user = await trpc.getUser.query(123);
    ```

4.  **gRPC for Internal Services**
    - **Protocol Buffers:** Binary format, smaller/faster than JSON
    - **Streaming:** Bi-directional streaming built-in
    - **Use Case:** Backend microservices, high-throughput systems
    - **Limitation:** Needs Envoy proxy for browser access

### Phase 2: Implementation & Security

**Logic ensuring integrity:**

1.  **Authentication & Authorization**
    - **AuthN:** Who are you? (JWT, OAuth).
    - **AuthZ:** What can you do? (RBAC/ABAC).
    - **Rule:** Never trust the client. Validate every input on the server.

2.  **Business Logic Isolation**
    - Keep Controllers "thin" (just parsing HTTP).
    - Put logic in Services/Domain layers.
    - **Transactions:** Ensure atomicity. If step B fails, step A must roll back.

3.  **Defensive Coding**
    - Handle timeouts and retries gracefully (Idempotency keys).
    - Sanitize inputs (SQL Injection, XSS prevention).
    - Rate Limiting: Protect your resources from abuse.

### Phase 3: Performance & Scalability

**Optimizing the flow:**

1.  **Database Query Optimization**
    - Eliminate N+1 queries.
    - Use `EXPLAIN ANALYZE` to check query cost.
    - Connection Pooling: Don't open a new connection for every request.

2.  **Caching Strategy**
    - Cache at the edge (CDN) for static assets.
    - Cache at the app level (Redis) for expensive computations.
    - **Hardest Problem:** Cache Invalidation. When does data expire?

3.  **Asynchronous Processing**
    - Offload heavy tasks (Email, Image Resizing) to Message Queues (RabbitMQ/SQS).
    - Don't block the main thread.

### Phase 3.5: Distributed Systems Patterns

**When you have multiple services:**

1.  **Event Sourcing**
    - **Pattern:** Store events, not current state
    - **Benefits:** Complete audit trail, time travel, replay events
    - **Use Case:** Financial systems, order processing
    - **Challenge:** Event schema evolution
    ```typescript
    // Instead of: UPDATE users SET balance = 100
    // Store: UserDepositedMoney(userId, amount, timestamp)
    ```

2.  **CQRS (Command Query Responsibility Segregation)**
    - **Pattern:** Separate read models from write models
    - **Benefits:** Optimize reads/writes independently
    - **Use Case:** Heavy read traffic with complex queries
    - **Tools:** MediatR, Event Store

3.  **Saga Pattern (Distributed Transactions)**
    - **Problem:** You can't use DB transactions across services
    - **Solution:** Choreography (events) or Orchestration (coordinator)
    - **Example:** Order placed → Reserve inventory → Charge card → Ship
    - **Failure:** Compensating transactions (refund if shipping fails)

4.  **Outbox Pattern (Reliable Events)**
    - **Problem:** How to update DB AND publish event atomically?
    - **Solution:** Write event to DB table, background worker publishes
    - **Benefits:** No lost events, exactly-once semantics
    - **Tools:** Debezium (CDC), Transactional Outbox

### Phase 4: Observability & Maintenance

**Keeping the lights on:**

1.  **Structured Logging**
    - Log context, not just text. `{ "userId": 123, "error": "db_timeout" }`.
    - Do not log PII (Passwords/Credit Cards).

2.  **Health Checks & Metrics**
    - Implement `/health` endpoints for Load Balancers.
    - Track Latency (p95, p99) and Error Rates.

3.  **Documentation**
    - Keep the API docs (Swagger) auto-generated or updated.
    - Document the "Why" in code comments for complex logic.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll add the database index later when it's slow."
- "The frontend validates this, so I don't need to." (Security hole).
- "I'll just loop through the database results in code." (Memory leak).
- "I'll store the secrets in the environment variables committed to Git."
- "I don't need a transaction for these two updates." (Data corruption).
- "It works with 10 users, it will work with 10,000."

**ALL of these mean: STOP. Return to Phase 1.**

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Design** | ERD, OpenAPI, Versioning | Schema defined, Contract agreed |
| **2. Logic** | Auth, Validation, Transactions | Secure, atomic operations |
| **3. Scale** | Caching, Indexing, Queues | p99 Latency under SLA |
| **4. Ops** | Logging, Metrics, Docs | Observable & Maintainable |

## 🛠️ Modern API Stack (2026)

### Frameworks
- **Node.js:** Fastify (fast), NestJS (enterprise)
- **Python:** FastAPI (modern), Django Ninja
- **Go:** Gin, Fiber, Echo
- **Rust:** Axum, Actix-web (bleeding edge)

### Databases
- **OLTP:** Postgres 17, MySQL 8.4, CockroachDB (distributed)
- **NoSQL:** MongoDB, DynamoDB
- **Cache:** Redis 7, Dragonfly, Valkey
- **Search:** Elasticsearch, Typesense, Meilisearch
- **Vector:** pgvector, Pinecone (AI embeddings)

### API Patterns
- **REST:** OpenAPI 3.1 spec
- **GraphQL:** Apollo Server, GraphQL Yoga
- **tRPC:** TypeScript full-stack
- **gRPC:** High-performance internal

### Observability
- **APM:** Datadog, New Relic, Sentry
- **Tracing:** OpenTelemetry, Jaeger
- **Logs:** Structured JSON (Pino, Winston)

### Database ORMs/Query Builders
- **TypeScript:** Prisma, Drizzle, Kysely
- **Python:** SQLAlchemy, Tortoise ORM
- **Go:** GORM, sqlc

## 📊 Advanced Database Optimization

### Index Strategy
```sql
-- B-tree index (default, equality/range)
CREATE INDEX idx_user_email ON users(email);

-- Covering index (includes SELECT columns)
CREATE INDEX idx_order_user_total ON orders(user_id) INCLUDE (total, created_at);

-- Partial index (filtered)
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- GIN index (arrays, JSON, full-text)
CREATE INDEX idx_tags ON posts USING GIN(tags);
```

### Query Optimization Checklist
- [ ] Run `EXPLAIN ANALYZE` on all queries
- [ ] Eliminate N+1 queries (use JOINs or DataLoader)
- [ ] Add indexes on foreign keys
- [ ] Use connection pooling (pg-pool, Prisma)
- [ ] Set appropriate `work_mem` for complex queries
- [ ] Monitor slow query log

### Modern Postgres Features (2026)
```sql
-- Vector similarity search (AI embeddings)
CREATE EXTENSION vector;
SELECT * FROM documents ORDER BY embedding <=> '[0.1, 0.2, ...]' LIMIT 10;

-- JSON operations
SELECT data->>'name' FROM users WHERE data @> '{"active": true}';

-- Generated columns
ALTER TABLE orders ADD COLUMN total_with_tax DECIMAL GENERATED ALWAYS AS (total * 1.1) STORED;
```
