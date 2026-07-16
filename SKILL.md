---
name: product-teaser
description: "Build a cinematic, product-accurate, beat-synced brand/product teaser (~20-40s) in HyperFrames by recreating the REAL product UI pixel-faithfully and animating it to a licensed music track, with a real human voice, a message-first opener, and a call-to-action close. Use when the ask is a 'brand teaser', 'product teaser', 'cinematic teaser', a hype reveal, or 'recreate my app's UI in a promo and sync it to music' where UI fidelity and music sync matter more than a stock-footage montage. Distinct from product-launch-video (which uses captured screenshots): here you REBUILD the interface in HTML/GSAP so it performs live. A clean, repeatable clear path from product UI to finished teaser."
---

# Product Teaser (cinematic, pixel-faithful, beat-synced)

Make a short brand teaser where the product performs its magic **live** on screen: the real UI, rebuilt pixel-faithfully in HTML/GSAP, animated to a licensed music track, carried by a real human voice, opening on a human moment and closing on a clear call to action. `references/worked-example.md` is a real finished build you can copy as a template.

This skill sits **on top of HyperFrames**. Use `/hyperframes` and its domain skills (`/hyperframes-core`, `/hyperframes-animation`, `/hyperframes-cli`, `/media-use`) for framework mechanics. This skill is the teaser-specific clear path.

## When to use this vs product-launch-video

- **This skill**: you want the product's UI to *act* on screen (a control responding, text appearing, a state changing), beat-synced to music, emotionally framed. You have, or can read, the **real UI source** and will rebuild it faithfully. Short (20-40s), teaser energy, one clear feeling.
- **`/product-launch-video`**: a 60-90s promo built from captured screenshots / site assets and narration. No live UI recreation.
- Unsure → read `/hyperframes` and pick from there.

---

## The clear path

Run these phases in order; each ends with a gate. Do not start animating until Phase 1 (product truth) is locked. The biggest quality lever is **fidelity and flow accuracy**, not motion.

### Phase 0: Foundations

Gather all five before Phase 1:

1. **The product UI source of truth**: the actual view code / design file for the interface you will show (a SwiftUI `View`, a React component, a Figma frame). You rebuild from this.
2. **A licensed music track** (or one you generate). The music drives the edit. Confirm the license.
3. **A real voice**: the founder's own voice (ElevenLabs), retrieved from a secret manager per call, never hardcoded. TTS reads as "AI made this."
4. **Brand tokens + fonts**, bundled locally in the project (`.woff2` in `fonts/`, exact hex, exact type scale). No network fonts at render time.
5. **A HyperFrames project**: `npx hyperframes init "videos/<project>" --non-interactive`.

**Gate:** all five in hand; you can point at the real UI source file.

### Phase 1: Product truth (fidelity and flow)

1. **Recreate the UI pixel-faithful from the real source.** Extract exact metrics, colors, fonts, and the exact set of states, and rebuild the component in HTML/CSS to match. Do not approximate or invent states.
2. **Match the real product flow.** Storyboard what the product actually does, step by step, and reproduce it exactly. Get the mechanic right before styling it.
3. **The owner is ground truth, even over the code.** If the shipping product does not visibly do something the code can, do not show it. Marketing reflects real, normal use.
4. **Do a fidelity check** after each build: re-read the real source against your recreation (states, colors, metrics, flow).

**Gate:** the recreation matches the source and the on-screen flow matches what the product truly does.

### Phase 2: Story to a music grid (beat-sync)

1. **Derive the beat grid from the track.** Decode to mono 8kHz, compute a positive-diff spectral-flux onset envelope, autocorrelate for tempo. Get BPM, beat, bar, the count-in transients, and the drop ("the one"). Lock those numbers.
2. **Storyboard the beats** against the grid: open cold on the count-in, drop the product on "the one," land every paste / cut / landing on a real beat, and cut dead air between beats.

**Gate:** a written beat map with absolute timestamps, each key moment tied to a beat, the drop identified.

### Phase 3: Build the architecture

1. **Beats are sub-compositions** (`data-composition-src="compositions/beatN.html"`), each with a paused GSAP timeline registered on `window.__timelines["<composition-id>"]`.
2. **Put media at the host root.** `<video>` and `<audio>` are direct children of `index.html`, never inside a sub-composition (media inside a sub-comp renders black / silent). Drive any per-scene media motion (a video's scale) on the **root** timeline.
3. **Paint full-bleed backgrounds on a `class="clip"` child**, not on `#root`.
4. Every timed element carries `class="clip"` + `data-start` + `data-duration` + `data-track-index`. No render-time clocks / random / network.

**Gate:** `npx hyperframes check` runs; media is at the root.

### Phase 4: Voice, SFX, and the mix

1. **The real voice drives the reveal.** Transcribe the VO for word timings (`npx hyperframes transcribe`) and reveal on-screen text to them.
2. **SFX minimal and on-brand.** No keystroke sounds for a voice-first product; cut any chime/sparkle the owner dislikes. One soft confirm on a real action is enough.
3. **Bake the music volume envelope into the audio file.** Duck under the voice, hold the groove elsewhere, fade at the end, and render that gain permanently into the `.mp3`. Reference it at `data-volume="1"` with no volume tween on the music. Fade at the true end rather than hard-stopping mid-groove.

**Gate:** the voice sits clearly above the music in the exported file.

### Phase 5: The bookends

1. **Opener: a message-first human hook, not a logo.** Open on a real human moment that names the problem, synced to the count-in, then hand off to the product on the drop (problem → relief). Use commercial-safe stock (Pexels), downscaled and graded into the brand world. Study intros you admire for register; never copy them.
2. **CTA close: resolve cinematically to an end card.** After the final brand moment lands and lingers a beat, transform into a clean end card with the action: a short invitation and a single saturated button carrying the URL. Build it inside the final composition so brand elements can travel (the wordmark scales down and rises to anchor the card) rather than hard-cutting. Music grooves under it, then fades at the true end.

**Gate:** the first ~4 seconds hook on a human truth and read the product; the last frames say exactly what to do next.

### Phase 6: QA, render, verify

1. `npx hyperframes check` → 0 errors. (Muted-grey UI-mock text may trip contrast *warnings*; that is intentional realism, not a blocker.)
2. **Snapshot key moments and read them:** `npx hyperframes snapshot --at <t1,t2,...> --no-end`; zoom into details with `--zoom "x,y,w,h"`.
3. **Render, then verify in the encoded MP4**, not the preview: extract frames across the opener, a mid beat, and the close; confirm no black media and no regressions. Measure the audio mix (`ffmpeg ... -af "atrim=A:B,volumedetect"`).
4. Deliver: open it locally and/or send the file for review.

**Gate:** encoded frames confirm every beat; audio verified; duration reported.

---

## Brand hygiene

Marketing reflects real, normal use and the brand's voice. Keep the owner's non-negotiables on a short list and check every frame against it: keep the accent color from drifting to a neighbor, do not show internal-only indicators, keep name casing and punctuation rules consistent, keep any "free" claim honest. Capture the list once and enforce it.

## Proof and sharing

`references/worked-example.md` is a real finished build (the Rekody "Someday" teaser): the beat map, the file layout, the timings, and where the rendered result lives. To share the skill, share this `product-teaser/` folder; it is self-contained method with no secrets.
