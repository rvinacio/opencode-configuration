# OpenCode Repository Inventory and Installation Sources

## Purpose

This document records the repositories evaluated during this setup, which ones were actually used, which ones were removed, and how the selected stack was installed. The goal is to make future setup on another machine reproducible without relying on memory.

---

## Final Repositories Used in Practice

### 1. `NoeFabris/opencode-antigravity-auth`

- Status: **installed and kept**
- Purpose: Google OAuth / Antigravity auth plugin exposing Google Gemini and Antigravity Claude model access inside OpenCode
- SSH: `git@github.com:NoeFabris/opencode-antigravity-auth.git`
- HTTPS: `https://github.com/NoeFabris/opencode-antigravity-auth.git`
- Installed via OpenCode plugin registry/config, not by manual git clone into the project

How it was integrated:

- added to `~/.config/opencode/opencode.json` as:

```json
"opencode-antigravity-auth@latest"
```

- Google model definitions were added under `provider.google.models`
- `~/.config/opencode/oh-my-opencode.json` was updated to set `google_auth: false` to avoid auth conflicts

Key local files affected:

- `~/.config/opencode/opencode.json`
- `~/.config/opencode/oh-my-opencode.json`
- `~/.config/opencode/antigravity-accounts.json`
- `~/.local/share/opencode/auth.json`

Validation performed:

- Antigravity models appeared in `opencode models`
- successful runtime calls with:
  - `google/antigravity-gemini-3-flash`
  - `google/antigravity-claude-sonnet-4-6`
  - `google/antigravity-claude-opus-4-6-thinking`

---

### 2. `ghoulr/opencode-websearch-cited`

- Status: **installed and kept**
- Purpose: cited web search with inline references and explicit sources
- SSH: `git@github.com:ghoulr/opencode-websearch-cited.git`
- HTTPS: `https://github.com/ghoulr/opencode-websearch-cited.git`

How it was integrated:

- added to `~/.config/opencode/opencode.json` as:

```json
"opencode-websearch-cited@1.2.0"
```

- kept **last** in the plugin list
- configured to prefer OpenAI backend for cited search because Google-backed cited search failed in this environment due to API enablement restrictions

Key local files affected:

- `~/.config/opencode/opencode.json`

Validation performed:

- plugin loaded in debug logs
- cited search completed successfully end-to-end

---

### 3. `code-yeongyu/oh-my-openagent` / `oh-my-opencode`

- Status: **installed and kept**
- Purpose: orchestration harness with agents, categories, routing, hooks, and operational workflow on top of base OpenCode
- SSH: `git@github.com:code-yeongyu/oh-my-openagent.git`
- HTTPS: `https://github.com/code-yeongyu/oh-my-openagent.git`
- Important note: naming in docs/history may alternate between `oh-my-opencode` and `oh-my-openagent`

How it was installed:

```bash
npx oh-my-opencode@3.12.3 install --no-tui --claude=no --openai=no --gemini=yes --copilot=no --opencode-zen=yes --zai-coding-plan=no --opencode-go=no
```

Key local files affected:

- `~/.config/opencode/opencode.json`
- `~/.config/opencode/oh-my-opencode.json`

Key local runtime/cache locations:

- `~/.cache/opencode/node_modules/oh-my-opencode/...`

Validation performed:

- plugin registered in OpenCode config
- config file generated successfully
- models/agents loaded in runtime
- later customized manually for preferred model mapping

---

### 4. `different-ai/opencode-scheduler`

- Status: **installed and kept**
- Purpose: recurring scheduled jobs via native OS scheduling backends
- SSH: `git@github.com:different-ai/opencode-scheduler.git`
- HTTPS: `https://github.com/different-ai/opencode-scheduler.git`

How it was integrated:

- added to `~/.config/opencode/opencode.json` as:

```json
"opencode-scheduler"
```

Validation performed:

- created a smoke-test job
- listed the job
- deleted the job
- verified cleanup

---

### 5. `vtemian/octto`

- Status: **installed and kept**
- Purpose: interactive browser-based questioning and brainstorming UI
- SSH: `git@github.com:vtemian/octto.git`
- HTTPS: `https://github.com/vtemian/octto.git`

How it was integrated:

- added to `~/.config/opencode/opencode.json` as:

```json
"octto"
```

Validation performed:

- tool registration verified in OpenCode runtime
- browser-first interactive UX left as optional later hands-on use

---

### 6. `kdcokenny/opencode-workspace`

- Status: **installed, but optional / not part of normal OpenCode launch**
- Purpose: separate curated workspace profile with its own agents, plugins, skills, and MCPs
- SSH: `git@github.com:kdcokenny/opencode-workspace.git`
- HTTPS: `https://github.com/kdcokenny/opencode-workspace.git`

How it was installed:

```bash
ocx init --global
ocx profile add ws --source tweak/p-1vp4xoqv --from https://tweakoc.com/r --global
```

How it is launched:

```bash
ocx oc -p ws
```

Important note:

- this profile is not active in the normal `opencode` app flow
- it only matters if the user explicitly launches the `ws` profile

Validation performed:

- `ocx --version`
- `ocx profile list --global`
- `ocx oc -p ws -- --version`
- `ocx oc -p ws -- agent list`

---

## Supporting Tool Used During Setup

### `ocx`

- Status: **installed and used**
- Purpose: profile/package manager used to install the `ws` workspace profile
- Main upstream repository: `kdcokenny/ocx`
- Installed path after remediation: `~/.opencode/bin/ocx`

Important setup note:

- initial npm install path failed because the npm package expected a global `bun` binary
- remediation was to install the standalone `ocx` binary using the official install script instead of the npm route

---

## Repositories Evaluated But Not Kept in the Final Stack

### Removed after evaluation

#### `backnotprop/plannotator`
- SSH: `git@github.com:backnotprop/plannotator.git`
- HTTPS: `https://github.com/backnotprop/plannotator.git`
- Decision: removed
- Why: native document/plan review inside the current workflow was enough; external plan review UI was not needed

#### `supermemoryai/opencode-supermemory`
- SSH: `git@github.com:supermemoryai/opencode-supermemory.git`
- HTTPS: `https://github.com/supermemoryai/opencode-supermemory.git`
- Decision: removed
- Why: user did not want external memory/API-key overhead

### Evaluated but not selected as final installed choices

#### `shekohex/opencode-google-antigravity-auth`
- SSH: `git@github.com:shekohex/opencode-google-antigravity-auth.git`
- HTTPS: `https://github.com/shekohex/opencode-google-antigravity-auth.git`
- Decision: not selected
- Why: overlapped with the chosen `NoeFabris/opencode-antigravity-auth`

#### `jenslys/opencode-gemini-auth`
- SSH: `git@github.com:jenslys/opencode-gemini-auth.git`
- HTTPS: `https://github.com/jenslys/opencode-gemini-auth.git`
- Decision: not selected as the primary home-machine auth path
- Why: useful as a fallback Gemini-only path, especially for corporate machines, but less complete than the chosen Antigravity plugin for the personal machine

#### `zenobi-us/opencode-skillful`
- SSH: `git@github.com:zenobi-us/opencode-skillful.git`
- HTTPS: `https://github.com/zenobi-us/opencode-skillful.git`
- Decision: not selected
- Why: overlapping capability versus the chosen harness stack

#### `spoons-and-mirrors/subtask2`
- SSH: `git@github.com:spoons-and-mirrors/subtask2.git`
- HTTPS: `https://github.com/spoons-and-mirrors/subtask2.git`
- Decision: not selected
- Why: overlapping decomposition/delegation role with the chosen orchestration stack

#### `kdcokenny/opencode-background-agents`
- SSH: `git@github.com:kdcokenny/opencode-background-agents.git`
- HTTPS: `https://github.com/kdcokenny/opencode-background-agents.git`
- Decision: not selected separately
- Why: async/background delegation was already covered by the selected harness/profile stack

#### `different-ai/openwork`
- SSH: `git@github.com:different-ai/openwork.git`
- HTTPS: `https://github.com/different-ai/openwork.git`
- Decision: not selected
- Why: more like a higher-level cowork/product UI than a necessary core local plugin for this personal setup

#### `Cluster444/agentic`
- SSH: `git@github.com:Cluster444/agentic.git`
- HTTPS: `https://github.com/Cluster444/agentic.git`
- Decision: not selected
- Why: overlap with workflow/orchestration goals already covered elsewhere

#### `darrenhinde/OpenAgentsControl`
- SSH: `git@github.com:darrenhinde/OpenAgentsControl.git`
- HTTPS: `https://github.com/darrenhinde/OpenAgentsControl.git`
- Decision: not selected
- Why: overlap with plan-first and approval-oriented orchestration already covered by the selected approach

#### `NeuralNomadsAI/CodeNomad`
- SSH: `git@github.com:NeuralNomadsAI/CodeNomad.git`
- HTTPS: `https://github.com/NeuralNomadsAI/CodeNomad.git`
- Decision: not selected
- Why: another command-center/productivity layer not needed in the final chosen stack

---

## Exact Order Used for the Real Setup

### Wave 0 - Baseline and backup

- capture current OpenCode state
- back up `~/.config/opencode`
- inspect existing versions and config files

### Wave 1 - Core workspace + OMO

1. install `ocx`
2. initialize `ocx`
3. install workspace profile `ws`
4. install `oh-my-opencode`
5. verify OpenCode still starts and loads plugins

### Wave 2 - Google auth

1. add `opencode-antigravity-auth`
2. add Google provider model configuration
3. disable OMO built-in `google_auth`
4. authenticate Google/Antigravity
5. verify real model execution

### Wave 3 - Scheduler

1. add `opencode-scheduler`
2. create/list/delete a smoke-test job

### Wave 4 - Cited search

1. add `opencode-websearch-cited` **last**
2. test cited search
3. switch cited-search backend preference to OpenAI after Google-backed cited search failed in this environment

### Wave 5 - Interactive UX

1. add `octto`
2. verify tool registration

### After evaluation

1. remove `plannotator`
2. remove `supermemory`
3. later customize OMO agent/category model mapping

---

## Manual Reinstallation Guidance for Another Computer

### Recommended order

1. update/verify base OpenCode first
2. install `ocx` only if the separate `ws` profile is actually desired
3. install `oh-my-opencode`
4. install `NoeFabris/opencode-antigravity-auth`
5. authenticate and verify Google/Antigravity access
6. install `opencode-scheduler`
7. install `octto`
8. install `opencode-websearch-cited` last
9. apply preferred OMO model remapping

### Corporate machine recommendation

Try this order:

1. `NoeFabris/opencode-antigravity-auth`
2. if Antigravity is blocked by company policy, test `jenslys/opencode-gemini-auth` as the fallback path for Gemini-only access

### Files to preserve or reproduce

- `~/.config/opencode/opencode.json`
- `~/.config/opencode/oh-my-opencode.json`
- `~/.config/opencode/antigravity-accounts.json` (if valid and portable)
- `~/.local/share/opencode/auth.json` (with caution; reauth may still be required)

---

## Main Local Documentation to Read Together

- `docs/opencode-global-install-plan.md`
- `docs/opencode-install-log.md`
- `docs/opencode-test-matrix.md`
- `docs/opencode-usage-guide.md`
- `docs/opencode-agent-models-and-replication-guide.md`
- `docs/opencode-repository-inventory-and-install-sources.md`

These documents together form the reproducible record of what was done on this machine.
