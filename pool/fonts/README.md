# VCR-JP

A pixel-perfect VHS / CRT On-Screen-Display typeface for retro Japanese
HUD overlays — clock readouts, "REC" indicators, containment-card mockups,
analog-horror titles, found-footage subtitles.

| Field | Value |
|---|---|
| Designer | Haley Wakamatsu (UkiyoMoji Fonts / @japanyoshi) |
| Built with | [FontStruct](https://fontstruct.com/fontstructions/show/2100555/vcr-jp) |
| Year | 2022 |
| Style | Pixel bitmap upscaled via Mullard SAA5050 process |
| License | **FontStruct Non-Commercial License** |

> **License caution.** The font is free for non-commercial use only. It
> ships in this repo so dev tooling, internal demos, and personal
> portfolio renders can reach it directly. Any output produced with this
> font that ships in paid / sponsored / monetized work needs separate
> permission from the author — contact via the FontStruct profile above.
> Do NOT bake this font into a default template that ralphy applies to
> user content without flagging the license in the template's
> `template.yaml` `attribution` block.

## Codepoint coverage (the non-obvious part)

The font name implies "Japanese support" but it does **not** carry the
standard Unicode Katakana block (U+30A0–U+30FF) or Hiragana
(U+3040–U+309F). It carries the **Halfwidth Katakana** block
(U+FF61–U+FF9F) — the narrow mono-spaced kana that VHS subtitle hardware
and old computer terminals actually shipped with. This is the authentic
register for a "found-tape" aesthetic; standard fullwidth kana would
read as a modern smartphone font.

Practical coverage:

| Block | Range | Coverage |
|---|---|---|
| Basic Latin (ASCII) | U+0020–U+007E | full |
| Latin-1 symbols (¥ × ÷) | U+00A5 / U+00D7 / U+00F7 | yes |
| Smart quotes | U+2018–U+201F | yes |
| Bullets | U+2022–U+2023 | yes |
| Block elements (▀ ▁ … ▟) | U+2580–U+259F | full |
| Halfwidth katakana (ｱｲｳｴｵ … ﾜｦﾝ) | U+FF61–U+FF9F | full |
| Halfwidth arrows | U+FFE9–U+FFEE | yes |
| Kanji | — | **only 11**: 万 円 千 土 年 日 月 木 水 火 金 (numerals, date/time markers, five elements) |

If you write fullwidth kana (`アベル`, `収容違反`), every glyph falls
back to `.notdef` and renders as a tofu box. Always convert to halfwidth
before passing the string to a renderer.

## Halfwidth conversion cheat sheet

| Fullwidth | Halfwidth | Reading | English |
|---|---|---|---|
| アベル | ｱﾍﾞﾙ | aberu | Abel (SCP-076-2's name) |
| 隔離対象 | ｶｸﾘﾀｲｼｮｳ | kakuri taishou | containment subject |
| 収容違反 | ｼｭｳﾖｳ ｲﾊﾝ | shuuyou ihan | containment breach |
| 極秘 | ｺﾞｸﾋ | gokuhi | top secret |
| 機密 | ｷﾐﾂ | kimitsu | classified |
| 不明 | ﾌﾒｲ | fumei | unknown |
| 危険 | ｷｹﾝ | kiken | danger |
| 録画中 | ﾛｸｶﾞﾁｭｳ | rokuga chuu | recording |
| 警告 | ｹｲｺｸ | keikoku | warning |
| 消滅 | ｼｮｳﾒﾂ | shoumetsu | vanish / extinguish |
| 観測 | ｶﾝｿｸ | kansoku | observation |
| 該当者なし | ｶﾞｲﾄｳｼｬ ﾅｼ | gaitousha nashi | no subject identified |

Voiced kana (ガ / ベ / etc.) become a base glyph + dakuten (ﾞ U+FF9E)
or handakuten (ﾟ U+FF9F): `ベ` → `ﾍ` + `ﾞ`. The font renders the marks
as separate characters next to the base — this is correct period
behavior, not a bug.

## ffmpeg drawtext usage

The font path must be **single-quoted inside** the filter string,
otherwise ffmpeg's parser eats the slashes when the path is bash-expanded:

```bash
FONT="$(pwd)/assets/fonts/vcr-jp/vcr-jp.ttf"

ffmpeg -i input.jpg \
  -vf "drawtext=fontfile='$FONT':text='ｼｭｳﾖｳ ｲﾊﾝ':fontcolor=0xFF3030:fontsize=160:x=(w-text_w)/2:y=h*0.7" \
  -q:v 2 output.jpg
```

Notes:

- **Single-quote the variable**: `fontfile='$FONT'` (not `fontfile=$FONT`).
  Bash expands `$FONT` inside the single quotes; ffmpeg sees a quoted
  path string and parses it as one option value instead of splitting on `/`.
- **Colons in the text string** need backslash-escape: `text='03\:47\:21'`.
- **Pixel scaling**: the font is a bitmap upscaled to a TTF outline. To
  preserve the chunky look, prefer `fontsize=160+` and skip `boxblur`
  *before* the drawtext step; let blur run *after* so it picks up the
  pixel edges. With the project's VHS chain (rgbashift → eq → boxblur
  → noise → drawgrid → vignette), draw text **first** so it inherits the
  same degradation.
- **Color picks**: yellow `0xF0E14C` for HUD chrome, red `0xFF3030` for
  warning / REC indicators. Avoid pure white — too clean for the register.

## Worked example

`workspace/projects/opium-pfp-001/assets/vhs/vhs-info-jp-breach.jpg` is
the canonical reference render: red `ｼｭｳﾖｳ ｲﾊﾝ` (containment breach)
centred under a yellow halfwidth-style data card, both passing through
the project's VHS degradation chain. The pixel font's hard edges are
what makes the chromatic-aberration + scanline pipeline read as
authentic VHS instead of "rendered". Smooth vector fonts (Hiragino,
Noto) survive the same pipeline but read as modern-with-filter, not
period.

## Picking a sibling font

For Latin overlays in the same composition, pair this with
**VT323** (the canonical CRT terminal font, OFL-licensed, used by the
`analog-horror-fridge-001` project). The two fonts share the same
8×8-ish bitmap heritage and align visually when stacked.

For longer fullwidth Japanese passages (when 漢字 are unavoidable),
fall back to a system Japanese font (`/System/Library/Fonts/Hiragino
Sans GB.ttc` on macOS) and accept the modern register — there is no
free pixel font that covers full kana + kanji at VCR-JP's quality bar.

## See also

- `notes/ideas/003-anti-ai-slop-photoreal-flash.md` — the anti-AI-slop
  photoreal register that this font's degradation pipeline pairs with.
- `workspace/projects/analog-horror-fridge-001/` — sibling project using
  VT323 for Latin overlays in the same VHS register.
