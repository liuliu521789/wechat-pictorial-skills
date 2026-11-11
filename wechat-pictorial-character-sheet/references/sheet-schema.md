# character-lock.md schema（本 skill 产出）

```yaml
role: "lead"
label: ""
cast_role: ""
sheet: "character-lead-sheet.png"
portrait: "character-lead.png"
anchor_zh: ""
anchor_en: ""
wardrobe: ""
traits: []
forbid_changes:
  - "no hairstyle change"
  - "no outfit change unless cast_role changes"
  - "same glasses and face across all panels"
status: "draft"   # user confirms → locked
source_skill: "wechat-pictorial-character-sheet"
```

`anchor_en` must include face traits + full wardrobe phrase so `wechat-pictorial-longform` can paste it unchanged.
