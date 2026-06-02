# Interactive Brokers API -- 专业级量化交易网关

## 概述

Interactive Brokers（盈透证券，简称 IB）是全球最大的在线经纪商之一，其 API 提供了直接接入全球 150+ 个市场的交易通道。IB API 是专业量化交易者和机构投资者构建自动化交易系统的首选接口，支持股票、期权、期货、外汇、债券等多种资产类别。通过 Python 的 `ib_insync` 库，开发者可以方便地连接 TWS（Trader Workstation）或 IB Gateway，实现实时行情获取、订单管理和账户监控。

## 基本信息

| 项目 | 详情 |
|------|------|
| 名称 | Interactive Brokers API |
| 开发者 | Interactive Brokers LLC |
| 首次发布 | 2000年代 |
| 当前版本 | TWS API 10.19+ / ib_insync 0.9.86+ |
| 许可证 | 商业使用（需IB账户） |
| 编程语言 | Python / Java / C++ / C# |
| 官网 | https://www.interactivebrokers.com |
| API 文档 | https://interactivebrokers.github.io/tws-api/ |
| Python 库 | https://github.com/erdewit/ib_insync |
| 适用平台 | Linux / macOS / Windows |

## 核心特性

### 1. ib_insync 使用

`ib_insync` 是 IB API 的 Python 异步封装库，大幅简化了与 TWS/IB Gateway 的交互。

```python
from ib_insync import *

def connect_ib(host='127.0.0.1', port=7497, client_id=1):
    '''连接Interactive Brokers

    Args:
        host: TWS/Gateway地址
        port: 端口 (7497=模拟, 7496=实盘)
        client_id: 客户端ID

    Returns:
        IB连接实例
    '''
    ib = IB()
    ib.connect(host, port, clientId=client_id)
    print(f'已连接到IB，账户: {ib.accountValues()[0].account}')
    return ib


def fetch_market_data(ib, symbol='AAPL', exchange='SMART', currency='USD'):
    '''获取实时市场数据

    Args:
        ib: IB连接实例
        symbol: 股票代码
        exchange: 交易所
        currency: 货币

    Returns:
        Ticker对象
    '''
    contract = Stock(symbol, exchange, currency)
    ib.qualifyContracts(contract)

    ticker = ib.reqMktData(contract, '', False, False)
    ib.sleep(2)  # 等待数据到达

    return ticker


def get_account_summary(ib):
    '''获取账户摘要信息

    Args:
        ib: IB连接实例

    Returns:
        账户信息字典
    '''
    summary = ib.accountSummary()
    account_info = {}

    for item in summary:
        account_info[item.tag] = {
            'value': item.value,
            'currency': item.currency
        }

    return account_info


def get_positions(ib):
    '''获取当前持仓

    Args:
        ib: IB连接实例

    Returns:
        持仓列表
    '''
    positions = ib.positions()
    pos_list = []

    for pos in positions:
        pos_list.append({
            'symbol': pos.contract.symbol,
            'sec_type': pos.contract.secType,
            'exchange': pos.contract.exchange,
            'position': pos.position,
            'avg_cost': pos.avgCost,
            'currency': pos.contract.currency
        })

    return pos_list
```

### 2. 高级订单类型

IB API 支持多种高级订单类型，适用于复杂的量化交易策略。

```python
from ib_insync import *

class IBOrderManager:
    '''IB订单管理器

    封装各种订单类型的创建和管理逻辑。
    '''
    def __init__(self, ib):
        self.ib = ib

    def create_stock_contract(self, symbol, exchange='SMART', currency='USD'):
        '''创建股票合约'''
        contract = Stock(symbol, exchange, currency)
        self.ib.qualifyContracts(contract)
        return contract

    def market_order(self, contract, action, quantity):
        '''市价单

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 数量
        '''
        order = MarketOrder(action, quantity)
        trade = self.ib.placeOrder(contract, order)
        return trade

    def limit_order(self, contract, action, quantity, limit_price):
        '''限价单

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 数量
            limit_price: 限价
        '''
        order = LimitOrder(action, quantity, limit_price)
        trade = self.ib.placeOrder(contract, order)
        return trade

    def stop_order(self, contract, action, quantity, stop_price):
        '''止损单

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 数量
            stop_price: 止损价格
        '''
        order = StopOrder(action, quantity, stop_price)
        trade = self.ib.placeOrder(contract, order)
        return trade

    def bracket_order(self, contract, action, quantity,
                      limit_price, take_profit_price, stop_loss_price):
        '''括号订单（Brackets）

        同时设置止盈和止损，自动管理风险。

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 数量
            limit_price: 入场限价
            take_profit_price: 止盈价格
            stop_loss_price: 止损价格
        '''
        # 主订单
        parent = LimitOrder(action, quantity, limit_price)
        parent.orderId = self.ib.client.getReqId()
        parent.transmit = False

        # 止盈订单
        take_profit = LimitOrder('SELL' if action == 'BUY' else 'BUY',
                                  quantity, take_profit_price)
        take_profit.parentId = parent.orderId
        take_profit.transmit = False

        # 止损订单
        stop_loss = StopOrder('SELL' if action == 'BUY' else 'BUY',
                               quantity, stop_loss_price)
        stop_loss.parentId = parent.orderId
        stop_loss.transmit = True

        # 提交订单
        trades = []
        for order in [parent, take_profit, stop_loss]:
            trade = self.ib.placeOrder(contract, order)
            trades.append(trade)

        return trades

    def trailing_stop(self, contract, action, quantity,
                      trail_percent=2.0):
        '''追踪止损单

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 数量
            trail_percent: 追踪百分比
        '''
        order = TrailOrder(action, quantity,
                           trailingPercent=trail_percent)
        trade = self.ib.placeOrder(contract, order)
        return trade

    def iceberg_order(self, contract, action, quantity,
                      limit_price, display_size=None):
        '''冰山订单

        大单拆分显示，隐藏真实委托量。

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            quantity: 总数量
            limit_price: 限价
            display_size: 每次显示数量
        '''
        if display_size is None:
            display_size = max(1, quantity // 10)

        order = LimitOrder(action, quantity, limit_price)
        order.displaySize = display_size
        trade = self.ib.placeOrder(contract, order)
        return trade

    def twap_order(self, contract, action, total_quantity,
                   limit_price, duration_minutes=30, num_slices=6):
        '''TWAP（时间加权平均价格）订单

        将大单拆分为多个小单在时间上均匀执行。

        Args:
            contract: 合约对象
            action: 'BUY' 或 'SELL'
            total_quantity: 总数量
            limit_price: 限价
            duration_minutes: 执行总时长(分钟)
            num_slices: 拆分份数
        '''
        slice_quantity = total_quantity // num_slices
        interval = duration_minutes * 60 / num_slices

        trades = []
        for i in range(num_slices):
            order = LimitOrder(action, slice_quantity, limit_price)
            order.tif = 'GTC'
            order.outsideRth = True
            trade = self.ib.placeOrder(contract, order)
            trades.append(trade)

            if i < num_slices - 1:
                self.ib.sleep(interval)

        return trades
```

### 3. Client Portal API

Client Portal API 提供了通过 REST 接口访问 IB 账户的方式，适合 Web 应用和远程管理。

```python
import requests
import json

class IBClientPortal:
    '''IB Client Portal API客户端

    通过REST API管理IB账户，
    适合Web应用和远程监控。
    '''
    def __init__(self, host='https://localhost:5000'):
        self.host = host
        self.session = requests.Session()
        self.session.verify = False  # 开发环境跳过SSL验证

    def authenticate(self, username, password):
        '''认证登录

        Args:
            username: IB用户名
            password: IB密码
        '''
        url = f'{self.host}/api/iserver/auth/ssologin'
        data = {'username': username, 'password': password}
        response = self.session.post(url, json=data)
        return response.json()

    def get_accounts(self):
        '''获取所有账户'''
        url = f'{self.host}/api/iserver/accounts'
        response = self.session.get(url)
        return response.json()

    def get_positions(self, account_id):
        '''获取持仓

        Args:
            account_id: 账户ID
        '''
        url = f'{self.host}/api/iserver/account/{account_id}/positions'
        response = self.session.get(url)
        return response.json()

    def place_order(self, account_id, order_data):
        '''下单

        Args:
            account_id: 账户ID
            order_data: 订单数据
        '''
        url = f'{self.host}/api/iserver/account/{account_id}/orders'
        response = self.session.post(url, json=order_data)
        return response.json()

    def get_open_orders(self, account_id):
        '''获取未完成订单'''
        url = f'{self.host}/api/iserver/account/{account_id}/orders'
        response = self.session.get(url)
        return response.json()

    def get_portfolio_summary(self, account_id):
        '''获取投资组合摘要'''
        url = f'{self.host}/api/portfolio/{account_id}/summary'
        response = self.session.get(url)
        return response.json()
```

## 应用场景

### 自动化交易系统
- 策略信号自动下单执行
- 多资产组合动态再平衡
- 条件触发式交易（突破、均值回归等）

### 算法交易
- TWAP/VWAP 大单拆分执行
- 冰山订单隐藏交易意图
- 追踪止损动态管理风险

### 多市场套利
- 跨市场价差套利（股票/期货/期权）
- 统计套利策略执行
- 外汇三角套利

### 账户管理
- 实时持仓和盈亏监控
- 风险指标计算（VaR、保证金）
- 多账户统一管理

## 优缺点分析

### 优点
- **全球市场**：150+ 个市场，覆盖股票、期权、期货、外汇等
- **低佣金**：行业领先的低交易佣金
- **专业级API**：功能全面，支持复杂订单类型
- **模拟交易**：Paper Trading 环境免费使用
- **数据丰富**：实时行情、历史数据、基本面数据
- **ib_insync**：Python 异步封装，开发体验优秀

### 缺点
- **依赖 TWS**：需要运行 TWS 或 IB Gateway
- **API 限额**：请求数据有频率限制
- **文档分散**：官方文档不够友好
- **认证复杂**：双因素认证和SSL证书配置繁琐
- **最低资金**：部分账户类型有最低资金要求
- **延迟**：通过 TWS 中转存在一定延迟

## 与其他工具对比

| 特性 | IB API | Alpaca API | Tiger API |
|------|--------|-----------|-----------|
| 市场覆盖 | 全球150+ | 美股 | 美股+部分亚太 |
| 资产类别 | 全品类 | 股票+加密 | 股票+期权+期货 |
| API 类型 | TWS + REST | REST/WebSocket | REST/WebSocket |
| 模拟交易 | 免费 | 免费 | 免费 |
| 佣金 | 低 | 零佣金 | 低 |
| Python库 | ib_insync | alpaca-trade-api | openapi |
| 学习曲线 | 高 | 低 | 中 |

## 推荐资源

### 官方资源
- IB API 文档：https://interactivebrokers.github.io/tws-api/
- ib_insync 文档：https://ib-insync.readthedocs.io/
- TWS 下载：https://www.interactivebrokers.com/en/trading/tws.php

### 学习资源
- ib_insync GitHub：https://github.com/erdewit/ib_insync
- IB API Python 教程
- "Algorithmic Trading with Interactive Brokers" - Rui Qiu

## 总结

Interactive Brokers API 为量化交易者提供了接入全球金融市场的专业级通道。通过 ib_insync 的 Python 封装，开发者可以高效地构建自动化交易系统，利用 IB 丰富的市场数据和低佣金优势执行复杂的量化策略。虽然需要运行 TWS 作为中间层且学习曲线较陡，但 IB API 的功能全面性和全球市场覆盖使其成为专业量化交易者的首选方案。建议先用 Paper Trading 环境充分测试策略后再切换到实盘。
