# wechat-pictorial-character-sheet

单独生成**主角三视图定妆图** + `character-lead.png` + `character-lock.md`，供长图 skill 锁脸使用。

## 调用

```text
@wechat-pictorial-character-sheet/SKILL.md
生成主角三视图：cast_role=people_assessor，中年戴眼镜米色夹克
```

```text
@wechat-pictorial-character-sheet/SKILL.md
cast_role=judge，开庭黑袍红襟，输出三视图和半身像
```

下游：把产出交给 `wechat-pictorial-longform` 做分镜出图。
