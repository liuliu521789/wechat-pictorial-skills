---
name: wechat-pictorial-character-sheet
description: "Generate a WeChat pictorial lead-character turnaround sheet (主角三视图/定妆图): front, three-quarter, side, optional half-body portrait, plus character-lock.md with anchors. Enforces role-correct wardrobe (e.g. judge black robe). Trigger on 主角三视图, 角色定妆图, character sheet, turnaround, character-lead-sheet, 生成人物三视图, or when the user asks only for a locked protagonist reference before story panels."
---

# wechat-pictorial-character-sheet

Generate **only** the lead character turnaround assets for pictorial long-form. Do not write storyboards or story panels unless the user explicitly asks to continue into `wechat-pictorial-longform`.

## How to invoke

```text
@wechat-pictorial-character-sheet/SKILL.md
生成主角三视图：人民陪审员，中年戴眼镜，米色夹克；用默认水彩风格
```

```text
@wechat-pictorial-character-sheet/SKILL.md
cast_role: judge；正式开庭黑袍红襟；输出三视图+半身像并写入 character-lock.md
```

```text
@wechat-pictorial-character-sheet/SKILL.md
根据我上传的人脸/成片，提特征后生成同画风三视图
```

豆包：粘贴本 `SKILL.md` 全文 + 同上任务；用返回的英文 prompt 在画图页生成。

## Inputs（缺则问一次）

| Field | Required | Notes |
|-------|----------|-------|
| `label` | preferred | 称呼，如 老周 |
| `cast_role` | yes | see `references/role-wardrobe.md` |
| `traits` | preferred | 年龄、发型、眼镜、体型 |
| `wardrobe` override | no | if empty, use role-wardrobe default for `cast_role` |
| style | no | user style refs, or `style-card.md`, or bundled default |
| existing photo | no | extract traits only; redraw in pictorial style — do not paste photo into final sheet |

## Style source

1. User-uploaded style refs → brief style notes for prompts  
2. Else existing `style-card.md` in the workspace  
3. Else copy look from sibling `../wechat-pictorial-longform/references/default-style.md` if present  
4. Else use `references/default-style-snippet.md`

## Pipeline

1. Resolve `cast_role` + wardrobe via `references/role-wardrobe.md` (court roles must look like court attire).  
2. Write `character-lock.md` per `references/sheet-schema.md` (`status: draft`).  
3. Compile prompts (sheet + optional portrait).  
4. Generate images with Cursor `GenerateImage` when available:
   - `character-lead-sheet.png` — one image, three views side by side  
   - `character-lead.png` — clean half-body or bust, same identity/outfit  
   Aspect: sheet `16:9` or `3:2`; portrait `3:4` or `1:1`.  
5. Show paths + anchors; on user confirm set `status: locked` in `character-lock.md`.  
6. Optional handoff line: continue with `wechat-pictorial-longform` using these files as the character lock.

## Sheet layout rules

- Same person, same outfit, same glasses/hair in every view  
- Views: **front | 3/4 | side** (left-to-right), full or three-quarter body  
- Neutral cream paper / soft watercolor ground matching style  
- No speech bubbles, no captions baked in (labels only if user asks; prefer none)  
- Optional small prop consistent with role (e.g. folder stack for assessor) — one prop max  

## Prompt skeleton (English)

```text
character turnaround sheet on textured cream paper, soft realistic watercolor,
three views of the SAME person left to right: front, three-quarter, side,
{anchor_en}, {role wardrobe clause},
identical face and outfit across views, clean empty margins,
WeChat pictorial character design sheet, no text, no watermark
```

Portrait:

```text
half-body portrait, {anchor_en}, {wardrobe}, soft realistic watercolor on cream paper,
same identity as turnaround sheet, quiet background, no text
```

## Deliverables

```text
character-lock.md
character-lead-sheet.png
character-lead.png
prompts/character-sheet.txt
prompts/character-portrait.txt
```

## Quality bar

- Three views clearly readable and identity-matched  
- Wardrobe matches `cast_role`  
- Anchors are copy-paste stable for later panel generation  
- No story scene backgrounds cluttering the sheet  
