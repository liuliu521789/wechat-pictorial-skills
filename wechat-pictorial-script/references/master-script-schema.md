# master-script.md schema

```yaml
title: ""
persona: ""
domain: ""
reader_feeling: ""
source:
  type: "idea|notes|draft"
  raw_excerpt: ""          # short quote of user input
panels:                    # 7–12 items; drives later shots 1:1 when possible
  - id: "01"
    beat: "setup"          # setup|hook|conflict|metaphor|turn|point|resolve
    narration: ""          # use \n or / for scroll breaks
    dialogue:
      - speaker: ""
        line: ""
    keyword: null          # string or null
    visual_intent: ""      # one concrete image sentence
    inferred: false        # true if agent filled gaps
text_bank:
  # flat list for layout / search
  - panel: "01"
    role: "narration|dialogue|keyword"
    content: ""
handoff:
  next_skill: "wechat-pictorial-longform"
  copy_file: "master-script.md"
  note: "Use panels[].narration|dialogue|keyword as copy; visual_intent informs shots only."
```

When revising, keep stable `id`s unless the user asks to reorder.
