# Table Tennis Tournament Manager

A self-hosted application for running a table tennis club's tournaments: organizing entrants into categories, running each category through a tournament system (predefined per the DTTB Wettspielordnung, or custom), and tracking results round by round.

## Language

**Tournament**:
A single event with a name and date(s), containing one or more Categories. Has a lifecycle state: created, running, or completed. The unit shown on the dashboard.
_Avoid_: Event, competition (as a top-level term)

**Category**:
A top-level division within a Tournament that Entrants sign up for (e.g. "Men's Singles U18", "Women's Doubles Open"). A Category runs through one or more ordered Stages; it does not itself have a single TournamentSystem — each of its Stages does.
_Avoid_: Division, bracket (as a synonym for Category), event (when nested under a Tournament)

**Entrant**:
The unit that occupies a DrawSlot and plays Matches within a Category — a single Player for a singles Category, a Pair for a doubles Category. Matches are always between two Entrants, never directly between Players in a doubles context. Scoped to one Category; carries a roster status (active or withdrawn) for that Category. Withdrawal's effect on already-generated Matches and Standing is TournamentSystem-specific — see TournamentSystem.
_Avoid_: Player (when the doubles case matters), competitor, participant

**Player**:
An individual person, with an identity independent of any one Category or Tournament — the same Player can be an Entrant in multiple Categories or Tournaments, and carries a Q-TTR ranking value used for Seeding. A Player may be the sole member of a singles Entrant, or one of two members of a doubles Pair.
_Avoid_: Entrant (when doubles pairing matters), user (Players are not application users — they don't log in)

**Pair**:
Two Players grouped together as a single Entrant for a doubles Category.
_Avoid_: Team, doubles entrant

**Stage**:
One ordered phase of a Category's competition (DTTB Wettspielordnung term: Turnierstufe), with its own TournamentSystem and config, producing Rounds. A Category with a "group then KO" format is a Category with two Stages: Stage 1 running a round-robin TournamentSystem, Stage 2 running a single-KO TournamentSystem. Seeding and Draw carry over from one Stage to the next.
_Avoid_: Phase, round (a Stage is not a Round — a Stage produces many Rounds)

**TournamentSystem**:
The algorithm governing how a Stage's Entrants are organized into Rounds and how progression/elimination/standings work. Predefined systems from the DTTB Wettspielordnung (single-KO, continued-KO, double-KO, round-robin groups, Swiss) and custom systems are both implementations of the same interface, registered together under a system id in one flat, code-level registry — "predefined" vs. "custom" is a naming distinction (fixed id vs. arbitrary id), not a structural one; there is no dynamic loading, since this is a single-admin, single-deploy app.

A TournamentSystem owns two responsibilities for its Stage:
- **Producing its Draw** from the Stage's Seeding (see Seeding, Draw). For KO-family systems (single/continued/double-KO) and round-robin, the entire Match structure for every Round — including Rounds that haven't been played yet — is determinable upfront from the Seeding alone, via Source references; only a system whose pairings genuinely depend on evolving results (Swiss) needs to do real work when a Round completes rather than at Draw time.
- **Computing its own Standing/tie-break ranking** from that Stage's Results and Entrant roster (not just Results — see Standing). The ranking cascade differs per system (e.g. Pluspunkte → Differenz → head-to-head → lot for round-robin, vs. wins → Buchholzzahl → head-to-head → lot for Swiss), so each system owns its own ranking logic rather than a central switch over system types.

A TournamentSystem also owns its own config shape (validated by that system's implementation; the Stage stores it as opaque data no central schema needs to know about) and, for withdrawal, may override the default behavior described under Result/Standing (Swiss is the one system that does).
_Avoid_: Format, mode, bracket type

**Seeding**:
The Q-TTR-derived rank order Entrants enter a Stage with (ties by lot; a later Stage's Seeding also folds in carryover from the immediately preceding Stage's Standing). Seeding order is computed once, uniformly, regardless of which TournamentSystem is running — it's *placement* of that order into DrawSlots (a KO bracket's fixed slot table, or a round-robin Stage's group distribution) that's TournamentSystem-specific. The Seeding actually used for a given Draw is snapshotted at Draw-creation time (see Draw), not recomputed live.
_Avoid_: Ranking (Ranking/rank-order is the general idea; Seeding is specifically the order Entrants enter a Stage with), rating (Q-TTR is a Player's rating; Seeding is the derived order)

**Round**:
One discrete step of a Stage's TournamentSystem, producing a batch of Matches (or, for Swiss Rounds after the first, a Pairing). Applies uniformly across systems: a KO round, a Swiss round, and a round-robin group's matchday are all "Rounds" — they differ in how their Matches are produced and what carries into the next Round, not in what a Round fundamentally is.
_Avoid_: Matchday, leg (as a standalone term — matchday may still appear in UI copy for round-robin Rounds, but the underlying concept is Round)

**Draw**:
The upfront assignment of Entrants to DrawSlots for a Stage (a KO bracket's slots, or a round-robin Stage's Group split), produced by placing the Stage's Seeding into slots per that TournamentSystem's placement rule. For a Swiss Stage, the Draw covers only round-1 seeding — later Rounds are produced as a Pairing instead, since Swiss pairings are recomputed from live standings each Round rather than fixed upfront. A Draw is a historical artifact once made, like a Result: the Seeding it was built from is snapshotted, so a later change to a Player's Q-TTR or a correction to a prior Stage's Result can't retroactively change what an already-made Draw looked like.
_Avoid_: Bracket (as a synonym for Draw — Bracket may still be used loosely in KO-specific UI copy, but the underlying concept is Draw)

**DrawSlot**:
One position within a Draw — either bound to an Entrant, or a bye. A round-robin Stage's DrawSlots are additionally grouped by Group.
_Avoid_: Slot (alone), position

**Pairing**:
A Swiss Round's Entrant-vs-Entrant matchups (from Round 2 onward), computed fresh each Round from the current Standing rather than fixed by a Draw.
_Avoid_: Draw (Pairing is recomputed per-Round; Draw is fixed upfront)

**Match**:
A single contest between two Entrants within a Round, with a final Result once played. Live per-point scoring is out of scope; only the final Result is recorded and broadcast. Each side of a Match is described by a Source.
_Avoid_: Fixture, game (game is reserved for a set within a Match, not the Match itself, if that granularity is ever modeled)

**Source**:
A reference describing where one side of a Match comes from — either a fixed Entrant, or the winner (or, for double-KO/continued-KO, the loser) of a specific earlier Match. Lets KO bracket progression, including double-KO's consolation-bracket crossing, be represented explicitly rather than derived from bracket-position arithmetic. When an Entrant withdraws, any not-yet-played Match whose Source resolves to them auto-resolves as a walkover to the opponent — this resolution is generic (not TournamentSystem-specific), since it follows from the Source graph alone. Such walkovers are recorded as Result revisions attributed to the Editor who recorded the withdrawal, marked as caused by it.
_Avoid_: Feed, link

**Result**:
The outcome of a completed Match: an ordered set-by-set ball score, a type (played, retired, or walkover), and a winner. A Result can be voided (annulled by the general forfeit rule) without being deleted — a voided Result stays in history but is excluded from Standing. Voiding is itself a Result revision, as is clearing a Result recorded by mistake (e.g. on the wrong Match), which returns the Match to unplayed. Clearing is a data-entry correction; voiding is a rule-based annulment — they are not interchangeable. Feeds Standing calculation and, depending on the TournamentSystem, later Rounds'/Stages' Draws or Pairings.

Default withdrawal behavior (applies to KO-family and round-robin): an Entrant's Results in the current Stage are voided, and further Matches depending on them resolve as walkovers via Source resolution. Swiss overrides this default — its results already played stand (not voided), and remaining Rounds are instead credited as explicit walkover losses, per its own scoring rules.
_Avoid_: Score (Score refers to a single set's ball count, not the Match-level outcome)

**Result revision**:
One entry in a Match's append-only Result history. Every recording or correction of a Match's Result adds a new revision; earlier revisions are never overwritten. The Match's current Result is its newest revision. Each revision records who made it (an Editor) and when. An Editor recording a Result states which revision they last saw; if a newer one has appeared since, the recording is refused and the Editor is shown the current Result, and must consciously choose to overwrite it. Recording a Result identical to the current one is a no-op, not a conflict.
_Avoid_: Version, edit, change (as the domain term)

**Editor**:
A person who has unlocked edit mode for a Tournament with its shared password. Identified only by a self-declared name, used for attribution (on Result revisions and in conflict messages) — never for authorization. Not a user account; all Editors of a Tournament have equal rights.
_Avoid_: User, admin, official (as the domain term — an Editor may be an official, but the concept is edit-mode access)

**Standing**:
A Category or Group's ranking of Entrants, computed by that Stage's TournamentSystem from non-voided Results *and* the Entrant roster (status: active/withdrawn) — never stored or materialized. Roster status matters, not just Results: a withdrawn Entrant is placed at the worst remaining position they could still reach, which isn't derivable from Results alone. Not applicable to systems without an ongoing table, like pure single-KO. For round-robin Stages, the DTTB Wettspielordnung's current text ranks by set/ball *differences* (won minus lost), not the "Satzquotient"/"Ballquotient" *ratio* terms common in informal club usage — both are cheap to derive from the same stored won/lost counts, so which one the UI surfaces is a later, not-yet-locked decision, not a schema constraint.
_Avoid_: Table, ranking, leaderboard

**Group**:
A sub-pool of Entrants within a round-robin-flavored Stage (e.g. Stage split into Group A / Group B), each producing its own Standing. Distinct from Category: a Category is what Entrants sign up for; a Group is a subdivision a Stage's Draw creates.
_Avoid_: Pool, bracket

## Open questions (not resolved here — belong on the wayfinder map)

- **Edit cascade**: when a completed Round's Result is edited after a later Round's Draw/Pairing already depended on it (e.g. Group Standing seeded a KO Draw), what happens to the later Round? Flagged as a downstream ticket (#6). Concurrency is settled (see Result revision); only the cascade semantics remain open.
- **Satzquotient vs. Differenz**: see the Standing entry above — not yet locked which term/formula the product surfaces, only that both are derivable from the same stored data.
