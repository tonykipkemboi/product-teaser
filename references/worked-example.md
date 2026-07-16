# Worked example: the Rekody "Someday" teaser

This is the real build the skill was distilled from, included as a concrete reference so you can see the clear path applied end to end. Rekody is a free, privacy-first, on-device voice dictation app for macOS. The teaser shows the product performing its magic live, opens on a human typing-struggle, and closes on a call to action. You can see the finished result at [rekody.com](https://rekody.com).

## Project shape

A standard HyperFrames project:

- **`index.html`** — the root timeline and the media hub. All `<video>` and `<audio>` live here (media must be a direct child of the host root).
- **`compositions/beatN-*.html`** — one sub-composition per beat, each with a paused GSAP timeline registered on `window.__timelines`.
- **`assets/{bgm,voice,sfx,footage}/`** — the baked music mix, the voice lines, the SFX, and the graded opener footage.
- **`fonts/`** — the brand fonts bundled as `.woff2` (no network fonts at render time).
- **`renders/`** — the output MP4 (1920x1080, ~30s, h264 + aac), plus the 9:16 and 1:1 platform cuts.
- **UI source of truth (fidelity):** the app's real view code. Exact metrics, colors, and states were read from it and rebuilt in HTML/CSS. Rebuild from whatever your product's real UI source is (a SwiftUI view, a React component, a Figma frame).

## Beat map (absolute seconds, on the 104.17 BPM funk grid)

Grid: beat 0.5769s, bar 2.3077s, count-in transients 1.083 / 1.660, **drop ("the one") 3.968**.

| Beat | Window | What happens | Key hits on the grid |
| --- | --- | --- | --- |
| 0 · Opener | 0 → 3.968 | Message-first human hook: real footage of hands thumb-typing a text (the pain), fading up from white, graded into the brand world. The line "Easier said than typed" reveals word-by-word on the count-in; its period is the brand teal dot. On the drop, a teal-white bloom takes over. | words on 1.083 / 1.660 / 2.237; dot 2.55; bloom into the drop 3.968 |
| 2 · Hero (Messages) | 3.968 → 10.025 | The armed pill snaps in on the drop; the voice speaks; the pill tail reveals the transcript. Mid-dictation a grab-hand lifts the pill up off the compose box (real draggable behavior). On stop the full text pastes into the field, a cursor clicks Send, pop. | pill snap 3.968; grab 5.987; paste 8.583; send 9.160 |
| 4 · Cross-app (Notes) | 10.025 → 16.371 | Pill already moved; second line dictated and pasted into a Notes doc. Same real flow. | paste 14.929 |
| 5 · Privacy | 16.371 → 20.698 | Wi-Fi to airplane, teal shield, "your voice never leaves your Mac." The groove keeps playing through (no silent break). | hard cut to lockup 20.698 |
| 6 · Lockup → CTA | 20.698 → 30.0 | The teal dot drops and bounces onto the baseline as the period of "rekody."; "you speak. rekody types." settles; a funk button accents; after a ~2s linger the lockup resolves cinematically into the end card: the wordmark travels up and shrinks, and the CTA rises in ("Download it free" + a solid teal `rekody.com` button). Music grooves under it, then fades to the end. | dot contact 21.852; button 24.159; CTA resolve ~25.0; music fade 28.5 → 30.0 |

## Architecture notes

- **Beats are sub-compositions**, each with a paused GSAP timeline on `window.__timelines`.
- **The opener footage is a root-level `<video>`** in `index.html`. Its scale drift and drop push are tweened on the **root** timeline. The opener beat sub-composition is transparent and paints the grade, the line, and the bloom over the footage; the teal wash uses normal compositing (a low-opacity overlay, not `mix-blend-mode`, so it tints the root-level video below).
- **All audio is at the root**: one continuous music cue (baked envelope), two voice lines, a few pops, one dot-landing sound.
- **The CTA lives inside the final lockup composition** so the wordmark can *travel* into the end card instead of a hard cut. The end fade came from an extended baked mix, spliced from the approved mix so nothing before the CTA changed.

## Platform cuts (9:16 and 1:1)

The 16:9 master serves landscape (X, LinkedIn, YouTube). Vertical and square were built as extra entry files in the same project (`index-vertical.html`, `index-square.html`) and rendered with `hyperframes render . -c <file>`, reusing every composition and asset. The treatment: the real 16:9 scenes scaled to full width and floated in a branded card with a persistent wordmark and muted-readable captions (social autoplays silent), and the ending plays full-frame native (a portrait clone of the lockup) so the CTA button stays big and readable.

## How to reuse this as a template

Keep the shape: `index.html` as the root timeline and media hub, one sub-composition per beat, a `fonts/` folder of bundled `.woff2`, and `assets/{bgm,voice,sfx,footage}/`. Swap the UI recreation for your product's (read *its* real source), re-derive the beat grid for your track, and keep the media-at-root, baked-audio, and fidelity rules.
