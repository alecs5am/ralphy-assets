# ralphy-assets

Asset and example repository for [`ralphy`](https://github.com/alecs5am/ugc-cli) (the UGC video generation CLI).

This repo exists so the main `ugc-cli` repo stays small. Heavy required assets (template trend music, reference videos) and complete example projects (full asset trees, render outputs, generation logs) live here and are pulled on demand.

## Layout

```
ralphy-assets/
├── manifest.json              # index of everything available (schema v2)
├── required/                  # template-required assets (auto-pulled on `ralphy template use <slug>`)
│   └── <template-slug>/
│       └── <file>
├── pool/                      # generic, multi-template asset pools — grouped by KIND
│   └── <kind>/                # e.g. italian-brainrot-characters, gameplay-loops, trend-music
│       ├── <slug>.<ext>
│       ├── INDEX.md
│       └── ATTRIBUTION.md     # required when items have non-trivial license
└── examples/                  # complete reference projects (heavy — GitHub Releases for >100MB)
    └── <example-id>/
        ├── BRIEF.md
        ├── scenario.json
        ├── prompts.json
        ├── asset-manifest.json
        ├── assets/{images,videos,voiceover,music,uploaded}/
        ├── render/final.mp4
        └── logs/{generations,user-prompts,user-assets}.jsonl
```

### Pool kinds (extensible)

The `pool/` layer is open-ended — adding a new kind is just adding a new key in `manifest.json:pool`. Code in the CLI doesn't need to change.

| Kind | Description | Status |
|---|---|---|
| `italian-brainrot-characters` | 33 canonical Italian Brainrot meme characters (Tralalero Tralala, Tung Tung Sahur, Ballerina Cappuccina, etc.) | ✅ populated |
| `gameplay-loops` | CC0 hypnotic-motion gameplay for brainrot templates (CS surf, Minecraft parkour, kinetic sand) | 🟡 planned |
| `trend-music` | Cleared / CC0 TikTok trend audio beds | 🟡 planned |
| `stock-broll` | Generic CC0 b-roll | 🟡 planned |
| _future kinds_ | sfx, transitions, fonts, overlays — add by creating a new pool key in manifest.json | — |

## How it's pulled

From the main `ugc-cli` workspace:

```bash
ralphy assets list                                          # show everything (required + pool + examples)
ralphy assets list --kind italian-brainrot-characters       # show one pool kind
ralphy assets list --template italian-brainrot              # show required + matching pool items
ralphy assets pull <template-slug>                          # pull the template's required bundle
ralphy assets pull-pool <kind>/<slug>                       # pull a single pool item
ralphy assets pull-pool <kind>/<slug> --install <project>   # pull + copy into project tree
ralphy assets catalog --write                               # regen docs/assets-catalog.md in ugc-cli
ralphy assets clean                                          # wipe local cache

ralphy example list                                          # show available example projects
ralphy example pull <id> --as <new-project-id>              # download a full example
```

Cache lives at `workspace/.ralph/asset-cache/` (per-workspace, gitignored).

## Hosting

- **`required/` assets** — fetched via `https://raw.githubusercontent.com/alecs5am/ralphy-assets/main/...` (free, fast, 100MB max per file).
- **`examples/` bundles** — large bundles (mp4 renders, hi-res keyframes) live in [GitHub Releases](https://github.com/alecs5am/ralphy-assets/releases) (2GB max per file). The `manifest.json` entry has `via: "release"` and a `releaseTag` for these.

## SHA-256 verification

Every entry in `manifest.json` has a `sha256` field. The CLI verifies the digest after download and refuses to use a corrupted file.

## Contributing an example

```bash
# in the main ugc-cli workspace, after a successful project ships:
ralphy example push <project-id> --to ../ralphy-assets/examples/<new-id>
```

This copies the project tree (skipping ephemera per `cli/commands/profile.ts` rules), updates `manifest.json`, and stages the assets in this repo for a manual commit + PR.

## Manifest schema (v2)

```jsonc
{
  "version": 2,
  "updated": "<iso-8601>",
  "baseUrl": "https://raw.githubusercontent.com/alecs5am/ralphy-assets/main",
  "releaseBaseUrl": "https://github.com/alecs5am/ralphy-assets/releases/download",

  "required": {
    "<key>": {
      "template": "<template-slug>",
      "path": "required/<...>",          // relative to baseUrl
      "destSubdir": "assets/music",       // where it goes in the project tree
      "sizeBytes": 983040,
      "sha256": "...",
      "description": "human-readable",
      "via": "raw" | "release",
      "releaseTag": "v1.0.0"              // only when via="release"
    }
  },

  "pool": {
    "<kind>": {                            // open-ended slug; new kinds = new keys, no code change
      "description": "human-readable, what's in this kind",
      "license": "<default-license>",      // optional; can be overridden per-item
      "attributionRequired": true,         // if true, this dir should have ATTRIBUTION.md
      "defaultDestSubdir": "assets/references/characters",
      "items": {
        "<item-slug>": {
          "path": "pool/<kind>/<file>",
          "sizeBytes": 87000,
          "sha256": "...",
          "description": "what this specific item is",
          "license": "...",                // overrides kind default
          "attribution": "...",
          "sourceUrl": "<original source>",
          "worksWith": ["<template-slug>"], // empty/absent = usable by any template
          "tags": ["character", "tiktok"],
          "destSubdir": "assets/uploaded",  // overrides kind default
          "destFilename": "<rename>",       // overrides basename(path)
          "via": "raw" | "release",
          "releaseTag": "v1.0.0"
        }
      }
    }
  },

  "examples": {
    "<id>": {
      "template": "<template-slug>",
      "manifest": "examples/<id>/asset-manifest.json",
      "sizeBytes": 47000000,
      "sha256": "<sha256 of a tarball>",
      "tarball": "examples/<id>.tar.gz",
      "via": "raw" | "release",
      "releaseTag": "v1.0.0",
      "description": "human-readable"
    }
  }
}
```

**Back-compat:** v1 manifests are still accepted by the ugc-cli loader (`pool` synthesized as empty `{}`). Upgrading the manifest to v2 unlocks the new `pool` layer; the CLI ≥ v2-aware version reads both.

author alecs5am
