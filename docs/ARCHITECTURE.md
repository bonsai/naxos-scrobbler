# Architecture

## Components

NML album / player page
- Content script: page context and album context observer
- Metadata extractor: visible DOM / documented page data only
- Playback observer: play, pause, seek, ended, track transition

Normalized track event flows to the MV3 service worker, which owns eligibility evaluation, durable queue and deduplication, Last.fm API requests, and diagnostics. The popup shows connection state, current track, enable/disable, queue, and export.

## Runtime boundaries
- Content script: observe the NML page; do not make Last.fm requests directly.
- Service worker: authenticate / submit to Last.fm and coordinate durable state.
- Popup: display status and user controls; no hidden background playback logic.
- Storage: persist queue and deduplication state before attempting network submission.

## Playback accounting

Track actual playback progression using player time observations and monotonic elapsed time. Handle pause, resume, seek, buffering, tab suspension, and track changes. Do not count a seek jump as listened time. If the player uses a separate window, determine whether the extension needs a content script on that exact NML origin after inspection.

## Normalized track model

    {
      "source": "nml",
      "albumId": null,
      "trackId": null,
      "album": null,
      "track": null,
      "artist": null,
      "albumArtist": null,
      "composer": null,
      "trackNumber": null,
      "durationSeconds": null,
      "startedAtUnix": null,
      "metadataConfidence": "unknown"
    }

Null means unavailable; never replace unknown values with invented metadata.

## Open technical questions
1. Is playback in the album page, a popup, or a separate player window?
2. Is the player an HTMLMediaElement or a custom player?
3. Which DOM/page data fields expose album and track metadata?
4. Are track transitions represented by DOM changes, player events, or both?
5. Which NML hostnames and URL patterns are used after login?
6. What Last.fm authentication approach is viable for the intended distribution?
