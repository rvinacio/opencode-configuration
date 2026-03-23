# OpenCode Test Matrix

## Baseline

- Verify `opencode --version`
- Verify `opencode --help`
- Inspect global config and plugin directories
- Snapshot current cache and logs state
- Result: passed

## Wave 1

- OpenCode starts successfully
- Workspace profile or commands are available
- `oh-my-openagent` installation is recognized
- No immediate command collisions or startup crash
- Result: passed
- Notes: `ocx` npm install was replaced with the standalone binary because the npm variant required a global `bun` binary.

## Wave 2

- Auth flow opens and completes
- Auth persists across a new invocation
- A model from the auth plugin responds successfully
- Result: passed
- Verified output: `ANTIGRAVITY_OK`

## Wave 3

- Scheduler commands load
- A trivial local scheduled job can be created and inspected
- Result: passed
- Scheduler smoke test job created, listed, deleted, and confirmed absent.

## Wave 4

- Cited search returns sources
- Result: passed
- `websearch_cited` passed after switching backend priority to OpenAI.

## Wave 5

- `octto` loads
- Interactive brainstorming flow starts successfully
- Result: partial
- Tool registration passed; browser-first brainstorming remains for later manual use.

## Final End-to-End

- Plan a task
- Execute with the core workspace/agent stack
- Use Google-backed model
- Perform cited web research
- Trigger a scheduled task
- Current state:
  - Google-backed model: passed
  - Cited web research: passed
  - Scheduled task: passed
  - Interactive Octto browser review: optional manual follow-up

## Post-Remap OMO Model Validation

- Config file changed: `~/.config/opencode/oh-my-opencode.json`
- JSON syntax validation: passed
- Runtime validation results:
  - `openai/gpt-5.4`: passed (`GPT54_OK`)
  - `openai/gpt-5.2`: passed (`GPT52_OK`)
  - `google/antigravity-claude-sonnet-4-6`: passed (`SONNET_OK`)
  - `google/antigravity-claude-opus-4-6-thinking --variant=max`: passed (`OPUS_OK`)
  - `google/gemini-3.1-flash-image-preview`: passed (successful text response)
- Open question:
  - `GPT-5.4 mini` is visible in the app UI, but direct CLI validation with obvious fully-qualified IDs still returned `Model not found` in this shell environment.
  - Practical implication: the mapping is preserved as requested, but should still be confirmed once from the normal app UI using an OMO path that selects `explore` or a `quick`-class task.
