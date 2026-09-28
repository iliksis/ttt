---
status: accepted
---

# Tech stack: Elixir/Phoenix LiveView on a single SQLite-backed Fly Machine

**Context**: A self-hosted, single-club app on fly.io with many concurrent viewers and several Editors per Tournament. ADR-0002 already requires every write within a Tournament to be serialized and every committed write to be broadcast on a per-Tournament topic; horizontal scaling of writers is explicitly not a goal. Scale is dozens of people per Tournament.

**Decision**:
- **Elixir/Phoenix, LiveView throughout** — viewer pages and edit mode alike. Each LiveView subscribes to its Tournament's Phoenix PubSub topic; there is no separate SPA, JSON API, or hand-rolled transport. Online-only (ADR-0002) matches LiveView's connected model.
- **Broadcasts are invalidations, not deltas**: `{:tournament_changed, tournament_id, write_seq}`. Each LiveView reloads what it shows; a reconnecting client just remounts. The per-Tournament `write_seq` is bumped inside every write transaction. The editor's "changed underneath you" warning compares the Match's current Result revision after reload.
- **SQLite on a Fly volume, one Machine.** ADR-0002's serialization lives in the database: every write runs in a SQLite `IMMEDIATE` transaction (write lock taken up front) that checks its preconditions, applies the change, bumps `write_seq`, and broadcasts only after commit. No per-Tournament GenServer. SQLite serializes the whole database, not just one Tournament — stricter than needed, free at this scale.
- **Backups: Fly's automatic daily volume snapshots, plus a manual snapshot when a Tournament completes** (convention), with snapshot retention raised above the default. No Litestream/LiteFS.
- **Deploys accept a few seconds of downtime** (a volume attaches to one Machine); by convention, no deploys while a Tournament is running.
- **Access**: the Organizer password is an instance-wide Fly secret. Each Tournament's edit password is stored hashed; unlocking puts `{tournament_id → editor_name, password_version}` into the signed Phoenix session cookie, checked by a LiveView `on_mount` hook. Changing the password bumps the version and invalidates existing unlocks.
- **TournamentSystem is an Elixir behaviour** (`validate_config/1`, `draw/2`, `standing/2`; optional `next_round/2` for Swiss pairings and `withdraw/2` for Swiss's withdrawal override), registered in one plain map module; a Stage's config is a JSON text column only its system interprets (ADR-0001).
- **Testing**: ExUnit with the Ecto SQL sandbox, `Phoenix.LiveViewTest` for edit flows including stale-revision refusal, StreamData property tests for each TournamentSystem's Draw/Standing invariants.

**Alternatives considered**:
- *TypeScript end-to-end* — rejected in favor of Phoenix's built-in PubSub/LiveView, which covers ADR-0002's broadcast requirement with no transport code.
- *Phoenix JSON API + React SPA* — rejected: two codebases and a client cache for no gain at this scale, and it discards LiveView's main benefit.
- *Postgres (Fly/managed)* — rejected: a second service to run and pay for, while ADR-0002 already rules out scaling beyond one app instance.
- *A GenServer per Tournament for serialization* — rejected: adds process lifecycle and supervision on top of a database transaction that is needed anyway and already guarantees the same ordering on one Machine.
- *Typed delta broadcasts* — rejected: every write type (Draw, Pairing, withdrawal cascade, void, …) would need its own patch logic in every view, plus missed-event replay.
- *Litestream to object storage* — rejected for now in favor of zero extra moving parts; see Consequences.

**Consequences**:
- **Accepted data-loss window**: if the volume is lost during a Tournament day, Results since the last snapshot are gone. The manual end-of-Tournament snapshot narrows but does not close this. Adding Litestream later is additive and doesn't change the app.
- **Escape hatch**: if one Machine ever stops being enough, move to Postgres and serialize per Tournament with `SELECT … FOR UPDATE` on the Tournament row inside the same write transaction; the invalidation-broadcast and write-shape stay unchanged. This is a revisit of this ADR and ADR-0002, not a tweak.
- **Verify at setup**: Fly's default snapshot retention and how far it can be raised; that the ecto_sqlite3 adapter can make `IMMEDIATE` the default transaction mode (otherwise begin write transactions explicitly as immediate).
