# 🏯 逆塔回响 (Echoes of the Inverted Tower)

> 一款魔塔 / 数值解谜 / 回合制 RPG，强调战斗结果可计算与资源决策驱动

[![文档状态](https://img.shields.io/badge/文档-整理中-yellow)]()
[![阶段](https://img.shields.io/badge/阶段-1--30F%20Demo-blue)]()

---

## 🎮 游戏简介

《逆塔回响》是一款以 **经典魔塔规则** 为核心的单机 RPG。玩家通过可计算的战斗与资源管理推进关卡，
在 9x9 格楼层中做出路线与消耗的权衡，形成“强复盘、强规划”的玩法体验。

### ✨ 核心特色

| 特色 | 说明 |
|------|------|
| 🧮 **可计算战斗** | 无随机伤害，战斗结果可预期 |
| 🔑 **资源决策** | HP/钥匙/金钱/路线等形成机会成本 |
| 🧭 **网格爬塔** | 9x9 逐格配置，关卡可迭代 |
| 🧩 **数值解谜** | 通过理解与规划通关，而非操作练习 |
| 🏁 **多结局挑战** | 支持速通/低属性通关等玩法 |

---

## 📁 文档结构

```
Echoes_of_the_Inverted_Tower_doc/
├── README.md                     # 项目文档说明（当前文件）
├── System_Doc_Pro.md             # 专业整理版系统文档
├── System_Doc.md                 # 基础整理版系统文档
├── converted/                    # OCR 清理后的可读数据
│   ├── README.md                 # 转换输出索引
│   ├── enemies.csv               # 敌人数据表
│   ├── items.csv                 # 道具数据表
│   ├── npcs.csv                  # NPC 数据表
│   ├── floors_1_30.csv           # 楼层摘要
│   ├── placements_1_30.csv       # 逐格放置（9x9）
│   ├── placements_summary.csv    # 逐层解析统计
│   ├── gdd_cleaned.md            # GDD 清理版文案
│   ├── config_readme_cleaned.md  # 配置 README 清理版
│   └── config_meta.csv           # 配置元信息
└── ocr_text/                     # OCR 原始文本
    ├── csv_png/                  # 表格类 OCR
    └── png/                      # 文档类 OCR
```

---

## 🚦 快速开始

### 对于策划/制作人
1. 阅读 `System_Doc_Pro.md`，了解核心系统与关键数值。
2. 查看 `converted/gdd_cleaned.md` 与 `converted/config_readme_cleaned.md`。
3. 使用 `converted/*.csv` 作为数值基线进行迭代。

### 对于程序开发
1. 以 `converted/enemies.csv`、`converted/items.csv`、`converted/npcs.csv` 作为数据导入源。
2. 关卡加载参考 `converted/placements_1_30.csv`（9x9 格子）。
3. 关卡配置与坐标规范见 `converted/config_meta.csv`。

---

## 📊 关键数据概览

| 项目 | 数值 |
|------|------|
| 楼层数量 | 30 |
| 网格尺寸 | 9x9 |
| 敌人总数 | 18（含 3 个 Boss） |
| 道具总数 | 11 |
| NPC 总数 | 3 |

---

## 🧾 数据来源说明

- 文档与数值来自 PNG OCR，已在 `converted/` 目录进行结构化清理。
- 个别名称或字段可能存在 OCR 噪声，建议以原图复核。

---

## ✅ 已知限制

- `placements_1_30.csv` 仍有少量格子未解析（详见 `converted/placements_summary.csv`）。
- `ShopOffers` 等表结构在素材中引用但未见完整数据。

