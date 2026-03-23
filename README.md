# OpenCode Configuration

This repository contains the documented record of a real OpenCode global setup performed on macOS, including:

- selected plugin stack
- installation order
- validation results
- agent/category/model mapping
- repository inventory with upstream URLs
- troubleshooting notes
- replication guidance for another machine, including a corporate machine

## Documents

- `docs/opencode-global-install-plan.md`
- `docs/opencode-install-log.md`
- `docs/opencode-test-matrix.md`
- `docs/opencode-usage-guide.md`
- `docs/opencode-troubleshooting-playbook.md`
- `docs/opencode-agent-models-and-replication-guide.md`
- `docs/opencode-repository-inventory-and-install-sources.md`

## Notes

- This repository records what was actually installed and tested on the source machine.
- Some repositories were evaluated but intentionally not kept in the final stack.
- `opencode-workspace` was installed as an optional `ocx` profile and is not part of normal `opencode` app startup unless explicitly launched.
