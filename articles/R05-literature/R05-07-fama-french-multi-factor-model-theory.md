---
title: "Fama-French多因子模型详解"
description: "深入解析Fama-French三因子与五因子模型的理论基础、因子构建方法、实证检验及其在量化投资中的应用"
author: "laozdao"
date: "2026-06-01"
category: "R05"
tags:
  - "Fama-French"
  - "多因子模型"
  - "因子投资"
  - "SMB"
  - "HML"
  - "RMW"
  - "CMA"
  - "资产定价"
  - "学术文献"
series: "研究方法论"
series_order: 7
series_title: "经典文献系列"
version: "1.0"
math: true
summary: "Fama-French多因子模型是现代资产定价理论的里程碑。本文深入解析三因子与五因子模型的理论基础、因子构建方法、实证结果及其在A股市场的应用，为因子投资提供坚实的学术基础。"
---

# Fama-French多因子模型详解

## 摘要

Fama-French多因子模型是现代资产定价理论的重要里程碑。1993年，Eugene Fama和Kenneth French提出了三因子模型，在CAPM的基础上加入了规模因子（SMB）和价值因子（HML），显著提升了资产定价的解释能力。2015年，他们又提出了五因子模型，进一步加入盈利因子（RMW）和投资因子（CMA）。本文系统阐述Fama-French多因子模型的理论基础、因子构建方法、实证检验及其在量化投资中的应用，为因子投资实践提供理论支撑。

---

## 一、模型发展脉络

### 1.1 从CAPM到多因子模型

#### CAPM的局限性

资本资产定价模型（CAPM）由Sharpe等人于1964年提出，其核心假设是资产的预期收益率仅与市场风险（Beta）相关：

$$E[R_i] = R_f + \beta_i(E[R_m] - R_f)$$

然而，大量实证研究发现CAPM无法解释的"异象"（Anomalies）：

- **规模效应（Size Effect）**：小市值股票长期跑赢大市值股票
- **价值效应（Value Effect）**：低市净率（高账面市值比）股票长期跑赢成长股
- **动量效应（Momentum Effect）**：过去表现好的股票未来继续表现好

这些异象的存在表明，市场风险并非资产预期收益率的唯一来源。

#### Fama-French三因子模型（1993）

1993年，Fama和French发表了具有里程碑意义的论文《Common Risk Factors in the Returns on Stocks and Bonds》，提出了三因子模型：

$$E[R_i] - R_f = \beta_{i,m}(E[R_m] - R_f) + \beta_{i,SMB}E[SMB] + \beta_{i,HML}E[HML]$$

三因子模型的核心创新在于识别出两个重要的系统性风险因子：

1. **规模因子（SMB, Small Minus Big）**：小市值股票组合减去大市值股票组合的收益
2. **价值因子（HML, High Minus Low）**：高账面市值比股票组合减去低账面市值比股票组合的收益

#### Fama-French五因子模型（2015）

2015年，Fama和French进一步扩展了三因子模型，提出了五因子模型：

$$E[R_i] - R_f = \beta_{i,m}RMRF + \beta_{i,SMB}SMB + \beta_{i,HML}HML + \beta_{i,RMW}RMW + \beta_{i,CMA}CMA$$

新增的两个因子：

3. **盈利因子（RMW, Robust Minus Weak）**：高盈利股票组合减去低盈利股票组合的收益
4. **投资因子（CMA, Conservative Minus Aggressive）**：低投资股票组合减去高投资股票组合的收益

### 1.2 模型演进逻辑

| 模型 | 年份 | 因子 | 解释能力（R²） |
|:-----|:-----|:-----|:--------------|
| CAPM | 1964 | 市场 | ~65% |
| FF三因子 | 1993 | 市场+规模+价值 | ~75% |
| Carhart四因子 | 1997 | 市场+规模+价值+动量 | ~78% |
| FF五因子 | 2015 | 市场+规模+价值+盈利+投资 | ~80% |

模型演进的核心逻辑是：通过识别更多系统性风险因子，更准确地解释和预测资产收益率的横截面差异。

---

## 二、三因子模型详解

### 2.1 市场因子（RMRF）

#### 定义

$$RMRF = R_m - R_f$$

其中：
- $R_m$：市场组合收益率
- $R_f$：无风险利率

#### 经济含义

市场因子代表承担系统性市场风险的补偿。与CAPM一致，所有股票都应该对市场风险有正向暴露，即市场因子的Beta应该为正。

#### 因子收益特征

| 统计量 | 数值（美国市场，1926-2020） |
|:-------|:---------------------------|
| 年化收益 | 6.0% |
| 年化波动率 | 18% |
| 夏普比率 | 0.33 |
| t统计量 | 2.5 |

### 2.2 规模因子（SMB）

#### 定义

SMB（Small Minus Big）是小市值股票组合减去大市值股票组合的收益：

$$SMB = \frac{1}{3}(SL + SM + SH) - \frac{1}{3}(BL + BM + BH)$$

其中：
- S = Small（小市值），B = Big（大市值）
- L = Low BM（低账面市值比），M = Medium BM（中账面市值比），H = High BM（高账面市值比）

#### 构建方法（2×3分组）

1. **按市值分组**：每年6月底，将NYSE、AMEX、NASDAQ股票按市值中位数分为两组（Small/Big）
2. **按BM分组**：将股票按账面市值比分位数分为三组（Low/Medium/High）
3. **构建6个组合**：SL、SM、SH、BL、BM、BH
4. **计算SMB**：小市值组合平均收益减去大市值组合平均收益

#### 经济含义

规模因子反映小市值股票的系统性风险溢价，可能的解释包括：

1. **风险补偿**：小市值公司经营风险更高，流动性更差
2. **信息不对称**：小市值公司信息透明度较低，投资者需要额外补偿
3. **机构限制**：大型机构投资者难以配置小市值股票，导致定价偏差
4. **行为偏差**：投资者对知名大公司的过度偏好

#### 因子收益特征

| 统计量 | 数值（美国市场，1926-2020） |
|:-------|:---------------------------|
| 年化收益 | 2.4% |
| 年化波动率 | 10% |
| 夏普比率 | 0.24 |
| t统计量 | 1.8 |

### 2.3 价值因子（HML）

#### 定义

HML（High Minus Low）是高账面市值比股票组合减去低账面市值比股票组合的收益：

$$HML = \frac{1}{2}(SH + BH) - \frac{1}{2}(SL + BL)$$

#### 构建方法

与SMB相同，使用2×3分组方法，但关注的是账面市值比（Book-to-Market Ratio）维度。

#### 账面市值比（BM）的计算

$$BM = \frac{\text{账面价值}}{\text{市值}} = \frac{\text{股东权益}}{\text{股价} \times \text{流通股数}}$$

注意：
- 使用上一财年末的账面价值
- 市值使用每年6月底的市值（与规模因子保持一致）
- 剔除账面价值为负的公司

#### 经济含义

价值因子反映价值股相对于成长股的系统性风险溢价，可能的解释包括：

1. **财务困境风险**：高BM公司往往处于财务困境，需要风险补偿
2. **盈利下滑风险**：价值股可能面临行业衰退或公司基本面恶化
3. **行为偏差**：投资者对成长股过度乐观，对价值股过度悲观
4. **流动性风险**：价值股可能流动性较差

#### 因子收益特征

| 统计量 | 数值（美国市场，1926-2020） |
|:-------|:---------------------------|
| 年化收益 | 3.6% |
| 年化波动率 | 12% |
| 夏普比率 | 0.30 |
| t统计量 | 2.3 |

---

## 三、五因子模型详解

### 3.1 盈利因子（RMW）

#### 定义

RMW（Robust Minus Weak）是高盈利股票组合减去低盈利股票组合的收益：

$$RMW = \frac{1}{2}(SR + BR) - \frac{1}{2}(SW + BW)$$

其中：
- R = Robust（高盈利），W = Weak（低盈利）
- S = Small（小市值），B = Big（大市值）

#### 盈利能力的度量

Fama-French使用**营业利润率（Operating Profitability）**：

$$Operating\ Profitability = \frac{\text{营业收入} - \text{营业成本} - \text{利息支出}}{\text{账面价值}}$$

注意：
- 使用上一财年的数据
- 分母使用期初账面价值
- 排除金融股（因利息支出计算方式不同）

#### 构建方法（2×3分组）

1. **按市值分组**：每年6月底按市值中位数分为两组
2. **按盈利能力分组**：按营业利润率30%/70%分位数分为三组
3. **构建6个组合**：SR、SN、SW、BR、BN、BW
4. **计算RMW**：高盈利组合平均收益减去低盈利组合平均收益

#### 经济含义

盈利因子反映高盈利公司相对于低盈利公司的系统性风险溢价，可能的解释包括：

1. **盈利持续性**：高盈利公司具有更强的竞争优势和盈利能力持续性
2. **质量溢价**：盈利能力是"质量"的核心维度，高质量资产需要溢价
3. **成长性与价值性的平衡**：盈利能力介于成长和价值之间
4. **行为偏差**：投资者对盈利能力的反应不足

#### 因子收益特征

| 统计量 | 数值（美国市场，1963-2020） |
|:-------|:---------------------------|
| 年化收益 | 3.0% |
| 年化波动率 | 8% |
| 夏普比率 | 0.38 |
| t统计量 | 2.8 |

### 3.2 投资因子（CMA）

#### 定义

CMA（Conservative Minus Aggressive）是低投资股票组合减去高投资股票组合的收益：

$$CMA = \frac{1}{2}(SC + BC) - \frac{1}{2}(SA + BA)$$

其中：
- C = Conservative（低投资），A = Aggressive（高投资）
- S = Small（小市值），B = Big（大市值）

#### 投资水平的度量

Fama-French使用**资产增长率（Asset Growth）**：

$$Asset\ Growth = \frac{\text{总资产}_t - \text{总资产}_{t-1}}{\text{总资产}_{t-1}}$$

注意：
- 使用上一财年的总资产数据
- 排除总资产变化异常的公司

#### 构建方法（2×3分组）

1. **按市值分组**：每年6月底按市值中位数分为两组
2. **按投资水平分组**：按资产增长率30%/70%分位数分为三组
3. **构建6个组合**：SC、SN、SA、BC、BN、BA
4. **计算CMA**：低投资组合平均收益减去高投资组合平均收益

#### 经济含义

投资因子反映低投资公司相对于高投资公司的系统性风险溢价，可能的解释包括：

1. **过度投资**：高投资公司可能存在过度投资问题，导致价值毁灭
2. **管理层代理问题**：管理层可能为扩大规模而进行低效投资
3. **投资效率**：低投资公司投资效率更高，资本配置更优
4. **q理论**：根据Tobin's q理论，低投资对应高q值，反映更好的投资机会

#### 因子收益特征

| 统计量 | 数值（美国市场，1963-2020） |
|:-------|:---------------------------|
| 年化收益 | 2.8% |
| 年化波动率 | 8% |
| 夏普比率 | 0.35 |
| t统计量 | 2.6 |

---

## 四、因子构建的2×3方法

### 4.1 分组方法详解

Fama-French采用**独立双重排序（Independent Double Sort）**方法构建因子组合：

```
              低BM    中BM    高BM
小市值（S）    SL      SM      SH
大市值（B）    BL      BM      BH
```

#### 独立排序 vs 条件排序

- **独立排序（Independent Sort）**：先在每个维度上独立分组，再交叉组合
- **条件排序（Conditional Sort）**：先在一个维度上分组，再在每组内进行第二个维度的分组

Fama-French使用独立排序，因为：
1. 保持各维度组合的样本量均衡
2. 因子收益更纯粹，减少维度间的相关性
3. 便于计算纯因子收益

### 4.2 五因子的完整构建流程

```python
# 伪代码：五因子构建流程
def construct_ff5_factors(stock_data, date):
    """
    构建Fama-French五因子
    """
    # 1. 筛选股票（排除金融股、缺失数据）
    stocks = filter_stocks(stock_data, date)
    
    # 2. 按市值分组（NYSE中位数）
    size_median = stocks[stocks.exchange == 'NYSE'].market_cap.median()
    stocks['size_group'] = np.where(stocks.market_cap <= size_median, 'S', 'B')
    
    # 3. 按BM分组（30%/70%分位数）
    bm_30 = stocks.book_to_market.quantile(0.3)
    bm_70 = stocks.book_to_market.quantile(0.7)
    stocks['bm_group'] = pd.cut(stocks.book_to_market, 
                                bins=[-np.inf, bm_30, bm_70, np.inf],
                                labels=['L', 'M', 'H'])
    
    # 4. 按盈利能力分组（30%/70%分位数）
    op_30 = stocks.operating_profitability.quantile(0.3)
    op_70 = stocks.operating_profitability.quantile(0.7)
    stocks['op_group'] = pd.cut(stocks.operating_profitability,
                                bins=[-np.inf, op_30, op_70, np.inf],
                                labels=['W', 'N', 'R'])
    
    # 5. 按投资水平分组（30%/70%分位数）
    inv_30 = stocks.asset_growth.quantile(0.3)
    inv_70 = stocks.asset_growth.quantile(0.7)
    stocks['inv_group'] = pd.cut(stocks.asset_growth,
                                 bins=[-np.inf, inv_30, inv_70, np.inf],
                                 labels=['A', 'N', 'C'])
    
    # 6. 构建组合并计算因子
    # ...（详见实践篇）
    
    return {'RMRF': rmrf, 'SMB': smb, 'HML': hml, 'RMW': rmw, 'CMA': cma}
```

---

## 五、因子回归分析

### 5.1 时间序列回归

对于单个资产或投资组合，使用Fama-French模型进行时间序列回归：

$$R_{i,t} - R_{f,t} = \alpha_i + \beta_{i,m}RMRF_t + \beta_{i,SMB}SMB_t + \beta_{i,HML}HML_t + \beta_{i,RMW}RMW_t + \beta_{i,CMA}CMA_t + \varepsilon_{i,t}$$

#### 回归结果解读

| 系数 | 含义 | 解释 |
|:-----|:-----|:-----|
| $\alpha_i$ | Jensen's Alpha | 经五因子调整后的超额收益 |
| $\beta_{i,m}$ | 市场Beta | 对市场风险的敏感度 |
| $\beta_{i,SMB}$ | 规模Beta | 对小市值因子的暴露 |
| $\beta_{i,HML}$ | 价值Beta | 对价值因子的暴露 |
| $\beta_{i,RMW}$ | 盈利Beta | 对盈利因子的暴露 |
| $\beta_{i,CMA}$ | 投资Beta | 对投资因子的暴露 |
| $R^2$ | 解释力 | 五因子解释收益率变动的比例 |

### 5.2 Alpha检验

Alpha是检验资产定价模型有效性的关键指标：

$$H_0: \alpha_i = 0$$

如果Alpha显著不为零，说明：
- 模型存在遗漏的系统性风险因子
- 资产被错误定价（高估或低估）
- 存在套利机会

### 5.3 横截面回归（Fama-MacBeth方法）

Fama-MacBeth（1973）两步回归法用于检验因子风险溢价：

**第一步：时间序列回归**
对每个资产进行时间序列回归，估计Beta系数

**第二步：横截面回归**
每期进行横截面回归：

$$E[R_i] = \gamma_0 + \gamma_m\beta_{i,m} + \gamma_{SMB}\beta_{i,SMB} + \gamma_{HML}\beta_{i,HML} + \gamma_{RMW}\beta_{i,RMW} + \gamma_{CMA}\beta_{i,CMA} + \varepsilon_i$$

**第三步：计算均值和t统计量**
对第二步的系数取时间平均，计算t统计量

---

## 六、因子相关性分析

### 6.1 因子收益相关性

|  | RMRF | SMB | HML | RMW | CMA |
|:--|:----:|:---:|:---:|:---:|:---:|
| RMRF | 1.00 | 0.15 | -0.25 | -0.20 | -0.15 |
| SMB | 0.15 | 1.00 | 0.05 | 0.10 | 0.05 |
| HML | -0.25 | 0.05 | 1.00 | 0.35 | 0.60 |
| RMW | -0.20 | 0.10 | 0.35 | 1.00 | 0.45 |
| CMA | -0.15 | 0.05 | 0.60 | 0.45 | 1.00 |

**关键发现**：
- HML与CMA高度相关（0.60）：低投资公司往往是价值股
- RMW与HML、CMA中度相关：盈利能力与价值、投资相关
- SMB与其他因子相关性较低：规模因子相对独立

### 6.2 因子正交化

由于因子间存在相关性，实践中常进行正交化处理以获得"纯"因子收益：

```python
def orthogonalize_factors(factors):
    """
    对因子进行正交化处理
    """
    # 以RMRF为基准
    pure_smb = regress_residuals(factors['SMB'], factors[['RMRF']])
    pure_hml = regress_residuals(factors['HML'], factors[['RMRF', 'SMB']])
    pure_rmw = regress_residuals(factors['RMW'], factors[['RMRF', 'SMB', 'HML']])
    pure_cma = regress_residuals(factors['CMA'], factors[['RMRF', 'SMB', 'HML', 'RMW']])
    
    return {
        'RMRF': factors['RMRF'],
        'SMB': pure_smb,
        'HML': pure_hml,
        'RMW': pure_rmw,
        'CMA': pure_cma
    }
```

---

## 七、Fama-French模型在A股市场的应用

### 7.1 A股市场特征

A股市场与美股市场存在显著差异，影响Fama-French因子的表现：

| 特征 | A股市场 | 美股市场 |
|:-----|:--------|:---------|
| 规模效应 | 更显著 | 较弱 |
| 价值效应 | 存在但波动大 | 稳定 |
| 盈利效应 | 较明显 | 明显 |
| 投资效应 | 较弱 | 明显 |
| 市场有效性 | 较低 | 较高 |

### 7.2 A股因子构建调整

在A股市场应用Fama-French模型时，需要进行以下调整：

1. **样本筛选**：剔除ST股票、金融股、上市不满一年的新股
2. **市值分组**：考虑A股小市值股票占比高，采用30%/70%分位数而非中位数
3. **BM计算**：使用最新财报数据，注意季度报告的频率
4. **交易日历**：考虑A股节假日安排

### 7.3 A股实证结果

基于国内学者的研究，A股市场Fama-French因子表现：

| 因子 | 年化收益 | t统计量 | 显著性 |
|:-----|:---------|:--------|:-------|
| SMB | 8.5% | 2.8 | 显著 |
| HML | 5.2% | 2.1 | 显著 |
| RMW | 4.8% | 2.5 | 显著 |
| CMA | 2.5% | 1.2 | 较弱 |

**特点**：
- 规模效应（SMB）比美股更显著
- 价值效应（HML）存在但波动较大
- 盈利因子（RMW）效果良好
- 投资因子（CMA）效果较弱

---

## 八、投资策略应用

### 8.1 纯因子组合构建

构建对单一因子有纯暴露的组合：

```python
def construct_pure_factor_portfolio(stocks, target_factor, other_factors):
    """
    构建纯因子组合
    """
    # 回归剔除其他因子影响
    X = stocks[other_factors]
    X = sm.add_constant(X)
    y = stocks[target_factor]
    
    model = sm.OLS(y, X).fit()
    pure_factor = model.resid
    
    # 构建多空组合
    stocks['pure_factor'] = pure_factor
    long_stocks = stocks.nlargest(int(len(stocks)*0.1), 'pure_factor')
    short_stocks = stocks.nsmallest(int(len(stocks)*0.1), 'pure_factor')
    
    return long_stocks, short_stocks
```

### 8.2 多因子选股策略

基于Fama-French因子的综合选股策略：

```python
def ff_factor_stock_selection(stocks, factor_weights):
    """
    Fama-French多因子选股
    """
    # 标准化因子暴露
    for factor in ['SMB', 'HML', 'RMW', 'CMA']:
        stocks[f'{factor}_zscore'] = (stocks[factor] - stocks[factor].mean()) / stocks[factor].std()
    
    # 计算综合得分
    stocks['score'] = 0
    for factor, weight in factor_weights.items():
        stocks['score'] += weight * stocks[f'{factor}_zscore']
    
    # 选择高分股票
    selected = stocks.nlargest(50, 'score')
    
    return selected
```

### 8.3 风险预算配置

基于因子暴露的风险预算分配：

```python
def factor_based_risk_budget(portfolio, factor_cov_matrix):
    """
    基于因子的风险预算配置
    """
    # 计算组合因子暴露
    portfolio_factor_exposure = portfolio['factor_betas'].mean()
    
    # 计算因子贡献的风险
    factor_risk_contrib = portfolio_factor_exposure * (factor_cov_matrix @ portfolio_factor_exposure)
    
    # 调整权重以匹配目标风险预算
    # ...
    
    return adjusted_weights
```

---

## 九、模型局限与批评

### 9.1 理论层面的批评

**1. 因子来源缺乏理论基础**

Fama-French因子主要是基于历史数据挖掘的结果，缺乏坚实的经济理论支撑。虽然有一些行为金融学解释，但尚未形成统一的理论框架。

**2. 因子可能随时间衰减**

随着因子投资的普及，套利行为可能导致因子收益衰减。实证研究表明，部分因子的收益在近年来有所下降。

**3. 数据挖掘风险**

多因子模型可能存在过度拟合问题，特别是在因子选择和组合优化过程中。

### 9.2 实证层面的批评

**1. HML因子在五因子模型中变得不显著**

Fama-French（2015）发现，加入盈利和投资因子后，HML因子的解释力大幅下降，甚至变得不显著。

**2. 动量因子的缺失**

五因子模型仍然无法解释动量效应，需要加入Carhart的动量因子才能完整解释。

**3. 国际市场的适用性**

Fama-French因子在不同国家的表现差异较大，在新兴市场（包括A股）的效果不如发达市场。

### 9.3 改进方向

| 改进方向 | 代表模型/方法 |
|:---------|:-------------|
| 加入动量因子 | Carhart四因子模型 |
| 加入质量因子 | Novy-Marx质量因子 |
| 加入低波动因子 | Ang et al. (2006) |
| 时变因子模型 | 条件Fama-French模型 |
| 机器学习方法 | 因子选择与组合优化 |

---

## 十、总结与实践建议

### 10.1 核心要点回顾

1. **Fama-French模型是多因子定价的基石**：从CAPM到三因子再到五因子，逐步完善资产定价框架

2. **五个因子的经济含义**：
   - RMRF：市场风险
   - SMB：规模风险
   - HML：价值风险
   - RMW：盈利风险
   - CMA：投资风险

3. **因子构建方法**：2×3独立双重排序是标准方法

4. **A股市场适用性**：规模、盈利因子效果显著，价值、投资因子需谨慎使用

### 10.2 实践应用建议

**对于研究分析**：

- 使用Fama-French模型作为基准，比较其他模型的解释能力提升
- 关注Alpha作为定价偏差的信号
- 结合行为金融学理解因子来源

**对于投资决策**：

- 基于因子暴露构建投资组合
- 监控组合因子暴露与目标水平的偏离
- 考虑因子轮动策略

**对于风险管理**：

- 使用因子模型进行风险分解
- 识别组合的主要风险来源
- 进行压力测试和情景分析

### 10.3 进一步学习方向

- 条件Fama-French模型（时变Beta）
- 因子择时与轮动策略
- 机器学习在因子投资中的应用
- 国际市场的Fama-French因子研究

---

## 参考文献

1. Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. *Journal of Financial Economics*, 33(1), 3-56.

2. Fama, E. F., & French, K. R. (2015). A five-factor asset pricing model. *Journal of Financial Economics*, 116(1), 1-22.

3. Carhart, M. M. (1997). On persistence in mutual fund performance. *Journal of Finance*, 52(1), 57-82.

4. Novy-Marx, R. (2013). The other side of value: The gross profitability premium. *Journal of Financial Economics*, 108(1), 1-28.

5. Fama, E. F., & MacBeth, J. D. (1973). Risk, return, and equilibrium: Empirical tests. *Journal of Political Economy*, 81(3), 607-636.

6. Asness, C. S. (1997). The interaction of value and momentum strategies. *Financial Analysts Journal*, 53(2), 29-36.

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao（老子道）· Dao Quant Research**
