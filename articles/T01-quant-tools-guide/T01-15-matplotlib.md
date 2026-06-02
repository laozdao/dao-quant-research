# matplotlib 经典可视化 -- 量化交易的数据可视化基石

## 概述

matplotlib 是 Python 数据可视化领域的基石库，由 John D. Hunter 于 2003 年创建。它提供了全面的 2D（及部分 3D）绘图功能，能够生成出版级质量的图表。在量化交易领域，matplotlib 是策略分析、回测报告和研究成果展示的核心工具。从简单的收益曲线到复杂的 K 线图、回撤分析和相关性热力图，matplotlib 提供了灵活而强大的可视化能力，几乎所有 Python 量化框架（如 Backtrader、Zipline）都依赖 matplotlib 进行结果展示。

## 基本信息

| 项目 | 详情 |
|------|------|
| 名称 | matplotlib |
| 开发者 | John D. Hunter / matplotlib 社区 |
| 首次发布 | 2003年 |
| 当前版本 | 3.9+ |
| 许可证 | PSF License |
| 编程语言 | Python / C (后端) |
| 官网 | https://matplotlib.org |
| GitHub | https://github.com/matplotlib/matplotlib |
| 适用平台 | Linux / macOS / Windows |

## 核心特性

### 1. 策略收益曲线可视化

```python
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import numpy as np
import pandas as pd

def plot_equity_curve(dates, strategy_returns, benchmark_returns=None,
                      title='策略收益曲线', figsize=(14, 7)):
    '''绘制策略收益曲线

    Args:
        dates: 日期序列
        strategy_returns: 策略日收益率序列
        benchmark_returns: 基准日收益率序列(可选)
        title: 图表标题
        figsize: 图表大小
    '''
    fig, ax = plt.subplots(figsize=figsize)

    # 计算累计收益
    strategy_cumulative = (1 + pd.Series(strategy_returns)).cumprod()
    strategy_cumulative.index = dates

    # 绘制策略曲线
    ax.plot(strategy_cumulative.index, strategy_cumulative.values,
            label='策略', color='#2196F3', linewidth=1.5)

    # 绘制基准曲线
    if benchmark_returns is not None:
        benchmark_cumulative = (1 + pd.Series(benchmark_returns)).cumprod()
        benchmark_cumulative.index = dates
        ax.plot(benchmark_cumulative.index, benchmark_cumulative.values,
                label='基准 (沪深300)', color='#FF9800',
                linewidth=1.5, linestyle='--')

    # 标注关键指标
    total_return = strategy_cumulative.iloc[-1] - 1
    sharpe = np.mean(strategy_returns) / (np.std(strategy_returns) + 1e-8) * np.sqrt(252)
    ax.text(0.02, 0.95,
            f'累计收益: {total_return:.2%}\nSharpe: {sharpe:.2f}',
            transform=ax.transAxes, fontsize=11,
            verticalalignment='top',
            bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.5))

    ax.set_title(title, fontsize=14, fontweight='bold')
    ax.set_xlabel('日期', fontsize=12)
    ax.set_ylabel('累计净值', fontsize=12)
    ax.legend(fontsize=11)
    ax.grid(True, alpha=0.3)
    ax.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m'))
    ax.xaxis.set_major_locator(mdates.MonthLocator(interval=3))
    fig.autofmt_xdate()
    plt.tight_layout()
    plt.savefig('equity_curve.png', dpi=150, bbox_inches='tight')
    plt.show()
```

### 2. K 线图绘制

```python
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from matplotlib.patches import Rectangle
import pandas as pd
import numpy as np

def plot_candlestick(df, volume=True, ma_periods=[5, 20, 60],
                     title='K线图', figsize=(16, 9)):
    '''绘制专业K线图

    Args:
        df: 包含OHLCV数据的DataFrame
        volume: 是否显示成交量
        ma_periods: 均线周期列表
        title: 图表标题
        figsize: 图表大小
    '''
    if volume:
        fig, (ax1, ax2) = plt.subplots(
            2, 1, figsize=figsize,
            gridspec_kw={'height_ratios': [3, 1]},
            sharex=True
        )
    else:
        fig, ax1 = plt.subplots(figsize=figsize)

    # 绘制K线
    for idx, row in df.iterrows():
        date_num = mdates.date2num(idx)
        open_p, high_p, low_p, close_p = row['open'], row['high'], row['low'], row['close']

        # K线实体
        color = 'red' if close_p >= open_p else 'green'
        body_height = abs(close_p - open_p)
        body_bottom = min(open_p, close_p)

        rect = Rectangle(
            (date_num - 0.3, body_bottom), 0.6, body_height,
            facecolor=color, edgecolor=color
        )
        ax1.add_patch(rect)

        # 上下影线
        ax1.plot([date_num, date_num], [low_p, high_p],
                 color=color, linewidth=0.8)

    # 绘制均线
    colors = ['#FF6B6B', '#4ECDC4', '#45B7D1']
    for period, color in zip(ma_periods, colors):
        if len(df) >= period:
            ma = df['close'].rolling(period).mean()
            ax1.plot(ma.index, ma.values,
                     label=f'MA{period}', color=color,
                     linewidth=1, alpha=0.8)

    # 绘制成交量
    if volume:
        for idx, row in df.iterrows():
            date_num = mdates.date2num(idx)
            color = 'red' if row['close'] >= row['open'] else 'green'
            ax2.bar(date_num, row['volume'],
                    width=0.6, color=color, alpha=0.6)
        ax2.set_ylabel('成交量', fontsize=11)
        ax2.grid(True, alpha=0.3)

    ax1.set_title(title, fontsize=14, fontweight='bold')
    ax1.set_ylabel('价格', fontsize=12)
    ax1.legend(fontsize=10)
    ax1.grid(True, alpha=0.3)
    ax1.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d'))
    ax1.xaxis.set_major_locator(mdates.WeekdayLocator(interval=2))
    fig.autofmt_xdate()
    plt.tight_layout()
    plt.savefig('candlestick.png', dpi=150, bbox_inches='tight')
    plt.show()
```

### 3. 回撤分析图

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

def plot_drawdown_analysis(dates, returns, figsize=(14, 6)):
    '''绘制回撤分析图

    Args:
        dates: 日期序列
        returns: 日收益率序列
        figsize: 图表大小
    '''
    cumulative = (1 + pd.Series(returns)).cumprod()
    running_max = cumulative.cummax()
    drawdown = (cumulative - running_max) / running_max

    fig, (ax1, ax2) = plt.subplots(
        2, 1, figsize=figsize,
        gridspec_kw={'height_ratios': [2, 1]},
        sharex=True
    )

    # 累计净值曲线
    ax1.plot(dates, cumulative.values, color='#2196F3', linewidth=1.2)
    ax1.fill_between(dates, cumulative.values, running_max.values,
                     where=(cumulative.values < running_max.values),
                     color='red', alpha=0.15)
    ax1.set_ylabel('累计净值', fontsize=12)
    ax1.set_title('回撤分析', fontsize=14, fontweight='bold')
    ax1.grid(True, alpha=0.3)

    # 回撤曲线
    ax2.fill_between(dates, drawdown.values, 0,
                     color='red', alpha=0.4)
    ax2.plot(dates, drawdown.values, color='red', linewidth=0.8)
    ax2.set_ylabel('回撤幅度', fontsize=12)
    ax2.set_xlabel('日期', fontsize=12)
    ax2.grid(True, alpha=0.3)

    # 标注最大回撤
    max_dd_idx = drawdown.idxmin()
    max_dd = drawdown.min()
    ax2.annotate(f'最大回撤: {max_dd:.2%}\n{max_dd_idx.strftime("%Y-%m-%d")}',
                 xy=(max_dd_idx, max_dd),
                 xytext=(max_dd_idx, max_dd - 0.05),
                 arrowprops=dict(arrowstyle='->', color='darkred'),
                 fontsize=10, color='darkred',
                 bbox=dict(boxstyle='round,pad=0.3', facecolor='lightyellow'))

    plt.tight_layout()
    plt.savefig('drawdown.png', dpi=150, bbox_inches='tight')
    plt.show()
```

### 4. 相关性热力图

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

def plot_correlation_heatmap(returns_df, method='pearson',
                              title='资产相关性热力图',
                              figsize=(12, 10)):
    '''绘制相关性热力图

    Args:
        returns_df: 收益率DataFrame (各列为不同资产)
        method: 相关系数计算方法 ('pearson', 'spearman', 'kendall')
        title: 图表标题
        figsize: 图表大小
    '''
    corr_matrix = returns_df.corr(method=method)

    fig, ax = plt.subplots(figsize=figsize)

    # 绘制热力图
    im = ax.imshow(corr_matrix.values, cmap='RdYlBu_r',
                   vmin=-1, vmax=1, aspect='auto')

    # 添加颜色条
    cbar = plt.colorbar(im, ax=ax, shrink=0.8)
    cbar.set_label('相关系数', fontsize=11)

    # 设置坐标轴
    labels = corr_matrix.columns
    ax.set_xticks(range(len(labels)))
    ax.set_yticks(range(len(labels)))
    ax.set_xticklabels(labels, rotation=45, ha='right', fontsize=10)
    ax.set_yticklabels(labels, fontsize=10)

    # 在每个格子中标注数值
    for i in range(len(labels)):
        for j in range(len(labels)):
            value = corr_matrix.values[i, j]
            color = 'white' if abs(value) > 0.6 else 'black'
            ax.text(j, i, f'{value:.2f}', ha='center', va='center',
                    color=color, fontsize=9)

    ax.set_title(title, fontsize=14, fontweight='bold')
    plt.tight_layout()
    plt.savefig('correlation_heatmap.png', dpi=150, bbox_inches='tight')
    plt.show()
```

## 应用场景

### 策略回测报告
- 累计收益曲线与基准对比
- 年化收益率、Sharpe Ratio 等指标展示
- 月度/年度收益热力图

### 技术分析图表
- K 线图与成交量
- 技术指标叠加（均线、布林带、MACD）
- 买卖信号标注

### 风险分析可视化
- 回撤分析图
- 收益分布直方图
- VaR/CVaR 可视化

### 研究报告图表
- 因子相关性分析
- 资产配置饼图
- 散点图和回归分析

## 优缺点分析

### 优点
- **功能全面**：几乎支持所有 2D 图表类型
- **高度可定制**：每个元素都可精细控制
- **出版质量**：输出图表质量高，适合论文和报告
- **生态丰富**：seaborn、mplfinance 等扩展库
- **稳定性高**：成熟稳定，API 变化少
- **后端灵活**：支持多种渲染后端（Agg、Qt、Tkinter）

### 缺点
- **非交互式**：默认生成静态图片，交互能力弱
- **API 繁琐**：部分复杂图表代码量大
- **大数据性能**：渲染大量数据点时较慢
- **3D 能力弱**：3D 绑图功能有限
- **样式配置**：默认样式不够美观，需要手动调整

## 与其他工具对比

| 特性 | matplotlib | Plotly | seaborn | mplfinance |
|------|-----------|--------|---------|------------|
| 交互性 | 静态 | 交互式 | 静态 | 静态 |
| K线图 | 需手动实现 | 内置 | 不支持 | 内置 |
| 热力图 | 支持 | 支持 | 优秀 | 不支持 |
| 性能 | 中 | 高 | 中 | 中 |
| 易用性 | 中 | 高 | 高 | 高 |
| 定制性 | 极高 | 高 | 中 | 中 |

## 推荐资源

### 官方资源
- matplotlib 文档：https://matplotlib.org/stable/contents.html
- matplotlib 画廊：https://matplotlib.org/stable/gallery/index.html
- mplfinance：https://github.com/matplotlib/mplfinance

### 扩展工具
- **seaborn**：https://seaborn.pydata.org/ - 统计可视化
- **mplfinance**：https://github.com/matplotlib/mplfinance - 金融图表
- **adjustText**：自动调整文本标签位置

## 总结

matplotlib 作为 Python 可视化的基石，在量化交易中承担着策略分析、回测报告和研究成果展示的核心角色。虽然它缺乏交互性且部分图表实现较为繁琐，但其全面的功能、高度的可定制性和出版级的输出质量使其在量化领域不可替代。建议搭配 mplfinance 简化金融图表绘制，使用 seaborn 增强统计图表的美观度，同时将复杂交互需求交给 Plotly 处理。
