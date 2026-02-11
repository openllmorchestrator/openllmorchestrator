# Open LLM Orchestrator
## MVP Architecture Specification

---

# 1. Core Principle

Open LLM Orchestrator is a Temporal-native AI orchestration platform.

All execution — retrieval, inference, routing — runs as durable Temporal workflows.

There is no alternate execution engine.

Temporal is the control plane.

---

# 2. MVP Scope

Phase 1 includes:

- Temporal-native execution
- gRPC-based OSS model containers
- Built-in RAG pipeline
- Automatic best-model selection
- Priority & concurrency management
- Docker and Kubernetes deployment

Excluded from MVP:

- Multi-model arbitration
- Parallel inference racing
- Cost-aware routing
- MCP tool execution
- SLA policy engine

---

# 3. System Architecture

## 3.1 Control Plane

Responsible for orchestration and decision-making.

Components:

- Temporal Server
- Workflow Definitions
- Activity Workers
- Model Registry
- Model Router
- Retrieval Service

---

## 3.2 Data Plane

Responsible for execution.

Components:

- OSS LLM Containers (gRPC + Temporal Worker)
- Embedding Container
- Vector Store
- Optional Redis Cache
- Persistent DB (Postgres / CockroachDB / SQLite)

All model containers expose gRPC interfaces.

---

# 4. Execution Flow

1. Client submits request
2. API Gateway starts Temporal Workflow
3. Workflow performs Retrieval (RAG)
4. Workflow selects model
5. Workflow dispatches model execution activity
6. Model returns response
7. Workflow completes
8. Response returned to client

Every step is durable.

---

# 5. Model Container Architecture

Each model container:

- Runs a Temporal Worker
- Polls specific Task Queue
- Implements gRPC contract
- Registers metadata on startup

---

## 5.1 Model Registration Metadata

Each model advertises:

model_id  
model_name  
model_type (llm / embedding)  
priority  
default_flag  
max_concurrency  
context_limit  
hardware_type (cpu / gpu)  

---

# 6. Task Queue Design

Temporal Task Queues:

llm  
embedding  

Each container polls only relevant queue.

Queue isolation enables:

- Horizontal scaling
- Load isolation
- Specialized hardware routing

---

# 7. Model Selection Strategy (MVP)

Workflow selects model in this order:

1. User-specified model (if healthy)
2. Default model
3. Highest priority healthy model
4. First available healthy model

No parallel execution in MVP.

---

# 8. Concurrency Management

Each model defines:

max_concurrency

Temporal worker enforces:

- Maximum concurrent activities
- Backpressure via queue

If model is at capacity:

- Router selects next available model

---

# 9. RAG Pipeline (MVP)

Steps:

1. Embed query
2. Retrieve top-k documents
3. Construct augmented prompt
4. Send to selected LLM

Embedding model runs as container.

Vector store options:

- In-memory (demo)
- SQLite
- External DB (future)

---

# 10. Configuration Modes

## 10.1 Production (Kubernetes)

- Helm-based deployment
- External Temporal
- External database
- Horizontal scaling of model containers

---

## 10.2 Docker Development

- Docker Compose
- Local Temporal server
- Vector store container
- Example LLM container

---

## 10.3 Local Demo Mode

- SQLite persistence
- In-memory vector store
- Local Temporal

Single-command startup.

---

# 11. Failure Handling

- Temporal retry policies
- Model health monitoring
- Automatic fallback to next model
- Clean workflow failure if no model available

No request is lost.

---

# 12. Observability (MVP)

Expose:

- Workflow duration
- Model execution latency
- Queue depth
- Error rate

Integrations planned:

- Prometheus
- OpenTelemetry
- Grafana

---

# 13. Final Definition

Open LLM Orchestrator v1 is:

A Temporal-native, pluggable OSS model orchestration platform with built-in RAG and automatic best-model selection.

It is:

Durable.
Scalable.
Model-agnostic.
Extensible.
Production-ready.
