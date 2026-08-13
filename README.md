# status-snapshot

An **independent fallback record** of the ShockwaveHQ public status feed
(task 410592d9 / STATUS-PAGE-SAME-FATE-INDEPENDENT-SNAPSHOT-01).

Every ~10 minutes a GitHub Action fetches the public payload from
`ops.shockwavehq.com/api/status`, wraps it with `published_at` / `expires_at`
watermarks and an honest `origin_fetch` record, and publishes it two ways:
the machine endpoint `https://raw.githubusercontent.com/Shockwavehq/status-snapshot/snapshot/snapshot.json`
(a single-commit orphan branch, force-replaced each run — the BREAK-GLASS route that must
stay documented even if a branded domain is added later) and the human page on GitHub Pages. [`index.html`](index.html) renders it there
verdict-free: last-observed facts and their age, never a claim about right now.

- The workflow needs **zero secrets** — the origin is public and the commit is same-repo.
- History is kept at depth 1 (each run amends); the commit doubles as repo activity
  so the schedule is never auto-disabled.
- If the origin is unreachable, the last good payload is preserved **with its
  original watermarks** and the failure is recorded — an expired snapshot is
  refused by consumers, never re-stamped.

This repository lives in a separate failure domain (GitHub) from the systems it
snapshots (Supabase + Vercel) — it is the surface that survives their outage.
