# Character lock

## Assets to write

```text
character-lead.png           # required for locked generate
character-lead-sheet.png     # optional: front / 3-4 / side on one sheet
character-lock.md            # required metadata
```

## character-lock.md schema

```yaml
role: "lead"
label: ""                    # e.g. 老周
sheet: "character-lead-sheet.png"   # optional
portrait: "character-lead.png"      # primary lock image
anchor_zh: ""                # paste verbatim every panel (Chinese ok for排版笔记)
anchor_en: ""                # paste verbatim into every image prompt
wardrobe: ""                 # fixed outfit
traits: []                   # age, hair, glasses, face shape — immutable
forbid_changes: []           # e.g. no new hairstyle, no suit swap mid-story
status: "draft|locked"       # generate panels only when locked (unless user overrides)
```

## How to build the lock

1. From style-card `characters.lead` (+ user notes), write `anchor_zh` / `anchor_en` — concrete, stable, no mood adjectives that change per shot.
2. Generate `character-lead-sheet.png` (character sheet) then a clean `character-lead.png` portrait/half-body in the **same style as style-card**.
3. Show the user; on approval set `status: locked`.
4. Do not batch story panels until `status: locked` unless user says「不用锁脸 / 直接出图」.

## Reuse rules

- Every panel prompt includes **identical** `anchor_en`.
- Every generate call passes `portrait` (and style refs) in `reference_image_paths` when the tool allows.
- Shot size/angle may change; identity fields must not.
- If face drifts: regenerate that panel only; if still drifting, refresh `character-lead.png` and re-lock before continuing.

## Tool hooks

| Tool | Lock method |
|------|-------------|
| Cursor `GenerateImage` | `reference_image_paths: [style refs…, character-lead.png]` |
| Midjourney | `--cref <character-lead url>` + `--cw 100` (face) or `80` (looser costume) |
| 即梦 / 豆包画图 | 角色参考或图生图上传 `character-lead.png`，强度偏高，并贴 `anchor_zh`/`anchor_en` |
