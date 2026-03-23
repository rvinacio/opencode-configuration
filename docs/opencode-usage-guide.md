# OpenCode Usage Guide

## Installed Stack

- Core workspace profile: `ocx` + `ws` profile from `kdcokenny/opencode-workspace`
- Core global plugins:
  - `opencode-antigravity-auth@latest`
  - `oh-my-opencode@latest`
  - `opencode-scheduler`
  - `octto`
  - `opencode-websearch-cited@1.2.0`

## Additional Detailed References

- Agent/category/model mapping and work-machine replication notes:
  - `docs/opencode-agent-models-and-replication-guide.md`
- Repository inventory, SSH/HTTPS origins, and installation sources:
  - `docs/opencode-repository-inventory-and-install-sources.md`

## How To Start

### Default global OpenCode

```bash
opencode
```

Use this when you want your normal global stack with the installed plugins.

### Important when running from inside another OpenCode/desktop-linked environment

If you are launching nested CLI tests from a shell that already inherits OpenCode desktop/session variables, unset them first:

```bash
env -u OPENCODE_CLIENT -u OPENCODE -u OPENCODE_PID -u OPENCODE_SERVER_USERNAME -u OPENCODE_SERVER_PASSWORD opencode
```

This avoids the `Session not found` failure seen during validation.

### Workspace profile mode

```bash
ocx oc -p ws
```

Use this when you specifically want the `opencode-workspace` profile with its bundled specialists and permissions.

## What Each Piece Is For

### `ocx` + `ws`

- Purpose: launches a curated multi-agent workspace profile without modifying each project.
- Best for: structured plan/build/research/review flows.
- Important: this profile is not active in normal `opencode` app usage unless explicitly launched.
- Useful checks:

```bash
ocx profile list --global
ocx oc -p ws -- agent list
```

### `oh-my-opencode`

- Purpose: the main advanced harness with orchestration, ultrawork flow, agent tuning, hooks, and extra tooling.
- Best for: intense implementation sessions where you want the harness to push work forward.
- Day-to-day hint: include `ultrawork` or `ulw` in prompts.
- Important: not every configured OMO agent appears as a top-level visible UI option; some exist mainly for delegation and internal routing.

### `opencode-antigravity-auth`

- Purpose: adds Google/Antigravity OAuth and exposes Gemini and Claude models through Google credentials.
- Best for: using Google-backed Gemini and Antigravity Claude models in OpenCode.
- Current local model examples:
  - `google/antigravity-claude-opus-4-6-thinking`
  - `google/antigravity-claude-sonnet-4-6`
  - `google/antigravity-gemini-3-flash`
  - `google/antigravity-gemini-3-pro`

### `opencode-scheduler`

- Purpose: recurring jobs using macOS `launchd`.
- Best for: repeated reporting, checks, searches, reminders, or maintenance tasks.
- Typical prompts:
  - `Schedule a daily job at 9am to ...`
  - `Show my scheduled jobs`
  - `Run the <job> job now`

### `octto`

- Purpose: interactive browser brainstorming and design clarification.
- Best for: vague ideas that need structured questioning before execution.
- Day-to-day usage: select the `octto` agent and describe your idea.

### `opencode-websearch-cited`

- Purpose: source-backed web research with inline citations.
- Best for: external research where traceable sources matter.
- Important: it is intentionally last in the plugin list.

### Built-in web search vs `opencode-websearch-cited`

- Core OpenCode already has `websearch`.
- Built-in `websearch` is enough for normal discovery and quick research.
- `opencode-websearch-cited` overlaps with that, but adds cleaner citation-style output and explicit source formatting.

#### Simple rule

- Use built-in `websearch` when you just want to find information.
- Use `websearch_cited` when you want a response that is easier to audit, quote, or turn into docs.

#### Why keep it

- You said you already use web search a lot.
- This plugin is the only one in your current stack that still adds distinct value without changing the UI much.
- It gives you a more citation-oriented answer format than the normal built-in search flow.

## Important Compatibility Choices Applied

- `opencode-antigravity-auth` is loaded before `oh-my-opencode`.
- `opencode-websearch-cited` is loaded last.
- `opencode-websearch-cited` currently prefers OpenAI for cited search because that backend validated cleanly in this environment.
- `oh-my-opencode` has `google_auth` disabled to avoid conflicting Google auth flows.
- OMO model assignments were later customized beyond the initial install defaults; see the remapping snapshot below and the dedicated replication guide.

## Current OMO Remapping Snapshot

The OMO configuration was later remapped to the user's preferred model choices. See:

- `docs/opencode-agent-models-and-replication-guide.md`

Highlights:

- `sisyphus`, `hephaestus`, `oracle`, `prometheus`, `metis` → `openai/gpt-5.4`
- `atlas`, `sisyphus-junior` → `openai/gpt-5.2`
- `momus` → `google/antigravity-claude-opus-4-6-thinking`
- `multimodal-looker` → `google/gemini-3.1-flash-image-preview`
- `writing` → `google/antigravity-claude-sonnet-4-6`
- `quick`, `unspecified-low`, `unspecified-high`, `explore` → intended `GPT-5.4 mini` mapping; UI-visible but still needs final app-level confirmation

## Current Gaps Before Final Validation

- Browser-first UX flows remain optional for later hands-on testing.
