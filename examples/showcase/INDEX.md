# Showcase renders

Flat collection of finished UGC mp4s — one per project. These are the **final cuts** shipped from a real working session in the `ugc-cli` repo, used as visual reference for what the pipeline can produce end-to-end. Each is 9:16 vertical, 1080×1920, 30fps, h264 + aac.

To pull one locally:

```bash
curl -L -o ./showcase-flipper.mp4 \
  https://raw.githubusercontent.com/alecs5am/ralphy-assets/main/examples/showcase/flipper-hypermotion-001.mp4
```

Or list / download via the CLI once the showcase entries hit `manifest.json`.

## Renders

| Project | Duration | Size | Style / Template | One-liner |
|---|---|---|---|---|
| [`glitter-cream-001.mp4`](glitter-cream-001.mp4) | 17.2s | 23 MB | UGC selfie (kling-v3.0-pro i2v) | Fluttershy-branded glitter-cream jar review — locked product + model master-shots passed as `--ref` on every gen to kill identity drift between scenes. |
| [`flipper-hypermotion-001.mp4`](flipper-hypermotion-001.mp4) | 15.1s | 26 MB | Japanese hyper-motion ad | Flipper Zero ad — 8 cuts at ~1.9s avg, orchestral-hyperpop with drum-fill stings, chibi orbit on tile-grid floor, ends on katakana slogan 「ハック・スベテヲ」. |
| [`occult-mockumentary-001.mp4`](occult-mockumentary-001.mp4) | 32.8s | 41 MB | Found-footage horror (voidstomper) | First-person handheld occult mockumentary — cult-circle discovery + monster turn, REC dot + timecode overlay, diegetic EN captions, no VO. seedance-2.0 for non-default physics motion. |
| [`analog-horror-fridge-001.mp4`](analog-horror-fridge-001.mp4) | 29.5s | 29 MB | Analog-horror PSA | "Compliance Bulletin 9-D — Your Dog Is Not Your Dog" — cold ElevenLabs robo-female VO, IF / DO-NOT / BUT / AND beat structure, glitchx font, yellow pixelated PSA icons, no music. Compressed from 188 MB → 29 MB via CRF 30. |
| [`nothing-hp1-001.mp4`](nothing-hp1-001.mp4) | 54.1s | 48 MB | Multi-scene product ad | Nothing HP1 headphones launch ad — 27 scenes, identity-locked wardrobe / hair refs propagated across all i2v batches, full UGC-style product reveal. |

## Provenance

All five projects are in `ugc-cli` under `workspace/projects/<id>/` with their full asset trees, gen-logs, scenarios, and prompts. The mp4s here are the **canonical final** picked at the end of each working session. Sizes are kept ≤ 50 MB so files stream from `raw.githubusercontent.com` directly (under GitHub's 100 MB raw limit, no Release upload needed).
