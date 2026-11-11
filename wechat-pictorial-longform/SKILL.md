---
name: wechat-pictorial-longform
description: "Build WeChat pictorial long-form panels (竖屏绘本长图): extract style, lock lead character, enforce role-correct wardrobe (e.g. Chinese judges in black robe with red panel, not casual suits), storyboard with text-safe regions, generate images. No style refs → bundled default-style.md. Trigger on 公众号绘本长图, 法院服饰, 角色服装, 法官袍, 定妆锁脸, 分镜出图, character-lead, or master-script from wechat-pictorial-script."---

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
2. Style card — from user refs **or**, if none, the **bundled corpus default** in `references/default-style.md` (extracted from the project’s root pictorial panels).
3. **Character lock** with `status: locked` (see below), unless user explicitly skips.

### Thin-copy rule

Idea / bullets only → run `wechat-pictorial-script` first; pause for approval unless「直接全流程」.

Never invent sensitive case facts; desensitize when needed.

## Pipeline

```text
master-script + (optional style refs)
  → Style Card   # no refs → copy bundled default-style.md
  → Character lock (sheet + portrait + anchors)
  → Shot list    # bias to default compositions_common when using bundled style
  → Text slots
  → Per-panel prompt (style block + anchor_en + character ref)
  → Image generation
```

Hard stops:

- No image generation until shots **and** text_slots exist for panels that need words.
- No **batch** panel generation until character `status: locked`, unless user overrides.

---

### 1. Style extraction

**If the user uploads 1–3 reference pages:** read them with vision → write `style-card.md` per `references/style-card.md`. Include concrete `characters.lead`.

**If the user uploads no style references:** do **not** invent a new look. Copy `references/default-style.md` into `style-card.md` (keep `bundled_default: true`). That file is the extracted look/composition grammar of the root-directory pictorial corpus (cream paper watercolor, muted beige–dusty blue, eye-level mediation/court framing, metaphor wides, detail cutaways, vertical paper bands). Tell the user in one line that the bundled default style is in use.

If refs are partial, fill gaps from the bundled default rather than switching aesthetics.

---

### 2. Character lock（固定主角形象）

Read `references/character-lock.md`. Mode `character` or automatic before `full` / first `generate`.

Prefer sibling skill `wechat-pictorial-character-sheet` when the user only wants a turnaround, or call that skill’s pipeline for sheet+portrait quality; then continue here with the produced lock files.

1. Assign lead `cast_role` from script (assessor / judge / mediator / etc.). Read `references/role-wardrobe.md` and set `wardrobe` to match that role.
2. Write `character-lock.md` with immutable `traits`, `cast_role`, `wardrobe`, `anchor_zh`, `anchor_en` (anchors must mention the correct outfit).
3. Generate character sheet then `character-lead.png` in the style-card look **wearing that wardrobe**.
4. Ask user to confirm appearance; set `status: locked`.
5. On later panels: **never rewrite** anchors; always attach `character-lead.png` when the tool allows.

If user already provides a lead portrait: write lock file from it (still label `cast_role`); set `locked` after confirm.

---

### 3. Storyboard from master script

Read `references/shot-schema.md`. Prefer **1 shot per** `master-script` `panels[]` id.  
When `style-card.md` has `bundled_default: true`, bias framings to that file’s `compositions_common` / `shot_language_defaults`.  
Fill `id`, `beat`, `shot_size`, `angle`, `action`, `setting`, `emotion`, `source_lines`, and **`cast`** (every visible person: `label`, `cast_role`, `wardrobe_note` per `references/role-wardrobe.md`).

Shot sizes: `wide | full | medium | close | detail`  
Angles: `eye-level | over-shoulder | two-shot | back-view | top-down | metaphor`  
One action focus; ≤3 props.  
Court staff clothing must match role (e.g. formal hearing → judge in black robe with red panel).

---

### 4. Text slots

Read `references/text-slots.md`. Copy strings verbatim from the master script into `text_slots` + `text_manifest`.  
Prefer images without glyphs; reserve quiet `region`s.

---

### 5. Prompt compile + generate

Compile via `references/prompt-compiler.md`:

- Style-card look + **identical** `anchor_en` every panel  
- **Role wardrobe clauses** for every `cast` member (from `role-wardrobe.md`)  
- Text-safe zones from slots  
- `refs.character: character-lead.png` for the locked lead only  
- Default aspect `9:16`

Generation:

1. Cursor `GenerateImage`: `reference_image_paths` includes style refs **and** `character-lead.png`; `aspect_ratio: "9:16"`.
2. One panel per call: `panel-01.png`, …
3. Approve panel 01 likeness before continuing the batch when locking matters.
4. Face drift → regen that panel; repeated drift → re-run `character` mode.
5. **Artifact QA (required):** reject/regen if the image shows a hard torn-paper or blank caption slab, a solid corner color block (esp. bottom-right), watermark/logo/`AI生成` badge, or a scene chopped by a straight empty band. Prefer soft atmospheric breathing room only.
6. Save `prompts/panel-XX.txt`.

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
- Every on-screen person has role-correct wardrobe  
- Text “留白” is soft continuous atmosphere — never a painted caption bar or corner color block  
- No watermarks / AI badges  
- Slots match master script; safe regions avoid faces/hands  
- Shared palette + locked lead across the set  
