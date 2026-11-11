# Storyboard schema

Write `storyboard.md` as YAML (or YAML fenced block) matching:

```yaml
title: ""
aspect_ratio: "9:16"
style_card: "style-card.md"
copy_summary: ""
shots:
  - id: "01"
    beat: "setup"         # setup|hook|conflict|metaphor|turn|point|resolve
    shot_size: "full"     # wide|full|medium|close|detail
    angle: "eye-level"    # eye-level|over-shoulder|two-shot|back-view|top-down|metaphor
    action: ""
    setting: ""
    emotion: ""
    cast:                 # required when people are visible
      - label: ""         # e.g. 审判长 / 老周 / 业主
        cast_role: ""     # see role-wardrobe.md: judge|lawyer|party_citizen|...
        wardrobe_note: "" # short Chinese or English outfit line matching role
    source_lines: []
    text_slots: []
text_manifest: []
```

Keep 7–12 shots unless the user specifies a count.  
Wardrobe must follow `references/role-wardrobe.md` (court roles wear court attire).
