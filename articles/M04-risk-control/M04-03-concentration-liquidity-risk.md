---
title: M04-03 集中度风险与流动性风险管理
description: 深入探讨量化投资中的集中度风险和流动性风险管理方法，包括持仓集中度控制、流动性指标监控、压力测试与应急预案
tags:
  - 集中度风险
  - 流动性风险
  - 风险管理
  - 持仓控制
  - 压力测试
category: model
difficulty: advanced
series: M04-risk-control
series_order: 3
created: 2026-05-21
updated: 2026-05-21
---

# M04-03 集中度风险与流动性风险管理

## 一、风险管理框架概述

### 1.1 风险分类体系

在DAO量化模型的风险控制引擎中，集中度风险和流动性风险是两大核心风险类型：

```
┌─────────────────────────────────────────────────────────────┐
│                    风险控制引擎架构                          │
├─────────────┬─────────────┬─────────────┬───────────────────┤
│   市场风险   │   信用风险   │  集中度风险  │    流动性风险     │
├─────────────┼─────────────┼─────────────┼───────────────────┤
│ 价格波动     │ 违约风险     │ 个股集中度   │ 市场流动性        │
│ 系统性风险   │ 评级下调     │ 行业集中度   │ 个股流动性        │
│ 利率风险     │ 对手方风险   │ 因子集中度   │ 资金流动性        │
│ 汇率风险     │ 担保风险     │ 策略集中度   │ 变现能力          │
└─────────────┴─────────────┴─────────────┴───────────────────┘
```

### 1.2 集中度与流动性风险的关联

| 风险类型 | 定义 | 主要表现 | 相互影响 |
|----------|------|----------|----------|
| 集中度风险 | 投资过度集中于特定标的 | 单一股票/行业占比过高 | 高集中度导致流动性需求集中 |
| 流动性风险 | 资产无法以合理价格变现 | 买卖价差扩大、成交困难 | 流动性差限制集中度调整能力 |

## 二、集中度风险管理

### 2.1 持仓集中度度量

#### 2.1.1 个股集中度指标

```python
import pandas as pd
import numpy as np

def calculate_stock_concentration(positions: pd.DataFrame) -> dict:
    """
    计算个股集中度指标
    
    Args:
        positions: 持仓DataFrame，包含stock_code, market_value, weight等列
        
    Returns:
        集中度指标字典
    """
    weights = positions['weight'].sort_values(ascending=False)
    
    metrics = {}
    
    # 1. 最大持仓占比
    metrics['max_single_position'] = weights.iloc[0]
    
    # 2. 前5大持仓占比
    metrics['top5_concentration'] = weights.head(5).sum()
    
    # 3. 前10大持仓占比
    metrics['top10_concentration'] = weights.head(10).sum()
    
    # 4. 赫芬达尔-赫希曼指数(HHI)
    metrics['hhi'] = (weights ** 2).sum()
    
    # 5. 有效持仓数量
    metrics['effective_n_stocks'] = 1 / metrics['hhi']
    
    # 6. 基尼系数
    metrics['gini_coefficient'] = calculate_gini(weights.values)
    
    return metrics

def calculate_gini(weights: np.ndarray) -> float:
    """
    计算基尼系数
    
    衡量持仓不平等程度
    """
    weights = np.sort(weights)
    n = len(weights)
    cumsum = np.cumsum(weights)
    return (2 * np.sum((np.arange(1, n + 1) * weights))) / (n * cumsum[-1]) - (n + 1) / n

# 示例数据
np.random.seed(42)
n_stocks = 50
weights = np.random.exponential(0.02, n_stocks)
weights = weights / weights.sum()

positions = pd.DataFrame({
    'stock_code': [f'SH{600000+i}' for i in range(n_stocks)],
    'weight': weights,
    'market_value': weights * 100000000  # 假设1亿组合
})

concentration = calculate_stock_concentration(positions)
print("个股集中度指标:")
for key, value in concentration.items():
    if isinstance(value, float):
        print(f"  {key}: {value:.4f}")
    else:
        print(f"  {key}: {value}")
```

#### 2.1.2 行业集中度度量

```python
def calculate_industry_concentration(positions: pd.DataFrame) -> dict:
    """
    计算行业集中度指标
    
    Args:
        positions: 包含industry列的持仓DataFrame
        
    Returns:
        行业集中度指标
    """
    industry_weights = positions.groupby('industry')['weight'].sum().sort_values(ascending=False)
    
    metrics = {}
    
    # 1. 最大行业占比
    metrics['max_industry_weight'] = industry_weights.iloc[0]
    
    # 2. 前3大行业占比
    metrics['top3_industry_concentration'] = industry_weights.head(3).sum()
    
    # 3. 行业HHI
    metrics['industry_hhi'] = (industry_weights ** 2).sum()
    
    # 4. 有效行业数量
    metrics['effective_n_industries'] = 1 / metrics['industry_hhi']
    
    # 5. 行业分布熵
    metrics['industry_entropy'] = -np.sum(industry_weights * np.log(industry_weights + 1e-10))
    
    return metrics

# 添加行业数据
industries = ['银行', '非银金融', '医药生物', '电子', '食品饮料', 
              '电力设备', '汽车', '计算机', '房地产', '化工'] * 5
positions['industry'] = industries[:n_stocks]

industry_conc = calculate_industry_concentration(positions)
print("\n行业集中度指标:")
for key, value in industry_conc.items():
    print(f"  {key}: {value:.4f}")
```

### 2.2 集中度风险控制

#### 2.2.1 集中度限制规则

```python
class ConcentrationLimits:
    """
    集中度限制管理
    """
    
    def __init__(self):
        # 默认限制参数
        self.limits = {
            'max_single_stock': 0.10,      # 单只股票最大10%
            'max_single_industry': 0.25,   # 单一行业最大25%
            'min_n_stocks': 20,            # 最少持仓数量
            'max_hhi': 0.05,               # HHI上限
            'max_top5_concentration': 0.40, # 前5大持仓上限
            'max_top10_concentration': 0.60 # 前10大持仓上限
        }
    
    def check_concentration(self, positions: pd.DataFrame) -> dict:
        """
        检查集中度是否超限
        
        Returns:
            检查结果字典
        """
        stock_conc = calculate_stock_concentration(positions)
        industry_conc = calculate_industry_concentration(positions)
        
        violations = []
        
        # 检查个股集中度
        if stock_conc['max_single_position'] > self.limits['max_single_stock']:
            violations.append({
                'type': '个股集中度',
                'metric': 'max_single_position',
                'value': stock_conc['max_single_position'],
                'limit': self.limits['max_single_stock'],
                'severity': 'high' if stock_conc['max_single_position'] > 0.15 else 'medium'
            })
        
        if stock_conc['top5_concentration'] > self.limits['max_top5_concentration']:
            violations.append({
                'type': '前5大持仓集中度',
                'metric': 'top5_concentration',
                'value': stock_conc['top5_concentration'],
                'limit': self.limits['max_top5_concentration'],
                'severity': 'medium'
            })
        
        if stock_conc['hhi'] > self.limits['max_hhi']:
            violations.append({
                'type': 'HHI集中度',
                'metric': 'hhi',
                'value': stock_conc['hhi'],
                'limit': self.limits['max_hhi'],
                'severity': 'medium'
            })
        
        # 检查行业集中度
        if industry_conc['max_industry_weight'] > self.limits['max_single_industry']:
            violations.append({
                'type': '行业集中度',
                'metric': 'max_industry_weight',
                'value': industry_conc['max_industry_weight'],
                'limit': self.limits['max_single_industry'],
                'severity': 'high' if industry_conc['max_industry_weight'] > 0.35 else 'medium'
            })
        
        # 检查持仓数量
        if len(positions) < self.limits['min_n_stocks']:
            violations.append({
                'type': '持仓数量',
                'metric': 'n_stocks',
                'value': len(positions),
                'limit': self.limits['min_n_stocks'],
                'severity': 'low'
            })
        
        return {
            'triggers_activated': triggers_activated,
            'recommended_actions': recommended_actions,
            'requires_immediate_action': any(t['level'] == 'high' for t in triggers_activated)
        }

# 使用示例
# contingency = RiskContingencyPlan()
# portfolio_state = {
#     'max_single_position': 0.18,
#     'liquidity_score': 35,
#     'current_drawdown': 0.12
# }
# response = contingency.monitor_and_trigger(portfolio_state)
```

## 五、风险监控仪表板

```python
def create_risk_dashboard(positions: pd.DataFrame, 
                         liquidity_data: dict,
                         historical_returns: pd.Series):
    """
    创建风险监控仪表板
    """
    import matplotlib.pyplot as plt
    
    fig = plt.figure(figsize=(16, 10))
    gs = fig.add_gridspec(2, 3, hspace=0.3, wspace=0.3)
    
    # 1. 持仓集中度分布
    ax1 = fig.add_subplot(gs[0, 0])
    weights = positions['weight'].sort_values(ascending=False)
    ax1.bar(range(len(weights)), weights * 100)
    ax1.axhline(y=10, color='red', linestyle='--', label='10%限制')
    ax1.set_xlabel('持仓排名')
    ax1.set_ylabel('权重(%)')
    ax1.set_title('持仓集中度分布')
    ax1.legend()
    
    # 2. 行业分布
    ax2 = fig.add_subplot(gs[0, 1])
    industry_weights = positions.groupby('industry')['weight'].sum().sort_values(ascending=False)
    ax2.pie(industry_weights, labels=industry_weights.index, autopct='%1.1f%%')
    ax2.set_title('行业分布')
    
    # 3. 流动性分布
    ax3 = fig.add_subplot(gs[0, 2])
    liquidity_scores = [calculate_stock_liquidity_score(liquidity_data.get(code, ohlcv))['total_score'] 
                       for code in positions['stock_code']]
    colors = ['green' if s >= 60 else 'yellow' if s >= 40 else 'red' for s in liquidity_scores]
    ax3.scatter(positions['weight'] * 100, liquidity_scores, c=colors, alpha=0.6)
    ax3.axhline(y=40, color='red', linestyle='--', alpha=0.5)
    ax3.set_xlabel('持仓权重(%)')
    ax3.set_ylabel('流动性评分')
    ax3.set_title('持仓流动性分布')
    
    # 4. 回撤曲线
    ax4 = fig.add_subplot(gs[1, :2])
    cumulative = (1 + historical_returns).cumprod()
    running_max = cumulative.expanding().max()
    drawdown = (cumulative - running_max) / running_max
    ax4.fill_between(drawdown.index, drawdown * 100, 0, alpha=0.5, color='red')
    ax4.axhline(y=-10, color='orange', linestyle='--', label='10%回撤线')
    ax4.set_ylabel('回撤(%)')
    ax4.set_title('历史回撤')
    ax4.legend()
    
    # 5. 风险指标汇总
    ax5 = fig.add_subplot(gs[1, 2])
    ax5.axis('off')
    
    # 计算关键指标
    conc = calculate_stock_concentration(positions)
    ind_conc = calculate_industry_concentration(positions)
    
    risk_summary = f"""
    风险指标汇总
    
    集中度风险:
    - 最大持仓: {conc['max_single_position']:.1%}
    - 前5大持仓: {conc['top5_concentration']:.1%}
    - HHI指数: {conc['hhi']:.4f}
    
    行业集中度:
    - 最大行业: {ind_conc['max_industry_weight']:.1%}
    - 前3大行业: {ind_conc['top3_industry_concentration']:.1%}
    
    当前回撤: {drawdown.iloc[-1]:.2%}
    最大回撤: {drawdown.min():.2%}
    """
    
    ax5.text(0.1, 0.9, risk_summary, transform=ax5.transAxes, 
            fontsize=10, verticalalignment='top', fontfamily='monospace')
    
    plt.savefig('risk_dashboard.png', dpi=150, bbox_inches='tight')
    plt.close()
```

## 六、最佳实践与建议

### 6.1 集中度管理最佳实践

| 实践要点 | 具体措施 | 实施频率 |
|----------|----------|----------|
| 定期监控 | 每日检查集中度指标 | 每日 |
| 动态调整 | 根据市场波动调整限制 | 每周 |
| 压力测试 | 模拟极端情况下的集中度风险 | 每月 |
| 行业分散 | 避免过度集中于单一行业 | 持续 |
| 个股限制 | 严格执行单只持仓上限 | 持续 |

### 6.2 流动性管理最佳实践

| 实践要点 | 具体措施 | 实施频率 |
|----------|----------|----------|
| 流动性评估 | 定期评估持仓流动性 | 每周 |
| 变现能力测试 | 测试不同时间尺度的变现能力 | 每月 |
| 低流动性限制 | 限制低流动性资产占比 | 持续 |
| 应急预案 | 制定流动性危机应对预案 | 每季度更新 |
| 压力测试 | 模拟流动性枯竭场景 | 每季度 |

### 6.3 风险管理的常见误区

1. **忽视集中度与流动性的关联**：高集中度会增加流动性风险
2. **静态的风险限制**：风险限制应根据市场环境动态调整
3. **过度分散**：过度分散可能导致收益平庸，需在集中与分散间平衡
4. **忽视尾部风险**：常规风险指标可能低估极端情况下的损失
5. **缺乏应急预案**：应提前制定各类风险事件的应对预案

## 七、总结

集中度风险与流动性风险是量化投资中不可忽视的两大风险类型：

1. **集中度管理**：通过HHI、基尼系数等指标量化集中度，设置合理的持仓限制
2. **流动性管理**：建立流动性评分体系，监控持仓变现能力
3. **压力测试**：定期进行集中度与流动性压力测试，评估极端情况下的风险暴露
4. **应急预案**：建立风险触发机制，制定明确的应对措施

通过系统化的集中度与流动性风险管理，可以有效控制组合风险，提升策略的稳健性。

---

*本文是DAO量化模型风险控制引擎系列第三篇，深入探讨了集中度风险与流动性风险的管理方法。*
is_compliant': len(violations) == 0,
            'violations': violations,
            'stock_concentration': stock_conc,
            'industry_concentration': industry_conc
        }
    
    def adjust_for_concentration(self, target_weights: pd.Series,
                                 current_prices: pd.Series) -> pd.Series:
        """
        根据集中度限制调整目标权重
        
        使用迭代方法逐步降低超限持仓
        """
        adjusted_weights = target_weights.copy()
        max_iterations = 100
        
        for _ in range(max_iterations):
            # 检查集中度
            positions = pd.DataFrame({
                'weight': adjusted_weights,
                'industry': [industries[i % len(industries)] for i in range(len(adjusted_weights))]
            })
            
            check_result = self.check_concentration(positions)
            
            if check_result['is_compliant']:
                break
            
            # 调整超限持仓
            for violation in check_result['violations']:
                if violation['metric'] == 'max_single_position':
                    # 降低最大持仓
                    max_idx = adjusted_weights.idxmax()
                    excess = adjusted_weights[max_idx] - self.limits['max_single_stock']
                    adjusted_weights[max_idx] -= excess
                    
                    # 将多余权重分配给其他持仓
                    other_stocks = adjusted_weights.index != max_idx
                    if other_stocks.sum() > 0:
                        adjusted_weights[other_stocks] += excess / other_stocks.sum()
        
        # 归一化
        adjusted_weights = adjusted_weights / adjusted_weights.sum()
        
        return adjusted_weights

# 使用示例
limits = ConcentrationLimits()
check_result = limits.check_concentration(positions)
print(f"\n集中度检查结果: {'通过' if check_result['is_compliant'] else '未通过'}")
if not check_result['is_compliant']:
    print("违规项:")
    for v in check_result['violations']:
        print(f"  - {v['type']}: {v['value']:.2%} > {v['limit']:.2%} ({v['severity']})")
```

#### 2.2.2 动态集中度管理

```python
class DynamicConcentrationManager:
    """
    动态集中度管理器
    
    根据市场波动性和组合表现动态调整集中度限制
    """
    
    def __init__(self, base_limits: dict):
        self.base_limits = base_limits
        self.current_limits = base_limits.copy()
        self.volatility_regime = 'normal'  # normal, high, extreme
    
    def update_limits_by_volatility(self, market_volatility: float):
        """
        根据市场波动率调整集中度限制
        
        Args:
            market_volatility: 市场波动率（年化）
        """
        if market_volatility < 0.15:
            self.volatility_regime = 'low'
            multiplier = 1.2  # 低波动可适度提高集中度
        elif market_volatility < 0.25:
            self.volatility_regime = 'normal'
            multiplier = 1.0
        elif market_volatility < 0.35:
            self.volatility_regime = 'high'
            multiplier = 0.8  # 高波动降低集中度
        else:
            self.volatility_regime = 'extreme'
            multiplier = 0.6  # 极端波动严格控制
        
        self.current_limits = {
            key: value * multiplier 
            for key, value in self.base_limits.items()
        }
    
    def get_position_sizing_factor(self, stock_volatility: float,
                                   correlation_with_portfolio: float) -> float:
        """
        根据个股风险特征计算仓位调整因子
        
        高波动、高相关性的股票应降低仓位
        """
        # 波动率调整
        vol_factor = 0.15 / max(stock_volatility, 0.10)
        
        # 相关性调整
        corr_factor = 1 - correlation_with_portfolio
        
        # 综合调整因子
        sizing_factor = min(vol_factor * corr_factor, 1.5)
        
        return max(sizing_factor, 0.3)  # 最低0.3倍
```

## 三、流动性风险管理

### 3.1 流动性指标度量

#### 3.1.1 市场流动性指标

```python
def calculate_market_liquidity_metrics(ohlcv_data: pd.DataFrame) -> pd.DataFrame:
    """
    计算市场流动性指标
    
    Args:
        ohllcv_data: 包含open, high, low, close, volume的DataFrame
        
    Returns:
        流动性指标DataFrame
    """
    df = ohlcv_data.copy()
    liquidity = pd.DataFrame(index=df.index)
    
    # 1. 买卖价差（使用高低价估算）
    liquidity['spread'] = (df['high'] - df['low']) / df['close']
    
    # 2. Amihud非流动性指标
    liquidity['amihud'] = np.abs(df['close'].pct_change()) / (df['volume'] * df['close'])
    liquidity['amihud'] = liquidity['amihud'].replace([np.inf, -np.inf], np.nan)
    
    # 3. 换手率
    # 假设流通股本为固定值，实际应用中需要真实数据
    shares_outstanding = df['volume'].rolling(window=252).max() * 10  # 估算
    liquidity['turnover'] = df['volume'] / shares_outstanding
    
    # 4. 成交量波动率
    liquidity['volume_volatility'] = df['volume'].pct_change().rolling(window=20).std()
    
    # 5. 价格冲击成本（简化计算）
    liquidity['price_impact'] = np.abs(df['close'].pct_change()) / np.sqrt(df['volume'])
    
    # 6. 市场深度指标（使用成交量分布）
    liquidity['market_depth'] = df['volume'] / liquidity['spread'].replace(0, np.nan)
    
    return liquidity

# 示例数据
dates = pd.date_range('2024-01-01', periods=60, freq='B')
np.random.seed(42)
ohlcv = pd.DataFrame({
    'open': 100 + np.cumsum(np.random.normal(0, 1, 60)),
    'high': 0,
    'low': 0,
    'close': 0,
    'volume': np.random.randint(1000000, 10000000, 60)
}, index=dates)

ohlcv['high'] = ohlcv['open'] + np.abs(np.random.normal(1, 0.5, 60))
ohlcv['low'] = ohlcv['open'] - np.abs(np.random.normal(1, 0.5, 60))
ohlcv['close'] = (ohlcv['open'] + ohlcv['high'] + ohlcv['low']) / 3 + np.random.normal(0, 0.3, 60)

liquidity_metrics = calculate_market_liquidity_metrics(ohlcv)
print("流动性指标:")
print(liquidity_metrics[['spread', 'turnover', 'volume_volatility']].tail())
```

#### 3.1.2 个股流动性评分

```python
def calculate_stock_liquidity_score(trading_data: pd.DataFrame,
                                   lookback_period: int = 20) -> dict:
    """
    计算个股流动性综合评分
    
    评分范围0-100，分数越高流动性越好
    """
    recent_data = trading_data.tail(lookback_period)
    
    scores = {}
    
    # 1. 成交量评分（占30%）
    avg_volume = recent_data['volume'].mean()
    volume_percentile = min(avg_volume / 10000000, 1.0)  # 以1000万为满分基准
    scores['volume_score'] = volume_percentile * 100
    
    # 2. 换手率评分（占25%）
    avg_turnover = recent_data['volume'].mean() / recent_data['volume'].max() / 10
    scores['turnover_score'] = min(avg_turnover * 100, 100)
    
    # 3. 价差评分（占25%）
    avg_spread = (recent_data['high'] - recent_data['low']).mean() / recent_data['close'].mean()
    spread_score = max(0, 100 - avg_spread * 10000)  # 价差越小分越高
    scores['spread_score'] = spread_score
    
    # 4. 成交量稳定性评分（占20%）
    volume_cv = recent_data['volume'].std() / recent_data['volume'].mean()
    stability_score = max(0, 100 - volume_cv * 100)
    scores['stability_score'] = stability_score
    
    # 综合评分
    scores['total_score'] = (
        scores['volume_score'] * 0.30 +
        scores['turnover_score'] * 0.25 +
        scores['spread_score'] * 0.25 +
        scores['stability_score'] * 0.20
    )
    
    # 流动性等级
    if scores['total_score'] >= 80:
        scores['liquidity_grade'] = 'A'
    elif scores['total_score'] >= 60:
        scores['liquidity_grade'] = 'B'
    elif scores['total_score'] >= 40:
        scores['liquidity_grade'] = 'C'
    else:
        scores['liquidity_grade'] = 'D'
    
    return scores

liquidity_score = calculate_stock_liquidity_score(ohlcv)
print("\n个股流动性评分:")
for key, value in liquidity_score.items():
    if isinstance(value, float):
        print(f"  {key}: {value:.2f}")
    else:
        print(f"  {key}: {value}")
```

### 3.2 流动性风险控制

#### 3.2.1 持仓流动性评估

```python
def assess_portfolio_liquidity(positions: pd.DataFrame,
                               liquidity_data: dict) -> dict:
    """
    评估组合整体流动性
    
    Args:
        positions: 持仓DataFrame
        liquidity_data: 各股票流动性数据的字典 {stock_code: liquidity_metrics}
        
    Returns:
        组合流动性评估结果
    """
    assessment = {}
    
    # 1. 加权平均流动性评分
    total_weight = positions['weight'].sum()
    weighted_liquidity = 0
    
    for _, row in positions.iterrows():
        code = row['stock_code']
        weight = row['weight'] / total_weight
        if code in liquidity_data:
            score = calculate_stock_liquidity_score(liquidity_data[code])['total_score']
            weighted_liquidity += score * weight
    
    assessment['weighted_liquidity_score'] = weighted_liquidity
    
    # 2. 流动性最差持仓占比
    low_liquidity_weight = 0
    for _, row in positions.iterrows():
        code = row['stock_code']
        if code in liquidity_data:
            score = calculate_stock_liquidity_score(liquidity_data[code])['total_score']
            if score < 40:  # D级流动性
                low_liquidity_weight += row['weight']
    
    assessment['low_liquidity_weight'] = low_liquidity_weight
    
    # 3. 变现能力评估
    portfolio_value = positions['market_value'].sum()
    
    # 估算不同时间内的可变现金额
    for days in [1, 3, 5, 10]:
        realizable = 0
        for _, row in positions.iterrows():
            code = row['stock_code']
            value = row['market_value']
            if code in liquidity_data:
                avg_daily_volume = liquidity_data[code]['volume'].mean()
                max_sellable = avg_daily_volume * days * row['close'] * 0.1  # 假设占日成交10%
                realizable += min(value, max_sellable)
        
        assessment[f'realizable_in_{days}d'] = realizable
        assessment[f'realizable_ratio_{days}d'] = realizable / portfolio_value
    
    return assessment
```

#### 3.2.2 流动性限制与预警

```python
class LiquidityRiskManager:
    """
    流动性风险管理器
    """
    
    def __init__(self):
        self.thresholds = {
            'min_liquidity_score': 40,      # 最低流动性评分
            'max_low_liquidity_weight': 0.20, # 低流动性持仓最大占比
            'min_realizable_1d': 0.30,       # 1天可变现比例
            'min_realizable_3d': 0.60,       # 3天可变现比例
            'max_position_illiquid': 0.05    # 单只低流动性股票最大持仓
        }
    
    def check_liquidity_risk(self, positions: pd.DataFrame,
                            liquidity_scores: dict) -> dict:
        """
        检查流动性风险
        """
        alerts = []
        
        # 1. 检查个股流动性
        for _, row in positions.iterrows():
            code = row['stock_code']
            if code in liquidity_scores:
                score = liquidity_scores[code]['total_score']
                if score < self.thresholds['min_liquidity_score']:
                    if row['weight'] > self.thresholds['max_position_illiquid']:
                        alerts.append({
                            'type': '个股流动性风险',
                            'stock': code,
                            'liquidity_score': score,
                            'position_weight': row['weight'],
                            'severity': 'high' if row['weight'] > 0.10 else 'medium'
                        })
        
        # 2. 检查组合流动性
        total_low_liquidity = sum(
            row['weight'] for _, row in positions.iterrows()
            if row['stock_code'] in liquidity_scores and
            liquidity_scores[row['stock_code']]['total_score'] < 40
        )
        
        if total_low_liquidity > self.thresholds['max_low_liquidity_weight']:
            alerts.append({
                'type': '组合流动性风险',
                'low_liquidity_weight': total_low_liquidity,
                'threshold': self.thresholds['max_low_liquidity_weight'],
                'severity': 'high'
            })
        
        return {
            'has_risk': len(alerts) > 0,
            'alerts': alerts
        }
    
    def calculate_liquidation_path(self, positions: pd.DataFrame,
                                  liquidity_data: dict,
                                  target_cash: float) -> pd.DataFrame:
        """
        计算平仓路径
        
        在流动性约束下，规划最优平仓顺序
        """
        liquidation_plan = []
        remaining_cash_needed = target_cash
        
        # 按流动性排序（流动性好的优先卖出）
        positions_with_liquidity = positions.copy()
        positions_with_liquidity['liquidity_score'] = positions_with_liquidity['stock_code'].map(
            lambda x: liquidity_data.get(x, {}).get('total_score', 0)
        )
        sorted_positions = positions_with_liquidity.sort_values('liquidity_score', ascending=False)
        
        for _, row in sorted_positions.iterrows():
            if remaining_cash_needed <= 0:
                break
            
            code = row['stock_code']
            position_value = row['market_value']
            
            # 估算该股票可卖出金额（考虑流动性约束）
            if code in liquidity_data:
                avg_daily_volume = liquidity_data[code]['volume'].mean()
                daily_sellable = avg_daily_volume * row['close'] * 0.1  # 日成交10%
                
                # 假设需要3天完成
                max_sellable = daily_sellable * 3
                sell_amount = min(position_value, max_sellable, remaining_cash_needed)
                
                liquidation_plan.append({
                    'stock_code': code,
                    'position_value': position_value,
                    'sell_amount': sell_amount,
                    'sell_ratio': sell_amount / position_value if position_value > 0 else 0,
                    'estimated_days': np.ceil(sell_amount / daily_sellable) if daily_sellable > 0 else 999
                })
                
                remaining_cash_needed -= sell_amount
        
        return pd.DataFrame(liquidation_plan)
```

## 四、压力测试与应急预案

### 4.1 集中度压力测试

```python
def concentration_stress_test(positions: pd.DataFrame,
                             scenarios: list) -> pd.DataFrame:
    """
    集中度压力测试
    
    测试极端情况下集中度风险的影响
    
    Args:
        positions: 持仓DataFrame
        scenarios: 压力测试场景列表
        
    Returns:
        压力测试结果
    """
    results = []
    
    for scenario in scenarios:
        scenario_result = {'scenario_name': scenario['name']}
        
        # 模拟各股票跌幅
        stressed_values = positions.copy()
        for _, row in positions.iterrows():
            stock_code = row['stock_code']
            
            # 根据场景应用不同跌幅
            if scenario['type'] == 'uniform':
                decline = scenario['decline']
            elif scenario['type'] == 'industry_specific':
                industry = row.get('industry', '其他')
                decline = scenario['industry_declines'].get(industry, 0.20)
            elif scenario['type'] == 'concentration_weighted':
                # 集中度越高跌幅越大
                weight = row['weight']
                decline = scenario['base_decline'] + (weight - 0.02) * 2
            else:
                decline = 0.20
            
            stressed_values.loc[stressed_values['stock_code'] == stock_code, 'stressed_value'] = \
                row['market_value'] * (1 - decline)
        
        # 计算组合损失
        original_value = positions['market_value'].sum()
        stressed_total = stressed_values['stressed_value'].sum()
        portfolio_loss = (original_value - stressed_total) / original_value
        
        scenario_result['portfolio_loss'] = portfolio_loss
        scenario_result['max_single_loss'] = (positions['market_value'] - 
                                              stressed_values['stressed_value']).max() / original_value
        
        # 集中度变化
        stressed_weights = stressed_values['stressed_value'] / stressed_total
        scenario_result['stressed_hhi'] = (stressed_weights ** 2).sum()
        
        results.append(scenario_result)
    
    return pd.DataFrame(results)

# 定义压力测试场景
stress_scenarios = [
    {
        'name': '市场普跌20%',
        'type': 'uniform',
        'decline': 0.20
    },
    {
        'name': '行业分化下跌',
        'type': 'industry_specific',
        'industry_declines': {
            '银行': 0.15,
            '非银金融': 0.25,
            '医药生物': 0.10,
            '电子': 0.30,
            '食品饮料': 0.12
        }
    },
    {
        'name': '集中度风险爆发',
        'type': 'concentration_weighted',
        'base_decline': 0.15
    }
]

stress_results = concentration_stress_test(positions, stress_scenarios)
print("\n集中度压力测试结果:")
print(stress_results.to_string(index=False))
```

### 4.2 流动性压力测试

```python
def liquidity_stress_test(positions: pd.DataFrame,
                         liquidity_data: dict,
                         scenarios: list) -> pd.DataFrame:
    """
    流动性压力测试
    
    测试市场流动性枯竭时的变现能力
    """
    results = []
    portfolio_value = positions['market_value'].sum()
    
    for scenario in scenarios:
        scenario_result = {'scenario_name': scenario['name']}
        
        # 流动性恶化倍数
        liquidity_deterioration = scenario['liquidity_deterioration']
        
        # 计算各场景下可变现金额
        for days in [1, 3, 5, 10]:
            realizable = 0
            
            for _, row in positions.iterrows():
                code = row['stock_code']
                value = row['market_value']
                
                if code in liquidity_data:
                    # 压力下的成交量
                    stressed_volume = liquidity_data[code]['volume'].mean() / liquidity_deterioration
                    max_sellable = stressed_volume * days * row.get('close', 1) * 0.05  # 更保守的5%
                    realizable += min(value, max_sellable)
            
            scenario_result[f'realizable_{days}d'] = realizable
            scenario_result[f'realizable_ratio_{days}d'] = realizable / portfolio_value
        
        results.append(scenario_result)
    
    return pd.DataFrame(results)

# 流动性压力测试场景
liquidity_scenarios = [
    {
        'name': '轻度流动性紧张',
        'liquidity_deterioration': 2  # 成交量减半
    },
    {
        'name': '中度流动性危机',
        'liquidity_deterioration': 5  # 成交量降至20%
    },
    {
        'name': '严重流动性枯竭',
        'liquidity_deterioration': 10  # 成交量降至10%
    }
]

# 添加close列
positions['close'] = 50 + np.random.normal(0, 10, len(positions))

liquidity_stress_results = liquidity_stress_test(positions, 
                                                 {f'SH{600000+i}': ohlcv for i in range(min(10, len(positions)))},
                                                 liquidity_scenarios)
print("\n流动性压力测试结果:")
print(liquidity_stress_results.to_string(index=False))
```

### 4.3 应急预案

```python
class RiskContingencyPlan:
    """
    风险应急预案
    """
    
    def __init__(self):
        self.triggers = {
            'concentration_breach': 0.15,    # 个股集中度超限
            'liquidity_crisis': 0.30,        # 流动性危机信号
            'drawdown_limit': 0.10,          # 回撤限制
            'volatility_spike': 0.30         # 波动率飙升
        }
        self.actions = {
            'reduce_position': 0.30,         # 减仓30%
            'hedge_ratio': 0.50,             # 对冲50%敞口
            'stop_trading': True             # 暂停交易
        }
    
    def monitor_and_trigger(self, portfolio_state: dict) -> dict:
        """
        监控风险指标并触发应急措施
        """
        triggers_activated = []
        recommended_actions = []
        
        # 检查集中度风险
        if portfolio_state.get('max_single_position', 0) > self.triggers['concentration_breach']:
            triggers_activated.append({
                'type': 'concentration',
                'level': 'high',
                'value': portfolio_state['max_single_position']
            })
            recommended_actions.append({
                'action': 'reduce_concentrated_positions',
                'target': portfolio_state['max_single_position'] * 0.5
            })
        
        # 检查流动性风险
        if portfolio_state.get('liquidity_score', 100) < 40:
            triggers_activated.append({
                'type': 'liquidity',
                'level': 'medium',
                'value': portfolio_state['liquidity_score']
            })
            recommended_actions.append({
                'action': 'increase_cash_buffer',
                'target': 0.20
            })
        
        # 检查回撤
        if portfolio_state.get('current_drawdown', 0) > self.triggers['drawdown_limit']:
            triggers_activated.append({
                'type': 'drawdown',
                'level': 'high',
                'value': portfolio_state['current_drawdown']
            })
            recommended_actions.append({
                'action': 'reduce_gross_exposure',
                'target': 0.70
            })
        
        return {
            '