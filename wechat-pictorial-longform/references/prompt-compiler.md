# Prompt compiler

For each shot, write `prompts/panel-XX.txt` with:

```text
## director
<Chinese one-liner: shot_size + angle + action>

## prompt
<English image prompt>

## negative
<from style-card.negative_prompt + text/letters/bubbles unless burn-in>

## refs
style: <paths>
character: character-lead.png   # required when lock status=locked

## character_anchor
<paste character-lock.md anchor_en verbatim — identical on every panel>

## text_safe
regions: <list from text_slots>
burn_in: false|true
burn_in_strings: <only if true>
```

## English prompt skeleton

```text
{shot_size} shot, {angle}, {action}, {setting},
{style medium}, {palette keywords}, {light}, {paper},
{anchor_en}, same person as character reference, consistent facial identity,
soft painterly edges, WeChat pictorial narrative panel,
keep {regions} as quiet empty negative space for later typography,
no text, no letters, no watermark, no speech bubbles
```

Never paraphrase `anchor_en` between panels.
Map regions to prompt phrases:

| region | phrase |
|--------|--------|
| top-band | empty calm area across the top fifth |
| bottom-band | empty calm paper band across the bottom fifth |
| upper-third | simplified low-detail upper third |
| lower-third | simplified low-detail lower third |
| left-rail / right-rail | quiet vertical margin on the left/right |
| center-soft | soft uncluttered mid field |
| speech-left / speech-right | open air beside the left/right speaker’s head |
| callout-box | flat quiet patch suitable for a small caption card |

If `burn_in_text: true`, omit “no text”, add exact strings and placement, and still keep hierarchy readable.
