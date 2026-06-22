---
title: "a-stock-data：A股全栈数据工具包，零依赖直连13大数据源"
date: "2026-06-22"
author: "laozdao"
category: "O01"
tags: ["开源项目", "A股数据", "数据工具包", "量化交易", "AI原生", "SKILL.md", "投研数据", "akshare替代"]
status: "published"
version: "1.0"
summary: "深度解析simonlin1212出品的a-stock-data——专为AI编程助手与量化开发者打造的A股全栈数据工具包，零akshare依赖，直连13大数据源28个端点，7层架构覆盖行情、研报、资金、新闻、公告全场景。"
difficulty: "intermediate"
reading_time: 35
series: "量化交易开源项目"
series_order: 13
---

# a-stock-data：A股全栈数据工具包，零依赖直连13大数据源

> 告别akshare的版本兼容噩梦和接口失效焦虑，a-stock-data用7层架构+28个数据端点，为A股量化开发者提供了一套零依赖、开箱即用的全栈数据基础设施。

---

## 一、项目概览

### 1.1 基本信息

| 项目属性 | 详情 |
|---------|------|
| **项目名称** | a-stock-data |
| **开发团队** | simonlin1212（个人开发者） |
| **GitHub** | [simonlin1212/a-stock-data](https://github.com/simonlin1212/a-stock-data) |
| **许可证** | MIT License |
| **主要语言** | Python（SKILL.md + 内嵌Python） |
| **定位** | A股全栈数据工具包 |
| **Stars** | 4,000+ |

### 1.2 项目定位

a-stock-data 是一款**专为AI编程助手与量化开发者打造的A股全栈数据工具包**。其最大创新在于：以独立 `SKILL.md` 文件为载体，采用**结构化Markdown + 内嵌Python**设计，将13个分散的A股原始数据源封装成可直接调用的工具集。

> **核心设计理念**："稳定性优先，AI原生"——在A股数据领域，"稳定"比"全"更重要。

### 1.3 与 Dao Quant 模型的关系

| 维度 | a-stock-data | Dao Quant 双引擎四层 |
|-----|-------------|---------------------|
| **核心定位** | A股数据采集基础设施 | 量化分析评分模型 |
| **技术路线** | 零依赖直连原生API | 因子计算+加权评分 |
| **使用方式** | AI Agent自然语言调用 | 系统化量化评估 |
| **数据覆盖** | 行情/研报/资金/新闻/公告 | 基本面/量价/风控 |
| **互补价值** | 提供底层数据供给 | 提供上层分析框架 |

---

## 二、核心架构与技术设计

### 2.1 七层架构全景

a-stock-data V3.0 采用极简清晰的七层架构，每层对应专属数据源与核心能力：

```
A股全栈数据 · 七层架构 · V3.0
│
├── 行情层 (mootdx + 腾讯财经 + 百度K线)
│   能力：K线(自带MA5/10/20) + 五档盘口 + PE/PB/市值 + 指数/ETF
│
├── 研报层 (东财reportapi + 同花顺 + iwencai)
│   能力：研报列表/PDF下载 + 机构一致预期EPS + 自然语言研报检索
│
├── 信号层 (同花顺 + 百度股市通 + 东财DC)
│   能力：强势股题材归因 + 北向资金 + 龙虎榜 + 解禁日历 + 行业排名
│
├── 资金面/筹码层 (东财datacenter + push2his)
│   能力：融资融券 + 大宗交易 + 股东户数 + 分红送转 + 120日资金流
│
├── 新闻层 (东财 + 财联社)
│   能力：个股新闻 + 分钟级财联社快讯 + 全球财经资讯
│
├── 基础数据层 (mootdx + 东财 + 新浪)
│   能力：季报37项财务字段 + F10公司资料 + 财报三表
│
└── 公告层 (巨潮cninfo + mootdx)
    能力：沪深北交所全量上市公司公告
```

### 2.2 数据源稳定性分级

项目对13大数据源进行了封禁风险评估，优先选用低风险接口：

| 风险等级 | 数据源 | 协议类型 | 说明 |
|---------|--------|---------|------|
| **极低封禁** | mootdx | TCP协议 | 通达信行情端口，几乎不封IP |
| **极低封禁** | 同花顺热点/北向 | HTTP | 公开数据接口 |
| **极低封禁** | 百度股市通 | HTTP | 公开行情接口 |
| **低封禁** | 腾讯财经 | HTTP | PE/PB/市值等基础数据 |
| **低封禁** | 东财全系 | HTTP | 研报、新闻、数据中心 |
| **低封禁** | 新浪财经 | HTTP | F10公司资料 |
| **低封禁** | 财联社 | HTTP直连 | 分钟级快讯 |
| **低封禁** | 巨潮公告 | HTTP | 官方公告平台 |
| **需API Key** | iwencai | HTTP | 自然语言研报检索 |

### 2.3 零依赖设计哲学

a-stock-data V3.0 彻底抛弃了akshare依赖，核心优势对比：

| 痛点 | akshare方案 | a-stock-data方案 |
|------|------------|-----------------|
| **版本兼容** | 频繁遭遇Pandas适配报错 | 零第三方数据封装依赖 |
| **接口失效** | 数据源变动导致接口不可用 | 直连原生HTTP API，自主可控 |
| **反爬策略** | 需手动适配Referer、请求头 | 多数接口零鉴权免费 |
| **部署复杂度** | 依赖链长，环境配置繁琐 | 3步2分钟极速部署 |
| **AI适配** | 无原生AI Agent支持 | SKILL.md完美兼容Claude/Codex |

---

## 三、28个核心数据端点详解

### 3.1 行情层（实时稳定，无IP封禁）

| 端点 | 数据源 | 核心能力 |
|------|--------|---------|
| **多周期K线** | mootdx | 日/周/月K线，支持复权 |
| **五档盘口** | mootdx | 实时买卖五档报价 |
| **逐笔成交** | mootdx | 46字段实时报价详情 |
| **PE/PB/市值** | 腾讯财经 | 基础估值指标，支持指数/ETF |
| **日K+均线** | 百度K线 | 直接返回MA5/10/20，无需手动计算 |

**行情数据获取示例**：

```python
import requests
import pandas as pd

# 百度K线：直接返回日K+均线
def get_kline_with_ma(stock_code):
    """获取日K线数据，包含MA5/10/20"""
    url = f"https://finance.pae.baidu.com/selfselect/getstockquotation"
    params = {
        "all": 1,
        "ktype": 1,
        "group": "quotation_minute",
        "query": stock_code,
        "finClientType": "pc"
    }
    response = requests.get(url, params=params)
    data = response.json()
    # 返回数据已包含MA5/10/20，无需二次计算
    return data

# 腾讯财经：获取PE/PB/市值等基础指标
def get_stock_basic(stock_code):
    """获取股票基础估值指标"""
    url = f"https://qt.gtimg.cn/q={stock_code}"
    response = requests.get(url)
    # 解析返回的字段：PE(TTM)、PB、市值、换手率、涨跌停价
    return response.text
```

### 3.2 研报层

| 端点 | 数据源 | 核心能力 |
|------|--------|---------|
| **研报列表** | 东财reportapi | 按股票/行业筛选研报 |
| **研报PDF下载** | 东财reportapi | 免鉴权下载PDF |
| **机构一致预期** | 同花顺 | 三年EPS预测 |
| **自然语言检索** | iwencai | 跨主题研报语义搜索 |

### 3.3 信号层（短线投研核心）

| 端点 | 数据源 | 核心能力 |
|------|--------|---------|
| **强势股题材归因** | 同花顺 | 当日涨停/强势股题材标签 |
| **北向资金** | 东财DC | 分钟级资金流向 |
| **龙虎榜** | 东财DC | 全市场龙虎榜排名 |
| **解禁日历** | 东财DC | 未来90天限售解禁预警 |
| **行业板块排名** | 东财DC | 行业涨跌排行、资金流入 |

### 3.4 资金面/筹码层（V3.0新增）

这是个人投资者最刚需的模块：

| 端点 | 数据源 | 核心能力 |
|------|--------|---------|
| **融资融券** | 东财datacenter | 两融余额、融资买入额 |
| **大宗交易** | 东财datacenter | 溢价/折价分析 |
| **股东户数** | 东财datacenter | 环比变化（筹码集中度判断） |
| **分红送转** | 东财datacenter | 历年分红送转记录 |
| **120日资金流** | push2his | 长短周期资金流向 |

**资金面数据获取示例**：

```python
# 东财数据中心统一查询接口
def eastmoney_datacenter(query_type, stock_code, **kwargs):
    """
    统一封装东财数据中心查询
    query_type: 'margin'（融资融券）/ 'blocktrade'（大宗交易）/
                'holder'（股东户数）/ 'dividend'（分红送转）/
                'fundflow'（资金流向）/ 'forecast'（业绩预测）
    """
    base_urls = {
        'margin': 'https://datacenter-web.eastmoney.com/api/data/v1/get',
        'blocktrade': 'https://datacenter-web.eastmoney.com/api/data/v1/get',
        'holder': 'https://datacenter-web.eastmoney.com/api/data/v1/get',
        'fundflow': 'https://push2his.eastmoney.com/api/qt/stock/fflow/daykline/get'
    }
    # 统一参数构建与请求发送
    # ...
    return data
```

### 3.5 新闻与公告层

| 端点 | 数据源 | 核心能力 |
|------|--------|---------|
| **个股新闻** | 东财 | 实时个股资讯 |
| **财联社快讯** | 财联社HTTP直连 | 分钟级财经快讯 |
| **全球资讯** | 东财 | 全球财经新闻 |
| **官方公告** | 巨潮cninfo | 沪深北交所全量公告 |

---

## 四、AI原生适配与实战应用

### 4.1 SKILL.md 设计模式

a-stock-data 的核心创新在于采用 **SKILL.md** 文件作为AI Agent的接口契约：

```
┌─────────────────────────────────────────┐
│  AI Agent (Claude/Codex/OpenClaw)       │
│  ─────────────────────────────────────  │
│  "帮我查贵州茅台的PE和今日资金流向"      │
└──────────────┬──────────────────────────┘
               │ 自然语言理解
               ▼
┌─────────────────────────────────────────┐
│  SKILL.md (结构化Markdown + Python)     │
│  ─────────────────────────────────────  │
│  - 数据端点定义                         │
│  - 请求参数说明                         │
│  - 内嵌Python代码示例                   │
│  - 返回字段解析                         │
└──────────────┬──────────────────────────┘
               │ 代码执行
               ▼
┌─────────────────────────────────────────┐
│  13大数据源 (HTTP API直连)               │
│  ─────────────────────────────────────  │
│  腾讯财经 / 东财 / 同花顺 / mootdx ...  │
└─────────────────────────────────────────┘
```

### 4.2 极速部署（3步2分钟）

```bash
# 1. 创建skill存放目录
mkdir -p ~/.claude/skills/a-stock-data

# 2. 下载官方SKILL.md配置文件
curl -o ~/.claude/skills/a-stock-data/SKILL.md \
  https://raw.githubusercontent.com/simonlin1212/a-stock-data/main/SKILL.md

# 3. 安装依赖（无需akshare）
pip install mootdx requests pandas stockstats
```

**不同AI助手的适配方式**：

| AI助手 | 接入方式 |
|--------|---------|
| **Claude Code** | 启动后直接自然语言提问，自动激活数据工具 |
| **Codex / OpenClaw** | 将SKILL.md内容粘贴至系统提示词 |
| **普通Python** | 复制文件内嵌Python代码，独立脚本运行 |

### 4.3 AI投研实战场景

无需写代码，对着AI助手说一句话即可完成专业投研分析：

| 投研场景 | 自然语言指令示例 |
|---------|----------------|
| **个股估值** | "帮我估算688017的估值，输出PE/PEG和消化年限" |
| **题材分析** | "今天强势股票有哪些，核心炒作题材是什么" |
| **资金流向** | "贵州茅台今日主力资金流入还是流出" |
| **龙虎榜调研** | "立讯精密近期是否登上龙虎榜，哪些营业部大举买入" |
| **解禁预警** | "帮我查这只股票未来3个月限售解禁情况" |
| **行业轮动** | "今日哪些行业涨幅居前，资金主力流入哪些板块" |

### 4.4 四套标准化投研流程

a-stock-data 内置了四套标准化调研流程，大幅降低投研门槛：

| 流程名称 | 耗时 | 覆盖维度 |
|---------|------|---------|
| **单票估值** | 30秒 | PE/PEG/消化年限 |
| **多股批量对比** | 1分钟 | 财务指标横向对比 |
| **主题研报检索** | 2分钟 | 跨主题研报语义搜索 |
| **新标的全维度调研** | 1分钟 | 行情+财务+资金+新闻+公告 |

---

## 五、与同类项目的对比分析

### 5.1 A股数据工具对比

| 维度 | a-stock-data | akshare | Tushare |
|------|-------------|---------|---------|
| **依赖方式** | 零依赖直连API | 封装第三方接口 | 封装第三方接口 |
| **部署难度** | 3步2分钟 | 需处理版本兼容 | 需注册Token |
| **AI原生** | SKILL.md完美适配 | 无原生支持 | 无原生支持 |
| **数据源数量** | 13个 | 多个 | 多个 |
| **数据端点** | 28个 | 数百个 | 数百个 |
| **稳定性** | 自主可控 | 依赖第三方 | 依赖第三方 |
| **费用** | 免费（除iwencai） | 免费 | 部分收费 |
| **适用场景** | AI投研/量化开发 | 数据采集/学术研究 | 专业量化/机构 |

### 5.2 a-stock-data 的独特优势

1. **零依赖稳定性**：彻底移除akshare，直连原生API，规避版本兼容和接口失效风险
2. **AI原生设计**：SKILL.md让AI Agent自然语言即可调用数据，无需写代码
3. **七层全栈覆盖**：从行情到公告，从资金到研报，一站式满足投研需求
4. **数据源分级**：明确标注封禁风险，优先使用低风险接口
5. **轻量化部署**：3步2分钟完成，无需复杂环境配置

---

## 六、实战代码示例

### 6.1 完整数据获取流程

```python
import requests
import pandas as pd
from datetime import datetime

class AStockDataToolkit:
    """A股全栈数据工具包核心类"""

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        })

    def get_kline(self, stock_code, period='day'):
        """获取K线数据（含均线）"""
        # 百度K线接口，直接返回MA5/10/20
        url = "https://finance.pae.baidu.com/selfselect/getstockquotation"
        params = {
            "all": 1,
            "ktype": 1 if period == 'day' else 2,
            "group": "quotation_minute",
            "query": stock_code,
            "finClientType": "pc"
        }
        response = self.session.get(url, params=params)
        return response.json()

    def get_basic_info(self, stock_code):
        """获取基础估值指标"""
        # 腾讯财经接口
        url = f"https://qt.gtimg.cn/q={stock_code}"
        response = self.session.get(url)
        # 字段解析：43位振幅，46位PB
        data = response.text
        return self._parse_tencent_data(data)

    def get_margin_trading(self, stock_code):
        """获取融资融券数据"""
        url = "https://datacenter-web.eastmoney.com/api/data/v1/get"
        params = {
            "sortColumns": "TRADE_DATE",
            "sortTypes": "-1",
            "pageSize": "500",
            "reportName": "RPTA_WEB_STOCKMARGIN",
            "columns": "ALL",
            "filter": f"(SECURITY_CODE='{stock_code}')"
        }
        response = self.session.get(url, params=params)
        return response.json()

    def get_northbound_flow(self, stock_code):
        """获取北向资金流向"""
        # 东财北向资金接口
        url = "https://datacenter-web.eastmoney.com/api/data/v1/get"
        params = {
            "sortColumns": "HOLD_DATE",
            "sortTypes": "-1",
            "pageSize": "100",
            "reportName": "RPT_MUTUAL_STOCK_NORTH",
            "columns": "ALL",
            "filter": f"(SECURITY_CODE='{stock_code}')"
        }
        response = self.session.get(url, params=params)
        return response.json()

    def get_dragon_tiger(self, date=None):
        """获取龙虎榜数据"""
        if date is None:
            date = datetime.now().strftime('%Y-%m-%d')
        url = "https://datacenter-web.eastmoney.com/api/data/v1/get"
        params = {
            "sortColumns": "NET_BUY_AMT",
            "sortTypes": "-1",
            "pageSize": "50",
            "reportName": "RPTA_WEB_DRAGONLIST",
            "columns": "ALL",
            "filter": f"(TRADE_DATE='{date}')"
        }
        response = self.session.get(url, params=params)
        return response.json()

    def _parse_tencent_data(self, raw_data):
        """解析腾讯财经返回数据"""
        # 腾讯API字段说明：
        # 字段43: 振幅, 字段46: PB
        # 实际字段索引需根据返回格式校准
        lines = raw_data.strip().split(';')
        result = {}
        for line in lines:
            if '=' in line:
                code, data = line.split('=')
                fields = data.split('~')
                result[code] = {
                    'name': fields[1] if len(fields) > 1 else '',
                    'price': fields[3] if len(fields) > 3 else '',
                    'change_pct': fields[5] if len(fields) > 5 else '',
                    'pe_ttm': fields[52] if len(fields) > 52 else '',
                    'pb': fields[46] if len(fields) > 46 else '',
                    'market_cap': fields[44] if len(fields) > 44 else ''
                }
        return result

# 使用示例
toolkit = AStockDataToolkit()

# 获取贵州茅台基础信息
basic = toolkit.get_basic_info('sh600519')
print(f"PE(TTM): {basic.get('v_sh600519', {}).get('pe_ttm')}")
print(f"PB: {basic.get('v_sh600519', {}).get('pb')}")

# 获取北向资金流向
north = toolkit.get_northbound_flow('600519')
print(f"北向持仓数据条数: {len(north.get('result', {}).get('data', []))}")

# 获取今日龙虎榜
dragon = toolkit.get_dragon_tiger()
print(f"龙虎榜数据条数: {len(dragon.get('result', {}).get('data', []))}")
```

### 6.2 与 Dao Quant 模型的集成思路

```python
# Dao Quant 双引擎四层模型 + a-stock-data 数据供给

class DaoQuantWithAStockData:
    """Dao Quant模型与a-stock-data数据工具集成"""

    def __init__(self):
        self.data = AStockDataToolkit()

    def fundamental_analysis(self, stock_code):
        """基本面引擎：获取盈利/成长/估值/财务数据"""
        # 获取基础估值
        basic = self.data.get_basic_info(stock_code)

        # 获取财务三表
        financial = self.data.get_financial_statements(stock_code)

        # 获取机构一致预期
        forecast = self.data.get_forecast_eps(stock_code)

        return {
            'valuation': basic,
            'financial': financial,
            'forecast': forecast
        }

    def volume_price_analysis(self, stock_code):
        """量价引擎：获取趋势/资金/筹码数据"""
        # 获取K线+均线
        kline = self.data.get_kline(stock_code)

        # 获取资金流向
        fundflow = self.data.get_fund_flow(stock_code)

        # 获取融资融券
        margin = self.data.get_margin_trading(stock_code)

        return {
            'kline': kline,
            'fundflow': fundflow,
            'margin': margin
        }

    def risk_analysis(self, stock_code):
        """风控引擎：获取波动率/流动性数据"""
        # 获取历史波动率
        kline = self.data.get_kline(stock_code, period='day')

        # 获取股东户数变化（筹码集中度）
        holders = self.data.get_share_holders(stock_code)

        return {
            'volatility': self._calc_volatility(kline),
            'holder_concentration': holders
        }

    def _calc_volatility(self, kline_data):
        """计算历史波动率"""
        # 基于K线数据计算20日波动率
        # ...
        pass
```

---

## 七、局限性与风险提示

### 7.1 当前局限

| 局限 | 说明 |
|------|------|
| **海外访问** | mootdx走通达信TCP端口，需国内IP；海外需配置代理 |
| **数据实时性** | 部分接口有15-30分钟延迟，非Level-2实时行情 |
| **iwencai依赖** | 自然语言研报检索需API Key，非完全免费 |
| **数据完整性** | 部分历史数据可能存在缺失 |
| **合规风险** | 大规模自动化爬取可能触发平台风控 |

### 7.2 风险提示

- 数据采集仅供学习研究，请遵守各平台使用协议
- 接口稳定性可能随平台政策变化而调整
- 投资有风险，数据工具不构成投资建议
- 建议本地缓存高频查询数据，减少API调用频率

---

## 八、总结与展望

### 8.1 核心价值

a-stock-data 代表了A股数据获取领域的一个重要趋势：**从依赖封装库转向直连原生API，从人工编码转向AI自然语言调用**。它不仅仅是一个数据工具包，更是AI原生投研工作流的基础设施。

### 8.2 对量化研究的启示

| 启示 | 说明 |
|------|------|
| **稳定性优先** | 减少中间依赖层，直连API更可控 |
| **AI原生设计** | SKILL.md模式让数据工具即插即用 |
| **分层架构** | 七层设计清晰解耦，便于维护和扩展 |
| **数据源分级** | 明确风险等级，优先使用低风险接口 |

### 8.3 未来展望

a-stock-data 的SKILL.md模式有望在以下方向持续演进：

1. **更多数据源接入**：接入Level-2行情、期权数据、可转债数据
2. **数据质量监控**：自动化检测接口可用性和数据完整性
3. **本地缓存优化**：智能缓存策略，减少重复API调用
4. **量化因子库**：基于底层数据构建标准化量化因子
5. **回测框架集成**：与Backtrader等框架深度集成

---

## 参考文献

1. simonlin1212. a-stock-data: A股全栈数据工具包. GitHub, 2026. [https://github.com/simonlin1212/a-stock-data](https://github.com/simonlin1212/a-stock-data)
2. 天一生水water. 告别akshare依赖！a-stock-data V3.0 A股全栈数据工具包. CSDN, 2026-06-17.
3. a-stock-data SKILL.md. [https://raw.githubusercontent.com/simonlin1212/a-stock-data/main/SKILL.md](https://raw.githubusercontent.com/simonlin1212/a-stock-data/main/SKILL.md)
4. mootdx文档. [https://github.com/mootdx/mootdx](https://github.com/mootdx/mootdx)
5. 东方财富数据中心. [https://data.eastmoney.com](https://data.eastmoney.com)

---

> **⚠️ 免责声明**：本文仅供学习研究交流，不构成任何投资建议。股市有风险，投资需谨慎。
>
> **© 2026 laozdao (老子道) · Dao Quant Research**
