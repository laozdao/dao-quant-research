# scikit-learn — 机器学习量化工具箱

> scikit-learn是Python机器学习的标准库，为量化交易提供从涨跌预测到因子挖掘的完整ML工具链。

---

## 概述

scikit-learn（简称sklearn）是Python中最广泛使用的机器学习库，由David Cournapeau于2007年发起，目前由一个活跃的社区维护。scikit-learn提供了分类、回归、聚类、降维、模型选择和预处理等完整的机器学习工具链，涵盖了从经典统计学习方法到集成学习的广泛算法。在量化交易领域，scikit-learn被广泛用于涨跌预测、因子挖掘、聚类分析、特征选择和模型评估等任务。

scikit-learn的设计哲学是"一致性API"——所有模型都遵循相同的接口规范（fit/predict/transform），使得模型切换和比较变得极其便捷。这种设计在量化研究中尤为重要，因为研究者通常需要快速尝试多种模型来寻找最优预测方案。scikit-learn建立在NumPy、SciPy和matplotlib之上，与pandas等数据处理库无缝集成。

---

## 基本信息表格

| 项目 | 详情 |
|------|------|
| 库名称 | scikit-learn |
| 最新稳定版 | scikit-learn 1.5.x |
| 创建者 | David Cournapeau |
| 首次发布 | 2007年 |
| 开源协议 | BSD-3-Clause |
| 官方网站 | https://scikit-learn.org |
| 安装方式 | `pip install scikit-learn` |
| 核心依赖 | NumPy, SciPy, matplotlib |
| 适用平台 | Windows, macOS, Linux |

---

## 核心特性

### 1. 涨跌预测

使用机器学习模型预测股票涨跌是量化交易中最常见的ML应用：

```python
'''scikit-learn涨跌预测完整流程'''

import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.model_selection import train_test_split, TimeSeriesSplit
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, accuracy_score, roc_auc_score
import yfinance as yf

# Step 1: 获取数据
data = yf.download('SPY', start='2015-01-01', end='2024-01-01')

# Step 2: 特征工程
def create_features(df):
    '''创建预测特征
    
    参数:
        df: 包含OHLCV数据的DataFrame
    返回:
        features: 特征DataFrame
    '''
    features = pd.DataFrame(index=df.index)

    # 价格特征
    features['return_1d'] = df['Close'].pct_change(1)
    features['return_5d'] = df['Close'].pct_change(5)
    features['return_10d'] = df['Close'].pct_change(10)
    features['return_20d'] = df['Close'].pct_change(20)

    # 波动率特征
    features['volatility_5d'] = features['return_1d'].rolling(5).std()
    features['volatility_20d'] = features['return_1d'].rolling(20).std()

    # 技术指标
    features['sma_5'] = df['Close'] / df['Close'].rolling(5).mean()
    features['sma_20'] = df['Close'] / df['Close'].rolling(20).mean()
    features['sma_ratio'] = features['sma_5'] / features['sma_20']

    # 成交量特征
    features['volume_ratio'] = df['Volume'] / df['Volume'].rolling(20).mean()

    # 价格位置
    features['high_low_ratio'] = df['High'] / df['Low']
    features['close_open_ratio'] = df['Close'] / df['Open']

    # 动量特征
    features['momentum_5d'] = df['Close'].pct_change(5).shift(1)
    features['momentum_20d'] = df['Close'].pct_change(20).shift(1)

    return features

features = create_features(data)

# Step 3: 创建标签（未来5日收益方向）
data['future_return'] = data['Close'].pct_change(5).shift(-5)
data['label'] = (data['future_return'] > 0).astype(int)

# 合并特征和标签
dataset = features.copy()
dataset['label'] = data['label']
dataset = dataset.dropna()

# Step 4: 时间序列分割（避免数据泄露）
X = dataset.drop('label', axis=1)
y = dataset['label']

# 按时间顺序分割：70%训练，30%测试
split_idx = int(len(X) * 0.7)
X_train, X_test = X.iloc[:split_idx], X.iloc[split_idx:]
y_train, y_test = y.iloc[:split_idx], y.iloc[split_idx:]

# Step 5: 特征标准化
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Step 6: 训练模型
rf_model = RandomForestClassifier(
    n_estimators=100,
    max_depth=10,
    min_samples_split=20,
    random_state=42,
    n_jobs=-1
)
rf_model.fit(X_train_scaled, y_train)

# Step 7: 预测与评估
y_pred = rf_model.predict(X_test_scaled)
y_prob = rf_model.predict_proba(X_test_scaled)[:, 1]

print('===== 模型评估 =====')
print(f'准确率: {accuracy_score(y_test, y_pred):.4f}')
print(f'AUC: {roc_auc_score(y_test, y_prob):.4f}')
print('\n分类报告:')
print(classification_report(y_test, y_pred, target_names=['下跌', '上涨']))

# 特征重要性
feature_importance = pd.Series(
    rf_model.feature_importances_, index=X.columns
).sort_values(ascending=False)
print('\n特征重要性排名:')
print(feature_importance.round(4))
```

### 2. 因子挖掘

scikit-learn可以用于从大量候选因子中筛选有效的预测因子：

```python
'''scikit-learn因子挖掘与特征选择'''

import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.feature_selection import SelectKBest, f_regression, mutual_info_regression
from sklearn.linear_model import Lasso, Ridge
from sklearn.preprocessing import StandardScaler
import yfinance as yf

# 获取数据
data = yf.download('SPY', start='2015-01-01', end='2024-01-01')

# 生成大量候选因子
def generate_candidate_factors(df):
    '''生成候选因子池
    
    参数:
        df: OHLCV数据
    返回:
        factors: 因子DataFrame
    '''
    factors = pd.DataFrame(index=df.index)
    close = df['Close']
    volume = df['Volume']
    high = df['High']
    low = df['Low']

    # 动量因子族
    for w in [1, 3, 5, 10, 20, 40, 60]:
        factors[f'momentum_{w}'] = close.pct_change(w)

    # 波动率因子族
    returns = close.pct_change()
    for w in [5, 10, 20, 40]:
        factors[f'volatility_{w}'] = returns.rolling(w).std()
        factors[f'downside_vol_{w}'] = returns.rolling(w).apply(
            lambda x: np.sqrt(np.mean(x[x < 0]**2))
        )

    # 均线偏离因子族
    for w in [5, 10, 20, 60]:
        factors[f'ma_deviation_{w}'] = (close - close.rolling(w).mean()) / close.rolling(w).mean()

    # 成交量因子族
    for w in [5, 10, 20]:
        factors[f'volume_sma_{w}'] = volume / volume.rolling(w).mean()
        factors[f'volume_std_{w}'] = volume.rolling(w).std() / volume.rolling(w).mean()

    # 价格形态因子
    factors['high_low_range'] = (high - low) / close
    factors['close_open_gap'] = (close - df['Open']) / df['Open']
    factors['overnight_return'] = df['Open'] / close.shift(1) - 1

    return factors

factors = generate_candidate_factors(data)

# 目标变量：未来5日收益率
target = data['Close'].pct_change(5).shift(-5)

# 清理数据
dataset = factors.copy()
dataset['target'] = target
dataset = dataset.dropna()

X = dataset.drop('target', axis=1)
y = dataset['target']

# 方法1：基于F统计量的单因子筛选
selector_f = SelectKBest(f_regression, k=10)
X_selected_f = selector_f.fit_transform(X, y)
f_scores = pd.Series(selector_f.scores_, index=X.columns).sort_values(ascending=False)
print('F统计量排名（前10）:')
print(f_scores.head(10).round(4))

# 方法2：Lasso回归进行因子筛选
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
lasso = Lasso(alpha=0.001, random_state=42)
lasso.fit(X_scaled, y)
lasso_coefs = pd.Series(lasso.coef_, index=X.columns)
lasso_coefs = lasso_coefs[lasso_coefs != 0].sort_values(key=abs, ascending=False)
print('\nLasso筛选的非零因子:')
print(lasso_coefs.round(6))

# 方法3：随机森林特征重要性
rf = RandomForestRegressor(n_estimators=100, max_depth=10, random_state=42, n_jobs=-1)
rf.fit(X, y)
rf_importance = pd.Series(rf.feature_importances_, index=X.columns).sort_values(ascending=False)
print('\n随机森林特征重要性（前10）:')
print(rf_importance.head(10).round(4))
```

### 3. 聚类分析

聚类分析在量化交易中用于股票分组、市场状态识别和异常检测：

```python
'''scikit-learn聚类分析在量化中的应用'''

import numpy as np
import pandas as pd
from sklearn.cluster import KMeans, DBSCAN, AgglomerativeClustering
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import yfinance as yf

# 获取多只股票数据
tickers = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'NVDA', 'META',
           'JPM', 'GS', 'BAC', 'WFC',
           'JNJ', 'PFE', 'UNH', 'ABBV', 'MRK']
data = yf.download(tickers, start='2022-01-01', end='2024-01-01')['Close']

# 计算特征
features = pd.DataFrame(index=tickers)
features['annual_return'] = data.pct_change().mean() * 252
features['volatility'] = data.pct_change().std() * np.sqrt(252)
features['sharpe'] = features['annual_return'] / features['volatility']
features['max_drawdown'] = data.apply(lambda x: (x / x.cummax() - 1).min())

# 计算相关性特征
returns = data.pct_change().dropna()
corr_with_spy = returns.corrwith(returns['SPY'] if 'SPY' in returns.columns else returns.iloc[:, 0])
features['correlation'] = corr_with_spy

features = features.dropna()

# 标准化特征
scaler = StandardScaler()
X_scaled = scaler.fit_transform(features)

# K-Means聚类
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
features['cluster_kmeans'] = kmeans.fit_predict(X_scaled)

# 层次聚类
agg = AgglomerativeClustering(n_clusters=3)
features['cluster_agg'] = agg.fit_predict(X_scaled)

# 输出聚类结果
print('===== K-Means聚类结果 =====')
for cluster_id in range(3):
    stocks = features[features['cluster_kmeans'] == cluster_id].index.tolist()
    avg_return = features[features['cluster_kmeans'] == cluster_id]['annual_return'].mean()
    avg_vol = features[features['cluster_kmeans'] == cluster_id]['volatility'].mean()
    print(f'\n群组 {cluster_id}:')
    print(f'  股票: {stocks}')
    print(f'  平均年化收益: {avg_return:.4f}')
    print(f'  平均波动率: {avg_vol:.4f}')

# PCA降维可视化
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
print(f'\nPCA解释方差比例: {pca.explained_variance_ratio_}')
```

---

## 应用场景

### 场景一：涨跌方向预测

使用分类模型预测股票未来涨跌方向，生成交易信号。

### 场景二：因子挖掘与筛选

从大量候选因子中筛选具有预测能力的有效因子。

### 场景三：股票聚类分组

基于风险收益特征对股票进行聚类，构建分散化投资组合。

### 场景四：市场状态识别

使用无监督学习识别不同的市场状态（牛市、熊市、震荡市），动态调整策略。

---

## 优缺点分析

### 优点

| 优点 | 说明 |
|------|------|
| 算法全面 | 覆盖分类、回归、聚类等主流ML算法 |
| API一致 | 统一的fit/predict接口，模型切换便捷 |
| 文档优秀 | 官方文档详尽，示例丰富 |
| 与pandas集成 | 数据处理管道无缝衔接 |
| 社区庞大 | 问题解决方案丰富 |

### 缺点

| 缺点 | 说明 |
|------|------|
| 不支持深度学习 | 仅覆盖传统ML算法 |
| 单机计算 | 不支持分布式训练 |
| 时间序列弱 | 对时间序列特化模型支持有限 |
| GPU不支持 | 无GPU加速（需切换到其他库） |
| 过拟合风险 | 金融数据噪声大，容易过拟合 |

---

## 与其他ML框架对比

| 特性 | scikit-learn | XGBoost | TensorFlow | PyTorch |
|------|-------------|---------|-----------|---------|
| 算法覆盖 | 经典ML全面 | 梯度提升树 | 深度学习 | 深度学习 |
| 学习曲线 | 平缓 | 平缓 | 陡峭 | 中等 |
| 训练速度 | 中等 | 快 | 快（GPU） | 快（GPU） |
| 金融应用 | 极多 | 多 | 中等 | 中等 |
| 可解释性 | 强 | 中等 | 弱 | 弱 |
| GPU支持 | 无 | 有 | 原生 | 原生 |

---

## 推荐资源

### 官方资源
- scikit-learn官方文档：https://scikit-learn.org/stable/
- scikit-learn教程：https://scikit-learn.org/stable/tutorial/

### 推荐书籍
- 《Hands-On Machine Learning with Scikit-Learn》 by Aurelien Geron
- 《Python Machine Learning》 by Sebastian Raschka

### 量化ML资源
- 《Advances in Financial Machine Learning》 by Marcos Lopez de Prado
- Quantopian/AlphaVenture的ML策略教程

---

## 总结

scikit-learn是量化交易机器学习应用的标准工具。其丰富的算法库、一致的API设计和与pandas的无缝集成，使得量化研究者可以快速构建从因子挖掘到涨跌预测的完整ML流水线。对于量化初学者，scikit-learn是进入机器学习量化领域的最佳起点。

然而，金融数据的特殊性（非平稳、噪声大、样本量有限）对机器学习方法提出了独特挑战。在使用scikit-learn进行量化研究时，必须特别注意：避免数据泄露（使用时间序列交叉验证）、控制过拟合（正则化和特征选择）、以及正确评估模型性能（考虑交易成本后的实际收益）。建议在掌握scikit-learn的经典方法后，进一步学习XGBoost等梯度提升树方法，它们在金融预测任务中通常表现更好。
