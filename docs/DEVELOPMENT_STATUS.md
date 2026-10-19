# Development status — 19 October 2026

## What this branch contains

- The initial Supabase/Postgres schema for profiles, the 36-team league, NHL players and games, roster entries, transactions, weekly fantasy matchups, daily fantasy points, and league settings.
- Row-level security policies so authenticated managers can read league-wide information but cannot directly write roster entries. A trusted server-side transaction processor must apply approved moves.
- League settings defaults for the 2026–27 season, the `America/Moncton` timezone, a $100,000,000 cap, 12 forwards, 6 defencemen, 2 goalies, up to 3 reserve players, and 5 regular-season moves.

## Important: this is foundation work, not a live release

This migration has been committed to the `league-foundation-oct-2026` branch only. It has not been applied to a Supabase project, and it does not yet provide working sign-in, team assignment, a live player database, an NHL schedule feed, a transaction-processing job, or automated matchup finalization. Do not merge or deploy it as a finished app.

## Required next implementation steps

1. Configure a Supabase project and environment variables; keep service-role credentials server-side only.
2. Add a controlled profile-provisioning flow and commissioner-only manager/team assignment.
3. Implement server-side transaction validation and Tuesday 04:00 Atlantic processing. Enforce the five-move regular-season limit atomically; preseason picks are exempt.
4. Build roster and salary-cap validation into the same transaction boundary. A player may be owned by multiple fantasy teams, but at most once per team.
5. Import a verified NHL player list and official 2026–27 schedule. Keep source timestamps and avoid fabricated scores.
6. Implement daily fantasy scoring and preserve per-day history; finalize head-to-head results at the weekly cutoff. The final scoring period must include NHL games through 10 April 2027 and must not finalize before those games finish.
7. Replace the prototype UI with authenticated, database-backed views for Standings, My Team, Players, Fantasy Schedule, NHL Schedule, League, Transactions, and Rules.
8. Add automated tests for RLS, cross-team player ownership, roster limits, salary cap, preseason exemptions, the five-move limit, pending effective times, Tuesday processing, and the final scoring period.

## Validation still required

Before production, run the migration against a disposable Supabase project, test all policies with manager and commissioner accounts, and exercise the transaction processor concurrently to ensure moves cannot exceed the league limit. Review all data ingestion and scoring against official NHL source data.
