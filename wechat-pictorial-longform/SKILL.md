---
name: wechat-pictorial-longform
description: "Build WeChat pictorial long-form panels (竖屏绘本长图): extract style from reference pages, turn a master script or full copy into camera shot lists, reserve text-safe regions per panel, and generate images. Trigger on 公众号绘本长图, 提取页面风格, 分镜构图, 文案留白落字, 按文案出图, WeChat storyboard panels, or when handed master-script.md from wechat-pictorial-script. If the user only has an idea or thin notes, run or point to wechat-pictorial-script first."
---

# wechat-pictorial-longform

Agent workflow for vertical narrative illustration pages. Execute; do not teach.

Upstream copy skill: `wechat-pictorial-script` → produces `master-script.md`. This skill consumes that (or equally complete copy), then style / shots / text slots / images.

## Modes

Infer mode from the user message. End-to-end with ready copy: **style → storyboard → text slots → generate**. End-to-end with only an idea: **script (upstream) → style → storyboard → text slots → generate**.

| Mode | Trigger examples | Output |
|------|------------------|--------|
| `script-handoff` | 只有想法/提纲/几句草稿 | defer to `wechat-pictorial-script` (or run it if that skill is available) |
| `style` | 提取风格、锁画风、分析成片 | `style-card.md` |
| `storyboard` | 分镜、构图、镜头 | `storyboard.md` |
| `text-layout` | 留白、落字、文案放哪 | update `storyboard.md` text_slots |
| `generate` | 出图、生成图片 | `panel-XX.png` + prompt log |
| `full` | 做成片 / 全流程（文案已齐） | style + storyboard + generate |

Default language for cards and slot copy: **Chinese**. Image prompts: **English** unless the generator is Chinese-only.

## Prerequisites

Before **storyboard** or **generate**:

1. **Master copy** — prefer `master-script.md` from `wechat-pictorial-script`, or user paste that already has ≥4 beats with conflict + turn + resolve.
2. Optional **reference images** for style. If missing, use `references/default-style.md` and mark `provisional: true`.
3. Optional **character refs**.

### Thin-copy rule

If the user has only an idea, bullets, or a short paragraph **and** asks for shots or images: **do not storyboard yet**.

1. Read and follow `wechat-pictorial-script/SKILL.md` (sibling path or `~/.cursor/skills/wechat-pictorial-script/`).
2. Write `master-script.md`.
3. Pause for approval unless the user said「直接全流程 / 不用确认」.
4. Continue with this skill using that file as copy.

Never invent undisclosed case facts. Desensitize names/places when the copy looks real-world sensitive.

## Pipeline

```text
idea / thin notes
  → [wechat-pictorial-script] master-script.md
references + master script
  → Style Card
  → Shot list (camera)          # prefer 1 shot per master-script panels[] id
  → Text slots (regions + exact strings from script)
  → Per-panel prompt
  → Image generation
```

Hard stop: do not call image generation until `storyboard.md` has shots **and** text_slots for every panel that needs words.

---

### 1. Style extraction

Read 1–3 reference pages with vision. Write `style-card.md` using the schema in `references/style-card.md`.

Extract only what is visible: medium, palette, light, edge treatment, paper, character look, recurring compositions, typography roles on the page (旁白 vs 对白), and a negative prompt list.

If references disagree, prefer the majority look and note conflicts in one line.

---

### 2. Storyboard from master script

Read `references/shot-schema.md`.

If `master-script.md` exists, map **each `panels[]` item → one shot** with the same `id`. Use:

- `visual_intent` → `action` + `setting`
- `narration` / `dialogue` / `keyword` → `source_lines` and later `text_slots`
- `beat` / emotion cues → `beat` / `emotion`

If the user pasted unstructured but complete copy, split into **7–12 shots** covering:

1. identity / setup  
2. hook / stakes  
3. conflict  
4. metaphor or widen  
5. soft turn / icebreak  
6. reasoned beat / keyword  
7. resolution / aftertaste  

For each shot fill: `id`, `beat`, `shot_size`, `angle`, `action`, `setting`, `emotion`, `source_lines`.

Shot sizes: `wide | full | medium | close | detail`  
Angles: `eye-level | over-shoulder | two-shot | back-view | top-down | metaphor`

One action focus per shot. ≤3 props.

---

### 3. Text slots (reservation + copy placement)

For each shot, decide whether the **finished page** carries text, and where.

Read `references/text-slots.md`. Attach `text_slots[]` to every shot.

Prefer copying strings **verbatim** from `master-script.md` (`narration`, `dialogue[].line`, `keyword`). Do not rewrite at this stage unless the user asks to tighten for layout.

Rules:

- Map every essential script line into exactly one slot. Do not drop meaning.
- Prefer **image without glyphs**; text is composited later unless the user insists on burned-in text.
- Each slot declares a `region` the generator must keep visually quiet.
- Conflict / dialogue shots: leave bubble-safe or caption-safe areas; still default to empty image + slot metadata.
- Metaphor / detail shots may be `text_slots: []` if surrounding panels carry the words.
- Output `text_manifest`: panel id → region → exact string.

Region vocabulary: `top-band | bottom-band | upper-third | lower-third | left-rail | right-rail | center-soft | speech-left | speech-right | callout-box`

---

### 4. Prompt compile + generate

For each shot to render, build a prompt from `references/prompt-compiler.md`:

- Subject, shot size, angle, setting, style-card locked look  
- Explicit **text-safe zone** instructions matching `text_slots[].region`  
- `no text, no letters, no logos, no speech bubbles` unless `burn_in_text: true`  
- Aspect ratio default `9:16` (use `3:4` if user asks)

Generation:

1. Prefer Cursor `GenerateImage` with `reference_image_paths` pointing at style refs when available; `aspect_ratio: "9:16"`.
2. One panel per call. Filename `panel-01.png`, `panel-02.png`, …
3. After each image, if the user asked for QA, check that reserved regions stay calm enough for type; regenerate that panel only if cluttered.
4. Save prompts to `prompts/panel-XX.txt`.

Do not mix in zine-poster skills (`scene-distillation-zine`, `scenes-gathered-zine`).

---

## Deliverables

```text
master-script.md       # from upstream skill when idea was thin
style-card.md
storyboard.md          # shots + text_slots + text_manifest
prompts/panel-XX.txt
panel-XX.png           # when generate/full
```

Chat reply: paths + `text_manifest` table only. No tutorial prose.

## Partial requests

- Idea only → `script-handoff` / run `wechat-pictorial-script`
- Style only → `style-card.md`
- Storyboard only → shots without images (requires master copy)
- “只规划留白/落字” → update text_slots from storyboard + script
- “按已有分镜出图” → compile prompts and generate

## Quality bar

- Thin ideas never skip the master script  
- Style card is observational  
- Slot strings match the master script  
- Reserved regions do not fight faces or hands  
- Panels share one palette and one lead character from the style card  
