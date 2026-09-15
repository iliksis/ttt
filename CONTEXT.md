# Table Tennis Tournament Manager

A self-hosted application for running a table tennis club's tournaments: organizing entrants into categories, running each category through a tournament system (predefined per the DTTB Wettspielordnung, or custom), and tracking results round by round.

## Language

**Tournament**:
A single event with a name and date(s), containing one or more Categories. Has a lifecycle state: created, running, or completed. The unit shown on the dashboard.
_Avoid_: Event, competition (as a top-level term)

**Category**:
A top-level division within a Tournament that Entrants sign up for (e.g. "Men's Singles U18", "Women's Doubles Open"). Each Category runs its own TournamentSystem independently of other Categories in the same Tournament.
_Avoid_: Division, bracket (as a synonym for Category), event (when nested under a Tournament)

**Entrant**:
The unit that occupies a slot in a Draw and plays Matches within a Category — a single Player for a singles Category, a Pair for a doubles Category. Matches are always between two Entrants, never directly between Players in a doubles context.
_Avoid_: Player (when the doubles case matters), competitor, participant

**Player**:
An individual person registered in the system. A Player may be the sole member of a singles Entrant, or one of two members of a doubles Pair.
_Avoid_: Entrant (when doubles pairing matters), user (Players are not application users — they don't log in)

**Pair**:
Two Players grouped together as a single Entrant for a doubles Category.
_Avoid_: Team, doubles entrant

**TournamentSystem**:
The algorithm governing how a Category's Entrants are organized into Rounds and how progression/elimination/standings work — e.g. a predefined system from the DTTB Wettspielordnung (single-KO, double-KO/consolation, Swiss, round-robin groups, group+KO combination) or a custom system defined in code. Configured per-Category.
_Avoid_: Format, mode, bracket type

**Round**:
One discrete stage of a Category's TournamentSystem, producing a batch of Matches. Applies uniformly across systems: a KO round, a Swiss round, and a round-robin group's matchday are all "Rounds" — they differ in how their Draw is produced and what carries into the next Round, not in what a Round fundamentally is.
_Avoid_: Matchday, stage, leg (as a standalone term — matchday may still appear in UI copy for round-robin Rounds, but the underlying concept is Round)

**Draw**:
The assignment of Entrants to Matches for a given Round. "Drawing" a Round is the act of producing this assignment (randomly seeded, ranking-seeded, or standings-derived, depending on the TournamentSystem).
_Avoid_: Bracket (as a synonym for Draw — Bracket may still be used loosely in KO-specific UI copy, but the underlying concept is Draw), pairing (as a noun for the whole Round's assignment — reserve for a single Entrant-vs-Entrant pairing if needed)

**Match**:
A single contest between two Entrants within a Round, with a final Result once played. Live per-point scoring is out of scope; only the final Result is recorded and broadcast.
_Avoid_: Fixture, game (game is reserved for a set within a Match, not the Match itself, if that granularity is ever modeled)

**Result**:
The outcome of a completed Match (e.g. sets/games won by each Entrant). Feeds Standing calculation and, depending on the TournamentSystem, later Rounds' Draws.
_Avoid_: Score (Score may refer to a single set's points if that granularity is ever modeled; Result is the Match-level outcome)

**Standing**:
A Category's ranking of Entrants at a point in time, derived from Results so far (e.g. a round-robin Group's table, or Swiss rankings). Not applicable to systems without an ongoing table, like pure single-KO.
_Avoid_: Table, ranking, leaderboard

**Group**:
A sub-pool of Entrants within a Category's round-robin stage (e.g. Category "Men's Singles U18" split into Group A / Group B), each producing its own Standing. Distinct from Category: a Category is what Entrants sign up for; a Group is a subdivision the Draw creates within a round-robin TournamentSystem.
_Avoid_: Pool, bracket

## Open questions (not resolved here — belong on the wayfinder map)

- **Edit cascade**: when a completed Round's Result is edited after a later Round's Draw already depended on it (e.g. Group Standings seeded a KO Draw), what happens to the later Round? Flagged as a downstream ticket — depends on the data model and conflict-strategy decisions, neither of which is locked yet.
