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
  lead: ""           # stable traits for consistency
  supporting: ""
compositions_common: []  # recurring framings seen in refs
typography_on_refs:
  narration: ""      # how旁白 appears on refs
  dialogue: ""
mood: []
negative_prompt: ""
```

Fill from vision of the references. Keep strings concrete and reusable inside image prompts.
