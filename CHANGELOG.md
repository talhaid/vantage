# Changelog

What has shipped in Vantage, newest first. This records releases rather than individual
commits; the application source is in a private repository.

---

## 1.0 — August 2026

First public version. Live at [vantage.talhaid.tech](https://vantage.talhaid.tech).

### Ingestion

- Publisher feeds read directly at source — global broadcasters and dailies, defence and
  security reporting, institutional and financial sources — fetched in parallel, each with
  its own timeout so one failing publisher does not affect the rest.
- Fifteen-minute refresh cycle, a 48-hour age limit at ingest, and collapsing of
  near-identical headlines from different outlets.
- Article images taken from the feed and re-fitted to the size the interface actually
  renders.

### Classification and location

- Five-axis category set: conflict, geopolitics, economy, humanitarian, technology, and a
  residual bucket for anything that cannot be decided.
- Rule-based scoring with a weighted vocabulary, context vetoes for ambiguous terms, and
  capped weight for signals outside the article text.
- An additional check on the humanitarian axis that requires evidence of scale, keeping
  reporting formats — galleries, single-rescue stories, human-interest follow-ups — out of
  the list.
- Location resolved from a static gazetteer, longest match first, with no external
  geocoding service in the request path.

### Briefing

- Twice-daily generation with Google Search grounding and no article text in the prompt.
- Fixed four-part structure: summary with three takeaways, the last 24 hours, a
  first-principles explainer, and three forward-looking vectors.
- Validation before storage — refusals, disclaimer language, fabrication markers and
  missing section anchors are rejected and retried.
- Turkish produced as a verified translation of the finished English text, with structural
  anchors hidden from the model and each title checked for completion.
- Retry with backoff on transient errors, and a cooldown after a failed run.

### Terminal

- Map and list driven by one filtered set, filterable by category, region, free text and
  time.
- Theme-aware map tiles, category-coloured markers, hover previews and touch tolerance.
- Windowed list rendering and a bounded image loader.
- Phone bottom sheet with drag detents and velocity-based selection.
- Local branding for sources that publish no image.

### Home and reading

- Card grid built from the current briefing, each card opening the section it came from.
- One wire story selected by hand as the day's lead.
- Full English and Turkish interface, including place names and briefing timestamps.
- Light and dark theme, animated page transitions, deep-linkable briefing sections.

### Platform

- Cached responses with background refresh on every expensive path; concurrent triggers
  collapse into a single run.
- Corrections applied at read time, so a refresh cycle never erases an edit.
- Generated markdown sanitised through an allowlist, served under a strict content
  security policy with frontend dependencies vendored.
- Offline mock mode and a prompt replay cache for development without API cost.
