# TI Team Package

Portable snapshot of the current global OpenCode TI team for reuse on another computer.

## What is inside

- `agents/` — copies of the current global agent files from `~/.config/opencode/agents/`
- `skills/` — copies of the current global skill folders from `~/.config/opencode/skills/`

Nothing else is packaged here.

## Team / families

- Business / Product
- Research / Discovery
- Frontend
- Backend / Platform
- Data
- AI
- Governance / Validation
- Game

## Routing rules to preserve

- `ti-team-routing` is the canonical routing contract.
- There is one top-level router; family routers refine, but do not override, that route.
- Frontend product work is a protected subsystem and must use the frontend studio chain in order:
  1. `frontend-business-strategist`
  2. `frontend-ux-architect`
  3. `frontend-visual-art-director`
  4. `frontend-implementation-architect`
  5. `frontend-experience-qa`
- Use the lightest valid path for simple local work; escalate to orchestration only when multiple families, unclear intent, or architecture tradeoffs are involved.
- Every cross-family handoff must include: original request, routing reason, artifact name, artifact contents, unresolved questions, and required validation mode.
- Category-first delegation is the default operating rule.
- Do not manually invoke `Sisyphus-Junior`; it is an internal executor, not a user-facing routing target.

## Global behavior summarized

From `~/.config/opencode/AGENTS.md` and the routing contract:

- keep scope tight; do exactly what is requested
- verify with diagnostics/tests/build when changes are made
- prefer explicit handoffs over vague summaries
- treat data, AI, governance, and game as first-class domains
- keep frontend product work on the protected studio path
