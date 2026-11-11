# Style Card schema

Write `style-card.md` exactly in this shape:

```yaml
provisional: false   # true if no user references
source_refs: []      # filenames or short labels
medium: ""           # e.g. watercolor gouache on textured paper
palette:
  primary: []
  secondary: []
  avoid: []
light: ""
edges: ""            # soft / feathered / paper-bleed
paper: ""            # cream / warm off-white / grain
characters:
  lead: ""           # concrete traits → seed for character-lock.md
  supporting: ""
character_lock_file: "character-lock.md"   # filled after mode character
character_portrait: "character-lead.png"
compositions_common: []  # recurring framings seen in refs
typography_on_refs:
  narration: ""      # how旁白 appears on refs
  dialogue: ""
mood: []
negative_prompt: ""
```

Fill from vision of the references. Keep `characters.lead` concrete enough to build a character lock without inventing a new face later.
