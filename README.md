# ralphy-assets

Asset and example repository for [`ralphy`](https://github.com/alecs5am/ugc-cli) (the UGC video generation CLI).

This repo exists so the main `ugc-cli` repo stays small. Heavy required assets (template trend music, reference videos) and complete example projects (full asset trees, render outputs, generation logs) live here and are pulled on demand.

## Layout

```
ralphy-assets/
├── manifest.json              # index of everything available
├── required/                  # template-required assets (small, hosted as raw URLs)
│   └── <template-slug>/
│       └── <file>
└── examples/                  # complete reference projects (can be heavy — uses GitHub Releases for >100MB)
    └── <example-id>/
        ├── BRIEF.md
        ├── scenario.json
        ├── prompts.json
        ├── asset-manifest.json
        ├── assets/{images,videos,voiceover,music,uploaded}/
        ├── render/final.mp4
        └── logs/{generations,user-prompts,user-assets}.jsonl
```

## How it's pulled

From the main `ugc-cli` workspace:

```bash
ralphy assets list                              # show what's available
ralphy assets pull soviet-nostalgic             # pull the soviet-nostalgic required bundle
ralphy assets pull --template <slug>            # pull only what a template needs
ralphy assets clean                              # wipe local cache

ralphy example list                              # show available example projects
ralphy example pull <id> --as <new-project-id>  # download a full example into workspace/projects/
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

## Manifest schema

```jsonc
{
  "version": 1,
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
  "examples": {
    "<id>": {
      "template": "<template-slug>",
      "manifest": "examples/<id>/asset-manifest.json",
      "sizeBytes": 47000000,
      "sha256": "<sha256 of a tarball>",
      "tarball": "examples/<id>.tar.gz",  // OR via release
      "via": "raw" | "release",
      "releaseTag": "v1.0.0",
      "description": "human-readable"
    }
  }
}
```
