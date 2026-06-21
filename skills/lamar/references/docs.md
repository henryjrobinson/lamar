# Canonical documentation set

Every repo keeps the same five documents in the same shape, so onboarding,
handoff, and review are identical everywhere. Lamar produces/updates these as
work happens; the org and tier dials set how rigorously they're kept.

| Doc | Path | Holds | When |
|---|---|---|---|
| **PRD** | `docs/prds/<feature>.md` | problem · intent · non-goals · acceptance criteria · data class · blast radius · test approach · rollout/rollback · open questions | per Tier 1–2 feature |
| **SESSION_HANDOFF.md** | `docs/` | what just happened, what's mid-flight, the next step | written at end of a work session; read at session start |
| **CURRENT_STATUS.md** | `docs/` | where the project is now (shipped / in-progress / blocked) | updated at milestones |
| **ROADMAP.md** | `docs/` | what's next, in priority order, phased | updated when priorities shift |
| **CHANGELOG.md** | repo root | dated entries grouped Added / Changed / Fixed / Security | before every prod deploy |

## When each is required (org + tier dials)
- **Solo:** keep them lightweight — `SESSION_HANDOFF` + `CHANGELOG` are the must-haves; `CURRENT_STATUS` / `ROADMAP` updated at milestones; PRD only for Tier 2.
- **With teammates:** all five mandatory and fully written — they're how someone picks up cold.
- **By tier:** Tier 0 → no docs. Tier 1 → handoff + a changelog entry (PRD optional). Tier 2 → full PRD required.

## Maintained by
`/prd` (PRD) · `/roadmap-update` (status + roadmap + handoff) · `/changelog` (release notes) · `/sync-docs` (deep product docs). At the right moments (end of session, before deploy, milestone), offer to run these rather than letting docs rot.

## Reconciliation (existing repos)
Converge on this shape **incrementally** — map existing scattered docs onto the canonical names as they're touched; don't big-bang migrate. The per-repo `doc_home` in `.lamar.md` records where PRDs live.
