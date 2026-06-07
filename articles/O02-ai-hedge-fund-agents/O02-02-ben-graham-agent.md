---
title: "Ben Graham Agent：证券分析之父的深度价值筛选法"
date: "2026-06-05"
author: "laozdao"
category: "O02"
tags: ["AI对冲基金", "格雷厄姆", "深度价值", "净净股", "Graham Number", "安全边际"]
status: "published"
version: "1.0"
summary: "深度解析AI Hedge Fund中的Ben Graham Agent——如何用LLM模拟格雷厄姆的防御型投资者策略，包括Graham Number估值、净净股筛选、收益稳定性分析等核心逻辑，并探讨其在A股小盘价值股中的适配性。"
difficulty: "intermediate"
reading_time: 12
series: "AI Hedge Fund Agent深度解析"
series_order: 02
---

# Ben Graham Agent：证券分析之父的深度价值筛选法

> "投资操作是基于深入分析，确保本金安全并获得适当回报的行为，不符合这些要求的都是投机。"——Benjamin Graham

---

## 一、投资者背景与哲学

### 1.1 格雷厄姆投资哲学核心

Benjamin Graham被誉为"证券分析之父"和"价值投资教父"，其哲学体系围绕两个核心角色展开：

| 投资者类型 | 核心特征 | Agent实现 |
|-----------|---------|----------|
| **防御型投资者** | 追求安全与稳定，分散持仓 | 严格的定量筛选规则 |
| **进取型投资者** | 愿意投入更多精力研究 | LLM定性深度分析 |

格雷厄姆的哲学建立在三个不可动摇的原则之上：

1. **安全边际**：以显著低于内在价值的价格买入，为估值误差留出缓冲
2. **市场先生**：将市场波动视为机会而非风险
3. **内在价值**：基于可量化的财务数据确定企业真实价值

### 1.2 Graham Number与净净股

格雷厄姆留给投资界两个经典的估值工具：

**Graham Number**：综合PE和PB的保守估值公式

```
Graham Number = sqrt(22.5 x EPS x BVPS)
```

其中22.5 = 15（目标PE上限）x 1.5（目标PB上限）。当股价低于Graham Number时，股票可能被低估。

**净净股（Net-Nets）**：更极端的深度价值策略

```
NCAV = 流动资产 - 全部负债
```

当市值低于NCAV时，意味着投资者可以以低于清算价值的价格买入企业。

---

## 二、Agent架构与实现

### 2.1 整体架构

```
┌─────────────────────────────────────────────────────────┐
│               Ben Graham Agent 架构                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌──────────┐   ┌──────────────┐   ┌───────────────┐   │
│  │ 财务数据  │──▶│ 三维评分体系  │──▶│ LLM定性分析    │   │
│  │ 获取模块  │   │ (满分15分)   │   │ (安全边际/前景) │   │
│  └──────────┘   └──────┬───────┘   └───────┬───────┘   │
│                         │                   │           │
│                         ▼                   ▼           │
│                  ┌─────────────────────────────┐        │
│                  │    综合评分 + 投资建议        │        │
│                  │    (满分15分, 信号阈值70%)    │        │
│                  └─────────────────────────────┘        │
└─────────────────────────────────────────────────────────┘
```

### 2.2 三维评分体系

Ben Graham Agent将评分分为三个维度，总分15分：

| 维度 | 最高分 | 评估重点 |
|------|-------|---------|
| **收益稳定性** | 4分 | 过去10年盈利的连续性和增长性 |
| **财务实力** | 5分 | 资产负债表健康度、流动性 |
| **估值吸引力** | 7分 | Graham Number、PE、PB、股息率 |

---

## 三、评分规则详解

### 3.1 收益稳定性（最高4分）

| 条件 | 得分 |
|------|------|
| 过去10年无亏损年度 | +2分 |
| 过去10年EPS年化增长>3% | +1分 |
| 过去3年EPS持续增长 | +1分 |

### 3.2 财务实力（最高5分）

| 条件 | 得分 |
|------|------|
| 流动比率 > 2.0 | +1分 |
| 当前比率 > 1.5 | +1分 |
| 长期债务/流动资产 < 1.0 | +1分 |
| 净净股（NCAV > 市值） | +2分 |

### 3.3 估值吸引力（最高7分）

| 条件 | 得分 |
|------|------|
| PE < 15 | +1分 |
| PB < 1.5 | +1分 |
| 股息率 > 2% | +1分 |
| 股价 < Graham Number | +2分 |
| EV/EBIT < 12 | +1分 |
| PEG < 1.5 | +1分 |

### 3.4 信号阈值

| 信号 | 阈值 | 含义 |
|------|------|------|
| Bullish | 综合评分 >= 70% (约10.5分) | 深度价值机会 |
| Bearish | 综合评分 <= 30% (约4.5分) | 估值过高 |
| Neutral | 介于两者之间 | 继续观望 |

---

## 四、代码实现分析

### 4.1 Graham Number计算

```python
import math

def calculate_graham_number(eps, bvps):
    """计算Graham Number"""
    if eps <= 0 or bvps <= 0:
        return 0
    graham_number = math.sqrt(22.5 * eps * bvps)
    return graham_number

def is_undervalued_by_graham(price, eps, bvps):
    """判断是否被Graham Number低估"""
    gn = calculate_graham_number(eps, bvps)
    return price < gn
```

### 4.2 净净股筛选

```python
def net_net_screen(current_assets, total_liabilities, market_cap):
    """净净股筛选"""
    ncav = current_assets - total_liabilities
    if ncav > market_cap:
        return True, ncav - market_cap  # 返回安全边际金额
    return False, market_cap - ncav
```

### 4.3 综合评分函数

```python
def ben_graham_score(financial_data, market_data):
    """格雷厄姆Agent综合评分"""
    score = 0

    # 收益稳定性（最高4分）
    if financial_data["loss_years_10yr"] == 0:
        score += 2
    if financial_data["eps_cagr_10yr"] > 0.03:
        score += 1
    if financial_data["eps_growth_3yr"]:
        score += 1

    # 财务实力（最高5分）
    if financial_data["current_ratio"] > 2.0:
        score += 1
    if financial_data["current_ratio"] > 1.5:
        score += 1
    if financial_data["long_debt_to_current_assets"] < 1.0:
        score += 1
    ncav = financial_data["current_assets"] - financial_data["total_liabilities"]
    if ncav > market_data["market_cap"]:
        score += 2  # 净净股加分

    # 估值吸引力（最高7分）
    if market_data["pe_ratio"] < 15:
        score += 1
    if market_data["pb_ratio"] < 1.5:
        score += 1
    if market_data["dividend_yield"] > 0.02:
        score += 1
    gn = calculate_graham_number(market_data["eps"], market_data["bvps"])
    if market_data["price"] < gn:
        score += 2
    if market_data["ev_ebit"] < 12:
        score += 1
    if market_data["peg_ratio"] < 1.5:
        score += 1

    return score  # 满分15分
```

---

## 五、与Dao Quant模型的对比启示

### 5.1 核心差异

| 维度 | Graham Agent | Dao Quant 双引擎四层 |
|------|-------------|---------------------|
| **估值哲学** | 极端保守，安全边际优先 | 相对估值，多因子平衡 |
| **筛选标准** | PE<15, PB<1.5硬性阈值 | 动态权重，无硬性阈值 |
| **适用标的** | 被低估的小盘股 | 全市场覆盖 |
| **技术分析** | 完全忽略 | 量价引擎25%权重 |
| **评分粒度** | 15分制，二元判断 | 100分制，连续评分 |

### 5.2 对Dao Quant的启示

1. **净净股策略可纳入极端低估识别**：当Dao Quant基本面评分极低但估值极便宜时，Graham的NCAV筛选可作为"抄底信号"
2. **Graham Number作为估值锚**：在Dao Quant的估值因子中增加Graham Number折扣率
3. **收益稳定性因子**：将"过去10年无亏损"作为财务健康度的加分项
4. **保守阈值设计**：Graham的硬性阈值思维可为Dao Quant提供极端情况的过滤标准

---

## 六、实战应用建议

### 6.1 A股小盘价值股适配性

格雷厄姆策略在A股小盘股中有天然适配性：

- **A股小盘股估值波动大**：更容易出现PE<15、PB<1.5的机会
- **壳资源价值**：A股退市制度下，小盘股存在额外隐性价值
- **流动性折价**：小盘股因流动性不足导致的折价，恰好提供安全边际

### 6.2 实战筛选流程

```
Graham策略A股筛选:
  1. 市值 < 50亿，排除ST股
  2. PE < 15 且 PB < 1.5
  3. 流动比率 > 1.5
  4. 过去3年无亏损
  5. NCAV > 市值（加分项）
  6. 综合评分 >= 10.5分 → 买入信号
```

### 6.3 局限性

- 净净股策略在牛市中严重跑输，需要极强的耐心
- Graham Number对高成长股不适用，可能错过优质成长机会
- A股财务数据质量参差不齐，需额外验证数据可靠性

---

## 参考文献

1. Graham, B. (1949). *The Intelligent Investor*. Harper & Brothers
2. Graham, B., Dodd, D. (1934). *Security Analysis*. McGraw-Hill
3. AI Hedge Fund GitHub Repository: [virattt/ai-hedge-fund](https://github.com/virattt/ai-hedge-fund)
4. 老子道量化研究系列：[M02-01-fundamental-engine-overview.md](../M02-fundamental-engine/M02-01-fundamental-engine-overview.md)
5. 老子道量化研究系列：[O01-11-ai-hedge-fund-legendary-investors.md](../O01-open-source-projects/O01-11-ai-hedge-fund-legendary-investors.md)

---

> **免责声明**：本文仅为学术研究和技术分析，不构成任何投资建议。AI Agent的投资决策基于历史数据和简化模型，实际投资需考虑更多因素。投资有风险，入市需谨慎。
