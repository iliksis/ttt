---
status: accepted
---

# Concurrency: serialized writes per Tournament + optimistic Result revisions

**Context**: Several Editors (sharing one password per Tournament, no individual accounts) and many viewers act on the same Tournament at once. Two Editors can record a Result for the same Match; a structural change (making a Draw or Swiss Pairing, withdrawing an Entrant) can race with a Result being recorded or corrected. Scale is a single club: dozens of players/viewers per Tournament, self-hosted on fly.io.

**Decision**:
- All writes within one Tournament are serialized: each runs as one atomic unit, and no two writes to the same Tournament interleave. Structural operations check their own preconditions inside that unit (e.g. a Swiss Pairing requires every Match of the previous Round to have a Result) and fail cleanly if they no longer hold.
- Results are stored as an append-only list of Result revisions per Match; the current Result is the newest revision. Recording, correcting, voiding, clearing, and auto-resolved walkovers all add a revision.
- Each Result write carries the revision the Editor last saw (optimistic check). If a newer revision exists, the write is refused and the Editor is shown the current Result, and must consciously choose to overwrite it. A write identical to the current Result is a no-op.
- Editors give a self-declared name when unlocking edit mode; it is recorded on every revision for attribution only, never for authorization.
- Every committed write to a Tournament triggers a broadcast on that Tournament's real-time topic (not only Result recordings), so open views and other Editors see structural changes too. The editor UI shows a soft warning when a Match being edited changes underneath; the server-side revision check remains authoritative.
- The app is online-only; there is no offline queue of edits to merge later.

**Alternatives considered**:
- *Last-write-wins with an audit log* — rejected: a wrong score can silently replace a correct one during a busy tournament, and the audit log only helps after the damage is noticed.
- *Pessimistic per-Match locks* — rejected: needs lock timeouts, "who has it open" UI, and stale-lock recovery, all disproportionate at this scale.
- *Optimistic versions on every entity* — rejected: structural operations touch many entities (a Draw, a Pairing, a withdrawal's walkover cascade); serializing per Tournament gives the same safety with one mechanism, and write volume per Tournament is tiny.

**Consequences**: the deployment must be able to serialize writes per Tournament — in practice one app instance per database, or a database whose transactions/row locks provide that serialization. Horizontal scaling of writers is not a goal and would require revisiting this ADR. Edit-cascade semantics (what happens to later Rounds/Stages when an already-depended-on Result changes) are decided separately (#6) but will run inside the same serialized write.
