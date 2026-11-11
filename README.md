# 绘本长图公众号 Skills

用 AI 做微信公众号竖屏绘本长图：总文案 → 风格 → **主角三视图定妆** → 分镜 / 留白落字 → 出图。

| Skill | 作用 |
|-------|------|
| [wechat-pictorial-script](./wechat-pictorial-script) | 想法 / 少量文案 → `master-script.md` |
| [wechat-pictorial-character-sheet](./wechat-pictorial-character-sheet) | **主角三视图 / 定妆图** + `character-lock.md` |
| [wechat-pictorial-longform](./wechat-pictorial-longform) | 风格 / 角色锁 / 镜头 / 落字 / 出图 |

```text
想法 → script → master-script.md
     → character-sheet → character-lead-sheet.png + character-lead.png
     → longform：style（无参考图用 bundled default）→ storyboard → panels
```

无成片参考时，longform 默认采用 `wechat-pictorial-longform/references/default-style.md`。  
仓库：https://github.com/liuliu521789/wechat-pictorial-skills

## 安装（Cursor）

```bash
git clone https://github.com/liuliu521789/wechat-pictorial-skills.git
cp -r wechat-pictorial-skills/wechat-pictorial-script \
      wechat-pictorial-skills/wechat-pictorial-character-sheet \
      wechat-pictorial-skills/wechat-pictorial-longform \
      ~/.cursor/skills/
```

Windows：复制到 `%USERPROFILE%\.cursor\skills\`。  
或不安装：对话里 `@对应目录/SKILL.md`。

## 如何调用

### 总文案

```text
@wechat-pictorial-script/SKILL.md
身份…… 想法…… 请输出 master-script，先不要出图
```

### 主角三视图（独立 skill，须上传人物照片）

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
生成主角三视图：cast_role=people_assessor，默认水彩风
```

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
cast_role=judge，正式开庭黑袍红襟；输出三视图+半身像并锁定
```

### 风格 / 分镜 / 出图

```text
@wechat-pictorial-longform/SKILL.md
请只做 style / storyboard+text-layout / generate（每镜带 character-lead.png）
```

## 豆包用户

把对应 `SKILL.md` **全文粘贴**进对话；三视图用 skill 给出的英文 prompt 在画图页生成。详见 [豆包AI操作指南.md](./豆包AI操作指南.md)。
