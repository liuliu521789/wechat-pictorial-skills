---
name: wechat-pictorial-character-sheet
description: "Generate a WeChat pictorial lead-character turnaround sheet (主角三视图/定妆图) from a REQUIRED user-uploaded person photo: front, three-quarter, side, half-body portrait, plus character-lock.md. Enforces role-correct wardrobe. Trigger on 主角三视图, 角色定妆图, character sheet, turnaround, 上传人物照片生成三视图, character-lead-sheet. Do not invent a face without a person reference image."
---

# wechat-pictorial-character-sheet

Generate **only** lead-character turnaround assets. Do not storyboard or make story panels unless the user asks to continue into `wechat-pictorial-longform`.

## Hard requirement — person reference photo

**The user must upload at least one clear photo of the person** (face visible; half-body preferred).

If no person image is attached and no local path is given:

1. **Stop.** Do not generate a sheet from imagination.  
2. Ask once: `请上传一张主角人物照片（正面或半身清晰人脸）作为参考，再生成三视图。`  
3. Resume only after the image is available.

Treat uploaded pictorial art of the same person as valid reference if a photo is unavailable but the user explicitly says it is the lead — still require *some* person image.

**Redraw rule:** Use the photo for identity (face, age, hair, glasses, body). Final sheet must be in pictorial watercolor style — **do not** paste, crop, or photoreal-trace the photo into the output.

## How to invoke

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
生成主角三视图：cast_role=people_assessor，穿着按身份；默认水彩风
```

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
cast_role=judge，正式开庭黑袍红襟；输出三视图+半身像并写 character-lock.md
```

豆包：粘贴本 `SKILL.md` + **先上传人物图** + 任务句；出图时把该图设为角色/参考图。

## Inputs

| Field | Required | Notes |
|-------|----------|-------|
| **person reference image** | **yes** | photo or clear likeness; hard gate |
| `cast_role` | yes | `references/role-wardrobe.md` |
| `label` | preferred | e.g. 老周 |
| `traits` | optional | override/extra; otherwise extract from photo |
| `wardrobe` | no | default from `cast_role` (may differ from clothes in the photo) |
| style | no | style refs / `style-card.md` / bundled default |

Wardrobe follows **role**, not necessarily the photo’s street clothes (e.g. photo in T-shirt + `cast_role=judge` → draw black robe, keep the face).

## Style source

1. User style refs → prompt notes  
2. Else workspace `style-card.md`  
3. Else sibling `../wechat-pictorial-longform/references/default-style.md`  
4. Else `references/default-style-snippet.md`

## Pipeline

1. **Confirm person image exists**; else stop and request upload.  
2. Vision-read the photo → face/hair/glasses/age notes.  
3. Resolve `cast_role` + wardrobe via `references/role-wardrobe.md`.  
4. Write `character-lock.md` (`status: draft`) per `references/sheet-schema.md`; set `source_person_ref` to the filename.  
5. Compile prompts; generate with Cursor `GenerateImage` when available:
   - `reference_image_paths` **must include the person photo** (and style refs if any)  
   - `character-lead-sheet.png` — front | 3/4 | side  
   - `character-lead.png` — half-body / bust  
   Sheet aspect `16:9` or `3:2`; portrait `3:4` or `1:1`  
6. User confirms → `status: locked`.  
7. Optional handoff to `wechat-pictorial-longform`.

## Sheet layout rules

- Likeness to the **uploaded person**; same face across all views  
- Outfit = role wardrobe (consistent on all views)  
- Cream paper / soft watercolor; no bubbles; no baked captions  
- One optional role prop max (e.g. folders)

## Prompt skeleton (English)

```text
character turnaround sheet on textured cream paper, soft realistic watercolor,
three views of the SAME person left to right: front, three-quarter, side,
likeness to the reference person photo, {anchor_en}, {role wardrobe},
identical face and outfit across views, illustrated not photographic,
clean empty margins, no text, no watermark
```

Portrait: same likeness + wardrobe, half-body, quiet ground, no text.

## Deliverables

```text
character-lock.md
character-lead-sheet.png
character-lead.png
prompts/character-sheet.txt
prompts/character-portrait.txt
```

## Quality bar

- No generation without a person reference image  
- Face matches reference; wardrobe matches `cast_role`  
- Anchors stable for later panels; sheet is design turnaround, not a story scene  
