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
cast_role: ""                # must match role-wardrobe.md
sheet: "character-lead-sheet.png"   # optional
portrait: "character-lead.png"      # primary lock image
anchor_zh: ""                # includes correct outfit wording
anchor_en: ""                # includes correct outfit wording
wardrobe: ""                 # fixed outfit — role-correct
traits: []                   # age, hair, glasses, face shape — immutable
forbid_changes: []           # e.g. no new hairstyle; no swapping into wrong role clothes
status: "draft|locked"
```

## How to build the lock

1. Decide `cast_role` from the script; set `wardrobe` via `role-wardrobe.md` (judge → black robe with red panel, etc.).
2. From style-card + notes, write `anchor_zh` / `anchor_en` that **name the outfit**, stable across panels.
3. Generate sheet + `character-lead.png` in style-card look **wearing that wardrobe**.
4. User confirms → `status: locked`. Do not batch panels until locked unless user overrides.

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
