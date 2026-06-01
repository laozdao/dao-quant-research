---
title: "Markowitz均值-方差模型Python实战指南"
description: "基于Python实现Markowitz均值-方差优化模型的完整教程，涵盖有效前沿构建、最优投资组合求解、风险分析与可视化"
author: "laozdao"
date: "2026-06-01"
category: "R05"
tags:
  - "Markowitz"
  - "Python"
  - "均值-方差优化"
  - "有效前沿"
  - "投资组合"
  - "量化投资"
  - "实践教程"
series: "研究方法论"
series_order: 12
series_title: "经典文献系列"
version: "1.0"
math: true
summary: "本文是Markowitz均值-方差模型的Python实战教程，详细介绍如何使用Python实现投资组合优化分析，包括数据获取、协方差矩阵估计、有效前沿构建、最优投资组合求解、风险贡献分析与可视化。"
---

# Markowitz均值-方差模型Python实战指南

## 摘要

本文是Markowitz均值-方差模型的Python实战教程，系统介绍如何利用Python实现投资组合优化分析的完整流程。文章涵盖数据获取与预处理、协方差矩阵估计（含收缩估计改进）、有效前沿构建、全局最小方差组合与切点组合求解、风险贡献分析、蒙特卡洛模拟以及回测框架等核心内容。通过详细的代码示例和A股市场实例，帮助读者将Markowitz理论快速应用于量化投资实践。

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

# 优化求解
from scipy.optimize import minimize

# 统计分析
import scipy.stats as stats

# 金融数据获取
import akshare as ak

# 设置中文显示
plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS']
plt.rcParams['axes.unicode_minus'] = False

# 设置显示选项
pd.set_option('display.max_columns', None)
pd.set_option('display.width', None)
```

### 1.2 获取股票市场数据

```python
def get_stock_data(stock_codes, start_date, end_date):
    """
    获取多只股票的日线行情数据

    Parameters:
    -----------
    stock_codes : list
        股票代码列表，如 ['000001', '600036', '601318']
    start_date : str
        开始日期，格式"YYYYMMDD"
    end_date : str
        结束日期，格式"YYYYMMDD"

    Returns:
    --------
    pd.DataFrame
        包含收盘价的DataFrame，列为股票代码，索引为日期
    """
    price_dict = {}

    for code in stock_codes:
        try:
            df = ak.stock_zh_a_hist(
                symbol=code,
                period="daily",
                start_date=start_date,
                end_date=end_date,
                adjust="qfq"  # 前复权
            )
            price_dict[code] = df.set_index('日期')['收盘'].sort_index()
        except Exception as e:
            print(f"获取 {code} 数据失败: {e}")

    prices = pd.DataFrame(price_dict)
    prices = prices.dropna(axis=1, how='all')
    prices = prices.dropna()  # 删除含缺失值的行

    return prices


def compute_returns(prices):
    """
    计算日对数收益率

    Parameters:
    -----------
    prices : pd.DataFrame
        收盘价数据

    Returns:
    --------
    pd.DataFrame
        日对数收益率
    """
    returns = np.log(prices / prices.shift(1))
    return returns.dropna()
```

### 1.3 示例数据获取

```python
# A股蓝筹股组合
stock_pool = {
    '000001': '平安银行',
    '600036': '招商银行',
    '601318': '中国平安',
    '600519': '贵州茅台',
    '000858': '五粮液',
    '600276': '恒瑞医药',
    '000333': '美的集团',
    '600887': '伊利股份',
    '601888': '中国中免',
    '300750': '宁德时代',
    '002594': '比亚迪',
    '601012': '隆基绿能',
}

# 获取近3年数据
start_date = "20230101"
end_date = "20251231"

stock_codes = list(stock_pool.keys())
prices = get_stock_data(stock_codes, start_date, end_date)
returns = compute_returns(prices)

print(f"数据维度: {returns.shape}")
print(f"股票数量: {len(stock_pool)}")
print(f"交易日数量: {len(returns)}")
print(f"年化收益率统计:\n{returns.mean() * 252}")
```

---

## 二、参数估计

### 2.1 预期收益率估计

```python
def estimate_expected_returns(returns, method='historical', window=None):
    """
    估计预期收益率

    Parameters:
    -----------
    returns : pd.DataFrame
        日收益率数据
    method : str
        估计方法: 'historical'(历史均值), 'exponential'(指数加权)
    window : int or None
        滚动窗口大小

    Returns:
    --------
    pd.Series
        年化预期收益率
    """
    if method == 'historical':
        if window:
            mu = returns.tail(window).mean() * 252
        else:
            mu = returns.mean() * 252
    elif method == 'exponential':
        # 指数加权移动平均，衰减因子0.94
        ewm_returns = returns.ewm(span=60).mean()
        mu = ewm_returns.iloc[-1] * 252
    else:
        raise ValueError(f"未知方法: {method}")

    return mu


# 估计预期收益率
mu_hist = estimate_expected_returns(returns, method='historical')
mu_ewm = estimate_expected_returns(returns, method='exponential')

print("历史均值法预期收益率:")
print(mu_hist.sort_values(ascending=False).head(5))
```

### 2.2 协方差矩阵估计

```python
def estimate_covariance(returns, method='sample', **kwargs):
    """
    估计协方差矩阵

    Parameters:
    -----------
    returns : pd.DataFrame
        日收益率数据
    method : str
        估计方法: 'sample'(样本), 'ledoit_wolf'(收缩), 'ewma'(指数加权)
    **kwargs : dict
        额外参数

    Returns:
    --------
    np.ndarray
        年化协方差矩阵
    """
    if method == 'sample':
        cov = returns.cov() * 252
    elif method == 'ledoit_wolf':
        # Ledoit-Wolf收缩估计
        from sklearn.covariance import LedoitWolf
        lw = LedoitWolf()
        lw.fit(returns)
        cov = pd.DataFrame(
            lw.covariance_ * 252,
            index=returns.columns,
            columns=returns.columns
        )
    elif method == 'ewma':
        # 指数加权协方差矩阵
        cov_ewma = returns.ewm(span=120).cov()
        # 取最后一个时间点的协方差矩阵
        last_date = returns.index[-1]
        cov = cov_ewma.loc[last_date] * 252
    else:
        raise ValueError(f"未知方法: {method}")

    return cov


# 估计协方差矩阵
cov_sample = estimate_covariance(returns, method='sample')
cov_lw = estimate_covariance(returns, method='ledoit_wolf')

print("样本协方差矩阵条件数:", np.linalg.cond(cov_sample))
print("Ledoit-Wolf协方差矩阵条件数:", np.linalg.cond(cov_lw))
```

### 2.3 相关性分析

```python
def analyze_correlation(returns):
    """
    分析资产间相关性

    Parameters:
    -----------
    returns : pd.DataFrame
        日收益率数据

    Returns:
    --------
    dict
        相关性统计信息
    """
    corr = returns.corr()

    # 提取上三角（不含对角线）
    mask = np.triu(np.ones_like(corr, dtype=bool), k=1)
    upper_tri = corr.where(mask)

    stats_dict = {
        '平均相关系数': upper_tri.stack().mean(),
        '中位数相关系数': upper_tri.stack().median(),
        '最大相关系数': upper_tri.stack().max(),
        '最小相关系数': upper_tri.stack().min(),
        '正相关性对数': (upper_tri.stack() > 0).sum(),
        '负相关性对数': (upper_tri.stack() < 0).sum(),
    }

    return stats_dict, corr


# 分析相关性
corr_stats, corr_matrix = analyze_correlation(returns)
print("相关性统计:")
for k, v in corr_stats.items():
    print(f"  {k}: {v:.4f}")

# 可视化相关性热力图
fig, ax = plt.subplots(figsize=(12, 10))
labels = [stock_pool.get(c, c) for c in corr_matrix.columns]
sns.heatmap(corr_matrix, annot=True, fmt='.2f', cmap='RdYlGn_r',
            center=0, xticklabels=labels, yticklabels=labels,
            ax=ax, vmin=-1, vmax=1)
ax.set_title('资产收益率相关系数矩阵', fontsize=14)
plt.tight_layout()
plt.savefig('correlation_heatmap.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## 三、有效前沿构建

### 3.1 投资组合统计量计算

```python
def portfolio_performance(weights, mu, cov):
    """
    计算投资组合的预期收益和风险

    Parameters:
    -----------
    weights : np.ndarray
        投资组合权重
    mu : pd.Series or np.ndarray
        预期收益率
    cov : pd.DataFrame or np.ndarray
        协方差矩阵

    Returns:
    --------
    tuple
        (年化收益率, 年化波动率, 夏普比率)
    """
    mu = np.asarray(mu)
    cov = np.asarray(cov)
    weights = np.asarray(weights)

    port_return = np.dot(weights, mu)
    port_volatility = np.sqrt(np.dot(weights.T, np.dot(cov, weights)))

    return port_return, port_volatility


def neg_sharpe_ratio(weights, mu, cov, rf=0.03):
    """
    负夏普比率（用于最小化）

    Parameters:
    -----------
    weights : np.ndarray
        投资组合权重
    mu : np.ndarray
        预期收益率
    cov : np.ndarray
        协方差矩阵
    rf : float
        无风险利率

    Returns:
    --------
    float
        负夏普比率
    """
    ret, vol = portfolio_performance(weights, mu, cov)
    return -(ret - rf) / vol
```

### 3.2 全局最小方差组合

```python
def find_min_variance_portfolio(mu, cov, n_assets):
    """
    求解全局最小方差组合

    Parameters:
    -----------
    mu : np.ndarray
        预期收益率
    cov : np.ndarray
        协方差矩阵
    n_assets : int
        资产数量

    Returns:
    --------
    dict
        最小方差组合信息
    """
    # 初始猜测：等权重
    init_weights = np.ones(n_assets) / n_assets

    # 约束条件：权重之和为1
    constraints = ({'type': 'eq', 'fun': lambda w: np.sum(w) - 1})

    # 不允许卖空
    bounds = tuple((0, 1) for _ in range(n_assets))

    # 优化目标：最小化方差
    def portfolio_variance(w):
        return w.T @ cov @ w

    result = minimize(
        portfolio_variance,
        init_weights,
        method='SLSQP',
        bounds=bounds,
        constraints=constraints,
        options={'ftol': 1e-12, 'maxiter': 1000}
    )

    if result.success:
        weights = result.x
        ret, vol = portfolio_performance(weights, mu, cov)
        return {
            'weights': weights,
            'return': ret,
            'volatility': vol,
            'sharpe': (ret - 0.03) / vol,
            'success': True
        }
    else:
        print(f"优化失败: {result.message}")
        return {'success': False}


# 求解最小方差组合
mu_arr = mu_hist.values
cov_arr = cov_lw.values
n = len(stock_pool)

mvp = find_min_variance_portfolio(mu_arr, cov_arr, n)
if mvp['success']:
    print("全局最小方差组合:")
    print(f"  年化收益率: {mvp['return']:.2%}")
    print(f"  年化波动率: {mvp['volatility']:.2%}")
    print(f"  夏普比率: {mvp['sharpe']:.4f}")

    # 显示主要持仓
    w_df = pd.DataFrame({
        '股票': list(stock_pool.values()),
        '代码': list(stock_pool.keys()),
        '权重': mvp['weights']
    }).sort_values('权重', ascending=False)
    print("\n持仓权重 (>1%):")
    print(w_df[w_df['权重'] > 0.01].to_string(index=False))
```

### 3.3 切点投资组合

```python
def find_tangency_portfolio(mu, cov, n_assets, rf=0.03):
    """
    求解切点投资组合（最大夏普比率）

    Parameters:
    -----------
    mu : np.ndarray
        预期收益率
    cov : np.ndarray
        协方差矩阵
    n_assets : int
        资产数量
    rf : float
        无风险利率

    Returns:
    --------
    dict
        切点组合信息
    """
    init_weights = np.ones(n_assets) / n_assets

    constraints = ({'type': 'eq', 'fun': lambda w: np.sum(w) - 1})
    bounds = tuple((0, 1) for _ in range(n_assets))

    result = minimize(
        neg_sharpe_ratio,
        init_weights,
        args=(mu, cov, rf),
        method='SLSQP',
        bounds=bounds,
        constraints=constraints,
        options={'ftol': 1e-12, 'maxiter': 1000}
    )

    if result.success:
        weights = result.x
        ret, vol = portfolio_performance(weights, mu, cov)
        return {
            'weights': weights,
            'return': ret,
            'volatility': vol,
            'sharpe': (ret - rf) / vol,
            'success': True
        }
    else:
        print(f"优化失败: {result.message}")
        return {'success': False}


# 求解切点组合
tangency = find_tangency_portfolio(mu_arr, cov_arr, n, rf=0.03)
if tangency['success']:
    print("切点投资组合:")
    print(f"  年化收益率: {tangency['return']:.2%}")
    print(f"  年化波动率: {tangency['volatility']:.2%}")
    print(f"  夏普比率: {tangency['sharpe']:.4f}")
```

### 3.4 有效前沿曲线

```python
def compute_efficient_frontier(mu, cov, n_assets, n_points=100,
                                 rf=0.03, min_ret=None, max_ret=None):
    """
    计算有效前沿上的投资组合

    Parameters:
    -----------
    mu : np.ndarray
        预期收益率
    cov : np.ndarray
        协方差矩阵
    n_assets : int
        资产数量
    n_points : int
        采样点数
    rf : float
        无风险利率
    min_ret : float or None
        最小目标收益率
    max_ret : float or None
        最大目标收益率

    Returns:
    --------
    dict
        有效前沿数据
    """
    if min_ret is None:
        min_ret = mu.min()
    if max_ret is None:
        max_ret = mu.max()

    target_returns = np.linspace(min_ret, max_ret, n_points)
    frontier_volatilities = []
    frontier_weights = []

    for target in target_returns:
        init_weights = np.ones(n_assets) / n_assets

        constraints = (
            {'type': 'eq', 'fun': lambda w: np.sum(w) - 1},
            {'type': 'eq', 'fun': lambda w, t=target: np.dot(w, mu) - t}
        )
        bounds = tuple((0, 1) for _ in range(n_assets))

        result = minimize(
            lambda w: w.T @ cov @ w,
            init_weights,
            method='SLSQP',
            bounds=bounds,
            constraints=constraints,
            options={'ftol': 1e-12, 'maxiter': 1000}
        )

        if result.success:
            vol = np.sqrt(result.x.T @ cov @ result.x)
            frontier_volatilities.append(vol)
            frontier_weights.append(result.x)
        else:
            frontier_volatilities.append(np.nan)
            frontier_weights.append(None)

    return {
        'returns': target_returns,
        'volatilities': np.array(frontier_volatilities),
        'weights': frontier_weights
    }


# 计算有效前沿
frontier = compute_efficient_frontier(mu_arr, cov_arr, n, n_points=200)
```

### 3.5 可视化有效前沿

```python
def plot_efficient_frontier(frontier, mvp, tangency, returns, stock_pool, rf=0.03):
    """
    可视化有效前沿

    Parameters:
    -----------
    frontier : dict
        有效前沿数据
    mvp : dict
        最小方差组合
    tangency : dict
        切点组合
    returns : pd.DataFrame
        收益率数据
    stock_pool : dict
        股票代码与名称映射
    rf : float
        无风险利率
    """
    fig, ax = plt.subplots(figsize=(14, 9))

    # 绘制有效前沿
    valid = ~np.isnan(frontier['volatilities'])
    ax.plot(frontier['volatilities'][valid], frontier['returns'][valid],
            'b-', linewidth=2, label='Efficient Frontier')

    # 绘制最小方差组合
    ax.scatter(mvp['volatility'], mvp['return'], c='red', s=200,
                marker='*', zorder=5, label=f'MVP (SR={mvp["sharpe"]:.2f})')

    # 绘制切点组合
    ax.scatter(tangency['volatility'], tangency['return'], c='gold',
                s=200, marker='D', zorder=5, edgecolors='black',
                label=f'Tangency (SR={tangency["sharpe"]:.2f})')

    # 绘制资本市场线
    cml_x = np.linspace(0, max(frontier['volatilities'][valid]) * 1.2, 100)
    cml_y = rf + tangency['sharpe'] * cml_x
    ax.plot(cml_x, cml_y, 'g--', linewidth=1.5,
            label=f'CML (rf={rf:.1%})')

    # 绘制无风险资产
    ax.scatter(0, rf, c='green', s=150, marker='o', zorder=5,
               label='Risk-Free Asset')

    # 绘制各资产
    for i, code in enumerate(returns.columns):
        ret_i = returns[code].mean() * 252
        vol_i = returns[code].std() * np.sqrt(252)
        ax.scatter(vol_i, ret_i, c='gray', s=60, alpha=0.6, zorder=3)
        ax.annotate(stock_pool.get(code, code), (vol_i, ret_i),
                    fontsize=8, alpha=0.7,
                    xytext=(5, 5), textcoords='offset points')

    # 绘制随机投资组合
    np.random.seed(42)
    n_random = 5000
    random_weights = np.random.dirichlet(np.ones(n), n_random)
    random_returns = random_weights @ mu_arr
    random_vols = np.sqrt(np.sum(random_weights * (cov_arr @ random_weights.T).T, axis=1))
    ax.scatter(random_vols, random_returns, c='lightblue', s=1, alpha=0.3)

    ax.set_xlabel('Annualized Volatility (σ)', fontsize=12)
    ax.set_ylabel('Annualized Return (μ)', fontsize=12)
    ax.set_title('Markowitz Efficient Frontier', fontsize=14)
    ax.legend(loc='upper left', fontsize=10)
    ax.grid(True, alpha=0.3)
    ax.set_xlim(0, None)
    ax.set_ylim(0, None)

    plt.tight_layout()
    plt.savefig('efficient_frontier.png', dpi=150, bbox_inches='tight')
    plt.show()


# 绘制有效前沿
plot_efficient_frontier(frontier, mvp, tangency, returns, stock_pool)
```

---

## 四、风险贡献分析

### 4.1 边际风险贡献与风险贡献

```python
def risk_contribution(weights, cov):
    """
    计算各资产的风险贡献

    Parameters:
    -----------
    weights : np.ndarray
        投资组合权重
    cov : np.ndarray
        协方差矩阵

    Returns:
    --------
    dict
        风险贡献分析结果
    """
    weights = np.asarray(weights)
    cov = np.asarray(cov)

    # 组合方差
    port_var = weights @ cov @ weights
    port_vol = np.sqrt(port_var)

    # 边际风险贡献 (MRC): ∂σ_p/∂w_i
    mrc = (cov @ weights) / port_vol

    # 风险贡献 (RC): w_i × MRC_i
    rc = weights * mrc

    # 百分比风险贡献 (PRC)
    prc = rc / port_vol

    return {
        'marginal_risk': mrc,
        'risk_contribution': rc,
        'pct_risk_contribution': prc,
        'portfolio_volatility': port_vol
    }


# 分析切点组合的风险贡献
rc_result = risk_contribution(tangency['weights'], cov_arr)

rc_df = pd.DataFrame({
    '股票': list(stock_pool.values()),
    '代码': list(stock_pool.keys()),
    '权重': tangency['weights'],
    '边际风险贡献': rc_result['marginal_risk'],
    '风险贡献': rc_result['risk_contribution'],
    '风险贡献占比': rc_result['pct_risk_contribution']
}).sort_values('风险贡献占比', ascending=False)

print("切点组合风险贡献分析:")
print(rc_df[rc_df['权重'] > 0.01].to_string(index=False))
```

### 4.2 风险贡献可视化

```python
def plot_risk_contribution(weights, cov, stock_pool, title='Risk Contribution'):
    """
    可视化风险贡献
    """
    rc_result = risk_contribution(weights, cov)

    # 筛选权重>0的资产
    mask = weights > 0.005
    names = [stock_pool[c] for c in stock_pool.keys() if mask[list(stock_pool.keys()).index(c)]]
    prc = rc_result['pct_risk_contribution'][mask]
    w = weights[mask]

    fig, axes = plt.subplots(1, 2, figsize=(16, 7))

    # 权重饼图
    axes[0].pie(w, labels=names, autopct='%1.1f%%', startangle=90)
    axes[0].set_title('Portfolio Weights', fontsize=12)

    # 风险贡献饼图
    axes[1].pie(prc, labels=names, autopct='%1.1f%%', startangle=90)
    axes[1].set_title('Risk Contribution', fontsize=12)

    plt.suptitle(title, fontsize=14)
    plt.tight_layout()
    plt.savefig('risk_contribution.png', dpi=150, bbox_inches='tight')
    plt.show()


plot_risk_contribution(tangency['weights'], cov_arr, stock_pool,
                       title='Tangency Portfolio Risk Contribution')
```

---

## 五、蒙特卡洛模拟

### 5.1 随机投资组合模拟

```python
def monte_carlo_portfolios(mu, cov, n_assets, n_simulations=10000):
    """
    蒙特卡洛模拟随机投资组合

    Parameters:
    -----------
    mu : np.ndarray
        预期收益率
    cov : np.ndarray
        协方差矩阵
    n_assets : int
        资产数量
    n_simulations : int
        模拟次数

    Returns:
    --------
    pd.DataFrame
        模拟结果
    """
    np.random.seed(42)

    results = []
    for _ in range(n_simulations):
        # 生成随机权重（Dirichlet分布确保权重之和为1且非负）
        weights = np.random.dirichlet(np.ones(n_assets))

        ret = np.dot(weights, mu)
        vol = np.sqrt(np.dot(weights.T, np.dot(cov, weights)))
        sharpe = (ret - 0.03) / vol

        results.append({
            'return': ret,
            'volatility': vol,
            'sharpe': sharpe,
            'weights': weights
        })

    return pd.DataFrame(results)


# 蒙特卡洛模拟
mc_results = monte_carlo_portfolios(mu_arr, cov_arr, n, n_simulations=10000)

print("蒙特卡洛模拟结果统计:")
print(f"  最大夏普比率: {mc_results['sharpe'].max():.4f}")
print(f"  最小波动率: {mc_results['volatility'].min():.2%}")
print(f"  收益率范围: [{mc_results['return'].min():.2%}, {mc_results['return'].max():.2%}]")
```

### 5.2 蒙特卡洛可视化

```python
def plot_monte_carlo(mc_results, mvp, tangency, rf=0.03):
    """
    可视化蒙特卡洛模拟结果
    """
    fig, ax = plt.subplots(figsize=(14, 9))

    # 按夏普比率着色
    scatter = ax.scatter(
        mc_results['volatility'],
        mc_results['return'],
        c=mc_results['sharpe'],
        cmap='RdYlGn',
        alpha=0.5,
        s=5
    )

    # 标记特殊组合
    ax.scatter(mvp['volatility'], mvp['return'], c='red', s=200,
                marker='*', zorder=5, label='MVP')
    ax.scatter(tangency['volatility'], tangency['return'], c='gold',
                s=200, marker='D', zorder=5, edgecolors='black',
                label='Tangency')

    plt.colorbar(scatter, label='Sharpe Ratio', ax=ax)
    ax.set_xlabel('Annualized Volatility', fontsize=12)
    ax.set_ylabel('Annualized Return', fontsize=12)
    ax.set_title('Monte Carlo Portfolio Simulation', fontsize=14)
    ax.legend()
    ax.grid(True, alpha=0.3)

    plt.tight_layout()
    plt.savefig('monte_carlo.png', dpi=150, bbox_inches='tight')
    plt.show()


plot_monte_carlo(mc_results, mvp, tangency)
```

---

## 六、带约束的投资组合优化

### 6.1 通用优化框架

```python
class PortfolioOptimizer:
    """
    投资组合优化器

    支持多种优化目标和约束条件
    """

    def __init__(self, mu, cov, rf=0.03):
        """
        Parameters:
        -----------
        mu : np.ndarray
            预期收益率
        cov : np.ndarray
            协方差矩阵
        rf : float
            无风险利率
        """
        self.mu = np.asarray(mu)
        self.cov = np.asarray(cov)
        self.rf = rf
        self.n_assets = len(mu)

    def minimize_variance(self, target_return=None,
                            weight_bounds=(0, 1),
                            max_weight=None):
        """最小化方差"""
        init_w = np.ones(self.n_assets) / self.n_assets

        constraints = [{'type': 'eq', 'fun': lambda w: np.sum(w) - 1}]
        if target_return is not None:
            constraints.append({
                'type': 'eq',
                'fun': lambda w, t=target_return: np.dot(w, self.mu) - t
            })

        bounds = [weight_bounds] * self.n_assets
        if max_weight is not None:
            bounds = [(0, max_weight)] * self.n_assets

        result = minimize(
            lambda w: w @ self.cov @ w,
            init_w, method='SLSQP',
            bounds=bounds, constraints=constraints,
            options={'ftol': 1e-12, 'maxiter': 1000}
        )

        return self._parse_result(result)

    def maximize_sharpe(self, weight_bounds=(0, 1), max_weight=None):
        """最大化夏普比率"""
        init_w = np.ones(self.n_assets) / self.n_assets

        constraints = [{'type': 'eq', 'fun': lambda w: np.sum(w) - 1}]
        bounds = [weight_bounds] * self.n_assets
        if max_weight is not None:
            bounds = [(0, max_weight)] * self.n_assets

        result = minimize(
            lambda w: -(w @ self.mu - self.rf) / np.sqrt(w @ self.cov @ w),
            init_w, method='SLSQP',
            bounds=bounds, constraints=constraints,
            options={'ftol': 1e-12, 'maxiter': 1000}
        )

        return self._parse_result(result)

    def minimize_cvar(self, alpha=0.05, n_sim=10000):
        """最小化CVaR（条件风险价值）"""
        init_w = np.ones(self.n_assets) / self.n_assets
        constraints = [{'type': 'eq', 'fun': lambda w: np.sum(w) - 1}]
        bounds = [(0, 1)] * self.n_assets

        def cvar_objective(w):
            # 模拟组合收益
            port_returns = np.random.multivariate_normal(self.mu, self.cov, n_sim) @ w
            var = np.percentile(port_returns, 100 * alpha)
            cvar = -port_returns[port_returns <= var].mean()
            return cvar

        result = minimize(cvar_objective, init_w, method='SLSQP',
                          bounds=bounds, constraints=constraints,
                          options={'ftol': 1e-8, 'maxiter': 500})

        return self._parse_result(result)

    def _parse_result(self, result):
        """解析优化结果"""
        if result.success:
            w = result.x
            ret = w @ self.mu
            vol = np.sqrt(w @ self.cov @ w)
            return {
                'weights': w,
                'return': ret,
                'volatility': vol,
                'sharpe': (ret - self.rf) / vol,
                'success': True
            }
        return {'success': False, 'message': result.message}
```

### 6.2 不同约束条件下的优化结果

```python
# 创建优化器
optimizer = PortfolioOptimizer(mu_arr, cov_arr, rf=0.03)

# 1. 无约束（仅不允许卖空）
opt1 = optimizer.minimize_variance()
print("无约束最小方差组合:")
print(f"  波动率: {opt1['volatility']:.2%}, 夏普: {opt1['sharpe']:.4f}")

# 2. 单只股票最大权重10%
opt2 = optimizer.minimize_variance(max_weight=0.10)
print("\n最大权重10%约束:")
print(f"  波动率: {opt2['volatility']:.2%}, 夏普: {opt2['sharpe']:.4f}")

# 3. 最大夏普比率（最大权重15%）
opt3 = optimizer.maximize_sharpe(max_weight=0.15)
print("\n最大夏普比率（最大权重15%）:")
print(f"  收益率: {opt3['return']:.2%}, 波动率: {opt3['volatility']:.2%}")
print(f"  夏普比率: {opt3['sharpe']:.4f}")

# 4. 最小化CVaR
opt4 = optimizer.minimize_cvar(alpha=0.05)
print("\n最小CVaR组合:")
print(f"  收益率: {opt4['return']:.2%}, 波动率: {opt4['volatility']:.2%}")
```

### 6.3 不同优化结果对比

```python
def compare_portfolios(optimizers_results, stock_pool):
    """
    对比不同优化策略的结果
    """
    comparison = []
    for name, result in optimizers_results.items():
        if result['success']:
            # 有效持仓数量
            n_holdings = np.sum(result['weights'] > 0.01)
            # 最大权重
            max_w = result['weights'].max()
            # HHI指数（集中度）
            hhi = np.sum(result['weights'] ** 2)

            comparison.append({
                '策略': name,
                '年化收益率': result['return'],
                '年化波动率': result['volatility'],
                '夏普比率': result['sharpe'],
                '持仓数量': n_holdings,
                '最大权重': max_w,
                'HHI指数': hhi
            })

    return pd.DataFrame(comparison)


# 对比不同策略
results_dict = {
    'MVP': mvp,
    'Tangency': tangency,
    'MaxWeight10%': opt2,
    'MaxSharpe15%': opt3,
}

comparison_df = compare_portfolios(results_dict, stock_pool)
print("投资组合策略对比:")
print(comparison_df.to_string(index=False))
```

---

## 七、回测框架

### 7.1 滚动窗口回测

```python
def rolling_backtest(returns, rebalance_freq=60, lookback=252,
                     method='min_variance', rf=0.03):
    """
    滚动窗口回测

    Parameters:
    -----------
    returns : pd.DataFrame
        日收益率数据
    rebalance_freq : int
        再平衡频率（交易日）
    lookback : int
        回看窗口（交易日）
    method : str
        优化方法: 'min_variance', 'max_sharpe', 'equal_weight'
    rf : float
        无风险利率

    Returns:
    --------
    pd.DataFrame
        回测结果
    """
    n_days = len(returns)
    portfolio_returns = []
    weights_history = []

    for i in range(lookback, n_days):
        # 每rebalance_freq天再平衡一次
        if (i - lookback) % rebalance_freq != 0 and len(portfolio_returns) > 0:
            # 使用上期权重
            daily_return = returns.iloc[i] @ weights_history[-1]
            portfolio_returns.append(daily_return)
            continue

        # 估计参数
        window_returns = returns.iloc[i - lookback:i]
        mu_est = window_returns.mean().values * 252
        cov_est = window_returns.cov().values * 252

        # 优化
        opt = PortfolioOptimizer(mu_est, cov_est, rf=rf)

        if method == 'min_variance':
            result = opt.minimize_variance(max_weight=0.15)
        elif method == 'max_sharpe':
            result = opt.maximize_sharpe(max_weight=0.15)
        elif method == 'equal_weight':
            n = len(mu_est)
            result = {
                'weights': np.ones(n) / n,
                'success': True
            }
        else:
            raise ValueError(f"未知方法: {method}")

        if result['success']:
            w = result['weights']
            weights_history.append(w)
            daily_return = returns.iloc[i] @ w
            portfolio_returns.append(daily_return)
        else:
            # 优化失败时使用等权重
            n = len(mu_est)
            w = np.ones(n) / n
            weights_history.append(w)
            daily_return = returns.iloc[i] @ w
            portfolio_returns.append(daily_return)

    # 构建结果DataFrame
    result_df = pd.DataFrame({
        'portfolio_return': portfolio_returns
    }, index=returns.index[lookback:])

    # 计算累计收益
    result_df['cumulative_return'] = (1 + result_df['portfolio_return']).cumprod()

    # 计算回测指标
    total_return = result_df['cumulative_return'].iloc[-1] - 1
    annual_return = (1 + total_return) ** (252 / len(result_df)) - 1
    annual_vol = result_df['portfolio_return'].std() * np.sqrt(252)
    sharpe = (annual_return - rf) / annual_vol
    max_drawdown = (result_df['cumulative_return'] /
                    result_df['cumulative_return'].cummax() - 1).min()

    print(f"\n{method} 回测结果:")
    print(f"  总收益率: {total_return:.2%}")
    print(f"  年化收益率: {annual_return:.2%}")
    print(f"  年化波动率: {annual_vol:.2%}")
    print(f"  夏普比率: {sharpe:.4f}")
    print(f"  最大回撤: {max_drawdown:.2%}")

    return result_df


# 执行回测
bt_mv = rolling_backtest(returns, method='min_variance')
bt_ms = rolling_backtest(returns, method='max_sharpe')
bt_ew = rolling_backtest(returns, method='equal_weight')
```

### 7.2 回测结果可视化

```python
def plot_backtest_results(results_dict, benchmark=None):
    """
    可视化回测结果
    """
    fig, axes = plt.subplots(2, 2, figsize=(16, 12))

    # 累计收益曲线
    ax1 = axes[0, 0]
    for name, df in results_dict.items():
        ax1.plot(df.index, df['cumulative_return'], label=name, linewidth=1.5)
    if benchmark is not None:
        ax1.plot(benchmark.index, benchmark['cumulative_return'],
                 label='Benchmark', linewidth=1.5, linestyle='--')
    ax1.set_title('Cumulative Returns', fontsize=12)
    ax1.set_xlabel('Date')
    ax1.set_ylabel('Cumulative Return')
    ax1.legend()
    ax1.grid(True, alpha=0.3)

    # 滚动波动率
    ax2 = axes[0, 1]
    for name, df in results_dict.items():
        rolling_vol = df['portfolio_return'].rolling(60).std() * np.sqrt(252)
        ax2.plot(rolling_vol.index, rolling_vol, label=name, linewidth=1, alpha=0.8)
    ax2.set_title('60-Day Rolling Volatility', fontsize=12)
    ax2.set_xlabel('Date')
    ax2.set_ylabel('Annualized Volatility')
    ax2.legend()
    ax2.grid(True, alpha=0.3)

    # 收益分布
    ax3 = axes[1, 0]
    for name, df in results_dict.items():
        ax3.hist(df['portfolio_return'], bins=50, alpha=0.5, label=name, density=True)
    ax3.set_title('Return Distribution', fontsize=12)
    ax3.set_xlabel('Daily Return')
    ax3.set_ylabel('Density')
    ax3.legend()
    ax3.grid(True, alpha=0.3)

    # 回撤曲线
    ax4 = axes[1, 1]
    for name, df in results_dict.items():
        drawdown = df['cumulative_return'] / df['cumulative_return'].cummax() - 1
        ax4.fill_between(drawdown.index, drawdown, alpha=0.3, label=name)
        ax4.plot(drawdown.index, drawdown, linewidth=0.5, alpha=0.5)
    ax4.set_title('Drawdown', fontsize=12)
    ax4.set_xlabel('Date')
    ax4.set_ylabel('Drawdown')
    ax4.legend()
    ax4.grid(True, alpha=0.3)

    plt.tight_layout()
    plt.savefig('backtest_results.png', dpi=150, bbox_inches='tight')
    plt.show()


# 绘制回测结果
plot_backtest_results({
    'Min Variance': bt_mv,
    'Max Sharpe': bt_ms,
    'Equal Weight': bt_ew
})
```

---

## 八、协方差矩阵改进方法

### 8.1 Ledoit-Wolf收缩估计

```python
def ledoit_wolf_shrinkage(returns):
    """
    Ledoit-Wolf收缩协方差矩阵估计

    Parameters:
    -----------
    returns : pd.DataFrame
        收益率数据

    Returns:
    --------
    tuple
        (收缩协方差矩阵, 最优收缩强度)
    """
    from sklearn.covariance import LedoitWolf

    lw = LedoitWolf()
    lw.fit(returns)

    print(f"最优收缩强度: {lw.shrinkage_:.4f}")
    print(f"样本协方差矩阵条件数: {np.linalg.cond(returns.cov()):.2f}")
    print(f"收缩协方差矩阵条件数: {np.linalg.cond(lw.covariance_):.2f}")

    return lw.covariance_ * 252, lw.shrinkage_


# 应用收缩估计
cov_lw, shrinkage = ledoit_wolf_shrinkage(returns)
```

### 8.2 因子模型协方差估计

```python
def factor_model_covariance(returns, n_factors=5):
    """
    基于统计因子模型的协方差矩阵估计

    Parameters:
    -----------
    returns : pd.DataFrame
        收益率数据
    n_factors : int
        因子数量

    Returns:
    --------
    np.ndarray
        估计的协方差矩阵
    """
    from sklearn.decomposition import PCA
    from sklearn.preprocessing import StandardScaler

    # 标准化
    scaler = StandardScaler()
    scaled_returns = scaler.fit_transform(returns)

    # PCA提取因子
    pca = PCA(n_components=n_factors)
    factors = pca.fit_transform(scaled_returns)

    # 因子载荷
    loadings = pca.components_.T * np.sqrt(pca.explained_variance_)

    # 因子协方差
    factor_cov = np.cov(factors.T) * 252

    # 特质方差
    residuals = scaled_returns - factors @ pca.components_
    diag_residual = np.diag(np.var(residuals, axis=0, ddof=1)) * 252

    # 总协方差
    total_cov = loadings @ factor_cov @ loadings.T + diag_residual

    print(f"因子解释方差比: {pca.explained_variance_ratio_.sum():.2%}")
    print(f"条件数: {np.linalg.cond(total_cov):.2f}")

    return total_cov


# 因子模型协方差估计
cov_factor = factor_model_covariance(returns, n_factors=5)
```

### 8.3 不同协方差估计方法对比

```python
def compare_covariance_methods(returns, stock_pool):
    """
    对比不同协方差矩阵估计方法
    """
    methods = {
        'Sample': estimate_covariance(returns, method='sample'),
        'Ledoit-Wolf': estimate_covariance(returns, method='ledoit_wolf'),
        'Factor Model': pd.DataFrame(
            factor_model_covariance(returns, n_factors=5),
            index=returns.columns, columns=returns.columns
        ),
    }

    print("协方差矩阵估计方法对比:")
    print("-" * 60)
    for name, cov in methods.items():
        cond = np.linalg.cond(cov)
        det_sign = '正定' if np.all(np.linalg.eigvals(cov) > 0) else '非正定'
        print(f"{name:15s}: 条件数={cond:10.2f}, {det_sign}")

    return methods


compare_covariance_methods(returns, stock_pool)
```

---

## 九、完整实战案例

### 9.1 完整投资组合优化流程

```python
def full_portfolio_optimization(stock_codes, stock_names, start_date, end_date,
                                  rf=0.03, max_weight=0.12, min_weight=0.02):
    """
    完整的投资组合优化流程

    Parameters:
    -----------
    stock_codes : list
        股票代码列表
    stock_names : list
        股票名称列表
    start_date : str
        开始日期
    end_date : str
        结束日期
    rf : float
        无风险利率
    max_weight : float
        单只股票最大权重
    min_weight : float
        单只股票最小权重

    Returns:
    --------
    dict
        优化结果
    """
    stock_pool = dict(zip(stock_codes, stock_names))

    # Step 1: 获取数据
    print("=" * 60)
    print("Step 1: 获取数据")
    prices = get_stock_data(stock_codes, start_date, end_date)
    returns = compute_returns(prices)
    print(f"  获取 {len(stock_codes)} 只股票, {len(returns)} 个交易日数据")

    # Step 2: 参数估计
    print("\nStep 2: 参数估计")
    mu = estimate_expected_returns(returns, method='historical')
    cov = estimate_covariance(returns, method='ledoit_wolf')
    print(f"  预期收益率范围: [{mu.min():.2%}, {mu.max():.2%}]")
    print(f"  协方差矩阵条件数: {np.linalg.cond(cov):.2f}")

    # Step 3: 相关性分析
    print("\nStep 3: 相关性分析")
    corr_stats, corr_matrix = analyze_correlation(returns)
    print(f"  平均相关系数: {corr_stats['平均相关系数']:.4f}")

    # Step 4: 优化求解
    print("\nStep 4: 投资组合优化")
    n = len(stock_codes)
    optimizer = PortfolioOptimizer(mu.values, cov.values, rf=rf)

    mvp = optimizer.minimize_variance(max_weight=max_weight)
    tangency = optimizer.maximize_sharpe(max_weight=max_weight)

    if mvp['success'] and tangency['success']:
        print(f"  MVP: 收益={mvp['return']:.2%}, 波动={mvp['volatility']:.2%}, "
              f"夏普={mvp['sharpe']:.4f}")
        print(f"  切点: 收益={tangency['return']:.2%}, 波动={tangency['volatility']:.2%}, "
              f"夏普={tangency['sharpe']:.4f}")

    # Step 5: 风险分析
    print("\nStep 5: 风险贡献分析")
    rc = risk_contribution(tangency['weights'], cov.values)

    # Step 6: 输出推荐组合
    print("\n" + "=" * 60)
    print("推荐投资组合（切点组合）")
    print("=" * 60)

    portfolio = pd.DataFrame({
        '股票代码': stock_codes,
        '股票名称': stock_names,
        '权重': tangency['weights'],
        '预期收益率': mu.values,
        '风险贡献占比': rc['pct_risk_contribution']
    }).sort_values('权重', ascending=False)

    portfolio = portfolio[portfolio['权重'] > 0.005]
    print(portfolio.to_string(index=False))

    return {
        'portfolio': portfolio,
        'mvp': mvp,
        'tangency': tangency,
        'mu': mu,
        'cov': cov,
        'returns': returns
    }


# 执行完整优化
result = full_portfolio_optimization(
    stock_codes=list(stock_pool.keys()),
    stock_names=list(stock_pool.values()),
    start_date="20230101",
    end_date="20251231",
    rf=0.03,
    max_weight=0.12
)
```

### 9.2 关键代码说明

| 代码模块 | 功能 | 关键函数 |
|:---------|:-----|:---------|
| 数据获取 | 从AkShare获取A股数据 | `get_stock_data()` |
| 参数估计 | 估计预期收益和协方差 | `estimate_expected_returns()`, `estimate_covariance()` |
| 有效前沿 | 计算前沿上的组合 | `compute_efficient_frontier()` |
| 最优组合 | MVP和切点组合求解 | `find_min_variance_portfolio()`, `find_tangency_portfolio()` |
| 风险分析 | 风险贡献分解 | `risk_contribution()` |
| 回测框架 | 滚动窗口回测 | `rolling_backtest()` |

---

## 十、总结与实践建议

### 10.1 核心要点

1. **协方差矩阵估计是关键**：使用收缩估计（Ledoit-Wolf）或因子模型估计，避免样本协方差矩阵的不稳定性
2. **约束条件很重要**：加入权重上下限约束，避免极端权重，提高组合的实用性
3. **参数敏感性需重视**：Markowitz模型对预期收益估计极其敏感，实际应用中需谨慎
4. **定期再平衡**：市场条件变化时需要定期更新参数和再平衡组合
5. **多种方法对比**：不要只依赖单一优化结果，对比不同方法和约束条件的结果

### 10.2 实践建议

| 建议 | 说明 |
|:-----|:-----|
| 使用收缩估计 | Ledoit-Wolf收缩可显著提高协方差矩阵的估计质量 |
| 加入约束条件 | 权重限制、行业限制等约束使组合更具可操作性 |
| 对比多种策略 | MVP、切点组合、等权重等多种策略对比选择 |
| 考虑交易成本 | 回测中加入交易成本，避免过度交易 |
| 定期再平衡 | 建议每月或每季度再平衡一次 |
| 敏感性分析 | 对关键参数进行敏感性分析，评估结果的稳健性 |

---

## 参考文献

1. Markowitz, H. (1952). Portfolio Selection. *The Journal of Finance*, 7(1), 77-91.
2. Ledoit, O. & Wolf, M. (2004). A Well-Conditioned Estimator for Large-Dimensional Covariance Matrices. *Journal of Multivariate Analysis*, 88(2), 365-411.
3. DeMiguel, V., Garlappi, L. & Uppal, R. (2009). Optimal Versus Naive Diversification. *Review of Financial Studies*, 22(5), 1915-1953.
4. Jagannathan, R. & Ma, T. (2003). Risk Reduction in Large Portfolios: Why Improving the Estimation of the Covariance Matrix is Not Enough. *Journal of Financial Economics*, 5(1), 55-80.
5. Rockafellar, R. T. & Uryasev, S. (2000). Optimization of Conditional Value-at-Risk. *Journal of Risk*, 2(3), 21-41.

---

## 免责声明

本文仅供学术研究和学习交流之用，不构成任何投资建议。投资有风险，入市需谨慎。文中提到的任何投资策略、模型和方法均需在实际投资中谨慎使用，并结合自身风险承受能力做出独立判断。作者不对因使用本文内容而造成的任何投资损失承担责任。
