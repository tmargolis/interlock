# Public Architecture Overview

This document describes what Interlock is, where it sits in the agentic stack, and what problems each of its eight layers addresses. It is intentionally high-level. Detailed architectural specifications are maintained in a private working repository and will be shared selectively as the project matures.

---

## Positioning: Where Interlock Sits

```
┌──────────────────────────────────────────┐
│          Applications & Consumers        │
└────────────────────┬─────────────────────┘
                     │
┌────────────────────▼─────────────────────┐
│                 Interlock                │
│           (Agent Control Plane)          │
└────────────────────┬─────────────────────┘
                     │
┌────────────────────▼─────────────────────┐
│  Agent Runtimes (LangGraph, CrewAI,      │
│  OpenAI Agents SDK, others)              │
│  MCP Servers (tools, data sources)       │
│  External / Third-party Agents (A2A)     │
└──────────────────────────────────────────┘
```

Interlock sits **above** individual agent runtimes and **below** business applications. It does not replace orchestration frameworks — they answer "what should happen next?" Interlock answers: **is what's happening safe, authorized, correct, and attributable?**

---

## What Interlock Is Not

Understanding the boundaries is as important as understanding what the system does:

- **Not an orchestration framework.** LangGraph, CrewAI, OpenAI Agents SDK, and others handle task decomposition and agent-to-agent orchestration within a defined runtime. Interlock operates above that layer.
- **Not an LLM provider abstraction.** Interlock does not choose or route between models. It governs agents that already have models.
- **Not an agent runtime.** Interlock does not execute agents. It governs fleets of agents that execute elsewhere.
- **Not a business-logic layer.** Domain-specific policies, workflows, and application logic live above Interlock, not inside it.
- **Not platform-owned governance.** Interlock is explicitly designed to be vendor-neutral — its governance layer is not owned by any LLM provider, cloud platform, or orchestration vendor.

---

## The Eight Layers

Each layer addresses one governance responsibility for a fleet of agents. They are best understood as a set of contracts — not necessarily eight separate services. Read sequentially, they move from "what exists in the fleet" to "what did the fleet do, and was it good."

### 1. Registry
**Question it answers:** What agents exist in this fleet, and what are they capable of?

The registry is the catalog layer. It maintains an inventory of available agents — who they act on behalf of, what tasks they accept, what capabilities they declare, and whether they are currently available and healthy. Discovery is the precondition for everything else: you cannot authorize, route, or observe an agent you don't know exists.

The registry is designed to be interoperable with emerging agent discovery standards, not a proprietary catalog.

---

### 2. Identity
**Question it answers:** Who is each agent acting on behalf of, and can the claim be verified?

Agents act on behalf of humans and organizations. The identity layer manages the authorization chain from principal to agent: who delegated what authority, to which agent, under what constraints, and for how long. This includes cross-boundary interactions — when your agent talks to someone else's agent, the identity layer ensures both sides can establish and verify the delegation context.

Critically, identity is not just machine-verifiable. The delegation chain should be legible to the human principal — not just auditable by an engineer reading a log.

---

### 3. Authorization
**Question it answers:** Is this specific action, by this agent, on behalf of this principal, allowed right now?

Authorization is the enforcement layer. It evaluates proposed agent actions against the principal's declared policy before those actions execute. This is not authentication (identity established that) — it is least-privilege enforcement at the action level, with approval gates for actions that exceed delegated scope.

The authorization layer also enforces interaction ownership boundaries: categories of interaction that agents should not handle autonomously regardless of what the principal has delegated.

---

### 4. Orchestration
**Question it answers:** What should happen next, and on which runtime?

The orchestration layer manages task routing and handoff across heterogeneous runtimes. It handles the state machine of a long-running task: which agent picks up which piece, how context is passed between runtimes, how human-in-the-loop gates are managed, and how the task recovers from partial failure.

The orchestration layer is runtime-aware but not runtime-locked. It can route to LangGraph, CrewAI, OpenAI Agents SDK, or future runtimes without requiring them to share a common execution model.

---

### 5. Durable Execution
**Question it answers:** How does a long-running task survive failures, restarts, and approval delays that span hours or days?

Many real-world agent tasks are not sub-second operations. A procurement agent may need to gather quotes, wait for a human to approve a budget, continue the negotiation, and close the loop — over the course of days. The durable execution layer ensures these tasks are persistent: they survive agent crashes, runtime restarts, and indefinite approval windows without losing their state or requiring the principal to restart from scratch.

---

### 6. Connectivity (Harness Boundary)
**Question it answers:** How do agents reach tools and data sources safely, with the right scope, provenance, and content checks?

The connectivity layer manages agent access to the outside world. It enforces that tool calls are made within authorized scope, that data retrieved from external sources carries provenance metadata, and that content flowing into agent context is checked for safety and injection risk. It is the harness boundary — the surface where agents interact with tools, APIs, databases, and external services, and where those interactions are governed.

MCP (Model Context Protocol) is the primary standard for this surface.

---

### 7. Observability
**Question it answers:** What actually happened across this fleet, and can we replay it?

The observability layer produces a unified telemetry stream across all agents in the fleet — regardless of which runtime they ran on or which vendor they were built by. It maintains an audit trail that is complete enough to reconstruct what happened in any task, who authorized it, and what the outcome was.

Observability in Interlock is not just for debugging. It is the foundation of accountability — the evidence layer that makes the rest of the governance surface meaningful.

The observability layer uses OpenTelemetry's GenAI semantic conventions as its telemetry substrate.

---

### 8. Evaluation & Outcomes
**Question it answers:** Was the output actually correct, and are the deployed agents producing the impact the principal intended?

This is the closing feedback loop. Individual task evaluation answers whether a specific output was correct. Outcome assurance answers a harder question: over time, across many tasks, is this fleet of agents actually doing what the principal set it up to do?

Both are necessary. An agent that passes individual task checks but drifts from its intended purpose over time is a governance failure, not just an evaluation failure.

---

## Protocol Foundations

Interlock is designed to be protocol-first. The governance layer should be built on open standards that any agent runtime can speak, rather than on proprietary interfaces that create platform dependency.

| Standard | Role in Interlock |
|---|---|
| A2A (Agent-to-Agent) | Agent discovery, task lifecycle, cross-boundary agent communication |
| MCP (Model Context Protocol) | Tool and data source connectivity at the harness boundary |
| OpenTelemetry (GenAI conventions) | Unified telemetry and audit trail across vendor boundaries |

These standards are live, actively governed, and gaining multi-vendor adoption. Interlock follows them rather than forking them.

---

## Cross-Cutting Concerns

Several governance problems cut across all eight layers rather than belonging to any one of them:

**Governance and compliance posture.** How does a principal or enterprise operator understand their compliance posture across the fleet? Interlock is designed to aggregate compliance signals from underlying components — model providers, orchestration frameworks, tool providers — rather than attempting to implement development-tier compliance controls that belong to those components.

**Personal and enterprise identity.** A control plane that only works for enterprises misses most of the surface area. Interlock is designed to span individual principals, family accounts, teams, and multi-organization deployments with a consistent identity and delegation model.

**Operational health and budget awareness.** Running a fleet of agents involves real costs — compute, API calls, tool invocations. The control plane should make cost and operational health visible to the principal, not just to a devops team.

**Multi-vendor agent coordination.** Cross-organization agent interaction — your agents working with a supplier's agents, a client's agents, a colleague's agents — is the hardest coordination surface. Interlock addresses this by treating the trust boundary as a first-class design concern rather than an edge case.

---

## A Note on This Document

This document describes Interlock's architecture at the layer and principle level. It intentionally omits schema and data model specifications, implementation choices within each layer, deployment architecture, evaluation mechanics, and specific technology selections within layers.

Those details are in a private working repository. If you are working on related problems and want to go deeper, reach out: [tmargo@gmail.com](mailto:tmargo@gmail.com)
