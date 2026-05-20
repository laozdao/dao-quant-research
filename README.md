# Dao Quant Research · 道·量化研究

<p align="center">
  <strong>"道生一，一生二，二生三，三生万物"</strong><br>
  <em>以东方哲学思维洞察 A 股市场，以量化严谨方法评估投资价值</em>
</p>

<p align="center">
  <a href="#-文章分类目录">📖 文章目录</a> ·
  <a href="#-模型概述">🏗️ 模型架构</a> ·
  <a href="WRITING-GUIDELINES.md">📝 写作规范</a> ·
  <a href="#-快速开始">🚀 快速开始</a>
</p>

---

## 🎯 关于本仓库

**Dao Quant Research** 是一个专注于 **中国 A 股市场量化分析** 的研究知识库，收录 100+ 篇 Markdown 格式的研究文章，系统性地记录和分享基于"双引擎四层融合模型"的量化分析研究。

### 核心方法论：双引擎四层融合模型

```
                        ┌──────────────────┐
                        │   综合评分输出     │
                        │  0-100 分 / 五档评级 │
                        └────────┬─────────┘
                                 │
           ┌─────────────────────┼─────────────────────┐
           │                     │                     │
           ▼                     ▼                     ▼
      ┌─────────┐          ┌─────────┐          ┌─────────┐
      │ 基本面引擎 │          │ 量价引擎  │          │ 风控引擎  │
      │   60%   │    +     │   25%   │    +     │   15%   │
      └────┬────┘          └────┬────┘          └────┬────┘
           │                     │                     │
      ┌────┴────┐          ┌────┴────┐          ┌────┴────┐
      │ 盈利能力  │          │ 趋势分析  │          │ 波动率   │
      │ 成长能力  │          │ 量价配合  │          │ 回撤控制  │
      │ 估值水平  │          │ 资金流向  │          │ 集中度   │
      │ 财务健康  │          │ 筹码分布  │          │ 流动性   │
      └─────────┘          └─────────┘          └─────────┘
```

### 哲学映射

| 哲学概念 | 量化映射 |
|---------|---------|
| **道法自然** | 尊重市场规律，不预测只评估 |
| **阴阳平衡** | 多空因子均衡配置，攻守兼备 |
| **无为而治** | 系统化评分，减少主观干预 |
| **大道至简** | 三层架构清晰可解释，不搞黑箱 |

---

## 📖 文章目录

### 🆕 最新文章

> 精选推荐与最新发布的研究文章

| 文章 | 分类 | 日期 | 难度 | 摘要 |
|------|------|------|------|------|
| **[老子道·芯鉴九维投资分析模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md)** | I05 | 2026-05-20 | 高级 | 以道家哲学为底层逻辑，构建覆盖电子行业五大细分赛道（半导体设备、AI芯片、半导体材料、存储芯片、消费电子）的九维量化投资分析模型 |

---

### 📚 完整分类目录

> 共收录 **1** 篇研究文章（持续更新中），按 **4 大板块、27 个子分类** 组织

#### M - 模型理论（Model Theory）

关于双引擎四层融合模型的理论阐述与方法论

| 子分类 | 代码 | 文章数 | 说明 |
|--------|------|:------:|------|
| [模型总览](./articles/M01-model-overview/) | M01 | — | 模型架构、设计哲学、整体介绍 |
| [基本面引擎](./articles/M02-fundamental-engine/) | M02 | — | 盈利能力、成长能力、估值、财务健康 |
| [量价引擎](./articles/M03-volume-price-engine/) | M03 | — | 趋势分析、量价配合、资金流向、筹码分布 |
| [风控引擎](./articles/M04-risk-control/) | M04 | — | 波动率、回撤控制、集中度、流动性风险 |
| [融合算法](./articles/M05-fusion-algorithm/) | M05 | — | 加权机制、评级映射、动态调整 |
| [因子检验](./articles/M06-factor-validation/) | M06 | — | 单因子有效性、IC测试、分层回测 |
| [模型迭代](./articles/M07-model-iteration/) | M07 | — | 版本更新、改进记录、回测对比 |

#### I - 行业研究（Industry Research）

特定行业的量化分析框架与案例研究

| 子分类 | 代码 | 文章数 | 说明 | 最新文章 |
|--------|------|:------:|------|---------|
| [银行业](./articles/I01-banking/) | I01 | — | 银行板块因子适配、特色指标 | — |
| [非银金融](./articles/I02-nonbank-finance/) | I02 | — | 保险、证券、多元金融 | — |
| [房地产](./articles/I03-real-estate/) | I03 | — | 房企量化分析、三道红线 | — |
| [医药生物](./articles/I04-pharma/) | I04 | — | 创新药、医疗器械、CXO | — |
| [电子半导体](./articles/I05-semiconductor/) | I05 | **1** | 芯片、消费电子、半导体设备 | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) |
| [新能源](./articles/I06-new-energy/) | I06 | — | 光伏、锂电、储能、新能源车 | — |
| [消费](./articles/I07-consumer/) | I07 | — | 白酒、食品饮料、家电 | — |
| [周期](./articles/I08-cyclical/) | I08 | — | 钢铁、煤炭、化工、有色 | — |
| [TMT](./articles/I09-tmt/) | I09 | — | 互联网、软件、传媒、通信 | — |
| [制造](./articles/I10-manufacturing/) | I10 | — | 机械、汽车、军工、电力设备 | — |

#### C - 个股案例（Case Studies）

具体股票的深度量化评分分析

| 子分类 | 代码 | 文章数 | 说明 |
|--------|------|:------:|------|
| [沪深300成分](./articles/C01-hs300/) | C01 | — | 大盘股深度分析 |
| [中证500成分](./articles/C02-zz500/) | C02 | — | 中盘股深度分析 |
| [创业板指](./articles/C03-chinext/) | C03 | — | 成长股深度分析 |
| [科创板](./articles/C04-star/) | C04 | — | 硬科技企业分析 |
| [北交所](./articles/C05-bse/) | C05 | — | 专精特新企业分析 |

#### R - 研究方法论（Research Methods）

研究工具、方法、思路与学术随笔

| 子分类 | 代码 | 文章数 | 说明 |
|--------|------|:------:|------|
| [研究工具](./articles/R01-tools/) | R01 | — | 数据源、Python库、可视化工具 |
| [数据处理](./articles/R02-data-processing/) | R02 | — | 数据清洗、特征工程、标准化 |
| [回测方法](./articles/R03-backtesting/) | R03 | — | 回测框架、评价指标、过拟合防范 |
| [研究随笔](./articles/R04-essays/) | R04 | — | 投资感悟、市场观察、失败总结 |
| [文献综述](./articles/R05-literature/) | R05 | — | 经典论文解读、学术前沿 |

---

### 📖 阅读指南

#### 按读者类型选择

| 读者类型 | 推荐阅读 | 说明 |
|---------|---------|------|
| **初学者** | 模型理论（M系列）→ 行业研究（I系列） | 先理解方法论，再看行业应用 |
| **行业研究员** | 行业研究（I系列）→ 个股案例（C系列） | 关注特定行业的分析框架 |
| **量化开发者** | 研究方法论（R系列）→ 模型理论（M系列） | 关注工具、数据和回测方法 |
| **投资者** | 个股案例（C系列）→ 行业研究（I系列） | 直接查看投资标的分析 |

#### 按投资场景选择

| 投资场景 | 推荐文章 | 说明 |
|---------|---------|------|
| **行业配置** | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) | 电子行业五大细分赛道对比分析 |
| **个股选择** | 个股案例（C系列） | 具体股票的深度量化评分 |
| **模型构建** | 模型理论（M系列） | 学习双引擎四层融合模型 |
| **工具方法** | 研究方法论（R系列） | 数据处理、回测框架等 |

#### 文章难度标识

| 标识 | 难度 | 适合读者 |
|------|------|---------|
| 🟢 初级 | beginner | 量化投资新手 |
| 🟡 中级 | intermediate | 有一定基础的投资者 |
| 🔴 高级 | advanced | 专业量化研究者 |

> 💡 **提示**：每篇文章的 Frontmatter 中都标注了 `difficulty` 和 `reading_time`，可根据自身情况选择。

---

## 📁 仓库结构

```
dao-quant-research/
├── README.md                          # 本文件：仓库首页
├── WRITING-GUIDELINES.md              # 写作规范（必读）
├── LICENSE                            # MIT 许可证
├── CITATION.cff                       # 引用信息
├── .gitignore                         # Git 忽略规则
│
├── articles/                          # 📖 研究文章（核心目录）
│   ├── M01-model-overview/            # 模型总览
│   ├── M02-fundamental-engine/        # 基本面引擎
│   ├── M03-volume-price-engine/       # 量价引擎
│   ├── M04-risk-control/              # 风控引擎
│   ├── M05-fusion-algorithm/          # 融合算法
│   ├── M06-factor-validation/         # 因子检验
│   ├── M07-model-iteration/           # 模型迭代
│   ├── I01-banking/                   # 银行业
│   ├── I02-nonbank-finance/           # 非银金融
│   ├── I03-real-estate/               # 房地产
│   ├── I04-pharma/                    # 医药生物
│   ├── I05-semiconductor/             # 电子半导体
│   ├── I06-new-energy/                # 新能源
│   ├── I07-consumer/                  # 消费
│   ├── I08-cyclical/                  # 周期
│   ├── I09-tmt/                       # TMT
│   ├── I10-manufacturing/             # 制造
│   ├── C01-hs300/                     # 沪深300成分股
│   ├── C02-zz500/                     # 中证500成分股
│   ├── C03-chinext/                   # 创业板指
│   ├── C04-star/                      # 科创板
│   ├── C05-bse/                       # 北交所
│   ├── R01-tools/                     # 研究工具
│   ├── R02-data-processing/           # 数据处理
│   ├── R03-backtesting/               # 回测方法
│   ├── R04-essays/                    # 研究随笔
│   └── R05-literature/                # 文献综述
│
├── assets/                            # 🖼️ 文章资源
│   ├── figures/                       # 图表图片
│   │   ├── M01/                       # 按分类存放
│   │   ├── M02/
│   │   └── ...
│   └── tables/                        # 数据表格
│       ├── M01/
│       └── ...
│
├── data/                              # 📊 研究数据（不纳入版本控制）
│   └── .gitkeep
│
├── templates/                         # 📝 文章模板
│   ├── article-template.md            # 标准文章模板
│   └── series-template.md             # 系列文章模板
│
└── scripts/                           # 🔧 辅助脚本
    ├── generate-index.py              # 生成文章索引
    ├── check-frontmatter.py           # 检查 Frontmatter
    └── update-readme.py               # 更新 README 目录
```

---

## 🚀 快速开始

### 对于读者

1. **按分类浏览**：点击上方目录中的分类链接
2. **按标签搜索**：查看文章 Frontmatter 中的 `tags` 字段
3. **查看最新**：关注 `status: published` 且日期较新的文章

### 对于贡献者

1. **阅读写作规范**：[WRITING-GUIDELINES.md](./WRITING-GUIDELINES.md)
2. **复制文章模板**：[templates/article-template.md](./templates/article-template.md)
3. **选择分类目录**：根据内容选择 `articles/` 下的对应子目录
4. **按规范命名文件**：`{分类代码}-{序号}-{kebab-case标题}.md`
5. **填写 Frontmatter**：所有必填字段必须完整
6. **质量检查**：对照规范中的检查清单自查

---

## 📝 写作规范速查

### 文件命名
```
M01-01-model-overview.md           # 模型总览第1篇
I07-02-baijiu-sector.md            # 行业研究-消费-白酒板块
C01-05-pingan-bank.md              # 个股案例-平安银行
```

### Frontmatter 必填字段
```yaml
---
title: "文章标题"
date: "2025-01-15"
author: "laozdao"
category: "M01"                       # 分类代码
tags: ["标签1", "标签2", "标签3"]
status: "published"                   # draft/published/archived/updated
version: "1.0"
summary: "100-200字摘要"
---
```

### 文章结构
```markdown
# 标题

> 一句话摘要（30字以内）

## 一、引言/背景
## 二、核心内容
## 三、结论与启示
## 四、附录（可选）
## 参考文献

---
> ⚠️ 免责声明：本文仅供学习研究，不构成投资建议。
```

---

## 📋 引用方式

如果本仓库的研究内容对您有帮助，请引用：

### BibTeX

```bibtex
@misc{laozdao2025daoquant,
  author       = {laozdao},
  title        = {Dao Quant Research: A-Share Quantitative Analysis Study},
  year         = {2026},
  howpublished = {\url{https://github.com/laozdao/dao-quant-research}},
  note         = {Accessed: YYYY-MM-DD}
}
```

### APA

> laozdao. (2026). *Dao Quant Research: A-Share Quantitative Analysis Study*. GitHub. https://github.com/laozdao/dao-quant-research

---

## 🤝 参与贡献

欢迎通过以下方式参与：

- 🐛 提交 [Issue](../../issues) — 报告错误或提出建议
- 📝 提交 PR — 按写作规范提交新文章或修正
- 💬 讨论 — 在 Issue 中分享你的量化研究心得

---

## 📄 许可证

本仓库采用 [MIT License](./LICENSE) 开源。

---

## ⚠️ 免责声明

> **本仓库所有研究文章仅供学习研究交流，不构成任何投资建议。**
>
> - 模型评分基于历史数据，过往表现不代表未来收益
> - 股市有风险，投资需谨慎
> - 作者不对因使用本仓库内容而造成的任何损失承担责任
