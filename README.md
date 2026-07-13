# Learning Library

Append-only store of educational YouTube videos discovered for research papers by the
outreach pipeline (`outreach-orchestrator`, `action=learning-path`).

- **`learning-library.jsonl`** — the durable store. One JSON object per line, one line per
  *video appearance*. Deduped by `url` at read time, so duplicate lines are harmless.
  Record: `{url, video_id, title, channel, views, duration_sec, concept, professor, paper_title, at}`.
- **Why JSONL, not SQLite:** it merges across machines by union. `.gitattributes` marks the
  file `merge=union`, so two machines appending different videos auto-merge with no conflict.
- **Sync:** git. `learning_library.py` pulls before a run and pushes after (best-effort),
  authenticating over HTTPS with `GITHUB_GARDEN_TOKEN` from
  `~/.config/atuin_inventory/secrets/keys`
  (remote is `taxkcd/library`). The token is never written into git config.

Do not hand-edit while a run is in progress.
