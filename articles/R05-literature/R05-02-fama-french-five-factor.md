---
title: "Fama-French五因子模型详解"
description: "深入解析Fama-French五因子模型的理论基础与实证方法，探讨因子投资的学术基础与实践应用"
author: "DAO量化研究组"
date: "2026-05-21"
category: "R系列-研究方法论"
tags: ["Fama-French", "五因子模型", "因子投资", "资产定价", "学术文献"]
series: "研究方法论"
series_order: 2
series_title: "经典文献系列"
version: "1.0"
math: true
---

# Fama-French五因子模型详解

## 摘要

Fama-French五因子模型是现代资产定价理论的里程碑。本文深入解析该模型的理论基础、因子构建方法、实证结果及其在量化投资中的应用，为因子投资提供坚实的学术基础。

## 一、模型发展脉络

### 1.1 从CAPM到多因子模型

#### CAPM（1964）

$$E[R_i] - R_f = \beta_i (E[R_m] - R_f)$$

**局限**：
- 无法解释规模效应
- 无法解释价值效应
- 无法解释动量效应

#### Fama-French三因子模型（1993）

$$E[R_i] - R_f = \beta_i^m (E[R_m] - R_f) + \beta_i^{SMB} E[SMB] + \beta_i^{HML} E[HML]$$

**创新**：
- 加入规模因子（SMB）
- 加入价值因子（HML）

#### Fama-French五因子模型（2015）

$$E[R_i] - R_f = \beta_i^m RMRF + \beta_i^{SMB} SMB + \beta_i^{HML} HML + \beta_i^{RMW} RMW + \beta_i^{CMA} CMA$$

**新增**：
- 盈利因子（RMW）
- 投资因子（CMA）

### 1.2 模型演进逻辑

| 模型 | 因子 | 解释能力 |
|------|------|---------|
| CAPM | 市场 | 低 |
| FF3 | 市场+规模+价值 | 中 |
| FF5 | 市场+规模+价值+盈利+投资 | 高 |

## 二、五因子详解

### 2.1 市场因子（RMRF）

#### 定义

$$RMRF = R_m - R_f$$

其中：
- $R_m$：市场组合收益率
- $R_f$：无风险利率

#### 经济含义

- 承担系统性风险的补偿
- 市场组合是最优风险组合

### 2.2 规模因子（SMB）

#### 定义

SMB（Small Minus Big）：小市值股票组合减去大市值股票组合的收益

#### 构建方法

```python
def construct_smb(stock_data):
    """
    构建SMB因子
    """
    # 按市值分组
    stock_data['size_group'] = pd.qcut(stock_data['market_cap'], 
                                       q=2, labels=['S', 'B'])
    
    # 计算组合收益
    small_returns = stock_data[stock_data['size_group'] == 'S'].groupby('date')['return'].mean()
    big_returns = stock_data[stock_data['size_group'] == 'B'].groupby('date')['return'].mean()
    
    # SMB = Small - Big
    smb = small_returns - big_returns
    
    return smb
```

#### 经济含义

- 小市值公司风险更高
- 流动性溢价
- 信息不对称

### 2.3 价值因子（HML）

#### 定义

HML（High Minus Low）：高账面市值比股票减去低账面市值比股票的收益

#### 构建方法

```python
def construct_hml(stock_data):
    """
    构建HML因子
    """
    # 按BM分组
    stock_data['bm_group'] = pd.qcut(stock_data['book_to_market'], 
                                     q=3, labels=['L', 'M', 'H'])
    
    # 计算组合收益
    high_returns = stock_data[stock_data['bm_group'] == 'H'].groupby('date')['return'].mean()
    low_returns = stock_data[stock_data['bm_group'] == 'L'].groupby('date')['return'].mean()
    
    # HML = High - Low
    hml = high_returns - low_returns
    
    return hml
```

#### 经济含义

- 价值股被低估
- 财务困境风险
- 行为偏差：过度反应

### 2.4 盈利因子（RMW）

#### 定义

RMW（Robust Minus Weak）：高盈利股票减去低盈利股票的收益

#### 构建方法

```python
def construct_rmw(stock_data):
    """
    构建RMW因子
    """
    # 按盈利能力分组
    stock_data['profit_group'] = pd.qcut(stock_data['operating_profitability'], 
                                         q=3, labels=['W', 'N', 'R'])
    
    # 计算组合收益
    robust_returns = stock_data[stock_data['profit_group'] == 'R'].groupby('date')['return'].mean()
    weak_returns = stock_data[stock_data['profit_group'] == 'W'].groupby('date')['return'].mean()
    
    # RMW = Robust - Weak
    rmw = robust_returns - weak_returns
    
    return rmw
```

#### 经济含义

- 盈利能力是质量的核心
- 盈利持续性
- 成长性与价值性的平衡

### 2.5 投资因子（CMA）

#### 定义

CMA（Conservative Minus Aggressive）：低投资股票减去高投资股票的收益

#### 构建方法

```python
def construct_cma(stock_data):
    """
    构建CMA因子
    """
    # 按投资水平分组
    stock_data['inv_group'] = pd.qcut(stock_data['asset_growth'], 
                                      q=3, labels=['A', 'N', 'C'])
    
    # 计算组合收益
    cons_returns = stock_data[stock_data['inv_group'] == 'C'].groupby('date')['return'].mean()
    agg_returns = stock_data[stock_data['inv_group'] == 'A'].groupby('date')['return'].mean()
    
    # CMA = Conservative - Aggressive
    cma = cons_returns - agg_returns
    
    return cma
```

#### 经济含义

- 过度投资导致价值毁灭
- 管理层代理问题
- 投资效率

## 三、因子构建的2x3方法

### 3.1 分组方法

Fama-French采用2x3分组方法：

```
        低BM   中BM   高BM
小市值   SL     SM     SH
大市值   BL     BM     BH
```

### 3.2 因子计算

```python
def construct_ff5_factors(stock_data):
    """
    构建FF5因子（2x3方法）
    """
    # 独立分组
    stock_data['size_group'] = pd.qcut(stock_data['market_cap'], 2, labels=['S', 'B'])
    stock_data['bm_group'] = pd.qcut(stock_data['book_to_market'], 3, labels=['L', 'M', 'H'])
    stock_data['op_group'] = pd.qcut(stock_data['operating_profitability'], 3, labels=['W', 'N', 'R'])
    stock_data['inv_group'] = pd.qcut(stock_data['asset_growth'], 3, labels=['A', 'N', 'C'])
    
    # 构建6个市值-BM组合
    portfolios = {}
    for size in ['S', 'B']:
        for bm in ['L', 'M', 'H']:
            mask = (stock_data['size_group'] == size) & (stock_data['bm_group'] == bm)
            portfolios[f'{size}{bm}'] = stock_data[mask].groupby('date')['return'].mean()
    
    # 计算SMB和HML
    SMB = (portfolios['SL'] + portfolios['SM'] + portfolios['SH']) / 3 - \
          (portfolios['BL'] + portfolios['BM'] + portfolios['BH']) / 3
    
    HML = (portfolios['SH'] + portfolios['BH']) / 2 - \
          (portfolios['SL'] + portfolios['BL']) / 2
    
    return {'SMB': SMB, 'HML': HML, ...}
```

## 四、实证结果

### 4.1 因子收益特征

| 因子 | 年化收益 | 波动率 | 夏普比率 | t统计量 |
|------|---------|--------|---------|---------|
| RMRF | 6.0% | 18% | 0.33 | 2.5 |
| SMB | 2.4% | 10% | 0.24 | 1.8 |
| HML | 3.6% | 12% | 0.30 | 2.3 |
| RMW | 3.0% | 8% | 0.38 | 2.8 |
| CMA | 2.8% | 8% | 0.35 | 2.6 |

### 4.2 因子相关性

|  | RMRF | SMB | HML | RMW | CMA |
|--|------|-----|-----|-----|-----|
| RMRF | 1.00 | 0.15 | -0.25 | -0.20 | -0.15 |
| SMB | 0.15 | 1.00 | 0.05 | 0.10 | 0.05 |
| HML | -0.25 | 0.05 | 1.00 | 0.35 | 0.60 |
| RMW | -0.20 | 0.10 | 0.35 | 1.00 | 0.45 |
| CMA | -0.15 | 0.05 | 0.60 | 0.45 | 1.00 |

### 4.3 解释能力

| 模型 | R²（平均） |
|------|-----------|
| CAPM | 0.65 |
| FF3 | 0.75 |
| FF5 | 0.80 |

## 五、因子回归分析

### 5.1 时间序列回归

```python
import statsmodels.api as sm

def factor_regression(stock_returns, factors):
    """
    因子回归分析
    """
    X = factors[['RMRF', 'SMB', 'HML', 'RMW', 'CMA']]
    X = sm.add_constant(X)
    y = stock_returns
    
    model = sm.OLS(y, X).fit()
    
    return {
        'alpha': model.params['const'],
        'betas': model.params[1:],
        't_values': model.tvalues,
        'r_squared': model.rsquared,
        'residuals': model.resid
    }
```

### 5.2 Alpha检验

```python
def test_alpha(returns, factors, significance=0.05):
    """
    检验Alpha是否显著
    """
    result = factor_regression(returns, factors)
    
    alpha = result['alpha']
    t_stat = result['t_values']['const']
    
    # 判断显著性
    significant = abs(t_stat) > 1.96  # 5%显著性水平
    
    return {
        'alpha': alpha,
        't_stat': t_stat,
        'significant': significant,
        'interpretation': '存在超额收益' if significant else '无超额收益'
    }
```

## 六、在A股市场的应用

### 6.1 A股因子构建

```python
def construct_china_ff5_factors(stock_data):
    """
    构建A股FF5因子
    """
    # 剔除ST股票
    stock_data = stock_data[~stock_data['is_st']]
    
    # 剔除金融股（计算BM时）
    non_financial = stock_data[stock_data['industry'] != '金融']
    
    # 按市值分组（考虑A股特点）
    # 小市值股票占比高，采用30/70分位
    stock_data['size_group'] = pd.qcut(stock_data['market_cap'], 
                                       q=[0, 0.3, 1.0], labels=['S', 'B'])
    
    # 构建因子...
    
    return factors
```

### 6.2 A股实证结果

| 因子 | 年化收益 | 显著性 |
|------|---------|--------|
| SMB | 8.5% | 显著 |
| HML | 5.2% | 显著 |
| RMW | 4.8% | 显著 |
| CMA | 3.5% | 较弱 |

**特点**：
- 规模效应更明显
- 价值效应存在但波动大
- 质量因子效果良好

## 七、投资策略应用

### 7.1 纯因子组合

```python
def construct_pure_factor_portfolio(stock_data, target_factor):
    """
    构建纯因子组合
    """
    # 回归剔除其他因子影响
    other_factors = ['SMB', 'HML', 'RMW', 'CMA']
    other_factors.remove(target_factor)
    
    # 正交化处理
    X = stock_data[other_factors]
    X = sm.add_constant(X)
    y = stock_data[target_factor]
    
    model = sm.OLS(y, X).fit()
    pure_factor = model.resid
    
    # 构建组合
    stock_data['pure_factor'] = pure_factor
    long_stocks = stock_data.nlargest(int(len(stock_data)*0.1), 'pure_factor')
    short_stocks = stock_data.nsmallest(int(len(stock_data)*0.1), 'pure_factor')
    
    return long_stocks, short_stocks
```

### 7.2 多因子选股

```python
def multi_factor_stock_selection(stock_data, factor_weights):
    """
    多因子选股
    """
    # 标准化因子
    for factor in factor_weights.keys():
        stock_data[f'{factor}_norm'] = (stock_data[factor] - stock_data[factor].mean()) / stock_data[factor].std()
    
    # 计算综合得分
    stock_data['score'] = 0
    for factor, weight in factor_weights.items():
        stock_data['score'] += weight * stock_data[f'{factor}_norm']
    
    # 选择高分股票
    selected = stock_data.nlargest(50, 'score')
    
    return selected
```

## 八、模型局限与批评

### 8.1 主要批评

1. **数据挖掘嫌疑**：因子可能是过度拟合的结果
2. **样本外失效**：部分因子在样本外表现不佳
3. **行为解释不足**：风险解释与行为解释之争
4. **动态变化**：因子收益随时间变化

### 8.2 改进方向

1. **加入动量因子**：Carhart四因子模型
2. **加入质量因子**：Novy-Marx质量因子
3. **加入低波动因子**：Ang et al. (2006)
4. **时变因子**：考虑因子收益的时变性

## 九、总结

### 核心要点

1. **五因子模型是重要框架**：为因子投资提供理论基础
2. **因子有经济逻辑**：不仅是统计现象
3. **实证效果良好**：解释能力强
4. **需要因地制宜**：不同市场需要调整

### 实践建议

1. **理解因子本质**：不只是跑回归
2. **关注因子动态**：因子收益会变化
3. **控制因子暴露**：避免过度集中
4. **结合其他方法**：与技术分析、机器学习结合

---

**参考文献**

1. Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. Journal of Financial Economics, 33(1), 3-56.
2. Fama, E. F., & French, K. R. (2015). A five-factor asset pricing model. Journal of Financial Economics, 116(1), 1-22.
3. Novy-Marx, R. (2013). The other side of value: The gross profitability premium. Journal of Financial Economics, 108(1), 1-28.

---

*本文档为DAO量化研究系列文章，系列编号：R05-02*
