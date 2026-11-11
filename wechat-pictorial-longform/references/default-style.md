# Bundled default style（根目录成片语料提取）

当用户**未上传参考图**时：把本文件内容写入工作区 `style-card.md`（可原样复制 YAML），并在 prompt 中锁定下列画风与构图。  
用户若上传了参考图：改用视觉提取结果，本文件仅作冲突时的回退。

`source_corpus`: root WeChat pictorial PNGs (watercolor narrative long-strip).

```yaml
provisional: false
bundled_default: true
source_refs:
  - "corpus:竖屏水彩纪实绘本长图（法院/调解/对话场景）"
medium: >
  soft realistic watercolor / light gouache on textured cream paper;
  delicate grey-brown outlines that melt into washes;
  visible paper tooth; not vector, not anime, not glossy 3D
palette:
  primary: ["cream paper #F5F0E6", "warm beige", "light khaki", "soft ochre"]
  secondary: ["muted walnut brown", "dusty navy / slate blue suits", "soft sage / leaf green accents", "wood furniture tones"]
  avoid: ["neon", "high-chroma purple", "cyber glow", "pure white studio backdrop", "harsh black comic ink"]
light: >
  soft diffused daylight or warm window side-light;
  low contrast; gentle falloff; calm professional warmth
edges: >
  soft painterly silhouettes; panels sit on continuous cream paper
  with quiet gaps between scenes (not hard comic gutters)
paper: "continuous textured warm off-white / cream ground across the whole strip"
characters:
  lead: >
    East Asian middle-aged man, neat short black hair, thin rectangular glasses,
    scholarly calm face, beige/khaki zip jacket over light collared shirt,
    dark trousers, often holding a thick stack of manila folders
  supporting: >
    same watercolor realism; judges/officials in navy suit + subtle red lapel pin;
    citizens in casual jackets; corporate reps in dark suits — distinct wardrobe, shared rendering
compositions_common:
  - "establishing full/wide: figure walking up courthouse steps or leaving classical columns (back or three-quarter)"
  - "medium dialogue at wooden desk / conference table, eye-level"
  - "over-shoulder or chest-up listener reacting (pen pause, thoughtful look)"
  - "two-shot confrontation across table (agitated left vs suited right, mediator/judge centered)"
  - "metaphor wide: symbolic backdrop (e.g. iceberg with faint family silhouettes under waterline) + small figures in foreground"
  - "detail cutaway: stacked hands on agreement, tea cup, documents — quiet lower or mid panel"
  - "memory/soft vignette inset for sensory beat (e.g. osmanthus / family at window) with faded edges"
  - "vertical rhythm: illustration block → quiet paper + narration band → next illustration; generous negative space"
shot_language_defaults:
  preferred_sizes: ["full", "medium", "close", "detail", "wide"]
  preferred_angles: ["eye-level", "over-shoulder", "two-shot", "back-view", "metaphor", "top-down"]
  rules:
    - "one action focus per panel; ≤3 props"
    - "faces and hands stay out of reserved text bands"
    - "backgrounds readable but desaturated so figures read first"
typography_on_refs:
  narration: "handwritten-feel short lines centered on cream paper between panels (added in layout, not baked in)"
  dialogue: "soft round rect speech bubbles in layout; leave speech-left/right air in the painting"
mood: ["warm", "documentary", "humane", "calm-serious", "trustworthy"]
prompt_style_block_en: >
  soft realistic Chinese watercolor narrative illustration on textured cream paper,
  muted beige brown dusty-blue palette, delicate blended outlines, low-contrast warm daylight,
  WeChat pictorial long-form comic panel, quiet negative space for captions,
  desaturated backgrounds, professional humane atmosphere
negative_prompt: >
  cartoon, anime, chibi, glossy 3d, photoreal stock photo, neon, purple glow,
  harsh black inking, cluttered UI, watermark, readable text, letters, logos, speech bubbles,
  oversaturated colors, cyberpunk, collage scrapbook
```

## Agent notes

- When applying this default, set `style-card.md` field `bundled_default: true` and cite `references/default-style.md`.
- Storyboard should bias toward `compositions_common` and `shot_language_defaults` above.
- Character lock may start from `characters.lead` here if the user has no separate portrait.
