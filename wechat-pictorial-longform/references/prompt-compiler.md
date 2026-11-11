# Prompt compiler

For each shot, write `prompts/panel-XX.txt` with:

```text
## director
<Chinese one-liner: shot_size + angle + action>

## cast_wardrobe
<label>: <cast_role> — <wardrobe clause EN>

## prompt
<English image prompt>

## negative
<style-card.negative_prompt + artifact bans below + wrong uniforms if needed>

## refs
style: <paths>
character: character-lead.png   # lead only, when lock status=locked

## character_anchor
<paste character-lock.md anchor_en verbatim>

## text_safe
regions: <list from text_slots>   # layout hints only
burn_in: false|true
```

## English prompt skeleton

```text
{shot_size} shot, {angle}, {action}, {setting},
{prompt_style_block_en},
{anchor_en}, same person as character reference,
{role wardrobe for other cast},
full-bleed continuous watercolor scene on cream paper,
soft natural breathing room toward {soft_reserve_phrase} that blends into the painting,
no separate caption bar, no torn-paper strip, no floating parchment rectangle,
no hard blank band, no corner color block, no watermark, no logo, no text, no speech bubbles
```

Never paraphrase `anchor_en`. Always expand `cast[]` wardrobe.  
If `bundled_default: true`, include full `prompt_style_block_en`.

### Soft reserve phrases (use these — NOT “empty rectangle”)

| region | phrase |
|--------|--------|
| top-band | softly quieter upper sky or wall wash, still continuous with the scene |
| bottom-band | softly quieter lower floor or ground wash fading into paper, continuous scene |
| upper-third | lighter low-detail air in the upper third, no hard slab |
| lower-third | simplified lower third without cutting the subject with a bar |
| left-rail / right-rail | slightly calmer side margin that still shows paper grain of the same painting |
| center-soft | a soft uncluttered pocket inside the scene, not a drawn box |
| speech-left / speech-right | open air beside the speaker’s head |
| callout-box | softly quieter side wash suitable for later type — no painted card |

### Required negative (append every panel)

```text
torn paper caption bar, deckle text strip, floating parchment rectangle,
hard blank beige band, solid corner color block, bottom-right color patch,
watermark, logo, AI生成, WORKBUDDY, UI badge, subtitle box,
readable text, letters, speech bubbles, collage frame, masked overlay
```

Court staff (when present): also  
`judge in casual clothes, western barrister wig, security vest instead of judicial police uniform`

### QA before accepting a panel

Reject and regenerate if you see:

1. Hard torn/blank caption slab  
2. Bottom-right (or any corner) solid color block / watermark badge  
3. Scene chopped by a straight empty band  

If `burn_in_text: true`, omit “no text”, place exact strings carefully, still forbid watermarks and corner blocks.
