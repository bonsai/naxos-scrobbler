# Naxos Scrobbler

Chrome Extension (Manifest V3) that observes album playback in Naxos Music Library and scrobbles eligible tracks to Last.fm.

## Status

**Design / discovery phase.** NML's authenticated player DOM and metadata structure have not yet been verified; selectors and player integration must not be guessed.

## Product decisions

- Source: Naxos Music Library (NML)
- Scope: tracks played as part of album playback
- Destination: Last.fm
- Platform: Chrome Extension Manifest V3
- Local history: JSONL export / append-only event model
- Audio: observe playback metadata only; never capture or download audio

## MVP sequence

1. Inspect the logged-in NML album and player DOM.
2. Build a read-only metadata / playback observer.
3. Validate track identity, duration, track changes, pause, seek, and album transitions.
4. Implement Last.fm authentication and scrobble submission.
5. Add durable queue, deduplication, diagnostics, and popup UI.

See docs/PRD.md, docs/ARCHITECTURE.md, docs/ADR.md, and docs/TEST_PLAN.md.
