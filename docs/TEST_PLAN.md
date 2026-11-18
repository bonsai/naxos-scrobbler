# Test Plan

## Discovery fixtures
- Logged-in NML album page with no playback.
- Album track list with titles, performers, and durations.
- Playback in progress.
- Player opened in a separate window, if supported by NML.
- Track transition and album completion.

## Playback state tests
- Play → pause → resume.
- Seek forward and backward.
- Buffering / stalled playback.
- Track change before eligibility.
- Track change after eligibility.
- Tab hidden or suspended.
- Service worker terminated and restarted.

## Metadata tests
- Composer distinct from performer.
- Multiple performers / conductor / orchestra.
- Multi-movement work.
- Missing track number or duration.
- Duplicate and changing DOM nodes.
- Non-Latin and accented titles.

## Last.fm tests
- Authentication success / denial / expiry.
- Now Playing update.
- Eligible scrobble.
- Network failure and retry.
- API rejection and malformed response.
- Duplicate submission prevention.
- Timestamp and batch behavior.

## Safety / privacy tests
- No audio bytes or stream URLs collected.
- No password or cookie access.
- Session key never appears in logs or exported JSONL.
- Disable switch prevents new submissions.
- Export contains only intended listening metadata.
