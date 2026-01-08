# 转换输出索引

本目录由 OCR 文本整理生成，优先使用 csv_png 的表格 OCR；Placements 使用 png 版逐格数据。

## 原始数据 (OCR 整理)

- enemies.csv：敌人数据
- items.csv：道具数据（含 Notes）
- npcs.csv：NPC 数据
- floors_1_30.csv：楼层摘要 (**已补充门数量/商店/记录碑字段**)
- placements_1_30.csv：逐格放置（9x9）
- placements_summary.csv：逐层格子解析统计
- gdd_cleaned.md：GDD 文案清理版
- config_readme_cleaned.md：配置 README 清理版
- config_meta.csv：配置元信息
- placements_unparsed.txt：未能解析的原始行（可选复核）

## 补充数据 (2026-01-08 新增)

以下文件为策划文档补充模板，标记 `NeedsConfirm=Y` 的字段需要确认：

- **PlayerConfig.csv**：玩家初始属性配置（HP/ATK/DEF/Gold/钥匙）
- **ShopOffers.csv**：商店售卖配置（升级/道具/钥匙价格）
- **DoorConfig.csv**：门消耗规则配置
- **EnemyNameCorrections.csv**：敌人名称 OCR 校对表
- **GameMechanics.md**：游戏机制说明文档（战斗/移动/存档/结局等待确认项）

## 使用说明

1. 所有 `NeedsConfirm=Y` 的数据为推测值，需策划确认
2. `EnemyNameCorrections.csv` 中 `Confirmed=N` 的名称需回源校对
3. `GameMechanics.md` 中 ❓ 标记的项目需要决策

## 更新日志

| 日期 | 内容 |
|------|------|
| 2026-01-08 | 新增 5 个补充配置文件，更新 floors_1_30.csv |
