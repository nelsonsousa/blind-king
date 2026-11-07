# Data Sources

Where the data lives, how to reach it read-only, and what is out of bounds.

**Never commit credentials here.** Name the environment variable that holds them; nothing else.

---

## Datasets

### `lichess-puzzles`

| Field | Value |
|---|---|
| **What it is** | Lichess puzzles, the project's only puzzle source (per the brief) |
| **Access command** | UNVERIFIED: `database.lichess.org` and `lichess.org/api` are blocked by the container proxy (checked 2026-11-06) |
| **Credentials env var** | None known |
| **Read-only** | Yes |
| **Environment** | Public |
| **Freshness** | Unverified |
| **Covers** | Unverified: format, fields, themes, difficulty ratings and licence have not been checked yet |
| **Sensitive fields** | None known |

---

## In bounds

Reading public Lichess puzzle data, once the domains are allowlisted on the host.

## Out of bounds

Nothing declared yet. Lichess's API rate limits and licence terms need checking before any
bulk use or redistribution inside the app.

## Status

The design gate returns `BLOCKED` on any premise that needs this data until the domains above
are added to the host `docker/allowlist.txt`, or a sample extract is placed in `/workspace/data/`.
