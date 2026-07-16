# product-teaser

A [Claude Code](https://claude.com/claude-code) skill for building **cinematic, pixel-faithful, beat-synced product teasers** in [HyperFrames](https://hyperframes.heygen.com). Instead of cropping screenshots into a video editor, you rebuild your product's real UI in HTML/GSAP so it performs live, sync it to a music track and a real voice, open on a human moment, and close on a call to action.

It is the method behind the Rekody "Someday" teaser. You can see the finished result at [rekody.com](https://rekody.com).

## Watch

The video below was built entirely with this skill: the product UI rebuilt in HTML/GSAP, beat-synced to music and a real voice, opening on a human moment and closing on a CTA. Press play.

<video src="https://github.com/tonykipkemboi/product-teaser/raw/main/media/someday-teaser.mp4" poster="https://github.com/tonykipkemboi/product-teaser/raw/main/media/poster.jpg" controls width="720"></video>

If the player does not load, [watch the file directly](https://github.com/tonykipkemboi/product-teaser/raw/main/media/someday-teaser.mp4).

## What it is

A clean, repeatable *clear path* from product UI to finished teaser, plus one worked example as a reference. It is method, not code you run: the skill guides an agent (or you) through the phases and flags the framework gotchas that would otherwise ship a broken render.

The path, in six phases:

1. **Foundations** — the real UI source, a licensed track, a real voice, brand tokens and fonts, a HyperFrames project.
2. **Product truth** — rebuild the UI pixel-faithful from the real source; match the real flow; the owner is ground truth even over the code.
3. **Story to a music grid** — derive the beat grid (onset + autocorrelation); drop the product on "the one."
4. **Architecture** — beats as sub-compositions; media at the host root; backgrounds on a clip child.
5. **Voice, SFX, and the mix** — the real voice drives the reveal; bake the music volume envelope into the file.
6. **The bookends** — a message-first human opener; a cinematic CTA close.
7. **QA and verify** — check, snapshot, render, then verify in the encoded MP4, not the preview.

Full detail in [`SKILL.md`](SKILL.md); the concrete build is in [`references/worked-example.md`](references/worked-example.md).

## Install

This is a Claude Code skill. Clone it into your skills directory:

```bash
git clone https://github.com/tonykipkemboi/product-teaser.git ~/.claude/skills/product-teaser
```

Restart your Claude Code session so the skill loads.

## Use

Type `/product-teaser`, or just ask for a product teaser and Claude will route to it. It is product-agnostic: point it at any app's real UI source, a track, and a voice.

## What it assumes

HyperFrames (a headless-browser + GSAP + ffmpeg renderer) for the build. A real voice (for example ElevenLabs) rather than robotic TTS. A licensed music track. Commercial-safe stock (for example Pexels) for any human footage. Your product's real UI source as the fidelity reference.

## License

MIT. See [`LICENSE`](LICENSE).

---

Built by [Tony Kipkemboi](https://rekody.com). If you make something with it, that is the whole point.
