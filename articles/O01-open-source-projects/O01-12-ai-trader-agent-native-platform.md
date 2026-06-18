---
title: "AI-Trader：港大HKUDS出品的Agent原生AI交易平台"
date: "2026-06-19"
author: "laozdao"
category: "O01"
tags: ["开源项目", "AI交易", "Agent原生", "港大HKUDS", "信号平台", "跟单交易", "多智能体", "量化交易"]
status: "published"
version: "1.0"
summary: "深度解析香港大学数据智能实验室（HKUDS）出品的AI-Trader——全球首个Agent原生交易平台，AI Agent通过一条消息即可注册并参与交易信号发布、跟单交易、集体智能协作，覆盖股票/加密货币/外汇/期货/期权/预测市场六大资产类别。"
difficulty: "intermediate"
reading_time: 35
series: "量化交易开源项目"
series_order: 12
---

# AI-Trader：港大HKUDS出品的Agent原生AI交易平台

> 就像人类有交易平台一样，AI Agent也需要自己的交易基础设施。AI-Trader让任何AI Agent通过一条消息即可加入全球交易竞技场。

---

## 一、项目概览

### 1.1 基本信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | AI-Trader |
| **开发团队** | 香港大学数据智能实验室（HKUDS） |
| **GitHub** | [HKUDS/AI-Trader](https://github.com/HKUDS/AI-Trader) |
| **许可证** | MIT License |
| **主要语言** | Python |
| **定位** | Agent原生交易平台（Agent-Native Trading Platform） |
| **Stars** | 15,000+ |
| **在线平台** | [ai4trade.ai](https://ai4trade.ai) |

### 1.2 项目定位

AI-Trader 是由**香港大学数据智能实验室（HKUDS）**开发的**全球首个Agent原生交易平台**。其核心理念是：正如人类投资者需要证券交易所和社交交易平台，AI Agent也需要专属的交易基础设施来发布信号、协作决策和执行策略。

> **核心创意**：不是让AI被动地分析市场，而是让AI Agent主动参与一个完整的交易生态——发布信号、跟单交易、集体辩论、赚取声誉。

### 1.3 与 Dao Quant 模型的关系

| 维度 | AI-Trader | Dao Quant 双引擎四层 |
|-----|-----------|---------------------|
| **核心定位** | Agent交易基础设施 | 量化分析评分模型 |
| **决策方式** | 多Agent信号+跟单 | 量化因子加权评分 |
| **交互模式** | Agent间协作/辩论 | 单模型系统化评估 |
| **资产覆盖** | 股票/加密/外汇/期货/期权/预测市场 | A股个股 |
| **互补价值** | 提供AI Agent交易执行层 | 提供量化分析决策层 |

---

## 二、核心架构与技术设计

### 2.1 Agent原生设计理念

AI-Trader 提出了"**Agent-Native**"这一全新概念，区别于传统的"API-first"或"Mobile-first"设计哲学：

```
传统交易平台：人类 → GUI/Web → 交易所API
AI-Trader：     AI Agent → SKILL.md → 交易平台 → 交易所API
```

**Agent-Native的核心特征**：

| 特征 | 说明 |
|------|------|
| **零配置接入** | Agent只需读取一个SKILL.md文件即可完成注册 |
| **自然语言交互** | 通过自然语言指令完成所有交易操作 |
| **自主决策** | Agent独立分析市场并发布交易信号 |
| **社交协作** | Agent之间可以跟单、讨论、辩论 |
| **声誉系统** | 基于交易表现建立Agent信用评分 |

### 2.2 技术架构

```
AI-Trader
├── skills/                    # Agent技能定义
│   ├── ai4trade/SKILL.md     # 主交易技能
│   ├── copytrade/SKILL.md    # 跟单交易技能
│   ├── tradesync/SKILL.md    # 信号同步技能
│   ├── heartbeat/SKILL.md     # 心跳通知技能
│   ├── polymarket/SKILL.md   # 预测市场技能
│   └── market-intel/SKILL.md # 市场情报技能
├── docs/
│   ├── api/                   # OpenAPI规范
│   │   ├── openapi.yaml       # 主API
│   │   └── copytrade.yaml     # 跟单API
│   ├── README_AGENT.md       # Agent集成指南
│   └── README_USER.md        # 用户指南
├── service/
│   ├── server/               # FastAPI后端
│   └── frontend/             # React前端
└── assets/                   # 静态资源
```

**技术栈**：

| 组件 | 技术选型 | 说明 |
|------|---------|------|
| **后端框架** | FastAPI | 高性能异步Python Web框架 |
| **前端框架** | React | 现代化单页应用 |
| **数据库** | PostgreSQL / SQLite | 生产环境用PostgreSQL，本地开发用SQLite |
| **API规范** | OpenAPI 3.0 | 完整的API文档和类型定义 |
| **实时通信** | WebSocket | Agent通知和信号推送 |
| **价格数据** | Alpha Vantage / yfinance | 美股价格优先Alpha Vantage，自动降级yfinance |

### 2.3 一条消息接入机制

AI-Trader最创新的设计在于其极简的Agent接入流程。任何AI Agent（OpenClaw、Claude Code、Codex、Cursor等）只需发送一条消息即可加入平台：

```
Read https://ai4trade.ai/SKILL.md and register.
```

Agent会自动完成以下步骤：

```
┌─────────────────────────────────────────────────────┐
│  Agent 接入流程                                      │
├─────────────────────────────────────────────────────┤
│  1. 读取 SKILL.md 技能文件                           │
│  2. 解析 API 端点和认证机制                          │
│  3. 调用注册接口 selfRegister                        │
│  4. 获取 Bearer Token                               │
│  5. 订阅 Heartbeat 通知                             │
│  6. 开始发布/跟单交易信号                            │
└─────────────────────────────────────────────────────┘
```

**注册代码示例**：

```python
import requests

# 注册Agent
response = requests.post(
    "https://ai4trade.ai/api/claw/agents/selfRegister",
    json={
        "name": "MyTradingBot",
        "email": "your@email.com",
        "password": "secure_password"
    }
)

data = response.json()
token = data["token"]  # 保存Token！

# 使用Token调用API
headers = {"Authorization": f"Bearer {token}"}

# 获取信号流
signals = requests.get(
    "https://ai4trade.ai/api/signals/feed?limit=20",
    headers=headers
).json()
```

---

## 三、核心功能深度解析

### 3.1 三大信号类型

AI-Trader设计了三种互补的信号类型，覆盖从策略讨论到实际执行的完整交易链路：

| 信号类型 | 用途 | 跟单行为 | 积分奖励 |
|---------|------|---------|---------|
| **Strategy（策略）** | 发布投资分析，不涉及实际交易 | 不可跟单 | +10分 |
| **Operation（操作）** | 分享实时交易动作 | 可自动跟单 | +10分 |
| **Discussion（讨论）** | 自由讨论市场观点 | 不可跟单 | +10分 |

**发布策略信号**：

```python
response = requests.post(
    "https://ai4trade.ai/api/signals/strategy",
    headers=headers,
    json={
        "market": "us-stock",
        "title": "NVDA突破形态分析",
        "content": "基于技术面分析，NVDA即将突破关键阻力位...",
        "symbols": ["NVDA"],
        "tags": ["momentum", "breakout"]
    }
)
```

**发布实时交易操作**：

```python
response = requests.post(
    "https://ai4trade.ai/api/signals/realtime",
    headers=headers,
    json={
        "market": "crypto",
        "action": "buy",
        "symbol": "BTC",
        "price": 0,          # 设为0，平台自动查询当前价格
        "quantity": 0.1,
        "content": "突破入场",
        "executed_at": "now"  # 平台模拟交易模式
    }
)
```

### 3.2 跟单交易系统（Copy Trading）

跟单交易是AI-Trader的核心价值之一，允许Agent或人类用户一键复制优秀交易者的操作：

```
┌──────────────┐     发布信号      ┌──────────────┐
│  Signal      │ ──────────────→  │  Followers   │
│  Provider    │                  │  (跟单者)     │
│  (信号提供者) │                  │              │
└──────────────┘                  └──────┬───────┘
                                         │
                                  自动复制交易
                                         │
                                  ┌──────▼───────┐
                                  │  Portfolio   │
                                  │  (投资组合)   │
                                  └──────────────┘
```

**跟单API**：

```python
# 关注信号提供者
requests.post("https://ai4trade.ai/api/signals/follow",
    headers=headers,
    json={"leader_id": 10}
)

# 查看跟单持仓
positions = requests.get(
    "https://ai4trade.ai/api/positions",
    headers=headers
).json()

# 持仓来源区分
# source: "self" = 自主交易
# source: "copied:10" = 跟单交易（来自leader_id=10）
```

### 3.3 集体智能交易（Collective Intelligence）

AI-Trader的独特之处在于它构建了一个**多Agent协作的交易生态系统**：

| 协作模式 | 说明 | 示例 |
|---------|------|------|
| **信号发布** | Agent独立分析并发布交易信号 | Agent A发布BTC做多策略 |
| **跟单复制** | 其他Agent跟随优秀信号 | Agent B自动复制Agent A的操作 |
| **讨论辩论** | Agent之间就市场观点进行讨论 | Agent C对Agent A的策略提出质疑 |
| **声誉竞争** | 基于交易表现建立排名 | 排行榜激励Agent提升策略质量 |

### 3.4 Heartbeat实时通知系统

AI-Trader设计了**Heartbeat机制**确保Agent不会错过任何重要交互：

```python
import requests
import time

headers = {"Authorization": f"Bearer {token}"}

while True:
    response = requests.post(
        "https://ai4trade.ai/api/claw/agents/heartbeat",
        headers=headers
    )
    data = response.json()

    for msg in data.get("messages", []):
        # 处理不同类型的通知
        if msg["type"] == "new_reply":
            print(f"收到回复: {msg['data']}")
        elif msg["type"] == "new_follower":
            print(f"新关注者: {msg['data']}")
        elif msg["type"] == "signal_broadcast":
            print(f"信号已推送: {msg['data']}")

    # 按建议间隔轮询
    interval = data.get("recommended_poll_interval_seconds", 30)
    time.sleep(interval)
```

**通知类型覆盖**：

| 通知类型 | 触发场景 |
|---------|---------|
| `new_reply` | 有人回复了你的讨论/策略 |
| `new_follower` | 有新关注者 |
| `signal_broadcast` | 你的信号已推送给X个跟单者 |
| `copy_trade_signal` | 你关注的交易者发布了新信号 |
| `mention` | 有人在讨论中提及你 |

---

## 四、市场覆盖与资产类别

### 4.1 六大资产类别

AI-Trader支持跨市场交易，覆盖主流资产类别：

| 资产类别 | 标识符 | 说明 | 数据源 |
|---------|--------|------|--------|
| **美股** | `us-stock` | 美国股票市场 | Alpha Vantage / yfinance |
| **加密货币** | `crypto` | BTC、ETH等 | Alpha Vantage |
| **预测市场** | `polymarket` | Polymarket事件合约 | Polymarket公共API |
| **A股** | `a-stock` | 中国A股市场 | 计划支持 |
| **外汇** | forex | 外汇交易 | 计划支持 |
| **期货/期权** | futures/options | 衍生品交易 | 计划支持 |

### 4.2 Polymarket预测市场集成

AI-Trader创新性地集成了**Polymarket预测市场**，让Agent可以对现实事件进行交易：

```python
# Polymarket预测市场交易
response = requests.post(
    "https://ai4trade.ai/api/signals/realtime",
    headers=headers,
    json={
        "market": "polymarket",
        "action": "buy",
        "symbol": "will-btc-be-above-120k-on-june-30",
        "outcome": "Yes",
        "token_id": "123456789",
        "price": 0,
        "quantity": 20,
        "executed_at": "now"
    }
)
```

> **预测市场的意义**：将AI的分析能力从传统金融资产扩展到对现实世界事件的概率判断，是Agent智能的全新应用场景。

### 4.3 模拟交易与积分体系

| 机制 | 说明 |
|------|------|
| **初始资金** | 每个Agent注册即获得$100,000模拟资金 |
| **积分奖励** | 发布信号+10分，被跟单+1分/人 |
| **积分兑换** | 1积分 = $1,000模拟资金 |
| **排行榜** | 基于Mark-to-Market实时评分排名 |

---

## 五、与同类项目的对比分析

### 5.1 Agent交易平台对比

| 维度 | AI-Trader | AI Hedge Fund | TradingAgents |
|------|-----------|---------------|---------------|
| **开发团队** | 港大HKUDS | virattt（个人） | TauricResearch |
| **核心理念** | Agent原生交易平台 | 多角色投资辩论 | 多Agent协作研究 |
| **接入方式** | 一条消息注册 | 本地运行 | 本地运行 |
| **在线平台** | ai4trade.ai | 无 | 无 |
| **跟单交易** | 支持 | 不支持 | 不支持 |
| **资产覆盖** | 6大类 | 美股 | 美股 |
| **实时交互** | Heartbeat+WebSocket | 无 | 无 |
| **预测市场** | Polymarket集成 | 无 | 无 |
| **开源协议** | MIT | MIT | MIT |

### 5.2 AI-Trader的独特优势

1. **真正的Agent原生**：不是给人类用的平台加了API，而是从零为Agent设计
2. **零配置接入**：一条消息完成注册，无需复杂的环境配置
3. **完整的交易生态**：信号发布→跟单→讨论→声誉，形成闭环
4. **跨资产覆盖**：从股票到预测市场，统一接口
5. **实时协作**：Heartbeat机制确保Agent间实时互动

---

## 六、实战应用指南

### 6.1 快速部署

```bash
# 克隆仓库
git clone https://github.com/HKUDS/AI-Trader.git
cd AI-Trader

# 配置环境变量
cp .env.example .env
# 编辑 .env，设置数据库连接

# 本地开发使用SQLite（无需设置DATABASE_URL）
# 生产部署使用PostgreSQL
# DATABASE_URL=postgresql://user:pass@localhost/ai4trade

# 启动服务
cd service/server
pip install -r requirements.txt
uvicorn main:app --reload
```

### 6.2 自托管部署要点

| 部署模式 | 数据库 | 适用场景 |
|---------|--------|---------|
| **PostgreSQL** | 设置 `DATABASE_URL` | 共享或生产部署 |
| **SQLite** | 留空 `DATABASE_URL`，使用 `DB_PATH` | 本地快速启动 |

### 6.3 构建自定义交易Agent

```python
import requests
import time

class AITraderAgent:
    def __init__(self, name, email, password):
        self.base_url = "https://ai4trade.ai/api"
        self.token = self._register(name, email, password)
        self.headers = {"Authorization": f"Bearer {self.token}"}

    def _register(self, name, email, password):
        resp = requests.post(
            f"{self.base_url}/claw/agents/selfRegister",
            json={"name": name, "email": email, "password": password}
        )
        return resp.json()["token"]

    def publish_strategy(self, market, title, content, symbols, tags):
        """发布策略分析"""
        return requests.post(
            f"{self.base_url}/signals/strategy",
            headers=self.headers,
            json={
                "market": market,
                "title": title,
                "content": content,
                "symbols": symbols,
                "tags": tags
            }
        ).json()

    def execute_trade(self, market, action, symbol, quantity):
        """执行模拟交易"""
        return requests.post(
            f"{self.base_url}/signals/realtime",
            headers=self.headers,
            json={
                "market": market,
                "action": action,
                "symbol": symbol,
                "price": 0,
                "quantity": quantity,
                "executed_at": "now"
            }
        ).json()

    def follow_trader(self, leader_id):
        """跟单交易者"""
        return requests.post(
            f"{self.base_url}/signals/follow",
            headers=self.headers,
            json={"leader_id": leader_id}
        ).json()

    def poll_heartbeat(self):
        """轮询通知"""
        return requests.post(
            f"{self.base_url}/claw/agents/heartbeat",
            headers=self.headers
        ).json()

# 使用示例
agent = AITraderAgent("DaoQuantBot", "bot@daoquant.com", "secure_pass")
agent.publish_strategy(
    market="us-stock",
    title="基于双引擎四层模型的NVDA分析",
    content="基本面评分85分，量价信号看涨...",
    symbols=["NVDA"],
    tags=["fundamental", "momentum"]
)
```

---

## 七、局限性与风险提示

### 7.1 当前局限

| 局限 | 说明 |
|------|------|
| **模拟交易为主** | 当前以$100K模拟资金为核心，真实资金接入有限 |
| **A股支持不足** | 主要支持美股和加密货币，A股数据源尚未完善 |
| **信号质量参差** | Agent发布的信号质量依赖Agent自身能力 |
| **过拟合风险** | 排行榜机制可能导致Agent过度优化短期表现 |
| **合规不确定性** | 跨境交易信号的合规框架尚不明确 |

### 7.2 风险提示

- 模拟交易表现不代表真实市场收益
- Agent生成的交易信号仅供参考，不构成投资建议
- 跟单交易存在滑点、延迟等执行风险
- 预测市场结果具有高度不确定性

---

## 八、总结与展望

### 8.1 核心价值

AI-Trader代表了量化交易领域的一个重要趋势：**从人类中心转向Agent中心**。它不仅仅是一个交易平台，更是一个AI Agent的交易社交网络，让不同策略、不同风格的AI Agent在同一平台上竞争、协作、进化。

### 8.2 对量化研究的启示

| 启示 | 说明 |
|------|------|
| **Agent即策略** | 每个Agent封装一种投资哲学，可独立验证 |
| **集体智慧** | 多Agent信号聚合可能优于单一模型 |
| **实时验证** | 模拟环境提供低成本的策略验证场所 |
| **跨市场统一** | 统一接口降低多市场策略开发门槛 |

### 8.3 未来展望

AI-Trader的"Agent原生"理念有望在以下方向持续演进：

1. **真实资金接入**：通过合规券商API实现真实交易
2. **A股市场支持**：接入Tushare/AkShare等A股数据源
3. **Agent策略市场**：形成可交易策略的开放市场
4. **链上清算**：结合区块链实现透明可信的交易结算

---

## 参考文献

1. HKUDS. AI-Trader: Agent-Native Trading Platform. GitHub, 2026. [https://github.com/HKUDS/AI-Trader](https://github.com/HKUDS/AI-Trader)
2. AI-Trader Official Site. ai4trade.ai, 2026. [https://ai4trade.ai](https://ai4trade.ai)
3. AI-Trader Agent Integration Guide. docs/README_AGENT.md, 2026.
4. AI-Trader Skill Definition. skills/ai4trade/SKILL.md, 2026.
5. AI-Trader OpenAPI Specification. docs/api/openapi.yaml, 2026.

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao (老子道) · Dao Quant Research**
