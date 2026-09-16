# DTTB Wettspielordnung: Tournament Systems

## Summary

This document catalogs the tournament/competition systems ("Austragungssysteme") defined by the
Deutscher Tischtennis-Bund (DTTB) in its official **Wettspielordnung (WO)** — the rulebook governing
all sanctioned table-tennis competitions in Germany — together with the seeding, draw, standings,
and tie-break rules for each.

**Source confidence: high.** The primary source (the DTTB Wettspielordnung PDF itself) was located,
downloaded, and read as plain text (via `pdftotext`), not summarized by a secondary source or by
model memory. All rule statements below are traceable to specific `§`/section numbers in that
document, and most are direct or near-direct translations of the German original, with the original
German quoted for the load-bearing sentences. No secondary sources were needed for the core systems;
two secondary sources are cited only for context/orientation in a couple of places, and are marked as
such.

The single most important structural finding: **the DTTB Wettspielordnung does not define "Group + KO
combination" as its own named system.** Instead, WO D 7.1 defines a tournament ("Konkurrenz") as
consisting of one or more sequential **"Turnierstufen"** (tournament stages), each of which
independently uses one of the systems defined in WO D 7.2–7.8. A "group then KO" event is simply a
tournament whose stage 1 uses the Gruppensystem (D 7.5) and whose stage 2 uses the
Einfaches K.-o.-System (D 7.2), linked by the seeding-carryover rule in D 5.6 and the draw rule in
D 6.3. This is covered in its own section below (Section 5) since it's exactly the mechanism the task
asked about, but it is a *composition* of two base systems, not a sixth named system.

## Sources

- **PRIMARY**: Deutscher Tischtennis-Bund e.V., *Wettspielordnung (WO)*, zuletzt beschlossen am 28. März 2026
  (5. DTTB-Bundesrat), Inkrafttreten ab 1. Juli 2026, zuletzt bearbeitet am 30. März 2026 (i.e. the version
  in force as of today, 2026-09-16).
  URL: https://www.tischtennis.de/fileadmin/documents/Satzungen_Ordnungen/2026/Stand_01.07.2026/03_WO__Gueltig_ab_01.07.26___30.3.2026__Reinfassung.pdf
  (linked from the official DTTB regulations index: https://www.tischtennis.de/dttb/regeln-satzung/satzung-ordnungen.html)
  Downloaded and converted to text locally (`pdftotext -layout`); all citations below reference this
  document's own `§`/Abschnitt/section numbering (Abschnitt D "Bestimmungen für Veranstaltungen in
  Turnierform", Abschnitt E "Grundlagen für Mannschaftskämpfe", Abschnitt K "Pokalmeisterschaften").
- **SECONDARY** (orientation only, not used for any specific rule claim below): general web-search
  result summaries surfaced while locating the primary PDF (not quoted or relied upon for content).
- Note: earlier/other-year mirrors of the same document were also found (2017, 2019, 2024) hosted by
  regional federations (Landesverbände) such as NRW, TTVSA, BTTV, TTBW, RTTVR — these confirm the
  document is a DTTB-wide text that Landesverbände republish verbatim with their own
  "Ausführungsbestimmungen" appended; they were not needed as sources since the current official PDF
  was successfully read directly.

---

## 1. Round-robin groups — "Gruppensystem ,,Jeder gegen jeden‘‘" (WO D 7.5)

**Definition (WO D 7.5):** "In Rundenform tritt jeder Spieler, jedes Paar bzw. jede Mannschaft gegen
jeden anderen bzw. jede andere an." (Round-robin form: every player/pair/team plays every other
player/pair/team once.)

**Seeding (Setzen) — WO D 5.1, D 5.2, D 5.4:**
- At least a quarter of the field in any "Konkurrenz" must be seeded (D 5.1): "Es muss mindestens ein
  Viertel des Teilnehmerfeldes einer Konkurrenz gesetzt werden."
- Seed order is derived from comparable **Q-TTR values** as of the tournament's cutoff date
  ("Stichtag", defined in D 1.4); for doubles/team events the sum of the relevant players' Q-TTR values
  is used (D 5.2). Ties in seed order are broken by lot ("Über die Reihenfolge in der Setzliste bei zwei
  oder mehr punktgleichen Spielern... entscheidet das Los", D 5.2).
- Group-specific seeding rule (D 5.4): at least as many players must be seeded as there are groups.
  - a) If the number of seeded players exactly equals the number of groups, each group gets exactly one
    seeded player (assignment or draw).
  - b) If more players are seeded than there are groups, the best seeded players are first distributed
    one-per-group (as in a), then the remaining seeded players are drawn into groups in seed-list order
    so the groups stay evenly filled, subject to D 6.2 (see below).

**Draw (Auslosung) — WO D 6.1–D 6.3:**
- The draw is public (D 6.1: "Die Auslosung ist öffentlich.").
- For at least the first tournament stage, the draw must keep players/pairs/teams from the same club,
  Kreis, Bezirk, member federation, or region apart as long as possible (D 6.2). Sport departments
  (Erwachsenensport/Jugendsport/Seniorensport) and Landesverbände may set different rules for later
  stages, but must publish them by the announcement/entry deadline (Ausschreibung).
- For later stages, the draw must keep players from the same group in the *previous* stage apart as
  long as possible (D 6.3).

**Standings / progression (Wertung) — WO D 7.5:**
Two parallel scoring schemes are defined, one for individual competitions and one for team competitions:

- *Individual competitions*: win = +1 "Pluspunkt", loss = −1 "Minuspunkt". Plus-points and
  minus-points, and won/lost sets and balls (points), are each summed across all matches. Placement
  is decided, in order: (1) higher plus-point total; (2) lower minus-point total (i.e., fewer losses,
  as tie-break on point total); (3) larger difference between sets won and sets lost across all matches
  played; (4) if still tied, larger difference between balls (rally points) won and lost; (5) if still
  tied among two or more, their head-to-head match(es) decide, applying game-point-, set-, and
  (if needed) ball-difference in that order; (6) if still tied, lot decides.
- *Team competitions*: win = +2 plus-points / −2 minus-points for the loser; a drawn team match gives
  each team +1/−1. Plus-points, minus-points, and won/lost game-points ("Spielpunkte" — the
  individual-rubber score within a team match), sets, and balls are summed. Placement order: (1) more
  plus-points; (2) fewer minus-points; (3) larger difference of game-points won/lost; (4) then larger
  set-difference; (5) then larger ball-difference; (6) if still tied among two-plus teams, their
  head-to-head team matches decide, applying table-point-, game-point-, set-, and ball-difference in
  that order; (7) if still tied, lot.

**Tie-breaking — WO D 7.5 (quoted above):** the exact cascade is: points (Pluspunkte) → fewer
minus-points → set-quotient (difference, not ratio — the WO uses "Differenz", a subtraction, not a
"Quotient"/ratio, despite the informal term "Satzquotient"/"Ballquotient" commonly used by clubs) →
ball-difference → head-to-head (in the same order of criteria) → lot. This is an important nuance for
a data model: **the current WO text uses differences ("Differenz"), not ratios/quotients**, contrary
to the informal terminology "Satzquotient"/"Ballquotient" often used colloquially (see Open Questions).

---

## 2. Single-elimination — "Einfaches K.-o.-System" (WO D 7.2)

**Definition (WO D 7.2):** "Der Gewinner eines Spiels bzw. Mannschaftskampfes kommt in die nächste
Runde und der Verlierer scheidet aus." (Winner advances, loser is eliminated.) The bracket size is
chosen based on entrant count: "eine 4er-, 8er-, 16er-, 32er-, 64er-, 128er-Turnierliste usw." (a
draw sheet sized as the next power of two: 4, 8, 16, 32, 64, 128, …).

**Seeding (Setzen) — WO D 5.1, D 5.3:**
- Same general seeding rule as above (≥ 1/4 of the field seeded, Q-TTR-based order, ties by lot).
- KO-specific placement (D 5.3): seed #1 and #2 go to the top and bottom slot of the bracket
  respectively. Further seeds are placed by a fixed draw-into-slot table:

  | Seed rank | Bracket size 8 | Bracket size 16 | Bracket size 32 | Bracket size 64 |
  |---|---|---|---|---|
  | 1, 2 | slot 1, slot 8 (fixed) | slot 1, slot 16 (fixed) | slot 1, slot 32 (fixed) | slot 1, slot 64 (fixed) |
  | 3, 4 | — | — | drawn into slots 8 & 9 | drawn into slots 32 & 33 |
  | 5–8 | — | — | drawn into slots 16, 17, 24, 25 | drawn into slots 16, 17, 48, 49 |
  | 9–16 | — | — | — | drawn into slots 8, 9, 24, 25, 40, 41, 56, 57 |

  (Reproduced from the table in D 5.3; for larger fields or seeding of more than a quarter of the
  field, the same pattern is extended analogously — "bei größeren Feldern oder bei Setzung von mehr
  als einem Viertel des Teilnehmerfeldes analog".)
- Byes ("Freilose"): unfilled bracket slots in round 1 are filled with byes, and **seeded players
  receive byes first, in seed-list order** (D 7.2: "Nicht belegte Rasterplätze der Turnierliste sind
  durch Freilose in der ersten Runde auszufüllen. Dabei sind zuerst den Gesetzten in der Reihenfolge
  der Setzliste Freilose zuzuteilen.").
- Seeding for later stages (D 5.6): players who were exempted/carried over from a prior stage are
  seeded first, then results from the immediately preceding stage are used.

**Draw (Auslosung):** same D 6.1–D 6.3 rules as above (public; same-club/region kept apart in stage 1;
same-group-in-prior-stage kept apart in later stages).

**Standings/progression:** binary — win advances, loss eliminates; no points table. Only the bracket
position (which determines eventual placement, e.g. quarterfinal losers share 5th–8th) determines
final standing, and even that fine-grained placement is normally undetermined in the simple KO system
(only 1st and 2nd place, and "shared" placements for the rest, unless a "Fortgesetztes K.-o.-System"
is used — see below).

**Tie-breaking:** not applicable in the simple KO system (no group standings to tie); the single
tie-break scenario is D 7.4's decisive rematch ("Stichkampf") in the double-KO final, see Section 3.

---

## 3. Double-elimination / consolation systems (WO D 7.3, D 7.4)

The WO defines two elimination variants beyond the simple KO:

### 3a. "Fortgesetztes K.-o.-System" (continued/consolation KO, WO D 7.3)
Same base structure as the simple KO system, **but losers of certain rounds do not leave the
tournament** — they instead play the other losers from the same round for the corresponding overall
placements (e.g., semifinal losers play for 3rd/4th place; quarterfinal losers play for 5th–8th
place, etc.). "Im Extremfall werden auf diese Weise alle Platzierungen des Gesamtfeldes ermittelt." —
in the extreme case, this determines the placement of the entire field, not just 1st/2nd.

### 3b. "Doppeltes K.-o.-System" (double-elimination, WO D 7.4)
"Ein Spieler/ein Paar/eine Mannschaft scheidet erst nach der zweiten Niederlage aus." (A
player/pair/team is eliminated only after their *second* loss.) This principle applies up to and
including the final. If the same two entrants would meet twice, the match is still played anyway,
though the "Kreuzen" (crossing) of losers into the consolation bracket ("Trostrunde") is designed to
largely prevent that outcome. **Tie-break mechanism specific to this system:** if both finalists enter
the final with exactly one loss each, "muss ein nochmaliger Stichkampf die Entscheidung bringen" — a
further decisive match ("Stichkampf") is required to decide the title. Bracket sizing and byes reuse
the D 7.2 rules ("Turnierliste und Freilose siehe WO D 7.2").

Seeding and draw rules for both variants are the same base rules as Section 2 (D 5.1/5.3, D 6.1–6.3),
since both are structurally KO brackets.

---

## 4. Swiss system — "Schweizer System" (WO D 7.6)

**Definition (WO D 7.6):** "Ähnlich dem Gruppensystem ,,Jeder gegen jeden‘‘, wobei jedoch nicht alle
Runden ausgetragen werden." (Similar to round-robin, but not all rounds are played.) The number of
rounds is at least the number of rounds a KO bracket of the same entrant count would need, and ideally
two more than that ("ist im Idealfall allerdings um zwei größer").

**Pairing per round:**
- Every player plays a different opponent each round; with an odd number of entrants, a different
  player each round gets a bye ("Freilos"), which counts as a win, so **every player always has the
  same number of games played** for scoring purposes.
- Pairings within a round are built to match players with the *same win count* against each other as
  much as possible: among all players with the current-highest win count, as many not-yet-played
  pairings as possible are formed; leftover top players are paired down against the next-highest win
  count against an opponent not yet played; this repeats down the standings, with winless players
  paired last, and (if necessary) the bye assigned last of all.
- Round 1 pairing/seeding: "sollten möglichst die stärksten Spieler wie beim K.-o.-System gesetzt
  werden" — the strongest players should, where possible, be seeded as in the KO system (i.e., reusing
  the D 5.3 seeding-into-bracket-slot logic conceptually for round-1 pairing).
- Before each subsequent round's draw, the current standings are recalculated and players sorted by
  win count. Players tied on win count may be **fine-sorted by their opponents' cumulative win counts
  ("Buchholzzahl"** = Buchholz number), where a bye-round win counts with the win total of the
  last-placed player in the table for this calculation.

**Standings/progression and tie-breaking (end of tournament):**
"Nach der letzten Runde hat der Spieler mit den meisten Siegen das Turnier gewonnen; bei gleicher
Anzahl an Siegen ist die Buchholzzahl maßgeblich. Ist auch diese gleich, entscheidet der direkte
Vergleich und andernfalls das Los." — After the last round, most wins determines the winner; ties are
broken by Buchholz number, then by head-to-head result, then by lot.

**Special retirement rule (WO D 7.6, penultimate paragraph):** if a player forfeits or prematurely
ends one of their matches, they must not continue in the tournament at all, but **remain in the
standings with the wins already achieved**, and are credited a walkover loss for every round they no
longer play. (This is explicitly different from the general forfeit rule in D 7.9, which is
disapplied for the Swiss system — "außer beim Schweizer System" — because match results already
played are NOT annulled the way they are in other systems.)

---

## 5. Group + KO combination ("Kombination aus Gruppen- und KO-System")

**Primary-source finding: this is not a separately-named/numbered system in the current WO.** Instead:

- WO D 7.1 establishes that a competition ("Konkurrenz") is held in one or more sequential
  **"Turnierstufen"** (tournament stages) over up to four consecutive days, and "Jede einzelne
  Turnierstufe wird in einem der unter WO D 7.2 bis D 7.8 definierten Austragungssysteme
  durchgeführt. Eine nachfolgende Turnierstufe darf in einem anderen dieser Austragungssysteme
  durchgeführt werden." — each stage independently uses one of the systems from D 7.2–7.8, and a later
  stage is explicitly permitted to use a *different* system than the previous stage. The
  Ausschreibung (announcement) must name the system used for each stage.
- The classic "groups then knockout" format is exactly this: stage 1 = Gruppensystem (D 7.5), stage 2+
  = Einfaches K.-o.-System (D 7.2) (or Fortgesetztes/Doppeltes KO).
- **Carry-over between stages** is governed by two rules already cited above:
  - Seeding for the later stage (D 5.6): "Zur Setzung von nachfolgenden Turnierstufen werden zunächst
    die von vorangegangenen Turnierstufen freigestellten Spieler berücksichtigt und danach die
    Ergebnisse der direkt vorangegangenen Turnierstufe verwendet." (Players exempted/carried over from
    the previous stage are seeded first; then the previous stage's results are used to seed the rest —
    i.e., group standings feed directly into KO seeding.)
  - Draw for the later stage (D 6.3): entrants who were in the same group in the immediately preceding
    stage must be kept apart as long as possible in the later-stage draw.
- A concrete example of this composition appears in **Abschnitt K (Pokalmeisterschaften / cup
  competitions), WO K 6**: for each round of cup play, the responsible body chooses between "dem
  Einfachen K.-o.-System gemäß WO D 7.2 und dem Gruppensystem gemäß WO D 7.5" — i.e., cup rounds can
  each independently be run as either a simple-KO or a group round, round by round, using the same two
  base systems as building blocks.

**Note on team-match internal format vs. tournament system:** WO K 8 additionally specifies that in
follow-on ("weiterführende") cup championships, the *internal order of play within each team match*
("Spielsystem") is WO E 6.4.2, the "Modifiziertes Swaythling-Cup-System" — this governs the
doubles/singles sequencing *inside* one team match, not the tournament-progression system, and is a
distinct concept from the D 7.x Austragungssysteme covered in this document. It's flagged here only
to avoid confusion between "Spielsystem" (match format, Abschnitt E) and "Austragungssystem"
(tournament progression system, Abschnitt D) — the WO itself uses both terms and they are easy to
conflate.

---

## 6. Other systems permitted but not defined by the WO

- **WO D 7.7**: for DTTB-level ("Bundesveranstaltungen") tournaments, further systems are permitted if
  described in the DTTB's "Durchführungsbestimmungen für Veranstaltungen des DTTB" (implementation
  regulations) — i.e., the WO explicitly defers definition of additional systems to a subordinate
  document not fetched in this research pass (see Open Questions).
- **WO D 7.8**: Landesverbände (member federations) may permit further systems for tournaments within
  their own jurisdiction.
- **WO D 1.3 (context, line ~1848)**: for invitational/open tournaments, only systems approved by the
  sanctioning member federation are allowed.

These are "escape hatches" rather than defined systems — the WO leaves them unspecified by design and
delegates to lower-level regulations, so a code abstraction should treat "systems 7.2–7.6" as the
closed, fully-specified set and treat anything else as an extension point / plugin, not something to
hard-code rules for.

---

## Cross-cutting rules relevant to any system (for the data model)

- **Forfeits/retirements (WO D 7.9, general case)**: if a player/pair/team fails to appear or quits
  mid-competition, they're barred from further play in that and later stages, and their played matches
  in the *current* stage are voided from the standings ("werden ... annulliert") — **except in the
  Swiss system**, where already-played results stand (see Section 4). The retiring entrant is placed
  at the worst remaining position they could still reach in that stage (and, if that implies
  qualification to a later stage, at the worst position there too).
- **Score recording on retirement/DQ (WO D 7.10)**: sets/balls played up to the retirement are still
  recorded; the incomplete set is recorded as X:11 (where X is the retiring side's ball count, winner
  gets at least X+2); remaining required sets are recorded 0:11. A full walkover is recorded 0:11 per
  required set.
- **Q-TTR/ranking-list relevance of matches (WO D 7.11–7.13)**: governs which individual rubbers count
  toward the German ranking list (Tischtennis-Rangliste) computation; not itself a tournament-system
  rule but relevant if the data model needs to flag TTR-relevant matches.
- **Oberschiedsrichter (WO D 8)**: a licensed referee supervises the draw and enforces rules; not a
  system rule but establishes that the draw is a supervised, auditable event.

---

## Open questions / gaps

1. **"Satzquotient"/"Ballquotient" (ratio) vs. "Differenz" (difference)**: the current WO text (D 7.5)
   defines tie-breaking using *differences* ("Differenz zwischen gewonnenen und verlorenen Sätzen/
   Bällen"), not ratios/quotients, even though German table-tennis club culture very commonly talks
   about "Satzquotient" and "Ballquotient" informally. I could not find the literal words "Quotient",
   "Satzquotient", or "Ballquotient" anywhere in the current primary-source text (confirmed via text
   search across the whole document). **If the existing ttt domain model or glossary already uses
   "quotient" terminology, that should be reconciled against this finding** — either an older WO
   version used ratios and the current one switched to differences, or "quotient" is informal club
   parlance for the difference-based system, or the ttt project's earlier glossary commit encoded a
   simplification. This is worth explicit verification against `docs/` glossary content already in
   the repo before building the data model.
2. **DTTB "Durchführungsbestimmungen für Veranstaltungen des DTTB"**: WO D 7.7 references this as the
   place where additional DTTB-only systems get defined. This document was not located/fetched in this
   pass — if the ttt project ever needs to support DTTB championship-specific formats beyond D 7.2–7.6,
   that document should be tracked down separately.
3. **Historical WO versions**: several older mirrors (2017, 2019, 2024) were found during search but
   not diffed against the current (2026) text. The rules quoted here are from the version currently in
   force (effective 2026-07-01); if the ttt club has historical tournaments run under older rule
   versions, the exact tie-break formulas (points/differences) may have changed release to release —
   not verified here.
4. **WO E 6 "Spielsysteme" (Swaythling-Cup etc.)**: only skimmed enough to confirm it's a different
   concept (order of play *within* one team match) from the Abschnitt D tournament-progression
   systems. If the ttt data model ever needs to model team-match internal rubber order (e.g. for cup
   competitions per WO K 8), that section (WO E 6.1–6.4, pages ~47–50 of the PDF) should be read in
   full separately — it was not exhaustively extracted here since it's out of scope for "how entrants
   are seeded/paired/progress through a tournament."
5. No genuinely independent secondary source was cross-checked against the primary text (e.g. a
   myTischtennis.de explainer) since the primary PDF was fully machine-readable and internally
   consistent; I judged this unnecessary and a better use of effort was extracting more primary text.
   If stronger corroboration is wanted, a secondary cross-check could still be done.
