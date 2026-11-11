# wechat-pictorial-character-sheet

单独生成**主角三视图定妆图** + `character-lead.png` + `character-lock.md`，供长图 skill 锁脸使用。

**必须先上传人物照片**，没有参考图则不生成。

## 调用

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
生成主角三视图：cast_role=people_assessor
```

```text
@wechat-pictorial-character-sheet/SKILL.md
（附上人物照片）
cast_role=judge，开庭黑袍红襟，输出三视图和半身像
```

下游：把产出交给 `wechat-pictorial-longform` 做分镜出图。
