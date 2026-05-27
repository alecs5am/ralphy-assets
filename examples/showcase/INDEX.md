# Showcase renders

Flat collection of finished UGC mp4s — one per project. These are the **final cuts** shipped from a real working session in the `ugc-cli` repo, used as visual reference for what the pipeline can produce end-to-end. Aspect ratios vary by template (9:16 vertical default, 1:1 square for broadcast trends, 16:9 horizontal for cinematic / spiderverse / tokyo-y2k), h264 + aac.

To pull one locally:

```bash
curl -L -o ./showcase-flipper.mp4 \
  https://raw.githubusercontent.com/alecs5am/ralphy-assets/main/examples/showcase/flipper-hypermotion-001.mp4
```

Or list / download via the CLI once the showcase entries hit `manifest.json`.

## Renders

| Project | Aspect | Duration | Size | Style / Template | One-liner |
|---|---|---|---|---|---|
| [`glitter-cream-001.mp4`](glitter-cream-001.mp4) | 9:16 | 17.2s | 8.3 MB | UGC selfie (kling-v3.0-pro i2v) | Fluttershy-branded glitter-cream jar review — locked product + model master-shots passed as `--ref` on every gen to kill identity drift between scenes. |
| [`flipper-hypermotion-001.mp4`](flipper-hypermotion-001.mp4) | 9:16 | 15.1s | 11.1 MB | Japanese hyper-motion ad | Flipper Zero ad — 8 cuts at ~1.9s avg, orchestral-hyperpop with drum-fill stings, chibi orbit on tile-grid floor, ends on katakana slogan 「ハック・スベテヲ」. |
| [`occult-mockumentary-001.mp4`](occult-mockumentary-001.mp4) | 9:16 | 32.8s | 15.5 MB | Found-footage horror (voidstomper) | First-person handheld occult mockumentary — cult-circle discovery + monster turn, REC dot + timecode overlay, diegetic EN captions, no VO. seedance-2.0 for non-default physics motion. |
| [`analog-horror-fridge-001.mp4`](analog-horror-fridge-001.mp4) | 9:16 | 29.5s | 28.0 MB | Analog-horror PSA | "Compliance Bulletin 9-D — Your Dog Is Not Your Dog" — cold ElevenLabs robo-female VO, IF / DO-NOT / BUT / AND beat structure, glitchx font, yellow pixelated PSA icons, no music. Already CRF-30 compressed from 188 MB master — kept as-is. |
| [`nothing-hp1-001.mp4`](nothing-hp1-001.mp4) | 9:16 | 54.1s | 15.6 MB | Multi-scene product ad | Nothing HP1 headphones launch ad — 27 scenes, identity-locked wardrobe / hair refs propagated across all i2v batches, full UGC-style product reveal. |
| [`playdate-pixel-001.mp4`](playdate-pixel-001.mp4) | 9:16 | 16.4s | 16.8 MB | Pixel-art product reveal | Playdate (Panic Inc.) yellow-brick handheld console — chunky 8-bit pixel-art aesthetic, hand-held tilt + crank gag, ends on the console silhouette over a yellow chroma slab. |
| [`kbo-broadcast-001.mp4`](kbo-broadcast-001.mp4) | 1:1 | 15.0s | 3.1 MB | Broadcast-cam realism (square) | Korean Baseball Organization "caught-on-TV" trend — 1:1 square framing to match real-broadcast 16:9 fov inside vertical feeds, audience-cam crowd reaction beat, gpt-5.4-image-2 → Kling i2v. |
| [`noski-people-001.mp4`](noski-people-001.mp4) | 9:16 | 73.9s | 8.9 MB | Photoreal lifestyle ensemble | Hyperreal-not-glossy still-photo register — Sony A7 IV + Sigma 35/85mm, Kodak Portra 400 ambient single-source light, identity-locked across ~10 anchor shots. Final v4. |
| [`skater-spiderverse-001.mp4`](skater-spiderverse-001.mp4) | 16:9 | 15.1s | 10.3 MB | Spiderverse skate duel | Comic-panel split + halftone dot bleed, two skater silhouettes mid-trick over a SF rooftop, ink-stroke captions on the impact frame. seedance-2.0 for the kickflip physics. |
| [`tokyo-y2k-001.mp4`](tokyo-y2k-001.mp4) | 16:9 | 75.0s | 26.4 MB | Tokyo Y2K cinematic | Long-form 75s tokyo-night y2k cinematic — neon-soaked alley dollies, rain-on-glass macro, JP signage typography slams, 16:9 letterboxed final cut. |
| [`fruit-drama-001.mp4`](fruit-drama-001.mp4) | 9:16 | 56.0s | 23.7 MB | Fruit anthropomorphic drama | Talking-fruit telenovela parody — face-on-fruit, popping-word captions, two-stem music bed (intro + climax), shot beat-locked to dialogue scaffold. |
| [`arena-rocker-001.mp4`](arena-rocker-001.mp4) | 16:9 | 30.1s | 30.8 MB | Toon-action (seedance-2.0 t2v) | Original glam arena rocker "Vex Calloway" — painterly Spider-Verse/Arcane homage. Two 15s blocks i2v-chained on the last frame: entrance from smoke → fretboard shred + pyro → leap over the crowd → 360° camera orbit revealing the stadium → Rammstein-style pyro wall + ink "SOLD OUT". Music banned inside seedance (diegetic SFX only), instrumental rock bed mixed in post with sidechain duck. |

## Provenance

All twelve projects are in `ugc-cli` under `workspace/projects/<id>/` with their full asset trees, gen-logs, scenarios, and prompts. The mp4s here are the **canonical final** picked at the end of each working session. All re-encoded at `libx264 preset=slow CRF≈20-24, aac 128k, +faststart` (visually-transparent recompression) — sizes stay well under GitHub's 100 MB raw limit so files stream from `raw.githubusercontent.com` directly without Release upload.
