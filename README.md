> **Superseded — this work lives forward in Headless Oracle.** The
> pre-trade safety framing described in this repository has been
> incorporated into the `environment.market_state` constraint type
> in the Verifiable Intent specification. The canonical current
> location is [Headless Oracle](https://headlessoracle.com) and
> [PR #9 on agent-intent/verifiable-intent](https://github.com/agent-intent/verifiable-intent/pull/9).
> Content below is retained for historical reference.

---

# Agent Pre-Trade Safety Standard (APTS) v1.0

**Status:** v1.0.0 — stable, public draft
**License:** Apache 2.0

---

## What it is

A 6-check pre-trade safety protocol for autonomous agents making financial execution
decisions. The standard defines the minimum gate an agent must pass before submitting
an order to a regulated exchange or market-dependent protocol.

It is vendor-neutral. Any conforming oracle implementation satisfies the requirements.
The reference implementation is [Headless Oracle](https://headlessoracle.com).

---

## Why it exists

Without a shared standard, every agent team reinvents the same safety checks independently —
and often gets them wrong. The common failure modes (DST phantom hours, stale cached status,
missed circuit breakers) are predictable and preventable. The standard creates a shared
vocabulary so:

- Agent frameworks can assert conformance
- Audit logs can record which checks passed before each trade
- Risk teams can verify agent behaviour without reading source code
- The ecosystem converges on a common baseline rather than diverging into n variations

---

## The 6 Checks

| # | Check | Fail action |
|---|---|---|
| 1 | Obtain a Signed Market Status Attestation | HALT |
| 2 | Verify no active circuit breakers (`source != OVERRIDE\|SYSTEM`) | HALT |
| 3 | Verify the settlement window is open | HALT |
| 4 | Verify the oracle receipt is fresh (`expires_at` in future) | Fetch fresh; HALT if unavailable |
| 5 | Verify the Ed25519 signature cryptographically | HALT |
| 6 | Halt on any failure — no permissive fallback | HALT |

Full specification: [STANDARD.md](./STANDARD.md)

---

## Conformance

The Headless Oracle reference implementation passes all 6 checks. Verify at runtime:

```
GET https://headlessoracle.com/v5/compliance
```

Returns a machine-readable JSON response with each check listed as `status: "pass"`.

---

## Files in this directory

| File | Purpose |
|---|---|
| [STANDARD.md](./STANDARD.md) | Full normative specification with code examples |
| [CHECKLIST.yaml](./CHECKLIST.yaml) | Machine-readable checklist for tooling integration |
| [BADGE.md](./BADGE.md) | Compliance badge for project READMEs |
| [CI-INTEGRATION.md](./CI-INTEGRATION.md) | Adding APTS verification to CI/CD pipelines |

---

## License

Copyright 2026 LembaGang / Headless Oracle contributors.
Licensed under the Apache License, Version 2.0.
See: https://www.apache.org/licenses/LICENSE-2.0
