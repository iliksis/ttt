---
status: accepted
---

# Edit cascade: side-based winners, stale snapshots, only withdrawal walkovers cascade

**Context**: A Result can be corrected, voided, or cleared after later Matches, Swiss Pairings, or a later Stage's Draw already depended on it (e.g. a group Standing seeded a KO Draw, or "winner of M3" has since played M7). The system must decide what happens to that dependent state without silently destroying Results that reflect what really happened at the table.

**Decision**:
- A Result records its winner as a **side** of the Match, not as an Entrant; the Entrant is resolved through the side's Source. Correcting an earlier Match's winner therefore re-resolves who occupied later Matches, and their Results remain valid — the common case (a typo in an earlier Result, while the real winner played on) fixes itself.
- Sources resolve through a Match's current winning side even if that Result is voided; voiding affects Standing only.
- Clearing a Result is refused while any Match fed by it through a Source has a Result; the Editor is shown those Matches and clears them first, starting from the latest.
- Walkovers caused by a withdrawal are derived, not entered, so they are the one thing that cascades automatically: whenever Source resolution changes, the same serialized write re-checks them, clearing ones whose side no longer resolves to a withdrawn Entrant and recording newly implied ones, each marked as caused by the correction.
- Draws and Swiss Pairings stay snapshots. When the Seeding/Standing they'd be built from today differs from their snapshot they are *stale* — computed, never stored, shown to Editors only. A stale Draw can be redrawn (or the latest Round re-paired) only while no Results exist in it; a redraw reuses the snapshot's lot outcomes and keeps the replaced Draw in history with Editor and time. Nothing is ever redrawn automatically.
- A completed Tournament must be reopened by an Editor (completed → running) before its Results can be corrected.

**Alternatives considered**:
- *Winner stored as an Entrant* — rejected: correcting an earlier winner leaves every played downstream Match naming an Entrant who no longer appears in it, turning the most common correction into a manual repair.
- *Auto-recompute / auto-clear downstream* (redraw on Standing change, cascade-clear dependent Results) — rejected: it reshuffles announced brackets and quietly wipes Results of Matches that were really played.
- *Lock earlier Results once a later Draw/Pairing exists* — rejected: blocks legitimate corrections; staleness surfacing gives the same protection without the lock.

**Consequences**: the schema stores a winning side on Result, and any code needing "who won" must resolve through Sources. Withdrawal-walkover resolution must be re-runnable (idempotent over the current Source graph), not a one-shot effect of the withdrawal. Draw snapshots must retain lot outcomes to support minimal redraws. All cascade work runs inside the per-Tournament serialized write from ADR 0002.
