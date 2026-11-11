# wechat-pictorial-longform

从**总文案**提取风格 → 分镜构图 → 文字留白落字 → 生成竖屏绘本图。

若只有想法或几句草稿，先用兄弟 skill：[`../wechat-pictorial-script`](../wechat-pictorial-script)。

## Install

```bash
cp -r wechat-pictorial-script ~/.cursor/skills/
cp -r wechat-pictorial-longform ~/.cursor/skills/
```

## Pipeline

```text
想法 / 少量文案
  → wechat-pictorial-script  → master-script.md
  → wechat-pictorial-longform → style / storyboard / panels
```

## Example prompts

```text
@wechat-pictorial-script/SKILL.md
想法：……
先出总文案
```

```text
@wechat-pictorial-longform/SKILL.md
按 master-script.md 分镜，标明每镜留白和落字，再生成 9:16 图
```
