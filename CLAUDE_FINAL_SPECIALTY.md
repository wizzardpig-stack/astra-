# ASTRA final specialty pass

Continue from branch `astra-final-launch` after commit `49667022751c90c9d59e34376b188593c76a60a9`.

Do not rewrite the astronomy engine or redesign the UI.

## Remaining interaction-heavy work

1. **True mobile multitouch sphere behavior**
   - one-finger horizontal rotates
   - one-finger vertical scrolls page
   - two-finger gesture is owned by the sphere
   - pinch zoom with restrained bounds
   - midpoint movement rotates
   - clean transition when one finger lifts
   - pointercancel cleanup
   - no page drag or browser pinch takeover while two fingers are on the sphere

2. **Bridge interaction choreography**
   - Bridge acts as an explicit mode
   - sphere becomes focus
   - SELECT BODY 1 then SELECT BODY 2
   - selected bodies get numbered markers and restrained glow
   - meaningful aspect candidates are emphasized
   - unlinked bodies are subdued
   - valid pair briefly pulses the relationship line before reading
   - tapping Bridge again cancels the mode cleanly

3. **Synth interaction choreography**
   - SELECT BODY 1, 2, 3
   - numbered markers and restrained glow
   - illuminate existing relationship geometry
   - if the current engine already detects a three-body pattern, emphasize that existing pattern
   - tapping Synth again cancels cleanly

4. **Signature Scan first-resolve overview**
   - 4 to 6 deterministic highlights max
   - major existing patterns first, then tightest major aspects, then Sun/Moon/ASC/MC relationships as needed
   - dim irrelevant geometry
   - concise explanation per stop
   - NEXT and SKIP OVERVIEW always available
   - finish by smoothly revealing the full Field
   - no new astrology scoring system

5. **Bake release patches into `app.html`**
   - remove the temporary loader string-patching architecture once behavior is verified
   - `app.html` itself must become the canonical public build
   - preserve the behavior currently added by `index.html`

## Mandatory regression targets

- 390x844 portrait
- 430x932 portrait
- 844x390 landscape
- 1024x768
- 1100x800
- 1180px wide
- `Mora & Co` Signature SVG + PNG
- Tromso/high-latitude Placidus fallback label
- unsupported city after selecting London
- local evening date
- keyboard tab through sphere into reading controls
- prefers-reduced-motion
- zero console errors

Do not merge to `main` until these interaction tests pass.
