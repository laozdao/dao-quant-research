# vectorbt — 向量化高速回测

> vectorbt利用向量化运算实现毫秒级回测，是参数优化和因子研究的极速引擎。

---

## 概述

vectorbt是一个基于NumPy和pandas的高性能向量化回测库，由polakowo于2020年创建。与传统事件驱动回测框架不同，vectorbt将回测过程转化为矩阵运算，利用NumPy的向量化能力实现极高的计算速度。vectorbt特别擅长大规模参数优化和因子回测，可以在数秒内完成数千组参数组合的回测。

vectorbt的核心设计理念是"用数组运算替代循环"。通过将价格数据、信号、仓位等全部表示为NumPy数组或pandas DataFrame，vectorbt可以一次性计算所有时间步和所有参数组合的回测结果。这种向量化方法在参数优化场景中具有压倒性优势——原本需要数小时才能完成的参数扫描，在vectorbt中只需几秒。

---

## 基本信息表格

| 项目 | 详情 |
|------|------|
| 库名称 | vectorbt |
| 最新稳定版 | vectorbt 0.26.x |
| 创建者 | polakowo |
| 首次发布 | 2020年 |
| 开源协议 | BSD-2-Clause |
| GitHub仓库 | https://github.com/polanik/vectorbt |
| 安装方式 | `pip install vectorbt` |
| 架构模式 | 向量化（Vectorized） |
| 核心依赖 | NumPy, pandas, plotly |
| 适用平台 | Windows, macOS, Linux |

---

## 核心特性

### 1. 基础回测

vectorbt提供了简洁的API来执行快速回测：

```python
'''vectorbt基础回测示例'''

import vectorbt as vbt
import numpy as np
import pandas as pd

# 获取价格数据
price_data = vbt.YFData.download('AAPL', start='2020-01-01', end='2024-01-01')
prices = price_data.get('Close')

# 简单的SMA交叉策略回测
fast_ma = vbt.MA.run(prices, window=10)
slow_ma = vbt.MA.run(prices, window=30)

# 生成交易信号
entries = fast_ma.ma_crossed_above(slow_ma)
exits = fast_ma.ma_crossed_below(slow_ma)

# 执行回测
pf = vbt.Portfolio.from_signals(
    close=prices,
    entries=entries,
    exits=exits,
    init_cash=100000,
    fees=0.001,
    freq='1D'
)

# 输出绩效统计
print(pf.stats())

# 绘制权益曲线
pf.plot().show()
```

### 2. 参数优化

vectorbt的参数优化能力是其最大亮点，支持大规模参数组合的并行回测：

```python
'''vectorbt大规模参数优化'''

import vectorbt as vbt
import numpy as np
import pandas as pd

# 获取价格数据
price_data = vbt.YFData.download('SPY', start='2015-01-01', end='2024-01-01')
prices = price_data.get('Close')

# 定义参数范围
fast_windows = np.arange(5, 50, 5)    # 快速均线: 5, 10, 15, ..., 45
slow_windows = np.arange(20, 100, 10)  # 慢速均线: 20, 30, 40, ..., 90

# 使用参数优化器进行大规模回测
# vectorbt会自动遍历所有参数组合
fast_ma = vbt.MA.run(prices, window=fast_windows, short_name='fast')
slow_ma = vbt.MA.run(prices, window=slow_windows, short_name='slow')

# 生成交叉信号（自动广播到所有参数组合）
entries = fast_ma.ma_crossed_above(slow_ma)
exits = fast_ma.ma_crossed_below(slow_ma)

# 执行回测（一次性计算所有参数组合）
pf = vbt.Portfolio.from_signals(
    close=prices,
    entries=entries,
    exits=exits,
    init_cash=100000,
    fees=0.001,
    freq='1D'
)

# 提取所有参数组合的绩效
returns = pf.total_return()
sharpe = pf.sharpe_ratio()
max_dd = pf.max_drawdown()

# 找到最优参数组合
best_idx = returns.idxmax()
print(f'最优参数: fast={fast_windows[best_idx[0]]}, slow={slow_windows[best_idx[1]]}')
print(f'最优总收益率: {returns.max():.4f}')
print(f'最优夏普比率: {sharpe.max():.4f}')

# 参数热力图
print('\n参数组合总收益率矩阵:')
returns_df = returns.unstack(level=1)
returns_df.index = fast_windows
returns_df.columns = slow_windows
print(returns_df.round(4))
```

### 3. 因子回测

vectorbt非常适合量化因子的批量回测和评估：

```python
'''vectorbt因子回测示例'''

import vectorbt as vbt
import numpy as np
import pandas as pd

# 模拟多只股票的价格数据
np.random.seed(42)
n_stocks = 50
n_days = 252 * 3  # 3年数据
dates = pd.date_range('2021-01-01', periods=n_days, freq='B')
tickers = [f'STOCK_{i:03d}' for i in range(n_stocks)]

# 生成价格矩阵
prices = pd.DataFrame(
    np.cumprod(1 + np.random.normal(0.0003, 0.015, (n_days, n_stocks))) * 100,
    index=dates,
    columns=tickers
)

# 计算多个因子
def calculate_momentum(prices, window):
    '''计算动量因子'''
    return prices.pct_change(window)

def calculate_volatility(prices, window):
    '''计算波动率因子（取负值，低波动优先）'''
    return -prices.pct_change().rolling(window).std()

def calculate_mean_reversion(prices, window):
    '''计算均值回归因子'''
    ma = prices.rolling(window).mean()
    return -(prices - ma) / ma

# 计算不同窗口的因子
factors = {}
for w in [5, 10, 20, 60]:
    factors[f'momentum_{w}'] = calculate_momentum(prices, w)
    factors[f'volatility_{w}'] = calculate_volatility(prices, w)
    factors[f'mr_{w}'] = calculate_mean_reversion(prices, w)

# 对每个因子进行分组回测
# 每期按因子值排序，做多前20%，做空后20%
results = {}
for factor_name, factor_values in factors.items():
    # 去除NaN
    clean_factor = factor_values.dropna()
    
    # 每期计算分位数
    q_upper = clean_factor.quantile(0.8, axis=1)
    q_lower = clean_factor.quantile(0.2, axis=1)
    
    # 生成信号：因子值 > 80分位做多，< 20分位做空
    entries = clean_factor > q_upper
    exits = clean_factor < q_lower
    
    # 回测
    pf = vbt.Portfolio.from_signals(
        close=prices,
        entries=entries,
        exits=exits,
        init_cash=100000,
        fees=0.001,
        freq='1D',
        cash_sharing=True
    )
    
    results[factor_name] = {
        'total_return': pf.total_return(),
        'sharpe': pf.sharpe_ratio(),
        'max_drawdown': pf.max_drawdown(),
        'win_rate': pf.trades.winning_trades.count() / max(pf.trades.count(), 1)
    }

# 汇总因子绩效
results_df = pd.DataFrame(results).T
results_df = results_df.sort_values('sharpe', ascending=False)
print('因子绩效排名:')
print(results_df.round(4))
```

### 4. 交互式可视化

```python
'''vectorbt交互式可视化'''

import vectorbt as vbt

# 获取数据
price_data = vbt.YFData.download('AAPL', start='2020-01-01', end='2024-01-01')
prices = price_data.get('Close')

# 回测
fast_ma = vbt.MA.run(prices, window=10)
slow_ma = vbt.MA.run(prices, window=30)
entries = fast_ma.ma_crossed_above(slow_ma)
exits = fast_ma.ma_crossed_below(slow_ma)

pf = vbt.Portfolio.from_signals(
    close=prices,
    entries=entries,
    exits=exits,
    init_cash=100000,
    fees=0.001
)

# 生成交互式图表
fig = pf.plot(subplots=[
    'orders', 'cum_returns', 'drawdowns', 'trade_pnl'
])
fig.show()

# 参数优化热力图
fast_windows = range(5, 50, 5)
slow_windows = range(20, 100, 10)
fast_ma = vbt.MA.run(prices, window=list(fast_windows))
slow_ma = vbt.MA.run(prices, window=list(slow_windows))

entries = fast_ma.ma_crossed_above(slow_ma)
exits = fast_ma.ma_crossed_below(slow_ma)
pf = vbt.Portfolio.from_signals(
    close=prices, entries=entries, exits=exits,
    init_cash=100000, fees=0.001
)

# 绘制夏普比率热力图
sharpe = pf.sharpe_ratio()
sharpe_df = sharpe.unstack(level=1)
sharpe_df.vbt.heatmap().show()
```

---

## 应用场景

### 场景一：大规模参数优化

当需要测试数百甚至数千组参数组合时，vectorbt的向量化运算具有压倒性优势。

### 场景二：因子研究

对大量因子进行快速回测和筛选，评估因子的预测能力和稳健性。

### 场景三：多股票组合回测

同时回测多只股票的策略表现，评估策略的横截面效果。

### 场景四：快速策略原型

在策略开发初期，使用vectorbt快速验证策略的基本逻辑和效果。

---

## 优缺点分析

### 优点

| 优点 | 说明 |
|------|------|
| 极速回测 | 向量化运算，参数优化速度极快 |
| API简洁 | 几行代码即可完成复杂回测 |
| 交互式图表 | 基于Plotly的交互式可视化 |
| 参数优化强 | 原生支持大规模参数扫描 |
| pandas集成 | 与pandas无缝协作 |

### 缺点

| 缺点 | 说明 |
|------|------|
| 交易细节弱 | 不擅长模拟复杂订单类型 |
| 学习资源少 | 社区和教程相对较少 |
| 内存消耗大 | 大规模参数优化时内存需求高 |
| 实盘对接无 | 不支持实盘交易对接 |
| 复杂策略受限 | 复杂的条件逻辑表达困难 |

---

## 与其他回测框架对比

| 特性 | vectorbt | Backtrader | Zipline | QuantConnect |
|------|----------|-----------|---------|-------------|
| 回测速度 | 极快 | 中等 | 中等 | 快 |
| 参数优化 | 极强 | 弱 | 中等 | 强 |
| 交易细节 | 弱 | 极强 | 强 | 强 |
| 学习曲线 | 平缓 | 陡峭 | 中等 | 中等 |
| 可视化 | 极强 | 中等 | 弱 | 强 |
| 实盘对接 | 无 | 支持 | 有限 | 原生 |

---

## 推荐资源

### 官方资源
- vectorbt官方文档：https://vectorbt.dev/
- GitHub仓库：https://github.com/polakowo/vectorbt

### 学习资源
- vectorbt官方教程和示例
- YouTube上的vectorbt教学视频

---

## 总结

vectorbt是量化研究中参数优化和因子筛选的利器。其向量化运算架构使得大规模参数扫描变得极其高效，原本需要数小时的回测任务可以在几秒内完成。对于专注于因子研究和参数优化的量化从业者，vectorbt是不可或缺的工具。

vectorbt的最佳使用场景是策略研究阶段——快速验证大量参数组合和因子组合的效果。一旦确定了最优参数，建议使用Backtrader进行更精细的事件驱动回测，验证策略在考虑滑点、手续费等细节后的真实表现。vectorbt与Backtrader的组合使用，是当前Python量化回测的最佳实践之一。
