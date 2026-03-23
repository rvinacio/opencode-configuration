# OpenCode Agent Models, Categories, and Replication Guide

## Purpose

This document explains the stack currently installed on this machine, with emphasis on:

- what was actually installed
- what was intentionally not kept
- what `oh-my-opencode` adds on top of OpenCode
- how agents and categories work
- which models are currently assigned to them
- what was validated at runtime
- what to reproduce on a work machine

---

## Current Installed Stack

### Global plugins loaded by OpenCode

From `~/.config/opencode/opencode.json`:

- `opencode-antigravity-auth@latest`
- `oh-my-opencode@latest`
- `opencode-scheduler`
- `octto`
- `opencode-websearch-cited@1.2.0`

### Separately installed workspace/profile tooling

- `ocx`
- `kdcokenny/opencode-workspace` profile `ws`

Important: the workspace profile is **not part of the normal `opencode` launch path**. It only runs when explicitly launched with:

```bash
ocx oc -p ws
```

If the user never launches that command, the workspace profile adds no day-to-day value in the normal OpenCode app flow.

---

## What Was Evaluated But Not Kept

### Removed later

- `backnotprop/plannotator`
- `supermemoryai/opencode-supermemory`

### Not kept as final installed choices

- `shekohex/opencode-google-antigravity-auth`
- `jenslys/opencode-gemini-auth`
- `code-yeongyu/oh-my-openagent` as a separate retained choice beyond the installed `oh-my-opencode` harness
- `zenobi-us/opencode-skillful`
- `spoons-and-mirrors/subtask2`
- `kdcokenny/opencode-background-agents`
- `different-ai/openwork`
- `Cluster444/agentic`
- `darrenhinde/OpenAgentsControl`
- `NeuralNomadsAI/CodeNomad`

Reason: overlap, redundancy, or lower fit versus the selected stack.

---

## Important Clarification: Why a Plannotator-Like Screen Appeared

The `plannotator` plugin is still removed from the OpenCode stack.

The browser screen that appeared during plan approval came from the environment's internal plan review workflow (`submit_plan`), not from the OpenCode plugin being reinstalled. In other words:

- `plannotator` plugin in the OpenCode stack: **removed**
- plan approval UI used by this assistant environment: **still exists independently**

So the plugin was not reintroduced into the user's OpenCode installation.

---

## What `oh-my-opencode` Is

`oh-my-opencode` is a harness/orchestration layer on top of base OpenCode.

It is not just "one more plugin". It changes how the system works by adding:

- agent roles
- category-based task routing
- opinionated orchestration behavior
- planning and delegation logic
- model assignment by role/task type
- hooks and workflow discipline

### Where it is installed

Runtime/plugin cache examples:

- `~/.cache/opencode/node_modules/oh-my-opencode/bin/oh-my-opencode.js`
- `~/.cache/opencode/node_modules/oh-my-opencode/dist/...`
- `~/.cache/opencode/node_modules/oh-my-opencode-darwin-arm64/bin/oh-my-opencode`

User configuration:

- `~/.config/opencode/oh-my-opencode.json`

### What it improves over plain OpenCode

Without OMO, OpenCode is a capable base tool with models, tools, and agent support.

With OMO, the system gains a stronger operating model:

- one main orchestrator for complex work
- specialist subagents for exploration, planning, review, and multimodal tasks
- predefined task classes (categories)
- better separation between planning, execution, and review
- clearer model specialization

Practical summary:

- OpenCode base = engine
- `oh-my-opencode` = workflow/orchestration layer on top of the engine

---

## User-Facing vs Internal Agents

Not every configured OMO agent appears as a top-level visible option in the app UI.

Some are visible/selectable. Others exist mainly for internal delegation.

### Agents defined in `~/.config/opencode/oh-my-opencode.json`

#### 1. `sisyphus`
- Purpose: main orchestrator
- Best for: general complex work, multi-step implementation, deciding what to do next
- Current model: `openai/gpt-5.4`
- Variant: `max`

#### 2. `hephaestus`
- Purpose: deep execution/implementation worker
- Best for: building and fixing code with more autonomy
- Current model: `openai/gpt-5.4`
- Variant: `medium`

#### 3. `oracle`
- Purpose: high-end consultant
- Best for: architecture, complex debugging, difficult tradeoffs, review
- Current model: `openai/gpt-5.4`
- Variant: `high`

#### 4. `explore`
- Purpose: lightweight explorer
- Best for: codebase discovery, finding patterns, locating relevant files/logic quickly
- Current model: `openai/gpt-5.4-mini`

#### 5. `multimodal-looker`
- Purpose: multimodal/visual analysis
- Best for: images, screenshots, PDFs, visual inputs
- Current model: `google/gemini-3.1-flash-image-preview`

#### 6. `prometheus`
- Purpose: planning agent
- Best for: plan-first flows, decomposing work before implementation
- Current model: `openai/gpt-5.4`
- Variant: `max`

#### 7. `metis`
- Purpose: pre-planning consultant
- Best for: scope clarification, ambiguity/risk detection before planning
- Current model: `openai/gpt-5.4`
- Variant: `max`

#### 8. `momus`
- Purpose: plan/quality reviewer
- Best for: checking plans for gaps, ambiguity, missing context, and rigor
- Current model: `google/antigravity-claude-opus-4-6-thinking`
- Variant: `max`

#### 9. `atlas`
- Purpose: practical secondary worker/coordinator
- Best for: lighter execution and structured task handling
- Current model: `openai/gpt-5.2`

#### 10. `sisyphus-junior`
- Purpose: lighter orchestrator/subtask worker
- Best for: smaller delegated tasks
- Current model: `openai/gpt-5.2`

### Why some agents do not appear in the main UI list

Configured agents can be used internally by the harness without being exposed as first-class manual options in the normal selector. That is why the user can see only a subset while the config file contains more.

---

## Categories in OMO

Categories are not the same thing as agents.

They are routing buckets that say:

> for this kind of task, use this model profile

### Current category configuration

#### `visual-engineering`
- Purpose: frontend, layout, styling, visual/UI work
- Current model: `google/gemini-3.1-pro-preview`
- Variant: `high`

#### `ultrabrain`
- Purpose: hardest logic and architecture work
- Current model: `opencode/gpt-5.4`
- Variant: `xhigh`

#### `deep`
- Purpose: deeper end-to-end work with research/execution
- Current model: `opencode/gpt-5.3-codex`
- Variant: `medium`

#### `artistry`
- Purpose: more creative/unconventional solution space
- Current model: `google/gemini-3.1-pro-preview`
- Variant: `high`

#### `quick`
- Purpose: trivial/small changes
- Current model: `openai/gpt-5.4-mini`

#### `unspecified-low`
- Purpose: uncategorized lighter work
- Current model: `openai/gpt-5.4-mini`

#### `unspecified-high`
- Purpose: uncategorized heavier work
- Current model: `openai/gpt-5.4-mini`

#### `writing`
- Purpose: docs and prose
- Current model: `google/antigravity-claude-sonnet-4-6`

### How categories are used

Categories generally come into play when the harness delegates work. They are a model-selection layer, not a UI menu the user normally clicks directly.

---

## Current Google Model Situation

The machine currently has both:

- Antigravity models
- standard/CLI-related Google Gemini models

This matters because earlier there was confusion caused by the UI not showing the full list clearly.

### Important reality check

`opencode models` showed that many Google models are available, including:

- `google/antigravity-gemini-3-flash`
- `google/antigravity-gemini-3-pro`
- `google/antigravity-gemini-3.1-pro`
- `google/antigravity-claude-sonnet-4-6`
- `google/antigravity-claude-opus-4-6-thinking`
- `google/gemini-2.5-flash`
- `google/gemini-2.5-pro`
- `google/gemini-3-flash-preview`
- `google/gemini-3-pro-preview`
- `google/gemini-3.1-pro-preview`
- `google/gemini-3.1-pro-preview-customtools`
- `google/gemini-3.1-flash-image-preview`
- and more

So the Google/Gemini family is available locally even when the UI makes the list look incomplete.

---

## Validation Performed After Model Remapping

After updating `~/.config/opencode/oh-my-opencode.json`, the config was validated as JSON-correct.

### Successful runtime checks

#### `openai/gpt-5.4`
Command result: `GPT54_OK`

#### `openai/gpt-5.2`
Command result: `GPT52_OK`

#### `google/antigravity-claude-sonnet-4-6`
Command result: `SONNET_OK`

#### `google/antigravity-claude-opus-4-6-thinking --variant=max`
Command result: `OPUS_OK`

#### `google/gemini-3.1-flash-image-preview`
Command ran successfully and returned a response, confirming the model is usable.

### One unresolved inconsistency: `GPT-5.4 mini`

The user interface shows `GPT-5.4 mini` as selectable.

However, direct CLI validation with:

- `openai/gpt-5.4-mini`
- `opencode/gpt-5.4-mini`

returned `Model not found` in this shell-driven test flow.

Current interpretation:

- the UI clearly exposes a `GPT-5.4 mini` option to the user
- but the direct CLI provider/model resolution from this environment did not accept the obvious fully-qualified IDs

This means the mapping is currently documented as the intended choice, but should still be validated once from the normal app UI in a real task using:

- `explore`
- `quick`
- `unspecified-low`
- `unspecified-high`

Until then, this specific mapping is **UI-visible but CLI-unconfirmed**.

---

## Should `opencode-workspace` Stay Installed?

### What it is

`opencode-workspace` is a bundled profile installed through `ocx`, not a normal always-on plugin.

### What it does well

- provides a separate curated multi-agent workspace
- includes its own agent set (`plan`, `build`, `coder`, `explore`, `researcher`, `scribe`, `reviewer`)
- adds MCP services and skills in a contained profile

### Limitation for this user's workflow

The user does not want to keep switching profiles and prefers the main OpenCode app flow.

Because of that, `opencode-workspace` currently behaves as optional parallel tooling rather than a core daily-use component.

### Recommendation status

It is reasonable to remove it later if the user confirms that they do not plan to use:

```bash
ocx oc -p ws
```

No removal was done in this phase.

---

## Work-Machine Replication Guidance

### Core idea

Replicate the documentation and installation steps in a way that lets the work machine try:

1. `NoeFabris/opencode-antigravity-auth` first
2. if blocked by corporate restrictions, test a Gemini-only fallback path such as `jenslys/opencode-gemini-auth`

### Why both matter

- home machine: Antigravity path exposed many useful Google and Claude models
- work machine: corporate Google setup may allow Gemini but block Antigravity-specific flows

### What should be documented for later GitHub publishing

- exact plugin list kept on the final personal machine
- exact config file paths touched
- exact OMO agent/category mapping chosen
- what was removed and why
- which runtime checks passed
- where a model was UI-visible but CLI-unconfirmed
- order of installation
- work-machine fallback strategy

---

## Files That Matter Most

### Main OpenCode config
- `~/.config/opencode/opencode.json`

### OMO config
- `~/.config/opencode/oh-my-opencode.json`

### Antigravity account storage
- `~/.config/opencode/antigravity-accounts.json`

### OpenCode auth store
- `~/.local/share/opencode/auth.json`

### Workspace profile (optional, separate)
- `~/.config/opencode/profiles/ws/opencode.jsonc`

---

## Final Status at This Point

- model remapping requested by the user: **applied**
- config syntax validation: **passed**
- runtime validation for most remapped models: **passed**
- `GPT-5.4 mini` direct CLI resolution: **still needs final UI-level confirmation**
- workspace removal: **not done yet**
- detailed local documentation: **this document created as the basis for later GitHub publication**
