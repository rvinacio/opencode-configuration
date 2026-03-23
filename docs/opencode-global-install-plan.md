# OpenCode Global Installation Plan

## Scope

- Install and validate the selected OpenCode plugin stack directly in `~/.config/opencode`.
- Use frequent checkpoints instead of a single final test pass.
- Prefer official OpenCode docs first when behavior is unclear or a failure occurs.

## Official References

- Config: `https://opencode.ai/docs/config`
- Plugins: `https://opencode.ai/docs/plugins`
- Providers: `https://opencode.ai/docs/providers`
- Troubleshooting: `https://opencode.ai/docs/troubleshooting`

## Selected Stack

1. `kdcokenny/opencode-workspace`
2. `code-yeongyu/oh-my-openagent`
3. `NoeFabris/opencode-antigravity-auth`
4. `different-ai/opencode-scheduler`
5. `ghoulr/opencode-websearch-cited`
6. `vtemian/octto`

## Execution Waves

### Wave 0 - Baseline

- Capture current OpenCode state.
- Back up `~/.config/opencode` before changes.
- Record logs, cache state, versions, and config inventory.

### Wave 1 - Core Workspace

- Install `opencode-workspace`.
- Install `oh-my-openagent`.
- Test startup, commands, agents, and coexistence.

### Wave 2 - Google Auth

- Install `opencode-antigravity-auth`.
- Validate auth flow, model discovery, and a real model invocation.

### Wave 3 - Scheduling

- Install `opencode-scheduler`.
- Test scheduled job behavior.

### Wave 4 - Cited Search

- Install `opencode-websearch-cited` last in plugin order.
- Test cited search.

### Wave 5 - Interactive UX

- Install `octto`.
- Test interactive brainstorming flow.

## Validation Policy

- After every install step, verify OpenCode still starts.
- After every wave, run feature-specific smoke tests.
- If a failure appears, stop and investigate before adding more plugins.
- Investigation order:
  1. OpenCode logs
  2. Official OpenCode docs
  3. Plugin docs
  4. Plugin ordering, config, cache, and auth state

## Deliverables

- Installation log
- Test matrix and results
- Usage guide for each installed component
- Troubleshooting playbook

## Removed After Evaluation

- `backnotprop/plannotator` removed because native doc review inside OpenCode was sufficient for this workflow.
- `supermemoryai/opencode-supermemory` removed because the user does not want external memory/API-key overhead.
