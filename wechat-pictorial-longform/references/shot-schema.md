# Storyboard schema

Write `storyboard.md` as YAML (or YAML fenced block) matching:

```yaml
title: ""
aspect_ratio: "9:16"
style_card: "style-card.md"
copy_summary: ""          # one sentence
shots:
  - id: "01"
    beat: "setup"         # setup|hook|conflict|metaphor|turn|point|resolve
    shot_size: "full"     # wide|full|medium|close|detail
    angle: "eye-level"    # eye-level|over-shoulder|two-shot|back-view|top-down|metaphor
    action: ""            # one focus
    setting: ""
    emotion: ""
    source_lines: []      # quotes from user copy covered by this shot
    text_slots: []        # see text-slots.md
text_manifest: []         # flattened list for layout; see text-slots.md
```

Keep 7–12 shots unless the user specifies a count.
