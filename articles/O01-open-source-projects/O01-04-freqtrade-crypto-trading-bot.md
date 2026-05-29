---
title: "Freqtrade：开源加密货币量化交易机器人"
date: "2026-05-27"
author: "laozdao"
category: "O01"
tags: ["开源项目", "Freqtrade", "加密货币", "量化交易", "交易机器人", "回测", "机器学习"]
status: "published"
version: "1.0"
summary: "深度解析Freqtrade——最流行的开源加密货币交易机器人，涵盖核心特性、策略开发、FreqAI机器学习模块及与Dao Quant模型的结合应用。"
difficulty: "intermediate"
reading_time: 35
series: "量化交易开源项目"
series_order: 4
---

# Freqtrade：开源加密货币量化交易机器人

> 从回测到实盘，从简单策略到机器学习，Freqtrade是加密货币量化交易者的完整解决方案。

---

## 一、项目概览

### 1.1 基本信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | Freqtrade |
| **开发团队** | Freqtrade Team（社区驱动） |
| **GitHub** | [freqtrade/freqtrade](https://github.com/freqtrade/freqtrade) |
| **许可证** | GPL-3.0 |
| **主要语言** | Python (98.5%) |
| **当前版本** | 2026.4（持续活跃维护） |
| **Stars** | 32,000+ |

### 1.2 项目定位

Freqtrade 是一个**免费开源的加密货币交易机器人**，支持所有主流交易所，可通过Telegram或WebUI控制，包含回测、绘图和资金管理工具以及机器学习策略优化。

> **核心价值**：让个人投资者也能拥有机构级的量化交易能力。

### 1.3 与 Dao Quant 模型的关系

| 维度 | Freqtrade | Dao Quant 双引擎四层 |
|-----|-----------|---------------------|
| **市场** | 加密货币 | A股股票 |
| **核心能力** | 自动交易、风险管理 | 股票评分、风险评估 |
| **适用场景** | 7x24小时自动交易 | 选股决策、组合构建 |
| **互补价值** | 可将Dao Quant评分应用于加密货币策略 |

---

## 二、核心特性

### 2.1 主要功能

| 功能 | 说明 |
|-----|------|
| **多交易所支持** | Binance、Kraken、OKX等主流交易所 |
| **现货+合约** | 支持现货和永续合约交易 |
| **回测引擎** | 基于历史数据的策略验证 |
| **FreqAI** | 自适应机器学习模块 |
| **Hyperopt** | 策略参数机器学习优化 |
| **WebUI** | 内置网页界面管理机器人 |
| **Telegram** | 移动端远程控制 |
| **Dry-run** | 模拟运行，不花真金白银 |

### 2.2 支持交易所

**现货交易所**：Binance、BingX、Bitget、Bitmart、Bybit、Gate.io、HTX、Kraken、OKX等

**合约交易所**：Binance Futures、Bitget Futures、Bybit Futures、OKX Futures等

---

## 三、快速入门

### 3.1 安装

```bash
# Docker方式（推荐）
docker compose pull
docker compose up -d

# 本地安装
git clone https://github.com/freqtrade/freqtrade.git
cd freqtrade
./setup.sh --install
```

### 3.2 配置

```json
{
    "max_open_trades": 3,
    "stake_currency": "USDT",
    "stake_amount": 100,
    "tradable_balance_ratio": 0.99,
    "fiat_display_currency": "USD",
    "dry_run": true,
    "cancel_open_orders_on_exit": false,
    "unfilledtimeout": {
        "entry": 10,
        "exit": 10
    },
    "exchange": {
        "name": "binance",
        "key": "your_exchange_key",
        "secret": "your_exchange_secret"
    }
}
```

### 3.3 策略示例

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta

class SampleStrategy(IStrategy):
    # 最小时间周期
    timeframe = '5m'
    
    # 止损设置
    stoploss = -0.10
    
    #  trailing stop
    trailing_stop = True
    
    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        # 计算指标
        dataframe['ema20'] = ta.EMA(dataframe, timeperiod=20)
        dataframe['ema50'] = ta.EMA(dataframe, timeperiod=50)
        dataframe['rsi'] = ta.RSI(dataframe, timeperiod=14)
        return dataframe
    
    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe['ema20'] > dataframe['ema50']) &  # 金叉
                (dataframe['rsi'] < 70)  # RSI不过高
            ),
            'enter_long'] = 1
        return dataframe
    
    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe['ema20'] < dataframe['ema50']) |  # 死叉
                (dataframe['rsi'] > 80)  # RSI超买
            ),
            'exit_long'] = 1
        return dataframe
```

---

## 四、FreqAI 机器学习模块

### 4.1 核心概念

FreqAI 是 Freqtrade 的自适应机器学习模块，能够：
- 自动训练模型适应市场变化
- 实时特征工程
- 预测入场/出场信号

### 4.2 配置示例

```json
{
    "freqai": {
        "enabled": true,
        "purge_old_models": 2,
        "train_period_days": 15,
        "backtest_period_days": 7,
        "identifier": "unique-id",
        "feature_parameters": {
            "include_timeframes": ["5m", "15m", "1h"],
            "include_corr_pairlist": ["BTC/USDT", "ETH/USDT"],
            "label_period_candles": 20,
            "include_shifted_candles": 2
        },
        "data_split_parameters": {
            "test_size": 0.33,
            "random_state": 1
        },
        "model_training_parameters": {
            "n_estimators": 1000
        }
    }
}
```

---

## 五、项目评估

### 5.1 优势分析

| 优势 | 说明 |
|-----|------|
| **功能全面** | 从回测到实盘的一站式解决方案 |
| **社区活跃** | 32k+ Stars，持续更新维护 |
| **文档完善** | 官方文档详尽，示例丰富 |
| **机器学习** | FreqAI模块提供AI能力 |
| **多交易所** | 支持几乎所有主流交易所 |
| **风险控制** | 完善的止损、仓位管理 |

### 5.2 局限与注意事项

| 局限 | 说明 | 应对建议 |
|-----|------|---------|
| **仅限加密货币** | 不支持股票、期货等传统市场 | 使用其他工具如Backtrader |
| **技术门槛** | 需要Python和Linux基础 | 从Docker版本开始 |
| **市场风险** | 加密货币波动极大 | 严格止损，小资金测试 |
| **API限制** | 交易所API可能受限 | 合理设置请求频率 |

---

## 六、总结

### 6.1 核心价值

Freqtrade 代表了**个人量化交易的开源力量**：

1. **民主化**：让普通投资者拥有专业工具
2. **自动化**：7x24小时无需人工干预
3. **可扩展**：支持自定义策略和机器学习

### 6.2 适用人群

| 人群 | 适用场景 |
|-----|---------|
| **加密货币投资者** | 自动化交易策略执行 |
| **量化爱好者** | 学习算法交易 |
| **技术开发者** | 构建自定义交易系统 |
| **风控重视者** | 严格的风险管理规则 |

---

## 参考文献

1. Freqtrade Official Documentation. https://www.freqtrade.io/
2. freqtrade. Freqtrade GitHub Repository. https://github.com/freqtrade/freqtrade

---

> ⚠️ **免责声明**：本文仅对开源项目进行技术介绍和分析，不构成任何投资建议。加密货币市场风险极高，可能导致全部本金损失。

---

*Powered by 老子道·Dao Quant Research | 道可道，非常道*
