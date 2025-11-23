# Product Development Context for AI Planner

## Objective
Provide an AI planning agent with concise, actionable context to design and sequence delivery of a cross-LLM memory & workflow platform built on Cognee. The agent should propose phased milestones, surface risks/dependencies, and align plans with enterprise-grade security and monetization requirements.

## Product Vision (target platform)
- **Cross-LLM memory** that ingests conversations from Claude, ChatGPT, and Gemini; centralizes summaries, embeddings, and graph relationships; and serves context back to clients.
- **Thin-client model**: Browser extension only captures/displays data; all LLM calls, summarization, routing, and feature gating live server-side.
- **Multi-project & team**: Users manage multiple projects/workspaces, with role-based access (viewer/editor/admin) and team contexts.
- **Enterprise readiness**: RBAC, audit logs, SSO/SAML path, data residency options, and server-side IP protection for prompts and workflows.
- **Monetization**: Subscription tiers (Free/Pro/Power/Team/Enterprise) enforced server-side with feature flags, Stripe validation, and usage limits.

## Current Cognee Strengths to Leverage
- **Ingestion pipeline**: `add` supports raw text/files/URLs and can be the server endpoint that receives extension payloads.【F:cognee/api/v1/add/add.py†L18-L200】
- **Graph-aware search**: Existing search modes (graph, RAG, lexical, summaries, code, Cypher) with dataset scoping can power cross-project retrieval UIs.【F:cognee/api/v1/search/search.py†L18-L200】
- **Provider abstraction**: Central LLM configuration allows swapping providers/models, a starting point for server-side model routing and cost policies.【F:cognee/infrastructure/llm/config.py†L13-L200】
- **User/tenant models**: SQLAlchemy models already store tenant-linked users and roles to extend into project/workspace RBAC.【F:cognee/modules/users/models/User.py†L13-L53】

## Key Gaps for the Planner to Address
- **Capture layer**: Need extensions or APIs for live chat capture; ingestion assumes prepared text/files.
- **Model routing & policy**: Absent cost-aware provider selection, fallbacks, and per-feature model mapping.
- **Automation**: No scheduled digests, related-conversation alerts, or evolution tracking.
- **Billing & feature gating**: Lacks Stripe flows, subscription tiers, usage metering, and kill-switch feature flags.
- **Product UI**: No dashboards for projects, search, notifications, or admin controls (Next.js app is a scaffold).【F:cognee-frontend/README.md†L1-L36】

## Constraints & Principles
- All business logic, prompts, and feature gating remain server-side; extensions stay dumb/thin.
- Avoid client-side secrets; use short-lived tokens and server-validated sessions.
- Favor incremental delivery: align milestones with Phase 1 (MVP individual), Phase 2 (multi-project), Phase 3 (team/enterprise).
- Uphold data protection: encrypted at rest/in transit, auditability, rate limiting tied to subscription tier.

## Required Planning Outputs
When invoked, the AI agent should produce:
1. **Milestone plan**: 3–5 sequential milestones with objectives, exit criteria, and user-facing deliverables.
2. **Workstreams**: Backend, frontend, infra/DevOps, and data/ML tracks per milestone with effort estimates and dependencies.
3. **Risk register**: Top risks (tech, security, compliance, adoption) with mitigations and owners.
4. **Decision log**: Explicit choices on LLM routing, embedding models, storage, auth/billing approach, and observability.
5. **Success metrics**: Activation/engagement and reliability targets per phase (e.g., ingestion success rate, search latency, daily active projects).

## Inputs the Agent Can Assume
- Cognee backend codebase with ingestion, search, and LLM config primitives.
- Next.js scaffold (`cognee-frontend/`) ready for new authenticated pages and dashboards.
- Ability to introduce Redis/Celery or equivalent queues for async jobs and pgvector for embeddings (per product brief).

## Interaction Guidelines for the Agent
- Keep plans implementation-focused (APIs, data models, UI pages, queues) rather than high-level marketing.
- Call out data flow for extension capture → server ingestion → summarization/embedding → search/delivery.
- Prefer reusable platform primitives (feature flags, job scheduler, notification service) over bespoke one-off features.
- Highlight where new FastAPI endpoints, database tables, or Next.js routes are needed.
- Explicitly map features to subscription tiers and roles to enforce security and monetization.
