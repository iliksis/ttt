---
status: accepted
---

# TournamentSystem: draw-time generation, self-owned standing, code-registered plugins

**Context**: A Stage's TournamentSystem (single-KO, continued-KO, double-KO, round-robin, Swiss, or custom) needs one interface that covers both DTTB-predefined and custom systems, and that can express D7.9's withdrawal/walkover rules without a later breaking change.

**Decision**:
- A TournamentSystem's only two responsibilities are producing its Draw from the Stage's Seeding, and computing its own Standing/tie-break ranking from Results + roster status. For KO-family and round-robin systems, the entire Match tree (including future Rounds) is generated once at Draw time via `Source` references to not-yet-played Matches — no per-Round system logic is needed. Only Swiss, whose pairings depend on live standings, does real work when a Round completes.
- Seeding *order* (Q-TTR sort, ties by lot, prior-Stage carryover) is computed once, generically, outside any TournamentSystem; only seed *placement* into DrawSlots is system-specific. The Seeding used for a Draw is snapshotted at Draw time so later Q-TTR changes or Result corrections can't retroactively alter a published bracket.
- Withdrawal has a generic default (void the Entrant's Results this Stage; unresolved `Source`s auto-resolve as walkovers to the opponent) that covers KO-family and round-robin; Swiss overrides it (results stand, remaining Rounds become explicit walkover losses) since it's the one system the WO carves out an exception for.
- Predefined and custom systems are both implementations of one interface, registered together under a system id in a single flat code-level registry — no dynamic loading, since this is a single-admin, single-deploy app. Each system owns and validates its own config shape as opaque data; there's no central config schema to keep in sync when a system is added.

**Alternatives considered**: requiring every TournamentSystem to generate Rounds incrementally (one "next round" call per completed Round) was rejected — it would force KO-family/round-robin systems to implement pointless per-round logic when their whole structure is already fixed at Draw time via `Source`, and would make the common case (most systems) look identical in shape to the one system (Swiss) that actually needs incremental state.

**Consequences**: adding a new predefined or custom system never touches shared engine code (no central Standing switch, no central config schema) — it only means writing a new registry entry. The trade-off is that `Source` resolution and default-withdrawal handling live in generic engine code that every KO-family/round-robin system implicitly depends on, so a future system that doesn't fit the "fully static Match tree" shape would need either a Swiss-style override or a deliberate revisit of this ADR.
