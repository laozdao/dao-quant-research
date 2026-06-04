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

**Dao Quant Research** 是一个专注于 **中国 A 股市场量化分析** 的研究知识库，收录 **108** 篇 Markdown 格式的研究文章，系统性地记录和分享基于"双引擎四层融合模型"的量化分析研究。

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

> 共收录 **108** 篇研究文章，覆盖 **28** 个子分类，按 **5 大板块** 组织

| 板块 | 目录数 | 文章数 | 平均每目录 | 说明 |
|------|:------:|:------:|:----------:|------|
| **M - 模型理论** | 7 | 22 | 3.1篇 | 双引擎四层模型完整解析 |
| **I - 行业研究** | 10 | 34 | 3.4篇 | 银行/非银/地产/医药/电子/新能源/消费/周期/TMT/制造 |
| **C - 个股案例** | 5 | 16 | 3.2篇 | 沪深300/中证500/创业板/科创板/北交所分析框架 |
| **R - 研究方法论** | 5 | **27** | 5.4篇 | 工具/数据处理/回测/随笔/文献综述 |
| **O - 开源项目** | 1 | 11 | 11篇 | 量化交易开源项目深度解析 |
| **总计** | **28** | **108** | **3.9篇** | 覆盖量化投资全流程 |

---

### 🆕 最新文章

> 精选推荐与最新发布的研究文章

| 文章 | 分类 | 日期 | 难度 | 阅读时间 | 摘要 |
|------|------|------|:----:|:--------:|------|
| **[老子道·芯鉴九维投资分析模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md)** | I05 | 2026-05-20 | 🔴 高级 | 45min | 以道家哲学为底层逻辑，构建覆盖电子行业五大细分赛道（半导体设备、AI芯片、半导体材料、存储芯片、消费电子）的九维量化投资分析模型 |
| **[Dao Quant 双引擎四层融合模型架构总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md)** | M01 | 2026-05-20 | 🟡 中级 | 25min | 系统阐述 Dao Quant 核心模型的整体架构、设计哲学与核心逻辑，建立整体框架认知 |
| **[基本面引擎概述](./articles/M02-fundamental-engine/M02-01-fundamental-engine-overview.md)** | M02 | 2026-05-20 | 🟡 中级 | 30min | 双引擎四层模型中占60%权重的基本面评估体系，涵盖盈利/成长/估值/财务四大维度 |
| **[量价引擎概述](./articles/M03-volume-price-engine/M03-01-volume-price-engine-overview.md)** | M03 | 2026-05-20 | 🟡 中级 | 30min | 占25%权重的趋势、量价、资金、筹码四维分析体系 |
| **[风控引擎概述](./articles/M04-risk-control/M04-01-risk-control-engine-overview.md)** | M04 | 2026-05-20 | 🟡 中级 | 25min | 占15%权重的波动率、回撤、集中度、流动性风险控制体系 |
| **[新能源行业量化分析](./articles/I06-new-energy/I06-01-new-energy-analysis.md)** | I06 | 2026-05-20 | 🟡 中级 | 35min | 光伏、锂电、储能产业周期分析与投资框架 |
| **[沪深300成分股分析框架](./articles/C01-hs300/C01-01-hs300-component-analysis.md)** | C01 | 2026-05-20 | 🟡 中级 | 30min | 大盘蓝筹的量化评估与选股策略 |
| **[ROE杜邦分析与盈利质量评估](./articles/M02-fundamental-engine/M02-02-roe-dupont-analysis.md)** | M02 | 2026-05-20 | 🟡 中级 | 30min | ROE三因素分解与盈利质量深度分析 |
| **[Python量化投资工具链实战](./articles/R01-tools/R01-02-python-quant-toolchain.md)** | R01 | 2026-05-20 | 🟡 中级 | 35min | Tushare/AkShare/Backtrader实战应用 |
| **[贵州茅台深度量化分析](./articles/C01-hs300/C01-02-maotai-quantitative-analysis.md)** | C01 | 2026-05-20 | 🟡 中级 | 40min | 品牌价值/盈利能力/现金流全面评估 |
| **[Fama-French五因子模型详解](./articles/R05-literature/R05-02-fama-french-five-factor.md)** | R05 | 2026-05-20 | 🔴 高级 | 40min | 市场/规模/价值/盈利/投资因子构建与实证 |
| **[TradingAgents多智能体LLM交易框架](./articles/O01-open-source-projects/O01-01-tradingagents-multi-agent-framework.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | TauricResearch开源的多Agent金融交易框架深度解析 |
| **[长鑫科技IPO过会：国产存储龙头上市影响量化分析](./articles/I05-semiconductor/I05-04-cxmt-ipo-impact-analysis.md)** | I05 | 2026-05-28 | 🟡 中级 | 35min | 基于Dao Quant双引擎四层融合模型，对科创板史上第二大IPO进行系统性量化分析，综合评分63.45分，评级C（中性） |
| **[华为韬(τ)定律深度解析：后摩尔时代的芯片产业新范式](./articles/I05-semiconductor/I05-05-huawei-tao-law-analysis.md)** | I05 | 2026-05-29 | 🔴 高级 | 40min | 基于Dao Quant双引擎四层融合模型，深度解析华为韬定律技术原理与产业影响，识别先进封装、Chiplet投资机会，综合评分67.5分，评级B（推荐） |
| **[Backtrader回测框架](./articles/O01-open-source-projects/O01-03-backtrader-python-backtesting.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | 21.7k+ Stars，最受欢迎的Python量化回测框架之王 |
| **[Freqtrade加密货币机器人](./articles/O01-open-source-projects/O01-04-freqtrade-crypto-trading-bot.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | 32k+ Stars，最流行的开源加密货币量化交易机器人 |
| **[StockSharp C#交易平台](./articles/O01-open-source-projects/O01-05-stocksharp-csharp-trading-platform.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | C#开发的算法交易平台，支持股票/外汇/加密货币多市场 |
| **[Riskfolio-Lib组合优化](./articles/O01-open-source-projects/O01-06-riskfolio-portfolio-optimization.md)** | O01 | 2026-05-27 | 🟡 中级 | 30min | Python投资组合优化库，现代投资组合理论实现 |
| **[FinRL深度强化学习](./articles/O01-open-source-projects/O01-07-finrl-deep-reinforcement-learning.md)** | O01 | 2026-05-27 | 🔴 高级 | 40min | AI4Finance出品，深度强化学习量化交易框架 |
| **[TradeMaster强化学习平台](./articles/O01-open-source-projects/O01-08-trademaster-rl-trading.md)** | O01 | 2026-05-27 | 🔴 高级 | 35min | NTU出品，专注强化学习的量化交易平台 |
| **[VN.PY国产量化框架](./articles/O01-open-source-projects/O01-09-vnpy-python-trading-platform.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | 国内最流行的Python量化交易框架，支持CTP接口 |
| **[宁德时代凝聚态电池技术突破：500Wh/kg航空级电池的商业化之路](./articles/C02-zz500/C02-04-catl-condensed-battery-analysis.md)** | C02 | 2026-06-01 | 🔴 高级 | 40min | 基于Dao Quant双引擎四层融合模型，解析宁德时代凝聚态电池500Wh/kg技术原理，分析航空+汽车双轮驱动商业化路径，综合评分70.95分，评级B（推荐） |
| **[城市更新十五五规划出炉：十五万亿级市场与投资机遇分析](./articles/I03-real-estate/I03-04-urban-renewal-15th-five-year-plan.md)** | I03 | 2026-05-31 | 🟡 中级 | 35min | 基于Dao Quant双引擎四层融合模型，解读城市更新十五五规划核心指标与15-20万亿市场空间，识别建筑、建材、地产、管网投资机会，综合评分65.8分，评级B（推荐） |
| **[算电协同提速：AI设施用电激增与绿色算力投资机遇](./articles/I06-new-energy/I06-04-computing-power-electricity-synergy.md)** | I06 | 2026-05-30 | 🟡 中级 | 35min | 基于Dao Quant双引擎四层融合模型，分析AI数据中心用电量激增现象，识别液冷、绿电、储能投资机会，综合评分67.25分，评级B（推荐） |
| **[Zipline回测引擎](./articles/O01-open-source-projects/O01-10-zipline-quantopian-backtesting.md)** | O01 | 2026-05-27 | 🟡 中级 | 35min | Quantopian出品的机构级Python回测框架 |
| **[AI Hedge Fund传奇投资者Agent](./articles/O01-open-source-projects/O01-11-ai-hedge-fund-legendary-investors.md)** | O01 | 2026-06-04 | 🟡 中级 | 35min | 25k+ Stars，用19个LLM Agent模拟巴菲特/芒格/林奇等传奇投资者协作决策 |
| **[CAPM资本资产定价模型详解](./articles/R05-literature/R05-05-CAPM-capital-asset-pricing-model-theory.md)** | R05 | 2026-06-01 | 🟡 中级 | 35min | 系统阐述CAPM理论基础、核心公式、Beta系数计算、假设条件及A股实证分析 |
| **[CAPM模型Python实战指南](./articles/R05-literature/R05-06-CAPM-python-implementation.md)** | R05 | 2026-06-01 | 🟡 中级 | 40min | 基于Python实现CAPM完整分析流程，涵盖数据获取、Beta计算、Alpha分析与可视化 |
| **[Fama-French多因子模型详解](./articles/R05-literature/R05-07-fama-french-multi-factor-model-theory.md)** | R05 | 2026-06-01 | 🔴 高级 | 45min | 深入解析三因子与五因子模型理论基础、因子构建方法、实证检验及A股应用 |
| **[Fama-French多因子模型Python实战指南](./articles/R05-literature/R05-08-fama-french-python-implementation.md)** | R05 | 2026-06-01 | 🔴 高级 | 50min | 基于Python实现Fama-French因子构建、回归分析，投资组合构建与回测 |
| **[APT套利定价理论详解](./articles/R05-literature/R05-09-apt-arbitrage-pricing-theory.md)** | R05 | 2026-06-01 | 🔴 高级 | 40min | 深入解析APT理论基础、与CAPM比较、因子识别方法、实证检验及量化应用 |
| **[APT套利定价理论Python实战指南](./articles/R05-literature/R05-10-apt-python-implementation.md)** | R05 | 2026-06-01 | 🔴 高级 | 45min | 基于Python实现APT因子识别、载荷估计、Fama-MacBeth回归与套利策略 |
| **[Markowitz均值-方差模型理论详解](./articles/R05-literature/R05-11-markowitz-mean-variance-model-theory.md)** | R05 | 2026-06-01 | 🔴 高级 | 45min | 深入解析Markowitz均值-方差优化模型的理论基础、有效前沿构建、最优组合求解及A股应用 |
| **[Markowitz均值-方差模型Python实战指南](./articles/R05-literature/R05-12-markowitz-python-implementation.md)** | R05 | 2026-06-01 | 🔴 高级 | 50min | 基于Python实现有效前沿构建、最优组合求解、风险贡献分析与回测验证 |
| **[Black-Litterman模型理论详解](./articles/R05-literature/R05-13-black-litterman-model-theory.md)** | R05 | 2026-06-01 | 🔴 高级 | 45min | 深入解析BL模型贝叶斯框架、观点融合机制、参数设定及资产配置应用 |
| **[Black-Litterman模型Python实战指南](./articles/R05-literature/R05-14-black-litterman-python-implementation.md)** | R05 | 2026-06-01 | 🔴 高级 | 50min | 基于Python实现BL模型完整流程，涵盖均衡收益计算、观点设定、后验融合与回测 |

---

### 📚 完整分类目录

#### M - 模型理论（Model Theory）

关于双引擎四层融合模型的理论阐述与方法论

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [模型总览](./articles/M01-model-overview/) | M01 | **3** | 模型架构、设计哲学、整体介绍 | [架构总览](./articles/M01-model-overview/M01-01-dao-quant-model-overview.md) · [数学原理](./articles/M01-model-overview/M01-02-dual-engine-four-layer-math.md) · [回测绩效](./articles/M01-model-overview/M01-03-model-backtest-performance-evaluation.md) |
| [基本面引擎](./articles/M02-fundamental-engine/) | M02 | **3** | 盈利能力、成长能力、估值、财务健康 | [引擎概述](./articles/M02-fundamental-engine/M02-01-fundamental-engine-overview.md) · [ROE杜邦分析](./articles/M02-fundamental-engine/M02-02-roe-dupont-analysis.md) · [成长因子](./articles/M02-fundamental-engine/M02-03-growth-factor-peg-valuation.md) |
| [量价引擎](./articles/M03-volume-price-engine/) | M03 | **4** | 趋势分析、量价配合、资金流向、筹码分布 | [引擎概述](./articles/M03-volume-price-engine/M03-01-volume-price-engine-overview.md) · [均线系统](./articles/M03-volume-price-engine/M03-02-moving-average-trend-tracking.md) · [资金流向](./articles/M03-volume-price-engine/M03-03-capital-flow-analysis.md) · [趋势分析实践](./articles/M03-volume-price-engine/M03-04-trend-analysis-best-practices.md) |
| [风控引擎](./articles/M04-risk-control/) | M04 | **3** | 波动率、回撤控制、集中度、流动性风险 | [引擎概述](./articles/M04-risk-control/M04-01-risk-control-engine-overview.md) · [VaR模型](./articles/M04-risk-control/M04-02-var-model-drawdown-control.md) · [集中度流动性](./articles/M04-risk-control/M04-03-concentration-liquidity-risk.md) |
| [融合算法](./articles/M05-fusion-algorithm/) | M05 | **3** | 加权机制、评级映射、动态调整 | [算法概述](./articles/M05-fusion-algorithm/M05-01-fusion-algorithm-overview.md) · [动态权重](./articles/M05-fusion-algorithm/M05-02-dynamic-weight-adaptive-scoring.md) · [机器学习融合](./articles/M05-fusion-algorithm/M05-03-machine-learning-factor-fusion.md) |
| [因子检验](./articles/M06-factor-validation/) | M06 | **3** | 单因子有效性、IC测试、分层回测 | [检验方法](./articles/M06-factor-validation/M06-01-factor-validation-methods.md) · [IC测试](./articles/M06-factor-validation/M06-02-ic-test-factor-validation.md) · [多因子组合](./articles/M06-factor-validation/M06-03-multi-factor-portfolio-optimization.md) |
| [模型迭代](./articles/M07-model-iteration/) | M07 | **3** | 版本更新、改进记录、回测对比 | [迭代记录](./articles/M07-model-iteration/M07-01-model-iteration-records.md) · [回测绩效](./articles/M07-model-iteration/M07-02-backtest-performance-improvement.md) · [AI应用](./articles/M07-model-iteration/M07-03-model-future-ai-applications.md) |

#### I - 行业研究（Industry Research）

特定行业的量化分析框架与案例研究

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [银行业](./articles/I01-banking/) | I01 | **3** | 银行板块因子适配、特色指标 | [分析框架](./articles/I01-banking/I01-01-banking-quant-framework.md) · [估值股息](./articles/I01-banking/I01-02-banking-valuation-dividend-strategy.md) · [区域行vs股份行](./articles/I01-banking/I01-03-regional-vs-joint-stock-banks.md) |
| [非银金融](./articles/I02-nonbank-finance/) | I02 | **3** | 保险、证券、多元金融 | [行业分析](./articles/I02-nonbank-finance/I02-01-nonbank-finance-analysis.md) · [内含价值](./articles/I02-nonbank-finance/I02-02-insurance-embedded-value-assessment.md) · [券商经纪业务](./articles/I02-nonbank-finance/I02-03-brokerage-business-cycle.md) |
| [房地产](./articles/I03-real-estate/) | I03 | **4** | 房企量化分析、三道红线 | [行业分析](./articles/I03-real-estate/I03-01-real-estate-analysis.md) · [周期择时](./articles/I03-real-estate/I03-02-real-estate-cycle-timing-strategy.md) · [REITs投资](./articles/I03-real-estate/I03-03-reits-infrastructure-investment.md) · [城市更新十五五](./articles/I03-real-estate/I03-04-urban-renewal-15th-five-year-plan.md) |
| [医药生物](./articles/I04-pharma/) | I04 | **3** | 创新药、医疗器械、CXO | [行业分析](./articles/I04-pharma/I04-01-pharma-analysis.md) · [创新药估值](./articles/I04-pharma/I04-02-innovative-drug-valuation-pipeline.md) · [医疗器械](./articles/I04-pharma/I04-03-medical-device-innovation.md) |
| [电子半导体](./articles/I05-semiconductor/) | I05 | **5** | 芯片、消费电子、半导体设备 | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) · [设备国产替代](./articles/I05-semiconductor/I05-02-semiconductor-equipment-localization.md) · [AI芯片](./articles/I05-semiconductor/I05-03-ai-chip-computing-demand.md) · [长鑫科技IPO](./articles/I05-semiconductor/I05-04-cxmt-ipo-impact-analysis.md) · [华为韬定律](./articles/I05-semiconductor/I05-05-huawei-tao-law-analysis.md) |
| [新能源](./articles/I06-new-energy/) | I06 | **4** | 光伏、锂电、储能、新能源车 | [行业分析](./articles/I06-new-energy/I06-01-new-energy-analysis.md) · [新能源车](./articles/I06-new-energy/I06-02-new-energy-vehicle-investment.md) · [储能产业](./articles/I06-new-energy/I06-03-energy-storage-industry-analysis.md) · [算电协同](./articles/I06-new-energy/I06-04-computing-power-electricity-synergy.md) |
| [消费](./articles/I07-consumer/) | I07 | **3** | 白酒、食品饮料、家电 | [行业分析](./articles/I07-consumer/I07-01-consumer-analysis.md) · [白酒品牌](./articles/I07-consumer/I07-02-liquor-brand-moat-analysis.md) · [家电出海](./articles/I07-consumer/I07-03-home-appliance-globalization.md) |
| [周期](./articles/I08-cyclical/) | I08 | **3** | 钢铁、煤炭、化工、有色 | [行业分析](./articles/I08-cyclical/I08-01-cyclical-analysis.md) · [煤炭供需](./articles/I08-cyclical/I08-02-coal-supply-demand-price.md) · [有色金属](./articles/I08-cyclical/I08-03-nonferrous-metals-cycle.md) |
| [TMT](./articles/I09-tmt/) | I09 | **3** | 互联网、软件、传媒、通信 | [行业分析](./articles/I09-tmt/I09-01-tmt-analysis.md) · [互联网平台](./articles/I09-tmt/I09-02-internet-platform-economy-analysis.md) · [通信运营商](./articles/I09-tmt/I09-03-telecom-operator-valuation.md) |
| [制造](./articles/I10-manufacturing/) | I10 | **3** | 机械、汽车、军工、电力设备 | [行业分析](./articles/I10-manufacturing/I10-01-manufacturing-analysis.md) · [高端制造](./articles/I10-manufacturing/I10-02-high-end-manufacturing-barriers.md) · [工业机器人](./articles/I10-manufacturing/I10-03-industrial-robot-automation.md) |

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
| [研究随笔](./articles/R04-essays/) | R04 | **3** | 投资感悟、市场观察、失败总结 | [研究随笔与感悟](./articles/R04-essays/R04-01-research-essays-and-insights.md) · [认知偏差](./articles/R04-essays/R04-02-cognitive-bias-quant-investing.md) · [量化心路历程](./articles/R04-essays/R04-03-quant-investing-journey.md) |
| [文献综述](./articles/R05-literature/) | R05 | **14** | 经典论文解读、学术前沿、涨停归因 | [文献综述](./articles/R05-literature/R05-01-literature-review-and-frontier.md) · [五因子模型](./articles/R05-literature/R05-02-fama-french-five-factor.md) · [机器学习量化](./articles/R05-literature/R05-03-ml-quant-applications.md) · [涨停归因模型](./articles/R05-literature/R05-04-limit-up-attribution-model.md) · [CAPM理论详解](./articles/R05-literature/R05-05-CAPM-capital-asset-pricing-model-theory.md) · [CAPM Python实战](./articles/R05-literature/R05-06-CAPM-python-implementation.md) · [Fama-French详解](./articles/R05-literature/R05-07-fama-french-multi-factor-model-theory.md) · [Fama-French实战](./articles/R05-literature/R05-08-fama-french-python-implementation.md) · [APT理论详解](./articles/R05-literature/R05-09-apt-arbitrage-pricing-theory.md) · [APT Python实战](./articles/R05-literature/R05-10-apt-python-implementation.md) · [Markowitz理论详解](./articles/R05-literature/R05-11-markowitz-mean-variance-model-theory.md) · [Markowitz Python实战](./articles/R05-literature/R05-12-markowitz-python-implementation.md) · [Black-Litterman理论详解](./articles/R05-literature/R05-13-black-litterman-model-theory.md) · [Black-Litterman Python实战](./articles/R05-literature/R05-14-black-litterman-python-implementation.md) |

#### O - 开源项目（Open Source Projects）

量化交易领域优秀开源项目的深度解析与评估

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [开源项目](./articles/O01-open-source-projects/) | O01 | **11** | 量化交易开源项目深度解析 | [TradingAgents](./articles/O01-open-source-projects/O01-01-tradingagents-multi-agent-framework.md) · [Microsoft Qlib](./articles/O01-open-source-projects/O01-02-microsoft-qlib-ai-quant-platform.md) · [Backtrader](./articles/O01-open-source-projects/O01-03-backtrader-python-backtesting.md) · [Freqtrade](./articles/O01-open-source-projects/O01-04-freqtrade-crypto-trading-bot.md) · [StockSharp](./articles/O01-open-source-projects/O01-05-stocksharp-csharp-trading-platform.md) · [Riskfolio-Lib](./articles/O01-open-source-projects/O01-06-riskfolio-portfolio-optimization.md) · [FinRL](./articles/O01-open-source-projects/O01-07-finrl-deep-reinforcement-learning.md) · [TradeMaster](./articles/O01-open-source-projects/O01-08-trademaster-rl-trading.md) · [VN.PY](./articles/O01-open-source-projects/O01-09-vnpy-python-trading-platform.md) · [Zipline](./articles/O01-open-source-projects/O01-10-zipline-quantopian-backtesting.md) · [AI Hedge Fund](./articles/O01-open-source-projects/O01-11-ai-hedge-fund-legendary-investors.md) |

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

## 🚀 快速开始

### 对于读者

1. **按分类浏览**：点击上方目录中的分类链接
2. **按标签搜索**：查看文章 Frontmatter 中的 `tags` 字段
3. **查看最新**：关注 `status: published` 且日期较新的文章

### 对于贡献者

1. **阅读写作规范**：[WRITING-GUIDELINES.md](./WRITING-GUIDELINES.md)
2. **阅读提交规范**：[COMMIT-GUIDELINES.md](./COMMIT-GUIDELINES.md)
3. **复制文章模板**：[templates/article-template.md](./templates/article-template.md)
4. **选择分类目录**：根据内容选择 `articles/` 下的对应子目录
5. **按规范命名文件**：`{分类代码}-{序号}-{kebab-case标题}.md`
6. **填写 Frontmatter**：所有必填字段必须完整
7. **质量检查**：对照规范中的检查清单自查

---

## 📝 写作规范速查

### 文件命名
```
M01-01-model-overview.md           # 模型总览第1篇
I07-02-baijiu-sector.md            # 行业研究-消费-白酒板块
C01-05-pingan-bank.md              # 个股案例-平安银行
```

### Frontmatter 必填字段
```yaml
---
title: "文章标题"
date: "2026-05-20"
author: "laozdao"
category: "M01"                       # 分类代码
tags: ["标签1", "标签2", "标签3"]
status: "published"                   # draft/published/archived/updated
version: "1.0"
summary: "100-200字摘要"
difficulty: "intermediate"            # beginner/intermediate/advanced
reading_time: 30                      # 预估阅读时间（分钟）
---
```

### 文章结构
```markdown
# 标题

> 一句话摘要（30字以内）

## 一、引言/背景
## 二、核心内容
## 三、结论与启示
## 四、附录（可选）
## 参考文献

---
> ⚠️ 免责声明：本文仅供学习研究，不构成投资建议。
```

---

## 📋 引用方式

如果本仓库的研究内容对您有帮助，请引用：

### BibTeX

```bibtex
@misc{laozdao2026daoquant,
  author       = {laozdao},
  title        = {Dao Quant Research: A-Share Quantitative Analysis Study},
  year         = {2026},
  howpublished = {\url{https://github.com/laozdao/dao-quant-research}},
  note         = {Accessed: YYYY-MM-DD}
}
```

### APA

> laozdao. (2026). *Dao Quant Research: A-Share Quantitative Analysis Study*. GitHub. https://github.com/laozdao/dao-quant-research

---

## 🤝 参与贡献

欢迎通过以下方式参与：

- 🐛 提交 [Issue](../../issues) — 报告错误或提出建议
- 📝 提交 PR — 按写作规范提交新文章或修正
- 💬 讨论 — 在 Issue 中分享你的量化研究心得

---

## 📄 许可证

本仓库采用 [MIT License](./LICENSE) 开源。

---

## ⚠️ 免责声明

> **本仓库所有研究文章仅供学习研究交流，不构成任何投资建议。**
>
> - 模型评分基于历史数据，过往表现不代表未来收益
> - 股市有风险，投资需谨慎
> - 作者不对因使用本仓库内容而造成的任何损失承担责任
