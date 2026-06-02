# yfinance — 免费市场数据获取

> yfinance是获取免费金融市场数据的最受欢迎的Python库，为量化研究提供便捷的数据入口。

---

## 概述

yfinance是一个开源Python库，通过Yahoo Finance API获取全球金融市场数据。该库由Ran Aroussi创建并维护，支持股票、ETF、指数、货币对、加密货币等多种资产类型的历史价格、实时行情、财务报表、期权链等数据。对于量化交易初学者和个人研究者而言，yfinance是最便捷的免费数据获取方案。

yfinance的最大优势在于其零成本、易安装、API简洁。虽然Yahoo Finance的数据质量不如专业数据供应商（如Bloomberg、Wind），但对于策略研究、回测验证和学习实验而言已经足够。需要注意的是，yfinance依赖于非官方API，可能存在数据延迟、请求频率限制和不稳定性等问题。

---

## 基本信息表格

| 项目 | 详情 |
|------|------|
| 库名称 | yfinance |
| 最新稳定版 | yfinance 0.2.x |
| 创建者 | Ran Aroussi |
| 首次发布 | 2017年 |
| 开源协议 | Apache 2.0 |
| GitHub仓库 | https://github.com/ranaroussi/yfinance |
| 安装方式 | `pip install yfinance` |
| 数据源 | Yahoo Finance |
| 费用 | 免费 |
| 依赖库 | requests, pandas, html5lib |

---

## 核心特性

### 1. 历史价格数据获取

yfinance最核心的功能是获取历史OHLCV（开高低收量）数据：

```python
'''yfinance获取历史价格数据'''

import yfinance as yf
import pandas as pd

# 获取单只股票的历史数据
aapl = yf.download('AAPL', start='2023-01-01', end='2024-01-01')
print(f'AAPL数据形状: {aapl.shape}')
print(aapl.head())

# 获取多只股票的历史数据
tickers = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'TSLA']
multi_data = yf.download(tickers, start='2023-01-01', end='2024-01-01')
print(f'多股票数据形状: {multi_data.shape}')

# 指定数据频率
# 日线数据（默认）
daily = yf.download('SPY', period='1y', interval='1d')

# 周线数据
weekly = yf.download('SPY', period='1y', interval='1wk')

# 月线数据
monthly = yf.download('SPY', period='5y', interval='1mo')

# 分钟线数据（最近7天内的数据）
intraday = yf.download('AAPL', period='5d', interval='5m')

# 使用period参数快速获取
data_1m = yf.download('SPY', period='1mo')    # 最近1个月
data_3m = yf.download('SPY', period='3mo')    # 最近3个月
data_1y = yf.download('SPY', period='1y')     # 最近1年
data_max = yf.download('SPY', period='max')   # 全部历史数据

print(f'SPY全部历史数据: {len(data_max)} 条')
```

### 2. 财务数据获取

yfinance可以获取公司的财务报表和关键财务指标：

```python
'''yfinance获取财务数据'''

import yfinance as yf

# 创建Ticker对象
aapl = yf.Ticker('AAPL')

# 获取基本信息
info = aapl.info
print('公司基本信息:')
print(f'  公司名称: {info.get("longName")}')
print(f'  行业: {info.get("industry")}')
print(f'  市值: ${info.get("marketCap", 0):,.0f}')
print(f'  市盈率(PE): {info.get("trailingPE")}')
print(f'  前瞻PE: {info.get("forwardPE")}')
print(f'  股息率: {info.get("dividendYield", 0):.4f}')
print(f'  Beta: {info.get("beta")}')

# 获取利润表（季度/年度）
quarterly_income = aapl.quarterly_income_stmt
annual_income = aapl.income_stmt
print(f'\n最近季度收入: ${quarterly_income.iloc[0]["Total Revenue"]:,.0f}')

# 获取资产负债表
balance_sheet = aapl.balance_sheet
print(f'总资产: ${balance_sheet.iloc[0]["Total Assets"]:,.0f}')

# 获取现金流量表
cashflow = aapl.cashflow
print(f'经营现金流: ${cashflow.iloc[0]["Operating Cash Flow"]:,.0f}')

# 获取关键财务指标
financials = aapl.financials
print(f'\n最近4个财年的净利润:')
for col in financials.columns[:4]:
    date_str = col.strftime('%Y-%m-%d')
    net_income = financials.loc['Net Income', col]
    print(f'  {date_str}: ${net_income:,.0f}')
```

### 3. 期权链数据

yfinance支持获取股票和ETF的期权链数据：

```python
'''yfinance获取期权链数据'''

import yfinance as yf
import pandas as pd

# 创建Ticker对象
spy = yf.Ticker('SPY')

# 获取所有到期日的期权链
exp_dates = spy.options
print(f'可用的期权到期日数量: {len(exp_dates)}')
print(f'最近5个到期日: {exp_dates[:5]}')

# 获取指定到期日的期权链
opt_chain = spy.option_chain(exp_dates[0])

# 看涨期权数据
calls = opt_chain.calls
print(f'\n看涨期权合约数量: {len(calls)}')
print(f'看涨期权字段: {list(calls.columns)}')

# 看跌期权数据
puts = opt_chain.puts
print(f'看跌期权合约数量: {len(puts)}')

# 筛选平值附近的期权
current_price = spy.info['regularMarketPrice']
atm_calls = calls[
    (calls['strike'] >= current_price * 0.95) &
    (calls['strike'] <= current_price * 1.05)
]
print(f'\n平值附近看涨期权:')
print(atm_calls[['strike', 'lastPrice', 'bid', 'ask', 'volume', 'openInterest']].head(10))

# 计算隐含波动率
def analyze_option_chain(ticker, expiry_date):
    '''分析期权链的隐含波动率微笑
    
    参数:
        ticker: yfinance Ticker对象
        expiry_date: 到期日字符串
    返回:
        iv_skew: 隐含波动率偏斜数据
    '''
    chain = ticker.option_chain(expiry_date)
    calls = chain.calls
    
    iv_data = pd.DataFrame({
        'strike': calls['strike'],
        'impliedVolatility': calls['impliedVolatility'],
        'lastPrice': calls['lastPrice'],
        'volume': calls['volume'],
        'openInterest': calls['openInterest']
    })
    
    # 计算moneyness
    current_price = ticker.info['regularMarketPrice']
    iv_data['moneyness'] = iv_data['strike'] / current_price
    
    return iv_data

iv_data = analyze_option_chain(spy, exp_dates[0])
print(f'\n隐含波动率范围: {iv_data["impliedVolatility"].min():.4f} - {iv_data["impliedVolatility"].max():.4f}')
```

### 4. 批量数据获取与缓存

```python
'''批量获取数据与本地缓存策略'''

import yfinance as yf
import pandas as pd
import os
from datetime import datetime

def batch_download(tickers, start_date, end_date, cache_dir='./data_cache'):
    '''批量下载股票数据并缓存
    
    参数:
        tickers: 股票代码列表
        start_date: 开始日期
        end_date: 结束日期
        cache_dir: 缓存目录
    返回:
        all_data: 合并后的DataFrame
    '''
    os.makedirs(cache_dir, exist_ok=True)
    all_data = {}
    
    for ticker in tickers:
        cache_file = os.path.join(cache_dir, f'{ticker}.parquet')
        
        if os.path.exists(cache_file):
            # 从缓存读取
            data = pd.read_parquet(cache_file)
            print(f'{ticker}: 从缓存加载 ({len(data)} 条)')
        else:
            # 从Yahoo Finance下载
            try:
                data = yf.download(ticker, start=start_date, end=end_date)
                if len(data) > 0:
                    data.to_parquet(cache_file)
                    print(f'{ticker}: 下载完成 ({len(data)} 条)')
                else:
                    print(f'{ticker}: 无数据')
                    continue
            except Exception as e:
                print(f'{ticker}: 下载失败 - {e}')
                continue
        
        all_data[ticker] = data['Close']
    
    if all_data:
        result = pd.DataFrame(all_data)
        return result
    return None

# 使用示例
sp500_tickers = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'NVDA',
                'META', 'TSLA', 'BRK-B', 'JPM', 'V']
prices = batch_download(sp500_tickers, '2020-01-01', '2024-01-01')
if prices is not None:
    print(f'\n成功获取 {prices.shape[1]} 只股票, {prices.shape[0]} 个交易日')
    print(f'最新收盘价:\n{prices.iloc[-1]}')
```

---

## 应用场景

### 场景一：策略研究与回测

获取历史价格数据进行策略回测，是最基本也是最常用的场景。

### 场景二：因子研究

获取多只股票的历史数据用于因子计算和横截面分析。

### 场景三：期权策略研究

获取期权链数据用于期权定价验证、波动率分析和期权策略回测。

### 场景四：基本面分析

获取财务报表数据进行基本面因子研究和公司估值分析。

---

## 优缺点分析

### 优点

| 优点 | 说明 |
|------|------|
| 完全免费 | 无需付费即可获取大量市场数据 |
| 安装简单 | pip一键安装，无复杂配置 |
| API简洁 | 几行代码即可获取数据 |
| 资产覆盖广 | 支持全球股票、ETF、货币、加密货币 |
| 数据丰富 | 价格、财务、期权等多种数据类型 |

### 缺点

| 缺点 | 说明 |
|------|------|
| 数据质量 | 存在缺失值、前复权偏差等问题 |
| 稳定性差 | 依赖非官方API，可能随时失效 |
| 频率限制 | 大量请求可能被限流或封禁 |
| 分钟数据 | 仅支持最近7天的分钟级数据 |
| 无Level 2 | 不支持逐笔成交和订单簿数据 |

---

## 与其他数据源对比

| 特性 | yfinance | Tushare | AKShare | Wind | Bloomberg |
|------|----------|---------|---------|------|-----------|
| 费用 | 免费 | 免费/付费 | 免费 | 付费 | 付费 |
| A股支持 | 一般 | 极好 | 极好 | 极好 | 极好 |
| 美股支持 | 好 | 一般 | 一般 | 好 | 极好 |
| 数据质量 | 中等 | 好 | 中等 | 极好 | 极好 |
| 稳定性 | 中等 | 好 | 中等 | 极好 | 极好 |
| 实时数据 | 延迟 | 延迟 | 延迟 | 实时 | 实时 |

---

## 推荐资源

### 官方资源
- GitHub仓库：https://github.com/ranaroussi/yfinance
- PyPI页面：https://pypi.org/project/yfinance/

### 替代方案
- Tushare：https://tushare.pro（A股数据）
- AKShare：https://github.com/akfamily/akshare（中国金融数据）
- pandas-datareader：https://github.com/pydata/pandas-datareader

---

## 总结

yfinance是量化交易入门阶段最实用的数据获取工具。对于个人研究者和量化初学者，yfinance提供了零成本、低门槛的市场数据获取方案，足以支撑策略研究、回测验证和学习实验等需求。

然而，yfinance的数据质量和稳定性无法满足专业级量化交易的需求。在生产环境中，建议使用专业数据供应商（如Wind、Bloomberg、Refinitiv）或付费API服务。对于A股市场，Tushare和AKShare是更好的选择。在使用yfinance时，建议实现本地数据缓存机制，既减少网络请求，又提高数据加载速度，同时避免因API不稳定导致的数据获取失败。
