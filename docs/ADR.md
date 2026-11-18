# Architecture Decision Records

## ADR-001: Chrome Extension MV3

**Status:** Accepted for MVP

**Decision:** Implement as a Chrome Extension using Manifest V3.

**Rationale:** NML playback and metadata can be observed in the browser where the user is already authenticated. A browser extension avoids a separate local daemon for the initial version.

**Consequences:**
- Content scripts observe NML pages and player surfaces.
- A background service worker owns Last.fm API requests and durable queue coordination.
- Persistent state must live in extension storage / IndexedDB; service-worker memory is not durable.
- Player integration must be verified against the actual logged-in NML experience before selectors are committed.

## ADR-002: Observe playback; do not control or capture audio

**Status:** Accepted

The extension reads player state and visible/page metadata only. It does not download, capture, proxy, or fingerprint audio, and it does not automate playback controls.

## ADR-003: Per-track scrobbling for album sessions

**Status:** Accepted

Album playback is the entry context, but each NML track or movement is independently evaluated and submitted to Last.fm. Album identity is retained for session grouping and deduplication.

## ADR-004: JSONL local event history

**Status:** Accepted

Use append-only JSONL records for exportable listening history and diagnostic events. Runtime queue and deduplication state may use IndexedDB or chrome.storage.local; JSONL is the portable export format, not the transactional queue database.

## ADR-005: Last.fm credentials

**Status:** Proposed — security review required

Use Last.fm's documented authentication flow. Keep the user session key local. A public extension cannot safely conceal a shared API secret; decide whether to use a small backend, user-provided credentials, or another compliant distribution model before public release.
