# Text slots schema

Each shot may include:

```yaml
text_slots:
  - id: "01-n1"
    role: "narration"     # narration|dialogue|keyword|info
    speaker: null
    content: ""
    region: "bottom-band" # layout hint for later typography — NOT a painted box in the PNG
    priority: 1
    burn_in_text: false
```

```yaml
text_manifest:
  - panel: "01"
    region: "bottom-band"
    role: "narration"
    content: ""
```

## Critical: soft reserve, not a hard slab

`region` tells **layout later where to place type**. It must **not** make the generator paint:

- a torn-paper / deckle caption strip  
- a floating parchment rectangle  
- a hard horizontal crop band of blank beige  
- a solid color block in any corner (especially bottom-right)

Instead the illustration stays one continuous scene. “留白” = **soft atmospheric breathing room** inside the painting (pale sky, quiet wall wash, simplified floor fade) that still belongs to the same watercolor field.

## Placement heuristics

| Copy type | Prefer region | Image must |
|-----------|---------------|------------|
| Short narration | `bottom-band` or `top-band` | slightly quieter sky/wall/floor in that zone — blended, not a bar |
| Long narration | split across panels | same |
| Dialogue | `speech-left` / `speech-right` | open air near heads; no painted bubbles |
| Keyword | `callout-box` | softly quieter mid-side wash — no drawn card |
| Bio / info | `lower-third` | avoid faces there; still continuous scene |

## Coverage check

Every public `source_lines` item is in some `text_slots.content` or marked `layout: omit`.
