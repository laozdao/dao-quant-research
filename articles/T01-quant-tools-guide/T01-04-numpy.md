# NumPy — 科学计算基础

> NumPy是Python科学计算的基石，为量化交易提供了高性能的数值运算和线性代数能力。

---

## 概述

NumPy（Numerical Python）是Python科学计算的基础库，由Travis Oliphant于2005年创建，是Python数值计算生态系统的核心。NumPy提供了强大的N维数组对象（ndarray）、广播机制、线性代数运算、傅里叶变换和随机数生成等功能。几乎所有Python科学计算库（包括pandas、scikit-learn、TensorFlow等）都建立在NumPy之上。

在量化交易领域，NumPy的重要性体现在三个方面：一是提供高性能的向量化运算，避免Python循环的性能瓶颈；二是提供丰富的数学函数，支持复杂的金融计算；三是作为数据交换的标准格式，NumPy数组是各量化库之间的通用数据接口。掌握NumPy的向量化编程思维，是编写高效量化代码的关键。

---

## 基本信息表格

| 项目 | 详情 |
|------|------|
| 库名称 | NumPy |
| 最新稳定版 | NumPy 2.0.x |
| 创建者 | Travis Oliphant |
| 首次发布 | 2005年 |
| 开源协议 | BSD-3-Clause |
| 官方网站 | https://numpy.org |
| 安装方式 | `pip install numpy` |
| 核心对象 | ndarray（N维数组） |
| 适用平台 | Windows, macOS, Linux |

---

## 核心特性

### 1. 向量化运算

向量化运算是NumPy最核心的优势，相比Python原生循环可提升数十倍到数百倍的性能：

```python
'''NumPy向量化运算示例'''

import numpy as np
import time

# 生成模拟价格数据
np.random.seed(42)
n_days = 252
n_stocks = 500

# 生成价格矩阵：500只股票 x 252个交易日
prices = np.cumprod(
    1 + np.random.normal(0.0003, 0.02, (n_stocks, n_days)),
    axis=1
) * 100

# 向量化计算收益率矩阵
returns = np.diff(prices, axis=1) / prices[:, :-1]
print(f'收益率矩阵形状: {returns.shape}')

# 向量化计算每只股票的年化收益率和波动率
annual_returns = np.prod(1 + returns, axis=1) ** (252 / returns.shape[1]) - 1
annual_volatility = returns.std(axis=1) * np.sqrt(252)
sharpe_ratio = annual_returns / annual_volatility

print(f'平均年化收益率: {annual_returns.mean():.4f}')
print(f'平均年化波动率: {annual_volatility.mean():.4f}')
print(f'平均夏普比率: {sharpe_ratio.mean():.4f}')

# 性能对比：向量化 vs 循环
prices_small = np.random.uniform(90, 110, (1000, 1000))

# 向量化方式
start = time.time()
result_vec = np.mean(prices_small, axis=1)
time_vec = time.time() - start

# 循环方式
start = time.time()
result_loop = np.array([np.mean(row) for row in prices_small])
time_loop = time.time() - start

print(f'\n向量化耗时: {time_vec:.6f}秒')
print(f'循环耗时: {time_loop:.6f}秒')
print(f'加速比: {time_loop / time_vec:.1f}倍')
```

### 2. 投资组合优化

NumPy的线性代数模块是投资组合优化计算的底层引擎：

```python
'''使用NumPy进行马科维茨投资组合优化'''

import numpy as np

# 模拟5只资产的预期收益率和协方差矩阵
n_assets = 5
expected_returns = np.array([0.12, 0.10, 0.08, 0.15, 0.11])

# 构建协方差矩阵
volatilities = np.array([0.20, 0.18, 0.15, 0.25, 0.22])
correlations = np.array([
    [1.00, 0.60, 0.30, 0.45, 0.55],
    [0.60, 1.00, 0.25, 0.40, 0.50],
    [0.30, 0.25, 1.00, 0.35, 0.30],
    [0.45, 0.40, 0.35, 1.00, 0.60],
    [0.55, 0.50, 0.30, 0.60, 1.00]
])
cov_matrix = np.outer(volatilities, volatilities) * correlations

# 最小方差投资组合（解析解）
def min_variance_portfolio(cov):
    '''计算最小方差投资组合权重
    
    参数:
        cov: 协方差矩阵 (n x n)
    返回:
        weights: 最优权重向量
    '''
    n = cov.shape[0]
    ones = np.ones(n)
    # 使用矩阵求逆求解: w = Sigma^-1 * 1 / (1^T * Sigma^-1 * 1)
    inv_cov = np.linalg.inv(cov)
    weights = inv_cov @ ones / (ones @ inv_cov @ ones)
    return weights

# 最大夏普比率投资组合
def max_sharpe_portfolio(expected_returns, cov, risk_free=0.03):
    '''计算最大夏普比率投资组合权重
    
    参数:
        expected_returns: 预期收益率向量
        cov: 协方差矩阵
        risk_free: 无风险利率
    返回:
        weights: 最优权重向量
    '''
    excess_returns = expected_returns - risk_free
    inv_cov = np.linalg.inv(cov)
    weights = inv_cov @ excess_returns / (np.ones(len(expected_returns)) @ inv_cov @ excess_returns)
    return weights

# 计算最优组合
mv_weights = min_variance_portfolio(cov_matrix)
ms_weights = max_sharpe_portfolio(expected_returns, cov_matrix)

# 计算组合统计量
def portfolio_stats(weights, expected_returns, cov_matrix):
    '''计算投资组合统计量'''
    port_return = weights @ expected_returns
    port_vol = np.sqrt(weights @ cov_matrix @ weights)
    sharpe = (port_return - 0.03) / port_vol
    return port_return, port_vol, sharpe

mv_stats = portfolio_stats(mv_weights, expected_returns, cov_matrix)
ms_stats = portfolio_stats(ms_weights, expected_returns, cov_matrix)

print('最小方差组合:')
print(f'  权重: {np.round(mv_weights, 4)}')
print(f'  预期收益: {mv_stats[0]:.4f}, 波动率: {mv_stats[1]:.4f}, 夏普: {mv_stats[2]:.4f}')
print('\n最大夏普组合:')
print(f'  权重: {np.round(ms_weights, 4)}')
print(f'  预期收益: {ms_stats[0]:.4f}, 波动率: {ms_stats[1]:.4f}, 夏普: {ms_stats[2]:.4f}')
```

### 3. 蒙特卡洛模拟

NumPy的随机数生成和向量化运算使蒙特卡洛模拟变得高效简洁：

```python
'''使用NumPy进行蒙特卡洛模拟'''

import numpy as np

def monte_carlo_option_pricing(S0, K, T, r, sigma, n_sims=100000, n_steps=252):
    '''蒙特卡洛欧式期权定价
    
    参数:
        S0: 标的资产当前价格
        K: 行权价
        T: 到期时间（年）
        r: 无风险利率
        sigma: 波动率
        n_sims: 模拟路径数
        n_steps: 时间步数
    返回:
        call_price: 看涨期权价格
        put_price: 看跌期权价格
    '''
    dt = T / n_steps
    
    # 一次性生成所有模拟路径 (n_sims x n_steps)
    # 使用几何布朗运动模型
    Z = np.random.standard_normal((n_sims, n_steps))
    drift = (r - 0.5 * sigma**2) * dt
    diffusion = sigma * np.sqrt(dt) * Z
    
    # 累积求和得到对数价格路径
    log_returns = drift + diffusion
    log_prices = np.log(S0) + np.cumsum(log_returns, axis=1)
    prices = np.exp(log_prices)
    
    # 取到期日价格
    terminal_prices = prices[:, -1]
    
    # 计算期权收益
    call_payoffs = np.maximum(terminal_prices - K, 0)
    put_payoffs = np.maximum(K - terminal_prices, 0)
    
    # 折现得到期权价格
    discount_factor = np.exp(-r * T)
    call_price = discount_factor * call_payoffs.mean()
    put_price = discount_factor * put_payoffs.mean()
    
    # 计算标准误差
    call_se = discount_factor * call_payoffs.std() / np.sqrt(n_sims)
    put_se = discount_factor * put_payoffs.std() / np.sqrt(n_sims)
    
    return call_price, put_price, call_se, put_se

# 定价示例
call, put, call_se, put_se = monte_carlo_option_pricing(
    S0=100, K=105, T=1.0, r=0.05, sigma=0.20, n_sims=100000
)

print(f'欧式看涨期权价格: {call:.4f} (标准误差: {call_se:.4f})')
print(f'欧式看跌期权价格: {put:.4f} (标准误差: {put_se:.4f})')

# VaR蒙特卡洛模拟
def monte_carlo_var(portfolio_value, returns_dist, n_sims=100000, confidence=0.95):
    '''蒙特卡洛VaR计算
    
    参数:
        portfolio_value: 投资组合当前价值
        returns_dist: 收益率分布参数 (mean, std)
        n_sims: 模拟次数
        confidence: 置信水平
    返回:
        var: 在险价值
    '''
    mu, sigma = returns_dist
    sim_returns = np.random.normal(mu, sigma, n_sims)
    sim_values = portfolio_value * (1 + sim_returns)
    var = np.percentile(sim_values, (1 - confidence) * 100)
    return portfolio_value - var

var_95 = monte_carlo_var(1000000, (0.0005, 0.015))
print(f'\n95%置信水平下的日VaR: ${var_95:,.2f}')
```

### 4. 矩阵运算与统计分析

```python
'''NumPy矩阵运算在量化中的应用'''

import numpy as np

# 协方差矩阵与相关矩阵
returns = np.random.normal(0.001, 0.02, (252, 10))  # 10只资产252天收益率

cov_matrix = np.cov(returns, rowvar=False)  # 协方差矩阵
corr_matrix = np.corrcoef(returns, rowvar=False)  # 相关系数矩阵

# 特征值分解（PCA降维）
eigenvalues, eigenvectors = np.linalg.eigh(cov_matrix)
sorted_idx = np.argsort(eigenvalues)[::-1]
eigenvalues = eigenvalues[sorted_idx]
eigenvectors = eigenvectors[:, sorted_idx]

# 解释方差比例
explained_variance_ratio = eigenvalues / eigenvalues.sum()
print('前5个主成分解释方差比例:')
for i in range(5):
    print(f'  PC{i+1}: {explained_variance_ratio[i]:.4f}')

# Cholesky分解（用于相关随机数生成）
L = np.linalg.cholesky(cov_matrix)
uncorrelated = np.random.standard_normal((10000, 10))
correlated = uncorrelated @ L.T
print(f'\n生成相关随机数矩阵形状: {correlated.shape}')
```

---

## 应用场景

### 场景一：高性能数值计算

所有需要大规模数值运算的量化场景，包括因子计算、风险模型、定价模型等。

### 场景二：投资组合优化

马科维茨均值-方差模型、Black-Litterman模型、风险平价模型等都需要矩阵运算。

### 场景三：蒙特卡洛模拟

期权定价、VaR计算、压力测试等需要大量随机模拟的场景。

### 场景四：信号处理与特征提取

主成分分析（PCA）、奇异值分解（SVD）等降维和特征提取方法。

---

## 优缺点分析

### 优点

| 优点 | 说明 |
|------|------|
| 极高性能 | C语言实现，向量化运算极快 |
| 广泛兼容 | 几乎所有科学计算库的底层依赖 |
| 数值稳定 | 成熟的线性代数算法实现 |
| 内存高效 | 连续内存布局，缓存友好 |
| 广播机制 | 不同形状数组间的智能运算 |

### 缺点

| 缺点 | 说明 |
|------|------|
| 学习曲线 | 广播机制和轴操作需要理解 |
| 调试困难 | 数组操作出错时不易定位 |
| 文档复杂 | 高级功能文档不够直观 |
| GPU支持弱 | 原生不支持GPU加速（需CuPy） |

---

## 与其他工具对比

| 特性 | NumPy | MATLAB | Julia | CuPy |
|------|-------|--------|-------|------|
| 执行速度 | 快 | 快 | 极快 | 极快（GPU） |
| 免费开源 | 是 | 否 | 是 | 是 |
| 生态丰富度 | 极高 | 高 | 中等 | 中等 |
| 学习曲线 | 中等 | 中等 | 平缓 | 平缓 |
| GPU支持 | 无 | 有 | 有 | 原生 |

---

## 推荐资源

### 官方资源
- NumPy官方文档：https://numpy.org/doc/stable/
- NumPy用户指南：https://numpy.org/doc/stable/user/

### 推荐书籍
- 《Python for Data Analysis》 by Wes McKinney（含NumPy章节）
- 《Elegant SciPy》 by Juan Nunez-Iglesias

### 在线资源
- NumPy官方教程：https://numpy.org/learn/
- SciPy Lecture Notes

---

## 总结

NumPy是Python量化交易生态系统的地基。虽然使用者通常不会直接调用NumPy的每一个函数，但pandas、scikit-learn、Backtrader等上层库都建立在NumPy之上。理解NumPy的数组运算、广播机制和向量化编程思维，对于编写高效的量化代码至关重要。

在量化交易中，NumPy最常见的应用场景包括：大规模因子计算、投资组合优化中的矩阵运算、蒙特卡洛模拟以及特征提取中的线性代数操作。建议量化初学者在学习pandas之前，先花1-2周时间系统学习NumPy的基础操作，这将为后续所有量化工具的学习打下坚实的性能优化基础。
