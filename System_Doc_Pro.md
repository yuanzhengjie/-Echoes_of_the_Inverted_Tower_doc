# 《逆塔回响》系统文档（专业整理版）

文档说明：
- 本文档基于 `C:\Users\XD\Downloads\doc` 的 PNG OCR 结果整理，主要来源为 GDD、README 与数值/楼层配置截图。
- OCR 存在少量识别噪声，所有“名称OCR不清/字段不清”已明确标注，建议回源校对。

## 1. 项目定位与目标
**项目性质**：独立单机  
**类型**：魔塔 / 数值解谜 / 回合制 RPG  
**平台**：PC（Windows/macOS）优先，预留移动端移植  

**核心卖点**
- 无随机战斗，战斗结果可计算、可预测。
- 资源有限（HP/钥匙/金钱/路线），每一步存在机会成本。
- 强复盘与挑战性，强调理解与规划而非操作练习。
- 支持多结局与极限挑战（速通/低属性通关）。

## 2. 核心系统

### 2.1 玩家资源与属性
- **HP**：生命值  
- **ATK**：攻击  
- **DEF**：防御  
- **GOLD**：金钱  
- **钥匙**：黄/蓝/红

### 2.2 战斗公式（经典魔塔）
- 玩家每回合伤害 = max(ATK - 敌DEF, 0)
- 敌人每回合伤害 = max(敌ATK - DEF, 0)
- 回合数 = ceil(敌HP / 玩家每回合伤害)
- 预计损失HP = (回合数 - 1) × 敌人每回合伤害
- 若玩家每回合伤害 = 0，则不可战胜

### 2.3 楼层节奏（1-30F Demo）
- **1-10F**：教学与基础计算；引入黄门与高防陷阱怪（铁甲卫）。
- **11-20F**：双路线与钥匙管理；引入蓝门、无视防御与吸血机制。
- **21-30F**：迷宫化布局与资源压缩；30F 为阶段 Boss。

## 3. 叙事与结局（草案）

### 3.1 NPC 角色（示例文案）
- **守塔人（引导）**：“塔不会阻止你，它只会记住你做过的每一次选择。”
- **商人（交易）**：“我卖的不是道具，是你未来少走的弯路。”
- **记录碑（碎片叙事）**：“当你以为自己在向上走时，塔正在把你推回更深处。”

### 3.2 结局
- **普通结局**：通关主线塔段。
- **理性结局**：不使用“时之沙”等重置类道具。
- **极限结局**：最终 ATK+DEF 达到阈值（OCR不清，需回源确认）。
- **真结局**：收集全部记录碑碎片并满足关键 NPC 存活条件（31F+扩展）。

## 4. 数据驱动与表结构

### 4.1 数据交付物
配置内容以 CSV 为主，包含：
- Enemies（敌人属性/机制）
- Items（道具/钥匙）
- NPCs（NPC与对话）
- ShopOffers（商人售卖/升级项，OCR未见具体表）
- Floors_1_30（楼层摘要/推荐属性/门/商店）
- Placements_1_30（9x9 逐格配置）
- BattleCalculator（Excel 战斗计算器）

### 4.2 坐标与格子规则
**网格**：9x9  
**坐标**：X=1..9, Y=1..9  
**CellType**：Wall / Floor / DoorY / DoorB / DoorR / Start / StairDown / StairUp  
**ContentType**：Enemy / Item / NPC / None  
**ContentID**：引用 Enemies/Items/NPCs 的 ID  
**Quantity**：默认 1

## 5. 数值表（OCR 汇总）

### 5.1 Enemies（敌人）
字段：EnemyID, Name, Tier, HP, ATK, DEF, Gold, SpecialType, SpecialParam, Notes

| ID | 名称 | Tier | HP | ATK | DEF | Gold | 特殊机制 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| E001 | 绿史莱姆 | 1 | 50 | 12 | 4 | 1 | 无 |
| E002 | 名称OCR不清（“ees”） | 1 | 60 | 14 | 5 | 2 | FirstStrike(0/1) |
| E003 | 名称OCR不清（“feed”） | 1 | 80 | 16 | 6 | 3 | 无 |
| E004 | 哥布林 | 1 | 90 | 18 | 8 | 4 | 无 |
| E005 | 铁甲卫 | 1 | 120 | 20 | 18 | 8 | HighDEF |
| E006 | 黑骑士 | 2 | 220 | 28 | 18 | 10 | 无 |
| E007 | 名称OCR不清（“吸血晤”） | 2 | 160 | 24 | 12 | 9 | Drain(5) |
| E008 | 法师学徒 | 2 | 140 | 30 | 8 | 10 | IgnoreDEF |
| E009 | 名称OCR不清（“出冲压路”） | 2 | 180 | 26 | 15 | 12 | CurseATK(1) |
| E010 | 石像魔像 | 2 | 260 | 30 | 24 | 14 | HighDEF |
| E011 | 名称OCR不清（“影为刺客”） | 3 | 200 | 40 | 16 | 15 | FirstStrike(1) |
| E012 | 名称OCR不清（“RE”） | 3 | 220 | 38 | 18 | 16 | IgnoreDEF |
| E013 | 名称OCR不清（“mest”） | 3 | 320 | 45 | 28 | 20 | Drain(8) |
| E014 | 镜质史莱姆 | 3 | 260 | 34 | 22 | 18 | Thorns(3) |
| E015 | 塔卫哨兵 | 3 | 400 | 48 | 32 | 25 | 无 |
| B010 | 10F楼层守卫 | 4 | 600 | 35 | 20 | 50 | 10F Boss |
| B020 | 20F楼层守卫 | 4 | 1200 | 50 | 35 | 120 | 20F Boss |
| B030 | 30F楼层守卫 | 4 | 2000 | 70 | 50 | 250 | BuffEveryN(5:+5ATK) |

**机制说明（来自说明页）**
- FirstStrike：先手额外 1 次伤害  
- Drain：每回合吸血  
- IgnoreDEF：无视玩家 DEF  
- CurseATK：战后 ATK-1（触发一次）  
- HighDEF：高防考验  
- Thorns：每回合反伤 3  
- 30F Boss：每 5 回合 ATK+5（可选实现）

### 5.2 Items（道具）
字段：ItemID, Name, Type, EffectHP, EffectATK, EffectDEF, EffectMaxHP, KeyColor, SpecialTag, Price, Notes

| ID | 名称 | 类型 | 效果 | 备注 |
| --- | --- | --- | --- | --- |
| 1001 | 红药水 | Consumable | HP+50 | 常见回复 |
| 1002 | 蓝药水 | Consumable | HP+200 | 中期回复 |
| 1003 | 黄药水 | Consumable | HP+500 | 后期回复 |
| 1010 | 攻击宝石 | Permanent | ATK+1 |  |
| 1011 | 防御宝石 | Permanent | DEF+1 |  |
| 1012 | 生命宝石 | Permanent | MaxHP+20 | 可选实现为立即+20HP上限并回复同值 |
| 1020 | 黄钥匙 | Key | 开黄门 |  |
| 1021 | 蓝钥匙 | Key | 开蓝门 |  |
| 1022 | 红钥匙 | Key | 开红门 |  |
| 1030 | 神秘卷轴 | Special | RevealDamage | 显示本层各敌人预计损失（UI功能） |
| 1031 | 时之沙 | Special | ResetFloor | 重置当前楼层一次（每局最多1个） |

### 5.3 NPC
字段：NpcID, Name, Role, DialogueKey, Notes

| ID | 名称 | 角色 | 对话键 | 备注 |
| --- | --- | --- | --- | --- |
| N001 | 守塔人 | Guide | TUTORIAL_GUARDIAN | 引导与世界观提示 |
| N002 | 商人 | Shop | SHOP_STANDARD | 出售升级与药水 |
| N003 | 记录碑（OCR不清） | Lore | LORE_OBELISK | 提供碎片文案与真结局线索 |

### 5.4 Floors（楼层摘要）
字段（OCR识别不完整）：Floor, Template, Theme, RecommendedATK, RecommendedDEF, RecommendedHP, Doors_Y/B/R, HasShop, HasLore, Notes

**稳定可读信息**：
- 1-10F：Template A，主题“起始区”，推荐属性约 ATK 5-16 / DEF 12-15 / HP 200-320
- 11-20F：Template B，主题“裂隙走廊”（OCR混杂），推荐属性约 ATK 20-25 / DEF 18-22 / HP 450-600
- 21-30F：Template C，主题“逆旋迷宫”，推荐属性约 ATK 30-36 / DEF 28-34 / HP 850-1100
- 10F / 20F / 30F 标记为 Boss 层

### 5.5 Placements（逐格配置）
Placements_1_30 以 9x9 逐格配置每层内容，典型字段：
- Floor, X, Y, CellType, ContentType, ContentID, Quantity, ScriptTag, Comment

**示例（Floor 1，节选）**
- (2,2) Start
- (3,3) Enemy E001
- (3,5) Item 1020
- (4,3) Item 1001
- (6,5) Enemy E003

## 6. 仍需回源确认的内容
- ShopOffers 表具体结构与数值
- Floors_1_30 中 Doors_Y/B/R、HasShop 等字段的具体数据
- 敌人名称中 OCR 不清部分（E002/E003/E007/E009/E011/E012/E013）
- 极限结局的 ATK+DEF 阈值

