# Open LLM Orchestrator

Open LLM Orchestrator is a Temporal-native AI orchestration platform designed for pluggable OSS model execution with built-in RAG support.

It enables durable workflow-driven inference, automatic model discovery, priority-based selection, and Kubernetes-ready scalability — built for backend engineers designing reliable AI infrastructure.

---

## 🚀 What Problem It Solves

Most AI frameworks focus on prompts and pipelines.

Open LLM Orchestrator focuses on infrastructure:

- Temporal-based durable execution
- Pluggable OSS model containers
- Built-in Retrieval Augmented Generation (RAG)
- Automatic best-model selection
- Priority & concurrency-aware routing
- Production-ready orchestration

This is not a wrapper around models — it is a control plane for AI systems.

---

## 🏗 MVP Scope

Phase 1 focuses on:

- Temporal-native execution
- gRPC-based OSS model containers
- Built-in RAG pipeline
- Automatic best-available model selection
- Priority & concurrency handling
- Docker + Kubernetes deployment modes

No SaaS providers in v1. Only open-source models.

---

## 🔄 Execution Flow

Client Request
↓
Start Temporal Workflow
↓
Retrieve Context (RAG)
↓
Select Model
↓
Execute Model Activity
↓
Return Response



All steps are durable via Temporal.

---

## 🤖 Model Selection Logic (MVP)

If the user:

- Specifies a model → that model is used (if healthy)
- Does not specify → system selects the best available active model

Selection priority:

1. User-selected model
2. Default model
3. Highest priority healthy model
4. First available healthy model

No parallel arbitration in MVP (planned for future phase).

---

## 📦 Architecture Overview

Open LLM Orchestrator follows strict separation:

### Control Plane
- Temporal Workflows
- Model Registry
- Model Router
- Retrieval Service

### Data Plane
- OSS LLM Containers (gRPC + Temporal Workers)
- Embedding Containers
- Vector Store

Each model container:
- Runs a Temporal Worker
- Polls dedicated Task Queues
- Registers capabilities on startup

---

## ⚙ Deployment Modes

### Local Demo
- SQLite
- In-memory vector store
- Local Temporal server

### Docker Development
- Docker Compose
- Temporal + Vector DB + Model containers

### Production
- Helm-based Kubernetes deployment
- External DB
- Horizontal model scaling

---

## 📜 License

Apache License 2.0

---

## 🌍 Vision

Open LLM Orchestrator aims to become the durable execution control plane for pluggable, scalable, OSS-based AI systems.

Simple.
Durable.
Extensible.
Production-ready.


