---
name: wechat-pictorial-longform
description: "Build WeChat pictorial long-form panels (竖屏绘本长图): extract style, lock a consistent lead character (定妆/锁脸), turn master script into shot lists with text-safe regions, and generate images. Trigger on 公众号绘本长图, 提取页面风格, 固定主角形象, 角色定妆, 锁脸, 分镜构图, 文案留白落字, 按文案出图, character-lead, or when handed master-script.md from wechat-pictorial-script. If the user only has an idea or thin notes, run wechat-pictorial-script first."
---

# wechat-pictorial-longform

Agent workflow for vertical narrative illustration pages. Execute; do not teach.

Upstream copy skill: `wechat-pictorial-script` → `master-script.md`. This skill: style → **character lock** → shots → text slots → images.

## How to invoke (for the user / teacher)

Cursor (after clone or `@` path):

```text
@wechat-pictorial-longform/SKILL.md
按 character 模式：根据风格卡生成主角定妆图并锁定
```

```text
@wechat-pictorial-longform/SKILL.md
已有 character-lead.png，按 generate 出图，每镜必须带角色锁
```

```text
@wechat-pictorial-longform/SKILL.md
全流程：风格 → 锁主角 → 分镜留白 → 生成 9:16（文案见 master-script.md）
```

豆包：粘贴本 `SKILL.md` 全文，再说上列同一句任务（豆包画图时手动上传 `character-lead.png` 作角色参考）。

## Modes

| Mode | Trigger examples | Output |
|------|------------------|--------|
| `script-handoff` | 只有想法/提纲 | defer to `wechat-pictorial-script` |
| `style` | 提取风格、锁画风 | `style-card.md` |
| `character` | 固定主角、定妆、锁脸、角色三视图 | `character-lock.md` + `character-lead.png` (+ optional sheet) |
| `storyboard` | 分镜、构图 | `storyboard.md` |
| `text-layout` | 留白、落字 | update text_slots |
| `generate` | 出图 | `panel-XX.png` |
| `full` | 文案已齐的成片流程 | style → character → storyboard → generate |

Default: cards/slots in **Chinese**; image prompts in **English** unless the generator is Chinese-only.

## Prerequisites

Before **generate**:

1. Master copy (`master-script.md` or equivalent complete copy).
2. Style card (from refs or `references/default-style.md` with `provisional: true`).
3. **Character lock** with `status: locked` (see below), unless user explicitly skips.

### Thin-copy rule

Idea / bullets only → run `wechat-pictorial-script` first; pause for approval unless「直接全流程」.

Never invent sensitive case facts; desensitize when needed.

## Pipeline

```text
master-script + style refs
  → Style Card
  → Character lock (sheet + portrait + anchors)   # before batch panels
  → Shot list
  → Text slots
  → Per-panel prompt (always includes anchor_en + character ref)
  → Image generation
```

Hard stops:

- No image generation until shots **and** text_slots exist for panels that need words.
- No **batch** panel generation until character `status: locked`, unless user overrides.

---

### 1. Style extraction

Vision on 1–3 refs → `style-card.md` per `references/style-card.md`.  
Include a concrete `characters.lead` string usable as the seed for character lock.

---

### 2. Character lock（固定主角形象）

Read `references/character-lock.md`. Mode `character` or automatic before `full` / first `generate`.

1. Write `character-lock.md` with immutable `traits`, `wardrobe`, `anchor_zh`, `anchor_en`.
2. Generate character sheet then `character-lead.png` in the style-card look.
3. Ask user to confirm appearance; set `status: locked`.
4. On later panels: **never rewrite** anchors; always attach `character-lead.png` as a reference when the tool supports it.

If user already provides a lead portrait: write lock file from it, skip sheet if they refuse, set `locked` after they confirm.

---

### 3. Storyboard from master script

Read `references/shot-schema.md`. Prefer **1 shot per** `master-script` `panels[]` id.  
Fill `id`, `beat`, `shot_size`, `angle`, `action`, `setting`, `emotion`, `source_lines`.

Shot sizes: `wide | full | medium | close | detail`  
Angles: `eye-level | over-shoulder | two-shot | back-view | top-down | metaphor`  
One action focus; ≤3 props.

---

### 4. Text slots

Read `references/text-slots.md`. Copy strings verbatim from the master script into `text_slots` + `text_manifest`.  
Prefer images without glyphs; reserve quiet `region`s.

---

### 5. Prompt compile + generate

Compile via `references/prompt-compiler.md`:

- Style-card look + **identical** `anchor_en` every panel  
- Text-safe zones from slots  
- `refs.character: character-lead.png`  
- Default aspect `9:16`

Generation:

1. Cursor `GenerateImage`: `reference_image_paths` includes style refs **and** `character-lead.png`; `aspect_ratio: "9:16"`.
2. One panel per call: `panel-01.png`, …
3. Approve panel 01 likeness before continuing the batch when locking matters.
4. Face drift → regen that panel; repeated drift → re-run `character` mode.
5. Save `prompts/panel-XX.txt`.

Do not use zine-poster skills for this pipeline.

---

## Deliverables

```text
style-card.md
character-lock.md
character-lead.png
character-lead-sheet.png   # optional
storyboard.md
prompts/panel-XX.txt
panel-XX.png
```

Chat: paths + `text_manifest` (+ character lock status). No tutorial prose.

## Partial requests

- `固定主角` / `定妆` / `锁脸` → `character` only  
- Style only → `style-card.md`  
- Storyboard only → no images  
- Generate with existing lock → reuse anchors + portrait refs  

## Quality bar

- Same `anchor_en` on every panel prompt  
- Character ref attached whenever the generator allows  
- Slots match master script; safe regions avoid faces/hands  
- Shared palette + locked lead across the set  
