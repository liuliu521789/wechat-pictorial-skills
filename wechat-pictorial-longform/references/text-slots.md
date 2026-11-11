# Text slots schema

Each shot may include:

```yaml
text_slots:
  - id: "01-n1"
    role: "narration"     # narration|dialogue|keyword|info
    speaker: null         # character label if dialogue
    content: ""           # exact Chinese (or user language) to place later
    region: "bottom-band" # top-band|bottom-band|upper-third|lower-third|left-rail|right-rail|center-soft|speech-left|speech-right|callout-box
    priority: 1           # 1 = must show
    burn_in_text: false   # true only if user wants glyphs in the PNG
```

Also maintain a flat list:

```yaml
text_manifest:
  - panel: "01"
    region: "bottom-band"
    role: "narration"
    content: ""
```

## Placement heuristics

| Copy type | Prefer region | Image must |
|-----------|---------------|------------|
| Short narration / diary line | `bottom-band` or `top-band` | keep that band low-detail paper/sky/wall |
| Long narration | split across adjacent panels; never one crowded band | same |
| Dialogue (two sides) | `speech-left` / `speech-right` upper chest height | leave open air near speakers’ heads; no painted bubbles |
| Keyword / 金句 | `callout-box` mid-side or lower third quiet field | flat wash behind the box zone |
| Bio / info block | `lower-third` or `bottom-band` | avoid faces in that third |

## Coverage check

Before generate: every `source_lines` item that must appear publicly is either in a `text_slots.content` or explicitly marked `layout: omit` with reason.
