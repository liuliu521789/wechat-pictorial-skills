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
character: <paths if any>

## text_safe
regions: <list from text_slots>
burn_in: false|true
burn_in_strings: <only if true>
```

## English prompt skeleton

```text
{shot_size} shot, {angle}, {action}, {setting},
{style medium}, {palette keywords}, {light}, {paper},
{lead character traits}, soft painterly edges,
WeChat pictorial narrative panel, cinematic storytelling illustration,
keep {regions} as quiet empty negative space for later typography,
no text, no letters, no watermark, no speech bubbles
```

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
