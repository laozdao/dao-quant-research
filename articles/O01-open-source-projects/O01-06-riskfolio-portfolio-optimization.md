---
title: "Riskfolio-Lib：Python投资组合优化库深度解析"
date: "2026-05-29"
author: "laozdao"
category: "O01"
tags: ["开源项目", "Riskfolio-Lib", "Python", "投资组合优化", "现代投资组合理论", "风险平价", "CVaR", "Black-Litterman"]
status: "published"
version: "1.0"
summary: "深度解析Riskfolio-Lib——基于Python的专业投资组合优化库，支持现代投资组合理论、风险预算、Black-Litterman模型等高级方法，涵盖数学原理、核心功能、实战应用及与Dao Quant模型的结合。"
difficulty: "intermediate"
reading_time: 40
series: "量化交易开源项目"
series_order: 6
---

# Riskfolio-Lib：Python投资组合优化库深度解析

> 从马科维茨均值-方差到风险平价，从Black-Litterman到因子投资——Riskfolio-Lib用优雅的Python接口封装了现代投资组合理论的精华。

---

## 一、项目概览

### 1.1 基本信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | Riskfolio-Lib |
| **开发团队** | Dany Cajas (个人开发者) |
| **GitHub** | [dcajasn/Riskfolio-Lib](https://github.com/dcajasn/Riskfolio-Lib) |
| **文档网站** | [https://riskfolio-lib.readthedocs.io](https://riskfolio-lib.readthedocs.io) |
| **许可证** | BSD-3-Clause |
| **主要语言** | Python |
| **当前版本** | v6.x（持续活跃维护）
| **Stars** | 3,500+，活跃社区
| **依赖库** | NumPy, Pandas, CVXPY, MOSEK/ECOS/SCS |

### 1.2 项目定位

Riskfolio-Lib 是一个**开源的Python库，专注于投资组合优化和资产配置**，基于现代投资组合理论(MPT)和风险管理的最新研究成果。

> **核心价值**：提供从经典均值-方差优化到前沿风险预算方法的完整工具集，帮助投资者构建风险调整后收益最优的投资组合。

### 1.3 与 Dao Quant 模型的关系

| 维度 | Riskfolio-Lib | Dao Quant 双引擎四层 |
|-----|--------------|---------------------|
| **核心能力** | 组合优化、资产配置 | 个股评分、风险识别 |
| **分析层级** | 组合层面 | 个股层面 |
| **输入数据** | 收益率、协方差矩阵 | 基本面、量价数据 |
| **输出结果** | 最优权重 | 投资评级 |
| **互补价值** | 为Dao Quant选股结果提供最优配置 | 为组合优化提供高质量输入 |

---

## 二、理论基础

### 2.1 现代投资组合理论 (MPT)

```
┌─────────────────────────────────────────────────────────────┐
│                现代投资组合理论核心框架                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  马科维茨均值-方差模型 (1952)                                │
│  ───────────────────────────                                │
│                                                             │
│  目标：在给定风险水平下最大化收益                            │
│       或在给定收益水平下最小化风险                           │
│                                                             │
│  数学表达：                                                  │
│  min   w'Σw                                                 │
│  s.t.  w'μ = R_target                                       │
│        Σw = 1                                               │
│        w ≥ 0  (可选)                                        │
│                                                             │
│  其中：                                                      │
│  - w: 资产权重向量                                          │
│  - Σ: 收益率协方差矩阵                                      │
│  - μ: 预期收益率向量                                        │
│  - R_target: 目标收益率                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 风险度量演进

| 风险度量 | 定义 | 特点 |
|---------|------|------|
| **标准差 (σ)** | 收益率波动 | 经典MPT度量，对称性 |
| **半方差** | 下行波动 | 只关注损失 |
| **VaR** | 分位数损失 | 直观但不满足次可加性 |
| **CVaR/ES** | 条件VaR/预期损失 | 满足一致性风险度量 |
| **CDaR** | 条件回撤 | 关注最大回撤 |
| **风险贡献** | 各资产对组合风险贡献 | 风险预算基础 |

### 2.3 优化方法分类

```
┌─────────────────────────────────────────────────────────────┐
│                Riskfolio-Lib 优化方法体系                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. 经典方法                                                 │
│     ├── 均值-方差优化 (Mean-Variance)                       │
│     ├── 均值-CVaR优化                                       │
│     └── 均值-CDaR优化                                       │
│                                                             │
│  2. 风险预算方法                                             │
│     ├── 风险平价 (Risk Parity)                              │
│     ├── 风险预算 (Risk Budgeting)                           │
│     └── 逆波动率加权                                        │
│                                                             │
│  3. 贝叶斯方法                                               │
│     └── Black-Litterman模型                                 │
│                                                             │
│  4. 因子模型                                                 │
│     ├── 统计因子 (PCA)                                      │
│     └── 宏观因子                                            │
│                                                             │
│  5. 鲁棒优化                                                 │
│     ├── 不确定性集方法                                      │
│     └── 最坏情况优化                                        │
│                                                             │
│  6. 多期优化                                                 │
│     └── 动态资产配置                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 三、核心功能详解

### 3.1 均值-方差优化

```python
import riskfolio as rp
import pandas as pd
import numpy as np

# 准备收益率数据
# returns: DataFrame，列为资产，行为时间
returns = pd.read_csv('asset_returns.csv', index_col=0, parse_dates=True)

# 创建投资组合对象
port = rp.Portfolio(returns=returns)

# 计算预期收益和协方差
port.assets_stats(method_mu='hist', method_cov='ledoit')
# method_mu: 'hist'(历史), 'ewma1'(指数加权), 'ewma2'...
# method_cov: 'hist', 'ledoit'(收缩估计), 'oas', 'shrunk'...

# 经典均值-方差优化
w = port.optimization(
    model='Classic',           # 经典模型
    rm='MV',                   # 风险度量: MV(方差), CVaR, CDaR...
    obj='Sharpe',              # 目标: Sharpe(最大夏普), MinRisk, MaxRet
    rf=0.02,                   # 无风险利率
    l=0,                       # 风险厌恶系数
    hist=True                  # 使用历史数据
)

print(w)
```

### 3.2 风险平价与风险预算

```python
# 风险平价 (等风险贡献)
w_rp = port.rp_optimization(
    model='Classic',
    rm='MV',
    rf=0.02,
    b=None  # b=None表示等风险贡献
)

# 自定义风险预算
risk_budget = pd.Series({
    'Asset_A': 0.4,   # 40%的风险预算
    'Asset_B': 0.3,   # 30%的风险预算
    'Asset_C': 0.2,   # 20%的风险预算
    'Asset_D': 0.1    # 10%的风险预算
})

w_rb = port.rp_optimization(
    model='Classic',
    rm='CVaR',         # 使用CVaR作为风险度量
    rf=0.02,
    b=risk_budget      # 自定义风险预算
)
```

#### 风险平价原理

```
风险平价核心思想：使每个资产对组合总风险的贡献相等

数学表达：
- 组合风险: R_p = √(w'Σw)
- 资产i的风险贡献: RC_i = w_i * (Σw)_i / R_p
- 风险平价条件: RC_i = RC_j 对所有i,j

特点：
1. 不依赖预期收益估计（更稳健）
2. 天然分散化
3. 杠杆后可获得较高夏普比率
```

### 3.3 Black-Litterman模型

Black-Litterman模型结合市场均衡收益和投资者观点，解决均值-方差优化对输入敏感的缺陷。

```python
# 定义投资者观点
# P: 观点矩阵 (K x N)，K个观点，N个资产
# Q: 观点收益向量 (K x 1)
# Omega: 观点不确定性矩阵 (K x K)

# 示例观点：
# 1. 资产A比资产B收益高2%
# 2. 资产C收益率为5%

P = pd.DataFrame([
    [1, -1, 0, 0],    # 观点1: Asset_A - Asset_B = 2%
    [0, 0, 1, 0]      # 观点2: Asset_C = 5%
], columns=returns.columns)

Q = pd.Series([0.02, 0.05])

# 使用Black-Litterman模型
bl_model = rp.BlackLitterman(
    X=returns,
    P=P,
    Q=Q,
    rf=0.02,
    w=market_weights,  # 市场均衡权重
    delta=2.5          # 风险厌恶系数
)

# 获取后验收益和协方差
bl_mu, bl_cov = bl_model.bl_model()

# 使用BL结果进行优化
port.mu = bl_mu
port.cov = bl_cov
w_bl = port.optimization(model='BL')
```

### 3.4 多因子模型

```python
# 基于因子的组合优化

# 准备因子数据
factors = pd.read_csv('factor_returns.csv', index_col=0, parse_dates=True)
exposures = pd.read_csv('factor_exposures.csv', index_col=0)

# 创建因子模型
port = rp.Portfolio(returns=returns)

# 使用PCA提取统计因子
port.factors = factors
port.factors_stats(method_mu='hist', method_cov='ledoit')

# 因子暴露优化
w_factor = port.optimization(
    model='FC',           # Factor Contribution模型
    rm='MV',
    obj='Sharpe',
    rf=0.02
)
```

### 3.5 风险度量对比

```python
# 不同风险度量下的优化结果对比

risk_measures = ['MV', 'MAD', 'MSV', 'FLPM', 'SLPM', 
                 'CVaR', 'EVaR', 'WR', 'MDD', 'ADD', 'CDaR']

results = {}

for rm in risk_measures:
    w = port.optimization(
        model='Classic',
        rm=rm,
        obj='Sharpe',
        rf=0.02
    )
    results[rm] = w

# 对比结果
comparison = pd.concat(results, axis=1)
print(comparison)
```

| 风险度量 | 全称 | 说明 |
|---------|------|------|
| **MV** | Mean-Variance | 方差 |
| **MAD** | Mean Absolute Deviation | 平均绝对偏差 |
| **MSV** | Semi Variance | 半方差 |
| **FLPM** | First Lower Partial Moment | 一阶下偏矩 |
| **SLPM** | Second Lower Partial Moment | 二阶下偏矩 |
| **CVaR** | Conditional Value at Risk | 条件风险价值 |
| **EVaR** | Entropic Value at Risk | 熵风险价值 |
| **WR** | Worst Realization | 最坏实现 |
| **MDD** | Maximum Drawdown | 最大回撤 |
| **ADD** | Average Drawdown | 平均回撤 |
| **CDaR** | Conditional Drawdown at Risk | 条件回撤风险 |

---

## 四、快速入门

### 4.1 安装

```bash
# 基础安装
pip install riskfolio-lib

# 完整安装（包含所有求解器）
pip install riskfolio-lib[full]

# 从源码安装
git clone https://github.com/dcajasn/Riskfolio-Lib.git
cd Riskfolio-Lib
pip install .
```

### 4.2 完整示例：构建最优组合

```python
import riskfolio as rp
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 1. 准备数据
# 下载资产收益率数据（示例使用Yahoo Finance数据）
assets = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'TSLA', 
          'JPM', 'BAC', 'WFC', 'GS', 'C',
          'JNJ', 'PFE', 'UNH', 'ABBV', 'MRK',
          'XOM', 'CVX', 'COP', 'EOG', 'SLB']

# 假设returns是已获取的日收益率DataFrame
returns = pd.read_csv('returns.csv', index_col=0, parse_dates=True)

# 2. 创建投资组合
port = rp.Portfolio(returns=returns)

# 3. 计算统计量
port.assets_stats(method_mu='hist', method_cov='ledoit')

# 4. 均值-方差优化（最大夏普比率）
w_mv = port.optimization(
    model='Classic',
    rm='MV',
    obj='Sharpe',
    rf=0.02
)

# 5. 风险平价
w_rp = port.rp_optimization(
    model='Classic',
    rm='MV'
)

# 6. 结果可视化
fig, ax = plt.subplots(2, 1, figsize=(12, 10))

# 权重对比
comparison = pd.concat([w_mv, w_rp], axis=1)
comparison.columns = ['Mean-Variance', 'Risk Parity']
comparison.plot(kind='bar', ax=ax[0])
ax[0].set_title('Portfolio Weights Comparison')
ax[0].set_ylabel('Weight')

# 有效前沿
points = 50
frontier = port.efficient_frontetier(
    model='Classic',
    rm='MV',
    points=points,
    rf=0.02
)

# 绘制有效前沿
ax[1].scatter(frontier['Risk'], frontier['Returns'], c=frontier['Sharpe'], 
              cmap='viridis', marker='o', s=10)
ax[1].plot(port.risk, port.ret, 'r*', markersize=15, label='Max Sharpe')
ax[1].set_xlabel('Risk (Standard Deviation)')
ax[1].set_ylabel('Expected Return')
ax[1].set_title('Efficient Frontier')
ax[1].legend()
ax[1].grid()

plt.tight_layout()
plt.show()

# 7. 组合分析
print("均值-方差组合:")
print(f"  预期收益: {port.ret:.4f}")
print(f"  风险: {port.risk:.4f}")
print(f"  夏普比率: {port.sharpe:.4f}")

print("\n风险平价组合:")
# 计算风险平价组合指标
rp_ret = returns @ w_rp
print(f"  预期收益: {rp_ret.mean() * 252:.4f}")
print(f"  风险: {rp_ret.std() * np.sqrt(252):.4f}")
print(f"  夏普比率: {(rp_ret.mean() * 252 - 0.02) / (rp_ret.std() * np.sqrt(252)):.4f}")
```

### 4.3 高级功能：约束条件

```python
# 添加各种约束条件

# 1. 权重上下限
port.lowerret = 0.10  # 最低预期收益10%
port.upperdev = 0.15  # 最高标准差15%

# 2. 资产类别约束
# 定义资产类别
asset_classes = {
    'AAPL': 'Tech', 'MSFT': 'Tech', 'GOOGL': 'Tech', 'AMZN': 'Tech', 'TSLA': 'Tech',
    'JPM': 'Finance', 'BAC': 'Finance', 'WFC': 'Finance', 'GS': 'Finance', 'C': 'Finance',
    'JNJ': 'Health', 'PFE': 'Health', 'UNH': 'Health', 'ABBV': 'Health', 'MRK': 'Health',
    'XOM': 'Energy', 'CVX': 'Energy', 'COP': 'Energy', 'EOG': 'Energy', 'SLB': 'Energy'
}

# 类别约束：每个行业不超过40%
class_constraints = pd.DataFrame({
    'Class': ['Tech', 'Finance', 'Health', 'Energy'],
    'Type': ['<=', '<=', '<=', '<='],
    'Limit': [0.40, 0.40, 0.40, 0.40]
})

port.asset_classes = asset_classes
port.class_constraints = class_constraints

# 3. 优化
w_constrained = port.optimization(
    model='Classic',
    rm='CVaR',
    obj='Sharpe',
    rf=0.02
)
```

---

## 五、项目评估

### 5.1 优势分析

| 优势 | 说明 |
|-----|------|
| **方法全面** | 覆盖从经典MPT到前沿风险预算方法 |
| **实现优雅** | 基于CVXPY，数学表达清晰 |
| **文档详尽** | 官方文档包含丰富示例和理论解释 |
| **求解器灵活** | 支持多种优化求解器（MOSEK、ECOS、SCS、OSQP） |
| **可视化强** | 内置多种组合分析图表 |
| **持续维护** | 版本迭代活跃，社区响应及时 |
| **学术支撑** | 基于严谨的金融理论 |
| **免费开源** | BSD许可证，商业友好 |

### 5.2 局限与注意事项

| 局限 | 说明 | 应对建议 |
|-----|------|---------|
| **收益估计** | 历史收益不代表未来 | 结合基本面分析和Dao Quant评分 |
| **协方差估计** | 高维协方差矩阵估计困难 | 使用Ledoit-Wolf等收缩估计 |
| **参数敏感** | 部分方法对输入参数敏感 | 进行稳健性检验 |
| **交易成本** | 未考虑交易费用和滑点 | 在实盘前进行成本调整 |
| **动态调整** | 静态优化，未考虑时变特性 | 定期再平衡或使用滚动窗口 |

### 5.3 与 Dao Quant 的结合应用

```
┌─────────────────────────────────────────────────────────────┐
│           Riskfolio-Lib + Dao Quant 综合应用框架            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Step 1: Dao Quant 股票筛选                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  双引擎四层模型                                        │   │
│  │  - 基本面分析 (60%) → 财务健康评分                    │   │
│  │  - 量价分析 (25%) → 技术动量评分                      │   │
│  │  - 风险控制 (15%) → 风险等级评定                      │   │
│  └─────────────────────────────────────────────────────┘   │
│                            ↓                                │
│  Step 2: 构建候选池                                          │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  筛选条件：                                            │   │
│  │  - 综合评分 > 75                                       │   │
│  │  - 风险等级 <= Medium                                  │   │
│  │  - 行业分散                                            │   │
│  └─────────────────────────────────────────────────────┘   │
│                            ↓                                │
│  Step 3: 数据准备                                            │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  - 获取候选股票历史收益率                              │   │
│  │  - 计算预期收益和协方差矩阵                            │   │
│  │  - 可使用Dao Quant评分作为收益先验                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                            ↓                                │
│  Step 4: Riskfolio-Lib 组合优化                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  优化方法选择：                                        │   │
│  │  - 均值-CVaR优化（稳健性）                             │   │
│  │  - 风险平价（分散化）                                  │   │
│  │  - Black-Litterman（融入观点）                         │   │
│  └─────────────────────────────────────────────────────┘   │
│                            ↓                                │
│  Step 5: 组合分析与监控                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  - 风险贡献分析                                        │   │
│  │  - 情景分析                                            │   │
│  │  - 定期再平衡                                          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**应用建议**：
1. 使用 Dao Quant 进行**个股筛选**和**质量评估**，构建高质量候选池
2. 使用 Riskfolio-Lib 进行**组合优化**，确定最优权重配置
3. 将 Dao Quant 评分作为 Black-Litterman 模型的**投资者观点**输入
4. 使用 Riskfolio-Lib 的**风险贡献分析**验证组合风险分散效果

---

## 六、典型案例

### 案例1：Dao Quant评分驱动的风险平价组合

```python
import riskfolio as rp
import pandas as pd
import numpy as np

# 假设已从Dao Quant获取股票评分
dao_scores = {
    '000001.SZ': 82, '000002.SZ': 78, '000063.SZ': 85,
    '000100.SZ': 72, '000333.SZ': 88, '000538.SZ': 80,
    '000568.SZ': 76, '000651.SZ': 83, '000725.SZ': 70,
    '000768.SZ': 79, '000858.SZ': 86, '002001.SZ': 81,
    '002007.SZ': 77, '002024.SZ': 74, '002027.SZ': 84,
    '002142.SZ': 75, '002230.SZ': 89, '002236.SZ': 73,
    '002304.SZ': 87, '002352.SZ': 71
}

# 筛选评分>75的股票
selected_stocks = [k for k, v in dao_scores.items() if v > 75]

# 获取这些股票的历史收益率
returns = pd.read_csv('a_share_returns.csv', index_col=0, parse_dates=True)
returns = returns[selected_stocks]

# 创建投资组合
port = rp.Portfolio(returns=returns)

# 使用Dao Quant评分调整预期收益
# 将评分映射到预期收益调整因子
score_series = pd.Series({k: v for k, v in dao_scores.items() if k in selected_stocks})
adjustment_factor = (score_series - 75) / 100  # 评分越高，预期收益越高

# 计算基础统计量
port.assets_stats(method_mu='hist', method_cov='ledoit')

# 调整预期收益
port.mu = port.mu * (1 + adjustment_factor)

# 风险平价优化
w = port.rp_optimization(
    model='Classic',
    rm='CVaR',  # 使用CVaR作为风险度量，更稳健
    rf=0.03     # 中国无风险利率约3%
)

# 分析结果
print("最优权重:")
print(w.sort_values(ascending=False))

# 风险贡献分析
risk_contrib = port.risk_contribution(w)
print("\n风险贡献:")
print(risk_contrib)
```

### 案例2：Black-Litterman与Dao Quant观点融合

```python
# 使用Dao Quant分析结果作为BL模型的投资者观点

# 假设Dao Quant分析得出以下观点：
# 1. 科技板块未来一年将跑赢大盘3%
# 2. 消费板块估值合理，预期收益8%
# 3. 金融板块受政策影响，预期收益5%

# 定义资产和板块
assets = ['Tech_1', 'Tech_2', 'Tech_3', 
          'Cons_1', 'Cons_2', 'Cons_3',
          'Fin_1', 'Fin_2', 'Fin_3']

# 观点矩阵P: 每个观点对应一行
P = pd.DataFrame([
    # 观点1: 科技板块平均 - 市场平均 = 3%
    [1/3, 1/3, 1/3, -1/9, -1/9, -1/9, -1/9, -1/9, -1/9],
    # 观点2: 消费板块平均 = 8%
    [0, 0, 0, 1/3, 1/3, 1/3, 0, 0, 0],
    # 观点3: 金融板块平均 = 5%
    [0, 0, 0, 0, 0, 0, 1/3, 1/3, 1/3]
], columns=assets)

# 观点收益向量Q
Q = pd.Series([0.03, 0.08, 0.05])

# 观点置信度（可选）
# Omega = np.diag([0.001, 0.002, 0.0015])  # 观点的不确定性

# 市场均衡权重（如市值加权）
market_weights = pd.Series({
    'Tech_1': 0.10, 'Tech_2': 0.08, 'Tech_3': 0.07,
    'Cons_1': 0.12, 'Cons_2': 0.10, 'Cons_3': 0.08,
    'Fin_1': 0.15, 'Fin_2': 0.12, 'Fin_3': 0.08
})

# 创建BL模型
bl = rp.BlackLitterman(
    X=returns,
    P=P,
    Q=Q,
    w=market_weights,
    delta=2.5,      # 风险厌恶系数
    rf=0.03
)

# 计算后验分布
bl_mu, bl_cov = bl.bl_model()

# 使用BL结果进行优化
port.mu = bl_mu
port.cov = bl_cov

w_bl = port.optimization(
    model='BL',
    rm='MV',
    obj='Sharpe',
    rf=0.03
)

print("Black-Litterman优化权重:")
print(w_bl)
```

---

## 七、版本演进

| 版本 | 时间 | 主要更新 |
|-----|------|---------|
| v1.x | 2020 | 基础功能，均值-方差优化 |
| v2.x | 2021 | 风险平价，CVaR优化 |
| v3.x | 2022 | Black-Litterman模型，因子模型 |
| v4.x | 2023 | 多期优化，鲁棒优化 |
| v5.x | 2024 | 性能优化，新风险度量 |
| v6.x | 2025 | 持续维护，文档完善 |

---

## 八、总结

### 8.1 核心价值

Riskfolio-Lib 代表了**Python生态中最专业的投资组合优化解决方案**：

1. **理论严谨**：基于现代投资组合理论和风险管理前沿研究
2. **方法全面**：从经典到前沿，覆盖各类优化需求
3. **实现优雅**：清晰的API设计，易于使用和扩展
4. **生产就绪**：基于成熟的优化求解器，可靠高效

### 8.2 适用人群

| 人群 | 适用场景 |
|-----|---------|
| **量化研究员** | 资产配置研究、风险预算分析 |
| **基金经理** | 组合构建、权重优化 |
| **FOF/MOM投资** | 子基金配置、风险分散 |
| **个人投资者** | 资产配置、投资组合管理 |
| **学术研究者** | 投资组合理论实证研究 |

### 8.3 与其他框架对比

| 框架 | 特点 | 适用场景 |
|-----|------|---------|
| **Riskfolio-Lib** | 方法全面、理论严谨 | 专业组合优化 |
| **PyPortfolioOpt** | 简洁易用 | 快速原型、教学 |
| **cvxportfolio** | 多期优化 | 动态资产配置 |
| **PortfolioLab** | 集成度高 | 端到端分析 |

---

## 参考文献

1. Markowitz H. Portfolio Selection. The Journal of Finance, 1952, 7(1): 77-91.
2. Black F, Litterman R. Global Portfolio Optimization. Financial Analysts Journal, 1992, 48(5): 28-43.
3. Qian E. Risk Parity Fundamentals. CRC Press, 2016.
4. Rockafellar R T, Uryasev S. Optimization of Conditional Value-at-Risk. Journal of Risk, 2000, 2: 21-42.
5. Cajas D. Riskfolio-Lib Documentation. https://riskfolio-lib.readthedocs.io
6. Cajas D. Riskfolio-Lib GitHub Repository. https://github.com/dcajasn/Riskfolio-Lib

---

> ⚠️ **免责声明**：本文仅对开源项目进行技术介绍和分析，不构成任何投资建议。投资组合优化基于历史数据和数学模型，实际投资结果可能因市场变化而有所不同。市场有风险，投资需谨慎。

---

*Powered by 老子道·Dao Quant Research | 道可道，非常道*
