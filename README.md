# 绘本长图公众号 Skills

用 AI 做微信公众号竖屏绘本长图：总文案 → 风格 → **主角定妆锁脸** → 分镜 / 留白落字 → 出图。

| Skill | 作用 |
|-------|------|
| [wechat-pictorial-script](./wechat-pictorial-script) | 想法 / 少量文案 → `master-script.md` |
| [wechat-pictorial-longform](./wechat-pictorial-longform) | 风格 / **角色锁** / 镜头 / 落字 / 出图 |

```text
想法 → script → master-script.md
     → longform：style（无参考图则用 bundled default-style）→ character → storyboard → panels
```

无成片参考时，longform 默认采用仓库内 `wechat-pictorial-longform/references/default-style.md`（根目录绘本配图提取的画风与构图）。
仓库：https://github.com/liuliu521789/wechat-pictorial-skills

## 安装（Cursor）

```bash
git clone https://github.com/liuliu521789/wechat-pictorial-skills.git
cp -r wechat-pictorial-skills/wechat-pictorial-script wechat-pictorial-skills/wechat-pictorial-longform ~/.cursor/skills/
```

Windows：复制到 `%USERPROFILE%\.cursor\skills\`。

或不安装：打开仓库后对话里 `@wechat-pictorial-xxx/SKILL.md`。

## 如何调用（复制即用）

### 1. 总文案

```text
@wechat-pictorial-script/SKILL.md
身份…… 想法…… 希望读者感受……
请输出 master-script，先不要分镜出图
```

### 2. 提取风格

```text
@wechat-pictorial-longform/SKILL.md
请只做 style：根据我上传的成片输出风格卡
```

### 3. 固定主角形象（定妆 / 锁脸）

```text
@wechat-pictorial-longform/SKILL.md
按 character 模式：根据风格卡生成主角定妆图（三视图+半身），
写出 character-lock.md（含 anchor），我确认后再 locked
```

已有主角图时：

```text
@wechat-pictorial-longform/SKILL.md
用这张图作为 character-lead.png，建立 character-lock 并锁定
```

### 4. 分镜 + 留白落字

```text
@wechat-pictorial-longform/SKILL.md
按 master-script 做 storyboard + text-layout，先不要出图
```

### 5. 出图（每镜带角色锁）

```text
@wechat-pictorial-longform/SKILL.md
按 generate：9:16，文字不烧进图，
每镜必须使用 character-lead.png + 同一 anchor_en
先出 panel-01 我对脸，再出其余
```

### 全流程（文案已齐）

```text
@wechat-pictorial-longform/SKILL.md
全流程：风格 → 锁主角 → 分镜留白 → 生成；文案见 master-script.md
```

## 豆包用户

不能自动安装 Skill：把对应 `SKILL.md` **全文粘贴**进对话，任务话术同上。  
出图时在豆包画图/即梦里 **上传 `character-lead.png` 作角色参考**。详见 [豆包AI操作指南.md](./豆包AI操作指南.md)。
