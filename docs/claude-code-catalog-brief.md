# Against the Fraud Daily — episode catalog + transcript agent

## Source feed (canonical — use this URL, not the CDN URL)

`https://muse.ai/podcasts/feed/1296615416870816/5da72c16-813d-4986-a7f7-014d18b94fab`

## What the feed gives you (verified 2026-11-09)

- Standard RSS 2.0 with iTunes tags. One `<item>` per episode.
- Per item: `title`, `description` (one-line summary), `pubDate`,
  `guid` (stable, `isPermaLink="false"` — **use this as the dedupe key**),
  `enclosure` (the MP3: `url`, `length` in bytes, `type="audio/mpeg"`),
  `itunes:duration` (seconds).
- **No transcript in the feed.** There is no `podcast:transcript` tag and no
  transcript URL anywhere in the XML. Transcripts must come from the audio.

## Two gotchas

1. **The feed URL 302-redirects** to a signed CDN URL (`cdn.fbsbx.com/...xml`
   with expiring query params). Fetch the canonical URL above fresh on every
   run and follow redirects. Never hardcode or cache the CDN URL — it expires.
2. The episode description is short. Rich detail (case names, amounts,
   relator names) lives in the transcript, not the feed.

## Task

Build a daily job that keeps a catalog of every episode plus a full transcript:

1. Poll the feed once a day (new episodes land ~7:36 AM America/Chicago).
2. For each `<item>` whose `guid` is not already in the catalog:
   - Download the MP3 from the `enclosure` URL.
   - Transcribe it (Whisper / faster-whisper). Speaker diarization is a
     nice-to-have, not required — hosts are Alex and Jordan.
   - Save the transcript as `transcripts/<YYYY-MM-DD>-<slug>.md`.
   - Append a row to `catalog.json` (or `catalog.md`) with: guid, title,
     pubDate, duration seconds, mp3 URL, description, transcript path.
3. Never re-download or re-transcribe an episode whose guid is already cataloged.
4. If the feed fetch fails or returns no items, retry later — do not wipe the catalog.

## Notes

- The show is fully scripted, so an exact transcript exists at the source;
  audio transcription is only needed because the feed does not carry it.
- First episode (already live): "Against the Fraud Daily - Monday,
  November 9, 2026", 349s, guid `ep-df92d4c5-ba77-48ce-8fdc-d702e458529a`.
