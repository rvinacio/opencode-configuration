---
name: ti-team-routing
description: Canonical TI-team routing contract for OpenCode and oh-my-opencode, defining top-level authority, family registry, handoff artifacts, and shared validation modes
compatibility: opencode
---

# TI Team Routing Contract

This skill is the canonical routing contract for the complete TI-team architecture.

Its job is to define:

- the single top-level routing authority
- the MVP family registry
- when each family should be used
- which artifact each family must hand off
- how quick and full validation work
- how ambiguous and cross-family work should degrade safely

## Core Authority Rule

There is exactly **one top-level router**.

- The top-level router classifies intent, domain, complexity, and ambiguity.
- Family routers may **refine** the route inside their own family.
- Family routers may **not override** the top-level decision and redirect the request into a different family without an explicit escalation reason.

This prevents routing drift between AGENTS rules, OMO prompt rules, and family-specific personas.

## Routing Priorities

1. Preserve protected subsystem rules first
2. Identify whether the task is simple local execution or initiative-level orchestration
3. Route to one primary family
4. Escalate to cross-family orchestration only when needed
5. Require explicit handoff artifacts before moving across families

## Protected Subsystem

### Frontend Product Studio

Frontend product work is a protected subsystem and must retain its existing chain.

For **website, landing page, web app, or mobile app frontend** work involving product framing, UX, visual direction, implementation architecture, or experience QA, route through:

1. `frontend-business-strategist`
2. `frontend-ux-architect`
3. `frontend-visual-art-director`
4. `frontend-implementation-architect`
5. `frontend-experience-qa`

Do not collapse this into a generic frontend pass.

## Family Registry

### 1. Business / Product

**Use when**:
- the request asks what should be built
- scope, value, sequencing, or tradeoffs are unclear
- the work needs roadmap or PRD-level framing

**Typical triggers**:
- strategy, roadmap, MVP, prioritization, business case, user story, scope

**Primary artifact**: `Business Brief`

**Minimum fields**:
- objective
- target user or stakeholder
- success event
- constraints
- scope boundaries
- strategic risks

### 2. Research / Discovery

**Use when**:
- the work needs user, competitor, market, or evidence gathering
- the request is vague and the problem must be framed before design/build

**Typical triggers**:
- research, benchmark, compare, explore, discover, validate assumptions

**Primary artifact**: `Research Synthesis`

**Minimum fields**:
- research question
- sources or evidence basis
- key findings
- open questions
- implications for product or implementation

### 3. Frontend

**Use when**:
- the request concerns interface behavior, UX, layout, visuals, interaction design, or frontend implementation

**Typical triggers**:
- page, screen, component, UX, UI, mobile app frontend, web app frontend, landing page

**Primary artifact**: `Experience Blueprint`

**Minimum fields**:
- core journey
- screen or section structure
- state coverage
- interaction priorities
- frontend implementation implications

**Protected rule**:
- must route through the existing frontend studio chain when work is product/frontend in nature

### 4. Backend / Platform

**Use when**:
- the work concerns APIs, services, auth, runtime architecture, deployment, observability, infrastructure boundaries, or service reliability

**Typical triggers**:
- API, route, backend, auth, service, deploy, runtime, CI/CD, infra, observability

**Primary artifact**: `Architecture Decision`

**Minimum fields**:
- problem statement
- chosen approach
- rejected alternatives
- interfaces or boundaries
- operational risks

### 5. Data

**Use when**:
- the task is about metrics, semantic layers, marts, pipelines, data contracts, SLAs, data quality, or analytical modeling

**Typical triggers**:
- KPI, semantic layer, mart, dbt, pipeline, SLA, lineage, data quality, metric definition

**Primary artifact**: `Data Contract`

**Minimum fields**:
- entities or datasets involved
- source and destination expectations
- quality expectations
- freshness or SLA
- ownership and downstream impact

**Separation rule**:
- data does not collapse into backend or database-only work

### 6. AI

**Use when**:
- the task involves LLM features, RAG, prompt architecture, evals, fallback logic, tool use, agent behavior, or model-release guardrails

**Typical triggers**:
- LLM, RAG, prompt, eval, fallback, retrieval, hallucination, agent tool, model behavior

**Primary artifact**: `Eval Report`

**Minimum fields**:
- target behavior
- evaluation method
- failure modes
- fallback strategy
- release recommendation

**Separation rule**:
- AI does not collapse into research or backend-with-API-calls

### 7. Governance / Validation

**Use when**:
- the task needs quality gates, release decisions, privacy/compliance review, policy checks, or cross-family validation discipline

**Typical triggers**:
- release review, governance, policy, compliance, privacy, validation, quality gate, approval

**Primary artifact**: `Release Verdict`

**Minimum fields**:
- scope reviewed
- checks performed
- pass/fail findings
- blockers
- go/no-go decision

### 8. Game

**Use when**:
- the request is genuinely about game design, game systems, runtime behavior, economy, telemetry, live ops, or gameplay QA

**Typical triggers**:
- game loop, combat, economy, level, progression, live ops, telemetry, multiplayer, balance

**Primary artifact**: `Game Loop Spec`

**Minimum fields**:
- player goal
- core loop
- systems involved
- telemetry or balancing needs
- technical/runtime risks

**Separation rule**:
- game is a real vertical, not frontend plus backend plus animation

## Simple vs Initiative-Level Routing

### Simple Local Execution

Use the lightest valid path when all are true:

- one domain
- clear request
- low ambiguity
- limited blast radius

Examples:
- fix a specific API bug
- adjust one backend query
- add one analytics transform

### Initiative-Level Orchestration

Escalate when any are true:

- multiple families are implicated
- business intent is unclear
- architecture tradeoffs matter
- the request mixes strategy and implementation
- the output affects multiple surfaces or teams

Default macro-flow for initiative work:

1. business/product
2. research/discovery
3. architecture or domain families
4. governance/validation

## Handoff Protocol

Every family handoff must include:

- original request
- routing reason
- artifact name
- artifact contents
- unresolved questions
- required validation mode

No family may hand off only “context” or a vague summary.

## Shared Validation Model

### Quick Validation

Use for simple, local, or intermediate work.

Quick validation must answer:

- is the route still correct?
- is the artifact complete enough for the next family?
- did the immediate checks pass?

Typical quick checks:
- config parse
- lint/type/test subset
- artifact completeness review
- family-specific sanity check

### Full Validation

Use before completion of initiative-level work, release-facing work, or cross-family deliverables.

Full validation must answer:

- is the route still defensible after implementation?
- do all required artifacts exist and agree?
- did shared checks and family-specific checks pass?
- is there a clear release or completion verdict?

Typical full checks:
- all quick checks
- cross-family consistency
- broader test/build/runtime validation
- governance or QA sign-off where required

### Family Extensions

- business/product extends validation with scope and success-metric coherence
- research extends validation with evidence quality and inference quality
- frontend extends validation with UX/state/accessibility/visual fidelity checks
- backend/platform extends validation with interface, reliability, and operational checks
- data extends validation with quality, freshness, trust, and downstream impact checks
- AI extends validation with eval quality, fallback quality, and risk checks
- governance extends validation with release/blocker clarity
- game extends validation with loop quality, balancing, telemetry, and runtime behavior checks

## Ambiguity and Escalation Rules

- If the request is vague but strategic, start in business/product or research/discovery.
- If the request spans 2+ families with architectural consequences, escalate to orchestration.
- If the request is trivial and local, do not force the full family chain.
- If routing conflict appears, preserve the top-level route and record the escalation reason explicitly.

## Routing Fixtures

| Prompt | Expected Family | Required Artifact | Validation Mode |
|---|---|---|---|
| "Define the MVP and roadmap for an internal analytics platform" | business/product | Business Brief | full |
| "Research how nonprofits benchmark donation retention dashboards" | research/discovery | Research Synthesis | quick |
| "Design the frontend UX for a B2B onboarding web app" | frontend via frontend studio | Experience Blueprint | full |
| "Add auth service boundaries and deployment considerations for a multi-tenant API" | backend/platform | Architecture Decision | full |
| "Create a trustworthy KPI semantic layer for weekly impact metrics" | data | Data Contract | full |
| "Design RAG fallback behavior and prompt eval criteria" | AI | Eval Report | full |
| "Review whether this release is blocked by privacy and validation concerns" | governance/validation | Release Verdict | full |
| "Design the core game loop and telemetry for a progression-based mobile game" | game | Game Loop Spec | full |
| "Fix a single broken SQL transform in the pipeline" | data | Data Contract | quick |
| "The login flow is broken and the web app UX also needs redesign" | orchestration: frontend + backend/platform | Architecture Decision + Experience Blueprint | full |

## Maintenance Rule

When AGENTS rules, OMO prompt rules, or family definitions need routing changes, update this contract first. Other layers should reference this contract, not invent independent routing philosophies.
