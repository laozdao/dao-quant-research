---
title: "数据处理与特征工程：量化研究的核心技能"
date: "2026-05-20"
author: "laozdao"
category: "R02"
tags:
  - "数据处理"
  - "特征工程"
  - "数据清洗"
  - "因子构建"
  - "量化研究"
status: "published"
version: "1.0"
summary: "本文系统介绍量化研究中的数据处理与特征工程方法，包括数据清洗、缺失值处理、异常值检测、因子构建、特征选择等核心技能。高质量的数据处理和特征工程是模型成功的关键。"
difficulty: "advanced"
reading_time: 25
series: "研究方法论系列"
series_order: 2
---

# 数据处理与特征工程：量化研究的核心技能

> **一句话摘要**：系统介绍量化研究中的数据处理与特征工程方法，帮助研究者构建高质量的因子和特征。

---

## 一、引言：数据质量决定模型上限

### 1.1 数据处理的重要性

在量化研究中，数据质量直接决定模型效果：

| 问题 | 影响 | 解决方案 |
|------|------|---------|
| **缺失值** | 模型无法训练 | 填充或删除 |
| **异常值** | 模型偏差 | 检测和处理 |
| **数据错误** | 错误信号 | 校验和清洗 |
| **幸存者偏差** | 回测失真 | 使用历史全量数据 |

### 1.2 特征工程的价值

好的特征比复杂的模型更重要：

```
┌─────────────────────────────────────────────────────────┐
│                   特征工程的价值                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  原始数据 ──► 特征工程 ──► 高质量特征 ──► 简单模型      │
│      │           │              │            │          │
│      │           │              │            ▼          │
│      │           │              │         良好效果      │
│      │           │              │                       │
│      │           ▼              │                       │
│      │      领域知识+技巧       │                       │
│      │                           │                       │
│      └───────────────────────────┘                       │
│                                                         │
│  结论：特征工程是连接原始数据和模型效果的桥梁           │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 二、数据清洗

### 2.1 缺失值处理

#### 2.1.1 缺失值类型

| 类型 | 说明 | 示例 |
|------|------|------|
| **MCAR** | 完全随机缺失 | 系统故障 |
| **MAR** | 随机缺失 | 小市值股票数据缺失 |
| **MNAR** | 非随机缺失 | ST股票数据缺失 |

#### 2.1.2 缺失值处理方法

```python
import pandas as pd
import numpy as np

# 1. 删除缺失值
df_clean = df.dropna()  # 删除任何有缺失的行
df_clean = df.dropna(subset=['close', 'volume'])  # 删除特定列缺失的行

# 2. 填充缺失值
# 前向填充（适用于时间序列）
df['close'] = df['close'].fillna(method='ffill')

# 均值填充
df['pe'] = df['pe'].fillna(df['pe'].mean())

# 中位数填充（对异常值更稳健）
df['pb'] = df['pb'].fillna(df['pb'].median())

# 行业均值填充
df['roe'] = df.groupby('industry')['roe'].transform(lambda x: x.fillna(x.mean()))
```

#### 2.1.3 缺失值处理策略

| 场景 | 策略 |
|------|------|
| 缺失比例<5% | 直接删除 |
| 缺失比例5%-30% | 填充处理 |
| 缺失比例>30% | 考虑删除该特征 |
| 时间序列 | 前向/后向填充 |
| 截面数据 | 均值/中位数填充 |

### 2.2 异常值检测与处理

#### 2.2.1 统计方法

```python
# 1. 标准差法
def detect_outliers_std(data, threshold=3):
    mean = np.mean(data)
    std = np.std(data)
    outliers = np.abs(data - mean) > threshold * std
    return outliers

# 2. IQR法（四分位距法）
def detect_outliers_iqr(data, k=1.5):
    Q1 = data.quantile(0.25)
    Q3 = data.quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - k * IQR
    upper_bound = Q3 + k * IQR
    outliers = (data < lower_bound) | (data > upper_bound)
    return outliers

# 3. MAD法（中位数绝对偏差法）
def detect_outliers_mad(data, threshold=3.5):
    median = np.median(data)
    mad = np.median(np.abs(data - median))
    modified_z_scores = 0.6745 * (data - median) / mad
    outliers = np.abs(modified_z_scores) > threshold
    return outliers
```

#### 2.2.2 异常值处理

```python
# 1. 截断处理（Winsorize）
from scipy.stats import mstats

df['pe_winsorized'] = mstats.winsorize(df['pe'], limits=[0.05, 0.05])

# 2. 分位数截断
lower = df['pe'].quantile(0.01)
upper = df['pe'].quantile(0.99)
df['pe_clipped'] = df['pe'].clip(lower, upper)

# 3. 标记异常值
df['is_outlier'] = detect_outliers_iqr(df['pe'])
```

### 2.3 数据校验

#### 2.3.1 逻辑校验

```python
def validate_data(df):
    errors = []
    
    # 1. 价格逻辑校验
    if not (df['low'] <= df['close']).all():
        errors.append("收盘价低于最低价")
    
    if not (df['close'] <= df['high']).all():
        errors.append("收盘价高于最高价")
    
    # 2. 收益率校验
    calculated_return = df['close'].pct_change()
    if not np.allclose(calculated_return, df['return'], rtol=1e-5):
        errors.append("收益率计算不一致")
    
    # 3. 财务数据校验
    if not (df['total_assets'] >= df['total_liabilities']).all():
        errors.append("资产小于负债")
    
    return errors
```

---

## 三、数据标准化

### 3.1 标准化方法

#### 3.1.1 Z-Score标准化

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
df['roe_zscore'] = scaler.fit_transform(df[['roe']])
```

$$
z = \frac{x - \mu}{\sigma}
$$

#### 3.1.2 Min-Max标准化

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler(feature_range=(0, 1))
df['roe_minmax'] = scaler.fit_transform(df[['roe']])
```

$$
x_{norm} = \frac{x - x_{min}}{x_{max} - x_{min}}
$$

#### 3.1.3 Rank标准化

```python
def rank_standardize(series):
    """Rank标准化，将数据转换为排名分位数"""
    return series.rank(pct=True)

df['roe_rank'] = df.groupby('trade_date')['roe'].transform(rank_standardize)
```

### 3.2 行业中性化

```python
def industry_neutralize(df, factor, industry_col='industry'):
    """行业中性化处理"""
    # 计算行业均值
    industry_mean = df.groupby(industry_col)[factor].transform('mean')
    # 减去行业均值
    return df[factor] - industry_mean

df['roe_neutral'] = industry_neutralize(df, 'roe')
```

### 3.3 市值中性化

```python
def market_cap_neutralize(df, factor, cap_col='market_cap'):
    """市值中性化处理"""
    from sklearn.linear_model import LinearRegression
    
    # 对数市值
    log_cap = np.log(df[cap_col])
    
    # 回归去除市值影响
    model = LinearRegression()
    model.fit(log_cap.values.reshape(-1, 1), df[factor])
    predicted = model.predict(log_cap.values.reshape(-1, 1))
    
    # 残差即为中性化后的因子
    return df[factor] - predicted

df['roe_neutral'] = market_cap_neutralize(df, 'roe')
```

---

## 四、特征构建

### 4.1 技术指标特征

```python
def calculate_technical_features(df):
    """计算技术指标特征"""
    
    # 1. 移动平均线
    df['ma_5'] = df['close'].rolling(window=5).mean()
    df['ma_20'] = df['close'].rolling(window=20).mean()
    df['ma_60'] = df['close'].rolling(window=60).mean()
    
    # 2. 价格位置
    df['price_position'] = (df['close'] - df['low'].rolling(20).min()) / \
                           (df['high'].rolling(20).max() - df['low'].rolling(20).min())
    
    # 3. 波动率
    df['volatility_20'] = df['close'].pct_change().rolling(20).std() * np.sqrt(252)
    
    # 4. 成交量特征
    df['volume_ma_20'] = df['volume'].rolling(20).mean()
    df['volume_ratio'] = df['volume'] / df['volume_ma_20']
    
    # 5. RSI
    delta = df['close'].diff()
    gain = (delta.where(delta > 0, 0)).rolling(window=14).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=14).mean()
    rs = gain / loss
    df['rsi'] = 100 - (100 / (1 + rs))
    
    return df
```

### 4.2 财务特征构建

```python
def calculate_financial_features(df):
    """计算财务特征"""
    
    # 1. 盈利能力
    df['roe'] = df['net_profit'] / df['equity']
    df['roa'] = df['net_profit'] / df['total_assets']
    df['gross_margin'] = (df['revenue'] - df['cost']) / df['revenue']
    
    # 2. 成长能力
    df['revenue_growth'] = df['revenue'].pct_change(4)  # 季度同比
    df['profit_growth'] = df['net_profit'].pct_change(4)
    
    # 3. 估值特征
    df['pe'] = df['market_cap'] / df['net_profit']
    df['pb'] = df['market_cap'] / df['equity']
    df['ps'] = df['market_cap'] / df['revenue']
    
    # 4. 财务健康
    df['debt_ratio'] = df['total_liabilities'] / df['total_assets']
    df['current_ratio'] = df['current_assets'] / df['current_liabilities']
    
    return df
```

### 4.3 量价特征构建

```python
def calculate_price_volume_features(df):
    """计算量价特征"""
    
    # 1. 收益率特征
    df['return_1d'] = df['close'].pct_change(1)
    df['return_5d'] = df['close'].pct_change(5)
    df['return_20d'] = df['close'].pct_change(20)
    
    # 2. 波动特征
    df['volatility_5d'] = df['return_1d'].rolling(5).std()
    df['volatility_20d'] = df['return_1d'].rolling(20).std()
    
    # 3. 趋势特征
    df['trend_20d'] = (df['close'] / df['close'].shift(20) - 1)
    df['trend_60d'] = (df['close'] / df['close'].shift(60) - 1)
    
    # 4. 成交量特征
    df['volume_change'] = df['volume'].pct_change()
    df['volume_trend'] = df['volume'].rolling(20).mean() / df['volume'].rolling(60).mean()
    
    # 5. 量价背离
    df['price_volume_corr'] = df['close'].rolling(20).corr(df['volume'])
    
    return df
```

---

## 五、特征选择

### 5.1 过滤法

```python
from sklearn.feature_selection import SelectKBest, f_regression

# 选择Top K个特征
selector = SelectKBest(score_func=f_regression, k=10)
X_selected = selector.fit_transform(X, y)

# 查看特征重要性
feature_scores = pd.DataFrame({
    'feature': X.columns,
    'score': selector.scores_
}).sort_values('score', ascending=False)
```

### 5.2 包装法

```python
from sklearn.feature_selection import RFE
from sklearn.ensemble import RandomForestRegressor

# 递归特征消除
estimator = RandomForestRegressor(n_estimators=100)
selector = RFE(estimator, n_features_to_select=10)
X_selected = selector.fit_transform(X, y)

# 查看选择的特征
selected_features = X.columns[selector.support_]
```

### 5.3 嵌入法

```python
from sklearn.linear_model import Lasso

# Lasso自动特征选择
lasso = Lasso(alpha=0.01)
lasso.fit(X, y)

# 查看非零系数特征
selected_features = X.columns[lasso.coef_ != 0]
```

### 5.4 特征重要性分析

```python
from sklearn.ensemble import RandomForestRegressor
import matplotlib.pyplot as plt

# 训练模型
model = RandomForestRegressor(n_estimators=100)
model.fit(X, y)

# 获取特征重要性
importance = pd.DataFrame({
    'feature': X.columns,
    'importance': model.feature_importances_
}).sort_values('importance', ascending=False)

# 可视化
plt.figure(figsize=(10, 6))
plt.barh(importance['feature'][:20], importance['importance'][:20])
plt.xlabel('Importance')
plt.title('Top 20 Feature Importance')
plt.show()
```

---

## 六、特征工程最佳实践

### 6.1 原则

| 原则 | 说明 |
|------|------|
| **可解释性** | 特征应有明确的经济含义 |
| **稳定性** | 特征应在不同时间段稳定 |
| **预测性** | 特征应与目标变量相关 |
| **正交性** | 避免高度相关的特征 |

### 6.2 检查清单

```
□ 数据清洗完成
  □ 缺失值处理
  □ 异常值处理
  □ 数据校验通过

□ 特征构建完成
  □ 技术指标
  □ 财务指标
  □ 量价指标
  □ 宏观指标

□ 特征处理完成
  □ 标准化
  □ 中性化
  □ 去极值

□ 特征选择完成
  □ 相关性分析
  □ 重要性分析
  □ 冗余特征剔除
```

---

## 七、结语

数据处理与特征工程是量化研究的核心技能。高质量的数据处理和精心设计的特征，是模型成功的基础。记住：Garbage In, Garbage Out。投入时间做好数据处理和特征工程，比盲目追求复杂模型更有价值。

---

## 参考文献

1. Feature Engineering for Machine Learning (Alice Zheng).
2. Python for Data Analysis (Wes McKinney).
3. Scikit-learn官方文档.

---

## 更新记录

| 版本 | 日期 | 更新内容 |
|------|------|---------|
| v1.0 | 2026-05-20 | 首次发布，介绍数据处理与特征工程方法 |

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao (老子道) · Dao Quant Research**
