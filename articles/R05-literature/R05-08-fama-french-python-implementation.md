---
title: "Fama-French多因子模型Python实战指南"
description: "基于Python实现Fama-French三因子与五因子模型的完整教程，涵盖因子构建、回归分析、投资组合构建与回测"
author: "laozdao"
date: "2026-06-01"
category: "R05"
tags:
  - "Fama-French"
  - "Python"
  - "多因子模型"
  - "因子构建"
  - "量化投资"
  - "实践教程"
series: "研究方法论"
series_order: 8
series_title: "经典文献系列"
version: "1.0"
math: true
summary: "本文是Fama-French多因子模型的Python实战教程，详细介绍如何使用Python实现因子构建、回归分析、投资组合构建与回测。文章提供完整的可运行代码示例，帮助量化投资者将Fama-French理论应用于实际投资决策。"
---

# Fama-French多因子模型Python实战指南

## 摘要

本文是Fama-French多因子模型的Python实战教程，系统介绍如何利用Python实现Fama-French三因子与五因子模型的完整分析流程。文章涵盖数据获取与预处理、因子构建（2×3分组方法）、因子回归分析、纯因子组合构建、多因子选股策略以及回测评估等核心内容。通过详细的代码示例和实际数据分析，帮助读者将Fama-French理论快速应用于量化投资实践。

---

## 一、环境准备与数据获取

### 1.1 必需的Python库

```python
# 数据处理
import pandas as pd
import numpy as np
from datetime import datetime, timedelta

# 可视化
import matplotlib.pyplot as plt
import seaborn as sns

# 统计分析
import scipy.stats as stats
import statsmodels.api as sm
from scipy.optimize import minimize

# 金融数据获取
import akshare as ak  # A股数据

# 设置中文显示
plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS']
plt.rcParams['axes.unicode_minus'] = False

# 设置显示选项
pd.set_option('display.max_columns', None)
pd.set_option('display.width', None)
```

### 1.2 获取A股数据

```python
def get_a_stock_data(start_date, end_date):
    """
    获取A股股票基本信息和历史数据
    
    Parameters:
    -----------
    start_date : str
        开始日期，格式"YYYYMMDD"
    end_date : str
        结束日期，格式"YYYYMMDD"
    
    Returns:
    --------
    tuple
        (stock_info, price_data, financial_data)
    """
    # 获取股票列表
    stock_info = ak.stock_zh_a_spot_em()
    
    # 筛选非ST、非金融股
    stock_info = stock_info[
        (~stock_info['名称'].str.contains('ST', na=False)) &
        (~stock_info['名称'].str.contains('银行|保险|证券|信托', na=False))
    ]
    
    print(f"筛选后股票数量: {len(stock_info)}")
    
    return stock_info


def get_stock_prices(stock_code, start_date, end_date):
    """
    获取单只股票历史价格数据
    
    Parameters:
    -----------
    stock_code : str
        股票代码
    start_date : str
        开始日期
    end_date : str
        结束日期
    
    Returns:
    --------
    pd.DataFrame
        包含日期、开盘、收盘、成交量等字段
    """
    try:
        df = ak.stock_zh_a_hist(
            symbol=stock_code,
            period="daily",
            start_date=start_date,
            end_date=end_date,
            adjust="qfq"  # 前复权
        )
        
        df['日期'] = pd.to_datetime(df['日期'])
        df = df.rename(columns={
            '日期': 'date',
            '开盘': 'open',
            '收盘': 'close',
            '成交量': 'volume',
            '成交额': 'amount'
        })
        
        # 计算收益率
        df['return'] = df['close'].pct_change()
        
        return df[['date', 'close', 'volume', 'amount', 'return']]
    
    except Exception as e:
        print(f"获取{stock_code}数据失败: {e}")
        return None


def get_market_data(start_date, end_date):
    """
    获取市场指数数据（沪深300作为市场代理）
    """
    try:
        df = ak.stock_zh_index_daily(symbol="sh000300")
        df['日期'] = pd.to_datetime(df['日期'])
        df = df[(df['日期'] >= start_date) & (df['日期'] <= end_date)]
        df = df.rename(columns={'日期': 'date', '收盘': 'close'})
        df['market_return'] = df['close'].pct_change()
        
        return df[['date', 'close', 'market_return']]
    except Exception as e:
        print(f"获取市场数据失败: {e}")
        return None
```

### 1.3 获取财务数据

```python
def get_financial_data(year, quarter):
    """
    获取财务数据用于计算BM、盈利能力等指标
    
    Parameters:
    -----------
    year : int
        年份
    quarter : int
        季度（1-4）
    
    Returns:
    --------
    pd.DataFrame
        财务指标数据
    """
    try:
        # 获取主要财务指标
        financial = ak.stock_financial_report_sina(
            stock="all", 
            symbol="主要财务指标"
        )
        
        # 获取资产负债表
        balance = ak.stock_financial_report_sina(
            stock="all",
            symbol="资产负债表"
        )
        
        # 合并数据并计算指标
        # ...
        
        return financial
    
    except Exception as e:
        print(f"获取财务数据失败: {e}")
        return None


def calculate_book_to_market(stock_data, financial_data):
    """
    计算账面市值比（Book-to-Market Ratio）
    
    BM = 股东权益 / 市值
    """
    # 股东权益（账面价值）
    book_value = financial_data['股东权益合计']
    
    # 市值
    market_cap = stock_data['总市值']
    
    # 计算BM
    bm_ratio = book_value / market_cap
    
    return bm_ratio


def calculate_operating_profitability(financial_data):
    """
    计算营业利润率（Operating Profitability）
    
    OP = (营业收入 - 营业成本 - 利息支出) / 账面价值
    """
    revenue = financial_data['营业收入']
    cogs = financial_data['营业成本']
    interest = financial_data['利息支出'].fillna(0)
    book_value = financial_data['股东权益合计']
    
    op = (revenue - cogs - interest) / book_value
    
    return op


def calculate_asset_growth(financial_data):
    """
    计算资产增长率（Asset Growth）
    
    AG = (总资产_t - 总资产_t-1) / 总资产_t-1
    """
    total_assets = financial_data['资产总计']
    asset_growth = total_assets.pct_change()
    
    return asset_growth
```

---

## 二、Fama-French因子构建

### 2.1 2×3分组方法实现

```python
def construct_ff_factors(stock_data, date, factor_type='3factor'):
    """
    构建Fama-French因子
    
    Parameters:
    -----------
    stock_data : pd.DataFrame
        包含股票收益率、市值、BM等数据的数据框
    date : datetime
        当前日期
    factor_type : str
        '3factor'或'5factor'
    
    Returns:
    --------
    dict
        包含各因子收益的字典
    """
    # 筛选有效数据
    stocks = stock_data.dropna(subset=['market_cap', 'bm_ratio', 'return'])
    
    # 剔除ST股票和金融股
    stocks = stocks[~stocks['is_st']]
    stocks = stocks[~stocks['industry'].isin(['银行', '保险', '证券'])]
    
    # ========== 按市值分组 ==========
    # 使用中位数作为分界点（Fama-French使用NYSE中位数）
    size_median = stocks['market_cap'].median()
    stocks['size_group'] = np.where(stocks['market_cap'] <= size_median, 'S', 'B')
    
    # ========== 按BM分组 ==========
    bm_30 = stocks['bm_ratio'].quantile(0.3)
    bm_70 = stocks['bm_ratio'].quantile(0.7)
    
    def assign_bm_group(bm):
        if bm <= bm_30:
            return 'L'
        elif bm <= bm_70:
            return 'M'
        else:
            return 'H'
    
    stocks['bm_group'] = stocks['bm_ratio'].apply(assign_bm_group)
    
    # ========== 构建6个市值-BM组合 ==========
    portfolios = {}
    for size in ['S', 'B']:
        for bm in ['L', 'M', 'H']:
            mask = (stocks['size_group'] == size) & (stocks['bm_group'] == bm)
            portfolio = stocks[mask]
            
            if len(portfolio) > 0:
                # 市值加权收益
                weights = portfolio['market_cap'] / portfolio['market_cap'].sum()
                portfolios[f'{size}{bm}'] = (portfolio['return'] * weights).sum()
            else:
                portfolios[f'{size}{bm}'] = np.nan
    
    # ========== 计算SMB和HML ==========
    # SMB = 1/3(SL + SM + SH) - 1/3(BL + BM + BH)
    smb = (portfolios['SL'] + portfolios['SM'] + portfolios['SH']) / 3 - \
          (portfolios['BL'] + portfolios['BM'] + portfolios['BH']) / 3
    
    # HML = 1/2(SH + BH) - 1/2(SL + BL)
    hml = (portfolios['SH'] + portfolios['BH']) / 2 - \
          (portfolios['SL'] + portfolios['BL']) / 2
    
    result = {
        'SMB': smb,
        'HML': hml,
        'portfolios': portfolios
    }
    
    # ========== 五因子：加入RMW和CMA ==========
    if factor_type == '5factor':
        # 按盈利能力分组
        op_30 = stocks['operating_profitability'].quantile(0.3)
        op_70 = stocks['operating_profitability'].quantile(0.3)
        
        def assign_op_group(op):
            if op <= op_30:
                return 'W'
            elif op <= op_70:
                return 'N'
            else:
                return 'R'
        
        stocks['op_group'] = stocks['operating_profitability'].apply(assign_op_group)
        
        # 按投资水平分组
        inv_30 = stocks['asset_growth'].quantile(0.3)
        inv_70 = stocks['asset_growth'].quantile(0.7)
        
        def assign_inv_group(inv):
            if inv <= inv_30:
                return 'C'
            elif inv <= inv_70:
                return 'N'
            else:
                return 'A'
        
        stocks['inv_group'] = stocks['asset_growth'].apply(assign_inv_group)
        
        # 构建盈利组合
        op_portfolios = {}
        for size in ['S', 'B']:
            for op in ['W', 'N', 'R']:
                mask = (stocks['size_group'] == size) & (stocks['op_group'] == op)
                portfolio = stocks[mask]
                if len(portfolio) > 0:
                    weights = portfolio['market_cap'] / portfolio['market_cap'].sum()
                    op_portfolios[f'{size}{op}'] = (portfolio['return'] * weights).sum()
        
        # RMW = 1/2(SR + BR) - 1/2(SW + BW)
        rmw = (op_portfolios['SR'] + op_portfolios['BR']) / 2 - \
              (op_portfolios['SW'] + op_portfolios['BW']) / 2
        
        # 构建投资组合
        inv_portfolios = {}
        for size in ['S', 'B']:
            for inv in ['A', 'N', 'C']:
                mask = (stocks['size_group'] == size) & (stocks['inv_group'] == inv)
                portfolio = stocks[mask]
                if len(portfolio) > 0:
                    weights = portfolio['market_cap'] / portfolio['market_cap'].sum()
                    inv_portfolios[f'{size}{inv}'] = (portfolio['return'] * weights).sum()
        
        # CMA = 1/2(SC + BC) - 1/2(SA + BA)
        cma = (inv_portfolios['SC'] + inv_portfolios['BC']) / 2 - \
              (inv_portfolios['SA'] + inv_portfolios['BA']) / 2
        
        result['RMW'] = rmw
        result['CMA'] = cma
    
    return result
```

### 2.2 滚动因子计算

```python
def calculate_rolling_ff_factors(price_data, financial_data, window='M'):
    """
    计算滚动Fama-French因子
    
    Parameters:
    -----------
    price_data : pd.DataFrame
        价格数据
    financial_data : pd.DataFrame
        财务数据
    window : str
        滚动窗口，'M'为月度，'Q'为季度
    
    Returns:
    --------
    pd.DataFrame
        因子收益时间序列
    """
    factors_list = []
    
    # 按月或季度分组
    if window == 'M':
        periods = price_data.groupby([price_data['date'].dt.year, price_data['date'].dt.month])
    else:
        periods = price_data.groupby([price_data['date'].dt.year, price_data['date'].dt.quarter])
    
    for (year, month), group in periods:
        # 获取当前期的财务数据
        current_financial = financial_data[
            (financial_data['year'] == year) &
            (financial_data['month'] <= month)
        ].groupby('stock_code').last().reset_index()
        
        # 合并价格和财务数据
        merged = pd.merge(group, current_financial, on='stock_code', how='inner')
        
        # 计算当期因子
        if len(merged) > 50:  # 确保有足够股票
            factors = construct_ff_factors(merged, group['date'].iloc[0])
            factors['date'] = group['date'].iloc[0]
            factors_list.append(factors)
    
    return pd.DataFrame(factors_list)
```

---

## 三、因子回归分析

### 3.1 单资产因子回归

```python
def ff_factor_regression(stock_returns, factors, model='3factor'):
    """
    Fama-French因子回归分析
    
    Parameters:
    -----------
    stock_returns : pd.Series
        股票收益率序列
    factors : pd.DataFrame
        因子收益数据框
    model : str
        '3factor'或'5factor'
    
    Returns:
    --------
    dict
        回归结果
    """
    # 准备数据
    data = pd.concat([stock_returns, factors], axis=1).dropna()
    
    # 选择因子
    if model == '3factor':
        X = data[['RMRF', 'SMB', 'HML']]
    else:  # 5factor
        X = data[['RMRF', 'SMB', 'HML', 'RMW', 'CMA']]
    
    X = sm.add_constant(X)  # 添加截距项（Alpha）
    y = data['return']
    
    # OLS回归
    model_fit = sm.OLS(y, X).fit()
    
    # 提取结果
    result = {
        'alpha': model_fit.params['const'],
        'alpha_t': model_fit.tvalues['const'],
        'alpha_p': model_fit.pvalues['const'],
        'beta_m': model_fit.params['RMRF'],
        'beta_smb': model_fit.params['SMB'],
        'beta_hml': model_fit.params['HML'],
        'r_squared': model_fit.rsquared,
        'adj_r_squared': model_fit.rsquared_adj,
        'residuals': model_fit.resid,
        'model': model_fit
    }
    
    if model == '5factor':
        result['beta_rmw'] = model_fit.params['RMW']
        result['beta_cma'] = model_fit.params['CMA']
    
    return result
```

### 3.2 多资产批量回归

```python
def batch_ff_regression(returns_df, factors_df, model='3factor'):
    """
    批量进行Fama-French回归分析
    
    Parameters:
    -----------
    returns_df : pd.DataFrame
        多资产收益率数据框（列为资产，行为日期）
    factors_df : pd.DataFrame
        因子收益数据框
    model : str
        '3factor'或'5factor'
    
    Returns:
    --------
    pd.DataFrame
        各资产的回归结果汇总
    """
    results = []
    
    for stock in returns_df.columns:
        try:
            result = ff_factor_regression(returns_df[stock], factors_df, model)
            
            row = {
                'stock': stock,
                'alpha': result['alpha'],
                'alpha_t': result['alpha_t'],
                'alpha_p': result['alpha_p'],
                'beta_m': result['beta_m'],
                'beta_smb': result['beta_smb'],
                'beta_hml': result['beta_hml'],
                'r_squared': result['r_squared']
            }
            
            if model == '5factor':
                row['beta_rmw'] = result['beta_rmw']
                row['beta_cma'] = result['beta_cma']
            
            results.append(row)
        
        except Exception as e:
            print(f"{stock}回归失败: {e}")
    
    return pd.DataFrame(results)
```

### 3.3 Fama-MacBeth横截面回归

```python
def fama_macbeth_regression(returns_df, factors_df, model='3factor'):
    """
    Fama-MacBeth两步回归法
    
    Parameters:
    -----------
    returns_df : pd.DataFrame
        资产收益率数据
    factors_df : pd.DataFrame
        因子收益数据
    model : str
        '3factor'或'5factor'
    
    Returns:
    --------
    dict
        Fama-MacBeth回归结果
    """
    # 第一步：时间序列回归估计Beta
    betas = batch_ff_regression(returns_df, factors_df, model)
    betas = betas.set_index('stock')
    
    # 第二步：横截面回归（每期）
    gamma_estimates = []
    
    for date in returns_df.index:
        # 当期收益率
        y = returns_df.loc[date]
        
        # 当期Beta
        X = betas[['beta_m', 'beta_smb', 'beta_hml']]
        if model == '5factor':
            X = betas[['beta_m', 'beta_smb', 'beta_hml', 'beta_rmw', 'beta_cma']]
        
        X = sm.add_constant(X)
        
        # 合并数据
        data = pd.concat([y, X], axis=1).dropna()
        
        if len(data) > 10:
            # 横截面回归
            model_fit = sm.OLS(data.iloc[:, 0], data.iloc[:, 1:]).fit()
            gamma_estimates.append(model_fit.params)
    
    # 第三步：计算均值和t统计量
    gamma_df = pd.DataFrame(gamma_estimates)
    
    results = {
        'gamma_mean': gamma_df.mean(),
        'gamma_std': gamma_df.std(),
        'gamma_t': gamma_df.mean() / (gamma_df.std() / np.sqrt(len(gamma_df))),
        'gamma_df': gamma_df
    }
    
    return results
```

---

## 四、因子分析与可视化

### 4.1 因子收益统计

```python
def analyze_factor_performance(factors_df):
    """
    分析因子收益表现
    
    Parameters:
    -----------
    factors_df : pd.DataFrame
        因子收益数据框
    
    Returns:
    --------
    pd.DataFrame
        因子统计摘要
    """
    stats_summary = pd.DataFrame()
    
    for factor in ['SMB', 'HML', 'RMW', 'CMA']:
        if factor in factors_df.columns:
            stats = {
                '年化收益': factors_df[factor].mean() * 252,
                '年化波动': factors_df[factor].std() * np.sqrt(252),
                '夏普比率': (factors_df[factor].mean() * 252) / (factors_df[factor].std() * np.sqrt(252)),
                '最大回撤': (factors_df[factor].cumsum() - factors_df[factor].cumsum().cummax()).min(),
                't统计量': factors_df[factor].mean() / (factors_df[factor].std() / np.sqrt(len(factors_df)))
            }
            stats_summary[factor] = stats
    
    return stats_summary.T
```

### 4.2 因子相关性热力图

```python
def plot_factor_correlation(factors_df):
    """
    绘制因子相关性热力图
    """
    # 计算相关性
    corr = factors_df[['RMRF', 'SMB', 'HML', 'RMW', 'CMA']].corr()
    
    # 绘图
    fig, ax = plt.subplots(figsize=(8, 6))
    
    sns.heatmap(
        corr,
        annot=True,
        fmt='.2f',
        cmap='RdBu_r',
        center=0,
        vmin=-1,
        vmax=1,
        square=True,
        ax=ax
    )
    
    ax.set_title('Fama-French因子相关性矩阵', fontsize=14)
    
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/ff_factor_correlation.png', dpi=150)
    plt.show()
    
    return fig
```

### 4.3 因子累积收益曲线

```python
def plot_factor_cumulative_returns(factors_df):
    """
    绘制因子累积收益曲线
    """
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    axes = axes.flatten()
    
    factors = ['SMB', 'HML', 'RMW', 'CMA']
    colors = ['blue', 'green', 'red', 'orange']
    
    for i, (factor, color) in enumerate(zip(factors, colors)):
        if factor in factors_df.columns:
            ax = axes[i]
            
            # 累积收益
            cum_returns = (1 + factors_df[factor]).cumprod() - 1
            
            ax.plot(cum_returns.index, cum_returns * 100, color=color, linewidth=1.5)
            ax.axhline(y=0, color='gray', linestyle='--', alpha=0.5)
            
            # 统计信息
            annual_return = factors_df[factor].mean() * 252 * 100
            sharpe = (factors_df[factor].mean() * 252) / (factors_df[factor].std() * np.sqrt(252))
            
            stats_text = f'年化收益: {annual_return:.2f}%\n夏普比率: {sharpe:.2f}'
            ax.text(
                0.05, 0.95, stats_text,
                transform=ax.transAxes,
                fontsize=10,
                verticalalignment='top',
                bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.8)
            )
            
            ax.set_title(f'{factor}因子累积收益', fontsize=12)
            ax.set_xlabel('日期')
            ax.set_ylabel('累积收益 (%)')
            ax.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.savefig('../assets/figures/R05/ff_factor_cumulative.png', dpi=150)
    plt.show()
    
    return fig
```

---

## 五、投资组合构建

### 5.1 纯因子组合

```python
def construct_pure_factor_portfolio(stocks_df, target_factor, other_factors):
    """
    构建纯因子组合
    
    Parameters:
    -----------
    stocks_df : pd.DataFrame
        股票数据
    target_factor : str
        目标因子名称
    other_factors : list
        其他因子名称列表
    
    Returns:
    --------
    tuple
        (多头组合, 空头组合)
    """
    # 回归剔除其他因子影响
    X = stocks_df[other_factors]
    X = sm.add_constant(X)
    y = stocks_df[target_factor]
    
    model = sm.OLS(y, X).fit()
    pure_factor = model.resid
    
    # 添加纯因子到数据框
    stocks_df['pure_factor'] = pure_factor
    
    # 构建多空组合（各10%）
    n_stocks = len(stocks_df)
    n_select = max(int(n_stocks * 0.1), 5)
    
    long_stocks = stocks_df.nlargest(n_select, 'pure_factor')
    short_stocks = stocks_df.nsmallest(n_select, 'pure_factor')
    
    return long_stocks, short_stocks
```

### 5.2 多因子选股策略

```python
def ff_multi_factor_strategy(stocks_df, factor_weights, n_select=50):
    """
    Fama-French多因子选股策略
    
    Parameters:
    -----------
    stocks_df : pd.DataFrame
        股票数据
    factor_weights : dict
        因子权重，如{'SMB': 0.3, 'HML': 0.3, 'RMW': 0.2, 'CMA': 0.2}
    n_select : int
        选股数量
    
    Returns:
    --------
    pd.DataFrame
        选中的股票
    """
    # 标准化因子暴露
    factors = list(factor_weights.keys())
    
    for factor in factors:
        if factor in stocks_df.columns:
            stocks_df[f'{factor}_zscore'] = (
                stocks_df[factor] - stocks_df[factor].mean()
            ) / stocks_df[factor].std()
    
    # 计算综合得分
    stocks_df['factor_score'] = 0
    for factor, weight in factor_weights.items():
        zscore_col = f'{factor}_zscore'
        if zscore_col in stocks_df.columns:
            stocks_df['factor_score'] += weight * stocks_df[zscore_col]
    
    # 选择高分股票
    selected = stocks_df.nlargest(n_select, 'factor_score')
    
    return selected
```

### 5.3 因子轮动策略

```python
def factor_momentum_strategy(factors_df, lookback=12, hold=1):
    """
    因子动量轮动策略
    
    Parameters:
    -----------
    factors_df : pd.DataFrame
        因子收益数据
    lookback : int
        回看期（月）
    hold : int
        持有期（月）
    
    Returns:
    --------
    pd.DataFrame
        策略收益
    """
    strategy_returns = []
    
    for i in range(lookback, len(factors_df) - hold, hold):
        # 回看期收益
        past_returns = factors_df.iloc[i-lookback:i]
        
        # 选择表现最好的因子
        best_factor = past_returns.mean().idxmax()
        
        # 持有期收益
        future_return = factors_df[best_factor].iloc[i:i+hold].mean()
        
        strategy_returns.append({
            'date': factors_df.index[i],
            'selected_factor': best_factor,
            'return': future_return
        })
    
    return pd.DataFrame(strategy_returns)
```

---

## 六、策略回测

### 6.1 简单回测框架

```python
def backtest_ff_strategy(returns_df, signals_df, rebalance_freq='M'):
    """
    Fama-French策略回测
    
    Parameters:
    -----------
    returns_df : pd.DataFrame
        股票收益率数据
    signals_df : pd.DataFrame
        选股信号数据
    rebalance_freq : str
        调仓频率
    
    Returns:
    --------
    dict
        回测结果
    """
    portfolio_returns = []
    
    # 按调仓频率分组
    if rebalance_freq == 'M':
        periods = signals_df.groupby([signals_df.index.year, signals_df.index.month])
    else:
        periods = signals_df.groupby([signals_df.index.year, signals_df.index.quarter])
    
    for (year, period), signal in periods:
        # 获取当期选中的股票
        selected_stocks = signal[signal['selected'] == 1].index
        
        # 获取下期收益
        next_period = returns_df.loc[
            (returns_df.index.year == year) & 
            (returns_df.index.month == period)
        ]
        
        if len(selected_stocks) > 0 and len(next_period) > 0:
            # 等权重组合收益
            portfolio_return = next_period[selected_stocks].mean(axis=1).mean()
            portfolio_returns.append({
                'date': next_period.index[0],
                'return': portfolio_return
            })
    
    portfolio_df = pd.DataFrame(portfolio_returns).set_index('date')
    
    # 计算绩效指标
    total_return = (1 + portfolio_df['return']).prod() - 1
    annual_return = portfolio_df['return'].mean() * 252
    annual_vol = portfolio_df['return'].std() * np.sqrt(252)
    sharpe = annual_return / annual_vol
    max_dd = (portfolio_df['return'].cumsum() - portfolio_df['return'].cumsum().cummax()).min()
    
    return {
        'portfolio_returns': portfolio_df,
        'total_return': total_return,
        'annual_return': annual_return,
        'annual_volatility': annual_vol,
        'sharpe_ratio': sharpe,
        'max_drawdown': max_dd
    }
```

### 6.2 绩效评估

```python
def evaluate_strategy_performance(portfolio_returns, benchmark_returns=None):
    """
    评估策略绩效
    
    Parameters:
    -----------
    portfolio_returns : pd.Series
        组合收益率
    benchmark_returns : pd.Series
        基准收益率（可选）
    
    Returns:
    --------
    pd.DataFrame
        绩效指标
    """
    # 基本指标
    metrics = {
        '总收益': (1 + portfolio_returns).prod() - 1,
        '年化收益': portfolio_returns.mean() * 252,
        '年化波动': portfolio_returns.std() * np.sqrt(252),
        '夏普比率': (portfolio_returns.mean() * 252) / (portfolio_returns.std() * np.sqrt(252)),
        '最大回撤': (portfolio_returns.cumsum() - portfolio_returns.cumsum().cummax()).min(),
        '胜率': (portfolio_returns > 0).mean(),
        '盈亏比': portfolio_returns[portfolio_returns > 0].mean() / abs(portfolio_returns[portfolio_returns < 0].mean()),
        'Calmar比率': (portfolio_returns.mean() * 252) / abs((portfolio_returns.cumsum() - portfolio_returns.cumsum().cummax()).min())
    }
    
    # 相对基准指标
    if benchmark_returns is not None:
        excess_returns = portfolio_returns - benchmark_returns
        metrics['Alpha'] = excess_returns.mean() * 252
        metrics['跟踪误差'] = excess_returns.std() * np.sqrt(252)
        metrics['信息比率'] = metrics['Alpha'] / metrics['跟踪误差']
        metrics['Beta'] = portfolio_returns.cov(benchmark_returns) / benchmark_returns.var()
    
    return pd.Series(metrics)
```

---

## 七、完整实战案例

### 7.1 单期因子构建案例

```python
def demo_factor_construction():
    """
    演示单期因子构建流程
    """
    print("=" * 60)
    print("Fama-French因子构建演示")
    print("=" * 60)
    
    # 模拟数据
    np.random.seed(42)
    n_stocks = 500
    
    stocks_df = pd.DataFrame({
        'stock_code': [f'{i:06d}' for i in range(n_stocks)],
        'market_cap': np.random.lognormal(10, 1.5, n_stocks),
        'bm_ratio': np.random.lognormal(0, 0.5, n_stocks),
        'operating_profitability': np.random.normal(0.1, 0.05, n_stocks),
        'asset_growth': np.random.normal(0.15, 0.1, n_stocks),
        'return': np.random.normal(0.001, 0.02, n_stocks),
        'is_st': False,
        'industry': np.random.choice(['科技', '消费', '医药', '制造'], n_stocks)
    })
    
    # 构建因子
    factors = construct_ff_factors(stocks_df, datetime.now(), factor_type='5factor')
    
    print("\n因子收益:")
    print(f"  SMB (规模因子): {factors['SMB']:.4f}")
    print(f"  HML (价值因子): {factors['HML']:.4f}")
    print(f"  RMW (盈利因子): {factors['RMW']:.4f}")
    print(f"  CMA (投资因子): {factors['CMA']:.4f}")
    
    print("\n6个市值-BM组合收益:")
    for portfolio, ret in factors['portfolios'].items():
        print(f"  {portfolio}: {ret:.4f}")
    
    return factors


# 运行演示
if __name__ == "__main__":
    demo_factors = demo_factor_construction()
```

### 7.2 多因子选股案例

```python
def demo_stock_selection():
    """
    演示多因子选股流程
    """
    print("\n" + "=" * 60)
    print("多因子选股演示")
    print("=" * 60)
    
    # 模拟股票池
    np.random.seed(42)
    n_stocks = 1000
    
    stocks_df = pd.DataFrame({
        'stock_code': [f'{i:06d}' for i in range(n_stocks)],
        'stock_name': [f'股票{i}' for i in range(n_stocks)],
        'SMB': np.random.normal(0, 1, n_stocks),
        'HML': np.random.normal(0, 1, n_stocks),
        'RMW': np.random.normal(0, 1, n_stocks),
        'CMA': np.random.normal(0, 1, n_stocks),
        'market_cap': np.random.lognormal(10, 1.5, n_stocks)
    })
    
    # 设置因子权重
    factor_weights = {
        'SMB': 0.25,  # 规模因子
        'HML': 0.25,  # 价值因子
        'RMW': 0.25,  # 盈利因子
        'CMA': 0.25   # 投资因子
    }
    
    # 选股
    selected = ff_multi_factor_strategy(stocks_df, factor_weights, n_select=50)
    
    print(f"\n选中 {len(selected)} 只股票:")
    print(selected[['stock_code', 'stock_name', 'factor_score']].head(10))
    
    print("\n选中股票的因子暴露均值:")
    print(selected[['SMB', 'HML', 'RMW', 'CMA']].mean())
    
    return selected


# 运行演示
if __name__ == "__main__":
    demo_selected = demo_stock_selection()
```

---

## 八、常见问题与注意事项

### 8.1 数据相关问题

| 问题 | 原因 | 解决方案 |
|:-----|:-----|:---------|
| 财务数据缺失 | 新股或ST股 | 剔除缺失数据或使用插值 |
| BM计算异常 | 账面价值为负 | 剔除负BM股票 |
| 收益率异常 | 停牌或涨跌停 | 使用复权价格，剔除极端值 |
| 样本量不足 | 分组后股票太少 | 调整分组阈值或使用条件排序 |

### 8.2 模型应用注意事项

1. **再平衡频率**：Fama-French建议年度再平衡（6月底），但实践中可根据需要调整
2. **市值加权 vs 等权重**：Fama-French使用市值加权，等权重可能产生不同结果
3. **行业中性**：考虑行业中性化处理，避免行业集中风险
4. **交易成本**：实际交易中需考虑滑点和佣金

### 8.3 A股市场特殊考虑

1. **小市值效应更显著**：A股小市值溢价明显，但流动性风险也高
2. **价值效应波动大**：价值因子在A股效果不如美股稳定
3. **盈利因子效果好**：质量类因子在A股表现较好
4. **投资因子效果弱**：CMA因子在A股解释力有限

---

## 九、总结

本文系统介绍了Fama-French多因子模型的Python实战应用，主要内容包括：

1. **数据获取**：使用AkShare获取A股价格和财务数据
2. **因子构建**：2×3独立双重排序方法构建SMB、HML、RMW、CMA因子
3. **回归分析**：时间序列回归和Fama-MacBeth横截面回归
4. **因子分析**：因子相关性、累积收益、统计特征分析
5. **投资组合**：纯因子组合、多因子选股、因子轮动策略
6. **回测评估**：简单回测框架和绩效评估指标

Fama-French模型作为多因子投资的基石，为量化投资提供了系统的理论框架。在A股市场应用时，需要根据市场特征进行适当调整，特别关注规模因子和盈利因子的效果。

---

## 参考代码库

本文使用的完整代码已整理为Jupyter Notebook，可在以下地址获取：

- [Fama-French实战Notebook](../code/R05/fama_french_practical_guide.ipynb)

---

## 参考文献

1. Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. *Journal of Financial Economics*, 33(1), 3-56.

2. Fama, E. F., & French, K. R. (2015). A five-factor asset pricing model. *Journal of Financial Economics*, 116(1), 1-22.

3. Carhart, M. M. (1997). On persistence in mutual fund performance. *Journal of Finance*, 52(1), 57-82.

4. Fama, E. F., & MacBeth, J. D. (1973). Risk, return, and equilibrium: Empirical tests. *Journal of Political Economy*, 81(3), 607-636.

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao（老子道）· Dao Quant Research**
