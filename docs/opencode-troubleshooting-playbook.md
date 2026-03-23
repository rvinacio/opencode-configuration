# OpenCode Troubleshooting Playbook

## First Checks

- Check logs in `~/.local/share/opencode/log/`
- Retry with `opencode --print-logs`
- Review official docs before making speculative changes

## Official Documentation First

1. `https://opencode.ai/docs/config`
2. `https://opencode.ai/docs/plugins`
3. `https://opencode.ai/docs/providers`
4. `https://opencode.ai/docs/troubleshooting`

## Common Recovery Actions

- Remove or reorder a newly added plugin
- Re-test the previous successful wave
- Clear `~/.cache/opencode` if provider/plugin cache looks stale
- Re-check `~/.local/share/opencode/auth.json` related auth state only when auth is the issue

## Validated Local Issues

### `Session not found` when running `opencode run`

- Cause seen here: nested CLI execution inherited desktop/session environment variables.
- Fix used successfully:

```bash
env -u OPENCODE_CLIENT -u OPENCODE -u OPENCODE_PID -u OPENCODE_SERVER_USERNAME -u OPENCODE_SERVER_PASSWORD opencode run ...
```

### `websearch_cited` fails on Google backend with project/API errors

- Symptom: Google backend returned `SERVICE_DISABLED` for a staging Google Cloud API.
- Fix used successfully: configure `provider.openai.options.websearch_cited.model` and place `openai` before `google` in `opencode.json` so cited search uses the first working backend.
