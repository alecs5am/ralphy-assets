# Fonts pool — attribution

Every font in `pool/fonts/` ships with its license preserved. The list
below is the source of truth for redistribution and downstream use. If
you add a new font, add a row here in the same shape and update
`manifest.json`.

## VCR-JP

- **Designer:** Haley Wakamatsu (UkiyoMoji Fonts / @japanyoshi)
- **Source:** <https://fontstruct.com/fontstructions/show/2100555/vcr-jp>
- **Built with:** [FontStruct](https://fontstruct.com)
- **License:** **FontStruct Non-Commercial License**
- **Year:** 2022

### Allowed uses

- Personal projects, portfolio, study, internal demos.
- Editorial / journalism in non-paid contexts.
- Open-source dev tooling that consumes the font (this repo, the
  `ugc-cli` CLI, agent-driven post-processing of personal renders).

### Not allowed without separate permission from the designer

- Paid / sponsored / monetised video output.
- Embedding the glyphs into a commercially distributed template.
- Re-bundling the .ttf inside any product offered for sale.

If a downstream Ralphy template ever wants to default to this font for
user-generated commercial output, the template's `template.yaml` MUST
carry an `attribution` block that points back here AND warns the user
to obtain a commercial license from the designer first. The pool's
`attributionRequired: true` flag in `manifest.json` is the lint hook
that surfaces this requirement.
