# 《逆塔回响》系统文档（基于OCR整理）

说明：
- 本文档由 `C:\Users\XD\Downloads\doc` 下 PNG 资料 OCR 汇总而成，存在少量识别噪声。
- 关键字段与结构以 README/GDD/表结构页为准；个别名称为“OCR不清”时做保留标注。

## 1. 项目概述
- 类型：魔塔 / 数值解谜 / 回合制 RPG
- 平台：PC（Windows/macOS）优先，预留移动端移植
- 核心卖点：无随机战斗、结果可计算；资源有限、决策驱动；强复盘与挑战性

## 2. 核心设计目标
- 所有战斗结果可预期（无随机伤害）
- 资源有限，每一步存在机会成本（钥匙、HP、金钱、路线）
- 通关依赖理解与规划，而非操作练习
- 支持多结局与极限挑战（速通/低属性通关）

## 3. 核心系统

### 3.1 玩家属性与资源
- 属性：HP、ATK、DEF、GOLD
- 钥匙：黄/蓝/红

### 3.2 战斗公式（经典魔塔）
- 玩家每回合伤害 = max(ATK - 敌DEF, 0)
- 敌人每回合伤害 = max(敌ATK - DEF, 0)
- 回合数 = ceil(敌HP / 玩家每回合伤害)
- 预计损失HP = (回合数 - 1) × 敌人每回合伤害
- 若玩家每回合伤害 = 0，则不可战胜

### 3.3 楼层节奏（1-30F Demo）
- 1-10F：教学与基础计算；引入黄门与高防陷阱怪（铁甲卫）
- 11-20F：双路线与钥匙管理；引入蓝门、无视防御、吸血机制
- 21-30F：迷宫化布局与资源压缩；30F 为阶段Boss

### 3.4 NPC（示例文案）
- 守塔人（引导）： “塔不会阻止你，它只会记住你做过的每一次选择。”
- 商人（交易）： “我卖的不是道具，是你未来少走的弯路。”
- 记录碑（碎片叙事）： “当你以为自己在向上走时，塔正在把你推回更深处。”

### 3.5 结局（草案）
- 普通结局：通关主线塔段
- 理性结局：不使用“时之沙”等重置类道具
- 极限结局：最终 ATK+DEF 达到阈值（OCR不清，需回源确认）
- 真结局：收集全部记录碑碎片，并满足关键 NPC 存活条件（31F+ 扩展）

## 4. 数据驱动与表结构
- Demo 使用 9x9 网格逐格配置，适合编辑器/表格快速迭代
- 9x9 坐标：X=1..9, Y=1..9
- CellType：Wall / Floor / DoorY / DoorB / DoorR / Start / StairDown / StairUp
- ContentType：Enemy / Item / NPC / None
- ContentID：引用 Enemies / Items / NPCs ID

### 4.1 Enemies.csv
字段：EnemyID, Name, Tier, HP, ATK, DEF, Gold, SpecialType, SpecialParam, Notes

### 4.2 Items.csv
字段：ItemID, Name, Type, EffectHP, EffectATK, EffectDEF, EffectMaxHP, KeyColor, SpecialTag, Price, Notes

### 4.3 NPCs.csv
字段：NpcID, Name, Role, DialogueKey, Notes

### 4.4 Floors_1_30.csv
字段（根据 README 与表头）：Floor, Template, Theme, RecommendedATK/DEF/HP, Doors_Y/B/R, HasShop, HasLore, Notes
说明：OCR 仅稳定识别出 RecommendedATK/DEF/HP；门数量与商店字段未能可靠提取。

### 4.5 Placements_1_30.csv
字段：Floor, X, Y, CellType, ContentType, ContentID, Quantity, ScriptTag, Comment

## 5. 数值表摘要（基于 OCR）

### 5.1 敌人（Enemies）
说明：名称含“OCR不清”表示原图识别不稳定，请回源核对。

| ID   | 名称 | Tier | HP | ATK | DEF | Gold | 特殊 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| E001 | 绿史莱姆 | 1 | 50 | 12 | 4 | 1 | 无 |
| E002 | 洞写蝙蝠（OCR不清） | 1 | 60 | 14 | 5 | 2 | FirstStrike(0/1) |
| E003 | Praca（OCR不清） | 1 | 80 | 16 | 6 | 3 | 无 |
| E004 | 哥布林 | 1 | 90 | 18 | 8 | 4 | 无 |
| E005 | 铁甲卫 | 1 | 120 | 20 | 18 | 8 | HighDEF |
| E006 | 黑骑士 | 2 | 220 | 28 | 18 | 10 | 无 |
| E007 | mins（OCR不清） | 2 | 160 | 24 | 12 | 9 | Drain(5) |
| E008 | 法师学徒 | 2 | 140 | 30 | 8 | 10 | IgnoreDEF |
| E009 | （OCR不清） | 2 | 180 | 26 | 15 | 12 | CurseATK(1) |
| E010 | 石像魔像 | 2 | 260 | 30 | 24 | 14 | HighDEF |
| E011 | 影妃刺客（OCR不清） | 3 | 200 | 40 | 16 | 15 | FirstStrike(1) |
| E012 | 术士 | 3 | 220 | 38 | 18 | 16 | IgnoreDEF |
| E013 | meat（OCR不清） | 3 | 320 | 45 | 28 | 20 | Drain(8) |
| E014 | 镜质史莱姆 | 3 | 260 | 34 | 22 | 18 | Thorns(3) |
| E015 | 塔卫哨兵 | 3 | 400 | 48 | 32 | 25 | 无 |
| B010 | 10F楼层守卫 | 4 | 600 | 35 | 20 | 50 | 10F Boss |
| B020 | 20F楼层守卫 | 4 | 1200 | 50 | 35 | 120 | 20F Boss |
| B030 | 30F楼层守卫 | 4 | 2000 | 70 | 50 | 250 | BuffEveryN(5:+5ATK) |

敌人机制备注（来自说明页）：
- FirstStrike：先手额外 1 次伤害
- Drain：每回合吸血（E007=5，E013=8）
- IgnoreDEF：无视玩家DEF
- CurseATK：战后 ATK-1（触发一次）
- HighDEF：高防考验
- Thorns：每回合反伤 3
- 30F Boss：每 5 回合 ATK+5（可选实现）

### 5.2 道具（Items）
| ID | 名称 | 类型 | 效果 |
| --- | --- | --- | --- |
| 1001 | 红药水 | Consumable | HP+50 |
| 1002 | 蓝药水 | Consumable | HP+200 |
| 1003 | 黄药水 | Consumable | HP+500 |
| 1010 | 攻击宝石 | Permanent | ATK+1 |
| 1011 | 防御宝石 | Permanent | DEF+1 |
| 1012 | 生命宝石 | Permanent | MaxHP+20（可选实现为立即+20HP并回复同值） |
| 1020 | 黄钥匙 | Key | 开黄门 |
| 1021 | 蓝钥匙 | Key | 开蓝门 |
| 1022 | 红钥匙 | Key | 开红门 |
| 1030 | 神秘卷轴 | Special | RevealDamage：显示本层各敌人预计损失（UI功能） |
| 1031 | 时之沙 | Special | ResetFloor：重置当前楼层一次（每局最多1个） |

### 5.3 NPC
| ID | 名称 | 角色 | 备注 |
| --- | --- | --- | --- |
| N001 | 守塔人 | Guide | 引导与世界观提示 |
| N002 | 商人 | Shop | 出售升级与药水 |
| N003 | 记录碑 | Lore | 提供碎片文案与真结局线索（OCR不清） |

### 5.4 楼层（Floors 1-30）
楼层主题与模板概览（OCR只稳定识别推荐属性）：
- 1-10F：Template A，主题“起始区”，推荐属性约 ATK 5-16 / DEF 12-15 / HP 200-320
- 11-20F：Template B，主题“裂隙走廊”，推荐属性约 ATK 20-25 / DEF 18-22 / HP 450-600
- 21-30F：Template C，主题“逆旋迷宫”，推荐属性约 ATK 30-36 / DEF 28-34 / HP 850-1100
备注：10F、20F、30F 标记为 Boss 层

### 5.5 关卡放置（Placements）
Placements_1_30 提供 1-30F 的 9x9 格逐格配置：
- 每行对应一个格子：Floor, X, Y, CellType, ContentType, ContentID, Quantity
- 示例（Floor 1）：Start、Wall、Floor、Enemy(E001)、Item(1001/1020) 等均已配置

## 6. 未包含或需回源确认的部分
- ShopOffers 表结构被引用，但未在 PNG 中找到具体数据表
- Floors_1_30 中 Doors_Y/B/R、HasShop 字段未可靠识别
- 敌人部分名称与结局阈值存在 OCR 不清，需要回源核对

