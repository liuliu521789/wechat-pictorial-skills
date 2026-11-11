# Role wardrobe（按身份定服饰）

出图前为每个出场人物标注 `cast_role`，服饰必须符合身份。禁止把法官画成普通西装、把法警画成便装安保。

## Cast roles → wardrobe

| cast_role | 中文身份 | 必须穿着（画面可辨） | 禁止 |
|-----------|----------|----------------------|------|
| `judge` | 法官（审判席/正式开庭） | 黑色法官袍，红色前襟装饰带清晰；可戴胸徽 | 普通藏青西装当「法官」、休闲夹克 |
| `judge_casual_mediation` | 法官主持调解（非开庭） | 深色制式/职业套装 + 法院胸徽或可辨身份标识；仍须职业庄重 | 花衬衫、羽绒服、棒球帽 |
| `court_clerk` | 书记员 | 深色职业套装或法院工作装，克制配饰 | 夸张首饰、运动装 |
| `judicial_police` | 法警 | 法警制式服装（深色制式外套/标志性肩章或标识，以当代中国法院常见形象为准） | 保安马甲、迷彩、便衣无标识 |
| `people_assessor` | 人民陪审员 | 与场景匹配的庄重便装或陪审工作标识；连载主角可固定米色夹克等已锁形象，但出庭正式场合可加「人民陪审员」胸牌 | 奇装异服 |
| `lawyer` | 律师 | 深色西装套装、衬衫、领带/领结得体；可持公文包/卷宗 | 牛仔短裤、连帽衫 |
| `prosecutor` | 检察人员 | 检察职业套装/识别得出的公务着装 | 休闲装 |
| `mediator_civilian` | 人民调解员/教师陪审等民间身份 | 干净得体的便装或单位工装（按脚本） | 与法官袍混淆 |
| `party_citizen` | 当事人（居民/业主） | 生活便装，年龄职业相符 | 法官袍、制式警服 |
| `party_corp` | 企业代表/开发商 | 商务西装或商务休闲 | 工地安全帽出现在正式调解室除非剧情需要 |
| `family` | 家属/老人小孩 | 生活常服，季节合理 | 制服乱穿 |

中国法院正式开庭场景：优先 `judge` = **黑袍红襟**，不要用「外国人假发白袍」。

## Per-shot rules

1. In `storyboard.md`, each shot that shows people must list `cast: [{ label, cast_role, wardrobe_note }]`.
2. Prompt compiler must emit an English wardrobe clause per person on screen.
3. Lead character lock `wardrobe` must match their `cast_role` (e.g. assessor ≠ judge robe unless the script says they are the judge).
4. If the script says 开庭/审判长/法槌 → force `judge` robe. If 调解室协商 → `judge_casual_mediation` or assessor/mediator as scripted — still no party clothing on court staff.
5. Supporting characters keep **role-correct clothes** even when face is not locked; only the lead uses `character-lead.png` face lock.

## Prompt snippets (English)

- judge: `Chinese judge in black judicial robe with red front panel, court badge, dignified posture`
- judge_casual_mediation: `Chinese judge in dark formal court business attire with small red court lapel badge, not a judicial robe if mediation setting`
- judicial_police: `Chinese court judicial police in proper uniform with visible insignia`
- lawyer: `lawyer in dark business suit and tie, holding case files`
- party_citizen: `ordinary Chinese resident in everyday clothes`
- party_corp: `corporate representative in navy business suit`
