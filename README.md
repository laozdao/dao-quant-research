# Dao Quant Research · 道·量化研究

<p align="center">
  <strong>"道生一，一生二，二生三，三生万物"</strong><br>
  <em>以东方哲学思维洞察 A 股市场，以量化严谨方法评估投资价值</em>
</p>

<p align="center">
  <a href="#-文章分类目录">📖 文章目录</a> ·
  <a href="#-模型概述">🏗️ 模型架构</a> ·
  <a href="WRITING-GUIDELINES.md">📝 写作规范</a> ·
  <a href="#-快速开始">🚀 快速开始</a>
</p>

---

## 🎯 关于本仓库

**Dao Quant Research** 是一个专注于 **中国 A 股市场量化分析** 的研究知识库，收录 **128** 篇 Markdown 格式的研究文章，系统性地记录和分享基于"双引擎四层融合模型"的量化分析研究。

### 核心方法论：双引擎四层融合模型

```
                        ┌──────────────────┐
                        │   综合评分输出     │
                        │  0-100 分 / 五档评级 │
                        └────────┬─────────┘
                                 │
           ┌─────────────────────┼─────────────────────┐
           │                     │                     │
           ▼                     ▼                     ▼
      ┌─────────┐          ┌─────────┐          ┌─────────┐
      │ 基本面引擎 │          │ 量价引擎  │          │ 风控引擎  │
      │   60%   │    +     │   25%   │    +     │   15%   │
      └────┬────┘          └────┬────┘          └────┬────┘
           │                     │                     │
      ┌────┴────┐          ┌────┴────┐          ┌────┴────┐
      │ 盈利能力  │          │ 趋势分析  │          │ 波动率   │
      │ 成长能力  │          │ 量价配合  │          │ 回撤控制  │
      │ 估值水平  │          │ 资金流向  │          │ 集中度   │
      │ 财务健康  │          │ 筹码分布  │          │ 流动性   │
      └─────────┘          └─────────┘          └─────────┘
```

### 哲学映射

| 哲学概念 | 量化映射 |
|---------|---------|
| **道法自然** | 尊重市场规律，不预测只评估 |
| **阴阳平衡** | 多空因子均衡配置，攻守兼备 |
| **无为而治** | 系统化评分，减少主观干预 |
| **大道至简** | 三层架构清晰可解释，不搞黑箱 |

---

## 📖 文章目录

### 📊 统计概览

> 共收录 **142** 篇研究文章，覆盖 **29** 个子分类，按 **5 大板块** 组织

| 板块 | 目录数 | 文章数 | 平均每目录 | 说明 |
|------|:------:|:------:|:----------:|------|
| **M - 模型理论** | 7 | **28** | 4.0篇 | 双引擎四层模型完整解析 |
| **I - 行业研究** | 10 | **39** | 3.9篇 | 银行/非银/地产/医药/电子/新能源/消费/周期/TMT/制造 |
| **C - 个股案例** | 5 | 16 | 3.2篇 | 沪深300/中证500/创业板/科创板/北交所分析框架 |
| **R - 研究方法论** | 5 | **28** | 5.6篇 | 工具/数据处理/回测/随笔/文献综述 |
| **O - 开源项目** | 2 | 31 | 15.5篇 | 量化交易开源项目深度解析 + AI Hedge Fund Agent系列 |
| **总计** | **29** | **142** | **4.9篇** | 覆盖量化投资全流程 |

---

### 🆕 最新文章

> 最近 10 篇发布的研究文章（完整目录见下方分类板块）

| 文章 | 分类 | 日期 | 难度 | 阅读时间 |
|------|------|------|:----:|:--------:|
| **[氧化钇（Y₂O₃）概念量化研究](./articles/I08-cyclical/I08-04-yttria-concept-quantification.md)** | I08 | 2026-06-20 | 🔴 高级 | 50min |
| **[AI-Trader：港大HKUDS出品的Agent原生AI交易平台](./articles/O01-open-source-projects/O01-12-ai-trader-agent-native-platform.md)** | O01 | 2026-06-19 | 🟡 中级 | 35min |
| **[美联储议息会议结论的量化分析方法](./articles/M03-volume-price-engine/M03-08-fomc-quantification.md)** | M03 | 2026-06-18 | 🔴 高级 | 55min |
| **[A股各板块轮动规律的量化分析方法](./articles/M03-volume-price-engine/M03-07-sector-rotation-quantification.md)** | M03 | 2026-06-17 | 🔴 高级 | 50min |
| **[WorldQuant 101因子（Alpha101）量化分析方法深度研究](./articles/M06-factor-validation/M06-07-worldquant-alpha101-analysis.md)** | M06 | 2026-06-16 | 🔴 高级 | 55min |
| **[国泰君安191因子（GTJA191）完整公式表](./articles/M06-factor-validation/M06-06-gtja191-formula-reference.md)** | M06 | 2026-06-15 | 🟢 初级 | 20min |
| **[国泰君安191因子（GTJA191）量化分析方法深度研究](./articles/M06-factor-validation/M06-05-gtja191-factor-analysis.md)** | M06 | 2026-06-15 | 🔴 高级 | 50min |
| **[A股大盘所处阶段判断的量化分析方法：多维度择时框架](./articles/M03-volume-price-engine/M03-06-market-stage-quantification.md)** | M03 | 2026-06-14 | 🔴 高级 | 45min |
| **[物理AI（Physical AI）产业链深度拆解：从虚拟智能到实体智能的五层架构](./articles/I05-semiconductor/I05-07-physical-ai-industry-chain.md)** | I05 | 2026-06-13 | 🔴 高级 | 40min |
| **[A股情绪量化方法论深度研究：大盘·行业·概念·个股四层体系](./articles/M03-volume-price-engine/M03-05-market-sentiment-quantification.md)** | M03 | 2026-06-12 | 🔴 高级 | 45min |

> 📌 **查看更多**：完整 **142** 篇文章请浏览下方 [📚 完整分类目录](#-完整分类目录)

---

### 📚 完整分类目录

#### M - 模型理论（Model Theory）

关于双引擎四层融合模型的理论阐述与方法论

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [模型总览](./articles/M01-model-overview/) | M01 | **3** | 模型架构、设计哲学、整体介绍 | [架构总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md) · [数学原理](./articles/M01-model-overview/M01-02-dual-engine-four-layer-math.md) · [回测绩效](./articles/M01-model-overview/M01-03-model-backtest-performance-evaluation.md) |
| [基本面引擎](./articles/M02-fundamental-engine/) | M02 | **3** | 盈利能力、成长能力、估值、财务健康 | [引擎概述](./articles/M02-fundamental-engine/M02-01-fundamental-engine-overview.md) · [ROE杜邦分析](./articles/M02-fundamental-engine/M02-02-roe-dupont-analysis.md) · [成长因子](./articles/M02-fundamental-engine/M02-03-growth-factor-peg-valuation.md) |
| [量价引擎](./articles/M03-volume-price-engine/) | M03 | **8** | 趋势分析、量价配合、资金流向、筹码分布、情绪量化、大盘阶段判断、板块轮动、美联储议息量化 | [引擎概述](./articles/M03-volume-price-engine/M03-01-volume-price-engine-overview.md) · [均线系统](./articles/M03-volume-price-engine/M03-02-moving-average-trend-tracking.md) · [资金流向](./articles/M03-volume-price-engine/M03-03-capital-flow-analysis.md) · [趋势分析实践](./articles/M03-volume-price-engine/M03-04-trend-analysis-best-practices.md) · [情绪量化](./articles/M03-volume-price-engine/M03-05-market-sentiment-quantification.md) · [大盘阶段判断](./articles/M03-volume-price-engine/M03-06-market-stage-quantification.md) · [板块轮动](./articles/M03-volume-price-engine/M03-07-sector-rotation-quantification.md) · [美联储议息量化](./articles/M03-volume-price-engine/M03-08-fomc-quantification.md) |
| [风控引擎](./articles/M04-risk-control/) | M04 | **3** | 波动率、回撤控制、集中度、流动性风险 | [引擎概述](./articles/M04-risk-control/M04-01-risk-control-engine-overview.md) · [VaR模型](./articles/M04-risk-control/M04-02-var-model-drawdown-control.md) · [集中度流动性](./articles/M04-risk-control/M04-03-concentration-liquidity-risk.md) |
| [融合算法](./articles/M05-fusion-algorithm/) | M05 | **3** | 加权机制、评级映射、动态调整 | [算法概述](./articles/M05-fusion-algorithm/M05-01-fusion-algorithm-overview.md) · [动态权重](./articles/M05-fusion-algorithm/M05-02-dynamic-weight-adaptive-scoring.md) · [机器学习融合](./articles/M05-fusion-algorithm/M05-03-machine-learning-factor-fusion.md) |
| [因子检验](./articles/M06-factor-validation/) | M06 | **7** | 单因子有效性、IC测试、分层回测、GTJA191因子、WorldQuant Alpha101 | [检验方法](./articles/M06-factor-validation/M06-01-factor-validation-methods.md) · [IC测试](./articles/M06-factor-validation/M06-02-ic-test-factor-validation.md) · [多因子组合](./articles/M06-factor-validation/M06-03-multi-factor-portfolio-optimization.md) · [基本面α因子](./articles/M06-factor-validation/M06-04-fundamental-alpha-factor-research.md) · [GTJA191因子](./articles/M06-factor-validation/M06-05-gtja191-factor-analysis.md) · [GTJA191公式表](./articles/M06-factor-validation/M06-06-gtja191-formula-reference.md) · [WQ Alpha101](./articles/M06-factor-validation/M06-07-worldquant-alpha101-analysis.md) |
| [模型迭代](./articles/M07-model-iteration/) | M07 | **3** | 版本更新、改进记录、回测对比 | [迭代记录](./articles/M07-model-iteration/M07-01-model-iteration-records.md) · [回测绩效](./articles/M07-model-iteration/M07-02-backtest-performance-improvement.md) · [AI应用](./articles/M07-model-iteration/M07-03-model-future-ai-applications.md) |

#### I - 行业研究（Industry Research）

特定行业的量化分析框架与案例研究

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [银行业](./articles/I01-banking/) | I01 | **3** | 银行板块因子适配、特色指标 | [分析框架](./articles/I01-banking/I01-01-banking-quant-framework.md) · [估值股息](./articles/I01-banking/I01-02-banking-valuation-dividend-strategy.md) · [区域行vs股份行](./articles/I01-banking/I01-03-regional-vs-joint-stock-banks.md) |
| [非银金融](./articles/I02-nonbank-finance/) | I02 | **3** | 保险、证券、多元金融 | [行业分析](./articles/I02-nonbank-finance/I02-01-nonbank-finance-analysis.md) · [内含价值](./articles/I02-nonbank-finance/I02-02-insurance-embedded-value-assessment.md) · [券商经纪业务](./articles/I02-nonbank-finance/I02-03-brokerage-business-cycle.md) |
| [房地产](./articles/I03-real-estate/) | I03 | **4** | 房企量化分析、三道红线 | [行业分析](./articles/I03-real-estate/I03-01-real-estate-analysis.md) · [周期择时](./articles/I03-real-estate/I03-02-real-estate-cycle-timing-strategy.md) · [REITs投资](./articles/I03-real-estate/I03-03-reits-infrastructure-investment.md) · [城市更新十五五](./articles/I03-real-estate/I03-04-urban-renewal-15th-five-year-plan.md) |
| [医药生物](./articles/I04-pharma/) | I04 | **3** | 创新药、医疗器械、CXO | [行业分析](./articles/I04-pharma/I04-01-pharma-analysis.md) · [创新药估值](./articles/I04-pharma/I04-02-innovative-drug-valuation-pipeline.md) · [医疗器械](./articles/I04-pharma/I04-03-medical-device-innovation.md) |
| [电子半导体](./articles/I05-semiconductor/) | I05 | **7** | 芯片、消费电子、半导体设备、物理AI | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) · [设备国产替代](./articles/I05-semiconductor/I05-02-semiconductor-equipment-localization.md) · [AI芯片](./articles/I05-semiconductor/I05-03-ai-chip-computing-demand.md) · [长鑫科技IPO](./articles/I05-semiconductor/I05-04-cxmt-ipo-impact-analysis.md) · [华为韬定律](./articles/I05-semiconductor/I05-05-huawei-tao-law-analysis.md) · [六氟化钨分析](./articles/I05-semiconductor/I05-06-wf6-tungsten-hexafluoride-analysis.md) · [物理AI产业链](./articles/I05-semiconductor/I05-07-physical-ai-industry-chain.md) |
| [新能源](./articles/I06-new-energy/) | I06 | **4** | 光伏、锂电、储能、新能源车 | [行业分析](./articles/I06-new-energy/I06-01-new-energy-analysis.md) · [新能源车](./articles/I06-new-energy/I06-02-new-energy-vehicle-investment.md) · [储能产业](./articles/I06-new-energy/I06-03-energy-storage-industry-analysis.md) · [算电协同](./articles/I06-new-energy/I06-04-computing-power-electricity-synergy.md) |
| [消费](./articles/I07-consumer/) | I07 | **4** | 白酒、食品饮料、家电、体育消费 | [行业分析](./articles/I07-consumer/I07-01-consumer-analysis.md) · [白酒品牌](./articles/I07-consumer/I07-02-liquor-brand-moat-analysis.md) · [家电出海](./articles/I07-consumer/I07-03-home-appliance-globalization.md) · [体育赛事影响](./articles/I07-consumer/I07-04-mega-sports-event-market-impact.md) |
| [周期](./articles/I08-cyclical/) | I08 | **4** | 钢铁、煤炭、化工、有色、稀土 | [行业分析](./articles/I08-cyclical/I08-01-cyclical-analysis.md) · [煤炭供需](./articles/I08-cyclical/I08-02-coal-supply-demand-price.md) · [有色金属](./articles/I08-cyclical/I08-03-nonferrous-metals-cycle.md) · [氧化钇概念量化](./articles/I08-cyclical/I08-04-yttria-concept-quantification.md) |
| [TMT](./articles/I09-tmt/) | I09 | **3** | 互联网、软件、传媒、通信 | [行业分析](./articles/I09-tmt/I09-01-tmt-analysis.md) · [互联网平台](./articles/I09-tmt/I09-02-internet-platform-economy-analysis.md) · [通信运营商](./articles/I09-tmt/I09-03-telecom-operator-valuation.md) |
| [制造](./articles/I10-manufacturing/) | I10 | **4** | 机械、汽车、军工、电力设备 | [行业分析](./articles/I10-manufacturing/I10-01-manufacturing-analysis.md) · [高端制造](./articles/I10-manufacturing/I10-02-high-end-manufacturing-barriers.md) · [工业机器人](./articles/I10-manufacturing/I10-03-industrial-robot-automation.md) · [SpaceX IPO影响分析](./articles/I10-manufacturing/I10-04-spacex-ipo-impact-analysis.md) |

#### C - 个股案例（Case Studies）

具体股票的深度量化评分分析

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [沪深300成分](./articles/C01-hs300/) | C01 | **3** | 大盘股深度分析 | [分析框架](./articles/C01-hs300/C01-01-hs300-component-analysis.md) · [贵州茅台](./articles/C01-hs300/C01-02-maotai-quantitative-analysis.md) · [平安银行](./articles/C01-hs300/C01-03-ping-an-deep-analysis.md) |
| [中证500成分](./articles/C02-zz500/) | C02 | **4** | 中盘股深度分析 | [分析框架](./articles/C02-zz500/C02-01-zz500-component-analysis.md) · [宁德时代](./articles/C02-zz500/C02-02-catl-quantitative-analysis.md) · [隆基绿能](./articles/C02-zz500/C02-03-longi-green-energy-deep-analysis.md) · [宁德时代凝聚态电池](./articles/C02-zz500/C02-04-catl-condensed-battery-analysis.md) |
| [创业板指](./articles/C03-chinext/) | C03 | **3** | 成长股深度分析 | [分析框架](./articles/C03-chinext/C03-01-chinext-component-analysis.md) · [迈瑞医疗](./articles/C03-chinext/C03-02-mindray-quantitative-analysis.md) · [东方财富](./articles/C03-chinext/C03-03-eastmoney-deep-analysis.md) |
| [科创板](./articles/C04-star/) | C04 | **3** | 硬科技企业分析 | [分析框架](./articles/C04-star/C04-01-star-market-analysis.md) · [中芯国际](./articles/C04-star/C04-02-smic-quantitative-analysis.md) · [寒武纪](./articles/C04-star/C04-03-cambricon-deep-analysis.md) |
| [北交所](./articles/C05-bse/) | C05 | **3** | 专精特新企业分析 | [分析框架](./articles/C05-bse/C05-01-bse-enterprise-analysis.md) · [贝特瑞](./articles/C05-bse/C05-02-btr-quantitative-analysis.md) · [吉林碳谷](./articles/C05-bse/C05-03-jilin-carbon-deep-analysis.md) |

#### R - 研究方法论（Research Methods）

研究工具、方法、思路与学术随笔

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [研究工具](./articles/R01-tools/) | R01 | **3** | 数据源、Python库、可视化工具 | [工具与数据源](./articles/R01-tools/R01-01-research-tools-and-data-sources.md) · [Python工具链](./articles/R01-tools/R01-02-python-quant-toolchain.md) · [量化平台对比](./articles/R01-tools/R01-03-quant-platform-comparison.md) |
| [数据处理](./articles/R02-data-processing/) | R02 | **3** | 数据清洗、特征工程、标准化 | [数据处理与特征工程](./articles/R02-data-processing/R02-01-data-processing-and-feature-engineering.md) · [数据清洗](./articles/R02-data-processing/R02-02-financial-data-cleaning-outliers.md) · [特征工程](./articles/R02-data-processing/R02-03-factor-construction-feature-engineering.md) |
| [回测方法](./articles/R03-backtesting/) | R03 | **4** | 回测框架、趋势判断、过拟合防范 | [回测方法与框架](./articles/R03-backtesting/R03-01-backtesting-methods-and-frameworks.md) · [交叉验证](./articles/R03-backtesting/R03-02-cross-validation-overfitting.md) · [事件驱动回测](./articles/R03-backtesting/R03-03-event-driven-backtest.md) · [ETF趋势分析](./articles/R03-backtesting/R03-04-etf-trend-analysis-framework.md) |
| [研究随笔](./articles/R04-essays/) | R04 | **4** | 投资感悟、市场观察、失败总结 | [研究随笔与感悟](./articles/R04-essays/R04-01-research-essays-and-insights.md) · [认知偏差](./articles/R04-essays/R04-02-cognitive-bias-quant-investing.md) · [量化心路历程](./articles/R04-essays/R04-03-quant-investing-journey.md) · [Serenity瓶颈投资方法论](./articles/R04-essays/R04-04-serenity-chokepoint-theory.md) |
| [文献综述](./articles/R05-literature/) | R05 | **14** | 经典论文解读、学术前沿、涨停归因 | [文献综述](./articles/R05-literature/R05-01-literature-review-and-frontier.md) · [五因子模型](./articles/R05-literature/R05-02-fama-french-five-factor.md) · [机器学习量化](./articles/R05-literature/R05-03-ml-quant-applications.md) · [涨停归因模型](./articles/R05-literature/R05-04-limit-up-attribution-model.md) · [CAPM理论详解](./articles/R05-literature/R05-05-CAPM-capital-asset-pricing-model-theory.md) · [CAPM Python实战](./articles/R05-literature/R05-06-CAPM-python-implementation.md) · [Fama-French详解](./articles/R05-literature/R05-07-fama-french-multi-factor-model-theory.md) · [Fama-French实战](./articles/R05-literature/R05-08-fama-french-python-implementation.md) · [APT理论详解](./articles/R05-literature/R05-09-apt-arbitrage-pricing-theory.md) · [APT Python实战](./articles/R05-literature/R05-10-apt-python-implementation.md) · [Markowitz理论详解](./articles/R05-literature/R05-11-markowitz-mean-variance-model-theory.md) · [Markowitz Python实战](./articles/R05-literature/R05-12-markowitz-python-implementation.md) · [Black-Litterman理论详解](./articles/R05-literature/R05-13-black-litterman-model-theory.md) · [Black-Litterman Python实战](./articles/R05-literature/R05-14-black-litterman-python-implementation.md) |

#### O - 开源项目（Open Source Projects）

量化交易领域优秀开源项目的深度解析与评估

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [开源项目](./articles/O01-open-source-projects/) | O01 | **12** | 量化交易开源项目深度解析 | [TradingAgents](./articles/O01-open-source-projects/O01-01-tradingagents-multi-agent-framework.md) · [Microsoft Qlib](./articles/O01-open-source-projects/O01-02-microsoft-qlib-ai-quant-platform.md) · [Backtrader](./articles/O01-open-source-projects/O01-03-backtrader-python-backtesting.md) · [Freqtrade](./articles/O01-open-source-projects/O01-04-freqtrade-crypto-trading-bot.md) · [StockSharp](./articles/O01-open-source-projects/O01-05-stocksharp-csharp-trading-platform.md) · [Riskfolio-Lib](./articles/O01-open-source-projects/O01-06-riskfolio-portfolio-optimization.md) · [FinRL](./articles/O01-open-source-projects/O01-07-finrl-deep-reinforcement-learning.md) · [TradeMaster](./articles/O01-open-source-projects/O01-08-trademaster-rl-trading.md) · [VN.PY](./articles/O01-open-source-projects/O01-09-vnpy-python-trading-platform.md) · [Zipline](./articles/O01-open-source-projects/O01-10-zipline-quantopian-backtesting.md) · [AI Hedge Fund](./articles/O01-open-source-projects/O01-11-ai-hedge-fund-legendary-investors.md) · [AI-Trader](./articles/O01-open-source-projects/O01-12-ai-trader-agent-native-platform.md) |
| [AI Hedge Fund Agent](./articles/O02-ai-hedge-fund-agents/) | O02 | **19** | AI对冲基金19个Agent深度解析 | [Buffett](./articles/O02-ai-hedge-fund-agents/O02-01-warren-buffett-agent.md) · [Graham](./articles/O02-ai-hedge-fund-agents/O02-02-ben-graham-agent.md) · [Munger](./articles/O02-ai-hedge-fund-agents/O02-03-charlie-munger-agent.md) · [Burry](./articles/O02-ai-hedge-fund-agents/O02-04-michael-burry-agent.md) · [Pabrai](./articles/O02-ai-hedge-fund-agents/O02-05-mohnish-pabrai-agent.md) · [Wood](./articles/O02-ai-hedge-fund-agents/O02-06-cathie-wood-agent.md) · [Fisher](./articles/O02-ai-hedge-fund-agents/O02-07-phil-fisher-agent.md) · [Lynch](./articles/O02-ai-hedge-fund-agents/O02-08-peter-lynch-agent.md) · [Growth](./articles/O02-ai-hedge-fund-agents/O02-09-growth-agent.md) · [Druckenmiller](./articles/O02-ai-hedge-fund-agents/O02-10-stanley-druckenmiller-agent.md) · [Taleb](./articles/O02-ai-hedge-fund-agents/O02-11-nassim-taleb-agent.md) · [Ackman](./articles/O02-ai-hedge-fund-agents/O02-12-bill-ackman-agent.md) · [Damodaran](./articles/O02-ai-hedge-fund-agents/O02-13-aswath-damodaran-agent.md) · [Jhunjhunwala](./articles/O02-ai-hedge-fund-agents/O02-14-rakesh-jhunjhunwala-agent.md) · [Valuation](./articles/O02-ai-hedge-fund-agents/O02-15-valuation-agent.md) · [Fundamentals](./articles/O02-ai-hedge-fund-agents/O02-16-fundamentals-agent.md) · [Technicals](./articles/O02-ai-hedge-fund-agents/O02-17-technicals-agent.md) · [Risk Manager](./articles/O02-ai-hedge-fund-agents/O02-18-risk-manager.md) · [Portfolio Manager](./articles/O02-ai-hedge-fund-agents/O02-19-portfolio-manager.md) |

---

### 📖 阅读指南

#### 按读者类型选择

| 读者类型 | 推荐阅读 | 说明 |
|---------|---------|------|
| **初学者** | [模型总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md) → [基本面引擎](./articles/M02-fundamental-engine/M02-01-fundamental-engine-overview.md) → [行业研究](./articles/I01-banking/) | 先理解方法论，再看行业应用 |
| **行业研究员** | [行业研究](./articles/I01-banking/) → [个股案例](./articles/C01-hs300/) | 关注特定行业的分析框架 |
| **量化开发者** | [研究工具](./articles/R01-tools/) → [数据处理](./articles/R02-data-processing/) → [回测方法](./articles/R03-backtesting/) | 关注工具、数据和回测方法 |
| **投资者** | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) → [个股案例](./articles/C01-hs300/) → [行业研究](./articles/I01-banking/) | 直接查看投资标的分析 |

#### 按投资场景选择

| 投资场景 | 推荐文章 | 说明 |
|---------|---------|------|
| **行业配置** | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) | 电子行业五大细分赛道对比分析 |
| **个股选择** | [沪深300分析框架](./articles/C01-hs300/C01-01-hs300-component-analysis.md) | 大盘蓝筹的量化评估与选股策略 |
| **模型构建** | [模型架构总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md) | 学习双引擎四层融合模型 |
| **工具方法** | [研究工具与数据源](./articles/R01-tools/R01-01-research-tools-and-data-sources.md) | 数据处理、回测框架等 |

#### 文章难度标识

| 标识 | 难度 | 适合读者 |
|------|------|---------|
| 🟢 初级 | beginner | 量化投资新手 |
| 🟡 中级 | intermediate | 有一定基础的投资者 |
| 🔴 高级 | advanced | 专业量化研究者 |

> 💡 **提示**：每篇文章的 Frontmatter 中都标注了 `difficulty` 和 `reading_time`，可根据自身情况选择。

---

## 📁 仓库结构

```
dao-quant-research/
├── README.md                          # 本文件：仓库首页
├── WRITING-GUIDELINES.md              # 写作规范（必读）
├── COMMIT-GUIDELINES.md               # 提交规范
├── LICENSE                            # MIT 许可证
├── CITATION.cff                       # 引用信息
├── .gitignore                         # Git 忽略规则
│
├── articles/                          # 📖 研究文章（核心目录）
│   ├── M01-model-overview/            # 模型总览 (1篇)
│   ├── M02-fundamental-engine/        # 基本面引擎 (1篇)
│   ├── M03-volume-price-engine/       # 量价引擎 (1篇)
│   ├── M04-risk-control/              # 风控引擎 (1篇)
│   ├── M05-fusion-algorithm/          # 融合算法 (1篇)
│   ├── M06-factor-validation/         # 因子检验 (1篇)
│   ├── M07-model-iteration/           # 模型迭代 (1篇)
│   ├── I01-banking/                   # 银行业 (1篇)
│   ├── I02-nonbank-finance/           # 非银金融 (1篇)
│   ├── I03-real-estate/               # 房地产 (1篇)
│   ├── I04-pharma/                    # 医药生物 (1篇)
│   ├── I05-semiconductor/             # 电子半导体 (1篇)
│   ├── I06-new-energy/                # 新能源 (1篇)
│   ├── I07-consumer/                  # 消费 (1篇)
│   ├── I08-cyclical/                  # 周期 (1篇)
│   ├── I09-tmt/                       # TMT (1篇)
│   ├── I10-manufacturing/             # 制造 (1篇)
│   ├── C01-hs300/                     # 沪深300成分股 (1篇)
│   ├── C02-zz500/                     # 中证500成分股 (1篇)
│   ├── C03-chinext/                   # 创业板指 (1篇)
│   ├── C04-star/                      # 科创板 (1篇)
│   ├── C05-bse/                       # 北交所 (1篇)
│   ├── R01-tools/                     # 研究工具 (1篇)
│   ├── R02-data-processing/           # 数据处理 (1篇)
│   ├── R03-backtesting/               # 回测方法 (1篇)
│   ├── R04-essays/                    # 研究随笔 (1篇)
│   ├── R05-literature/                # 文献综述 (1篇)
│   ├── O01-open-source-projects/      # 开源项目 (1篇)
│
├── templates/                         # 📝 文章模板
│   └── article-template.md            # 标准文章模板
│
└── data/                              # 📊 研究数据（不纳入版本控制）
    └── .gitkeep
```

---

## 🏗️ 模型概述

### 双引擎四层融合模型

**Dao Quant** 采用 **"双引擎四层融合模型"** 对 A 股进行量化评估：

- **基本面引擎（60%）**：盈利能力、成长能力、估值水平、财务健康
- **量价引擎（25%）**：趋势分析、量价配合、资金流向、筹码分布
- **风控引擎（15%）**：波动率、回撤控制、集中度、流动性

最终输出 **0-100 分** 的综合评分与 **五档评级**（S/A/B/C/D）。

### 核心特色

| 特色 | 说明 |
|------|------|
| 🔄 **动态权重** | 根据市场环境自动调整各引擎权重 |
| 📊 **多因子融合** | 综合 30+ 个量化因子，避免单一指标偏差 |
| 🎯 **可解释性** | 每个评分都有明确的因子贡献分解 |
| ⚡ **实时更新** | 支持日度/周度数据更新与评分刷新 |

---

## 🚀 快速开始

### 阅读顺序建议

```
1. 了解模型 → [模型架构总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md)
2. 学习引擎 → [基本面引擎](./articles/M02-fundamental-engine/M02-01-fundamental-engine-overview.md) / [量价引擎](./articles/M03-volume-price-engine/M03-01-volume-price-engine-overview.md)
3. 查看案例 → [行业研究](./articles/I01-banking/) / [个股分析](./articles/C01-hs300/)
4. 深入方法 → [研究工具](./articles/R01-tools/) / [回测方法](./articles/R03-backtesting/)
```

### 环境准备

```bash
# 克隆仓库
git clone https://github.com/laozdao/dao-quant-research.git

# 安装依赖（如需运行代码示例）
pip install pandas numpy matplotlib seaborn tushare akshare backtrader
```

---

## 🤝 贡献指南

欢迎提交 Issue 和 PR！请参考：

- [写作规范](WRITING-GUIDELINES.md) - 文章格式与内容标准
- [提交规范](COMMIT-GUIDELINES.md) - Git 提交信息规范

### 文章命名规范

```
{分类代码}-{序号}-{文章标题英文简写}.md

示例：
- M01-01-dao-quant-model-overview.md
- I05-03-ai-chip-computing-demand.md
- C01-02-maotai-quantitative-analysis.md
```

---

## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

---

## 📬 联系方式

- GitHub Issues: [提交问题或建议](https://github.com/laozdao/dao-quant-research/issues)
- 邮箱: laozdao@126.com

---

<p align="center">
  <em>"知者不惑，仁者不忧，勇者不惧"</em><br>
  <strong>以量化之道，探投资之真</strong>
</p>
