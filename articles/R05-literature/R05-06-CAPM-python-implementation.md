---
title: "CAPM模型Python实战指南"
description: "基于Python实现CAPM模型的完整教程，涵盖数据获取、Beta计算、Alpha分析、实证检验与可视化，帮助量化投资者将CAPM理论应用于实践"
author: "laozdao"
date: "2026-06-01"
category: "R05"
tags:
  - "CAPM"
  - "Python"
  - "量化投资"
  - "Beta系数"
  - "Jensen Alpha"
  - "实证分析"
  - "实践教程"
series: "研究方法论"
series_order: 6
series_title: "经典文献系列"
version: "1.0"
math: true
summary: "本文是CAPM模型的Python实战指南，详细介绍如何使用Python实现CAPM模型的完整分析流程，包括历史数据获取、Beta系数计算与调整、Alpha分析、实证检验与可视化。文章提供可运行的完整代码示例，帮助量化投资者将CAPM理论应用于实际投资决策。"
---

# CAPM模型Python实战指南

## 摘要

本文是CAPM（资本资产定价模型）的Python实战教程，系统介绍如何利用Python实现CAPM模型的完整分析流程。文章涵盖数据获取与预处理、Beta系数计算与调整方法、Jensen's Alpha分析、模型实证检验以及结果可视化等核心内容。通过详细的代码示例和实际数据分析，帮助读者将CAPM理论快速应用于量化投资实践。

---

## 一、环境准备与数据获取

### 1.1 必需的Python库

实现CAPM模型分析需要以下Python库：

```python
# 数据处理
import pandas as pd
import numpy as np

# 可视化
import matplotlib.pyplot as plt
import seaborn as sns

# 统计分析
import scipy.stats as stats
import statsmodels.api as sm

# 金融数据获取（选择其中一种或多种）
import akshare as ak      # 免费数据源，支持A股
# import tushare as ts    # 需要注册
# import baostock as bs   # 免费数据源

# 设置中文显示
plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS']
plt.rcParams['axes.unicode_minus'] = False
```

### 1.2 使用AkShare获取A股数据

AkShare是国内优秀的免费金融数据接口，适合获取A股历史数据：

```python
import akshare as ak
import pandas as pd
from datetime import datetime, timedelta

def get_stock_data(stock_code, start_date, end_date):
    """
    获取股票历史行情数据
    
    Parameters:
    -----------
    stock_code : str
        股票代码，如"000001"（平安银行）
    start_date : str
        开始日期，格式"YYYYMMDD"
    end_date : str
        结束日期，格式"YYYYMMDD"
    
    Returns:
    --------
    pd.DataFrame
        包含日期、开盘、收盘、最高、最低、成交量等字段
    """
    try:
        # 获取日线数据
        df = ak.stock_zh_a_hist(
            symbol=stock_code,
            period="daily",
            start_date=start_date,
            end_date=end_date,
            adjust="qfq"  # 前复权
        )
        
        # 数据清洗
        df['日期'] = pd.to_datetime(df['日期'])
        df = df.rename(columns={
            '日期': 'date',
            '开盘': 'open',
            '收盘': 'close',
            '最高': 'high',
            '最低': 'low',
            '成交量': 'volume',
            '成交额': 'amount'
        })
        
        # 计算日收益率
        df['return'] = df['close'].pct_change()
        
        return df[['date', 'open', 'close', 'high', 'low', 'volume', 'amount', 'return']]
    
    except Exception as e:
        print(f"数据获取失败: {e}")
        return None

def get_market_data(start_date, end_date):
    """
    获取沪深300指数数据作为市场组合代理
    """
    try:
        # 沪深300指数代码为"000300"
        df = ak.stock_zh_index_daily(symbol="sh000300")
        
        # 筛选日期范围
        df['日期'] = pd.to_datetime(df['日期'])
        df = df[(df['日期'] >= start_date) & (df['日期'] <= end_date)]
        
        # 计算日收益率
        df = df.rename(columns={'日期': 'date', '收盘': 'close'})
        df['market_return'] = df['close'].pct_change()
        
        return df[['date', 'close', 'market_return']]
    
    except Exception as e:
        print(f"市场数据获取失败: {e}")
        return None
```

### 1.3 数据合并与预处理

将个股数据与市场数据合并：

```python
def merge_stock_market_data(stock_code, start_date, end_date):
    """
    合并个股数据与市场数据
    
    Parameters:
    -----------
    stock_code : str
        股票代码
    start_date : str
        开始日期，格式"YYYYMMDD"
    end_date : str
        结束日期，格式"YYYYMMDD"
    
    Returns:
    --------
    pd.DataFrame
        合并后的数据框
    """
    # 获取个股数据
    stock_df = get_stock_data(stock_code, start_date, end_date)
    if stock_df is None:
        return None
    
    # 获取市场数据
    market_df = get_market_data(start_date, end_date)
    if market_df is None:
        return None
    
    # 合并数据
    merged_df = pd.merge(stock_df, market_df, on='date', how='inner')
    
    # 获取无风险利率（使用一年期国债收益率作为代理）
    # 简化处理：使用常量年化无风险利率 3%
    rf_annual = 0.03
    rf_daily = (1 + rf_annual) ** (1/252) - 1
    merged_df['rf'] = rf_daily
    
    # 计算超额收益
    merged_df['excess_return'] = merged_df['return'] - merged_df['rf']
    merged_df['excess_market_return'] = merged_df['market_return'] - merged_df['rf']
    
    # 剔除缺失值
    merged_df = merged_df.dropna()
    
    return merged_df
```

---

## 二、Beta系数计算与可视化

### 2.1 基础Beta计算

使用线性回归计算Beta系数：

```python
def calculate_beta(regression_df):
    """
    使用OLS回归计算Beta系数
    
    Parameters:
    -----------
    regression_df : pd.DataFrame
        包含 'excess_return' 和 'excess_market_return' 列的数据框
    
    Returns:
    --------
    dict
        包含alpha、Beta、R方等统计量
    """
    # 准备回归数据
    X = sm.add_constant(regression_df['excess_market_return'])
    y = regression_df['excess_return']
    
    # OLS回归
    model = sm.OLS(y, X).fit()
    
    # 提取结果
    result = {
        'alpha': model.params['const'],
        'alpha_t': model.tvalues['const'],
        'alpha_p': model.pvalues['const'],
        'beta': model.params['excess_market_return'],
        'beta_t': model.tvalues['excess_market_return'],
        'beta_p': model.pvalues['excess_market_return'],
        'r_squared': model.rsquared,
        'adj_r_squared': model.rsquared_adj,
        'residuals': model.resid,
        'model': model
    }
    
    return result


def calculate_beta_simple(returns, market_returns, rf=0.03/252):
    """
    使用协方差/方差法计算Beta（简化版）
    
    Formula: Beta = Cov(Ri, Rm) / Var(Rm)
    
    Parameters:
    -----------
    returns : array-like
        资产收益率序列
    market_returns : array-like
        市场收益率序列
    rf : float
        日无风险利率
    
    Returns:
    --------
    float
        Beta系数
    """
    excess_returns = returns - rf
    excess_market = market_returns - rf
    
    covariance = np.cov(excess_returns, excess_market)[0, 1]
    market_variance = np.var(excess_market, ddof=1)
    
    beta = covariance / market_variance
    
    return beta
```

### 2.2 Beta可视化分析

绘制CAPM特征线与散点图：

```python
def plot_capm_characteristic_line(df, result, stock_name="股票"):
    """
    绘制CAPM特征线（Characteristic Line）
    
    散点图：X轴为市场超额收益，Y轴为资产超额收益
    回归线：CAPM特征线
    """
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    
    # 左图：CAPM特征线
    ax1 = axes[0]
    
    # 散点图
    ax1.scatter(
        df['excess_market_return'] * 100,  # 转换为百分比
        df['excess_return'] * 100,
        alpha=0.5,
        s=20,
        label='日收益率'
    )
    
    # 回归线
    x_line = np.linspace(
        df['excess_market_return'].min(),
        df['excess_market_return'].max(),
        100
    )
    y_line = result['alpha'] + result['beta'] * x_line
    ax1.plot(
        x_line * 100,
        y_line * 100,
        'r-',
        linewidth=2,
        label=f'特征线: y = {result["alpha"]:.4f} + {result["beta"]:.2f}x'
    )
    
    # 标注重要统计量
    stats_text = f"""
    α (日) = {result['alpha']*100:.4f}%
    α (年化) = {result['alpha']*252*100:.2f}%
    α t统计量 = {result['alpha_t']:.2f}
    α p值 = {result['alpha_p']:.4f}
    
    β = {result['beta']:.3f}
    β t统计量 = {result['beta_t']:.2f}
    R² = {result['r_squared']:.4f}
    """
    
    ax1.text(
        0.05, 0.95, stats_text,
        transform=ax1.transAxes,
        fontsize=10,
        verticalalignment='top',
        bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.8)
    )
    
    ax1.set_xlabel('市场超额收益率 (%)', fontsize=11)
    ax1.set_ylabel('资产超额收益率 (%)', fontsize=11)
    ax1.set_title(f'{stock_name} - CAPM特征线', fontsize=12)
    ax1.legend(loc='lower right')
    ax1.grid(True, alpha=0.3)
    ax1.axhline(y=0, color='gray', linestyle='--', alpha=0.5)
    ax1.axvline(x=0, color='gray', linestyle='--', alpha=0.5)
    
    # 右图：收益率分布
    ax2 = axes[1]
    
    # 残差分布
    residuals = result['residuals'] * 100
    ax2.hist(residuals, bins=50, alpha=0.7, edgecolor='black')
    ax2.axvline(x=0, color='red', linestyle='--', linewidth=2)
    ax2.set_xlabel('残差 (%)', fontsize=11)
    ax2.set_ylabel('频数', fontsize=11)
    ax2.set_title('CAPM残差分布', fontsize=12)
    ax2.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/capm_characteristic_line.png', dpi=150, bbox_inches='tight')
    plt.show()
    
    return fig
```

### 2.3 滚动Beta计算

分析Beta的时变特性：

```python
def calculate_rolling_beta(df, window=60):
    """
    计算滚动Beta系数
    
    Parameters:
    -----------
    df : pd.DataFrame
        包含超额收益率数据的数据框
    window : int
        滚动窗口大小（交易日数）
    
    Returns:
    --------
    pd.Series
        滚动Beta序列
    """
    # 计算滚动协方差和方差
    cov = df['excess_return'].rolling(window=window).cov(df['excess_market_return'])
    market_var = df['excess_market_return'].rolling(window=window).var()
    
    # 计算滚动Beta
    rolling_beta = cov / market_var
    
    return rolling_beta


def plot_rolling_beta(df, window=60, stock_name="股票"):
    """
    绘制滚动Beta时间序列图
    """
    rolling_beta = calculate_rolling_beta(df, window)
    
    fig, ax = plt.subplots(figsize=(12, 5))
    
    # 滚动Beta曲线
    ax.plot(
        df['date'],
        rolling_beta,
        'b-',
        linewidth=1.5,
        label=f'{window}日滚动Beta'
    )
    
    # 均值线
    mean_beta = rolling_beta.mean()
    ax.axhline(
        y=mean_beta,
        color='red',
        linestyle='--',
        linewidth=2,
        label=f'均值Beta: {mean_beta:.3f}'
    )
    
    # Beta=1参考线
    ax.axhline(y=1, color='gray', linestyle=':', alpha=0.7, label='Beta=1')
    
    # 标注统计量
    stats_text = f"""
    均值Beta: {rolling_beta.mean():.3f}
    标准差: {rolling_beta.std():.3f}
    最大值: {rolling_beta.max():.3f}
    最小值: {rolling_beta.min():.3f}
    """
    
    ax.text(
        0.02, 0.98, stats_text,
        transform=ax.transAxes,
        fontsize=10,
        verticalalignment='top',
        bbox=dict(boxstyle='round', facecolor='lightblue', alpha=0.8)
    )
    
    ax.set_xlabel('日期', fontsize=11)
    ax.set_ylabel('Beta系数', fontsize=11)
    ax.set_title(f'{stock_name} - 滚动Beta分析 ({window}日窗口)', fontsize=12)
    ax.legend(loc='upper right')
    ax.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/rolling_beta.png', dpi=150, bbox_inches='tight')
    plt.show()
    
    return fig
```

---

## 三、Beta调整与优化

### 3.1 Blume调整方法

Blume（1971）提出的Beta调整公式：

```python
def blume_adjustment(sample_betas, method='simple'):
    """
    使用Blume方法调整Beta系数
    
    Blume发现：β_{t+1} ≈ 0.576 + 0.636 * β_t
    
    Parameters:
    -----------
    sample_betas : list or array
        历史Beta估计值
    method : str
        调整方法，'simple'或'regression'
    
    Returns:
    --------
    np.array
        调整后的Beta数组
    """
    if method == 'simple':
        # 简单Blume调整
        # 历史上发现的规律：β_{t+1} = 0.333 + 0.666 * β_t
        adjusted_betas = 1/3 + 2/3 * np.array(sample_betas)
    
    elif method == 'regression':
        # 基于历史数据回归调整
        # 需要足够长的Beta时间序列
        # β_{t+1} = a + b * β_t
        betas = np.array(sample_betas[:-1])
        next_betas = np.array(sample_betas[1:])
        
        # 简单线性回归
        b = np.corrcoef(betas, next_betas)[0, 1] * (next_betas.std() / betas.std())
        a = next_betas.mean() - b * betas.mean()
        
        # 调整最新Beta
        adjusted_betas = a + b * np.array(sample_betas)
    
    else:
        raise ValueError(f"未知的调整方法: {method}")
    
    return adjusted_betas


def vasicek_adjustment(sample_beta, market_beta=1.0, sample_variance=None, n_periods=60):
    """
    使用Vasicek方法调整Beta系数
    
    基于贝叶斯收缩估计，将样本Beta向市场Beta收缩
    
    Formula: β_adjusted = (n * β_sample + k * β_market) / (n + k)
    
    其中 k = σ²_market / σ²_sample
    
    Parameters:
    -----------
    sample_beta : float
        样本Beta估计
    market_beta : float
        市场Beta（通常为1）
    sample_variance : float
        Beta估计的方差（可选）
    n_periods : int
        估计Beta使用的观测数
    
    Returns:
    --------
    float
        调整后的Beta
    """
    # 如果未提供方差，使用默认值
    if sample_variance is None:
        # 根据经验，Beta估计的方差约为 0.0333
        sample_variance = 0.0333
    
    # 计算收缩因子
    k = 0.0333 / sample_variance  # 经验值
    
    # Vasicek调整
    adjusted_beta = (n_periods * sample_beta + k * market_beta) / (n_periods + k)
    
    return adjusted_beta
```

### 3.2 Bloomberg Beta调整方法

机构投资者常用的调整方法：

```python
def bloomberg_beta_adjustment(raw_beta, market_beta=1.0):
    """
    Bloomberg风格的Beta调整
    
    Bloomberg使用的调整公式：
    Adjusted Beta = (2/3) * Raw Beta + (1/3) * 1.0
    
    这是对Blume调整的简化版本
    
    Parameters:
    -----------
    raw_beta : float
        原始Beta估计
    market_beta : float
        市场Beta（通常为1）
    
    Returns:
    --------
    float
        调整后的Beta
    """
    adjusted_beta = 2/3 * raw_beta + 1/3 * market_beta
    
    return adjusted_beta


def morgan_stanley_beta_adjustment(raw_beta, sample_size=60):
    """
    Morgan Stanley风格的Beta调整
    
    调整公式考虑样本量的影响：
    Adjusted Beta = Raw Beta * (T / (T + 2))
    
    Parameters:
    -----------
    raw_beta : float
        原始Beta估计
    sample_size : int
        用于估计Beta的观测数量
    
    Returns:
    --------
    float
        调整后的Beta
    """
    # 样本量调整因子
    adjustment_factor = sample_size / (sample_size + 2)
    
    adjusted_beta = raw_beta * adjustment_factor + (1 - adjustment_factor) * 1.0
    
    return adjusted_beta
```

---

## 四、Jensen's Alpha分析

### 4.1 Alpha计算与检验

```python
def calculate_jensen_alpha(df, risk_free_rate=0.03):
    """
    计算Jensen's Alpha及其统计显著性
    
    Jensen's Alpha = E[Ri] - (Rf + β * (E[Rm] - Rf))
    
    Parameters:
    -----------
    df : pd.DataFrame
        包含收益率数据的数据框
    risk_free_rate : float
        年化无风险利率
    
    Returns:
    --------
    dict
        包含Alpha及各项统计量
    """
    # 日无风险利率
    rf_daily = (1 + risk_free_rate) ** (1/252) - 1
    
    # 超额收益
    excess_return = df['return'] - rf_daily
    excess_market = df['market_return'] - rf_daily
    
    # OLS回归
    X = sm.add_constant(excess_market)
    model = sm.OLS(excess_return, X).fit()
    
    # Alpha（日度）
    alpha_daily = model.params['const']
    
    # Alpha（年化）
    alpha_annual = alpha_daily * 252
    
    # 统计量
    alpha_t = model.tvalues['const']
    alpha_p = model.pvalues['const']
    
    # 信息比率（年化）
    information_ratio = alpha_annual / (model.resid.std() * np.sqrt(252))
    
    return {
        'alpha_daily': alpha_daily,
        'alpha_annual': alpha_annual,
        'alpha_t': alpha_t,
        'alpha_p': alpha_p,
        'beta': model.params['excess_market_return'],
        'r_squared': model.rsquared,
        'information_ratio': information_ratio,
        'model': model
    }
```

### 4.2 Alpha的分解与分析

```python
def decompose_jensen_alpha(df, risk_free_rate=0.03):
    """
    将Alpha分解为不同来源
    
    Total Return = Rf + β * (Rm - Rf) + α + ε
    
    Parameters:
    -----------
    df : pd.DataFrame
        收益率数据
    risk_free_rate : float
        年化无风险利率
    
    Returns:
    --------
    pd.DataFrame
        分解后的收益组成
    """
    rf_daily = (1 + risk_free_rate) ** (1/252) - 1
    
    # 各收益组成部分
    decomposition = pd.DataFrame(index=df.index)
    
    decomposition['rf'] = rf_daily
    decomposition['market_premium'] = df['market_return'] - rf_daily
    decomposition['beta_effect'] = df['beta'] * decomposition['market_premium']
    decomposition['actual_return'] = df['return']
    
    # 分解Alpha（使用滚动Beta）
    rolling_beta = calculate_rolling_beta(df, window=60)
    decomposition['beta'] = rolling_beta
    decomposition['expected_return'] = rf_daily + rolling_beta * decomposition['market_premium']
    decomposition['alpha'] = decomposition['actual_return'] - decomposition['expected_return']
    
    return decomposition


def plot_alpha_decomposition(decomposition, stock_name="股票"):
    """
    可视化Alpha分解结果
    """
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    
    # 1. 累积收益对比
    ax1 = axes[0, 0]
    cumulative_return = (1 + decomposition['actual_return']).cumprod() - 1
    cumulative_expected = (1 + decomposition['expected_return']).cumprod() - 1
    cumulative_rf = (1 + decomposition['rf']).cumprod() - 1
    
    ax1.plot(cumulative_return * 100, label='实际累积收益', linewidth=1.5)
    ax1.plot(cumulative_expected * 100, label='CAPM预期收益', linewidth=1.5, linestyle='--')
    ax1.plot(cumulative_rf * 100, label='无风险收益', linewidth=1.5, linestyle=':')
    ax1.fill_between(
        cumulative_return.index,
        (cumulative_expected * 100).values,
        (cumulative_return * 100).values,
        alpha=0.3,
        color='green' if cumulative_return.iloc[-1] > cumulative_expected.iloc[-1] else 'red',
        label='Alpha累计'
    )
    ax1.set_xlabel('日期')
    ax1.set_ylabel('累积收益率 (%)')
    ax1.set_title('累积收益对比')
    ax1.legend()
    ax1.grid(True, alpha=0.3)
    
    # 2. Alpha时间序列
    ax2 = axes[0, 1]
    ax2.plot(decomposition['alpha'] * 100, 'b-', alpha=0.7, linewidth=0.8)
    ax2.axhline(y=0, color='red', linestyle='--')
    ax2.fill_between(
        decomposition.index,
        0,
        decomposition['alpha'] * 100,
        where=decomposition['alpha'] > 0,
        alpha=0.3,
        color='green',
        label='正Alpha'
    )
    ax2.fill_between(
        decomposition.index,
        0,
        decomposition['alpha'] * 100,
        where=decomposition['alpha'] < 0,
        alpha=0.3,
        color='red',
        label='负Alpha'
    )
    ax2.set_xlabel('日期')
    ax2.set_ylabel('日Alpha (%)')
    ax2.set_title('Jensen Alpha时间序列')
    ax2.legend()
    ax2.grid(True, alpha=0.3)
    
    # 3. Alpha分布直方图
    ax3 = axes[1, 0]
    alpha_clean = decomposition['alpha'].dropna()
    ax3.hist(alpha_clean * 100, bins=50, alpha=0.7, edgecolor='black')
    ax3.axvline(x=0, color='red', linestyle='--', linewidth=2)
    ax3.axvline(x=alpha_clean.mean() * 100, color='blue', linestyle='-', linewidth=2, 
                label=f'均值: {alpha_clean.mean()*100:.3f}%')
    ax3.set_xlabel('日Alpha (%)')
    ax3.set_ylabel('频数')
    ax3.set_title('Alpha分布')
    ax3.legend()
    ax3.grid(True, alpha=0.3)
    
    # 4. Beta与Alpha关系
    ax4 = axes[1, 1]
    beta_clean = decomposition['beta'].dropna()
    alpha_aligned = alpha_clean.loc[beta_clean.index]
    ax4.scatter(beta_clean, alpha_aligned * 100, alpha=0.3, s=20)
    ax4.axhline(y=0, color='red', linestyle='--', alpha=0.5)
    ax4.axvline(x=1, color='gray', linestyle=':', alpha=0.5)
    ax4.set_xlabel('Beta系数')
    ax4.set_ylabel('日Alpha (%)')
    ax4.set_title('Beta vs Alpha 散点图')
    ax4.grid(True, alpha=0.3)
    
    plt.suptitle(f'{stock_name} - Alpha分解分析', fontsize=14, y=1.02)
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/alpha_decomposition.png', dpi=150, bbox_inches='tight')
    plt.show()
    
    return fig
```

---

## 五、CAPM实证检验

### 5.1 零Alpha检验

```python
def test_alpha_significance(alpha_result, significance=0.05):
    """
    检验Alpha是否显著不为零
    
    H0: α = 0 (CAPM完全定价)
    H1: α ≠ 0 (存在定价偏差)
    
    Parameters:
    -----------
    alpha_result : dict
        calculate_jensen_alpha返回的结果
    significance : float
        显著性水平（默认0.05）
    
    Returns:
    --------
    dict
        检验结果
    """
    alpha = alpha_result['alpha_daily']
    t_stat = alpha_result['alpha_t']
    p_value = alpha_result['alpha_p']
    
    # 临界值（双尾检验）
    critical_value = stats.t.ppf(1 - significance/2, df=len(alpha_result['model'].resid) - 2)
    
    # 结论
    if abs(t_stat) > critical_value:
        if alpha > 0:
            conclusion = f"Alpha显著为正（t={t_stat:.3f}），资产被低估"
        else:
            conclusion = f"Alpha显著为负（t={t_stat:.3f}），资产被高估"
        reject_h0 = True
    else:
        conclusion = f"Alpha不显著（t={t_stat:.3f}），CAPM定价合理"
        reject_h0 = False
    
    return {
        'alpha': alpha,
        't_statistic': t_stat,
        'p_value': p_value,
        'critical_value': critical_value,
        'reject_h0': reject_h0,
        'conclusion': conclusion
    }
```

### 5.2 系统性风险溢价的有效性检验

```python
def test_capm_restrictions(df, risk_free_rate=0.03):
    """
    检验CAPM限制条件
    
    检验1: 截距是否为0 (E[α] = 0)
    检验2: 市场Beta的加权平均是否为1 (E[β_w] = 1)
    
    Parameters:
    -----------
    df : pd.DataFrame
        收益率数据
    risk_free_rate : float
        年化无风险利率
    
    Returns:
    --------
    dict
        各项检验结果
    """
    rf_daily = (1 + risk_free_rate) ** (1/252) - 1
    
    # 超额收益
    excess_return = df['return'] - rf_daily
    excess_market = df['market_return'] - rf_daily
    
    # OLS回归
    X = sm.add_constant(excess_market)
    model = sm.OLS(excess_return, X).fit()
    
    # 检验1：截距为零
    alpha_test = stats.ttest_1samp(model.resid, 0)
    
    # 检验2：需要横截面回归
    # 这里简化为检验Beta的稳定性
    rolling_beta = calculate_rolling_beta(df, window=60)
    beta_test = stats.ttest_1samp(rolling_beta.dropna(), 1)
    
    return {
        'alpha_test': {
            'statistic': alpha_test.statistic,
            'p_value': alpha_test.pvalue
        },
        'beta_stability_test': {
            'statistic': beta_test.statistic,
            'p_value': beta_test.pvalue,
            'mean_beta': rolling_beta.mean(),
            'std_beta': rolling_beta.std()
        },
        'r_squared': model.rsquared,
        'adj_r_squared': model.rsquared_adj
    }
```

---

## 六、完整分析案例

### 6.1 单只股票CAPM分析

```python
def capm_analysis(stock_code, stock_name, start_date="20200101", end_date="20251231"):
    """
    对单只股票进行完整的CAPM分析
    
    Parameters:
    -----------
    stock_code : str
        股票代码
    stock_name : str
        股票名称
    start_date : str
        开始日期
    end_date : str
        结束日期
    
    Returns:
    --------
    dict
        完整分析结果
    """
    print(f"\n{'='*60}")
    print(f"  {stock_name} ({stock_code}) CAPM分析报告")
    print(f"{'='*60}")
    
    # 1. 数据获取
    print("\n[1] 数据获取...")
    df = merge_stock_market_data(stock_code, start_date, end_date)
    if df is None or len(df) < 30:
        print("数据获取失败或数据不足")
        return None
    
    print(f"    数据期间: {df['date'].min().date()} 至 {df['date'].max().date()}")
    print(f"    有效观测数: {len(df)}")
    
    # 2. Beta计算
    print("\n[2] Beta系数计算...")
    beta_result = calculate_beta(df)
    print(f"    原始Beta: {beta_result['beta']:.4f}")
    print(f"    Beta t统计量: {beta_result['beta_t']:.4f}")
    print(f"    R²: {beta_result['r_squared']:.4f}")
    
    # 3. Beta调整
    print("\n[3] Beta调整...")
    adjusted_beta = bloomberg_beta_adjustment(beta_result['beta'])
    print(f"    Bloomberg调整Beta: {adjusted_beta:.4f}")
    vasicek_beta = vasicek_adjustment(beta_result['beta'])
    print(f"    Vasicek调整Beta: {vasicek_beta:.4f}")
    
    # 4. Alpha分析
    print("\n[4] Jensen's Alpha分析...")
    alpha_result = calculate_jensen_alpha(df)
    print(f"    日Alpha: {alpha_result['alpha_daily']*100:.4f}%")
    print(f"    年化Alpha: {alpha_result['alpha_annual']*100:.2f}%")
    print(f"    Alpha t统计量: {alpha_result['alpha_t']:.4f}")
    print(f"    Alpha p值: {alpha_result['alpha_p']:.4f}")
    print(f"    信息比率: {alpha_result['information_ratio']:.4f}")
    
    # 5. Alpha显著性检验
    alpha_test = test_alpha_significance(alpha_result)
    print(f"\n    Alpha检验结论: {alpha_test['conclusion']}")
    
    # 6. 可视化
    print("\n[5] 生成可视化图表...")
    plot_capm_characteristic_line(df, beta_result, stock_name)
    plot_rolling_beta(df, window=60, stock_name=stock_name)
    
    # Alpha分解
    decomposition = decompose_jensen_alpha(df)
    plot_alpha_decomposition(decomposition, stock_name)
    
    # 7. 汇总结果
    summary = {
        'stock_code': stock_code,
        'stock_name': stock_name,
        'period': f"{df['date'].min().date()} 至 {df['date'].max().date()}",
        'observations': len(df),
        'beta_raw': beta_result['beta'],
        'beta_adjusted': adjusted_beta,
        'beta_vasicek': vasicek_beta,
        'r_squared': beta_result['r_squared'],
        'alpha_daily': alpha_result['alpha_daily'],
        'alpha_annual': alpha_result['alpha_annual'],
        'alpha_t': alpha_result['alpha_t'],
        'alpha_p': alpha_result['alpha_p'],
        'information_ratio': alpha_result['information_ratio'],
        'alpha_significant': alpha_test['reject_h0'],
        'conclusion': alpha_test['conclusion']
    }
    
    print("\n[6] 分析汇总...")
    print(f"    Beta: {summary['beta_raw']:.3f} (调整后: {summary['beta_adjusted']:.3f})")
    print(f"    年化Alpha: {summary['alpha_annual']*100:.2f}%")
    print(f"    Alpha显著: {'是' if summary['alpha_significant'] else '否'}")
    print(f"    R²: {summary['r_squared']:.4f}")
    
    return summary


# 示例：分析平安银行（000001）
if __name__ == "__main__":
    result = capm_analysis("000001", "平安银行", "20200101", "20251231")
```

### 6.2 多只股票CAPM对比分析

```python
def multi_stock_capm_analysis(stock_list, start_date="20200101", end_date="20251231"):
    """
    对多只股票进行CAPM对比分析
    
    Parameters:
    -----------
    stock_list : list of tuples
        [(股票代码, 股票名称), ...]
    start_date : str
        开始日期
    end_date : str
        结束日期
    
    Returns:
    --------
    pd.DataFrame
        对比分析结果表格
    """
    results = []
    
    for stock_code, stock_name in stock_list:
        print(f"\n正在分析: {stock_name} ({stock_code})...")
        
        try:
            # 获取数据
            df = merge_stock_market_data(stock_code, start_date, end_date)
            if df is None or len(df) < 30:
                print(f"  数据不足，跳过")
                continue
            
            # CAPM分析
            beta_result = calculate_beta(df)
            alpha_result = calculate_jensen_alpha(df)
            alpha_test = test_alpha_significance(alpha_result)
            
            # 收集结果
            results.append({
                '股票代码': stock_code,
                '股票名称': stock_name,
                'Beta': beta_result['beta'],
                '调整Beta': bloomberg_beta_adjustment(beta_result['beta']),
                'R²': beta_result['r_squared'],
                '日Alpha(%)': alpha_result['alpha_daily'] * 100,
                '年化Alpha(%)': alpha_result['alpha_annual'] * 100,
                'Alpha_t': alpha_result['alpha_t'],
                'Alpha显著': '是' if alpha_test['reject_h0'] else '否',
                '信息比率': alpha_result['information_ratio'],
                '观测数': len(df)
            })
            
        except Exception as e:
            print(f"  分析失败: {e}")
    
    # 转换为DataFrame
    results_df = pd.DataFrame(results)
    results_df = results_df.sort_values('年化Alpha(%)', ascending=False)
    
    return results_df


def plot_multi_stock_comparison(results_df):
    """
    可视化多只股票的CAPM对比分析
    """
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    
    # 1. Beta对比柱状图
    ax1 = axes[0, 0]
    colors = ['green' if b < 1 else 'red' for b in results_df['Beta']]
    ax1.barh(results_df['股票名称'], results_df['Beta'], color=colors, alpha=0.7)
    ax1.axvline(x=1, color='black', linestyle='--', linewidth=2, label='Beta=1')
    ax1.set_xlabel('Beta系数')
    ax1.set_title('Beta系数对比')
    ax1.legend()
    ax1.grid(True, alpha=0.3, axis='x')
    
    # 2. Alpha对比柱状图
    ax2 = axes[0, 1]
    colors = ['green' if a > 0 else 'red' for a in results_df['年化Alpha(%)']]
    ax2.barh(results_df['股票名称'], results_df['年化Alpha(%)'], color=colors, alpha=0.7)
    ax2.axvline(x=0, color='black', linestyle='--', linewidth=2)
    ax2.set_xlabel('年化Alpha (%)')
    ax2.set_title('年化Alpha对比')
    ax2.grid(True, alpha=0.3, axis='x')
    
    # 3. Beta vs Alpha散点图
    ax3 = axes[1, 0]
    scatter = ax3.scatter(
        results_df['Beta'],
        results_df['年化Alpha(%)'],
        s=100,
        c=results_df['R²'],
        cmap='RdYlGn',
        alpha=0.7
    )
    ax3.axhline(y=0, color='gray', linestyle='--', alpha=0.5)
    ax3.axvline(x=1, color='gray', linestyle='--', alpha=0.5)
    
    # 添加标签
    for i, row in results_df.iterrows():
        ax3.annotate(
            row['股票名称'],
            (row['Beta'], row['年化Alpha(%)']),
            xytext=(5, 5),
            textcoords='offset points',
            fontsize=8
        )
    
    ax3.set_xlabel('Beta系数')
    ax3.set_ylabel('年化Alpha (%)')
    ax3.set_title('Beta vs Alpha (颜色=R²)')
    plt.colorbar(scatter, ax=ax3, label='R²')
    ax3.grid(True, alpha=0.3)
    
    # 4. R²对比
    ax4 = axes[1, 1]
    ax4.barh(results_df['股票名称'], results_df['R²'], alpha=0.7, color='steelblue')
    ax4.set_xlabel('R² (CAPM解释力)')
    ax4.set_title('CAPM解释力对比 (R²)')
    ax4.grid(True, alpha=0.3, axis='x')
    
    plt.suptitle('多股票CAPM分析对比', fontsize=14, y=1.02)
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/multi_stock_capm.png', dpi=150, bbox_inches='tight')
    plt.show()
    
    return fig


# 示例：多只股票对比
if __name__ == "__main__":
    stock_list = [
        ("000001", "平安银行"),
        ("000002", "万科A"),
        ("600519", "贵州茅台"),
        ("600036", "招商银行"),
        ("601318", "中国平安")
    ]
    
    results = multi_stock_capm_analysis(stock_list, "20200101", "20251231")
    print("\n分析结果汇总:")
    print(results.to_string(index=False))
    
    plot_multi_stock_comparison(results)
```

---

## 七、常见问题与注意事项

### 7.1 数据相关问题

| 问题 | 原因 | 解决方案 |
|:-----|:-----|:---------|
| 收益率计算错误 | 复权方式选择不当 | 使用前复权或后复权，保持一致性 |
| Beta估计不稳定 | 数据噪声大 | 增加样本量或使用滚动估计 |
| R²过低 | 个股与市场关联弱 | 考虑使用行业指数作为市场代理 |
| 缺失值过多 | 停牌或数据获取失败 | 剔除停牌期间或使用插值 |

### 7.2 模型应用注意事项

1. **无风险利率选择**：根据分析目的选择合适的无风险利率，期限应与投资期限匹配

2. **市场组合代理**：A股市场常用沪深300指数作为市场代理，但存在一定偏差

3. **Beta时变性**：Beta系数会随时间变化，建议定期更新

4. **样本选择偏差**：仅使用历史数据估计可能存在前视偏差

### 7.3 结果解读建议

1. **Alpha显著性**：Alpha显著不等于一定有投资价值，需考虑交易成本

2. **R²解释**：R²低说明CAPM解释力弱，但不一定意味着模型错误

3. **Beta风险**：高Beta不等于高风险，反映的是系统性风险暴露程度

---

## 八、总结

本文系统介绍了CAPM模型的Python实战应用，主要内容包括：

1. **数据获取**：使用AkShare获取A股股票和市场指数数据
2. **Beta计算**：通过OLS回归和协方差/方差法计算Beta系数
3. **Beta调整**：Blume、Vasicek、Bloomberg等Beta调整方法
4. **Alpha分析**：Jensen's Alpha计算与显著性检验
5. **实证可视化**：CAPM特征线、滚动Beta、Alpha分解等图表
6. **多股票对比**：批量分析多只股票的CAPM特征

CAPM作为经典资产定价模型，虽然在实证中存在诸多局限性，但其核心理念——系统性风险是资产定价的核心因素——仍具有重要的理论和实践价值。将CAPM与多因子模型、机器学习方法结合使用，可以构建更完善的量化投资框架。

---

## 参考代码库

本文使用的完整代码已整理为Jupyter Notebook，可在以下地址获取：

- [CAPM实战Notebook](../code/R05/capm_practical_guide.ipynb)

---

## 参考文献

1. Sharpe, W. F. (1964). Capital asset prices: A theory of market equilibrium under conditions of risk. *Journal of Finance*, 19(3), 425-442.

2. Blume, M. E. (1971). On the assessment of risk. *Journal of Finance*, 26(1), 1-10.

3. Vasicek, O. A. (1973). A note on using cross-sectional information in estimation of equilibrium expected returns. *Journal of Finance*, 28(5), 1233-1240.

4. Jensen, M. C. (1968). The performance of mutual funds in the period 1945-1964. *Journal of Finance*, 23(2), 389-416.

5. Roll, R. (1977). A critique of the asset pricing theory's tests. *Journal of Financial Economics*, 4(2), 129-176.

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao（老子道）· Dao Quant Research**
