# OpenCode Installation Log

## Status Legend

- `planned`
- `in_progress`
- `passed`
- `partial`
- `failed`
- `skipped`

## Baseline

- Status: `passed`
- Timestamp: `2026-03-22 20:06`
- OpenCode binary: `/opt/homebrew/bin/opencode`
- OpenCode version: `1.2.15`
- Node.js version: `v25.2.1`
- npm version: `11.6.2`
- Bun global binary: not installed
- Existing global config dir: `~/.config/opencode`
- Existing global config file: `~/.config/opencode/opencode.json` did not exist
- Existing global package file: `~/.config/opencode/package.json` only referenced `@opencode-ai/plugin`
- Existing global custom command: `~/.config/opencode/commands/initialize-workspace.md`
- Existing storage dir: `~/.local/share/opencode`
- Existing cache dir: `~/.cache/opencode`
- Baseline `opencode models` worked before plugin installation
- Backup created: `backups/opencode-config-20260322-200622.tgz`

## Wave Results

### Wave 1

- Status: `passed`
- Installed `ocx` first via `npm`, but the npm build expected a global `bun` binary and failed at runtime.
- Remediation: removed the npm install and reinstalled `ocx` as a standalone binary with the official install script into `~/.opencode/bin`.
- Ran `ocx init --global`.
- Installed workspace profile with `ocx profile add ws --source tweak/p-1vp4xoqv --from https://tweakoc.com/r --global`.
- Verified `ocx --version` returned `2.0.2`.
- Verified workspace profile launch path with `ocx oc -p ws -- --version`.
- Verified workspace profile registration with `ocx profile list --global`.
- Verified workspace profile agents and permissions with `ocx oc -p ws -- agent list`.
- Installed `oh-my-opencode` noninteractively with `npx oh-my-opencode@3.12.3 install --no-tui --claude=no --openai=no --gemini=yes --copilot=no --opencode-zen=yes --zai-coding-plan=no --opencode-go=no`.
- Verified global plugin registration in `~/.config/opencode/opencode.json`.
- Verified generated config in `~/.config/opencode/oh-my-opencode.json`.
- Verified plugin loading through `opencode --print-logs --log-level DEBUG models`.
- `opencode run ...` initially returned `Session not found` inside this assistant environment.
- Root cause: inherited desktop-linked environment variables (`OPENCODE_CLIENT`, `OPENCODE`, `OPENCODE_PID`, `OPENCODE_SERVER_USERNAME`, `OPENCODE_SERVER_PASSWORD`).
- Workaround used for real CLI validation: run OpenCode with those variables unset.
- Upgraded core OpenCode from `1.2.15` to `1.2.27` via `opencode upgrade 1.2.27 -m brew` before deeper runtime validation.

### Wave 2

- Status: `passed`
- Added `opencode-antigravity-auth@latest` to `~/.config/opencode/opencode.json`.
- Added the full Google model configuration to `~/.config/opencode/opencode.json`.
- Updated `~/.config/opencode/oh-my-opencode.json` with `google_auth: false` to avoid auth conflicts.
- Updated `multimodal-looker` to use `google/antigravity-gemini-3-flash`.
- Verified plugin load in logs.
- Verified Google provider registration through `opencode --print-logs --log-level DEBUG models google`.
- Verified Antigravity models appear in model listing.
- `opencode auth login` did not expose the Antigravity OAuth method correctly in this environment, so a direct OAuth workaround was executed.
- Saved Antigravity account storage to `~/.config/opencode/antigravity-accounts.json`.
- Synced OpenCode auth store by adding a `google` OAuth entry in `~/.local/share/opencode/auth.json`.
- Verified real model execution with:
  - `env -u OPENCODE_CLIENT -u OPENCODE -u OPENCODE_PID -u OPENCODE_SERVER_USERNAME -u OPENCODE_SERVER_PASSWORD opencode run --model google/antigravity-gemini-3-flash --variant minimal "Respond with exactly ANTIGRAVITY_OK"`
- Result: `ANTIGRAVITY_OK`

### Wave 3

- Status: `passed`
- Added `opencode-scheduler` to `~/.config/opencode/opencode.json`.
- Verified the plugin was installed and loaded by OpenCode from npm cache.
- Scheduler smoke test executed successfully:
  - created `oc-smoke-test`
  - listed `oc-smoke-test`
  - deleted `oc-smoke-test`
  - verified deletion with `SCHEDULER_OK`
- Later cleanup: removed `plannotator` from the stack and deleted its generated command files because the user did not need the external plan-review UI.

### Wave 4

- Status: `passed`
- Added `opencode-websearch-cited@1.2.0` as the last plugin in the global plugin list.
- Initially configured `provider.google.options.websearch_cited.model` to `gemini-2.5-flash`.
- Google-backed cited search hit a project API enablement error for `Gemini for Google Cloud API (Staging)`.
- Added `provider.openai.options.websearch_cited.model = gpt-5.2` before `provider.google` so the cited search plugin uses the first working configured backend.
- Verified both plugins load in debug logs.
- Verified cited web search end-to-end with:
  - `env -u OPENCODE_CLIENT -u OPENCODE -u OPENCODE_PID -u OPENCODE_SERVER_USERNAME -u OPENCODE_SERVER_PASSWORD opencode run --model google/antigravity-gemini-3-flash --variant minimal "Use the websearch_cited tool to answer in one sentence: what is OpenCode?"`
- Result: cited search succeeded after switching backend priority.
- Later cleanup: removed `supermemory` from the stack and deleted its generated command files because the user did not want external memory/API-key overhead.

### Wave 5

- Status: `passed`
- Added `octto` before `opencode-websearch-cited` so cited web search stays last in plugin order.
- Verified `octto` loads in OpenCode debug logs.
- Verified Octto tools register during runtime initialization (`pick_one`, `pick_many`, `confirm`, `rank`, `rate`, `ask_text`, `ask_image`, `ask_file`, `ask_code`, `show_diff`, `show_plan`, `show_options`, `review_section`, `thumbs`, `emoji_react`, `slider`, `get_answer`, `get_next_answer`, `list_questions`, `cancel_question`, `push_question`, `create_brainstorm`, `get_session_summary`, `end_brainstorm`, `await_brainstorm_complete`).
- Full browser-driven brainstorming review remains optional for later hands-on use.
