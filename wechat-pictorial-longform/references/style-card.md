# Style Card schema

Write `style-card.md` exactly in this shape:

```yaml
provisional: false
bundled_default: false  # true when copied from references/default-style.md
source_refs: []         # user filenames, or corpus label if bundled
medium: ""
palette:
  primary: []
  secondary: []
  avoid: []
light: ""
edges: ""
paper: ""
characters:
  lead: ""
  supporting: ""
character_lock_file: "character-lock.md"
character_portrait: "character-lead.png"
compositions_common: []
shot_language_defaults: {}    # optional; present on bundled default
prompt_style_block_en: ""     # paste into every panel prompt
typography_on_refs:
  narration: ""
  dialogue: ""
mood: []
negative_prompt: ""
```

From user refs: fill by vision.  
No user refs: copy `references/default-style.md` wholesale.
