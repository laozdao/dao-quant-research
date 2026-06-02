# pandas — 数据分析核心库

> pandas是Python数据分析的基石，为量化交易提供了高效的数据结构和时间序列处理能力。

---

## 概述

pandas是Python中最核心的数据分析库，由Wes McKinney于2008年创建，其名称来源于"panel data"（面板数据）。pandas提供了两种核心数据结构：Series（一维）和DataFrame（二维），使得结构化数据的操作变得直观高效。在量化交易中，pandas几乎是所有数据处理流程的起点和终点，从市场数据的清洗、技术指标的计算到因子的构建和回测结果的整理，pandas无处不在。

pandas建立在NumPy之上，继承了NumPy的高性能数组运算能力，同时提供了更高级的数据操作接口，包括分组聚合、时间序列重采样、合并连接、透视表等功能。对于量化交易从业者而言，pandas是与Excel同等重要甚至更为关键的工具。

---

## 基本信息表格

| 项目 | 详情 |
|------|------|
| 库名称 | pandas |
| 最新稳定版 | pandas 2.2.x |
| 创建者 | Wes McKinney |
| 首次发布 | 2008年 |
| 开源协议 | BSD-3-Clause |
| 官方网站 | https://pandas.pydata.org |
| 安装方式 | `pip install pandas` |
| 依赖库 | NumPy, python-dateutil, pytz |
| 适用平台 | Windows, macOS, Linux |

---

## 核心特性

### 1. 时间序列处理

pandas对时间序列数据提供了原生支持，这是量化交易中最核心的功能之一：

```python
'''pandas时间序列处理示例'''

import pandas as pd
import numpy as np

# 创建时间序列数据
dates = pd.date_range('2023-01-01', periods=100, freq='B')  # 工作日频率
prices = pd.Series(
    np.cumprod(1 + np.random.normal(0.001, 0.02, 100)) * 100,
    index=dates,
    name='Close'
)

# 基本时间序列操作
print(prices.index)                    # 查看时间索引
print(prices.resample('W').last())     # 按周重采样取最后值
print(prices.resample('M').mean())      # 按月重采样取均值

# 时间对齐：自动按索引对齐
returns = prices.pct_change()
print(returns.head())

# 滚动窗口计算
rolling_mean = prices.rolling(window=20).mean()
rolling_std = prices.rolling(window=20).std()
rolling_max = prices.rolling(window=20).max()

# 指数加权移动平均
ewm_mean = prices.ewm(span=20).mean()

# 移位操作（计算过去N日收益率）
shifted = prices.shift(1)
past_return = (prices - shifted) / shifted

# 时间筛选
q1_data = prices['2023-01':'2023-03']
print(f'2023年Q1共 {len(q1_data)} 个交易日')
```

### 2. 因子计算

pandas是量化因子研究的核心工具，支持高效的因子计算和向量化操作：

```python
'''使用pandas计算量化因子'''

import pandas as pd
import numpy as np

# 模拟多只股票的价格数据
np.random.seed(42)
stocks = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'TSLA']
dates = pd.date_range('2023-01-01', periods=252, freq='B')

# 创建多索引DataFrame
price_data = pd.DataFrame(
    np.cumprod(1 + np.random.normal(0.0005, 0.02, (252, 5))) * 100,
    index=dates,
    columns=stocks
)

# 因子1：动量因子（过去20日收益率）
momentum_20d = price_data.pct_change(20)

# 因子2：波动率因子（过去20日收益率标准差）
volatility_20d = price_data.pct_change().rolling(20).std()

# 因子3：成交量加权价格比率
volume_data = pd.DataFrame(
    np.random.randint(1000000, 50000000, (252, 5)),
    index=dates,
    columns=stocks
)
vwap_ratio = (price_data * volume_data).rolling(5).sum() / \
             (volume_data.rolling(5).sum() * price_data)

# 因子4：均值回归因子（价格偏离均线程度）
ma_20 = price_data.rolling(20).mean()
ma_deviation = (price_data - ma_20) / ma_20

# 因子5：高低价振幅因子
# 假设有最高价和最低价数据
high_data = price_data * (1 + np.abs(np.random.normal(0, 0.01, (252, 5))))
low_data = price_data * (1 - np.abs(np.random.normal(0, 0.01, (252, 5))))
amplitude = (high_data - low_data) / price_data

# 构建因子DataFrame
factors = pd.concat([
    momentum_20d.add_suffix('_mom'),
    volatility_20d.add_suffix('_vol'),
    ma_deviation.add_suffix('_mad'),
    amplitude.add_suffix('_amp')
], axis=1)

print(f'因子矩阵形状: {factors.shape}')
print(f'最新一行因子值:\n{factors.iloc[-1].head(10)}')

# 因子标准化（横截面标准化）
factors_standardized = factors.groupby(level=0, group_keys=False).apply(
    lambda x: (x - x.mean()) / x.std()
)
```

### 3. 数据清洗

量化交易中数据质量至关重要，pandas提供了全面的数据清洗工具：

```python
'''pandas数据清洗示例'''

import pandas as pd
import numpy as np

# 模拟含脏数据的股票数据
data = pd.DataFrame({
    'date': pd.date_range('2023-01-01', periods=100, freq='B'),
    'open': np.random.uniform(90, 110, 100),
    'high': np.random.uniform(100, 120, 100),
    'low': np.random.uniform(80, 100, 100),
    'close': np.random.uniform(90, 110, 100),
    'volume': np.random.randint(1000000, 50000000, 100)
})

# 人为添加脏数据
data.loc[5, 'close'] = np.nan           # 缺失值
data.loc[10, 'high'] = 50               # 异常低值
data.loc[15, 'low'] = 200                # 异常高值
data.loc[20, 'volume'] = 0              # 零成交量
data.loc[25:30, 'close'] = data.loc[25:30, 'open']  # 开收价完全相同

# 1. 检测缺失值
print('缺失值统计:')
print(data.isnull().sum())

# 2. 处理缺失值
data['close'] = data['close'].fillna(method='ffill')  # 前向填充

# 3. 检测并修正价格异常
# 最高价应 >= 开盘价、收盘价
data.loc[data['high'] < data[['open', 'close']].max(axis=1), 'high'] = \
    data[['open', 'close']].max(axis=1)
# 最低价应 <= 开盘价、收盘价
data.loc[data['low'] > data[['open', 'close']].min(axis=1), 'low'] = \
    data[['open', 'close']].min(axis=1)

# 4. 过滤零成交量数据
data = data[data['volume'] > 0]

# 5. 去除重复数据
data = data.drop_duplicates(subset=['date'])

# 6. 设置日期索引
data = data.set_index('date')

print(f'清洗后数据形状: {data.shape}')
print(f'数据完整性: {data.notnull().mean().mean():.2%}')
```

### 4. 分组聚合与多股票分析

```python
'''pandas分组聚合与多股票分析'''

import pandas as pd
import numpy as np

# 创建面板数据（多股票多日期）
np.random.seed(42)
dates = pd.date_range('2023-01-01', periods=252, freq='B')
sectors = ['Tech', 'Finance', 'Healthcare']
stocks_info = {
    'AAPL': ('Tech', 150), 'MSFT': ('Tech', 300),
    'JPM': ('Finance', 140), 'GS': ('Finance', 350),
    'JNJ': ('Healthcare', 160), 'PFE': ('Healthcare', 30)
}

panel_data = []
for ticker, (sector, base_price) in stocks_info.items():
    prices = pd.Series(
        np.cumprod(1 + np.random.normal(0.0003, 0.015, 252)) * base_price,
        index=dates
    )
    df = pd.DataFrame({
        'ticker': ticker,
        'sector': sector,
        'close': prices.values,
        'volume': np.random.randint(1000000, 50000000, 252)
    }, index=dates)
    panel_data.append(df)

df = pd.concat(panel_data)

# 按行业分组计算统计量
sector_stats = df.groupby('sector').agg({
    'close': ['mean', 'std', 'min', 'max'],
    'volume': 'sum'
})
print('行业统计:')
print(sector_stats)

# 按月分组计算月度收益率
monthly_returns = df.groupby(['ticker', pd.Grouper(freq='M')])['close'].last()
monthly_returns = monthly_returns.unstack('ticker').pct_change()
print(f'\n月度收益率相关矩阵:')
print(monthly_returns.corr().round(3))
```

---

## 应用场景

### 场景一：市场数据预处理

从各种数据源获取的原始数据通常包含缺失值、异常值和格式不一致等问题，pandas是数据清洗的首选工具。

### 场景二：因子研究与构建

量化因子研究需要对大量股票数据进行横截面和时间序列计算，pandas的分组聚合和滚动窗口功能非常适合这类任务。

### 场景三：回测数据处理

将回测结果整理为结构化数据，计算绩效指标，生成分析报告。

### 场景四：多资产分析

对股票、债券、期货等多资产数据进行统一处理和比较分析。

---

## 优缺点分析

### 优点

| 优点 | 说明 |
|------|------|
| API设计优秀 | 直观易用，学习曲线平缓 |
| 时间序列支持 | 原生时间索引，重采样、滚动窗口等功能完善 |
| 数据结构灵活 | DataFrame可处理各种表格数据 |
| I/O支持全面 | 支持CSV、Excel、SQL、JSON、HDF5等格式 |
| 社区庞大 | 问题解决方案丰富 |

### 缺点

| 缺点 | 说明 |
|------|------|
| 内存占用大 | 大数据场景下内存消耗显著 |
| 单线程限制 | 核心操作无法利用多核CPU |
| 链式操作复杂 | 复杂的数据处理管道可读性差 |
| 警告信息多 | 版本更新频繁，废弃警告较多 |

---

## 与其他工具对比

| 特性 | pandas | R data.table | Polars | Spark |
|------|--------|-------------|--------|-------|
| 数据规模 | 中小 | 中大 | 大 | 极大 |
| 执行速度 | 中等 | 快 | 极快 | 快 |
| 学习曲线 | 平缓 | 中等 | 平缓 | 陡峭 |
| 生态丰富度 | 极高 | 中等 | 低 | 高 |
| 时间序列支持 | 极强 | 强 | 强 | 中等 |

---

## 推荐资源

### 官方资源
- pandas官方文档：https://pandas.pydata.org/docs/
- pandas Cookbook：https://pandas.pydata.org/docs/user_guide/cookbook.html

### 推荐书籍
- 《Python for Data Analysis》 by Wes McKinney
- 《Learning pandas》 by Daniel Chen

### 在线资源
- pandas官方YouTube频道教程
- Kaggle上的pandas微课程

---

## 总结

pandas是量化交易数据处理的核心工具，几乎所有Python量化工作流都离不开它。其优雅的API设计、强大的时间序列处理能力和丰富的数据操作接口，使得pandas成为量化从业者日常工作中使用频率最高的库之一。

对于量化初学者，建议将pandas作为第一个深入学习的数据分析工具。掌握pandas的DataFrame操作、时间序列处理、分组聚合等核心功能，将为后续学习回测框架、因子研究和机器学习打下坚实基础。在处理超大规模数据时，可以考虑使用Polars或Dask作为pandas的替代或补充方案。
