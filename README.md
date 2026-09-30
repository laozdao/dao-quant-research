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

**Dao Quant Research** 是一个专注于 **中国 A 股市场量化分析** 的研究知识库，收录 **261** 篇 Markdown 格式的研究文章，系统性地记录和分享基于"双引擎四层融合模型"的量化分析研究。

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

> 共收录 **261** 篇研究文章，覆盖 **29** 个子分类，按 **5 大板块** 组织

| 板块 | 目录数 | 文章数 | 平均每目录 | 说明 |
|------|:------:|:------:|:----------:|------|
| **M - 模型理论** | 7 | **28** | 4.0篇 | 双引擎四层模型完整解析 |
| **I - 行业研究** | 10 | **72** | 7.2篇 | 银行/非银/地产/医药/电子/新能源/消费/周期/TMT/制造 |
| **C - 个股案例** | 5 | 16 | 3.2篇 | 沪深300/中证500/创业板/科创板/北交所分析框架 |
| **R - 研究方法论** | 5 | **112** | 22.4篇 | 工具/数据处理/回测/随笔/文献综述 |
| **O - 开源项目** | 2 | 32 | 16篇 | 量化交易开源项目深度解析 + AI Hedge Fund Agent系列 |
| **总计** | **29** | **261** | **9.0篇** | 覆盖量化投资全流程 |

---

### 🆕 最新文章

> 最近 10 篇发布的研究文章（完整目录见下方分类板块）

| 文章 | 分类 | 日期 | 难度 | 阅读时间 |
|------|------|------|:----:|:--------:|
| **[买断式逆回购深度解码：1.2万亿的真正收款人是财政部不是企业、债券所有权过户意味着央行正在从最后贷款人变成首要持债人——10月8日1.2万亿操作净投放仅2000亿央行对冲政府债发行的实质是给财政融资提供承接环境而不是给实体注入流动性、质押式与买断式在法理和会计上是完全不同的事质押是抵押贷款买断是真实买卖、央行资产负债表正从持有对银行的债权转向持有政府的债券从最后贷款人变成首要持债人、有期限可回滚名字不叫量化宽松但它做的正是同一件事、多价位中标不生成统一利率是刻意拒绝创造可以被市场用来倒逼降息的新价格信号、财政货币协同真实含义不是货币宽松而是财政获得更稳定融资条件央行提供的是流动性环境不是最终偿付能力问题从市场愿不愿意买变成货币要不要配合、政策意图可观测性下降决定不再发生在一个公告里而分散在每一天的操作中、能读懂DR007日常波动的人比只能读公告的人拥有真正的信息优势、押品荒是银行资产端的流动性质量不高不是央行总量不够买断式逆回购治的是表内结构不是资金总量、押品荒不是周期问题是结构问题买断式逆回购不是应急工具很可能成为常态化操作、政策工具的专业化与市场解读的通俗化之间正在裂开越来越宽的鸿沟每一次粗颗粒的误读都会在情绪上制造一次偏离而偏离最终会向事实回归、误把1.2万亿当净投放1.2万亿总操作规模是媒体口径净投放才是货币口径两者相差六倍、把买断式逆回购当成债市单边走牛的保证量随价动不是单向加码、三大最锋利判断第一受益人是财政、资产端向政府信用迁移是QE另一条路径、政策信号从公告转移到日常操作——与A股债市/银行/基建/中下游产业链投资机会深度量化分析](./articles/R04-essays/R04-88-pboc-outright-reverse-repo-1-2-trillion-treasury-fiscal-credit-coordination-ownership-transfer-quantitative-easing-a-share-quantification.md)** | R04 | 2026-10-01 | 🔴 高级 | 55min |
| **[9月PMI深度解码：50.1是两个经济体被平均出来的数字，建筑业新订单45.7说明工地在吃老本，需求不足时的通胀本质是一种税收——生产指数51.7大幅上行vs新订单50.5微降在手订单46.0收缩生产跑在需求前面、企业为何在订单不足时提速开工因为停产比开工更贵设备折旧工人流失供应链断链这轮生产回暖里有一部分是维持性生产不是扩张性生产、原材料库存48.2产成品库存47.6双双收缩企业既不敢补库也不敢停产对中长期需求审慎用开工维持生存、从业人员48.4下行企业没因短期生产回暖扩招人如果真相信需求回来了一定会先招人不招人就是不信、50.1和49.9几乎没有经济含义用单月站上50推断经济反转是方法论错误、该看新订单减产成品库存差值新订单代表需求流入产成品库存代表未消化供给靠新订单抬升是健康改善靠库存主动去化是衰退式改善、价格冲上60.8%环比上行4.2pct出厂价格54.0%上行3.6pct本轮上涨来自外部供给冲击非国内总需求过热、购进价与出厂价差值是被挤压掉的利润空间、需求不足时通胀本质是税收不创造新需求只重新分配利润从弱者流向强者从议价能力弱中下游流向掌握资源的产业链上游、无声财富转移规模以万亿计不会出现在任何政策文件里、与工业利润结构繁荣是同一枚硬币同一件事从工业利润看是结构性繁荣从PMI看是成本挤压、外部大宗上行是输入性国内宽松陷两难宽松加剧价格压力收紧掐灭弱修复政策选择空间比表面窄得多、建筑业商务活动指数50.3环比大幅上行3.4pct是非制造业反弹最大贡献但新订单45.7深度收缩商务活动扩张说明工地忙新订单收缩说明未来活没签下来存量赶工建筑业在吃老本、PSL钱要变新订单需走完项目储备审批开工流程这中间有时滞建筑业新订单45.7很可能决定2027年上半年的基建成色是比50.1重要得多市场关注最少的一个数字、服务业业务活动预期55.5乐观但新订单46.5收缩预期好于订单是弱修复阶段典型形态最容易在情绪里误读成拐点、K型分化更准确说法是两个经济体被平均成一个数字大企业50.6扩张中型49.7小型48.9收缩电信互联网货币金融保险景气55以上房地产资本市场服务收缩、宏观数据在改善微观体感在恶化两者都真因为测的不是一个东西宏观测多周期加权平均企业只活在自己的周期、判断经济看平均数不如看分布双峰分布平均数落谷底不描述任何一方与1-8月工业利润62%增量来自一个行业同一方法论第二次显形、宽松传导不到需求只会推高价格PSL扩围科创再贷款100%支农支小改善供给端PMI揭示矛盾在需求端、政策宽松边际效力在衰减而价格副作用在上升、经济主要约束从资金变成意愿货币政策到达边界收入预期就业信心房地产稳定民间投资回报率不是降息能解决、政策能提供弹药提供不了靶标这份PMI最清楚信号就是靶标还没有出现、三个最锋利判断维持性生产可持续性天然不足单月站上50不构成趋势性结论、需求不足时通胀是无声财富转移做多上游和做多中下游是方向相反的两个交易、建筑业新订单45.7决定2027上半年基建成色若不能回扩张区间四季度基建高景气是一次性脉冲而非周期起点、把站上50.1当全面强复苏/价格大涨当内需过热/建筑业大涨当地产回暖三大误区、新订单减产成品库存差值购进与出厂剪刀差建筑业新订单中小企业PMI五大跟踪信号——与A股上游资源/中下游制造/基建链/地产链/服务业产业链投资机会深度量化分析](./articles/R04-essays/R04-87-september-2026-pmi-two-economies-maintenance-production-inflation-tax-construction-new-orders-a-share-quantification.md)** | R04 | 2026-09-30 | 🔴 高级 | 55min |
| **[央行四工具PSL结构性货币政策深度解码：算力网进了PSL意味着AI基建正在公用事业化、这场准财政操作真正的瓶颈不是钱而是没人借——PSL投的是基建资本金再贷款补贴特定领域信贷本应由财政做的事现在由货币政策执行（土地收入下行财政扩张空间被压缩结构性定向投放是货币工具里最接近财政补贴的形态）、科创再贷款支持比例从60%提到100%补贴给谁取决于定价权在谁手里资金供给充足需求不足时补贴未必落到企业身上很可能变成银行净息差、四项工具改善的是资金成本不是资金需求弹药充足和有人开枪是两件事、PSL性质从商业投资品变成准公共品拿到PSL的项目天然具备成本优势成本优势在竞争里迅速转化为价格优势低成本政策资金建设产能会拉低整个行业算力价格给算力价格战提供弹药供给端获得补贴的行业商品化和价格战来得更快、不是雨露均沾的利好是一次成本优势的再分配、补贴不会凭空落到终端会停在链条上有定价权的那一环、判断科创再贷款政策真实效果不能看投放规模要看科创企业实际贷款利率有没有下行规模是政策意愿利率才是政策落点、再贷款不承担兜底坏账责任坏账是滞后的投放期量增风险显现推后两三年用再贷款推动信贷本质是把风险确认时点从当下推迟到未来、央行被迫承担准财政职能不再只管总量水龙头还要管水流向哪个池子、财政没有能力做定向货币政策只好替它做定向全面降准是普惠的无法定向结构性工具可以精确到行业、隔夜逆回购修利率管道四工具在这条管道上装一排定向阀门一个管价格一个管流向组合起来才是完整框架升级、PSL三段历史2014-2019棚改货币化2022-2023保交楼城中村2026六张网同一个工具投向不同宏观后果可以完全不同市场用棚改记忆理解本轮PSL是错误套用同一段历史、本轮PSL不会推高资产价格因为没有最终付款人PSL不是凭空创造需求是把未来财政支出提前了水没有消失只是被挪到前面、能产生现金流的网是投资产生不了的网是负债、资产负债表式谨慎不是借不到钱是不想借钱、政策工具箱充裕政策意愿明确但工具能改变资金分配改变不了需求意愿、方向明确力度可控不要把工具发布直接等同于业绩兑现、三个最锋利判断算力网进PSL是AI基建公用事业化的开始公用事业化另一面是价格战、60%到100%补贴真实落点要看利率而不是规模判断成败唯一标准是科创企业实际融资成本有没有降、本轮宽松本质是央行在财政受限条件下承担准财政职能边界也在财政货币政策能解决分配问题解决不了需求问题能提供弹药提供不了靶标——与A股算力网/新型电网/通信网/硬科技设备更新/传统基建/民营中小微产业链投资机会深度量化分析](./articles/R04-essays/R04-86-pboc-structural-tools-psl-quasi-fiscal-computing-power-network-utility-a-share-quantification.md)** | R04 | 2026-09-30 | 🔴 高级 | 55min |
| **[科技部十五五科技攻关发布会深度解码：重构创新链与产业链匹配机制、从科研产出转向产业可用——全链条攻关AI量子生物制造集成电路工业母机三大领域分层布局、支持企业常态化参与国家科技创新决策出题人部分改变用应用考核技术用市场考核产品、创新资源向企业集聚大企业带动中小企龙头出题中小企业配套产学研协同、基础研究重大转向逼迫企业走向从0到1但政策不改变商业现实只有现金流充裕战略定力强龙头有能力大规模投入、高水平自立自强不是闭门造车主动参与全球科技治理避免被规则层面卡脖子、京津冀长三角粤港澳三大科创中心研发经费占全国58.8%PCT国际专利占全国81.1%全国创新格局三大高地引领各地差异化配套、机制改革慢变量财政资源有限不可能所有赛道齐头并进开放合作存在外部不确定性三重现实约束、国家重大专项出题人变化验收导向论文到应用考核、区分真正深度参与专项龙头vs受益平台开放的专精特新vs蹭题材概念股三类标的——与A股AI/量子/生物制造/集成电路/工业母机/三深科创产业链投资机会深度量化分析](./articles/R04-essays/R04-85-sci-tech-ministry-15th-fyp-key-technology-conquest-innovation-chain-industry-chain-a-share-quantification.md)** | R04 | 2026-09-29 | 🔴 高级 | 55min |
| **[新型电池十五五规划深度解码：PPB级缺陷率是一份禁止新玩家入场的公告、电池正从汽车零部件变成能源基础设施——十亿分之一缺陷率是政策用质量标准完成产能出清的优雅而残酷的设计、终结低价内卷死结的政策工具（门槛足够高低端产能自然出清价格战烈度下降）、2030年全固态电池初步实现规模化应用逐字拆解初步规模化应用三个词全在往回收、政策语言与市场解读之间的语义损耗副词被丢弃名词被放大、固态电解质/良率/成本工程化大山一座没搬走、钠电池定位是补充而非替代液流电池主攻长时储能三条路线互补共存是供应链风险对冲、最被忽略的定义切换电池从汽车零部件升级为新型能源体系基础支柱零部件是周期股基础设施是公用事业属性、最被低估的四个字算电协同AI数据中心成为电池新客户从零到一的需求引擎算力扩张尽头是电力问题电力问题的解法里储能不可或缺、真正的短板不是锂是软件电池仿真热失控模拟刚强度分析大量依赖海外重硬轻软是中国制造业最难改的习惯、系统集成环节BMS热管理大量故障不是电芯本身、这份规划的另一半读者在海外标准体系检测认证碳足迹数字护照实质是出海合规指南从卖产品走向参与制定国际规则、十五五命题从拼产能拼规模换成全链条综合实力这个行业已经从欢迎所有人入场走到只对少数人发牌——与A股电池材料/装备/工业软件/系统集成/储能/算电协同产业链投资机会深度量化分析](./articles/R04-essays/R04-84-new-battery-industry-15th-fyp-ppb-defect-rate-energy-infrastructure-a-share-quantification.md)** | R04 | 2026-09-29 | 🔴 高级 | 55min |
| **[工业利润15.7%深度解码：这是一个行业穿了全国的制服、利润正在变成仓库里的存货、AI繁荣的一部分成本正由电厂无声承担——电子一个行业贡献全部利润增量的62%、粗略测算剔除电子后其余工业利润增速仅约6.5%不到整体一半、累计数15.7%Vs单月4.2%用后视镜开车、辛普森悖论工业版整体利润率上升而大多数企业利润率并没有改善、利润增速15.7%营收仅6.6%八个百分点差价是价格修复而非产量修复、应收账款29.48万亿+产成品存货7.36万亿周转天数双双拉长利润没有变成现金变成仓库里的东西和别人的欠条、利润表好看现金流难看应相信现金流、上游采矿利润+35.1%电力热力利润-12.0%电价受管制涨价成本压在发电企业身上、AI算力高毛利部分建立在其消耗电力没有反映真实资源价格之上、黑色金属冶炼利润暴跌62.4%、亮眼总量数据会延迟政策响应统计乐观与体感悲观落差系统性延迟政策、外商及港澳台利润仅+2.3%私营+10.4%低于整体股份制+20.4%受益者不明显利润红利没有普惠中小民营、点状繁荣vs广谱复苏新旧动能剧烈切换——与A股AI算力链/电力公用事业/大宗商品/传统制造业/地产链产业链投资机会深度量化分析](./articles/R04-essays/R04-83-industrial-profit-15-percent-sector-uniform-mimpson-paradox-power-transfer-a-share-quantification.md)** | R04 | 2026-09-28 | 🔴 高级 | 55min |
| **[运营商Token经营深度解码：真相不是卖词元而是抢AI时代的计费权——Token被刻意包装成流量但边际成本恒定而非递减（背后是GPU/电力/折旧刚性消耗）、流量边际成本趋近零而Token每卖一份真实烧掉一次算力、把恒定成本当递减成本卖价格战必然打到所有人不赚钱、Token根本不是商品（100万Token没说清是什么液体、不同模型能力天差地别、厂商统计口径不一致、每百万Token价格是计量数量不计量质量的残缺尺子）、伪价格战用不同质商品比价必然劣币驱逐良币、国家标准不是效率工具而是信任工具、真商品不是Token而是SLA服务等级承诺、运营商是极少数能为服务等级背书的主体（全国算网一体/运维体系/政企信用/结算合规）、三大运营商路径摊开（移动Token办公室平台聚合/电信9.9元千万Token标准化消费品/联通元珠计量计费产业场景）、通信史铁律运营商从未赢过技术竞赛但一直赢着账单、AI比移动互联网更接近运营商地盘（算力/网络/边缘/电力）、全国智能算力2185 EFLOPS上架率71.4%硬件不再瓶颈、算力不等于词元硬件不等于商业价值、从比特传输者升级为智能服务分发运营商——与A股三大运营商/云厂商/智算中心/IDC/算力网络产业链投资机会深度量化分析](./articles/R04-essays/R04-82-telecom-token-operations-billing-rights-sla-pseudo-price-war-a-share-quantification.md)** | R04 | 2026-09-27 | 🔴 高级 | 55min |
| **[Meta Muse应用影响排名深度解码：AI Agent时代App的'打开权'正在被剥夺——Muse跨网站网页自动化/后台持续监控/多平台信息汇总/代办预订/邮件日程自动处理五大原生强项、比价聚合与生活预订类App首当其冲影响★★★★★、个人生活行政工具/任务型搜索/返利导购/C端自动化工具依次承压、电商平台改变流量入口而不替代履约、社交内容平台几乎无冲击且是Meta分发渠道、企业核心SaaS仅作为外部执行层不可替代记录系统——冲击大小取决于产品价值是否集中在信息搜集跨平台对比网页重复操作、最大杀伤力不是直接替代而是抢夺用户意图入口、App打开权被削弱独立App价值下降——与A股AI Agent/SaaS转型/导购电商/本地生活/办公软件产业链投资机会深度量化分析](./articles/R04-essays/R04-81-meta-muse-ai-agent-app-impact-ranking-user-intent-entrance-a-share-quantification.md)** | R04 | 2026-09-26 | 🔴 高级 | 50min |
| **[北京现房细则深度解码：不是让买房更安全、而是给房地产金融化进程画上句号——真正杀招是被忽略的主办银行制、信用定价从房企集团主体信用下沉到单个项目现金流、规模第一次从资产变成负债、取消预售融资等于取消开发商二十年最大隐性补贴即购房者无偿垫资等价于对开发商隐性加息、居民部门与开发商部门历史性脱钩、2027年底过渡期窗口必然制造抢跑供给先高后低、分化逻辑不是国企赢而是低资金成本赢、高周转基因被制度淘汰、开发商从金融公司退回制造公司、旧地产估值框架已作废——与A股房地产/代建/银行/保险/家居建材产业链投资机会深度量化分析](./articles/R04-essays/R04-80-beijing-completed-home-sales-rules-main-bank-system-scale-liability-shift-a-share-quantification.md)** | R04 | 2026-09-25 | 🔴 高级 | 55min |
| **[DeepSeek DSec沙盒基础设施深度解码：Agent时代算力竞争从GPU走向'执行环境基建'——单单元160节点/3万CPU核/250TB内存、每日运行300万个沙盒、峰值并发38万、每秒新建5000+沙盒、直面AI训练中主动'作弊'Reward-Hacking奖励劫持、Agent训练核心瓶颈从GPU/Token转移到CPU侧大规模有状态可隔离执行环境、四层沙盒统一抽象/乐高式分层镜像+3FS按需加载/有状态rollout与GPU抢占解耦/CPU调度分层治理四大底层技术、AI会主动攻击环境安全下沉到执行层、模型权重只是一半另一半是大规模有状态沙盒基础设施——与A股AI训练算力/CPU服务器/分布式存储/云原生安全产业链投资机会深度量化分析](./articles/R04-essays/R04-79-deepseek-dsec-sandbox-infrastructure-agent-training-cpu-execution-environment-a-share-quantification.md)** | R04 | 2026-09-24 | 🔴 高级 | 50min |
| **[矢量网络分析仪(VNA)深度解码：底层逻辑不是'测S参数的射频仪器'而是微波世界的'函数观测器'——双相参接收机架构同时捕获幅度+相位是它和所有测试仪器最本质的鸿沟、标量网络分析仪SNA永远替代不了、误差修正/校准SOLT/TRL矩阵求逆是灵魂而非归零、从S参数到阻抗/群时延/VSWR/TDR全衍生参数数学同源、壁垒是硬件+算法+生态三层叠加软件甚至比硬件更卡脖子、需求主线从通信转向算力/高速SerDes/毫米波雷达/光矢量网络分析仪、2025全球VNA市场5.88亿美元CAGR6.46%、外资占约70%高端毫米波货期排至一年以上、国产替代从下往上场景分层突破——与A股测试仪器/射频芯片/高速互联产业链投资机会深度量化分析](./articles/R04-essays/R04-78-vector-network-analyzer-vna-rf-test-instrument-ai-high-speed-interconnect-a-share-quantification.md)** | R04 | 2026-09-24 | 🔴 高级 | 50min |
| **[央行隔夜逆回购上限升至1万亿深度解码：1万亿买的不是水是弹药、货币宽松形态正从扩表变成扩额度、央行正被迫成为银行间市场的最后隔夜贷款人——隔夜交易占比超90%期限结构极度短期化、滚续风险与2013钱荒教训、价格型调控代价是央行向市场期限选择妥协、稳定DR001等于修复利率传导管道、9月28日-10月8日连续4个工作日开展隔夜逆回购每日操作量不超过10000亿元、此前上限6000亿元——货币政策框架转型标志性实践与A股债市/银行/流动性敏感资产投资机会深度量化分析](./articles/R04-essays/R04-77-pboc-overnight-reverse-repo-1t-ceiling-expansion-to-quota-shift-last-resort-overnight-lender-a-share-quantification.md)** | R04 | 2026-09-24 | 🔴 高级 | 55min |
| **[国内AI软件产业深度解码：模型能力快速商品化下'功能交付'转向'业务结果交付'——项目制/私有化/数据孤岛三大本土约束、深度增强/混合/薄壳外挂三层产品结构、工业/医疗/财税三大垂直赛道内在矛盾、数据复利/招标机制/基座依赖/估值错配四大结构性风险、存量深度改造+低责任垂类智能体+合规高可靠系统三条突围主线、产品价值甄别框架——全球MaaS以35.9%CAGR成最快赛道、中国Token调用量连续3周超美国、AI Agent 2030年市场突破5万亿与A股AI软件/智能体/工业软件/财税数字化产业链投资机会深度量化分析](./articles/R04-essays/R04-76-china-ai-software-industry-model-commoditization-business-result-delivery-data-moat-a-share-quantification.md)** | R04 | 2026-09-23 | 🔴 高级 | 55min |
| **[A/H地产股集体走强深度解码：存量时代定调背后是'叙事重估'而非'旧周期归来'——人民日报'绝非行业退潮'纠偏两大误区、万科A两连板/世联行我爱我家涨停、资金优先博弈存量服务赛道、2026年前8月二手房交易占比52%正式超新房成交易主力、新房销售面积同比-12.1% vs 二手房网签+10.6%一降一增完成交易结构历史性切换、户均超1.1套总量缺口消失但结构性缺口巨大、'项目公司制+主办银行制+现房销售'制度变革与房企K型分化——预期先行Beta行情下A股地产/经纪服务/物业/城市更新产业链投资机会深度量化分析](./articles/R04-essays/R04-75-ah-property-stock-surge-stock-era-narrative-revaluation-not-old-cycle-a-share-quantification.md)** | R04 | 2026-09-22 | 🔴 高级 | 55min |
| **[MLCC'电子工业大米'极端行情深度解码：不是简单周期复苏、是算力浪潮重构被动元件底层供需秩序——原厂AI高容型号上调15-35% vs 华强北现货部分料号暴涨10倍'一小时一价'、AI服务器单机用量为传统10倍、一颗高容MLCC单位产能消耗相当于7颗消费料、扩产周期18-24个月与设备良率双重刚性约束、2026全球MLCC市场1341亿元、高盛预测AI服务器MLCC需求2025-2030增长4.3倍——'冰火两重天'结构性行情与A股MLCC/上游材料/设备产业链投资机会深度量化分析](./articles/R04-essays/R04-74-mlcc-ai-server-high-capacity-shortage-10x-spot-surge-structural-divergence-a-share-quantification.md)** | R04 | 2026-09-21 | 🔴 高级 | 55min |
| **[我国高速载波与无线双模通信技术重大突破深度解码：中国电科院构建完整双模通信标准体系、双模自适应协同传输无线速率50倍提升、数据采集频率从每日96次升至1440次、国内首条双模通信单元检定流水线年均检定量提升40倍、双模通信单元全国安装超2亿只覆盖27个省级电力公司、近三年直接经济效益22.4亿元——新型电力系统末端通信升级与A股电力物联网/智能电表/双模通信产业链投资机会深度量化分析](./articles/R04-essays/R04-73-dual-mode-communication-hplc-rf-breakthrough-200m-units-27-provincial-grid-a-share-quantification.md)** | R04 | 2026-09-20 | 🔴 高级 | 55min |
| **[国务院办公厅转发十部门《关于促进房车消费的若干措施》深度解码：国办函〔2026〕82号、9条举措覆盖房车全链条、小微型自行式房车不设强制报废年限、租赁房车使用年限15年放宽至25年、C6驾照考点扩容、房车营地纳入国土空间规划'一张图'——2026年房车注册量有望破3万辆、保有量近30万辆与A股房车/露营/新能源房车产业链投资机会深度量化分析](./articles/R04-essays/R04-72-rv-consumption-measures-guo-ban-han-82-10-departments-new-energy-rv-camping-a-share-quantification.md)** | R04 | 2026-09-19 | 🔴 高级 | 55min |
| **[住建部宣布我国房地产进入存量时代深度解码：二手房交易占比从2020年27%升至2026年前8月52%、供求关系重大变化、'两个转向两个转变三化转型'、十五五'好房子+城市更新+数智住建'三大主线——《城市更新十五五规划》落地与A股城市更新/物业服务/智能建造/全屋智能产业链投资机会深度量化分析](./articles/R04-essays/R04-71-housing-stock-era-secondhand-trading-52pct-city-renewal-15th-five-year-good-house-a-share-quantification.md)** | R04 | 2026-09-18 | 🔴 高级 | 55min |
| **[医药工业发展'十五五'规划深度解码：工信部等十部门联合发布、2030年规上营收超3.5万亿元、创新药产业规模年均增速超20%、FIC首创新药占全球比例达25%、创新医疗器械上市超200个——License-out出海破千亿美元半年、从量变到质变的生物医药关键五年与A股创新药产业链投资机会深度量化分析](./articles/R04-essays/R04-70-pharma-industry-15th-five-year-plan-35t-revenue-20pct-innovative-drug-growth-fic-25pct-license-out-a-share-quantification.md)** | R04 | 2026-09-18 | 🔴 高级 | 55min |
| **[华为昇腾960超节点深度解码：4096卡高速互联、全球首个NPO近封装光学技术、最大8E FP8算力与1PB HBM容量、5500个Hi-ONE替代4.8万颗800G光模块功耗降低超550千瓦——国产算力从单芯片竞争转向系统级创新与A股光互连/液冷/AI硬件产业链投资机会深度量化分析](./articles/R04-essays/R04-69-huawei-ascend960-supernode-npo-optical-interconnect-liquid-cooling-a-share-quantification.md)** | R04 | 2026-09-18 | 🔴 高级 | 55min |
| **[文化和旅游发展'十五五'规划深度解码：2030年国内出游83亿人次、入境旅游1.9亿人次、文化及旅游相关产业增加值占GDP比重稳步提升、九大重点任务与'文旅+百业'——从假期流量到全年常量的产业跃升与A股文旅产业链投资机会深度量化分析](./articles/R04-essays/R04-68-culture-tourism-15th-five-year-plan-83b-domestic-trips-190m-inbound-tourism-a-share-quantification.md)** | R04 | 2026-09-17 | 🔴 高级 | 55min |
| **[美联储时隔三年再度加息深度解码：9月FOMC以12:0上调25个基点至3.75%-4.00%、沃什时代首次加息、2026年底点阵图中值上修至4.1%预示年内二次加息、10年期美债重新站上5%与美元指数破百——美国货币政策转鹰大拐点与A股情绪/汇率/估值三链冲击深度量化分析](./articles/R04-essays/R04-67-fed-september-2026-first-hike-warsh-era-25bp-375-400-dot-plot-41pct-a-share-three-chain-quantification.md)** | R04 | 2026-09-17 | 🔴 高级 | 55min |
| **[加密货币里程碑法案遭否决深度解码：美参议院50票对49票、比特币濒临7.5万美元、24小时11.5万人爆仓、10年期美债收益率5.045%创18年新高、美联储加息概率飙升至94.5%——全球'加密+债市+油价'三重冲击与A股风险传导路径深度量化分析](./articles/R04-essays/R04-66-crypto-clarity-act-defeat-btc-plunge-treasury-yield-5pct-hike-odds-a-share-ripple-quantification.md)** | R04 | 2026-09-16 | 🔴 高级 | 55min |
| **[电子信息制造业'十五五'规划深度解码：工信部、发改委2030年规上营收突破30万亿、集成电路全链条攻关、AI硬件底座与先进计算生态——从17.4万亿到30万亿的2.3倍增容与A股电子信息产业链投资机会深度量化分析](./articles/R04-essays/R04-65-electronic-information-manufacturing-15th-five-year-plan-30t-ic-way-ai-hardware-a-share-quantification.md)** | R04 | 2026-09-15 | 🔴 高级 | 55min |
| **[8月金融统计数据深度解码：前八月社融增量23.91万亿同比少增2.64万亿、政府债券托底与企业中长期信贷独强、住户贷款收缩1.03万亿——'企业加杠杆/居民去杠杆'结构分野与A股传导路径深度量化分析](./articles/R04-essays/R04-64-aug-2026-financial-stats-social-financing-credit-structure-m1-rebound-a-share-quantification.md)** | R04 | 2026-09-14 | 🔴 高级 | 55min |
| **[折叠屏集体'上桌'深度解码：华为Mate XT2秒罄、苹果iPhone Duo入局、阔折叠成主流形态——高端手机战火重燃与A股折叠屏产业链投资机会深度量化分析](./articles/R04-essays/R04-63-foldable-phones-huawei-mate-xt2-apple-iphone-duo-broadfold-a-share-supply-chain-quantification.md)** | R04 | 2026-09-13 | 🔴 高级 | 55min |
| **[美国8月CPI深度解码：核心通胀环比反弹、9月加息概率飙升至88.8%、AI资本开支削弱货币传导——美联储议息前夜的大类资产共振与A股传导路径量化分析](./articles/R04-essays/R04-62-us-august-cpi-core-inflation-uptick-hike-odds-ai-capex-monetary-transmission-a-share-quantification.md)** | R04 | 2026-09-12 | 🔴 高级 | 55min |
| **[苹果2026秋季发布会深度解码：iPhone Duo首款折叠机、iPhone 18全系涨价、Apple Intelligence全设备落地——端侧AI生态重构与A股果链产业链投资机会深度量化分析](./articles/R04-essays/R04-61-apple-fall-2026-keynote-iphone-duo-end-side-ai-a-share-supply-chain-quantification.md)** | R04 | 2026-09-10 | 🔴 高级 | 55min |
| **[2026年8月CPI同比上涨0.8% PPI同比上涨3.8%：PPI环比转正、再通胀信号确立、上游资源品高景气与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-60-aug-2026-cpi-ppi-reinflation-a-share-quantification.md)** | R04 | 2026-09-09 | 🔴 高级 | 55min |
| **[6G商用前夜深度解码：信息通信行业"十五五"规划适时启动6G、8000亿核心产业规模、45股3.46万亿市值——万亿级赛道开启与A股6G产业链投资机会深度量化分析](./articles/R04-essays/R04-59-ict-15th-five-year-plan-6g-commercialization-a-share-quantification.md)** | R04 | 2026-09-08 | 🔴 高级 | 55min |
| **[3000亿注资8家金融央企深度解码：未雨绸缪的前瞻性布局、4万亿资产扩张撬动效应、破净估值修复——银行保险板块估值重塑与A股投资机会深度量化分析](./articles/R04-essays/R04-58-mof-300b-financial-soe-capital-injection-bank-insurance-a-share-quantification.md)** | R04 | 2026-09-07 | 🔴 高级 | 55min |
| **[GPT-6 Astra深度解码：ARC-AGI-3得分98.6%、高盛盛赞AI牛市转折点、Token量价博弈——AGI时代开启与A股算力产业链估值重塑深度量化分析](./articles/R04-essays/R04-57-openai-gpt6-astra-agi-breakthrough-a-share-ai-computing-quantification.md)** | R04 | 2026-09-06 | 🔴 高级 | 55min |
| **[8月非农爆表深度解码：16.2万vs5.6万三倍预期、加息概率跳升58.4%、特朗普极限施压——美联储独立性与A股传导路径量化分析](./articles/R04-essays/R04-56-us-august-nfp-surprise-hike-odds-trump-rate-cut-pressure-a-share-quantification.md)** | R04 | 2026-09-05 | 🔴 高级 | 55min |
| **[工信部动力电池大会深度解码：全固态电池400Wh/kg突破、钠电池产业化、产能出清与标准话语权——锂电产业高质量发展与A股产业链估值重塑深度量化分析](./articles/R04-essays/R04-55-miit-power-battery-high-end-ai-green-global-a-share-lithium-quantification.md)** | R04 | 2026-09-04 | 🔴 高级 | 55min |
| **[十部门中小企业十五五规划深度解码：2.2万家专精特新小巨人、600个特色产业集群、国家基金二期——硬科技黄金时代与A股专精特新板块估值重塑深度量化分析](./articles/R04-essays/R04-54-sme-15th-five-year-plan-specialized-new-a-share-quantification.md)** | R04 | 2026-09-03 | 🔴 高级 | 55min |
| **[霍尔木兹海峡黑天鹅深度解码：全球债市大抛售、日债30年破3%、油价破90美元——滞胀幽灵重现与A股板块分化配置深度量化分析](./articles/R04-essays/R04-53-hormuz-strait-black-swan-global-bond-selloff-a-share-impact-quantification.md)** | R04 | 2026-09-02 | 🔴 高级 | 55min |
| **[苹果库克交棒特努斯深度解码：4万亿市值的"想象力牢笼"、Vision Pro折戟与AI Siri翻盘——CEO世纪交接下A股果链产业链估值重塑深度量化分析](./articles/R04-essays/R04-52-apple-cook-ternus-ceo-succession-a-share-supply-chain-quantification.md)** | R04 | 2026-09-01 | 🔴 高级 | 55min |
| **[商务部等7部门商品消费扩容升级实施意见深度解码：60万亿社零目标、三大十万亿级市场、四大万亿级品类——"十五五"消费革命与A股大消费板块估值重塑深度量化分析](./articles/R04-essays/R04-51-mofcom-7-ministries-commodity-consumption-expansion-upgrading-a-share-quantification.md)** | R04 | 2026-08-31 | 🔴 高级 | 55min |
| **[工信部AI应用服务商培育专项行动深度解码：2000→3000家资源池目标、四大重点任务、FDE工程师与Token采购——AI应用服务链重构与A股板块估值重塑深度量化分析](./articles/R04-essays/R04-50-miit-ai-service-provider-cultivation-action-a-share-quantification.md)** | R04 | 2026-08-31 | 🔴 高级 | 55min |
| **[《中医药振兴发展"十五五"规划》深度解码：10项量化指标、9800亿产业规模、数智化赋能与A股中医药板块估值重塑深度量化分析](./articles/R04-essays/R04-49-tcm-15th-five-year-plan-revival-development-a-share-quantification.md)** | R04 | 2026-08-31 | 🔴 高级 | 55min |
| **[沃什杰克逊霍尔首秀震惊市场：美联储加息窗口重启、前瞻指引终结、40万亿美债困局与A股防御性配置策略深度量化分析](./articles/R04-essays/R04-48-warsh-jackson-hole-hawkish-signal-fed-hike-fiscal-debt-a-share-quantification.md)** | R04 | 2026-08-29 | 🔴 高级 | 55min |
| **[央行房地产信贷管理重大改革深度解码：房贷期限30→40年、主办银行制、现房销售配套、11项旧规废止——房地产新模式下的制度重构与A股配置策略深度量化分析](./articles/R04-essays/R04-47-pboc-real-estate-credit-reform-40-year-mortgage-a-share-quantification.md)** | R04 | 2026-08-28 | 🔴 高级 | 55min |
| **[发改委8月例行发布会六大信号深度解码：8000亿两重工程、六张网三网融合、具身智能商业化、集成电路全链条攻关与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-46-ndrc-august-2026-press-conference-six-networks-ai-ic-a-share-quantification.md)** | R04 | 2026-08-28 | 🔴 高级 | 55min |
| **[上海战略性新兴产业"十五五"规划深度解码：2.1万亿目标、三大先导+十二大支柱+十大前沿产业体系与A股上海板块估值重塑深度量化分析](./articles/R04-essays/R04-45-shanghai-15th-five-year-strategic-emerging-industries-a-share-quantification.md)** | R04 | 2026-08-26 | 🔴 高级 | 55min |
| **[英伟达Q2财报大超预期深度解码：962亿营收翻倍、2028财年再增70%、供应仍是瓶颈——AI算力长坡厚雪逻辑再确认与A股算力产业链估值重塑深度量化分析](./articles/R04-essays/R04-44-nvidia-q2-earnings-ai-computing-power-a-share-quantification.md)** | R04 | 2026-08-27 | 🔴 高级 | 55min |
| **[AI办公四巨头全部入局：豆包工作压哨登场、WorkBuddy领跑、千问办公攻B端、库库AI破亿——Agent时代产业链重构与A股AI应用板块估值重塑深度量化分析](./articles/R04-essays/R04-43-ai-office-big-four-competition-agent-a-share-quantification.md)** | R04 | 2026-08-25 | 🔴 高级 | 55min |
| **[央行5000亿元MLF操作：流动性管理精细化、货币政策适度宽松延续与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-42-pboc-500bn-mlf-operation-liquidity-management-a-share-quantification.md)** | R04 | 2026-08-24 | 🔴 高级 | 55min |
| **[2026世界机器人大会具身智能三大暗战：大脑模型、数据采集与灵巧手的产业链重构与A股机器人板块估值重塑深度量化分析](./articles/R04-essays/R04-41-wrc-2026-robot-brain-data-dexterous-hand-a-share-quantification.md)** | R04 | 2026-08-22 | 🔴 高级 | 55min |
| **[美债收益率飙至19年新高、杜德利警告美股泡沫2027年底前破裂：五大风险信号解码、全球资产定价锚动摇与A股防御性配置策略深度量化分析](./articles/R04-essays/R04-40-us-treasury-yield-surge-dudley-stock-bubble-warning-a-share-quantification.md)** | R04 | 2026-08-21 | 🔴 高级 | 55min |
| **[美国232关税突袭中国无人机产业：分级关税壁垒、供应链重构博弈与A股无人机板块结构性配置策略深度量化分析](./articles/R04-essays/R04-39-us-232-tariff-china-drone-supply-chain-reconstruction-a-share-quantification.md)** | R04 | 2026-08-20 | 🔴 高级 | 55min |
| **[发改委建立"六张网""2+3+N"协调机制：算力电网通信三网融合、7万亿基建投资加速与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-38-six-networks-coordination-mechanism-2-plus-3-plus-n-infra-investment-a-share-quantification.md)** | R04 | 2026-08-19 | 🔴 高级 | 55min |
| **[住房公积金管理条例二十年最大修订：10.9万亿沉睡资金激活、住房消费全链条打通与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-37-housing-provident-fund-regulation-revision-consumption-activation-a-share-quantification.md)** | R04 | 2026-08-18 | 🔴 高级 | 55min |
| **[2026年7月社会消费品零售总额同比增长0.6%：消费引擎降速换挡、大件品类集体承压与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-36-july-2026-retail-sales-0-6pct-consumption-weakness-structural-divergence-a-share-quantification.md)** | R04 | 2026-08-17 | 🔴 高级 | 55min |
| **['牛来'现象深度解码：一部被玩梗出圈的电影、一场'牛字辈'玄学炒作与A股情绪博弈量化分析](./articles/R04-essays/R04-35-niulai-phenomenon-meme-stock-speculation-market-sentiment-a-share-quantification.md)** | R04 | 2026-08-16 | 🔴 高级 | 55min |
| **[日本央行加息预期突袭全球：套息交易平仓冲击、大宗商品集体跳水与A股防御性配置策略深度量化分析](./articles/R04-essays/R04-34-japan-boj-hike-shock-carry-trade-unwind-commodities-plunge-a-share-quantification.md)** | R04 | 2026-08-13 | 🔴 高级 | 55min |
| **[美国7月CPI同比3.4%全面符合预期：通胀降温趋势确认、美联储9月加息预期降至42%与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-33-us-july-2026-cpi-3-4pct-fed-september-hike-cooling-a-share-quantification.md)** | R04 | 2026-08-13 | 🔴 高级 | 55min |
| **[上海印发《软件和信息服务业发展"十五五"规划》：4万亿产业蓝图、十万卡智算集群与A股软件算力板块估值重塑深度量化分析](./articles/I09-tmt/I09-09-shanghai-software-info-service-15th-five-year-plan-a-share-quantification.md)** | I09 | 2026-08-12 | 🔴 高级 | 55min |
| **[央行印发《"十五五"改革发展规划》：金融强国蓝图、双支柱框架确立与A股五大主线估值重塑深度量化分析](./articles/R04-essays/R04-32-pboc-15th-five-year-reform-development-plan-a-share-quantification.md)** | R04 | 2026-08-11 | 🔴 高级 | 55min |
| **[美国7月非农就业转负、美联储加息动力骤减：劳动力市场拐点确认、全球宽松预期重启与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-31-us-july-2026-nfp-negative-fed-hike-power-fades-a-share-quantification.md)** | R04 | 2026-08-10 | 🔴 高级 | 55min |
| **[2026年7月CPI同比上涨0.5% PPI回落至3.5%：温和通胀延续、K型分化加深与A股结构性配置策略深度量化分析](./articles/R04-essays/R04-30-july-2026-cpi-ppi-data-a-share-quantification.md)** | R04 | 2026-08-09 | 🔴 高级 | 55min |
| **[央行连续21个月增持黄金：7608万盎司储备创新高、去美元化战略加速与A股黄金板块估值重塑深度量化分析](./articles/I08-cyclical/I08-10-pboc-21-month-gold-buying-a-share-quantification.md)** | I08 | 2026-08-07 | 🔴 高级 | 55min |
| **[宇树科技科创板IPO定价150.80元：人形机器人第一股登场、610亿估值锚定与A股机器人产业链估值重塑深度量化分析](./articles/I10-manufacturing/I10-08-unitree-ipo-humanoid-robot-first-stock-a-share-quantification.md)** | I10 | 2026-08-06 | 🔴 高级 | 55min |
| **[商务部五箭齐发对美反制：无人机出口管制、CCC认证暂停与A股国产替代板块估值重塑深度量化分析](./articles/R04-essays/R04-29-mofcom-five-arrow-us-countermeasures-drone-ccc-certification-a-share-quantification.md)** | R04 | 2026-08-05 | 🔴 高级 | 55min |
| **[GB 44721-2026自动驾驶安全强制国标发布：L3/L4准入基线确立、产业链合规化提速与A股智能驾驶板块估值重塑深度量化分析](./articles/I10-manufacturing/I10-07-autonomous-driving-safety-standard-gb44721-a-share-quantification.md)** | I10 | 2026-08-04 | 🔴 高级 | 55min |
| **[新型电力系统建设"十五五"规划落地：5万亿电网投资、3亿千瓦储能与A股新能源产业链估值重塑深度量化分析](./articles/I06-new-energy/I06-11-new-power-system-15th-five-year-plan-5-trillion-investment-a-share-quantification.md)** | I06 | 2026-08-03 | 🔴 高级 | 55min |
| **[央行2026年下半年工作会议：适度宽松货币政策延续、逆周期调节加力与A股流动性框架重塑深度量化分析](./articles/R04-essays/R04-28-pboc-h2-2026-work-conference-moderately-loose-monetary-policy-a-share-quantification.md)** | R04 | 2026-08-02 | 🔴 高级 | 55min |
| **[ChinaJoy 2026"与AI同游"：B端AI渗透率86%但C端AI原生游戏爆款悬念与A股游戏板块估值重塑深度量化分析](./articles/I09-tmt/I09-08-chinajoy-2026-ai-native-gaming-a-share-quantification.md)** | I09 | 2026-08-02 | 🔴 高级 | 55min |
| **[四部委联合发布金融机构治理22条：破除'例外论'、穿透式监管与A股金融板块估值重塑深度量化分析](./articles/R04-essays/R04-27-four-ministries-financial-governance-22-measures-a-share-quantification.md)** | R04 | 2026-08-01 | 🔴 高级 | 55min |
| **[政治局7·30会议定调下半年：六张网7万亿基建+人工智能+智能经济与A股配置策略深度量化分析](./articles/R04-essays/R04-26-politburo-july-2026-six-networks-ai-plus-a-share-quantification.md)** | R04 | 2026-07-31 | 🔴 高级 | 55min |
| **[美联储7月鹰派按兵不动：三票反对创十年纪录、加息预期升温与A股配置策略深度量化分析](./articles/R04-essays/R04-25-fed-july-2026-hawkish-hold-three-dissent-a-share-quantification.md)** | R04 | 2026-07-30 | 🔴 高级 | 55min |
| **[SK海力士Q2财报不及预期但存储超级周期延续：HBM4扩产、LTA长期协议与A股存储产业链投资策略深度量化分析](./articles/I05-semiconductor/I05-17-sk-hynix-q2-2026-earnings-hbm4-storage-super-cycle-a-share-quantification.md)** | I05 | 2026-07-29 | 🔴 高级 | 55min |
| **[美国中期选举百日倒计时：高盛预警8月波动冲击与A股配置策略深度量化分析](./articles/R04-essays/R04-24-us-midterm-election-august-volatility-a-share-quantification.md)** | R04 | 2026-07-28 | 🔴 高级 | 55min |
| **[长鑫科技科创板上市深度量化分析：3.31万亿市值登顶A股与半导体产业链估值重塑](./articles/I05-semiconductor/I05-16-cxmt-star-market-listing-a-share-valuation-reshape-quantification.md)** | I05 | 2026-07-27 | 🔴 高级 | 55min |
| **[风光装机历史性超越火电：2026能源产业生态报告深度量化分析与A股新能源投资策略](./articles/I06-new-energy/I06-10-wind-solar-surpass-thermal-2026-energy-report-quantification.md)** | I06 | 2026-07-25 | 🔴 高级 | 55min |
| **[美联储加息预期骤升对A股的影响：油价破百、通胀重燃与央行对冲的三重博弈量化分析](./articles/R04-essays/R04-23-fed-rate-hike-july2026-a-share-impact-quantification.md)** | R04 | 2026-07-24 | 🔴 高级 | 55min |
| **[长征十号乙海上网系回收成功：中国商业航天可回收拐点深度量化分析](./articles/I10-manufacturing/I10-06-cz10b-maritime-net-recovery-a-share-quantification.md)** | I10 | 2026-07-11 | 🔴 高级 | 55min |
| **[台风灾害对A股板块影响的深度量化分析：历史规律与投资策略](./articles/R04-essays/R04-14-typhoon-annual-impact-a-share-quantification.md)** | R04 | 2026-07-12 | 🔴 高级 | 55min |
| **[氦气出口禁令深度量化分析：战略资源管控下的半导体气体国产替代拐点](./articles/I05-semiconductor/I05-14-helium-export-ban-semiconductor-gas-domestic-substitution-quantification.md)** | I05 | 2026-07-13 | 🔴 高级 | 55min |
| **[扩大消费十五五规划深度量化分析：60万亿消费市场重塑](./articles/I07-consumer/I07-07-consumption-15th-five-year-plan-a-share-quantification.md)** | I07 | 2026-07-14 | 🔴 高级 | 55min |
| **[美国CPI六年来首现环比负增长：通胀拐点与A股配置策略量化分析](./articles/R04-essays/R04-15-us-cpi-negative-june2026-a-share-impact-quantification.md)** | R04 | 2026-07-15 | 🔴 高级 | 55min |
| **[2026上半年金融数据深度量化分析：社融20.84万亿结构性变革](./articles/R04-essays/R04-16-h1-2026-financial-data-structural-change-a-share-quantification.md)** | R04 | 2026-07-16 | 🔴 高级 | 55min |
| **[韩国杠杆ETF监管风暴：KOSPI暴跌与A股传导深度量化分析](./articles/R04-essays/R04-17-korea-leveraged-etf-ban-a-share-impact-quantification.md)** | R04 | 2026-07-17 | 🔴 高级 | 55min |
| **[中美300亿美元对等降税深度量化分析：贸易缓和下的A股板块重构与投资策略](./articles/R04-essays/R04-22-china-us-30b-tariff-reduction-a-share-impact-quantification.md)** | R04 | 2026-07-23 | 🔴 高级 | 55min |
| **[NPO近封装光学超级节点：从腾讯Q4部署到华为OPEN NPO标准的光互联革命深度量化分析](./articles/I05-semiconductor/I05-15-npo-near-packaged-optics-super-node-a-share-quantification.md)** | I05 | 2026-07-22 | 🔴 高级 | 55min |
| **[Cell重磅：中国科学家破译玉米高产密码与种业产业化拐点深度量化分析](./articles/R04-essays/R04-21-maize-harvest-index-cell-breakthrough-seed-industry-a-share-quantification.md)** | R04 | 2026-07-22 | 🔴 高级 | 55min |
| **[央企领衔A股回购增持潮：500亿国新资金入场与市场底信号深度量化分析](./articles/R04-essays/R04-20-cso-led-buyback-surge-a-share-impact-quantification.md)** | R04 | 2026-07-20 | 🔴 高级 | 55min |
| **[工信部2026上半年工业和信息化发展情况深度量化分析：通信网升级、AI赋能与装备工业出口的A股投资机遇](./articles/R04-essays/R04-19-miit-h1-2026-industrial-telecom-development-a-share-quantification.md)** | R04 | 2026-07-20 | 🔴 高级 | 55min |
| **[中国经济2026上半年半年报：五关键词解码新质生产力驱动的结构性变革与A股投资策略量化分析](./articles/R04-essays/R04-18-h1-2026-economy-half-year-report-a-share-quantification.md)** | R04 | 2026-07-19 | 🔴 高级 | 55min |
| **[电池消费税新政深度量化分析：成熟技术征税与前沿技术免税双轨博弈](./articles/I06-new-energy/I06-09-battery-consumption-tax-policy-a-share-quantification.md)** | I06 | 2026-07-18 | 🔴 高级 | 55min |
| **["十五五"碳达峰行动方案：储能与虚拟电厂爆发的前夜量化分析](./articles/I06-new-energy/I06-08-carbon-peak-15th-five-year-energy-storage-vpp-quantification.md)** | I06 | 2026-07-09 | 🔴 高级 | 55min |
| **[DeepSeek V4正式发布与自研推理芯片对A股的深度量化分析](./articles/I05-semiconductor/I05-13-deepseek-v4-chip-a-share-impact-quantification.md)** | I05 | 2026-07-09 | 🔴 高级 | 55min |
| **[旅游强国建设十五五规划对A股文旅板块影响的深度量化分析](./articles/I07-consumer/I07-06-tourism-powerhouse-15th-five-year-quantification.md)** | I07 | 2026-07-07 | 🔴 高级 | 55min |
| **[智谱等大模型公司登录A股对股市影响的深度量化分析](./articles/I09-tmt/I09-07-ai-model-a-share-ipo-impact-quantification.md)** | I09 | 2026-07-07 | 🔴 高级 | 55min |
| **[华为韬定律V2：后摩尔时代半导体范式升级深度量化分析](./articles/I05-semiconductor/I05-12-huawei-tao-law-v2-semiconductor-paradigm-quantification.md)** | I05 | 2026-07-05 | 🔴 高级 | 55min |
| **[循环经济十五五规划出炉：8万亿赛道深度量化分析](./articles/I06-new-energy/I06-07-circular-economy-15th-five-year-quantification.md)** | I06 | 2026-07-04 | 🔴 高级 | 55min |
| **[A股交易新规：主板ST股涨跌幅调整至10%深度量化分析](./articles/R04-essays/R04-13-a-share-trading-rules-st-limit-10pct-quantification.md)** | R04 | 2026-07-04 | 🔴 高级 | 55min |
| **[美国6月非农就业爆冷对A股影响的量化分析](./articles/R04-essays/R04-12-us-nfp-june2026-a-share-impact-quantification.md)** | R04 | 2026-07-02 | 🔴 高级 | 55min |
| **[猪养殖周期规律的变化与现状量化分析](./articles/I08-cyclical/I08-09-pig-farming-cycle-change-quantification.md)** | I08 | 2026-07-02 | 🔴 高级 | 55min |
| **[八部门工业互联网+AI实施意见对A股影响的量化分析](./articles/I09-tmt/I09-06-industrial-internet-ai-implementation-quantification.md)** | I09 | 2026-07-01 | 🔴 高级 | 55min |
| **[工业气体产业链量化选股模型与投资策略](./articles/R04-essays/R04-11-industrial-gas-portfolio-selection-strategy-quantification.md)** | R04 | 2026-06-30 | 🔴 高级 | 55min |
| **[稀有气体氦氖氪氙战略资源稀缺性量化分析](./articles/I08-cyclical/I08-08-gas-rare-noble-helium-neon-krypton-xenon-quantification.md)** | I08 | 2026-06-30 | 🔴 高级 | 55min |
| **[电子特种气体半导体粮食稀缺性与国产替代量化分析](./articles/I05-semiconductor/I05-11-gas-electronic-special-semiconductor-quantification.md)** | I05 | 2026-06-30 | 🔴 高级 | 55min |
| **[氢气碳中和核心载能体量化分析](./articles/I06-new-energy/I06-06-gas-hydrogen-carbon-neutral-quantification.md)** | I06 | 2026-06-30 | 🔴 高级 | 55min |
| **[氩气与二氧化碳芯片清洗新星的量化分析](./articles/I05-semiconductor/I05-10-gas-argon-co2-chip-cleaning-quantification.md)** | I05 | 2026-06-30 | 🔴 高级 | 55min |
| **[氧气与氮气空分双雄的量化分析](./articles/I08-cyclical/I08-07-gas-oxygen-nitrogen-air-separation-quantification.md)** | I08 | 2026-06-30 | 🔴 高级 | 55min |
| **[工业气体全景与市场格局量化分析](./articles/I08-cyclical/I08-06-industrial-gas-overview-market-landscape-quantification.md)** | I08 | 2026-06-30 | 🔴 高级 | 55min |
| **[A股6月末千亿元解禁潮冲击的量化分析](./articles/R04-essays/R04-10-a-share-unlock-wave-june2026-impact-quantification.md)** | R04 | 2026-06-29 | 🔴 高级 | 55min |
| **[美伊冲突升级对油价与A股影响的量化分析](./articles/I08-cyclical/I08-05-iran-conflict-oil-price-a-share-impact-quantification.md)** | I08 | 2026-06-29 | 🔴 高级 | 55min |
| **[美联储降息预期与A股资金面重构的量化分析](./articles/R04-essays/R04-09-fed-rate-cut-a-share-capital-flow-quantification.md)** | R04 | 2026-06-29 | 🔴 高级 | 55min |
| **[625亿以旧换新补贴对消费产业链影响的量化分析](./articles/I07-consumer/I07-05-trade-in-subsidy-consumption-chain-quantification.md)** | I07 | 2026-06-29 | 🔴 高级 | 55min |
| **[MLCC超级周期：AI+新能源车双重驱动的量化分析](./articles/I09-tmt/I09-05-mlcc-super-cycle-ai-ev-quantification.md)** | I09 | 2026-06-29 | 🔴 高级 | 55min |
| **[央行+证监会政策共振对A股结构性影响的量化分析](./articles/R04-essays/R04-08-pboc-csrc-policy-resonance-june2026-quantification.md)** | R04 | 2026-06-29 | 🔴 高级 | 55min |
| **[长鑫存储295亿IPO对半导体产业链估值重塑的量化分析](./articles/I05-semiconductor/I05-09-cxmt-ipo-semiconductor-valuation-reshape-quantification.md)** | I05 | 2026-06-29 | 🔴 高级 | 55min |
| **[日韩股市剧烈波动对A股影响的深度量化分析](./articles/R04-essays/R04-07-japan-korea-stock-volatility-a-share-impact-quantification.md)** | R04 | 2026-06-28 | 🔴 高级 | 55min |
| **[1-5月规模以上工业企业利润增长18.8%对股市影响的量化分析](./articles/R04-essays/R04-06-industrial-profit-june2026-quantification.md)** | R04 | 2026-06-27 | 🔴 高级 | 55min |
| **[新型能源体系"十五五"规划对股市影响的量化分析](./articles/I06-new-energy/I06-05-new-energy-system-15th-five-year-quantification.md)** | I06 | 2026-06-26 | 🔴 高级 | 55min |
| **[美光科技FY26Q3财报对股市影响的量化分析](./articles/I05-semiconductor/I05-08-micron-fy26q3-earnings-impact-quantification.md)** | I05 | 2026-06-25 | 🔴 高级 | 55min |
| **[AI产业链大起大落对其他行业影响的量化研究](./articles/R04-essays/R04-05-ai-industry-impact-quantification.md)** | R04 | 2026-06-24 | 🔴 高级 | 55min |
| **[金刚石散热材料概念量化研究](./articles/I10-manufacturing/I10-05-diamond-heat-dissipation-quantification.md)** | I10 | 2026-06-23 | 🔴 高级 | 55min |
| **[a-stock-data：A股全栈数据工具包，零依赖直连13大数据源](./articles/O01-open-source-projects/O01-13-a-stock-data-fullstack-toolkit.md)** | O01 | 2026-06-22 | 🟡 中级 | 35min |
| **[美联储议息会议结论的量化分析方法](./articles/M03-volume-price-engine/M03-08-fomc-quantification.md)** | M03 | 2026-06-18 | 🔴 高级 | 55min |
| **[AI-Trader：港大HKUDS出品的Agent原生AI交易平台](./articles/O01-open-source-projects/O01-12-ai-trader-agent-native-platform.md)** | O01 | 2026-06-19 | 🟡 中级 | 35min |
| **[A股各板块轮动规律的量化分析方法](./articles/M03-volume-price-engine/M03-07-sector-rotation-quantification.md)** | M03 | 2026-06-17 | 🔴 高级 | 50min |
| **[WorldQuant 101因子（Alpha101）量化分析方法深度研究](./articles/M06-factor-validation/M06-07-worldquant-alpha101-analysis.md)** | M06 | 2026-06-16 | 🔴 高级 | 55min |
| **[国泰君安191因子（GTJA191）完整公式表](./articles/M06-factor-validation/M06-06-gtja191-formula-reference.md)** | M06 | 2026-06-15 | 🟢 初级 | 20min |
| **[A股大盘所处阶段判断的量化分析方法：多维度择时框架](./articles/M03-volume-price-engine/M03-06-market-stage-quantification.md)** | M03 | 2026-06-14 | 🔴 高级 | 45min |

> 📌 **查看更多**：完整 **261** 篇文章请浏览下方 [📚 完整分类目录](#-完整分类目录)

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
| [电子半导体](./articles/I05-semiconductor/) | I05 | **17** | 芯片、消费电子、半导体设备、物理AI、美光财报、长鑫存储、电子特气、芯片清洗气体、韬定律、DeepSeek、氦气出口禁令、NPO光互联、长鑫科创板上市、SK海力士存储超级周期 | [芯鉴九维模型](./articles/I05-semiconductor/I05-01-daocore-9dim-model.md) · [设备国产替代](./articles/I05-semiconductor/I05-02-semiconductor-equipment-localization.md) · [AI芯片](./articles/I05-semiconductor/I05-03-ai-chip-computing-demand.md) · [长鑫科技IPO](./articles/I05-semiconductor/I05-04-cxmt-ipo-impact-analysis.md) · [华为韬定律](./articles/I05-semiconductor/I05-05-huawei-tao-law-analysis.md) · [六氟化钨分析](./articles/I05-semiconductor/I05-06-wf6-tungsten-hexafluoride-analysis.md) · [物理AI产业链](./articles/I05-semiconductor/I05-07-physical-ai-industry-chain.md) · [美光财报量化](./articles/I05-semiconductor/I05-08-micron-fy26q3-earnings-impact-quantification.md) · [长鑫存储IPO估值重塑](./articles/I05-semiconductor/I05-09-cxmt-ipo-semiconductor-valuation-reshape-quantification.md) · [氩气CO₂芯片清洗](./articles/I05-semiconductor/I05-10-gas-argon-co2-chip-cleaning-quantification.md) · [电子特气量化](./articles/I05-semiconductor/I05-11-gas-electronic-special-semiconductor-quantification.md) · [韬定律V2量化](./articles/I05-semiconductor/I05-12-huawei-tao-law-v2-semiconductor-paradigm-quantification.md) · [DeepSeek V4芯片量化](./articles/I05-semiconductor/I05-13-deepseek-v4-chip-a-share-impact-quantification.md) · [氦气出口禁令量化](./articles/I05-semiconductor/I05-14-helium-export-ban-semiconductor-gas-domestic-substitution-quantification.md) · [NPO超级节点量化](./articles/I05-semiconductor/I05-15-npo-near-packaged-optics-super-node-a-share-quantification.md) · [长鑫科创板上市量化](./articles/I05-semiconductor/I05-16-cxmt-star-market-listing-a-share-valuation-reshape-quantification.md) · [SK海力士存储超级周期量化](./articles/I05-semiconductor/I05-17-sk-hynix-q2-2026-earnings-hbm4-storage-super-cycle-a-share-quantification.md) |
| [新能源](./articles/I06-new-energy/) | I06 | **11** | 光伏、锂电、储能、新能源车、新型能源体系、氢能、循环经济、碳达峰储能、电池消费税、风光超越火电、新型电力系统 | [行业分析](./articles/I06-new-energy/I06-01-new-energy-analysis.md) · [新能源车](./articles/I06-new-energy/I06-02-new-energy-vehicle-investment.md) · [储能产业](./articles/I06-new-energy/I06-03-energy-storage-industry-analysis.md) · [算电协同](./articles/I06-new-energy/I06-04-computing-power-electricity-synergy.md) · [新型能源体系十五五量化](./articles/I06-new-energy/I06-05-new-energy-system-15th-five-year-quantification.md) · [氢气碳中和量化](./articles/I06-new-energy/I06-06-gas-hydrogen-carbon-neutral-quantification.md) · [循环经济十五五量化](./articles/I06-new-energy/I06-07-circular-economy-15th-five-year-quantification.md) · [碳达峰储能虚拟电厂量化](./articles/I06-new-energy/I06-08-carbon-peak-15th-five-year-energy-storage-vpp-quantification.md) · [电池消费税新政量化](./articles/I06-new-energy/I06-09-battery-consumption-tax-policy-a-share-quantification.md) · [风光超越火电量化](./articles/I06-new-energy/I06-10-wind-solar-surpass-thermal-2026-energy-report-quantification.md) · [新型电力系统5万亿投资量化](./articles/I06-new-energy/I06-11-new-power-system-15th-five-year-plan-5-trillion-investment-a-share-quantification.md) |
| [消费](./articles/I07-consumer/) | I07 | **7** | 白酒、食品饮料、家电、体育消费、以旧换新、旅游强国、扩大消费 | [行业分析](./articles/I07-consumer/I07-01-consumer-analysis.md) · [白酒品牌](./articles/I07-consumer/I07-02-liquor-brand-moat-analysis.md) · [家电出海](./articles/I07-consumer/I07-03-home-appliance-globalization.md) · [体育赛事影响](./articles/I07-consumer/I07-04-mega-sports-event-market-impact.md) · [以旧换新量化](./articles/I07-consumer/I07-05-trade-in-subsidy-consumption-chain-quantification.md) · [旅游强国十五五量化](./articles/I07-consumer/I07-06-tourism-powerhouse-15th-five-year-quantification.md) · [扩大消费十五五规划量化](./articles/I07-consumer/I07-07-consumption-15th-five-year-plan-a-share-quantification.md) |
| [周期](./articles/I08-cyclical/) | I08 | **10** | 钢铁、煤炭、化工、有色、地缘冲突、工业气体、生猪养殖、黄金储备 | [行业分析](./articles/I08-cyclical/I08-01-cyclical-analysis.md) · [煤炭供需](./articles/I08-cyclical/I08-02-coal-supply-demand-price.md) · [有色金属](./articles/I08-cyclical/I08-03-nonferrous-metals-cycle.md) · [氧化钇概念量化](./articles/I08-cyclical/I08-04-yttria-concept-quantification.md) · [美伊冲突油价量化](./articles/I08-cyclical/I08-05-iran-conflict-oil-price-a-share-impact-quantification.md) · [工业气体全景](./articles/I08-cyclical/I08-06-industrial-gas-overview-market-landscape-quantification.md) · [氧气氮气量化](./articles/I08-cyclical/I08-07-gas-oxygen-nitrogen-air-separation-quantification.md) · [稀有气体量化](./articles/I08-cyclical/I08-08-gas-rare-noble-helium-neon-krypton-xenon-quantification.md) · [猪养殖周期量化](./articles/I08-cyclical/I08-09-pig-farming-cycle-change-quantification.md) · [央行21个月增持黄金量化](./articles/I08-cyclical/I08-10-pboc-21-month-gold-buying-a-share-quantification.md) |
| [TMT](./articles/I09-tmt/) | I09 | **8** | 互联网、软件、传媒、通信、MLCC、工业互联网、大模型、AI游戏、智算集群 | [行业分析](./articles/I09-tmt/I09-01-tmt-analysis.md) · [互联网平台](./articles/I09-tmt/I09-02-internet-platform-economy-analysis.md) · [通信运营商](./articles/I09-tmt/I09-03-telecom-operator-valuation.md) · [MLCC超级周期量化](./articles/I09-tmt/I09-05-mlcc-super-cycle-ai-ev-quantification.md) · [工业互联网AI量化](./articles/I09-tmt/I09-06-industrial-internet-ai-implementation-quantification.md) · [大模型A股上市影响量化](./articles/I09-tmt/I09-07-ai-model-a-share-ipo-impact-quantification.md) · [ChinaJoy AI游戏量化](./articles/I09-tmt/I09-08-chinajoy-2026-ai-native-gaming-a-share-quantification.md) · [上海软信业十五五规划量化](./articles/I09-tmt/I09-09-shanghai-software-info-service-15th-five-year-plan-a-share-quantification.md) |
| [制造](./articles/I10-manufacturing/) | I10 | **8** | 机械、汽车、军工、电力设备、金刚石散热、商业航天、自动驾驶、人形机器人 | [行业分析](./articles/I10-manufacturing/I10-01-manufacturing-analysis.md) · [高端制造](./articles/I10-manufacturing/I10-02-high-end-manufacturing-barriers.md) · [工业机器人](./articles/I10-manufacturing/I10-03-industrial-robot-automation.md) · [SpaceX IPO影响分析](./articles/I10-manufacturing/I10-04-spacex-ipo-impact-analysis.md) · [金刚石散热量化](./articles/I10-manufacturing/I10-05-diamond-heat-dissipation-quantification.md) · [长征十号乙海上网系回收量化](./articles/I10-manufacturing/I10-06-cz10b-maritime-net-recovery-a-share-quantification.md) · [自动驾驶安全国标量化](./articles/I10-manufacturing/I10-07-autonomous-driving-safety-standard-gb44721-a-share-quantification.md) · [宇树科技IPO人形机器人量化](./articles/I10-manufacturing/I10-08-unitree-ipo-humanoid-robot-first-stock-a-share-quantification.md) |

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
| [研究随笔](./articles/R04-essays/) | R04 | **88** | 投资感悟、市场观察、宏观量化 | [研究随笔与感悟](./articles/R04-essays/R04-01-research-essays-and-insights.md) · [认知偏差](./articles/R04-essays/R04-02-cognitive-bias-quant-investing.md) · [量化心路历程](./articles/R04-essays/R04-03-quant-investing-journey.md) · [Serenity瓶颈投资方法论](./articles/R04-essays/R04-04-serenity-chokepoint-theory.md) · [AI产业链影响量化](./articles/R04-essays/R04-05-ai-industry-impact-quantification.md) · [工业利润量化分析](./articles/R04-essays/R04-06-industrial-profit-june2026-quantification.md) · [日韩股市波动量化](./articles/R04-essays/R04-07-japan-korea-stock-volatility-a-share-impact-quantification.md) · [央行证监会政策共振](./articles/R04-essays/R04-08-pboc-csrc-policy-resonance-june2026-quantification.md) · [美联储降息资金面](./articles/R04-essays/R04-09-fed-rate-cut-a-share-capital-flow-quantification.md) · [解禁潮冲击量化](./articles/R04-essays/R04-10-a-share-unlock-wave-june2026-impact-quantification.md) · [工业气体选股策略](./articles/R04-essays/R04-11-industrial-gas-portfolio-selection-strategy-quantification.md) · [美国非农爆冷量化](./articles/R04-essays/R04-12-us-nfp-june2026-a-share-impact-quantification.md) · [交易新规ST涨跌幅量化](./articles/R04-essays/R04-13-a-share-trading-rules-st-limit-10pct-quantification.md) · [台风灾害A股影响量化](./articles/R04-essays/R04-14-typhoon-annual-impact-a-share-quantification.md) · [美国CPI负增长A股量化](./articles/R04-essays/R04-15-us-cpi-negative-june2026-a-share-impact-quantification.md) · [上半年金融数据量化](./articles/R04-essays/R04-16-h1-2026-financial-data-structural-change-a-share-quantification.md) · [韩国杠杆ETF监管量化](./articles/R04-essays/R04-17-korea-leveraged-etf-ban-a-share-impact-quantification.md) · [经济半年报A股量化](./articles/R04-essays/R04-18-h1-2026-economy-half-year-report-a-share-quantification.md) · [工信部工信发展量化](./articles/R04-essays/R04-19-miit-h1-2026-industrial-telecom-development-a-share-quantification.md) · [央企回购增持量化](./articles/R04-essays/R04-20-cso-led-buyback-surge-a-share-impact-quantification.md) · [玉米高产密码种业量化](./articles/R04-essays/R04-21-maize-harvest-index-cell-breakthrough-seed-industry-a-share-quantification.md) · [中美降税A股量化](./articles/R04-essays/R04-22-china-us-30b-tariff-reduction-a-share-impact-quantification.md) · [美联储加息A股量化](./articles/R04-essays/R04-23-fed-rate-hike-july2026-a-share-impact-quantification.md) · [美国中期选举波动量化](./articles/R04-essays/R04-24-us-midterm-election-august-volatility-a-share-quantification.md) · [美联储7月鹰派按兵不动量化](./articles/R04-essays/R04-25-fed-july-2026-hawkish-hold-three-dissent-a-share-quantification.md) · [政治局六张网AI+量化](./articles/R04-essays/R04-26-politburo-july-2026-six-networks-ai-plus-a-share-quantification.md) · [金融机构治理22条量化](./articles/R04-essays/R04-27-four-ministries-financial-governance-22-measures-a-share-quantification.md) · [央行下半年工作会议量化](./articles/R04-essays/R04-28-pboc-h2-2026-work-conference-moderately-loose-monetary-policy-a-share-quantification.md) · [商务部五箭齐发对美反制量化](./articles/R04-essays/R04-29-mofcom-five-arrow-us-countermeasures-drone-ccc-certification-a-share-quantification.md) · [7月CPI PPI数据量化](./articles/R04-essays/R04-30-july-2026-cpi-ppi-data-a-share-quantification.md) · [7月非农转负加息动力骤减量化](./articles/R04-essays/R04-31-us-july-2026-nfp-negative-fed-hike-power-fades-a-share-quantification.md) · [央行十五五改革发展规划量化](./articles/R04-essays/R04-32-pboc-15th-five-year-reform-development-plan-a-share-quantification.md) · [美国7月CPI加息预期降温量化](./articles/R04-essays/R04-33-us-july-2026-cpi-3-4pct-fed-september-hike-cooling-a-share-quantification.md) · [日本加息套息交易平仓量化](./articles/R04-essays/R04-34-japan-boj-hike-shock-carry-trade-unwind-commodities-plunge-a-share-quantification.md) · [牛来现象玄学炒作量化](./articles/R04-essays/R04-35-niulai-phenomenon-meme-stock-speculation-market-sentiment-a-share-quantification.md) · [7月社零消费降速结构分化量化](./articles/R04-essays/R04-36-july-2026-retail-sales-0-6pct-consumption-weakness-structural-divergence-a-share-quantification.md) · [公积金条例修订消费激活量化](./articles/R04-essays/R04-37-housing-provident-fund-regulation-revision-consumption-activation-a-share-quantification.md) · [六张网2+3+N协调机制量化](./articles/R04-essays/R04-38-six-networks-coordination-mechanism-2-plus-3-plus-n-infra-investment-a-share-quantification.md) · [美232关税无人机供应链重构量化](./articles/R04-essays/R04-39-us-232-tariff-china-drone-supply-chain-reconstruction-a-share-quantification.md) · [美债收益率飙升杜德利泡沫警告量化](./articles/R04-essays/R04-40-us-treasury-yield-surge-dudley-stock-bubble-warning-a-share-quantification.md) · [WRC机器人三大暗战量化](./articles/R04-essays/R04-41-wrc-2026-robot-brain-data-dexterous-hand-a-share-quantification.md) · [央行5000亿MLF操作量化](./articles/R04-essays/R04-42-pboc-500bn-mlf-operation-liquidity-management-a-share-quantification.md) · [AI办公四巨头竞争量化](./articles/R04-essays/R04-43-ai-office-big-four-competition-agent-a-share-quantification.md) · [英伟达Q2财报AI算力量化](./articles/R04-essays/R04-44-nvidia-q2-earnings-ai-computing-power-a-share-quantification.md) · [上海十五五战略性新兴产业量化](./articles/R04-essays/R04-45-shanghai-15th-five-year-strategic-emerging-industries-a-share-quantification.md) · [发改委8月发布会六大量化](./articles/R04-essays/R04-46-ndrc-august-2026-press-conference-six-networks-ai-ic-a-share-quantification.md) · [央行房地产信贷改革量化](./articles/R04-essays/R04-47-pboc-real-estate-credit-reform-40-year-mortgage-a-share-quantification.md) · [沃什杰克逊霍尔鹰派信号量化](./articles/R04-essays/R04-48-warsh-jackson-hole-hawkish-signal-fed-hike-fiscal-debt-a-share-quantification.md) · [中医药十五五规划振兴量化](./articles/R04-essays/R04-49-tcm-15th-five-year-plan-revival-development-a-share-quantification.md) · [工信部AI服务商培育量化](./articles/R04-essays/R04-50-miit-ai-service-provider-cultivation-action-a-share-quantification.md) · [商务部商品消费扩容升级量化](./articles/R04-essays/R04-51-mofcom-7-ministries-commodity-consumption-expansion-upgrading-a-share-quantification.md) · [苹果CEO交接果链量化](./articles/R04-essays/R04-52-apple-cook-ternus-ceo-succession-a-share-supply-chain-quantification.md) · [霍尔木兹黑天鹅债市量化](./articles/R04-essays/R04-53-hormuz-strait-black-swan-global-bond-selloff-a-share-impact-quantification.md) · [中小企业十五五专精特新量化](./articles/R04-essays/R04-54-sme-15th-five-year-plan-specialized-new-a-share-quantification.md) · [动力电池大会锂电产业量化](./articles/R04-essays/R04-55-miit-power-battery-high-end-ai-green-global-a-share-lithium-quantification.md) · [非农爆表加息预期量化](./articles/R04-essays/R04-56-us-august-nfp-surprise-hike-odds-trump-rate-cut-pressure-a-share-quantification.md) · [GPT-6 Astra算力产业链量化](./articles/R04-essays/R04-57-openai-gpt6-astra-agi-breakthrough-a-share-ai-computing-quantification.md) · [3000亿金融央企注资量化](./articles/R04-essays/R04-58-mof-300b-financial-soe-capital-injection-bank-insurance-a-share-quantification.md) · [6G商用与信息通信十五五量化](./articles/R04-essays/R04-59-ict-15th-five-year-plan-6g-commercialization-a-share-quantification.md) · [8月CPI/PPI再通胀量化](./articles/R04-essays/R04-60-aug-2026-cpi-ppi-reinflation-a-share-quantification.md) · [苹果秋季发布会果链量化](./articles/R04-essays/R04-61-apple-fall-2026-keynote-iphone-duo-end-side-ai-a-share-supply-chain-quantification.md) · [美国8月CPI与AI资本开支量化](./articles/R04-essays/R04-62-us-august-cpi-core-inflation-uptick-hike-odds-ai-capex-monetary-transmission-a-share-quantification.md) · [折叠屏集体上桌量化](./articles/R04-essays/R04-63-foldable-phones-huawei-mate-xt2-apple-iphone-duo-broadfold-a-share-supply-chain-quantification.md) · [8月金融数据社融信贷量化](./articles/R04-essays/R04-64-aug-2026-financial-stats-social-financing-credit-structure-m1-rebound-a-share-quantification.md) · [电子信息制造十五五30万亿量化](./articles/R04-essays/R04-65-electronic-information-manufacturing-15th-five-year-plan-30t-ic-way-ai-hardware-a-share-quantification.md) · [加密法案否决美债油价三重冲击量化](./articles/R04-essays/R04-66-crypto-clarity-act-defeat-btc-plunge-treasury-yield-5pct-hike-odds-a-share-ripple-quantification.md) · [美联储9月首次加息点阵图41%量化](./articles/R04-essays/R04-67-fed-september-2026-first-hike-warsh-era-25bp-375-400-dot-plot-41pct-a-share-three-chain-quantification.md) · [文化和旅游十五五83亿出游量化](./articles/R04-essays/R04-68-culture-tourism-15th-five-year-plan-83b-domestic-trips-190m-inbound-tourism-a-share-quantification.md) · [华为昇腾960 NPO光互连超节点量化](./articles/R04-essays/R04-69-huawei-ascend960-supernode-npo-optical-interconnect-liquid-cooling-a-share-quantification.md) · [医药工业十五五创新药出海量化](./articles/R04-essays/R04-70-pharma-industry-15th-five-year-plan-35t-revenue-20pct-innovative-drug-growth-fic-25pct-license-out-a-share-quantification.md) · [房地产存量时代城市更新量化](./articles/R04-essays/R04-71-housing-stock-era-secondhand-trading-52pct-city-renewal-15th-five-year-good-house-a-share-quantification.md) · [房车消费十部门措施量化](./articles/R04-essays/R04-72-rv-consumption-measures-guo-ban-han-82-10-departments-new-energy-rv-camping-a-share-quantification.md) · [双模通信HPLC+RF电网突破量化](./articles/R04-essays/R04-73-dual-mode-communication-hplc-rf-breakthrough-200m-units-27-provincial-grid-a-share-quantification.md) · [MLCC AI服务器高容紧缺量化](./articles/R04-essays/R04-74-mlcc-ai-server-high-capacity-shortage-10x-spot-surge-structural-divergence-a-share-quantification.md) · [A/H地产股存量时代叙事重估量化](./articles/R04-essays/R04-75-ah-property-stock-surge-stock-era-narrative-revaluation-not-old-cycle-a-share-quantification.md) · [国内AI软件产业模型商品化量化](./articles/R04-essays/R04-76-china-ai-software-industry-model-commoditization-business-result-delivery-data-moat-a-share-quantification.md) · [央行隔夜逆回购1万亿上限扩额度量化](./articles/R04-essays/R04-77-pboc-overnight-reverse-repo-1t-ceiling-expansion-to-quota-shift-last-resort-overnight-lender-a-share-quantification.md) · [VNA矢量网络分析仪高速互联量化](./articles/R04-essays/R04-78-vector-network-analyzer-vna-rf-test-instrument-ai-high-speed-interconnect-a-share-quantification.md) · [DSec沙盒Agent训练基础设施量化](./articles/R04-essays/R04-79-deepseek-dsec-sandbox-infrastructure-agent-training-cpu-execution-environment-a-share-quantification.md) · [北京现房细则主办银行制规模负债化量化](./articles/R04-essays/R04-80-beijing-completed-home-sales-rules-main-bank-system-scale-liability-shift-a-share-quantification.md) · [Meta Muse AI Agent应用影响排名量化](./articles/R04-essays/R04-81-meta-muse-ai-agent-app-impact-ranking-user-intent-entrance-a-share-quantification.md) · [运营商Token经营计费权SLA量化](./articles/R04-essays/R04-82-telecom-token-operations-billing-rights-sla-pseudo-price-war-a-share-quantification.md) · [工业利润15.7%行业制服辛普森悖论量化](./articles/R04-essays/R04-83-industrial-profit-15-percent-sector-uniform-mimpson-paradox-power-transfer-a-share-quantification.md) · [新型电池十五五PPB禁入公告能源基础设施量化](./articles/R04-essays/R04-84-new-battery-industry-15th-fyp-ppb-defect-rate-energy-infrastructure-a-share-quantification.md) · [科技部十五五科技攻关创新链产业链匹配量化](./articles/R04-essays/R04-85-sci-tech-ministry-15th-fyp-key-technology-conquest-innovation-chain-industry-chain-a-share-quantification.md) · [央行四工具PSL准财政算力网公用事业化量化](./articles/R04-essays/R04-86-pboc-structural-tools-psl-quasi-fiscal-computing-power-network-utility-a-share-quantification.md) · [9月PMI两个经济体维持性生产通胀税收量化](./articles/R04-essays/R04-87-september-2026-pmi-two-economies-maintenance-production-inflation-tax-construction-new-orders-a-share-quantification.md) · [买断式逆回购1.2万亿财政货币协同所有权过户隐性QE量化](./articles/R04-essays/R04-88-pboc-outright-reverse-repo-1-2-trillion-treasury-fiscal-credit-coordination-ownership-transfer-quantitative-easing-a-share-quantification.md) |
| [文献综述](./articles/R05-literature/) | R05 | **14** | 经典论文解读、学术前沿、涨停归因 | [文献综述](./articles/R05-literature/R05-01-literature-review-and-frontier.md) · [五因子模型](./articles/R05-literature/R05-02-fama-french-five-factor.md) · [机器学习量化](./articles/R05-literature/R05-03-ml-quant-applications.md) · [涨停归因模型](./articles/R05-literature/R05-04-limit-up-attribution-model.md) · [CAPM理论详解](./articles/R05-literature/R05-05-CAPM-capital-asset-pricing-model-theory.md) · [CAPM Python实战](./articles/R05-literature/R05-06-CAPM-python-implementation.md) · [Fama-French详解](./articles/R05-literature/R05-07-fama-french-multi-factor-model-theory.md) · [Fama-French实战](./articles/R05-literature/R05-08-fama-french-python-implementation.md) · [APT理论详解](./articles/R05-literature/R05-09-apt-arbitrage-pricing-theory.md) · [APT Python实战](./articles/R05-literature/R05-10-apt-python-implementation.md) · [Markowitz理论详解](./articles/R05-literature/R05-11-markowitz-mean-variance-model-theory.md) · [Markowitz Python实战](./articles/R05-literature/R05-12-markowitz-python-implementation.md) · [Black-Litterman理论详解](./articles/R05-literature/R05-13-black-litterman-model-theory.md) · [Black-Litterman Python实战](./articles/R05-literature/R05-14-black-litterman-python-implementation.md) |

#### O - 开源项目（Open Source Projects）

量化交易领域优秀开源项目的深度解析与评估

| 子分类 | 代码 | 文章数 | 说明 | 文章列表 |
|--------|------|:------:|------|---------|
| [开源项目](./articles/O01-open-source-projects/) | O01 | **13** | 量化交易开源项目深度解析 | [TradingAgents](./articles/O01-open-source-projects/O01-01-tradingagents-multi-agent-framework.md) · [Microsoft Qlib](./articles/O01-open-source-projects/O01-02-microsoft-qlib-ai-quant-platform.md) · [Backtrader](./articles/O01-open-source-projects/O01-03-backtrader-python-backtesting.md) · [Freqtrade](./articles/O01-open-source-projects/O01-04-freqtrade-crypto-trading-bot.md) · [StockSharp](./articles/O01-open-source-projects/O01-05-stocksharp-csharp-trading-platform.md) · [Riskfolio-Lib](./articles/O01-open-source-projects/O01-06-riskfolio-portfolio-optimization.md) · [FinRL](./articles/O01-open-source-projects/O01-07-finrl-deep-reinforcement-learning.md) · [TradeMaster](./articles/O01-open-source-projects/O01-08-trademaster-rl-trading.md) · [VN.PY](./articles/O01-open-source-projects/O01-09-vnpy-python-trading-platform.md) · [Zipline](./articles/O01-open-source-projects/O01-10-zipline-quantopian-backtesting.md) · [AI Hedge Fund](./articles/O01-open-source-projects/O01-11-ai-hedge-fund-legendary-investors.md) · [AI-Trader](./articles/O01-open-source-projects/O01-12-ai-trader-agent-native-platform.md) · [a-stock-data](./articles/O01-open-source-projects/O01-13-a-stock-data-fullstack-toolkit.md) |
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
