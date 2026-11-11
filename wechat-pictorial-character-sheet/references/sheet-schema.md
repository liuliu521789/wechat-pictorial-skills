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
source_person_ref: ""   # required: user-uploaded person photo filename/path
```

Hard gate: do not write a lock or generate sheets if `source_person_ref` is empty.  
`anchor_en` must include face traits from the photo + role wardrobe so `wechat-pictorial-longform` can paste it unchanged.
