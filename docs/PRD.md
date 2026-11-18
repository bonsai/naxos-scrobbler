# Product Requirements: Naxos Scrobbler

## Goal

Automatically record eligible individual tracks played during album playback in Naxos Music Library (NML) to the user's Last.fm account.

## User pain

Classical listeners use NML, but their listening history may not be reflected in Last.fm. Classical metadata is hierarchical and often splits one work into multiple movements. Manual logging is tedious and inconsistent.

## Scope

### In scope (MVP)
- NML album playback only.
- Observe current track and playback state without controlling the player.
- Extract track title, album title, performer/artist, duration, and track number when available.
- Preserve composer and richer performer credits in local metadata.
- Apply Last.fm scrobble eligibility rules.
- Submit each eligible movement/track to Last.fm.
- Prevent duplicate submissions and recover queued submissions after service-worker restarts.
- Provide a local JSONL history export.

### Out of scope (MVP)
- Other streaming services.
- Radio, search-result previews, isolated single-track playback outside album context.
- Audio capture, downloading, fingerprinting, or stream URL extraction.
- Cloud sync or a hosted backend.
- Automatic metadata enrichment from unrelated third-party databases.

## Functional requirements
1. Detect an album playback context.
2. Identify track transitions reliably.
3. Accumulate actual playback time; do not count paused or seek-skipped time.
4. Treat each movement/track as an independent Last.fm track.
5. Submit Now Playing updates when useful and scrobbles when eligible.
6. Persist pending work and deduplication keys.
7. Expose connection state, current track, queue state, and recent outcomes.
8. Allow users to pause/disable scrobbling and export local history.

## Scrobble eligibility

Follow Last.fm's current API rules. The usual duration threshold is the lesser of half the track duration or four minutes, with a minimum track-length requirement. Verify against current official documentation during implementation. Do not count seek jumps as listened time.

## Metadata mapping
- artist: primary performer / artist credit, not automatically the composer.
- track: movement or track title as supplied by NML.
- album: album title.
- albumArtist: album-level performer credit, if available.
- trackNumber: NML track order, if available.
- duration: track duration in seconds.
- timestamp: Unix time when the track began playing.
- composer: retain locally; Last.fm submission support must follow its API schema.

Do not fabricate missing metadata. Record extraction confidence and source fields locally.

## Privacy and security
- No audio capture or download.
- No collection of NML passwords or session cookies.
- Request only required host permissions.
- Store the user's Last.fm session key locally; never log it.
- Do not ship a reusable API shared secret in a public extension package without an explicit mitigation decision.
- Explain all data sent to Last.fm.

## Acceptance criteria
- A full album session produces one local record per eligible track.
- Pauses and seek jumps do not inflate listened time.
- Repeated observer events do not create duplicate scrobbles.
- Service-worker restart does not lose pending submissions.
- Missing/ambiguous metadata is surfaced and not silently misattributed.
- User can disable submission and export local history.
