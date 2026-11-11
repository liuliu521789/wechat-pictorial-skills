---
name: wechat-pictorial-script
description: "Expand a rough idea or sparse notes into the master pictorial script (总文案) for WeChat vertical story panels—beat narration, dialogue, keywords—before storyboard or image generation. Trigger on 总文案, 扩写文案, 想法写成绘本文案, 少量文字优化成完整故事稿, master script for pictorial longform, or when copy is too thin to shot-list. Hands off to wechat-pictorial-longform for shots and images."
---

# wechat-pictorial-script

Turn a user’s idea or thin draft into one **master script** that can drive every panel. Execute; do not teach.

Downstream skill for shots / text slots / images: `wechat-pictorial-longform` (sibling folder). Do not storyboard or generate images in this skill unless the user explicitly asks to continue in the same turn—then read and follow that skill after writing `master-script.md`.

## When to run

Run when any of these is true:

- User gives an idea, outline, bullets, or a short paragraph and wants full pictorial copy
- User asks to 扩写 / 优化成总文案 / 写成可出图的故事稿
- `wechat-pictorial-longform` finds copy too thin (see Gate below) and defers here

Skip if the user already provides a complete beat-ready script and only wants shots or images.

## Inputs

Collect once (ask only for gaps):

| Field | Required | Notes |
|-------|----------|-------|
| `idea` or draft text | yes | raw user material |
| `persona` | strongly preferred | who narrates (first person / witness) |
| `domain` | preferred | e.g. mediation, hospital, education |
| `reader_feeling` | preferred | emotion after reading |
| `facts` | as available | numbers, roles, setting—do not invent case facts |
| `length` | optional | default ~7 beats, 7–12 panel lines |

If facts are missing, invent **only** generic connective tissue (atmosphere, pacing). Mark invented lines with `inferred: true`. Never fabricate real names, agencies, verdicts, or identifiable private details; desensitize when the draft looks sensitive.

## Gate — thin copy

Treat copy as **thin** if fewer than ~4 usable story beats, or no conflict + turn + resolution. Then expand here before any shot list.

## Output

Write `master-script.md` using `references/master-script-schema.md`.

Also return a short chat summary: title, beat count, and whether the user should approve before `wechat-pictorial-longform`.

### Writing rules

1. **One master script for the whole set of images** — not per-panel prompts yet.
2. Structure as **7 beats** (may split a beat into two panel lines → 7–12 `panels` entries).
3. Each panel line has:
   - `narration` — short scrollable lines (can use `/` line breaks)
   - `dialogue` — optional list of `{speaker, line}`
   - `keyword` — optional 金句 / callout
   - `visual_intent` — one sentence of what should be seen (for the next skill; not an image prompt)
4. Voice: first-person witness by default if persona fits; warm, concrete, mobile-scroll pacing.
5. Preserve user meaning; expand clarity and rhythm, do not change their stance.
6. End with `usage`: how to call `wechat-pictorial-longform` with this file.

### Beat map (default)

| # | beat id | Must contain |
|---|---------|--------------|
| 1 | setup | who + today’s action |
| 2 | hook | stakes that widen |
| 3 | conflict | opposing voices or freeze |
| 4 | metaphor | imageable widen (iceberg, season, etc.) only if it fits; else deepen stakes |
| 5 | turn | human detail that softens |
| 6 | point | reason / keyword the reader can screenshot |
| 7 | resolve | outcome + aftertaste / identity |

## Modes

| Mode | Behavior |
|------|----------|
| `expand` (default) | idea/notes → `master-script.md` |
| `revise` | user feedback → patch the same file |
| `continue` | after approve, load `../wechat-pictorial-longform/SKILL.md` (or installed twin) and run storyboard(+generate) using this master script as copy |

## Quality bar

- Every panel line is placeable as on-page text later  
- Conflict and turn both exist  
- No wall-of-text narration; prefer short lines  
- `visual_intent` is concrete enough to shot-list without inventing a new plot  
