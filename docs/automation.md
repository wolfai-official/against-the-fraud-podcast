# How this repo is updated

Every morning (~7:36 AM CT) a new episode of Against the Fraud Daily is
generated and published to the show's RSS feed. The same run then adds the
episode to this repo:

- `transcripts/<YYYY-MM-DD>-against-the-fraud-daily.md` — the full
  word-for-word transcript (the show is scripted, so this is exact, not
  auto-transcribed audio).
- `episodes/<YYYY-MM-DD>.md` — episode notes: summary, cases, FCA practice
  note, tech angle, and verified reference links (DOJ releases, court
  records, major news outlets).
- `catalog.json` — the new episode appended to the machine-readable catalog.
- `README.md` — a new row in the episode table.

Only text files are pushed — never the MP3. Audio lives on the podcast feed
(linked from each episode note and from `catalog.json`).

## For a coding agent

`docs/claude-code-catalog-brief.md` is a ready-to-paste brief for building a
catalog/transcript pipeline directly from the RSS feed. The feed URL is in
the README.
