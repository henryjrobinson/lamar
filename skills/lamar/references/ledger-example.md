# The return-trip ledger

When you hand a wish off to an unattended run, the loose-end policy
(`references/gates.md`) sorts every judgement call into three buckets:
**Decided** (low-stakes, a default covered it — logged as FYI), **Flagged**
(consequential but reversible — done, but confirm), and **Blocked**
(irreversible / customer-data / security / cost — refused to guess, left for
you). The ledger is the run's report of those calls. It's what makes walking
away trustworthy: you come back to a short, honest list of exactly what was
assumed, what to double-check, and what still needs your eyes — not a wall of
diff you have to reverse-engineer.

## Sample report

*Wish: "Add a `/healthz` endpoint to the API and wire it into the deploy check."*

### ✅ Decided (low-stakes — FYI only)
- Named the route `/healthz` (matched existing `/readyz`; spec said "health endpoint").
- Returned `{"status":"ok"}` with HTTP 200 — no schema was specified.
- Put the handler in `routes/health.py` alongside the other liveness routes.

### ⚠️ Flagged (reversible — confirm)
- Health check only pings the process, **not** the database. Cheap and won't
  flap, but it'll report healthy during a DB outage — say the word and I'll add
  a DB ping.
- Wired the endpoint into `deploy.yml` with a 5s timeout / 3 retries. Reasonable
  defaults; tune if your cold starts run longer.

### ⛔ Blocked (needs you — did not guess)
- The endpoint is currently **unauthenticated**, so it's publicly reachable.
  Exposing internal liveness is a security call I won't make for you — confirm
  it's fine, or tell me which auth/allowlist to gate it behind.

---
See also `references/gates.md` for the loose-end policy this ledger reports
against, and `references/docs.md` for the canonical documentation set.
