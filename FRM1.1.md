# Reading 1 风险管理的基础模块（The Building Blocks of Risk Management）

# 1. 风险与风险管理的基本概念

## 1.1 风险（Risk）

在投资语境中，**风险（Risk）** 指结果的不确定性（uncertainty surrounding outcomes）。投资者通常更关心负面结果，也就是**非预期投资损失（unexpected investment losses）**，而不是正面的意外收益。 

FRM考试重点：

- 风险不一定等于“潜在损失金额很大”。
- 真正重要的是损失的**变动性（variability）**，尤其是损失可能突然升高到超出预期的水平。
- 风险与收益之间通常存在权衡关系：风险越高，潜在收益越高；风险越低，潜在收益也较低。 

## 1.2 风险管理（Risk Management）与承担风险（Risk Taking）

**风险管理（Risk Management）** 是一系列旨在降低或消除实体发生预期损失可能性的活动，同时也要管理某些成本的非预期波动。它不仅是防御性的，也涉及企业有意识地决定愿意承担多少风险以获取未来不确定收益。 

**承担风险（Risk Taking）** 是为了追求额外收益而主动接受额外风险。它更偏机会导向（opportunistic action）。 

简单区分：

- 风险管理：关注如何识别、衡量、控制和监控风险。
- 承担风险：关注为了更高收益主动接受风险。

# 2. 风险管理流程（Risk Management Process）

**风险管理流程（Risk Management Process）** 是一套正式步骤，用来判断预期收益是否足以补偿预期风险，并考虑是否可以在保持类似收益的同时降低风险。 

核心步骤包括：

1. 识别风险（Identify risks）
2. 衡量并管理风险（Measure and manage risks）
3. 区分预期风险与非预期风险（Distinguish between expected and unexpected risks）
4. 处理风险之间的关系（Address the relationships among risks）
5. 制定风险缓释策略（Develop a risk mitigation strategy）
6. 监控风险缓释策略并根据需要调整（Monitor and adjust） 

## 2.1 风险识别方法

风险管理者可以使用多种方法识别风险：

- **头脑风暴（Brainstorming）**：向业务负责人和前线员工收集潜在风险。
- **行业资源（Industry resources）**：例如监管标准、行业调查、专家意见。
- **历史损失数据分析（Actual loss data analysis）**：分析损失规模和频率。
- **情景分析（Scenario analysis）**：考虑未来可能发生的风险情景及其结果。 

## 2.2 风险的已知与未知分类

风险可以从“预期”到“完全未知”形成一个光谱：

- **预期损失（Expected loss）**
- **非预期损失（Unexpected loss）**
- **已知的未知（Known unknowns）**，也称 **Knightian uncertainty**
- **未知的未知（Unknown unknowns）**，通常对应尾部风险事件（tail risk events） 

【《损失类别图：展示预期损失、非预期损失、已知未知和未知未知之间的层级关系》。《第3页》】

## 2.3 四种风险处理方式

企业面对风险时通常有四种选择：

1. **避免风险（Avoid risk）**  
    例如出售某条产品线、退出某些市场或司法辖区。
    
2. **保留风险（Retain risk）**  
    如果预期收益足以补偿损失概率和频率，企业可以选择保留风险。
    
3. **缓释风险（Mitigate risk）**  
    降低风险敞口的损失规模或发生频率。
    
4. **转移风险（Transfer risk）**  
    通过衍生品（derivatives）、结构化产品（structured products）或保险（insurance）将风险转移给第三方。 
    

## 2.4 风险管理的局限

FRM考试常考风险管理并不能完全消除风险。风险管理往往只是把风险从一方转移到另一方，因此在整体经济层面可能类似**零和博弈（zero-sum game）**。如果风险过度集中于少数参与者，可能导致系统性危机，例如2007–2009年金融危机。 

# 3. 风险衡量与管理工具

## 3.1 定量风险指标（Quantitative Risk Measures）

### 3.1.1 风险价值（Value at Risk, VaR）

**VaR（Value at Risk）** 是在给定置信水平下估计的损失金额。例子：某金融机构一天期95%置信水平VaR为250万美元，表示任意一天有5%的概率损失超过250万美元。 

VaR适用于：

- 流动性较好的头寸（liquid positions）
- 正常市场环境（normal market circumstances）
- 短期风险衡量（short period） 

VaR不适用于或较危险的情况：

- 非正常市场环境
- 非流动性头寸（illiquid positions）
- 长期风险衡量 

例子：如果某证券的月收益率在95%置信水平下对应的下尾收益为 -15.5%，投资金额为1,000,000美元，则一个月VaR为：

$VaR = 15.5\% \times 1{,}000{,}000 = 155{,}000$

即一个月VaR为155,000美元。 

【《月收益率直方图：用于说明历史VaR如何由收益率分布左尾计算得出》。《第4页》】

> FRM提醒：该例子属于**历史VaR（Historical VaR）**。 

### 3.1.2 经济资本（Economic Capital）

**经济资本（Economic Capital）** 是为了覆盖非预期损失所需的流动资本。如果一天期VaR为250万美元，企业持有250万美元流动储备，则表示其有足够经济资本覆盖该一天期尾部风险事件。 

## 3.2 定性风险评估（Qualitative Risk Assessment）

### 3.2.1 情景分析（Scenario Analysis）

**情景分析（Scenario Analysis）** 考虑未来潜在风险因素及其不同结果，通常比较最好情景（best-case scenario）和最坏情景（worst-case scenario）。它是一种“假如……会怎样”的分析工具，用于理解即使概率很小但损失可能很大的情况。 

### 3.2.2 压力测试（Stress Testing）

**压力测试（Stress Testing）** 是情景分析的一种形式，通常一次调整一个参数，观察其对企业的影响。例如，测试利率大幅上升对债券组合价值的影响。 

参数来源可以分为：

- **历史参数（Historically sourced parameters）**：可观察，但过去趋势未必持续。
- **估计参数（Estimated variables）**：基于假设预测，可能带来估计误差和模型风险（model risk）。 

## 3.3 企业风险管理（Enterprise Risk Management, ERM）

**企业风险管理（ERM）** 是在整个企业层面管理风险的综合流程。它不是部门孤岛式管理，而是自上而下地考虑风险如何影响多个部门。 

ERM的目标：

- 理解公司整体风险（company-wide risks）
- 将风险规划纳入战略业务规划（strategic business planning）
- 将风险信息与管理行动连接起来 

FRM考试重点：

- ERM不只是风险汇总。
- 不能把企业风险简单压缩成一个数字，例如VaR或经济资本。
- 风险是多维度的，需要统计分析和风险经理的判断共同发挥作用。 

# 4. 预期损失与非预期损失

## 4.1 预期损失（Expected Loss, EL）

**预期损失（Expected Loss, EL）** 是企业在正常经营过程中预计会发生的平均损失。通常可以通过统计分析在较短时间范围内较可靠地估计。 

EL通常取决于：

1. 风险发生概率
2. 风险事件的金额敞口
3. 风险发生时的预期损失严重程度 

在银行信用风险中：

$EL = EAD \times PD \times LGD$

其中：

- **EAD（Exposure at Default）**：违约时风险敞口
- **PD（Probability of Default）**：违约概率
- **LGD（Loss Given Default）**：违约损失率 

FRM提醒：如果EL能够较有把握地建模，它可以像可预测费用或变动成本一样处理。 

## 4.2 非预期损失（Unexpected Loss, UL）

**非预期损失（Unexpected Loss, UL）** 是超过平均预期损失的损失。由于其本质上是“非预期”的，因此预测难度较高。 

重要例子：

- 经济衰退时，汽车制造企业贷款组合可能出现大量违约。
- 违约可能同时集中发生，形成**相关性风险（Correlation Risk）**。
- 房地产贷款中，借款人违约率上升的同时，抵押品房地产价值下降，使回收率下降，也会导致损失超过预期。 

# 5. 风险与收益的关系

风险与收益存在自然权衡关系。一般而言，承担更大风险通常意味着更高潜在收益，但也要考虑收益的不确定性。 

可以理解为：

- 可用概率函数衡量的收益波动部分：风险（risk）
- 难以衡量的部分：不确定性（uncertainty）或非预期损失（unexpected loss） 

例子：

- 政府债券（government bonds）的信用/违约风险低于公司债券（corporate bonds），因此其他条件相同下收益率更低。
- 但风险收益关系不仅由信用风险决定，还可能受到流动性风险（liquidity risk）和税收影响（taxation impacts）影响。 

FRM考试重点：

- 对于交易不活跃或非公开交易资产，风险/收益权衡更难分析。
- 风险经理需要深入分析细粒度损失驱动因素（granular loss drivers）。
- 人工智能（artificial intelligence）和机器学习（machine learning）可以帮助识别重要损失驱动因素。 

# 6. 利益冲突与风险管理

风险管理中的一大结构性问题是**利益冲突（Conflicts of Interest）**。最了解风险存在、概率和影响的人，有时可能试图利用这些风险获利。 

例子：

- 流氓交易员（rogue traders）
- 管理层隐瞒风险，以推动短期股价上涨，从而提高个人股票薪酬收益 

缓解利益冲突的三步：

1. 前线员工和部门经理识别风险
2. 建立具有日常监督的稳健风险管理系统
3. 定期进行独立审计，确保前两步有效运行 

# 7. 主要风险类型（Types of Risk）

企业面临的风险可以分为：

- 市场风险（Market Risk）
- 信用风险（Credit Risk）
- 流动性风险（Liquidity Risk）
- 操作风险（Operational Risk）
- 法律与监管风险（Legal and Regulatory Risk）
- 商业与战略风险（Business and Strategic Risk）
- 声誉风险（Reputation Risk） 

## 7.1 市场风险（Market Risk）

**市场风险（Market Risk）** 是市场价格和利率不断变化带来的风险。主要包括四类：

1. 利率风险（Interest Rate Risk）
2. 股票价格风险（Equity Price Risk）
3. 外汇风险（Foreign Exchange Risk）
4. 商品价格风险（Commodity Price Risk） 

### 7.1.1 利率风险（Interest Rate Risk）

利率风险来自利率水平变化的不确定性。市场利率上升时，债券价值下降。收益率曲线形状变化或平行移动也会产生利率风险。 

若对冲工具与被对冲债券之间的价格关系发生不利变化，就会产生**基差风险（Basis Risk）**。 

### 7.1.2 股票价格风险（Equity Price Risk）

股票价格风险指股票价格波动风险，可分为：

- **一般市场风险（General Market Risk）**：股票价格对整体市场指数变化的敏感性，无法通过分散化消除。
- **特定风险（Specific Risk）**：公司特定因素导致的股价变化，可通过持有相关性不完全的资产来缓解。 

### 7.1.3 外汇风险（Foreign Exchange Risk）

外汇风险来自完全或部分未对冲的外币头寸，可能由货币价格变化相关性不完美以及国际利率变化导致。 

### 7.1.4 商品价格风险（Commodity Price Risk）

商品价格风险来自商品价格波动，例如贵金属、基本金属、农产品和能源。由于部分商品集中在少数市场参与者手中，交易流动性较低，价格波动可能高于金融证券。商品价格还可能出现价格跳跃（price discontinuities）。 

## 7.2 信用风险（Credit Risk）

**信用风险（Credit Risk）** 是交易对手未能履行合同义务导致损失的风险。 

四个子类型：

1. 违约风险（Default Risk）
2. 破产风险（Bankruptcy Risk）
3. 降级风险（Downgrade Risk）
4. 结算风险（Settlement Risk） 

### 7.2.1 违约风险（Default Risk）

借款人可能无法支付利息和/或本金。违约概率（PD）是信用风险管理的核心。 

### 7.2.2 破产风险（Bankruptcy Risk）

交易对手可能完全停止经营。风险管理关注点是抵押品清算价值是否足以弥补违约损失。 

### 7.2.3 降级风险（Downgrade Risk）

交易对手信用质量下降，债权人可能要求更高利率以补偿风险。对债权人而言，降级风险最终可能发展为违约风险。 

### 7.2.4 结算风险（Settlement Risk）/交易对手风险（Counterparty Risk）

在衍生品交易中，到结算日时，亏损方可能拒绝支付或无法支付。这也称为**交易对手风险（Counterparty Risk）** 或 **Herstatt Risk**。 

例子：某期货合约结算时，一方应获得500,000美元，但交易对手只能支付400,000美元。则：

- 回收价值（Recovery Value）= 400,000美元
- 违约损失（LGD）= 100,000美元
- 回收率（Recovery Rate）= 80%
- LGD比例 = 20% 

## 7.3 流动性风险（Liquidity Risk）

**流动性风险（Liquidity Risk）** 分为：

1. 融资流动性风险（Funding Liquidity Risk）
2. 市场流动性风险（Market Liquidity Risk）或交易流动性风险（Trading Liquidity Risk） 

### 7.3.1 融资流动性风险（Funding Liquidity Risk）

企业无法偿还或再融资债务、满足交易对手现金义务或满足资本提款需求。银行业中，短期存款与长期贷款之间的期限错配就是典型例子。 

### 7.3.2 市场流动性风险（Market Liquidity Risk）

企业暂时无法找到交易对手，导致无法以合理价格将资产变现。若急需交易，可能必须大幅折价出售，造成巨大损失。 

## 7.4 操作风险（Operational Risk）

**操作风险（Operational Risk）** 是由于内部流程不足或失败、人为错误或外部事件导致的潜在损失。 

来源包括：

- 技术风险（Technology Risk）
- 内部控制不足（Insufficient internal controls）
- 管理层能力不足
- 欺诈（Fraud）
- 员工错误
- 自然灾害
- 网络安全风险（Cybersecurity Risk）
- 流氓交易员（Rogue traders） 

FRM重点：衍生品交易由于杠杆高、定价模型复杂、资产可能缺乏流动性，因此特别容易受到操作风险影响。 

## 7.5 法律与监管风险（Legal and Regulatory Risk）

**法律风险（Legal Risk）** 是诉讼给企业带来的不确定性。例如，金融交易一方起诉另一方以终止交易。 

**监管风险（Regulatory Risk）** 是政府行为带来的不确定性。例如税法变化或保证金要求变化，可能改变某笔交易的收益结构。 

## 7.6 商业风险与战略风险（Business and Strategic Risk）

### 商业风险（Business Risk）

商业风险来自影响收入或成本结构的变量变化，例如：

- 客户需求趋势
- 产品定价政策
- 生产投入成本
- 供应商谈判
- 新产品创新
- 运输延误
- 生产成本超支 

### 战略风险（Strategic Risk）

战略风险涉及长期商业战略决策的不确定性，例如大额资本投资、设备投资、人力资本投资或新产品失败。银行放宽贷款标准以扩大贷款规模，结果在市场压力期贷款风险大幅上升，也属于战略风险。 

## 7.7 声誉风险（Reputation Risk）

**声誉风险（Reputation Risk）** 是企业由于公众认知或消费者接受度下降而遭受损失的风险。可能来源于：

- 市场对企业财务稳健性的信心下降
- 公众认为企业没有公平对待利益相关者 

声誉风险常常是其他风险事件的结果。例如银行遭遇重大信用风险事件，可能引发声誉损害；网络攻击等操作风险也可能带来声誉风险。社交媒体会放大声誉风险，因为信息传播速度很快，且信息未必准确。 

# 8. 风险因素互动与风险汇总

## 8.1 风险因素互动（Risk Factor Interactions）

风险管理中的重大危险是原本看似独立的风险因素之间存在相关性。一个导致贷款违约的细分因素，可能进一步引发信用风险、操作风险、商业风险和声誉风险。 

这种风险联动在非预期损失中尤其危险，因为风险之间的相关性会放大损失。 

## 8.2 风险汇总（Risk Aggregation）

**风险汇总（Risk Aggregation）** 是在企业层面衡量所有风险的过程。复杂性越高，风险假设越不可靠。 

例子：

- 单只股票的市场风险可以用历史波动率和名义金额衡量。
- 衍生品交易更复杂，波动率可能更高，不同合约风险敞口可能相互抵消，因此名义金额不一定能代表真实风险。 

期权希腊值（Option Greeks）如：

- Delta
- Gamma
- Theta
- Vega

可以用于建模不确定性，但不能简单汇总到企业整体风险层面。 

# 9. VaR、Expected Shortfall与RAROC

## 9.1 VaR的局限

VaR虽然是常用企业风险指标，但有明显缺点：

1. 实务中存在不同版本的VaR。
2. VaR依赖简化假设。
3. 风险经理可通过调整天数或置信水平改变计算结果。
4. VaR只给出损失门槛，不衡量尾部损失的严重程度。 

## 9.2 预期尾部损失（Expected Shortfall）

**Expected Shortfall** 是用于估计总尾部风险损失规模的统计指标。它关注超过VaR门槛后的损失大小，因此比VaR更能反映尾部损失的严重性。 

## 9.3 风险调整资本收益率（Risk-Adjusted Return on Capital, RAROC）

RAROC用于衡量每单位风险带来的收益：

$RAROC = \frac{after\text{-}tax\ risk\text{-}adjusted\ expected\ return}{economic\ capital}$

该公式本质上衡量“每单位风险的收益”，分子需要根据预期损失进行调整。 

RAROC的实际应用包括：

1. **业务比较（Business comparison）**：比较不同业务部门。
2. **投资分析（Investment analysis）**：评估新产品或新业务。
3. **定价策略（Pricing strategy）**：判断定价是否足以补偿风险。
4. **风险管理（Risk management）**：识别收益不足以覆盖风险的领域。 

FRM考试重点：RAROC应与股权资本成本（cost of equity）比较，只有超过资本成本的风险收益指标才应被认为可接受。 

# 10. FRM考试高频考点总结

## 10.1 必须掌握的定义

- 风险（Risk）：结果的不确定性。
- 风险管理（Risk Management）：降低、管理和监控损失可能性的流程。
- 承担风险（Risk Taking）：为了额外收益主动接受额外风险。
- 预期损失（Expected Loss, EL）：正常经营中预计发生的平均损失。
- 非预期损失（Unexpected Loss, UL）：超过平均预期损失的损失。
- VaR（Value at Risk）：给定置信水平下的损失门槛。
- 经济资本（Economic Capital）：覆盖非预期损失所需资本。
- ERM（Enterprise Risk Management）：企业整体层面的综合风险管理。
- Expected Shortfall：衡量超过VaR门槛后的尾部损失规模。
- RAROC：衡量每单位经济资本带来的风险调整后收益。

## 10.2 必背公式

$EL = EAD \times PD \times LGD$

$RAROC = \frac{after\text{-}tax\ risk\text{-}adjusted\ expected\ return}{economic\ capital}$

## 10.3 常见易错点

- 风险管理不能消除整个经济体的风险，只能转移、缓释、保留或避免风险。
- VaR不是尾部损失大小，只是给定置信水平下的损失阈值。
- Expected Shortfall比VaR更关注尾部损失的严重程度。
- 一般市场风险无法通过分散化消除，特定风险可以通过分散化缓释。
- 操作风险包括内部流程失败、人为错误、系统问题、欺诈、网络安全等。
- 弱内部控制和职责分离不足属于操作风险。
- 贷款抵押品价值低于贷款余额且借款人现金流困难时，违约风险和破产风险都会上升。
- 股票薪酬可能加剧管理层短期逐利行为，不一定能缓解风险管理利益冲突。

# 11. 一句话记忆

本章核心是：**风险管理不是消除风险，而是在识别、衡量、区分、关联、缓释和监控风险的过程中，判断承担的风险是否获得了足够补偿；FRM考试尤其重视VaR、EL/UL、ERM、主要风险类型、风险相关性、Expected Shortfall和RAROC。**

# Reading 2 公司如何管理金融风险？（How Do Firms Manage Financial Risk?）

# 0. 本章考试重点（Exam Focus）

本章延续 Reading 1 的风险管理基础，重点讨论企业如何设定风险管理目标、决定保留/避免/缓释/转移哪些风险，并通过**风险映射（Risk Mapping）**识别和排序关键风险。FRM考试尤其需要关注：风险敞口管理（managing risk exposures）、风险对冲（hedging risk exposures）、外汇风险（foreign currency risk）以及风险管理工具对企业的影响。 

# 1. 企业风险管理策略（Strategies for Risk Management）

企业可以从四种高层次风险管理策略中选择：

1. 接受风险（Accept / Retain the risk）
2. 避免风险（Avoid the risk）
3. 缓释风险（Mitigate the risk）
4. 转移风险（Transfer the risk） 

高级管理层（Senior Management）和董事会（Board of Directors）最终负责策略选择，风险经理主要提供分析和决策支持。 

## 1.1 接受/保留风险（Accept / Retain Risk）

企业可能选择保留已知风险，常见原因包括：

- 风险对企业影响很小，管理成本可能超过风险本身。
- 投资者或所有者希望暴露于该风险因素。例如，金矿所有者可能希望直接暴露于黄金价格波动。
- 风险成本可以被纳入产品定价，并转嫁给客户。 

FRM理解：  
如果某风险是企业商业模式的一部分，且收益足以补偿风险，企业可能选择保留它。

## 1.2 避免风险（Avoid Risk）

如果某项风险不是企业正常经营的自然组成部分，企业可以考虑完全避免该风险。例如停止某个业务部门的活动。 

书中举例：  
通用电气（General Electric）本质上是工业企业，但其金融服务业务在2007–2009年金融危机期间给公司带来严重压力，之后管理层意识到需要出售金融风险相关业务以保护核心业务。 

## 1.3 缓释风险（Mitigate Risk）

如果企业选择保留风险，也可以通过内部措施降低风险影响。 

例子：

- 银行通过更高贷款利率、更短期限、更多抵押品要求来缓释信用风险（Credit Risk）。
- 制造商通过自动化投资来缓释劳动力成本上升风险。
- 运输公司通过升级更节能的飞机、卡车或车辆来缓释燃料成本上升风险。 

## 1.4 转移风险（Transfer Risk）

企业也可以把风险转移给第三方，例如：

- 购买保险（Insurance）
- 使用衍生品（Derivatives） 

但风险转移通常有成本，并且会引入**交易对手风险（Counterparty Risk）**，因为企业依赖第三方在风险事件发生时履约。 

FRM重点：  
风险转移不是风险消失，而是将风险承担者从企业转移到第三方。

## 1.5 成本收益分析（Cost-Benefit Analysis）

理性企业应在充分成本收益分析后选择风险管理策略。 

例子：  
如果公司通过最坏情景分析（Worst-case Scenario Analysis）估计网络威胁可能造成7,500万美元损失，而该风险与核心业务相关，企业不能避免它，就需要比较购买保险和购买新设备缓释风险的成本，最终可能采用“缓释 + 风险转移”的组合。 

# 2. 风险偏好（Risk Appetite）与风险决策

## 2.1 五步风险管理流程

企业风险管理流程包括：

1. 识别风险偏好（Identify risk appetite）
2. 映射已知风险（Map known risks）
3. 将风险偏好操作化（Operationalize the risk appetite）
4. 实施计划（Implement a plan）
5. 监控并根据需要调整计划（Monitor and adjust） 

## 2.2 风险偏好（Risk Appetite）

**风险偏好（Risk Appetite）** 是企业愿意保留的风险水平和风险类型。它有两个重要组成部分：

- **风险意愿（Risk Willingness）**：企业为了实现商业目标而接受风险的意愿。
- **风险能力（Risk Ability）**：企业实际能够承担风险的能力，可能受到内部风险控制和监管约束限制。 

例子：  
银行的杠杆率（Leverage Ratio），即一级资本（Tier 1 Capital）占银行资产的比例，不得低于3%。这属于监管约束，会限制银行的风险能力。 

## 2.3 风险容量、风险偏好与实际接受风险

企业实际风险水平应低于最大风险容量（Risk Capacity），因为风险估计可能存在误差。 

例如：

- 总风险容量（Total Risk Capacity）= 2亿美元
- 内部风险偏好（Internal Risk Appetite）= 1.7亿美元
- 实际接受风险（Actual Risk Accepted）可能应设置在更低水平，例如1.5亿美元，以留出误差空间。 

【《风险容量、内部风险偏好与实际接受风险之间的嵌套关系图》。《第4页》】

## 2.4 董事会的作用（Role of the Board of Directors）

高级管理层和董事会需要清楚定义企业风险偏好，并以定量或定性方式向利益相关者沟通。 

沟通方式包括：

- 定性说明哪些风险要保留，哪些风险要避免、缓释或转移。
- 使用定量指标，例如风险价值（Value at Risk, VaR），说明在某置信水平和期间内可容忍的最大损失。
- 使用压力测试（Stress Testing）分析严重负面情景下的损失水平。 

### 债权人与股东的冲突

董事会设定风险偏好时，需要处理债权人（Debtholders）与股东（Shareholders）之间的潜在冲突：

- 债权人通常更关心降低风险，因为其收益上限通常只是利息。
- 股东可能愿意承担低概率但高收益的风险，以提高股权回报。 

## 2.5 设定风险偏好时的复杂问题

董事会可能需要考虑以下问题：

1. **风险偏好一致性（Unity of Risk Appetite）**  
    不同风险类型是否应有不同风险偏好？
    
2. **创业机会（Entrepreneurial Opportunities）**  
    某些风险是否可能带来竞争优势？
    
3. **相关风险层次（Layers of Correlated Risks）**  
    某个经营单位是否同时带来多种高度相关风险？
    
4. **时间范围（Time Horizon）**  
    企业应优先对冲短期风险还是长期风险？
    
5. **风险限额容忍区间（Risk Limit Tolerance Bands）**  
    是否给管理人员明确的风险操作区间？
    
6. **声誉影响（Reputational Impact）**  
    风险偏好会向利益相关者传递什么信号？
    
7. **风险衡量（Risk Measurement）**  
    企业层面的风险应如何衡量？VaR、名义限额和压力测试都有价值，但企业需要选择适合自身业务模式的指标。 
    

# 3. 风险映射（Risk Mapping）

## 3.1 风险映射的定义

企业确定风险偏好后，应建立所有已知风险的清单，这一过程称为**风险映射（Risk Mapping）**。风险映射系统性考虑所有已知或潜在会影响现金流的风险。 

需要考虑的风险类型包括：

- 市场风险（Market Risk）
- 信用风险（Credit Risk）
- 流动性风险（Liquidity Risk）
- 操作风险（Operational Risk）
- 法律与监管风险（Legal and Regulatory Risk）
- 商业与战略风险（Business and Strategic Risk）
- 声誉风险（Reputation Risk） 

风险经理还应考虑：

- 风险之间的相关性（Correlation Risk）
- 风险之间可能存在的抵消效应，即风险净额化（Risk Netting） 

## 3.2 风险映射需要的细节

风险映射越细，结果越有用。企业至少需要了解：

- 风险敞口规模（Magnitude of Exposure）
- 风险发生时间（Timing of Exposure）
- 风险发生地点（Location of Exposure）
- 计算方法：当前值还是预测值（Current or Projected Values）
- 是否存在风险净额化（Risk Netting） 

例子1：铜价格风险  
如果制造商暴露于铜价风险，风险经理需要知道企业需要多少铜、何时需要、在哪些地区或生产设施需要，而不是只知道“企业有铜风险”。 

例子2：外币销售风险  
如果企业在12个国家销售产品，涉及8种货币，则需要明确当前销售、预期销售、金额、时间和具体币种，并考虑企业是否需要保留部分外币用于运营。 

FRM重点：  
风险映射的最终目标是帮助管理层决定哪些风险应保留、避免、缓释或转移。 

# 4. 对冲风险敞口（Hedging Risk Exposures）

并非所有可以对冲的风险都应该对冲。有些投资者希望企业保留特定风险暴露，例如商品价格风险或货币风险。 

对冲的主要目标是：

- 提高财务稳定性（Financial Stability）
- 降低财务困境风险（Financial Distress Risk），例如破产风险或声誉风险 

## 4.1 对冲的实际优点（Hedging Advantages in Practice）

### 1. 降低资本成本（Cost of Capital）

企业通过降低盈利或现金流波动，可能降低债务或股权资本成本，从而促进经济增长。 

### 2. 提高举债能力（Debt Capacity）

现金流更稳定的企业可能获得更高债务容量，并且贷款契约限制可能更少。 

### 3. 改善现金流（Cash Flow Enhancement）

对冲可能不只是转移风险，还可能带来现金流改善。例如商品期货对冲头寸可能产生较大盈利。 

### 4. 降低税负波动

如果对冲平滑收入和成本，企业税务负担可能下降，从而直接改善现金流。 

### 5. 传递稳定信号（Signaling）

稳定的经营表现向贷款人、客户、供应商和员工传递企业稳健的信号，也可能反映在股价中。 

### 6. 方便业务规划

风险受控后，企业业务规划更容易，管理层也可能锁定较好的利润率。 

### 7. 衍生品可能比保险更便宜

使用互换（Swaps）或期权（Options）等衍生品有时比购买保险更便宜。 

## 4.2 对冲的理论缺点（Hedging Disadvantages in Theory）

### 1. Modigliani-Miller观点

在1958年，Franco Modigliani 和 Merton Miller 认为，在完全竞争资本市场、无交易成本和无税收的假设下，企业和个人投资者可以以相同成本进行同样金融交易，因此企业对冲不会改变公司价值。 

FRM理解：  
该观点依赖“不存在交易成本和税收”的假设，现实中并不符合。

### 2. CAPM观点

William Sharpe 在1964年提出资本资产定价模型（Capital Asset Pricing Model, CAPM），认为在完美资本市场下，企业只需关注系统性风险（Systematic Risk / Beta Risk），无需关注非系统性风险（Unsystematic Risk / Idiosyncratic Risk），因为投资者可通过分散化消除后者。 

FRM理解：  
现实中分散化也有交易成本，完美资本市场假设并不现实。 

### 3. “零和博弈”观点

一些市场参与者认为对冲是零和博弈（Zero-Sum Game），不会长期增加企业盈利或现金流，只是把现金流在不同期间移动。 

但该观点也假设完美资本市场，并假设衍生品定价完全反映所有风险因素。现实中衍生品定价非常复杂，不一定准确反映所有风险因素，因此对冲未必总是零和博弈。 

## 4.3 对冲的实际缺点（Hedging Disadvantages in Practice）

对冲可能带来以下现实问题：

- 管理层注意力从核心业务转移。
- 合规成本上升，例如披露、审计和监控成本。
- 衍生品结构复杂，杠杆会增加风险分析难度。
- 对冲某一风险可能引入新的非预期风险。
- 对冲可能改变付款结构，例如将短期付款换成未来大额付款，从而转移或放大风险。
- 使用衍生品可能披露企业原本希望保密的运营信息。 

FRM重点：  
对冲并非免费午餐。对冲工具本身也可能创造新的风险。

## 4.4 对冲实施中的挑战

企业实施对冲策略时可能遇到：

1. **风险映射错误（Mis-mapping Risk Exposures）**  
    可能选错风险、漏掉风险或错误估计风险，导致衍生品名义金额过大或过小。 
    
2. **市场趋势变化（Changing Market Trends）**  
    商品价格、外汇汇率、利率等变量动态变化，企业风险管理过程也必须动态调整。 
    
3. **沟通不足（Poor Communication）**  
    对冲策略和潜在后果如果未充分传达，可能放大损失。 
    
4. **缺乏专业能力（Lack of Human Capital）**  
    对冲需要特定技能、知识、研究和时间，企业可能需要外包给可信第三方风险经理。 
    

### MGRM案例

MG Refining and Marketing（MGRM）在1993年的失败是沟通不足的典型例子。MGRM是Metallgesellschaft AG的美国子公司，曾与客户签订10年期、总计1.5亿桶汽油和取暖油交付协议，并使用滚动短期期货和OTC互换进行对冲。油价变化导致重大保证金追缴，母公司关闭对冲头寸并锁定重大损失；之后油市反转，使未对冲敞口产生第二波损失。 

## 4.5 建立强风险文化（Risk Culture）

为应对对冲挑战，企业应建立强内部风险文化。建议包括：

1. 定期沟通风险目标和风险限额接近被突破时的预警信号。
2. 培训关键员工，使其统一理解风险管理目标。
3. 关键员工应了解突破风险限额的后果。
4. 董事会应能够清楚说明企业前十大风险。 

# 5. 操作风险与金融风险的对冲方法

## 5.1 操作风险（Operational Risk）与金融风险（Financial Risk）

本章中，对冲操作风险主要覆盖生产和销售活动，即收入与费用相关风险，可理解为利润表风险（Income Statement Risks）。 

金融风险（Financial Risk）则与资产负债表（Balance Sheet）相关，即资产和负债风险。 

## 5.2 定价风险（Pricing Risk）

输入成本可能严重影响企业竞争能力，因此企业可以通过购买远期合约（Forward Contract）或期货合约（Futures Contract），提前锁定某一投入品的固定价格和数量。 

例子：  
如果企业需要某种原材料，可以使用远期或期货提前锁定购买价格，降低未来成本不确定性。 

## 5.3 外汇风险（Foreign Currency Risk）

对冲外汇风险的目标是控制汇率波动对未来现金流、资产和负债公允价值的影响。 

### 5.3.1 收入对冲（Revenue Hedging）

当企业向外国客户销售产品并以外币收款时，若外币兑换成本币时贬值，企业会遭受损失。企业可以对预期外币收入进行部分对冲。 

可用工具包括：

- **货币看跌期权（Currency Put Options）**：保证当汇率跌破执行价格时，企业仍有已知最低回报。
- **远期合约（Forward Contracts）**：以提前确定且企业可接受的汇率锁定未来回报。 

### 5.3.2 资产负债表敞口对冲

企业还需关注汇率波动对外国投资净货币资产（Net Monetary Assets）的影响。远期合约常用于抵消汇率变化对净货币资产造成的损益。 

此外，外币债务（Foreign Currency Debt）也可以作为自然对冲（Natural Hedge），抵消外国投资资产价值下降。 

FRM重点：  
如果对冲成本过高，企业可能故意保留部分外币敞口不对冲。 

## 5.4 利率风险（Interest Rate Risk）

对冲利率风险的目标是控制企业资产或负债对不利利率波动的净敞口。 

常用工具：

- 利率互换（Interest Rate Swaps）
- 互换期权（Swaptions） 

这些工具既可以帮助企业避免损失，也可能帮助企业降低借款成本。与外汇风险一样，如果对冲成本过高，部分利率敞口可能被故意保留不对冲。 

# 6. 静态与动态对冲策略

## 6.1 静态对冲（Static Hedging Strategy）

**静态对冲策略（Static Hedging Strategy）** 是较简单的过程：企业先确定风险头寸，然后使用合适的对冲工具尽可能匹配该风险头寸，以最小化基差风险（Basis Risk）。 

## 6.2 动态对冲（Dynamic Hedging Strategy）

**动态对冲策略（Dynamic Hedging Strategy）** 更复杂，因为它承认标的风险头寸的属性会随时间变化。若企业希望维持初始风险头寸，就需要持续调整对冲，产生额外交易成本，并需要更多监控时间和精力。 

## 6.3 其他对冲考虑事项

企业还需要考虑：

- 对冲时间范围（Hedging Time Horizon）与业绩评估期间是否匹配。
- 衍生品对冲的财务会计影响。
- 如果对冲工具与基础头寸不是完全匹配，损益可能需要计入利润表。
- 衍生品税务处理复杂，并影响现金流；不同国家税法也可能不同。 

# 7. 风险管理工具的影响

企业需要决定对冲策略是一次性事件，还是更广泛风险管理计划的一部分。这被称为**风险管理计划规模适配（Rightsizing a Risk Management Program）**。 

如果企业采用广泛风险管理策略，就需要投资复杂系统并聘请有经验的交易员。 

## 7.1 风险限额（Risk Limits）

企业应根据风险映射结果决定需要控制哪些风险限额。书中列出的风险限额包括：

【《风险限额表：列示止损限额、名义限额、风险特定限额、期限/缺口限额、集中度限额、希腊值限额、VaR和压力测试/情景分析等限额的目的与潜在弱点》。《第12页》】

重要风险限额包括：

- **止损限额（Stop Loss Limits）**
- **名义限额（Notional Limits）**
- **风险特定限额（Risk Specific Limits）**
- **期限/缺口限额（Maturity / Gap Limits）**
- **集中度限额（Concentration Limits）**
- **希腊值限额（Greek Limits）**
- **风险价值（Value at Risk, VaR）**
- **压力测试或情景分析（Stress Testing or Scenario Analysis）** 

## 7.2 风险管理是成本中心还是利润中心？

企业需要明确风险管理目标：

- **成本中心（Cost Center）**：目标是最小化风险对企业的负面影响。
- **利润中心（Profit Center）**：目标是通过风险管理工具直接增加净利润、提升股东价值。 

企业还需要决定风险管理工具的实际成本是分摊到业务部门，还是记录在企业整体层面。 

# 8. 衍生品工具（Derivatives Instruments）

企业常用风险管理工具包括：

## 8.1 远期合约（Forward Contracts）

远期合约是场外交易（Over-the-Counter, OTC）产品，由两个交易对手直接签订，条款可完全定制。结算可以是现金，也可以是实物，例如石油桶。 

## 8.2 期货合约（Futures Contracts）

期货合约是交易所交易产品（Exchange-Traded Product），条款标准化，不能像远期合约一样定制。期货通过清算中介降低交易对手风险。 

## 8.3 互换合约（Swap Contracts）

互换是可定制的OTC产品，双方同意交换经济头寸。例如利率互换中，一方支付固定利率，同时从对手方收取浮动利率。 

## 8.4 看涨期权（Call Option Contracts）

看涨期权买方有权但无义务在特定执行价格买入标的证券。欧式期权（European Options）只能在到期日行权，美式期权（American Options）可在到期日前行权。 

## 8.5 看跌期权（Put Option Contracts）

看跌期权买方有权但无义务在特定执行价格卖出标的证券。欧式期权只能在到期日行权，美式期权可在到期日前行权。 

## 8.6 奇异期权（Exotic Option Contracts）

奇异期权是更复杂的期权，具有不同变体。例如亚洲期权（Asian Options）使用平均价格。 

## 8.7 互换期权（Swaption Contracts）

互换期权赋予买方权利但无义务，在未来某个日期进入一项互换合约，且互换条款在互换期权开始时已确定。 

# 9. 交易所衍生品 vs OTC衍生品

## 9.1 交易所交易衍生品（Exchange-Traded Derivatives）

优点：

- 流动性较好
- 交易成本较低
- 交易对手风险较低，因为通过交易所清算 

缺点：

- 条款标准化
- 可能无法精确匹配风险管理者在标的资产、时间或交割地点上的需求
- 可能产生基差风险（Basis Risk） 

## 9.2 场外衍生品（OTC Derivatives）

优点：

- 高度定制
- 可更精确匹配企业风险敞口 

缺点：

- 流动性较低
- 成本可能较高
- 存在较高交易对手风险和结算风险（Settlement Risk） 

## 9.3 航空燃油风险例子

航空公司高度暴露于航空燃油价格风险。由于行业竞争激烈，航空公司通常无法直接将燃油价格波动转嫁给客户。市场上没有专门的交易所交易航空燃油产品，因此航空公司可以考虑使用与原油相关的产品，但这会产生基差风险，并要求管理原油与航空燃油之间的价差。许多航空公司更倾向使用能针对航空燃油的OTC产品，但这会带来交易对手风险。 

Delta Airlines 曾采取不同方式，通过购买炼油厂进行垂直整合，以控制航空燃油价格，但这也使其暴露于炼油业务自身风险。 

# 10. FRM考试必背公式与概念

本章没有复杂计算公式，但以下概念必须掌握：

## 10.1 四种风险管理策略

- 接受风险（Accept / Retain Risk）
- 避免风险（Avoid Risk）
- 缓释风险（Mitigate Risk）
- 转移风险（Transfer Risk） 

## 10.2 风险偏好核心结构

- 风险偏好（Risk Appetite）= 企业愿意保留的风险水平和类型
- 风险意愿（Risk Willingness）= 想承担多少风险
- 风险能力（Risk Ability）= 实际能承担多少风险
- 实际风险水平应低于风险偏好，风险偏好应低于风险容量。 

## 10.3 风险映射关键维度

- 规模（Magnitude）
- 时间（Timing）
- 地点（Location）
- 当前/预测口径（Current or Projected Values）
- 净额化可能性（Risk Netting） 

## 10.4 对冲工具比较

- 远期（Forward）：OTC，可定制，有交易对手风险。
- 期货（Futures）：交易所交易，标准化，交易对手风险较低，但可能有基差风险。
- 互换（Swap）：OTC，可交换经济头寸。
- 看涨期权（Call Option）：买方有权买入。
- 看跌期权（Put Option）：买方有权卖出。
- 奇异期权（Exotic Option）：复杂期权结构。
- 互换期权（Swaption）：未来进入互换的权利。 

# 11. 高频易错点总结

1. **使用期货和远期合约对冲外汇风险属于风险转移（Transfer Risk），不是单纯保留或避免风险。** 
    
2. **风险偏好可以只用定性方式表达，也可以用定量方式表达。** 
    
3. **债权人和股东的风险偏好不同：债权人更偏好降低风险，股东可能愿意承担低概率高收益风险。** 
    
4. **在现实中，使用衍生品对冲不一定是零和博弈，因为衍生品定价复杂，未必反映所有风险因素。** 
    
5. **购买保险和使用金融衍生品都可用于风险管理，但教材区分保险与金融衍生品意义上的对冲。** 
    
6. **期货合约标准化，不适合高度定制、动态且特殊现金流的风险敞口。** 
    
7. **OTC产品可定制，但交易对手风险更高。** 
    
8. **对冲某个风险可能创造新的风险，尤其是使用复杂衍生品时。** 

# 12. 一句话记忆

本章核心是：**企业风险管理不是简单“把风险都对冲掉”，而是先设定风险偏好（Risk Appetite），再通过风险映射（Risk Mapping）理解风险的规模、时间、地点和相关性，最后选择接受、避免、缓释或转移风险；对冲可以提高稳定性，但也会带来成本、复杂性、交易对手风险和新的风险敞口。**

# Reading 3 风险管理治理（The Governance of Risk Management）

# 0. 本章考试重点（Exam Focus）

本章重点是**公司治理（Corporate Governance）**与**风险管理治理（Risk Governance）**。FRM考试需要特别关注：公司治理和风险管理的最佳实践、企业内部各功能部门在风险管理生态系统中的相互依赖关系，以及董事会主要委员会的目的和职责，例如风险管理委员会（Risk Management Committee）、薪酬委员会（Compensation Committee）和审计委员会（Audit Committee）。

# 1. 公司治理（Corporate Governance）基础

## 1.1 公司治理的定义

广义上，**公司治理（Corporate Governance）** 是企业为运营业务而建立的一系列流程，涉及股东（Shareholders）、高级管理层（Senior Management）以及董事会（Board of Directors）。

公司治理从较模糊的原则逐渐发展为明确的最佳实践，主要是受到一系列大型公司治理失败事件的推动，例如 Enron、WorldCom、Global Crossing 和 Parmalat SpA。

## 1.2 Sarbanes-Oxley Act, SOX

**Sarbanes-Oxley Act（SOX）** 是2002年美国出台的法律，针对上市公司施加严格的财务报告和审计要求。

SOX的重要要求包括：

- 首席财务官（Chief Financial Officer, CFO）和首席执行官（Chief Executive Officer, CEO）必须亲自确认并认证提交给美国证券交易委员会（Securities and Exchange Commission, SEC）的财务文件准确性。
- CFO和CEO必须证明所有披露能准确反映公司状况。
- 公司必须具备特定内部控制，例如董事会和审计委员会组成方面的要求。
- 内部控制缺陷和发现的欺诈行为必须及时、准确地披露给投资者和监管机构。
- 公司报告程序和内部控制必须每年接受审计。
- 审计委员会成员姓名必须公开披露，且成员应理解会计原则、理解财务报表并具备审计经验。

> FRM提醒：欧洲监管机构没有采用类似SOX的强制规则，而是采用“遵守或解释（Comply-or-Explain）”模式，即企业可以选择不遵守推荐最佳实践，但必须解释原因。

# 2. 2007–2009年金融危机后的监管与风险治理变化

## 2.1 金融危机暴露的风险管理失败

2007–2009年金融危机与多项风险管理失败有关。危机核心是大量证券化抵押贷款产品，即**抵押贷款支持证券（Mortgage-Backed Securities, MBS）**，与**次级贷款（Subprime Loans）**相关。次级贷款违约率上升后，相关MBS在金融系统内造成巨大损失。

危机也显示，许多金融机构和评级机构缺乏充分的风险评估和控制系统。这些失败来自承销标准下降、监督失效、管理层偏重短期利润而非长期伦理决策，以及复杂结构化产品的过度使用。

## 2.2 金融危机后的关键教训

银行业在金融危机中的风险管理失败带来以下教训：

### 1. 利益相关者优先级（Stakeholder Priority）

部分企业有多元利益相关者，例如存款人、借款人、监管机构、员工、债券持有人和股东。这些群体的需求可能相互冲突，使风险管理更复杂。

### 2. 董事会组成（Board Composition）

金融危机没有清楚证明“董事会独立性一定改善结果”。在银行业中，董事是内部人还是外部人并未表现出明显不同结果，这可能是由于外部力量无法仅靠独立性缓释。

### 3. 董事会风险监督（Board Risk Oversight）

危机明确说明董事会成员需要在风险管理过程中更加主动。董事会成员需要接受教育，以理解其监督角色的重要性，以及董事会与风险管理基础设施之间的联系。

### 4. 风险偏好（Risk Appetite）

董事会需要清楚表达并沟通公司的风险偏好，并将风险预算转化为企业层面的风险限额系统。

### 5. 薪酬（Compensation）

董事会应控制管理层薪酬制度，避免激励不必要的冒险行为。可以考虑延期奖金（Deferred Bonus Payments）和追回条款（Clawback Provisions）。

# 3. Basel框架与金融危机后的银行监管

## 3.1 Basel Committee on Banking Supervision, BCBS

**Basel Committee on Banking Supervision（BCBS）** 由全球多个司法辖区的银行监管机构组成，制定一系列银行风险管理和资本充足标准。这些标准本身不具有法律强制力，但代表稳健风险管理最佳实践。

## 3.2 Basel I

**Basel I**，即1988年巴塞尔协议，建立了银行资本充足标准的统一方法。它主要关注信用风险（Credit Risk），建议银行最低资本为风险加权资产（Risk-Weighted Assets）的8%。

## 3.3 Basel II

**Basel II** 在2006年取代Basel I，将交易活动和贷款活动都纳入资本充足标准，同时提出披露建议和监管监督标准。

## 3.4 Basel III

**Basel III** 是对2007–2009年金融危机的直接回应，同时考虑公司特定风险（Idiosyncratic Risk）和市场层面系统性风险（Systematic Risk）。

Basel III的重要内容包括：

- **一级资本（Tier 1 Capital）**：核心银行实力指标，限于普通股权益和留存收益。
- **流动性覆盖率（Liquidity Coverage Ratio, LCR）**：银行必须持有足够高流动性资产，以满足30天现金需求。
- **净稳定资金比率（Net Stable Funding Ratio, NSFR）**：鼓励银行拥有至少一年稳定现金流，以支持所需运营。
- **宏观审慎覆盖（Macroprudential Overlay）**：用于降低系统性风险和顺周期性。

### Basel III的五项宏观审慎措施

1. 杠杆率（Leverage Ratio）限制：一级资本 / 合并总资产，最低为3%。
2. 逆周期资本缓冲（Countercyclical Capital Buffer）。
3. 全球系统重要性银行需满足最低总损失吸收资本标准。
4. 鼓励尽可能多的交易进行中央清算，以降低交易对手风险（Counterparty Risk）。
5. 改进风险建模和压力测试，以更好捕捉尾部风险（Tail Risk）。

## 3.5 BCBS 2015年银行风险管理指南

BCBS在2015年发布修订后的银行业风险管理指南，主要包括：

1. 董事会最终负责监督高级管理层执行公司风险偏好、战略目标和治理框架。
2. 董事会成员应具备监督职责所需的资格、知识和技能。
3. 董事会应建立自身运作政策，以强化治理目标。
4. 高级管理层应按照董事会批准的战略进行日常经营。
5. 集团企业中，母公司董事会应最终监督整个集团所有成员的运营。
6. 企业应有独立风险管理职能，并由首席风险官（Chief Risk Officer, CRO）日常监督，向董事会汇报。
7. 董事会最终负责监督风险识别、监控和控制，包括决定风险应保留、避免、缓释还是转移。
8. 风险偏好和风险管理流程需要有效传达到公司各层级。
9. 董事会最终负责监督合规风险管理。
10. 应进行定期审计，以向董事会反馈风险管理进展。
11. 董事会应组织和监督薪酬结构，使管理层对风险决策承担财务责任。
12. 公司应向利益相关者充分披露风险管理流程。

## 3.6 Fundamental Review of the Trading Book, FRTB

2016年，Basel III扩展纳入**交易账簿基本审查（Fundamental Review of the Trading Book, FRTB）**。FRTB旨在扩大市场风险敞口的覆盖范围，重点关注银行交易台通过衍生品、期货以及其他复杂金融资产引入的风险。

# 4. Dodd-Frank Act

## 4.1 背景：Glass-Steagall与Graham-Leach-Bliley

在1999年前，美国银行受**Glass-Steagall Act**约束，商业银行不能在同一公司内经营投资银行部门，核心思想是保护存款人免受交易波动影响。1999年，**Graham-Leach-Bliley Act**取消了这一隔离，使银行控股公司可转为金融服务控股公司，将商业银行、投资银行、保险和经纪交易业务置于同一公司架构下。

金融危机后，美国在2010年7月出台**Dodd-Frank Act**，以处理金融消费者保护和市场稳定问题。

## 4.2 Dodd-Frank七项核心内容

1. **强化美联储（Strengthen the Fed）**  
    美联储获得对资产超过500亿美元的系统重要性金融机构（Systemically Important Financial Institutions, SIFIs）的监督权。
    
2. **终结“大而不倒”（Ending Too Big to Fail）**  
    法案结束“大而不倒”理论，并建立有序清算授权，以处理大型金融机构失败。
    
3. **处置计划（Resolution Plan / Living Will）**  
    所有SIFIs必须向美联储提交“生前遗嘱（Living Will）”，说明公司陷入困境时的治理处置规划。
    
4. **衍生品市场（Derivatives Markets）**  
    Dodd-Frank试图通过降低交易对手风险来提高衍生品市场透明度。
    
5. **沃尔克规则（Volcker Rule）**  
    禁止银行进行自营交易（Proprietary Trading），即用银行自有资金交易。
    
6. **消费者保护（Consumer Protection）**  
    Dodd-Frank成立消费者金融保护局（Consumer Financial Protection Bureau），监管面向消费者的金融产品。
    
7. **压力测试（Stress Testing）**  
    动态压力测试必须采用自上而下方法，纳入宏观经济冲击及其对信用风险、流动性风险、市场风险和操作风险等多类风险的影响，并纳入银行流动性规划。
    

> FRM提醒：欧洲也开始考虑类似Dodd-Frank的规则，称为**监督审查与评估程序（Supervisory Review and Evaluation Process）**，要求压力测试并采用前瞻性风险规划。

# 5. 风险管理治理最佳实践

## 5.1 董事会独立性与能力

董事会应由多数独立成员组成，以保持决策和批准管理层决策时的客观性。所有董事会成员都应具备公司业务和行业的基础知识；缺乏相关知识者应在加入董事会前接受补充培训。

## 5.2 从股东利益到所有利益相关者

传统观点认为董事会应服务股东最佳利益，但现在更强调考虑所有利益相关者。不同利益相关者目标可能并不一致，例如债权人更关注下行风险，而股东更可能支持适度冒险以追求更高回报。

## 5.3 代理风险（Agency Risk）

**代理风险（Agency Risk）** 指所有者和经营者不是同一群体时产生的风险。董事会需要关注管理层是否有激励为了个人薪酬而承担短期风险，却忽略长期影响。

例如，许多股票薪酬计划可能在管理层达到短期目标时授予期权，但未充分考虑这些决策的长期影响。

## 5.4 薪酬委员会的治理作用

董事会中的**薪酬委员会（Compensation Committee）** 应设计与公司目标一致、并能减少代理风险的管理层薪酬方案。可以加入长期目标和**追回条款（Clawback Provision）**，即在特定事件发生时要求管理层返还奖金。

## 5.5 CEO与董事会主席分离

最佳实践建议 CEO 与董事会主席（Chairperson of the Board）应由两个不同且独立的人担任。这样能保证董事会有效监督管理层。

### MF Global案例

2010年，Jon Corzine同时担任MF Global的CEO和董事会主席，并忽视CRO警告，进行了大量欧洲债务工具自营交易。当希腊债务危机波及其他欧洲主权债务问题时，MF Global产生约16亿美元灾难性损失并最终破产。该案例说明CEO与董事会主席角色分离和CRO作用的重要性。

# 6. 董事会在风险管理中的最佳实践职责

董事会在风险管理中应发挥核心作用，需要理解公司已知风险、潜在影响，并表达企业层面的风险偏好。董事会也要确保风险偏好能清楚传达给利益相关者。

董事会应关注经济绩效（Economic Performance），而不只是会计绩效（Accounting Performance）。企业决策应与授权风险限额和战略目标保持一致。

实际操作中，董事会应：

1. 明确表达企业层面的风险偏好（Enterprise-Level Risk Appetite）。
2. 决定已知风险应保留、避免、缓释或转移。
3. 建立并维持CRO职位，CRO向CEO汇报，并在需要时可直接接触整个董事会。
4. 建立由了解公司风险的成员组成的风险委员会。
5. 将薪酬委员会工作与公司风险偏好和风险委员会工作连接起来。
6. 维持独立审计委员会，以监督相关行动。

# 7. 风险治理实施（Risk Governance Implementation）

## 7.1 风险顾问董事（Risk Advisory Director）

当董事会成员多来自公司行业以外时，建议设立独立的**风险顾问董事（Risk Advisory Director）**。该董事应深入理解特定行业风险因素，并向董事会提供专业风险建议。

风险顾问董事的职责包括：

- 参加风险委员会和审计委员会会议，提供行业特定指导。
- 定期与高级管理层会面，作为董事会与管理层之间的联络人。
- 教育董事会成员了解公司治理和风险管理最佳实践。

无论是否有风险顾问董事，董事会都应审查：

- 公司风险管理政策。
- 定期风险管理报告。
- 风险偏好及其对业务战略的影响。
- 内部控制。
- 财务报表和披露。
- 关联方和关联方交易。
- 内部或外部审计报告。
- 行业公司治理最佳实践。
- 竞争对手和行业风险管理实践。

## 7.2 风险管理委员会（Risk Management Committee）

**风险管理委员会（Risk Management Committee）** 是董事会下属委员会，负责设定公司风险偏好，并独立监控持续风险管理。

其职责包括：

- 与内部和外部审计师保持联系，以确保遵守监管规定和内部风险限额等相关政策。
- 监督公司所有已知风险。
- 批准高层级风险决策。
- 在银行情境下，参与批准超过特定限额或接近阈值的信贷额度。

## 7.3 薪酬委员会（Compensation Committee）

**薪酬委员会（Compensation Committee）** 独立于管理层，负责讨论和批准关键管理人员薪酬。

薪酬委员会应避免设计基于短期利润或收入的奖金计划，因为这些金额相对容易被管理层操纵。

可采用的长期风险约束机制包括：

- 延期薪酬，直到长期结果明确后再支付。
- 追回条款（Clawbacks），如果长期结果与短期结果不一致，可追回此前奖金。
- 奖金债券（Bonus Bonds），若特定监管比率被突破，奖金债券可被取消。

# 8. 风险偏好与业务战略

## 8.1 风险偏好必须匹配业务战略

企业风险偏好（Risk Appetite）必须与业务战略（Business Strategy）保持一致。若公司战略目标是发放盈利性贷款，风险限额就应设置相应信用风险参数；若目标是经营稳定，可能需要使用期货来管理操作风险或外汇风险。

## 8.2 风险监督层级

风险治理结构通常为：

- 董事会通过风险委员会设定企业层面的风险偏好。
- CRO负责日常风险监督。
- CRO可在企业层面风险限额仍处于董事会设定容忍区间内时，批准临时突破已沟通风险限额。
- CRO应向CEO汇报，但功能上也是董事会与高级管理层之间的联络人。
- CRO也会参与高级风险管理团队，该团队还包括CEO、CFO、财务主管、首席合规官和各业务职能负责人。

## 8.3 风险限额与业务机会的冲突

企业常面临业务机会与风险限额之间的张力。例如银行可能遇到一笔潜在盈利贷款，但该贷款会超过已批准的信用风险限额。此时CRO以及最终风险委员会可以批准限额延伸，也可以拒绝该贷款。

风险偏好通过风险限额被操作化，并可通过压力测试（Stress Testing）和风险价值（Value at Risk, VaR）在资产类别层面和业务部门层面进行监控。

FRM重点：

- 正常业务活动不应频繁触发风险限额突破。
- 风险限额应保留误差空间（Margin for Error）。
- 风险限额例外应以书面形式记录在每日风险限额例外报告中，并提交风险委员会了解和审查。

## 8.4 激励机制（Incentives）与风险文化

薪酬委员会需要确保管理层薪酬强化公司风险偏好。很多奖金结构鼓励短期利润，忽略长期风险，形成类似看涨期权（Call Option-like Payoff）的非对称收益结构：管理层享受利润收益，但避免承担损失痛苦。

金融危机后，G20国家建议的管理层薪酬改革包括：

1. 取消多年期奖金保证。
2. 使用延期支付和追回条款，使补充薪酬更对称并鼓励长期思维。
3. 限制激励型薪酬金额。
4. 建立披露要求，提高薪酬方案透明度。
5. 确认董事会薪酬小组委员会的独立性。

### Bonus Bond例子

**奖金债券（Bonus Bond）** 是只有在达到特定阈值时才支付收益的债券。UBS使用此制度；如果监管资本比率低于7.5%，其高管将失去奖金债券。

# 9. 企业功能部门的相互依赖

## 9.1 相互依赖（Interdependence）

公司各功能部门在风险管理和报告方面相互依赖。风险委员会监督风险管理过程，CRO监控日常风险限额，但前线经理和员工才是实际执行公司风险政策的人。

【《企业风险管理中高级管理层、业务部门、财务与运营部门、风险管理部门之间循环互动关系图》。《第11页》】

## 9.2 各部门职责

### 高级管理层（Senior Management）

高级管理层在风险委员会监督和协助下，设定公司风险偏好、设计并监督风险政策，并根据风险限额评估绩效。

### 业务部门（Business Unit）

业务部门负责实施已批准的风险政策，并及时识别任何例外情况。

### 财务与运营部门（Finance and Operations）

财务和运营部门实际执行风险缓释和风险转移交易，并分析当前风险管理工具，以确保维持风险限额。它们也帮助进行风险和业务规划。

### 风险管理部门（Risk Management Function）

由CRO领导的风险管理部门负责监控风险限额和控制，管理风险管理过程，并定期与高级管理层和风险委员会沟通。

# 10. 审计委员会（Audit Committee）

## 10.1 审计委员会的基本职责

**审计委员会（Audit Committee）** 是董事会下属委员会，传统上负责公司财务报表合理准确性和监管报告要求，同时也承担与风险管理流程相关的责任。

审计委员会需要确保：

- 董事会制定的政策正在被执行。
- 这些政策足以充分监控和控制风险敞口。

## 10.2 内部审计师的作用

内部审计师向审计委员会汇报，其职责包括：

- 监控风险管理程序。
- 跟踪现有系统进展。
- 确认现有政策和系统的有效性。
- 验证是否遵守合规标准。
- 对VaR等风险指标计算的有效性发表意见。
- 在涉及市场风险时，验证用于风险监控的定价模型，例如衍生品估值模型。
- 对内部风险估计中使用的假设，例如波动率和相关性，发表意见。

## 10.3 审计委员会的独立性与专业性

有效审计委员会的核心要求是独立于底层业务活动。审计职能需要独立于风险管理政策的日常实施。

审计委员会所有成员都必须具备足够的财务知识，包括理解相关会计规则，例如U.S. GAAP和IFRS、财务报表和内部控制。整体上，审计委员会应在独立性、业务知识和提出专业质疑问题的能力之间取得平衡。

审计委员会主要应独立于管理层，但也应与管理层合作并频繁沟通，以确保出现的问题得到处理和解决。

# 11. FRM考试必背重点

## 11.1 关键英文词汇

- 公司治理（Corporate Governance）
- 风险治理（Risk Governance）
- 董事会（Board of Directors）
- 高级管理层（Senior Management）
- 风险偏好（Risk Appetite）
- 风险限额（Risk Limits）
- 首席风险官（Chief Risk Officer, CRO）
- 风险管理委员会（Risk Management Committee）
- 薪酬委员会（Compensation Committee）
- 审计委员会（Audit Committee）
- 风险顾问董事（Risk Advisory Director）
- 代理风险（Agency Risk）
- 追回条款（Clawback Provision）
- 延期奖金（Deferred Bonus）
- 奖金债券（Bonus Bond）
- 压力测试（Stress Testing）
- 风险价值（Value at Risk, VaR）
- 流动性覆盖率（Liquidity Coverage Ratio, LCR）
- 净稳定资金比率（Net Stable Funding Ratio, NSFR）
- 交易账簿基本审查（Fundamental Review of the Trading Book, FRTB）

## 11.2 高频考点总结

1. 董事会最终负责企业层面的风险管理和风险偏好表达。
2. CEO和董事会主席最好分离，以保持董事会对管理层的监督能力。
3. CRO应向CEO汇报，但在需要时能接触整个董事会。
4. 风险管理委员会负责设定风险偏好，并独立监控风险管理。
5. 薪酬委员会应设计能降低代理风险、鼓励长期稳健决策的薪酬制度。
6. 审计委员会不仅负责财务报表，还负责监督风险管理政策是否被执行并是否有效。
7. 风险偏好必须与业务战略一致；风险限额是风险偏好的操作化工具。
8. 各功能部门相互依赖：高级管理层设定政策，业务部门执行政策，财务与运营部门执行风险缓释/转移，风险管理部门监督整体流程。

# 12. 一句话记忆

本章核心是：**董事会是风险治理的最终责任主体，必须设定并沟通风险偏好，建立CRO、风险委员会、薪酬委员会和审计委员会等治理机制，使业务战略、风险限额、管理层激励和内部审计相互一致；FRM考试尤其重视董事会职责、委员会分工、CRO角色、薪酬激励与代理风险，以及功能部门之间的风险管理联动。**

# Reading 4 信用风险转移机制（Credit Risk Transfer Mechanisms）

# 0. 本章考试重点（Exam Focus）

本章重点讲银行如何**缓释和转移信用风险（Credit Risk Mitigation and Transfer）**。FRM考试需要掌握：信用违约互换（Credit Default Swaps, CDSs）、担保债务凭证（Collateralized Debt Obligations, CDOs）如何实现风险转移，信用衍生品在2007–2009年金融危机中的作用，以及传统信用风险转移机制，如盯市（Marking-to-Market）、风险敞口净额结算（Exposure Netting）和抵押品流程（Collateral Process）。

# 1. 信用风险与信用衍生品

## 1.1 信用风险（Credit Risk）

**信用风险（Credit Risk）** 是借款人违约的风险，也是银行持有的核心风险敞口。信用衍生品（Credit Derivatives）通常是表外工具（Off-Balance Sheet Instruments），可帮助机构隔离并转移特定风险敞口。

FRM提醒：  
参与信用衍生品的交易双方都必须理解自己保留了哪些信用风险、什么事件会触发损失，以及买入或卖出信用衍生品带来的全部义务。

# 2. 信用违约互换（Credit Default Swaps, CDSs）

## 2.1 CDS定义

**信用违约互换（Credit Default Swap, CDS）** 是一种金融衍生品，当参考工具（Reference Instrument）的发行人违约时，CDS会产生赔付。参考工具可以是公司债券或证券化固定收益工具。

CDS类似保险合同：  
买方定期支付保费，通常是季度保费（Quarterly Premium Payments）；如果发生违约，买方获得赔付。

## 2.2 CDS的优点

### 1. 促进创新（Spur Innovation）

CDS买方理论上受到信用风险保护，因此可能更愿意为风险较高的机会提供资金。这种资本可得性可能促进创新和经济增长。

### 2. 现金流潜力（Cash-Flow Potential）

CDS卖方可以获得连续保费现金流。理论上，如果卖方把CDS合同分散到不同地区和行业，一个区域的违约损失可能被其他未触发CDS的保费收入抵消。

### 3. 信用风险价格发现（Risk Price Discovery）

CDS可以帮助市场发现特定借款人的信用风险价格。债券价格也反映信用风险，但债券价格还包含利率风险等其他因素；CDS更接近对单一借款人信用风险的纯粹定价。

## 2.3 CDS的缺点

### 1. 历史监管薄弱（Historically Weak Regulation）

CDS在2007–2009年金融危机前监管不足，因此存在交易对手风险（Counterparty Risk）：CDS买方不能保证CDS卖方在违约事件发生时一定能履约。

### 2. 虚假的安全感（False Sense of Security）

CDS可能使固定收益投资者产生虚假安全感，从而支持比没有信用风险转移工具时更高风险的发行人。这既可能带来融资便利，也可能鼓励过度冒险。

# 3. 担保债务凭证（Collateralized Debt Obligations, CDOs）

## 3.1 CDO定义

**担保债务凭证（Collateralized Debt Obligation, CDO）** 是一种结构化产品，银行可用它把贷款重新打包后出售给二级市场投资者，从而减轻自身信用风险。

CDO可能包含多种**资产支持证券（Asset-Backed Securities, ABSs）**，包括商业或住宅抵押贷款、汽车贷款、信用卡债务或其他贷款产品。CDO中贷款通常偏向抵押贷款，并可能通过**抵押贷款支持证券（Mortgage-Backed Security, MBS）**形式存在。

当CDO只包含抵押贷款时，技术上称为**担保抵押债务凭证（Collateralized Mortgage Obligation, CMO）**。CDO也可能包含**资产支持商业票据（Asset-Backed Commercial Paper）**。

## 3.2 CDO-squared

有时，CDO会包含另一个CDO中未能直接出售给投资者的重新包装部分，这种产品称为**CDO-squared**。CDO-squared可以把较高风险贷款部分与较低风险贷款打包，以提高对投资者的吸引力。

FRM重点：  
CDO-squared增加复杂性的主要目的，是让产品更容易销售给潜在投资者，而不是增强风险缓释能力。

## 3.3 分层结构（Tranches）

CDO中的贷款会被金融工程师组织成不同**分层（Tranches）**，即“切片”。这些分层用于分配信用风险，并满足评级机构要求。

主要分层包括：

- **权益层（Equity Tranche / Junior Tranche / Toxic Waste）**：最劣后，利率高，但只有其他层都获得付款后才收到现金流，因此风险最高。
- **夹层（Mezzanine Tranches）**：付款顺序高于权益层。
- **超优先层（Super Senior Tranche）**：通常评级为AAA，是最安全、最先获得付款的一层，但支付给投资者的利率较低。

## 3.4 CDO的优点

### 1. 增加利润潜力（Increased Profit Potential）

银行可以发放贷款、重新包装成结构化产品并出售，再用出售所得发放新贷款。这种循环提高贷款周转率，从而增加利润潜力。

### 2. 直接风险转移（Direct Risk Transfer）

通过证券化过程，银行可以有效地把信用风险转移给投资者。

### 3. 扩大贷款可得性（Loan Access）

由于银行可以重新包装并出售贷款，原本可能无法获得贷款的个人也可能获得融资。

## 3.5 CDO的缺点

### 1. 鼓励更高风险承担（Encourages Increased Risk Taking）

如果银行知道可以转移信用风险，它们可能发放比原本愿意接受的更高风险贷款，从而给投资者带来非预期风险。

### 2. 风险集中可能性（Risk Concentration Potential）

结构化产品可能在投资者未充分意识到的情况下集中暴露于高风险借款人，若这些借款人违约，投资者可能遭受非预期损失。

### 3. 高复杂性（High Complexity）

结构化产品非常复杂，投资者、评级机构和监管者都可能难以完全理解其风险。

# 4. 担保贷款凭证（Collateralized Loan Obligations, CLOs）

**担保贷款凭证（Collateralized Loan Obligation, CLO）** 与CDO非常相似，都是重新打包贷款并按分层结构出售的结构化产品。不同点是，CLO主要由银行贷款组成，而这些银行贷款通常经历较严格的承销流程。

CLO在2007–2009年金融危机中没有经历CDO市场那样严重的违约问题，主要原因是CDO市场对抵押贷款有较重暴露。因此，金融危机后，CLO仍能吸引投资者兴趣，而CDO很快失去市场兴趣。

# 5. 传统信用风险缓释机制

除了直接使用信用衍生品，银行还可以使用传统方法转移或缓释信用风险。

## 5.1 购买第三方保险（Purchase Third-Party Insurance）

银行可以为单个借款人或一组借款人的失败购买保险。如果保险针对单个借款人，这种保险覆盖技术上称为**担保（Guarantee）**。

## 5.2 风险敞口净额结算（Exposure Netting）

当银行与同一交易对手有多个风险产品敞口时，通常会按最终财务影响对这些敞口进行净额结算。

## 5.3 盯市（Marking-to-Market）

**盯市（Marking-to-Market）** 是指定期重估信用衍生品，并立即向获利方转移所需付款。这样可以防止一方在信用衍生品到期时无法支付大额尾款。盯市主要用于交易所交易衍生品。

## 5.4 要求抵押品（Requiring Collateral）

银行发放贷款时通常要求借款人提供抵押品（Collateral），抵押品可抵消贷款人的信用风险敞口。

### 错向风险（Wrong Way Risk）

**错向风险（Wrong Way Risk）** 是指抵押品价值受到与借款人违约风险相同因素的负面影响。

例子：  
能源公司借款购买生产所需石油，并用石油作为抵押品。如果油价下跌，公司可能出现经营问题并触发违约，同时抵押品价值也下降。

## 5.5 终止条款（Termination Clause）

银行可以在信用风险转移交易中加入终止条款；若触发事件发生，交易头寸自动终止。触发事件可能包括信用评级下调或未达到特定财务指标，例如毛利水平或利息覆盖率。

## 5.6 重新转让与贷款银团（Reassignment and Syndication）

银行可约定在触发事件发生时自动将信用风险转移给第三方。银行也可把一笔贷款的信用风险分散给其他贷款人，这称为**贷款银团（Syndication）**。

贷款银团通常用于大型贷款。主办银行（Lead Bank）通常保留约20%的贷款，并寻找其他银行持有剩余约80%。

银团安排分为：

- **坚定承诺（Firm Commitment）**：主办银行保证借款人可获得全部所需贷款；若找不到其他银行分担，主办银行必须自行承担全部风险。
- **尽最大努力（Best-Efforts Basis）**：主办银行尽力寻找合作银行，但不保证借款人获得全部资金。

FRM提醒：  
银团贷款本身不能精准转移某些特定风险；银行仍保留其持有贷款部分对应的全部风险，可能需要同时使用信用衍生品来转移不想保留的风险。

# 6. 信用衍生品与2007–2009年金融危机

## 6.1 CDS市场的系统性风险

2007–2009年金融危机是信用衍生品风险转移能力的一次现实测试。危机暴露出系统性集中风险：太少的流动性提供者成为大量信用衍生品的交易对手，而实际敞口规模远超市场参与者理解。

2007年CDS市场名义金额达到45万亿美元，超过美国股票、美国国债和未偿还抵押贷款的总和。市场规模膨胀的原因之一，是投资者购买并不持有资产对应的CDS，试图从负面市场变化中获利。

## 6.2 Lehman Brothers与AIG例子

CDS存在交易对手风险，这在Lehman Brothers破产时充分暴露。Lehman Brothers约6,000亿美元未偿债务中，大约4,000亿美元由CDS覆盖。

Lehman Brothers倒闭后，CDS卖方，例如American International Group（AIG）和Citadel hedge fund company，也几乎崩溃，因为它们没有预期所有CDS合约会同时到期。

## 6.3 CDO在危机中的作用

CDO允许银行将多笔贷款从资产负债表中移出，并重新包装成新的固定收益衍生资产出售给投资者。危机前，许多高风险借款人的贷款被发放，目的就是重新包装并出售。

可调利率贷款（Adjustable-Rate Loans）和次级贷款（Subprime Loans）的频繁使用最终引发问题。当可调利率贷款进入重设日期（Reset Dates）时，借款人发现自己无法偿还债务，违约水平上升使投资者对CDO产品的兴趣完全停止。

## 6.4 利率上升与房价下跌

美联储（Federal Reserve）在2004至2006年提高联邦基金利率，这一过程与可调利率抵押贷款的重设日期重合。当消费者付款上升到难以负担的水平时，房价也在下跌，借款人既无法负担还款，也无法出售房屋。结果是大规模违约，并波及MBS、CDO和CDO-squared产品。

FRM重点：  
问题不在于信用风险转移工具本身存在，而在于这些工具被误用甚至滥用。

# 7. 危机后的监管变化

## 7.1 Dodd-Frank Act

**Dodd-Frank Wall Street Reform Act of 2009（Dodd-Frank）** 被制定出来，以解决金融危机前监管不足的问题。其内含的**Volcker Rule** 禁止商业存款银行进行自营交易，并禁止其投资衍生品，例如CDS。

Dodd-Frank还要求Commodity Futures Trading Commission监管所有互换合约，包括CDS。

## 7.2 Section 15G

2014年，美国证券交易委员会（Securities and Exchange Commission, SEC）在Securities and Exchange Act中加入**Section 15G**。该规则要求证券化产品发起人，例如MBS、CDO和CLO的发起人，必须在资产负债表上保留至少5%的信用风险。

Section 15G的目标是让发起人更加关注其重新包装并出售给投资者的产品质量。发起人不得转移或缓释这5%的风险，必须以原始形式保留该信用风险。

# 8. 证券化与特殊目的实体

## 8.1 证券化（Securitization）

**证券化（Securitization）** 是把贷款重新包装成一个可在二级市场出售给投资者的新产品的过程。

证券化包括四个关键步骤：

1. 创建**特殊目的实体（Special Purpose Vehicle, SPV）**。SPV是表外法律实体，类似发行母公司的半隐藏子公司，并以投资者难以分析的方式持有金融资产。
2. SPV使用借入资金从一家或多家银行购买贷款资产，并创建结构化产品，如CMO、CDO或CLO。
3. SPV中的贷款按优先级或信用评级排列，并分成不同分层，形成SPV内部的风险层级。
4. 不同分层随后在二级市场出售给投资者。

## 8.2 可进入SPV的贷款类型

可被放入SPV形成结构化贷款产品的贷款包括：商业抵押贷款、住宅抵押贷款、汽车贷款、信用卡贷款、学生贷款，以及无法被重新包装进其他SPV产品的其他贷款。

## 8.3 买入并持有模式（Buy-and-Hold Strategy）

传统银行模式是**买入并持有策略（Buy-and-Hold Strategy）**。在该模式下，银行发放贷款并保留在账上，通过定期收取利息来补偿自己承担的信用风险。

## 8.4 发起并分销模式（Originate-to-Distribute, OTD Model）

**发起并分销模式（Originate-to-Distribute, OTD Model）** 是证券化带来的创新。银行发放贷款时的明确意图，是将贷款证券化并把结构化产品出售给投资者。

在OTD模式下，银行不保留信用风险，而是通过为证券化产品提供贷款来源收取费用；贷款利息则属于结构化产品投资者。OTD模式激励银行追求高贷款数量，而不是高贷款质量；相反，买入并持有模式激励银行关注贷款质量。

FRM提醒：  
OTD模式的热情部分来自Basel资本充足要求，因为某些负债以表外形式持有时更容易满足资本要求。

# 9. OTD模式的优缺点

## 9.1 OTD模式优点

### 1. 银行盈利能力（Bank Profitability）

OTD模式可以降低短期盈利波动，并优化资本使用。

### 2. 风险管理（Risk Management）

信用风险和利率风险可以分散到不同市场参与者之间。

### 3. 投资者选择（Investor Options）

投资者可以接触到更广泛的信用产品，这些分散化工具以前并不容易直接获得。

### 4. 贷款可得性（Loan Access）

OTD模式使借款人能获得更多信用产品，并降低借款成本。

## 9.2 OTD模式缺点

### 1. 道德风险（Moral Hazard）

由于银行发放贷款后会出售贷款，银行没有足够激励维持最高承销标准。

### 2. 激励错配（Misaligned Incentives）

OTD模式鼓励关注短期盈利，而不是长期稳定或可持续性。

### 3. 不透明性（Opaqueness）

证券化过程对投资者缺乏透明度，使投资者难以准确理解自己承担的风险。

## 9.3 结构化投资工具（Structured Investment Vehicle, SIV）

有时银行会把证券化产品保留在高度杠杆化的表外资产中，这种资产称为**结构化投资工具（Structured Investment Vehicle, SIV）**。SIV的目的是从短期利率与长期利率之间的利差中获利，但当危机中基础贷款违约率飙升时，这种安排反噬银行。

FRM提醒：  
SIV是SPV的一种，关注短期与长期利率之间的差异。

# 10. 本章重要图表

本文件未提供需要总结的图片或表格。

# 11. FRM考试必背重点

## 11.1 关键英文词汇

- 信用风险（Credit Risk）
- 信用衍生品（Credit Derivatives）
- 信用违约互换（Credit Default Swap, CDS）
- 担保债务凭证（Collateralized Debt Obligation, CDO）
- 担保贷款凭证（Collateralized Loan Obligation, CLO）
- 资产支持证券（Asset-Backed Securities, ABSs）
- 抵押贷款支持证券（Mortgage-Backed Security, MBS）
- 担保抵押债务凭证（Collateralized Mortgage Obligation, CMO）
- 分层（Tranches）
- 权益层（Equity Tranche）
- 夹层（Mezzanine Tranche）
- 超优先层（Super Senior Tranche）
- 风险敞口净额结算（Exposure Netting）
- 盯市（Marking-to-Market）
- 抵押品（Collateral）
- 错向风险（Wrong Way Risk）
- 终止条款（Termination Clause）
- 贷款银团（Syndication）
- 坚定承诺（Firm Commitment）
- 尽最大努力（Best-Efforts Basis）
- 证券化（Securitization）
- 特殊目的实体（Special Purpose Vehicle, SPV）
- 发起并分销模式（Originate-to-Distribute, OTD Model）
- 买入并持有策略（Buy-and-Hold Strategy）
- 结构化投资工具（Structured Investment Vehicle, SIV）

## 11.2 高频易错点

1. **信用衍生品本身不是金融危机的直接原因，误用和滥用才是关键问题。**
2. **CDS像保险，但存在交易对手风险，尤其当大量合约同时触发时。**
3. **CDO通过证券化把银行贷款风险转移给投资者，但也可能鼓励银行降低承销标准。**
4. **CLO类似CDO，但主要包含银行贷款，并且金融危机后仍保持投资者兴趣。**
5. **错向风险是抵押品价值与借款人信用质量同时恶化。**
6. **贷款银团能分散贷款风险，但不能精准转移特定风险。**
7. **Section 15G要求证券化发起人保留至少5%信用风险，且不能转移或缓释。**
8. **OTD模式激励贷款数量而非贷款质量，因此可能产生道德风险和激励错配。**
9. **证券化提高贷款可得性和风险分散能力，但过程不透明。**

# 12. 一句话记忆

本章核心是：**银行可以通过CDS、CDO、CLO、保险、净额结算、盯市、抵押品、终止条款和贷款银团等方式转移或缓释信用风险；但证券化和OTD模式若激励错配、信息不透明且监管不足，就可能把风险从单家银行扩散为系统性危机。**

# Reading 5 现代投资组合理论与资本资产定价模型（Modern Portfolio Theory and the CAPM）

# 0. 本章考试重点（Exam Focus）

本章介绍**现代投资组合理论（Modern Portfolio Theory, MPT）**、**有效前沿（Efficient Frontier）**、**资本市场线（Capital Market Line, CML）**、**证券市场线（Security Market Line, SML）**、**贝塔（Beta）**与**资本资产定价模型（Capital Asset Pricing Model, CAPM）**。FRM考试尤其重视CAPM计算，以及风险调整绩效指标，例如**夏普比率（Sharpe Measure）**、**特雷诺比率（Treynor Measure）**、**詹森阿尔法（Jensen’s Alpha）**、**信息比率（Information Ratio）**和**索提诺比率（Sortino Ratio）**。

# 1. 现代投资组合理论（Modern Portfolio Theory, MPT）

## 1.1 Markowitz投资组合理论

**Harry Markowitz** 在20世纪50年代初奠定了现代投资组合理论（MPT）的基础。MPT的核心思想是：理性且厌恶风险的投资者会在给定目标收益下尽量降低投资组合风险，或者在给定风险水平下追求最高收益。

Markowitz理论的主要假设包括：

1. **收益率服从正态分布（Returns are normally distributed）**  
    投资者只关注收益分布的均值（Mean）和方差（Variance），忽略偏度（Skewness）和峰度（Kurtosis）。
    
2. **投资者理性且厌恶风险（Rational and risk-averse investors）**  
    若两个投资机会风险相同，理性投资者会选择预期收益更高的投资。
    
3. **资本市场完美（Perfect capital markets）**  
    不存在税收、佣金和交易成本；所有投资者都能获得全部信息；市场参与者之间存在完全竞争。
    

## 1.2 分散化与相关性（Diversification and Correlation）

投资组合收益（Portfolio Return）是各资产收益的加权平均，但投资组合方差（Portfolio Variance）取决于资产之间的相关性（Correlation）。

FRM考试重点：

- 当相关系数 $ρ = +1$ 时，没有分散化收益，投资组合方差只是个别资产方差的加权平均。
- 当 $ρ < 1$ 时，可以产生分散化收益，投资组合风险下降。
- 相关性越低，分散化收益越大。
- 当 $ρ = -1$ 时，理论上可以构造零方差投资组合，即合成无风险资产（Synthetic Risk-Free Asset）。

【《相关性对投资组合风险的影响：展示不同相关系数下两资产组合风险如何变化》。《第2页》】

## 1.3 公司特定风险与市场风险

通过持有足够大的分散化投资组合，投资者可以降低甚至消除**公司特定风险（Company-Specific Risk / Idiosyncratic Risk）**。例如会计欺诈、网络攻击、关键人员流失等，只影响单个公司而不影响整个市场。

当公司特定风险被分散掉后，投资组合主要暴露于**一般市场风险（General Market Risk / Systematic Risk）**。因此，在MPT和CAPM框架下，投资者不应因承担可分散的公司特定风险而获得额外补偿，风险补偿应来自市场风险暴露。

# 2. 有效前沿（Efficient Frontier）

## 2.1 有效前沿定义

**有效前沿（Efficient Frontier）** 是在不同风险水平下能提供最高预期收益的投资组合集合。理性投资者追求每单位风险的最大收益，因此在没有无风险资产时，只会选择有效前沿上的投资组合。

【《有效前沿图：展示全球最小方差组合、低效组合和不可达到组合的位置》。《第3页》】

## 2.2 全球最小方差组合（Global Minimum Variance Portfolio）

有效前沿最左端的点是**全球最小方差组合（Global Minimum Variance Portfolio）**，即在所有有效组合中总风险最低的组合。

FRM考试重点：

- 有效前沿下方的组合是低效组合（Inefficient Portfolios），因为同样风险下存在收益更高的组合。
- 有效前沿上方的组合不可达到（Unattainable）。
- 投资者根据自身风险厌恶程度选择有效前沿上的不同位置。

# 3. 资本市场线（Capital Market Line, CML）

## 3.1 引入无风险资产

当引入**无风险资产（Risk-Free Asset）** 后，投资者会将无风险资产与某个有效投资组合结合，以最大化风险调整收益。教材中常用美国短期国债（U.S. Treasury Bill, T-bill）作为无风险资产代理。

在投资者具有**同质预期（Homogeneous Expectations）** 的情况下，只有一条与有效前沿相切的线，这条线就是**资本市场线（Capital Market Line, CML）**。

【《资本市场线图：展示无风险资产、市场组合M以及CML与有效前沿的切点》。《第4页》】

## 3.2 市场组合（Market Portfolio）

CML的切点组合被定义为**市场组合（Market Portfolio）**，理论上包含世界上所有风险资产。在实务中，股票市场指数常被用作市场组合的代理，例如S&P 500。

所有投资者都会持有某种组合：

- 无风险资产（Risk-Free Asset）
- 市场组合（Market Portfolio）

风险厌恶程度较高的投资者会更多持有无风险资产；风险偏好较高的投资者可能借入无风险资金并加杠杆投资市场组合。

## 3.3 CML公式

资本市场线公式为：

$E(R_p)=R_f+\left[\frac{E(R_M)-R_f}{\sigma_M}\right]\sigma_p$

其中：

- $E(R_p)$：投资组合预期收益
- $R_f$：无风险利率（Risk-Free Rate）
- $E(R_M)$：市场组合预期收益
- $\sigma_M$：市场组合标准差
- $\sigma_p$：投资组合标准差

CML斜率等于市场组合的**夏普比率（Sharpe Measure）**。

FRM易错点：  
CML用于有效投资组合，不用于单个证券的必要收益率计算；单个证券应使用CAPM或SML。

# 4. CAPM的假设（Assumptions of CAPM）

**资本资产定价模型（Capital Asset Pricing Model, CAPM）** 由 William Sharpe 和 John Lintner 在20世纪60年代发展而来，建立在MPT和CML思想之上。

CAPM主要假设包括：

1. 信息免费可得（Information is freely available）。
2. 市场无摩擦（Frictionless markets），没有税收、佣金和交易成本。
3. 资产可无限分割（Fractional investments are possible）。
4. 完全竞争（Perfect competition），单个投资者不能影响市场价格。
5. 投资者只基于预期收益和方差做决策。
6. 投资者可以按无风险利率无限借入和贷出。
7. 投资者具有同质预期，即对预期收益、方差和协方差有相同预测。

FRM提醒：  
CAPM假设明显不完全现实，因此不能机械依赖CAPM结果。

# 5. 贝塔（Beta）与系统性风险

## 5.1 Beta定义

**贝塔（Beta）** 衡量资产收益对市场收益变动的敏感度，也就是资产的**系统性风险（Systematic Risk）**。在CAPM中，资产预期收益只取决于其对市场风险的贡献，因为公司特定风险可以被分散掉。

Beta公式：

$\beta_i=\frac{Cov(R_i,R_M)}{\sigma_M^2}$

也可写为：

$\beta_i=\rho_{i,M}\frac{\sigma_i}{\sigma_M}$

其中：

- $Cov(R_i,R_M)$：资产收益与市场收益的协方差
- $\sigma_M^2$：市场收益方差
- $\rho_{i,M}$：资产与市场收益相关系数
- $\sigma_i$：资产收益标准差
- $\sigma_M$：市场收益标准差

## 5.2 Beta解释

- $\beta = 1$：资产与市场一对一变动。
- $\beta > 1$：资产比市场波动更大，市场风险更高，通常称为周期性股票（Cyclical Stock），例如奢侈品股票。
- $\beta < 1$：资产比市场波动更小，称为防御性股票（Defensive Stock），例如公用事业股票。
- 周期性股票通常在经济扩张期表现较好，防御性股票通常在经济衰退期表现较好。

## 5.3 Beta计算例子

若市场标准差为20%，资产A标准差为30%，资产与市场相关系数为0.8，则：

$\beta_A=0.8\times\frac{0.30}{0.20}=1.2$

若资产A与市场收益协方差为0.048，市场方差为 $0.20^2=0.04$，则：

$\beta_A=\frac{0.048}{0.04}=1.2$

【《Beta计算与回归估计图：展示用相关系数/协方差计算Beta，以及用回归斜率估计Beta》。《第6页》】

## 5.4 用回归估计Beta

实务中，Beta通常通过将资产收益对市场收益进行回归估计。回归图中，资产超额收益为因变量，市场超额收益为自变量，最小二乘回归线（Least Squares Regression Line）的斜率就是Beta估计值。

# 6. 证券市场线（Security Market Line, SML）与CAPM推导

## 6.1 SML定义

**证券市场线（Security Market Line, SML）** 是CAPM的图形表示，描述资产预期收益与Beta之间的线性关系。

【《证券市场线图：展示无风险利率截距、市场组合点和SML斜率》。《第7页》】

CAPM推导思路：

- 公司特定风险可以分散掉。
- 预期收益只取决于Beta。
- 预期收益是Beta的线性函数。

## 6.2 SML截距与斜率

当 $\beta = 0$ 时，没有系统性风险，唯一符合条件的资产是无风险资产，因此SML截距为 $R_f$。

市场组合的Beta等于1，因此市场组合坐标为 $(1,R_M)$。SML斜率为：

$\frac{R_M-R_f}{1-0}=R_M-R_f$

该斜率称为**市场风险溢价（Market Risk Premium, MRP）**。

## 6.3 CAPM公式

CAPM核心公式为：

$E(R_i)=R_f+\beta_i[E(R_M)-R_f]$

其中：

- $E(R_i)$：资产 $i$ 的预期收益或必要收益率
- $R_f$：无风险利率
- $\beta_i$：资产 $i$ 的Beta
- $E(R_M)-R_f$：市场风险溢价（Market Risk Premium, MRP）

CAPM计算出的收益率可视为投资者要求的**最低必要收益率（Minimum Required Return）**或**门槛收益率（Hurdle Rate）**。

## 6.4 CAPM估值判断

若分析师预测收益率不同于CAPM必要收益率，则证券可能被错误定价。

- 若预测收益率 < CAPM必要收益率：证券被高估（Overvalued），位于SML下方。
- 若预测收益率 > CAPM必要收益率：证券被低估（Undervalued），位于SML上方。

## 6.5 CAPM计算例子

若某股票的无风险利率为4%，市场风险溢价为5%，Beta为1.5，则：

$E(R)=0.04+1.5(0.05)=0.115=11.5\%$

因此该股票的门槛收益率为11.5%。若投资者预测该股票未来收益率高于11.5%，则股票低估；若预测收益率低于11.5%，则股票高估。

【《Sky-Air股票CAPM计算图：给出市场风险溢价、无风险利率和Beta，并展示SML上的必要收益率》。《第8页》】

# 7. CML与SML比较

## 7.1 CML

**资本市场线（CML）** 描述有效投资组合的预期收益与总风险（标准差）之间的关系。其横轴是标准差 $\sigma$，只适用于有效组合。

## 7.2 SML

**证券市场线（SML）** 描述单个资产或投资组合的预期收益与系统性风险（Beta）之间的关系。其横轴是 $\beta$，可用于单个证券、低效组合和有效组合。

FRM重点区分：

- CML使用总风险（Total Risk / Standard Deviation）。
- SML使用系统性风险（Systematic Risk / Beta）。
- CML的斜率是市场夏普比率。
- SML的斜率是市场风险溢价（MRP）。

【《均值-方差分析图：展示无风险资产与组合P的连接线，并用于考查市场组合、Beta和全球最小方差组合概念》。《第9页》】

# 8. 风险调整绩效指标（Risk-Adjusted Performance Measures）

投资组合经理不能只看原始收益率（Raw Return），还必须分析为获得这些收益承担了多少风险。因此，需要使用风险调整收益指标。

本章介绍的绩效指标包括：

- 夏普绩效指数（Sharpe Performance Index, SPI）
- 特雷诺绩效指数（Treynor Performance Index, TPI）
- 詹森绩效指数 / 詹森阿尔法（Jensen’s Performance Index / Jensen’s Alpha）
- 跟踪误差（Tracking Error）
- 信息比率（Information Ratio, IR）
- 索提诺比率（Sortino Ratio）

## 8.1 夏普绩效指数（Sharpe Performance Index, SPI）

**夏普比率（Sharpe Measure）** 衡量每单位总风险带来的超额收益。它使用标准差作为风险度量。

公式：

$SPI=\frac{E(R_p)-R_f}{\sigma_p}$

其中：

- $E(R_p)-R_f$：投资组合超额收益
- $\sigma_p$：投资组合标准差，即总风险

适用场景：

- 可用于所有投资组合，因为它使用总风险。
- 对未充分分散化组合特别有用，因为这类组合仍有较高公司特定风险。

【《Sharpe、Treynor与Jensen公式图：列示三种传统风险调整绩效指标公式》。《第11页》】

## 8.2 特雷诺绩效指数（Treynor Performance Index, TPI）

**特雷诺比率（Treynor Measure）** 衡量每单位系统性风险带来的超额收益。它与Sharpe比率分子相同，但分母使用Beta而不是标准差。

公式：

$TPI=\frac{E(R_p)-R_f}{\beta_p}$

适用场景：

- 更适合已经充分分散化的投资组合。
- 因为充分分散化后，公司特定风险被消除，剩余风险主要是系统性风险。

市场组合的Treynor比率等于市场风险溢价：

$TPI_M=\frac{E(R_M)-R_f}{1}=E(R_M)-R_f=MRP$

## 8.3 詹森阿尔法（Jensen’s Alpha）

**詹森阿尔法（Jensen’s Alpha）** 比较投资组合实际或预期收益与CAPM要求收益之间的差异。

公式：

$JPI=\alpha_p=E(R_p)-[R_f+\beta_p(E(R_M)-R_f)]$

解释：

- $\alpha_p = 0$：投资组合收益等于CAPM要求收益，没有错误定价。
- $\alpha_p > 0$：投资组合表现优于CAPM要求收益，可能被低估，投资者可买入或持有。
- $\alpha_p < 0$：投资组合表现低于CAPM要求收益。

Jensen’s Alpha适合比较系统性风险水平相同的投资组合。

【《绩效指标计算示例：计算股票组合与市场组合的Sharpe、Treynor和Jensen’s Alpha》。《第12页》】

## 8.4 跟踪误差（Tracking Error）

**跟踪误差（Tracking Error）** 是投资组合收益与基准收益差异的标准差。它衡量主动收益相对于基准的波动性。

公式：

$Tracking\ Error=\sigma(R_p-R_B)$

其中：

- $R_p$：投资组合收益
- $R_B$：基准收益

FRM提醒：  
有些实务人士也会把跟踪误差简单称为 $R_p-R_B$，但本章用于信息比率分母的定义是收益差异的标准差。

## 8.5 信息比率（Information Ratio, IR）

**信息比率（Information Ratio, IR）** 衡量每单位主动风险带来的主动收益。

公式：

$IR=\frac{E(R_p)-E(R_B)}{Tracking\ Error}$

其中：

- $E(R_p)-E(R_B)$：主动收益（Active Return）
- Tracking Error：主动风险（Active Risk）

## 8.6 索提诺比率（Sortino Ratio）

**索提诺比率（Sortino Ratio）** 类似Sharpe比率，但有两个变化：

1. 用最低可接受收益（Minimum Acceptable Return, $R_{MIN}$）替代无风险利率。
2. 用下行偏差（Downside Deviation）替代标准差。

公式：

$Sortino\ Ratio=\frac{R_p-R_{MIN}}{Downside\ Deviation}$

下行偏差只衡量低于最低可接受收益的收益波动，高于 $R_{MIN}$ 的收益不被视为风险。

【《信息比率与索提诺比率计算示例：给出主动组合、FTSE 100、跟踪误差、最低可接受收益和下行偏差》。《第14页》】

【《Sortino Ratio题目数据表：列示无风险利率、最低可接受收益、基准收益、组合收益、Beta、标准差和下行偏差》。《第15页》】

# 9. 公式汇总：FRM必须背熟

## 9.1 CML公式

$E(R_p)=R_f+\left[\frac{E(R_M)-R_f}{\sigma_M}\right]\sigma_p$

## 9.2 Beta公式

$\beta_i=\frac{Cov(R_i,R_M)}{\sigma_M^2}$

$\beta_i=\rho_{i,M}\frac{\sigma_i}{\sigma_M}$

## 9.3 CAPM公式

$E(R_i)=R_f+\beta_i[E(R_M)-R_f]$

## 9.4 Sharpe Measure

$SPI=\frac{E(R_p)-R_f}{\sigma_p}$

## 9.5 Treynor Measure

$TPI=\frac{E(R_p)-R_f}{\beta_p}$

## 9.6 Jensen’s Alpha

$\alpha_p=E(R_p)-[R_f+\beta_p(E(R_M)-R_f)]$

## 9.7 Tracking Error

$Tracking\ Error=\sigma(R_p-R_B)$

## 9.8 Information Ratio

$IR=\frac{E(R_p)-E(R_B)}{Tracking\ Error}$

## 9.9 Sortino Ratio

$Sortino\ Ratio=\frac{R_p-R_{MIN}}{Downside\ Deviation}$

上述公式均为本章关键概念总结中的考试重点。

【《关键概念公式页：汇总CAPM、CML、Beta、Sharpe、Treynor、Jensen’s Alpha、Information Ratio和Sortino Ratio公式》。《第16页》】

【《Module Quiz 5.2答案计算页：展示CostSave股票CAPM必要收益率与估值判断》。《第17页》】

【《Module Quiz 5.3答案计算页：展示市场预期收益反推与Sortino Ratio计算》。《第18页》】

# 10. 高频易错点总结

1. **CML不能用于单个证券必要收益率计算；CAPM/SML才用于单个证券。**
2. **CML横轴是标准差，总风险；SML横轴是Beta，系统性风险。**
3. **市场组合Beta等于1。**
4. **CAPM必要收益率是最低要求收益率，不一定等于分析师预测收益率。**
5. **预测收益率高于CAPM必要收益率，证券低估；预测收益率低于CAPM必要收益率，证券高估。**
6. **Sharpe使用标准差，适合所有组合，尤其关注总风险。**
7. **Treynor和Jensen’s Alpha使用Beta，更适合充分分散化组合。**
8. **Treynor高但Sharpe低，通常说明组合未充分分散化，因为总风险较高。**
9. **Information Ratio使用相对于基准的超额收益和跟踪误差。**
10. **Sortino Ratio只惩罚下行波动，不惩罚高于最低可接受收益的波动。**

# 11. 一句话记忆

本章核心是：**MPT说明分散化可以消除公司特定风险，CML描述有效组合的总风险与收益关系，SML/CAPM描述单个资产的Beta与必要收益率关系；FRM考试必须熟练掌握CAPM、Beta、Sharpe、Treynor、Jensen’s Alpha、Information Ratio和Sortino Ratio的公式、适用场景与估值判断。**

# Reading 6 套利定价理论与多因子风险收益模型（The Arbitrage Pricing Theory and Multifactor Models of Risk and Return）

# 0. 本章考试重点（Exam Focus）

本章比较**资本资产定价模型（Capital Asset Pricing Model, CAPM）**与**套利定价理论（Arbitrage Pricing Theory, APT）**。CAPM认为资产预期收益只由其对市场组合的暴露决定，即Beta；APT则认为资产预期收益由多个与宏观经济相关的风险因子暴露决定，这些风险暴露称为**因子Beta（Factor Betas）**。FRM考试重点包括：用单因子模型和多因子模型计算预期收益、理解Fama-French三因子模型，以及如何用多因子方法构建对冲组合。

# 1. 套利定价理论（Arbitrage Pricing Theory, APT）

## 1.1 APT的基本思想

传统CAPM用单一市场指数，例如S&P 500 Index，解释金融资产收益，并用Beta衡量资产对该指数的风险暴露。1976年，Steven Ross提出**套利定价理论（APT）**，它是一种多因子模型，用多个风险因子解释金融资产收益。

APT中的风险因子可以包括：

- 金融指数（Financial Indices），例如股票指数、债券指数或商品指数。
- 宏观经济变量（Macroeconomic Variables），例如GDP、利率指标、生产指标和就业变量。

在APT语境中，“套利（Arbitrage）”并不是指模型预期存在套利机会，而是指模型在多个风险因子下衡量预期收益；APT假设不存在可利用的套利机会，即使出现，也会很快被市场参与者交易行为消除。

## 1.2 APT公式

APT的核心思想是用多个系统性风险因子更细致地解释资产预期收益。一般形式可写为：

$E(R_i)=R_f+\beta_{i,1}RP_1+\beta_{i,2}RP_2+\cdots+\beta_{i,k}RP_k$

其中：

- $E(R_i)$：资产 $i$ 的预期收益
- $R_f$：无风险利率（Risk-Free Rate）
- $\beta_{i,k}$：资产 $i$ 对第 $k$ 个因子的敏感度，即因子Beta（Factor Beta）
- $RP_k$：第 $k$ 个风险因子的风险溢价（Risk Premium）

## 1.3 APT的假设

APT的假设相对简单，主要包括：

1. 市场参与者追求利润最大化。
2. 市场无摩擦（Frictionless Markets），即没有交易成本、税收或卖空限制等障碍。
3. 不存在套利机会；若出现套利机会，会被追求利润最大化的投资者迅速利用并消除。

FRM重点：APT的假设比CAPM更少、更简单，但它没有规定必须使用哪些因子，因此灵活性更高，同时也带来模型选择难度。

## 1.4 APT可使用的宏观因子示例

APT没有指定固定因子。Chen、Roll和Ross提出过一种APT因子结构，包括：

- 短期与长期利率之间的利差，即收益率曲线（Yield Curve）。
- 预期通胀与非预期通胀（Expected versus Unexpected Inflation）。
- 工业生产（Industrial Production）。
- 低风险与高风险公司债收益率之间的利差。

【《APT模型公式与宏观风险因子示例：展示APT多因子公式及Chen、Roll和Ross提出的四类宏观因子》。《第2页》】

## 1.5 APT与CAPM比较

### CAPM

CAPM只考虑资产对市场组合的风险暴露，即市场Beta。它认为资产预期收益由市场风险解释。

### APT

APT使用多个风险因子解释资产收益，能更细致地捕捉系统性风险来源。APT不规定固定因子，因此分析师可根据资产特征选择合适因子。

FRM考试对比：

- CAPM：单因子模型（Single-Factor Model），因子通常是市场组合。
- APT：多因子模型（Multifactor Model），可包含多个宏观经济或金融市场因子。
- APT更灵活，但也需要定期检查因子和更新因子Beta，因为金融市场是动态的。

# 2. 多因子模型的输入（Multifactor Model Inputs）

## 2.1 多因子模型公式

对股票 $i$，多因子模型可写为：

$R_i=E(R_i)+\beta_{i,1}F_1+\beta_{i,2}F_2+\cdots+\beta_{i,k}F_k+e_i$

其中：

- $R_i$：股票实际收益
- $E(R_i)$：股票预期收益
- $F_k$：第 $k$ 个风险因子
- $\beta_{i,k}$：股票对第 $k$ 个因子的敏感度，也称因子载荷（Factor Loading）或因子敏感度（Factor Sensitivity）
- $e_i$：误差项，代表模型无法解释的公司特定收益，即非系统性风险（Idiosyncratic Risk）

## 2.2 因子Beta（Factor Beta）

每个风险因子都需要对应一个Beta。因子Beta衡量股票收益对该因子变化的敏感程度。

例子：若GDP因子Beta为2.0，市场预期GDP为3.2%，实际GDP为2.2%，则GDP surprise为：

$2.2\%-3.2\%=-1.0\%$

由于Beta为2.0，股票预期受到的影响为：

$2.0\times(-1.0\%)=-2.0\%$

即股票收益预期下降2%。

## 2.3 误差项（Error Term）

误差项 $e_i$ 表示模型没有解释的公司特定收益。来源可能包括：

- 被遗漏但与股票收益相关的变量。
- 随机性或非理性市场行为。
- 公司特定突发事件，例如罢工、自然灾害或关税不确定性。

由于公司特定事件是随机的，误差项的期望值通常为0。

# 3. 单因子模型与多因子模型计算

## 3.1 单因子模型（Single-Factor Model）

单因子模型只考虑一个宏观因子对股票收益的影响。教材用HealthCare Inc.（HCI）普通股说明GDP surprise对收益的影响。

模型为：

$R_{HCI}=E(R_{HCI})+\beta_{GDP*} F_{GDP*}+e_{HCI}$

已知：

- HCI预期收益 = 10%
- GDP surprise因子Beta = 2.0
- 预期GDP增长率 = 3.2%
- 实际GDP增长率 = 2.6%

GDP surprise为：

$GDP^*=2.6\%-3.2\%=-0.60\%$

所以模型预测收益为：

$R_{HCI}=10\%+2.0(-0.60\%)=8.8\%$

若实际收益为8.25%，则8.25%与8.8%的差异可能来自公司特定风险，或来自单因子模型没有捕捉到的其他系统性风险。

## 3.2 多因子模型（Multifactor Model）

多因子模型可以加入多个系统性风险因子。教材继续加入消费者信心 surprise（Consumer Sentiment Surprise, $CS^*$）。

模型为：

$R_{HCI}=E(R_{HCI})+\beta_{GDP*} F_{GDP*}+\beta_{CS*}F_{CS*}+e_{HCI}$

新增信息：

- 消费者信心Beta = 1.5
- 预期消费者信心增长率 = 1.0%
- 实际消费者信心增长率 = 0.75%

消费者信心 surprise为：

$CS^*=0.75\%-1.0\%=-0.25\%$

因此预测收益为：

$R_{HCI}=10\%+2.0(-0.60\%)+1.5(-0.25\%)=8.43\%$

多因子模型预测值8.43%比单因子模型的8.8%更接近实际收益8.25%，说明加入更多相关系统性因子可以提升模型解释能力。

FRM提醒：因子与Beta都需要定期更新和验证，因为它们会随时间动态变化。

# 4. 相关性、分散化与APT

## 4.1 APT依赖充分分散化组合

APT通常应用于**充分分散化投资组合（Well-Diversified Portfolio）**。充分分散化意味着组合中资产之间相关性足够低，从而可以消除大部分公司特定风险。

分散化逻辑：

- 同一资产类别内的资产通常相关性更高。
- 不同资产类别之间的资产相关性通常更低，例如商品、房地产、工业公司和公用事业。
- 多资产类别组合会受到更多不同系统性因子影响，因此多因子模型特别适合分析。

## 4.2 APT关系适用范围

APT的主要结论是，充分分散化组合的预期收益与其因子Beta成比例。该关系不一定对每一个单独证券都成立。若组合中某一个证券违反APT关系，它对整体组合影响可能太小，不足以产生有意义的套利机会。

但如果充分分散化组合中大量证券都违反APT关系，就可能出现组合层面的套利机会。因此，APT关系必须对几乎所有证券成立。

# 5. 用多因子模型构建对冲组合

## 5.1 多因子对冲的核心思想

多因子模型能识别具体因子暴露，投资者可据此构建**因子组合（Factor Portfolios）**，有选择地保留某些风险暴露，并通过目标化资产配置缓释其他暴露。

【《三个投资组合的因子敏感度表：列示Portfolio 1、Portfolio 2和Portfolio 3对GDP surprise、Consumer sentiment surprise、Unemployment surprise和Manufacturing surprise的因子Beta》。《第6页》】

## 5.2 对冲某一个因子暴露

若投资者想消除GDP surprise风险，可寻找与GDP surprise相关且因子敏感度相同的资产或组合。教材例子中，Portfolio 1和Portfolio 2对GDP surprise的因子敏感度均为0.50。

若投资者做多Portfolio 1、做空Portfolio 2，则GDP surprise的净Beta为0，从而消除GDP surprise风险；但这种操作仍会保留或引入其他因子暴露，例如消费者信心或失业率 surprise。

## 5.3 对冲多个因子暴露

投资者也可使用衍生品构造完全对冲组合。例如构建Portfolio H：

- 50%配置在只暴露于GDP surprise的衍生品。
- 30%配置在只暴露于消费者信心 surprise的衍生品。
- 剩余20%配置在无风险资产。

若投资者做多Portfolio 1并做空Portfolio H，就可以有效缓释GDP surprise和消费者信心 surprise两个风险暴露。

## 5.4 多因子对冲与套利机会

如果Portfolio 1预期收益为12%，而对冲组合Portfolio H预期收益为10%，投资者可做多Portfolio 1、做空Portfolio H，从而在中性化因子暴露后获得潜在2%的套利利润：

$12\%-10\%=2\%$

如果Portfolio H预期收益为14%，投资者则可做多Portfolio H、做空Portfolio 1，同样隔离出潜在2%的套利机会。

## 5.5 多因子对冲的挑战

多因子对冲存在以下挑战：

1. **模型风险（Model Risk）**  
    对冲基于模型计算，若因子敏感度变化，或模型遗漏更合适的因子，对冲可能失效。
    
2. **再平衡频率问题（Rebalancing Frequency）**  
    再平衡太频繁会产生交易成本并侵蚀利润；再平衡太少可能导致不想要的风险暴露，因为市场关系会动态变化，并可能增加跟踪误差（Tracking Error）。
    
3. **平稳性假设问题（Stationarity Assumption）**  
    如果资产分布随时间并不稳定，或市场压力时期模型假设不成立，例如2007–2009年金融危机期间，对冲效果可能受到影响。
    

# 6. Fama-French三因子模型

## 6.1 从CAPM到多因子模型

CAPM是单因子模型，其公式为：

$E(R_i)=R_f+\beta_i[E(R_M)-R_f]$

多因子APT可写为：

$E(R_i)=R_f+\beta_{i,1}RP_1+\beta_{i,2}RP_2+\cdots+\beta_{i,k}RP_k+e_i$

APT的主要弱点是没有说明应选择哪些因子。Fama和French在1996年提出著名的三因子模型，明确纳入三个因子。

【《CAPM与APT公式页：展示CAPM单因子形式和APT多因子形式》。《第7页》】

## 6.2 Fama-French三因子

**Fama-French Three-Factor Model** 包含：

1. 市场风险溢价（Market Risk Premium）
2. 规模因子：小减大（Small Minus Big, SMB）
3. 估值因子：高减低（High Minus Low, HML）

### SMB：Small Minus Big

**SMB（Small Minus Big）** 是小公司股票收益与大公司股票收益之间的差异。该因子调整公司规模影响，因为小公司历史上通常比大公司有更高收益。

### HML：High Minus Low

**HML（High Minus Low）** 是高账面市值比（Book-to-Market）股票收益与低账面市值比股票收益之间的差异。高Book-to-Market意味着低Price-to-Book，因为二者互为倒数。该因子表达的是：起始估值较低的公司可能跑赢起始估值较高的公司。

FRM提醒：SMB是做多小公司、做空大公司的对冲策略；HML是做多高Book-to-Market公司、做空低Book-to-Market公司的对冲策略。

## 6.3 Fama-French三因子模型公式

$E(R_i)=R_f+\beta_{i,M}MRP+\beta_{i,SMB}SMB+\beta_{i,HML}HML+e_i$

其中：

- $\beta_{i,M}$：资产对市场风险溢价的敏感度
- $\beta_{i,SMB}$：资产对规模因子的敏感度
- $\beta_{i,HML}$：资产对估值因子的敏感度
- $MRP$：市场风险溢价
- $SMB$：小公司减大公司收益差
- $HML$：高Book-to-Market减低Book-to-Market收益差

【《Fama-French三因子模型公式与计算示例：展示市场、SMB、HML三个因子如何计算股票预期收益》。《第8页》】

## 6.4 Fama-French计算例子

已知某公司：

- 市场Beta：$\beta_M=0.85$
- SMB敏感度：$\beta_{SMB}=1.65$
- HML敏感度：$\beta_{HML}=-0.25$
- 股权风险溢价：$MRP=8.5%$
- SMB因子：$SMB=2.5%$
- HML因子：$HML=1.75%$
- 无风险利率：$R_f=2.75%$

则预期收益为：

$E(R_i)=2.75\%+0.85(8.5\%)+1.65(2.5\%)-0.25(1.75\%)$

$E(R_i)=2.75\%+7.225\%+4.125\%-0.4375\%=13.6625\%\approx13.66\%$

若实际收益不同于13.66%，差异称为**Alpha（α）**。Alpha可能来自公司特定风险，也可能说明模型需要加入其他因子来更好预测该股票未来收益。

## 6.5 Fama-French模型扩展

Fama-French三因子模型不是唯一选择，但它是广为人知的多因子模型。后来模型有扩展：

- Mark Carhart在1997年加入动量因子（Momentum Factor），形成四因子模型。
- Fama和French在2015年提出加入盈利能力因子“Robust Minus Weak, RMW”和投资风格因子“Conservative Minus Aggressive, CMA”。

# 7. FRM考试必背公式

## 7.1 APT多因子预期收益

$E(R_i)=R_f+\beta_{i,1}RP_1+\beta_{i,2}RP_2+\cdots+\beta_{i,k}RP_k$

## 7.2 多因子实际收益模型

$R_i=E(R_i)+\beta_{i,1}F_1+\beta_{i,2}F_2+\cdots+\beta_{i,k}F_k+e_i$

## 7.3 单因子模型

$R_i=E(R_i)+\beta_iF_i+e_i$

## 7.4 Fama-French三因子模型

$E(R_i)=R_f+\beta_{i,M}MRP+\beta_{i,SMB}SMB+\beta_{i,HML}HML+e_i$

# 8. 高频易错点总结

1. **APT不是固定因子模型**：APT没有预设必须使用哪些变量，因子可由分析师自定义。
2. **APT比CAPM更灵活**：CAPM只考虑市场Beta，APT可考虑多个宏观经济或金融市场因子。
3. **因子Beta可正可负**：它衡量股票与因子之间关系的方向和强度，不要求必须为正。
4. **加入更多因子不能消除公司特定风险**：多因子模型主要增强对系统性风险的解释能力。
5. **APT依赖充分分散化组合**：通过低相关资产组合减少公司特定风险后，系统性因子更重要。
6. **多因子模型可用于定向对冲**：投资者可以只对冲某个因子，也可以尝试对冲全部计算出的因子暴露。
7. **多因子对冲仍有模型风险**：因子选择错误、Beta变化或再平衡不当都会影响效果。
8. **Fama-French三因子明确包含规模因子SMB和估值因子HML，不包含动量因子**；动量因子是Carhart后来加入的。

# 9. 一句话记忆

本章核心是：**CAPM用一个市场Beta解释收益，APT和多因子模型用多个因子Beta解释收益；多因子模型更灵活、能捕捉更丰富的系统性风险，也能用于定向对冲，但因子选择、Beta更新和模型风险是FRM考试最常考的陷阱。**

# Reading 7 有效风险数据汇总与风险报告原则（Principles for Effective Data Aggregation and Risk Reporting）

# 0. 本章考试重点（Exam Focus）

本章是高度定性的章节，核心是**Basel Committee on Banking Supervision（BCBS）**关于有效**风险数据汇总（Risk Data Aggregation）**与**风险报告（Risk Reporting）**的原则。FRM考试重点在于理解：风险数据应当准确（accurate）、完整（complete）、及时（timely）、全面（comprehensive）且可适应（adaptable）；同时，银行不能为了满足某一个原则而牺牲其他原则。

# 1. 风险数据汇总的定义与好处

## 1.1 风险数据汇总（Risk Data Aggregation）

根据BCBS，**风险数据汇总（Risk Data Aggregation）** 是指按照银行风险报告要求，对风险数据进行定义、收集和处理，使银行能够衡量其表现是否符合风险容忍度/风险偏好（Risk Tolerance / Risk Appetite）。该过程包括拆分、分类和合并数据及数据集。

## 1.2 有效风险数据汇总与报告的好处

有效的风险数据汇总和报告系统可以为银行带来以下好处：

1. **增强预见问题的能力（Anticipate Problems）**  
    汇总数据使风险经理能够从整体角度理解风险，而不是孤立地看待单个风险。
    
2. **在财务压力下识别恢复路径**  
    在金融压力时期，有效风险数据汇总可帮助银行识别恢复财务健康的可行路径，例如识别合适的合并伙伴以恢复银行财务可行性。
    
3. **提高问题处置能力（Improved Resolvability）**  
    监管机构应能访问汇总风险数据，以解决银行健康与可行性相关问题；这对全球系统重要性银行（Global Systemically Important Banks, G-SIBs）尤其重要。
    
4. **提升战略决策能力与盈利能力**  
    强化风险职能后，银行能更好地进行战略决策、提高效率、降低损失概率，并最终提高盈利能力。
    

# 2. 数据质量、模型风险与实施挑战

## 2.1 数据与模型风险（Model Risk）

金融机构广泛使用模型来分析风险敞口和指导日常经营。模型依赖数据，因此数据获取是**模型风险（Model Risk）**的重要组成部分，尤其属于**输入风险（Input Risk）**。模型风险还包括估计风险（Estimation Risk）、估值风险（Valuation Risk）和对冲风险（Hedging Risk）。

FRM重点：即使模型开发过程中的小错误，也可能给银行带来严重后果。

## 2.2 银行数据管理的历史问题

银行历史上的数据收集通常是分散的，发生在部门或业务职能层面。数据可能来自不同来源而被重复记录，也可能因系统更换而被忽视或销毁。不同代存储设备之间不兼容，也加剧了数据管理问题。

BCBS认为，银行数据质量不足以在业务线之间有效汇总和报告风险敞口，因此发布了14项原则来帮助银行改革数据汇总和报告流程，即**BCBS 239**。

## 2.3 BCBS 239的目标

BCBS 239的目标是使银行能够更好地衡量其表现是否符合风险容忍度。其要求适用于模型开发中使用的数据，也与模型风险管理相关。

模型开发者必须证明模型开发中使用的数据与模型理论和方法一致；模型必须经过审查和验证。

## 2.4 数据标准化的重要性

不同部门之间的数据标准必须一致。如果数据没有标准化，银行可能无法理解真实风险。例如，如果不同部门对同一客户使用不同识别代码，银行可能无法识别该客户同时拥有汽车贷款、抵押贷款和信用卡，从而低估其整体风险敞口。

# 3. Principle 1 — 治理（Governance）

## 3.1 核心要求

BCBS认为，银行的风险数据汇总能力和风险报告实践应受到强有力治理安排约束，并与BCBS其他原则和指导保持一致。风险数据汇总应成为银行整体风险管理框架的一部分。

## 3.2 治理原则的具体要求

风险数据汇总和风险报告实践应当：

- 完全文档化（Fully documented）。
- 由具备IT、数据和风险报告专业知识的人员独立审查和验证。
- 在新产品开发、收购和剥离等新举措中被纳入考虑。
- 不受银行物理位置、地理存在或法律组织结构影响。
- 成为高级管理层的优先事项，并获得财务和人力资源支持。
- 得到董事会支持，董事会应了解银行对BCBS关键治理原则的实施和遵守情况。

FRM重点：风险数据汇总和报告系统需要大量财务和人力资源投入，其收益通常在长期体现，而不是短期立即体现。

# 4. Principle 2 — 数据架构与IT基础设施（Data Architecture and IT Infrastructure）

## 4.1 核心要求

银行应设计、建设并维护数据架构和IT基础设施，以充分支持风险数据汇总能力和风险报告实践；这不仅适用于正常时期，也适用于压力或危机时期，同时还要满足其他原则。

## 4.2 Principle 2的具体要求

银行应做到：

- 将风险数据汇总和报告纳入银行规划流程，并进行业务影响分析。
- 在银行集团范围内建立统一的数据分类和架构。
- 可以使用多个数据模型，但必须有强健的自动化对账机制。
- 数据架构应包括元数据（Metadata）以及法律实体、交易对手、客户和账户数据的命名惯例。
- 明确数据相关的问责制、角色、责任和所有权。
- 在数据生命周期和技术基础设施各环节设置充分控制。

## 4.3 数据模型（Data Models）

教材列出四类主要数据模型：

### 1. 语义数据模型（Semantic Data Models）

按照逻辑顺序组织数据，并包括数据基本含义和数据关系等语义信息。

### 2. 概念数据模型（Conceptual Data Models）

最抽象的数据模型，用于映射数据库中的概念和关系，并确认人类如何理解系统和系统目标。

### 3. 逻辑数据模型（Logical Data Models）

尽可能详细描述数据，但不关注实施。

### 4. 物理数据模型（Physical Data Models）

定义建立数据库所需组件，包括数据库表结构、列名和值、主键、外键以及表之间关系。物理数据模型把概念和逻辑数据转换为可在软硬件平台上实施的数据。

# 5. Principle 3 — 准确性与完整性（Accuracy and Integrity）

## 5.1 核心要求

银行应能够生成准确可靠的风险数据，以满足正常时期和压力/危机时期的报告准确性要求。数据汇总应尽量自动化，以降低错误概率。

## 5.2 Principle 3的具体要求

- 数据汇总和报告应准确可靠。
- 风险数据控制应像会计数据控制一样强健。
- 当银行依赖人工流程或桌面应用，如电子表格和数据库时，应建立有效控制以保证数据质量。
- 数据应与其他银行数据，包括会计数据进行对账，以确保准确性。
- 银行应努力为每类特定风险建立单一权威风险数据来源。
- 风险人员应能访问风险数据，以便有效汇总、验证、对账并报告数据。
- 风险数据定义应在全行范围内保持一致，银行可维护风险数据概念和术语字典。

FRM重点：虽然数据汇总应尽量自动化，但在需要专业判断时，人工干预是适当的；银行需要在人工和自动化风险管理系统之间保持平衡。

## 5.3 人工绕行（Manual Workaround）

监管者期望银行记录人工和自动风险数据汇总系统，并说明何时存在人工绕行、为什么这些绕行对数据准确性重要，以及如何减少人工绕行的影响。

# 6. Principle 4 — 完整性（Completeness）

## 6.1 核心要求

银行应能够捕捉并汇总整个银行集团所有重大风险数据。数据应能按业务线、法律实体、资产类型、行业、地区和其他相关分类提供，以便识别和报告风险敞口、风险集中和新兴风险。

## 6.2 Principle 4的具体要求

- 表内风险和表外风险都应被汇总。
- 风险衡量和汇总方法应清楚且足够具体，使高级管理层和董事会能评估风险敞口。
- 并非所有风险都必须用同一个指标表达。
- 风险数据应完整；若不完整，银行应识别并向监管者解释不完整区域。

# 7. Principle 5 — 及时性（Timeliness）

## 7.1 核心要求

银行应能及时生成汇总且最新的风险数据，同时满足准确性与完整性、完整性和适应性等原则。具体时间要求取决于风险性质、潜在波动性、对银行整体风险状况的重要性，以及正常和压力/危机情况下的报告频率要求。

## 7.2 关键风险示例

在压力或危机情况下，系统应能快速生成所有关键风险的汇总数据。关键风险包括但不限于：

- 对大型企业借款人的汇总信用敞口。
- 交易对手信用风险敞口，包括衍生品。
- 交易敞口、头寸和操作限额。
- 按地区和部门划分的市场集中度。
- 流动性风险指标。
- 时间敏感的操作风险指标。

FRM重点：及时性的要求因业务线不同而不同。例如，投资组合经理需要比公司贷款部门更快速、更频繁地评估风险数据。

# 8. Principle 6 — 适应性（Adaptability）

## 8.1 核心要求

银行应能生成汇总风险数据，以满足广泛的按需、临时风险管理报告要求，包括压力/危机情况下的请求、内部需求变化以及监管查询。

## 8.2 适应性的具体表现

适应性包括：

- 汇总流程应灵活，使管理者能够快速评估风险并辅助决策。
- 数据应可定制，例如异常情况、仪表盘和关键结论，并允许用户深入调查特定风险。
- 应能纳入新业务方面或影响银行整体风险的外部因素。
- 监管变化应纳入风险数据汇总。
- 银行应能从汇总风险数据中提取细节，例如某一国家或地区的风险。

## 8.3 原则之间的相互作用

准确性与完整性、完整性、及时性和适应性会相互影响。银行不应为了速度而牺牲完整性，也不应为了准确性而使数据变得无法灵活适应特定需求。银行在建立和维护风险数据汇总框架时，应同时考虑所有标准。

FRM高频点：**银行不能把某一个原则置于其他原则之上。**

# 9. Principle 7 — 报告准确性（Accuracy）

## 9.1 核心要求

风险管理报告应准确、精确地传达汇总风险数据，并以精确方式反映风险。报告应经过对账和验证。

## 9.2 报告准确性的具体要求

银行应：

- 定义生成风险报告的流程。
- 建立数据合理性检查。
- 描述需要验证的数据数学和逻辑关系。
- 创建错误报告，用于识别、报告并解释数据弱点或错误。
- 确保风险近似方法，如情景分析、敏感性分析、压力测试和其他风险建模方法可靠、准确且及时。

监管者期望银行对普通时期和压力/危机时期风险数据施加准确性要求，其重要性应类似于会计重要性（Accounting Materiality）。如果遗漏会影响风险决策，则该遗漏是重大的。

# 10. Principle 8 — 全面性（Comprehensiveness）

## 10.1 核心要求

风险管理报告应覆盖组织内所有重大风险领域。报告深度和范围应与银行运营规模、复杂度、风险状况以及报告接收者需求相一致。

## 10.2 风险报告应包括的内容

风险报告应包含但不限于：

- 信用风险（Credit Risk）。
- 市场风险（Market Risk）。
- 流动性风险（Liquidity Risk）。
- 操作风险（Operational Risk）。
- 压力测试结果（Stress Test Results）。
- 资本充足性（Capital Adequacy）。
- 监管资本（Regulatory Capital）。
- 流动性预测（Liquidity Projections）。
- 资本预测（Capital Projections）。
- 风险集中（Risk Concentrations）。
- 融资计划（Funding Plans）。

FRM提醒：教材将这些综合风险称为Pillar 1和Pillar 2风险。Pillar 1风险包括市场风险、信用风险和操作风险；Pillar 2风险扩展包括商业风险、声誉风险、集中风险、战略风险等。

# 11. Principle 9 — 清晰性与有用性（Clarity and Usefulness）

## 11.1 核心要求

风险管理报告应以清晰简洁方式传达信息。报告应易于理解，同时足够全面，以支持知情决策。报告内容应根据接收者需求定制。

## 11.2 报告内容

风险报告应包括：

- 风险数据（Risk Data）。
- 风险分析（Risk Analysis）。
- 风险解释（Interpretation of Risks）。
- 风险的定性说明（Qualitative Explanations of Risks）。

不同接收者需要不同信息。例如，风险委员会需要的信息未必与董事会完全相同；交易员需要的信息也可能不同于贷款人员。随着数据汇总层级提高，对定性解释和说明的需求也会增加。

# 12. Principle 10 — 频率（Frequency）

## 12.1 核心要求

董事会和高级管理层或其他接收者应设定风险管理报告生成和分发频率。频率要求应反映接收者需求、报告风险性质、风险变化速度，以及报告对风险管理和决策的重要性。压力/危机时期应提高报告频率。

## 12.2 频率要求的特点

- 报告频率取决于接收者、风险类型和报告目的。
- 银行应定期测试在正常和压力/危机时期是否能在既定时间框架内准确生成报告。
- 在压力/危机期间，流动性、信用和市场风险报告可能需要立即生成，以应对快速上升的风险。
- 某些情况下，由于数据量极大，报告频率反而可能需要放慢，例如随机现金流模拟。

# 13. Principle 11 — 分发（Distribution）

## 13.1 核心要求

风险管理报告应分发给相关方，同时保持必要的保密性。

## 13.2 有效与无效风险报告

有效风险报告应提供有用信息，使管理者能够采取预防性行动；报告应是动态的，界面应易于使用，并允许管理者利用风险数据进行严格分析。

无效风险报告通常表现为不灵活、难以理解或难以应用，且无法支持管理者深入回答细节问题。

教材还指出，研究显示银行在遵守这些原则方面存在困难；一项PwC研究显示，银行在风险报告原则7–11方面表现较好，但在数据汇总原则3–6方面表现较差，原则1和2也存在较低合规率。

# 14. 本章图片与表格

本文件未提供需要总结的图片或表格。

# 15. FRM考试必背重点

## 15.1 关键英文词汇

- 风险数据汇总（Risk Data Aggregation）
- 风险报告（Risk Reporting）
- 风险容忍度 / 风险偏好（Risk Tolerance / Risk Appetite）
- 模型风险（Model Risk）
- 输入风险（Input Risk）
- 估计风险（Estimation Risk）
- 估值风险（Valuation Risk）
- 对冲风险（Hedging Risk）
- BCBS 239
- 治理（Governance）
- 数据架构（Data Architecture）
- IT基础设施（IT Infrastructure）
- 准确性与完整性（Accuracy and Integrity）
- 完整性（Completeness）
- 及时性（Timeliness）
- 适应性（Adaptability）
- 全面性（Comprehensiveness）
- 清晰性与有用性（Clarity and Usefulness）
- 频率（Frequency）
- 分发（Distribution）
- 元数据（Metadata）
- 人工绕行（Manual Workaround）

## 15.2 11项原则速记

1. **Governance**：风险数据汇总应纳入整体风险管理框架。
2. **Data Architecture and IT Infrastructure**：正常和压力时期都要支持风险数据汇总和报告。
3. **Accuracy and Integrity**：风险数据应准确可靠，并尽量自动化。
4. **Completeness**：覆盖所有重大风险数据，包括表内和表外风险。
5. **Timeliness**：及时生成最新汇总风险数据。
6. **Adaptability**：满足临时、按需和监管查询需求。
7. **Accuracy**：风险报告应准确、精确、经过验证。
8. **Comprehensiveness**：风险报告覆盖所有重大风险领域。
9. **Clarity and Usefulness**：报告应清晰、有用，并针对接收者定制。
10. **Frequency**：报告频率取决于接收者、风险性质和风险变化速度。
11. **Distribution**：报告应及时分发给相关方，同时保持保密性。

## 15.3 高频易错点

1. **风险数据汇总不是单纯IT问题，而是治理问题。** 高级管理层和董事会必须投入资源并持续支持。
2. **银行应努力为每类风险建立单一权威数据源，而不是多个数据源。**
3. **及时性不能牺牲准确性和完整性。** 银行不能为了快而忽略数据质量。
4. **风险报告应根据接收者需求定制。** 董事会、风险委员会、交易员和贷款人员需要的信息不同。
5. **人工绕行不是必然违规，但必须被记录、解释，并提出减少影响的措施。**
6. **压力/危机时期报告频率通常提高，但在数据量极大的模型输出中，频率可能需要放慢以保持质量检查。**
7. **有效风险数据汇总的直接好处之一是提高银行压力或失败时的问题处置能力（Resolvability）。**

# 16. 一句话记忆

本章核心是：**有效风险数据汇总和报告要求银行在治理、数据架构、准确性、完整性、及时性、适应性、报告质量、报告频率和分发机制之间取得平衡；FRM考试尤其重视BCBS原则之间不能互相牺牲，以及高级管理层和董事会必须为RDARR投入足够资源。**

# Reading 8 企业风险管理与未来趋势（Enterprise Risk Management and Future Trends）

# 0. 本章考试重点（Exam Focus）

本章重点讲**企业风险管理（Enterprise Risk Management, ERM）**。ERM是相对较新的风险管理概念，产生于对传统“各部门各自管理风险”的模式的反思。FRM考试需要掌握：从孤岛式风险管理（Silo-Based Risk Management）转向企业整体风险管理（Enterprise-Wide Risk Management）的原因，ERM的优缺点，ERM项目的五个重要维度，强风险文化（Risk Culture）的重要性，以及情景分析（Scenario Analysis）和压力测试（Stress Testing）如何与银行资本规划（Capital Planning）联系起来。

# 1. 企业风险管理（ERM）与传统孤岛式风险管理

## 1.1 传统孤岛式风险管理（Silo-Based Risk Management）

企业面临多种风险，包括信用风险（Credit Risk）、市场风险（Market Risk）、流动性风险（Liquidity Risk）、操作风险（Operational Risk）、商业风险（Business Risk）和信息技术风险（Information Technology Risk, IT Risk）。在传统风险管理方法下，每种主要风险通常由公司内部特定部门单独评估和管理，这种方法称为**孤岛式风险管理系统（Silo-Based Risk Management System）**。例如，交易员管理市场风险，精算师管理保险风险，管理层分析商业风险。

传统方法的问题是忽略了风险的动态性和相互依赖性。某一类风险可能影响另一类风险；从公司整体角度看，不同风险或其对冲工具也可能互相抵消。若把风险逐项孤立处理，可能导致企业层面出现低效率和成本高昂的过度对冲（Overhedging）。

## 1.2 企业风险管理（Enterprise Risk Management, ERM）

**企业风险管理（ERM）** 是一种综合、集中化的风险管理框架，用于在企业整体层面管理关键风险。ERM不是把风险当成各部门的独立问题，而是把风险视为企业整体的一部分。风险管理工具仍包括避免（Avoiding）、保留（Retaining）、缓释（Mitigating）和转移（Transferring）风险，但ERM强调这些决策应从企业整体战略角度统一考虑。

【《孤岛式风险管理与ERM比较表：比较风险管理范围、风险经理角色、风险衡量方法、对冲决策、风险管理与资本管理及融资策略之间的联系》。《第2页》】

# 2. 企业采用ERM的动机（ERM Motivations）

企业采用ERM有多项好处：

1. **定义企业整体风险偏好（Risk Appetite）**  
    ERM帮助管理者定义整个企业愿意承担的风险水平，并帮助企业遵守风险约束。
    
2. **聚焦最大威胁**  
    ERM让管理者关注威胁企业生存的重大风险，而不是只关注某个部门或业务线的日常风险。
    
3. **识别来自单个业务线的企业级风险**  
    某些单一业务线可能产生影响整个企业运营的风险，ERM有助于从企业层面识别这些风险。
    
4. **更好管理新兴风险（Emerging Risks）**  
    网络威胁（Cyber Threats）、声誉风险（Reputation Risk）和反洗钱风险（Anti-Money Laundering Risk, AML Risk）等风险更适合在企业层面管理。
    
5. **支持监管合规（Regulatory Compliance）**  
    ERM有助于企业满足监管要求。
    
6. **增强利益相关者信心**  
    ERM能让股东和其他利益相关者更放心。
    
7. **理解交叉风险与相关性**  
    ERM帮助管理者理解交叉风险（Crossover Risks），即一种风险引发其他风险，以及不同风险类型之间的相关性。
    
8. **优化风险转移成本**  
    ERM可根据不同风险规模更好地管理风险转移总成本。
    
9. **将压力测试资本成本纳入定价和决策**  
    与压力测试相关的资本成本可被纳入定价和经营决策。
    
10. **将风险纳入商业模式和战略选择**  
    ERM使风险成为银行商业模式选择和战略决策的一部分。
    

# 3. ERM治理与实施最佳实践

## 3.1 公司治理的重要性

**公司治理（Corporate Governance）** 对成功实施ERM非常关键。有效治理要求高级管理层和董事会具备适当的组织实践和流程，以充分控制风险。成功的公司治理框架要求高级管理层和董事会明确企业风险偏好以及风险和损失容忍水平。

管理层还应持续支持风险管理举措，并确保公司拥有实施ERM所需的风险管理技能和组织结构。有效ERM框架还要求所有关键风险被整合进ERM项目，并且各相关方的风险角色和责任被明确界定，包括首席风险官（Chief Risk Officer, CRO）的角色。

# 4. ERM项目的五个重要维度

ERM围绕五个重要维度组织：

## 4.1 目标（Targets）

银行应设定正确的风险目标，且这些风险目标不应与机构战略目标冲突。目标包括：

- 企业风险偏好（Risk Appetite）。
- 在风险偏好约束下的战略目标。

操作机制如薪酬计划（Compensation Plans）和全球风险限额（Global Risk Limits）应与企业风险偏好相连接。

## 4.2 结构（Structure）

ERM结构需要定义相关方角色，例如CRO、全球风险委员会（Global Risk Committee）和其他风险委员会，并说明企业治理结构。该结构应确保识别企业整体风险，并考虑直接损失和间接损失。ERM结构还应建立报告线，例如业务线经理、风险委员会和CRO之间的报告关系。

## 4.3 识别与指标（Identification and Metrics）

企业风险应从对公司影响、风险严重性以及理想情况下的发生频率角度进行衡量。ERM的目标之一是确保公司拥有合适指标，以捕捉全公司层面的风险。

常用指标包括：

- 情景分析（Scenario Analysis）。
- 压力测试（Stress Testing）。
- 风险价值（Value at Risk, VaR）。
- 总风险成本方法（Total Cost of Risk Approaches）。
- 企业层面风险映射（Enterprise-Wide Risk Mapping）。
- 风险特定指标（Risk-Specific Metrics）。
- 风险标记工具（Risk Flagging Tools）。

风险经理还必须关注风险集中（Risk Concentrations），例如贷款组合中的信用风险集中、行业集中、贷款类型集中，或借款人对单一供应商过度依赖。金融机构也可能过度依赖特定数据提供商、风险分析提供商和技术提供商。

## 4.4 ERM策略（ERM Strategies）

企业必须说明在企业整体和业务线层面管理风险的方法和策略。是否避免、缓释或转移风险，应在企业层面进行决策；风险转移工具也必须被识别。

## 4.5 风险文化（Culture）

风险文化是ERM的核心。企业必须通过高层管理者到基层员工的目标、实践和行为，让员工理解风险管理的重要性。

FRM重点：仅仅任命CRO不一定能改善风险文化。如果员工认为这只是“重新包装”风险文化而没有实质改变，则效果有限。此外，ERM项目必须全面，例如如果CRO没有审查薪酬计划，就可能忽视薪酬制度鼓励冒险行为的问题。

# 5. 风险文化（Risk Culture）

## 5.1 风险文化定义

企业的**风险文化（Risk Culture）** 是影响员工行为的目标、习惯、价值观和信念，包括显性和隐性因素。这些企业规范指导个人如何理解风险并对风险作出反应。

银行监管者在2007–2009年金融危机后，将风险文化视为银行失败的主要贡献因素之一。教材列举的弱风险文化例子包括LIBOR利率操纵丑闻、银行洗钱以及金融危机前许多金融机构的次级贷款文化。

## 5.2 建立强风险文化的困难

建立强风险文化很困难，因为风险文化是多层次的。员工进入企业时已有个人风险态度，人口特征、家庭背景、经历、性格以及职业准则都会影响个人风险偏好。同事和管理层也会影响员工风险行为。风险文化存在于企业层面、群体层面和个人层面。

## 5.3 金融稳定委员会（Financial Stability Board, FSB）的四个风险文化指标

FSB提出四个风险文化指标：

1. **高层基调（Tone from the Top）**  
    管理层行为是否与既定风险目标/风险偏好冲突？薪酬计划是否支持公司价值观，还是鼓励冒险？董事会如何沟通风险偏好与公司战略目标的匹配？
    
2. **有效沟通与质疑（Effective Communication and Challenge）**  
    反对意见是否被重视？是否评估管理层对异议的开放程度？风险管理是否具有足够地位？
    
3. **激励（Incentives）**  
    薪酬计划是否支持并符合风险偏好和风险文化？
    
4. **问责（Accountability）**  
    期望是否清晰？升级流程是否被使用？
    

## 5.4 建立强风险文化的其他因素

企业还可以通过以下因素建立强风险文化：

- 员工是否了解企业风险偏好（Knowledge of the Firm’s Risk Appetite）。
- 是否有风险培训以及共同风险语言，即风险素养（Risk Literacy）。
- 风险信息是否在企业内流动（Flow of Risk Information）。
- 管理者是否在风险与回报决策上与企业风险偏好保持一致。
- CRO和风险经理在公司内是否有足够地位（Risk Management Stature）。
- 是否存在举报和升级机制（Whistleblowing and Escalation）。
- 董事会能否说出企业前十大风险。
- 违反风险标准的员工是否受到纪律处分。
- 公司能否识别风险文化问题和事件，并说明采取了哪些应对措施。

## 5.5 阻碍强风险文化的因素

可能阻碍企业形成稳健风险文化的因素包括：

1. **风险指标变成风险杠杆（Risk Indicators Become Risk Levers）**  
    企业可能更容易“管理指标”，而不是真正改善风险文化。
    
2. **风险教育不足（Risk Education）**  
    风险教育不应只面向员工，也应包括董事会成员。董事会必须能够列出关键企业风险，并将其与企业风险偏好联系起来。
    
3. **跨组织与跨时间风险（Risk Across Organization and Across Time）**  
    风险通常在业务线中形成，而不是在企业层面形成。业务线可能发展出自己的风险文化，例如销售文化，因此企业需要机制识别跨多个业务线出现的风险。
    
4. **文化周期（Culture Cycle）**  
    风险文化在危机时期更加明显；危机带来的痛感会随时间消退，风险行为可能在危机后减弱，但随着记忆淡化又重新出现。
    
5. **数据诅咒（Curse of Data）**  
    可用于风险分析的数据越来越多，企业可能需要使用机器学习从大量信息中提取有用洞察。
    

# 6. 情景分析与压力测试

## 6.1 敏感性分析 vs 情景分析

**敏感性分析（Sensitivity Analysis）** 是一次改变一个变量，并评估模型对该变量变化的敏感程度，例如评估净利润对某一变量变化的影响。

**情景分析（Scenario Analysis）** 则同时考察多个变量，并建立叙事来解释变量为何变化以及这些变化的影响。企业会建立复杂金融模型，以评估不同情景对企业风险和绩效的影响。

## 6.2 情景分析在ERM中的作用

2007–2009年金融危机显示，VaR等概率风险指标存在弱点；实际风险水平可能远超VaR模型所设想的极端情况。因此，情景分析和压力测试成为ERM项目中最重要的风险识别工具。情景分析帮助企业理解异常事件和尾部事件（Tail Events）的影响。

## 6.3 情景分析的优点

情景分析的优点包括：

- 风险发生频率不重要，只要风险是合理可能的（Plausible）。
- 情景可以直观且透明。
- 企业必须想象最坏情况及其后果。
- 有助于企业聚焦关键风险类型和风险敞口。
- 有助于发现潜在预警信号并制定应急计划。
- 可以是前瞻性的假设事件，也可以基于历史数据。
- 可以简单，也可以高度复杂。
- 可用于制定风险偏好、设定风险限额和资本充足规划。

## 6.4 情景分析的缺点

情景分析的缺点包括：

- 不利事件概率难以估计。
- 情景分析本质上是定性的，因此本身不能直接量化风险，尽管可以围绕情景建立定量模型。
- 情景很复杂。
- 情景可能低估可能事件及其影响。
- 企业可能没有开发“正确”的情景，因为只能充分开发有限数量的情景。
- 情景往往基于上一次重大危机，而不是未来可能出现的新风险。
- 情景分析可信度可能难以评估。

FRM提醒：情景分析的某些特点可能既是优点也是缺点。例如，情景叙事直观透明是优点，但情景本身复杂又是缺点。

# 7. 压力测试与资本规划

## 7.1 金融危机前后的变化

2007–2009年金融危机前，银行通常基于真实历史事件构建历史情景，例如1997年亚洲危机、1998年俄罗斯债务暂停偿付、2001年9月11日事件对金融市场的影响、2007年次级抵押贷款危机，以及2008年Lehman Brothers失败导致的交易对手危机。

金融危机后，监管者认为银行没有充分识别风险如何相互作用，也没有充分考虑压力时期市场参与者行为如何变化。监管者还指出，银行测试的情景比2007–2009年实际发生的情况温和，因此要求银行证明其能承受更严酷、更现实的情景。

## 7.2 美国银行压力测试

美国银行压力测试始于2009年的**监督资本评估计划（Supervisory Capital Assessment Program, SCAP）**。**Dodd-Frank Act Stress Tests, DFAST** 在年中进行，适用于资产不低于100亿美元的银行；**Comprehensive Capital Analysis and Review, CCAR** 在年末进行，适用于资产不低于500亿美元的银行。DFAST和CCAR使用相同监管情景，但DFAST更具规定性、报告要求较少，并且资本行动假设有限。

自2011年以来，美联储（Federal Reserve）进行年度压力测试。美联储要求银行考虑三个宏观经济情景：

1. **基准情景（Baseline）**  
    基于大型银行经济学家的共识经济预测。
    
2. **不利情景（Adverse）**  
    假设经济温和下滑。
    
3. **严重不利情景（Severely Adverse）**  
    假设全球衰退/萧条，并伴随固定收益投资需求下降。
    

## 7.3 CCAR与资本规划

CCAR要求进行九个季度的预测，非常复杂，要求银行动态预测资产负债表和利润表。银行必须预测收入、贷款损失准备、违约贷款和债务证券降级相关的信用损失、新贷款规则以及监管比率。

银行还必须基于每个情景提交资本计划，包括：

- 预测九个季度内的资本来源和资本用途。
- 描述用于判断资本充足性的方法。
- 提交详细资本政策计划。
- 讨论会影响资本充足性和流动性的业务计划变化。
- 展示在每个情景下银行仍能维持最低资本标准。
- 解释必要时如何筹集资本。
- 讨论股息支付、股票回购和其他影响资本状况的因素。

## 7.4 ERM与资本规划的联系

ERM和资本规划不完全相同，但二者高度相互关联。银行可能在CCAR报告中说明，如果遇到困难，将发行**或有可转换债券（Contingent Convertible Bonds, CoCos）**。CoCos在银行资本出现问题时转换为股权，从而在压力时期减轻银行现金流出。转换可由会计事件触发，例如一级资本水平，也可由市场事件触发，例如银行股价下跌。

从ERM角度看，CoCos类似银行保险，是一种风险转移工具。由于CoCos在冲击后价值降低，它们可能鼓励更少冒险和更强风险文化，因为高管薪酬会暴露于下行风险。CoCos的另一个特点是，即使涉及风险转移，也不需要事先定义具体风险类型。

## 7.5 最低资本标准

截至2018年，最低普通股一级资本比率（Common Equity Tier 1 Capital Ratio）为4.5%，一级风险资本要求（Tier 1 Risk-Based Capital Requirement）为6%，总风险资本比率（Total Risk-Based Capital Ratio）为8%，一级杠杆率（Tier 1 Leverage Ratio）为4%。银行必须在所有情景下满足这些最低标准。若银行在压力测试下无法满足最低资本标准，则必须降低风险偏好。

## 7.6 CCAR对ERM的促进作用

CCAR要求银行业务线经理共同讨论风险，这是ERM流程的关键。CCAR帮助银行通过九个季度逐步展开情景，而不是只进行某一时点冲击；它允许风险因素相互联系，而不是将风险孤立处理；同时允许风险因素动态变化。

由于银行测试相同情景，监管者能更好理解系统性风险，并可更好比较不同银行风险敞口。教材还指出，企业可进行**反向压力测试（Reverse Stress Tests）**，即先确定关键绩效指标（Key Performance Indicators, KPIs）的最坏结果，再倒推哪些情景导致这些结果，从而识别最脆弱业务线和关键风险因素。

# 8. 本章图片与表格

- 【《孤岛式风险管理与ERM比较表：比较风险管理范围、风险经理角色、风险衡量方法、对冲决策、风险管理与资本管理及融资策略之间的联系》。《第2页》】
- 【《Module Quiz 8.2题目图：列示关于压力测试、公司治理和风险文化的判断题陈述》。《第11页》】

# 9. FRM考试必背重点

## 9.1 关键英文词汇

- 企业风险管理（Enterprise Risk Management, ERM）
- 孤岛式风险管理（Silo-Based Risk Management）
- 风险偏好（Risk Appetite）
- 风险与损失容忍度（Risk and Loss Tolerance）
- 首席风险官（Chief Risk Officer, CRO）
- 全球风险委员会（Global Risk Committee）
- 风险文化（Risk Culture）
- 高层基调（Tone from the Top）
- 有效沟通与质疑（Effective Communication and Challenge）
- 激励（Incentives）
- 问责（Accountability）
- 情景分析（Scenario Analysis）
- 敏感性分析（Sensitivity Analysis）
- 压力测试（Stress Testing）
- 资本规划（Capital Planning）
- Dodd-Frank Act Stress Tests（DFAST）
- Comprehensive Capital Analysis and Review（CCAR）
- Supervisory Capital Assessment Program（SCAP）
- 或有可转换债券（Contingent Convertible Bonds, CoCos）
- 反向压力测试（Reverse Stress Tests）

## 9.2 ERM五个维度速记

1. **Targets**：风险偏好和战略目标。
2. **Structure**：CRO、风险委员会、治理结构和报告线。
3. **Identification and Metrics**：用情景分析、压力测试、VaR、风险映射等识别和衡量企业风险。
4. **ERM Strategies**：企业层面决定风险避免、缓释或转移策略。
5. **Culture**：风险文化是ERM的核心。

## 9.3 高频易错点

1. **ERM不是简单任命CRO**；任命CRO很常见，但不是ERM的必要条件，也不能自动修复风险文化问题。
2. **孤岛式管理容易忽略风险相关性和抵消效应**，可能造成企业层面的过度对冲。
3. **强ERM让企业聚焦最大风险，而不是只关注业务线日常风险。**
4. **风险偏好和战略目标是ERM项目中的重要Targets。**
5. **风险文化必须贯穿整个组织，不只是业务线经理的问题。**
6. **情景分析不同于敏感性分析**：情景分析同时改变多个变量，并有叙事解释；敏感性分析一次改变一个变量。
7. **情景分析既可基于历史，也可前瞻假设；考试中不要误以为压力测试只使用历史情景。**
8. **压力测试和资本规划高度相关**；若银行在压力情景下无法满足最低资本标准，就必须降低风险偏好。
9. **CCAR强化ERM思想**：它让业务线共同讨论风险，考虑风险之间的动态联动，并帮助监管者比较银行风险敞口。

# 10. 一句话记忆

本章核心是：**ERM把风险从部门孤岛提升到企业整体层面，强调风险偏好、治理结构、风险识别指标、企业级风险策略和风险文化；FRM考试尤其重视ERM优于孤岛模式的原因、强风险文化、情景分析/压力测试，以及压力测试如何服务于银行资本规划。**

# Reading 9 从金融灾难中学习（Learning from Financial Disasters）

# 0. 本章考试重点（Exam Focus）

本章通过多个金融灾难案例，说明不同风险如何导致重大损失或机构倒闭。FRM考试应重点掌握：利率风险（Interest Rate Risk）、流动性风险（Liquidity Risk）、模型风险（Model Risk）、流氓交易员风险（Rogue Trader Risk）、声誉风险（Reputation Risk）、网络风险（Cyber Risk）、金融工程风险（Financial Engineering Risk）和公司治理失败（Corporate Governance Failure）。同时要理解静态对冲（Static Hedge）与动态对冲（Dynamic Hedge）的区别，以及各案例的失败原因和可预防措施。

# 1. 利率风险：1980年代美国S&L危机

## 1.1 利率风险（Interest Rate Risk）

**利率风险（Interest Rate Risk）** 是由于利率水平波动导致损失的风险，传统上可用**久期（Duration）**衡量敏感度。

银行和储蓄贷款机构（Savings and Loan, S&L）的基本业务模式是吸收短期存款，并发放长期贷款，通过短期负债成本与长期资产收益之间的利差获利。

## 1.2 S&L危机的背景

美国S&L行业在1980年代的危机体现了未管理利率风险对企业乃至整个行业的影响。Regulation Q最初于1933年建立，并于1966年扩展到S&L，使S&L能够限制短期存款支付利率，从而最大化客户存款与长期抵押贷款之间的利差。

在1960年代末和1970年代初，收益率曲线向上倾斜，短期利率低于长期利率，S&L可以“ride the curve”，从正利差中获利。

## 1.3 危机爆发原因

1970年代末通胀上升，美联储提高利率，导致S&L的短期资金成本上升，原本的贷款利差被侵蚀甚至变为负利差。教材举例：若S&L向客户支付6%的短期存款利率，却只从长期抵押贷款收到5%，则会产生负利润利差。

为了弥补损失，一些S&L降低贷款标准并进入高风险贷款，最终导致行业重大损失。美国纳税人最终进行了约1,600亿美元的行业救助，约35%的S&L立即退出市场。

## 1.4 S&L危机的教训

银行可以通过以下方式缓释利率风险：

- 匹配资产与负债久期（Duration Matching）。如果资产和负债相关且久期相近，利率波动时二者价值变化可相互抵消。
- 使用利率衍生品（Interest Rate Derivatives），例如利率上限（Caps）、利率下限（Floors）和互换（Swaps）。

FRM易错点：延长贷款资产期限会提高资产久期，从而加大利率风险，不是缓释利率风险的方法。

# 2. 流动性风险案例

## 2.1 流动性风险（Liquidity Risk）

**流动性风险（Liquidity Risk）** 是实体无法满足短期现金需求的风险。它可来自外部市场条件、内部运营问题、资产负债表结构问题，或这些因素的组合。

# 3. Lehman Brothers：短期融资支持长期非流动资产

## 3.1 案例背景

**Lehman Brothers** 是1850年成立的投资银行。2000年代早期，Lehman大量投资于美国房地产证券化资产，积极参与发放贷款、重新包装成证券化资产并出售给投资者的过程，同时也在自身资产负债表上保留大量证券化资产。

即使2006年房价开始下降，Lehman仍继续深度参与证券化抵押贷款市场。

## 3.2 失败原因

Lehman的融资模式放大了房地产市场崩溃的影响。到2007年，Lehman的杠杆率，即资产/权益比率，为31:1。其核心融资策略是依赖短期日常回购市场（daily repo markets）借款，并用这些短期资金支持长期且相对非流动的证券化资产。

当房地产泡沫在2007年下半年破裂后，投资者越来越不愿意在短期市场借钱给Lehman及其他市场参与者。2008年9月15日凌晨，Lehman CEO宣布公司被迫申请破产。

## 3.3 教训

Lehman案例说明：用短期负债支持长期、非流动资产，在短期融资市场消失时会产生致命流动性风险。Lehman倒闭的根本原因之一是高级管理层未能充分管理银行流动性风险。

# 4. Continental Illinois：集中贷款与短期融资压力

## 4.1 案例背景

**Continental Illinois** 曾是芝加哥最大的银行，并积极从事商业和工业贷款。在1970年代末，它是油气市场的重要参与者。Oklahoma的Penn Square Bank发放油气贷款，并将大型贷款转给包括Continental Illinois在内的大银行。

## 4.2 失败原因

1981年能源市场大幅下行，Penn Square Bank破产。Continental持有约10亿美元与Penn Square相关的贷款，因此产生重大损失。

同时，Continental依赖从美联储借短期资金和出售存款证（Certificates of Deposit, CDs）融资。当这些资金来源不足以满足其增长的流动性需求时，它转向海外高利率货币市场。1984年5月，其融资问题传到海外市场后，银行即使支付更高利率也无法借款，存款人在10天内提取约20%的活期存款。

## 4.3 教训

Continental Illinois案例显示：信用损失、融资来源不足和存款挤兑可共同触发流动性危机。该案例也被认为与“太大而不能倒（Too Big to Fail）”概念的使用有关。

# 5. Northern Rock：OTD模式与存款挤兑

## 5.1 案例背景

**Northern Rock** 是英国快速增长的抵押贷款银行。在2007–2009年金融危机前多年，其资产即贷款每年增长20%。它使用**发起并分销模式（Originate-to-Distribute, OTD Model）**，即发放贷款后重新包装并出售，资金来源是短期市场借款。

由于位于英国，Northern Rock可接触美国、欧洲和亚洲的全球多元融资市场。

## 5.2 失败原因

2007年初违约率上升，冲击全球证券化抵押贷款市场。金融危机加剧后，Bank of England内部决定为Northern Rock提供支持；当支持消息泄露后，持有Northern Rock活期存款的人迅速提款。

当时英国法律只保证存款2,000英镑以内，并对最高33,000英镑部分提供90%保证。随后英国监管者表示将对Northern Rock所有存款提供100%保证，挤兑才逐渐平息，但该银行最终转为公有。

## 5.3 教训

Northern Rock说明：依赖短期批发融资支持长期贷款资产，在证券化市场压力和公众信心丧失时，会迅速变成流动性危机。

# 6. 流动性危机的共同教训

金融危机后，美联储开始要求大型美国银行进行定期压力测试，推动银行使用资产负债管理（Asset/Liability Management, ALM）或衍生品如利率互换来缓释流动性风险。

ALM存在两个关键权衡：

1. **流动性风险与利率风险权衡**  
    若银行使用期限短于资产的融资来源来降低利率风险，则流动性风险更高。
    
2. **成本与风险缓释权衡**  
    降低流动性风险意味着负债期限要更接近贷款资产期限，但长期融资来源更昂贵。
    

银行也可建立紧急流动性渠道，例如指定贷款人的信用额度，但若未使用仍需支付费用，因此银行必须平衡备用融资带来的风险降低与成本。

# 7. 对冲策略：静态对冲与动态对冲

## 7.1 静态对冲（Static Hedging Strategy）

**静态对冲（Static Hedging Strategy）** 是购买与被对冲头寸尽量匹配的对冲工具。匹配关系只在部署对冲时衡量，之后不调整，关注的是策略到期时结果。

优点：

- 监督要求较低。
- 交易成本较低。

缺点：

- 不会根据基础风险敞口变化而调整。

## 7.2 动态对冲（Dynamic Hedging Strategy）

**动态对冲（Dynamic Hedging Strategy）** 是部署对冲工具后，频繁再平衡对冲头寸，例如每日、每月或每季度调整。

优点：

- 更能匹配不断变化的市场力量和风险敞口。

缺点：

- 需要持续监督。
- 交易成本更高。
- 存在额外模型风险。

动态对冲中常见做法是**滚动对冲（Rolling Hedge）**，例如用一个月期货合约对冲长期风险敞口，并在每个月合约到期前滚动到下一批短期期货。

# 8. Metallgesellschaft / MGRM：滚动对冲导致流动性风险

## 8.1 案例背景

**Metallgesellschaft Refining and Marketing (MGRM)** 是一家国际集团的美国子公司。1993年，它向客户提供固定价格、固定数量的取暖油和汽油长期合约，期限为5年或10年。固定价格通常比未来12个月期货平均价格高3至5美元/桶。

客户若在现货价格高于合同固定价格时退出合约，MGRM需要向客户支付期货价格与合约价格差额的一半；后续合约中，客户可用更高固定价格换取全部差额。

## 8.2 风险暴露与对冲方式

这些客户合约使MGRM暴露于能源价格上涨风险，相当于公司持有长期远期合约空头。MGRM使用短期期货多头进行**stack-and-roll hedging strategy**，即用短期期货对冲长期敞口。

公司使用短期期货是因为远期市场替代品不可得，长期期货合约流动性很低。MGRM用一个月期货合约对冲长期敞口，每月卖出即将到期合约并买入新一批一个月期货。

## 8.3 Backwardation与Contango

若期货曲线处于**现货溢价（Backwardation）**，即现货价格高于未来交割价格，则滚动对冲可能盈利，因为现有期货合约可相对下一月对冲成本以盈利卖出。

若市场处于**期货溢价（Contango）**，即未来交割价格高于现货价格，则滚动对冲会产生损失。

## 8.4 失败原因

1993年底，现货油价大幅下跌，市场进入contango，导致MGRM出现13亿美元保证金追缴。由于客户合约是长期性质，公司没有现金覆盖这笔账面损失，母公司要求MGRM关闭所有对冲头寸，将未实现损失变为已实现损失。

教材指出，MGRM可能本可以存活，如果它提前获得短期融资来源来满足保证金追缴需求。该案例说明，滚动对冲策略可能创造流动性风险，并最终成为失败原因。

## 8.5 MGRM的三个额外考虑

1. **会计规则影响**  
    根据德国会计规则并遵循IFRS，MGRM需要报告期货合约损失，但不能显示客户合约对应的每日收益，巨额损失可能影响信用评级和业务能力。
    
2. **交易流动性风险（Trading Liquidity Risk）**  
    当公司需要平仓大量期货头寸时，可能需要数日才能降低市场冲击；MGRM案例中需要10天。
    
3. **税务处理差异**  
    对冲工具的税务处理因司法辖区不同而异，税后结果可能显著不同于税前结果。
    

# 9. 模型风险（Model Risk）

## 9.1 模型风险来源

复杂金融产品使用数学模型估值。模型风险可能包括：

1. 使用错误模型。
2. 模型设定错误。
3. 使用不完整数据。
4. 使用错误估计量。
5. 做出错误假设。

# 10. Niederhoffer：错误假设与尾部风险

## 10.1 案例背景

Victor Niederhoffer是一名成功的对冲基金交易员。他认为卖出大量深度价外（Deep Out-of-the-Money, OTM）S&P 500指数看跌期权是一种低风险策略。只要S&P 500单日跌幅小于5%，他就能赚取期权权利金。

基于历史经验和正态分布假设，在1990年代中期，单日5%下跌看似几乎不可能。

## 10.2 失败原因与教训

1997年10月，亚洲危机蔓延到美国市场，S&P 500单日下跌7%，导致5,000万美元保证金追缴，Niederhoffer无法满足要求。经纪商清算所有看跌期权，锁定巨大损失并抹去基金全部权益。

教训：模型假设可能错误，竞争性市场不存在免费午餐。

# 11. Long-Term Capital Management：相关性、杠杆与流动性危机

## 11.1 案例背景

**Long-Term Capital Management (LTCM)** 成立于1994年，核心成员包括前Federal Reserve Board副主席David Mullins、诺贝尔奖得主Robert Merton和Myron Scholes，以及Salomon Brothers债券套利交易团队的经验丰富交易员。倒闭前，LTCM有48亿美元权益和1,250亿美元资产，杠杆率为25:1。

其名义资产价值超过1万亿美元。金融机构常因LTCM核心成员声誉而豁免初始保证金，使其能进一步加杠杆。

## 11.2 策略与失败

LTCM核心策略是相对价值交易（Relative Value Strategy），即买入一种资产并卖出另一种资产，以捕捉两者之间被认为异常的价差。教材举例包括买入英国公司债并卖空英国政府债，以及买入西班牙或意大利主权债并卖空German bunds。

1998年8月，俄罗斯政府意外违约并使货币贬值，引发**flight to quality**，投资者涌入LTCM做空的安全资产，如U.S. Treasuries和German bunds。结果LTCM一个月内资产价值下降超过40%，即48亿美元权益中损失约20亿美元。

Federal Reserve Bank of New York随后协调一组银行向LTCM注资35亿美元，换取90%股权和完全管理控制权。

## 11.3 LTCM的关键教训

1. **监控相关性（Monitor Correlations）**  
    危机中地理分散化失效，债券与股票之间相关性也上升。LTCM模型假设低频高严重性事件在时间上不相关，但实际危机中相关性大幅收敛。
    
2. **关注流动性（Watch Liquidity）**  
    LTCM无法承受短期冲击以等待中期策略回归，原因包括极高杠杆和市场条件恶化。当LTCM需要平仓时，也与模仿者竞争市场流动性。
    
3. **审视损失假设（Consider Loss Assumptions）**  
    LTCM严重依赖10天期VaR，教材指出10天窗口太短，无法承受短期市场冲击；压力测试是更好的方法。
    
4. **加强披露（Enhance Disclosure）**  
    LTCM作为对冲基金不需要披露大量头寸细节，策略实质没有充分披露。
    
5. **无例外要求初始保证金（Require Initial Margin Posting Without Exception）**  
    若贷款方始终要求LTCM在使用杠杆时缴纳初始保证金，LTCM可能不会拥有如此高杠杆，也会有缓冲来应对短期流动性危机。
    

# 12. London Whale：调整模型掩盖风险

## 12.1 案例背景

JPMorgan是美国最大的金融控股公司之一，也是全球最大衍生品交易商之一，尤其活跃于信用衍生品。2012年初，其首席投资办公室（Chief Investment Office, CIO）负责管理3,500亿美元超额活期存款，并用这些资金进行合成信用衍生品大规模押注，最终导致62亿美元交易损失并短暂扰乱全球市场。

JPMorgan伦敦办公室处理合成衍生品交易，其中交易员Bruno Iksil因交易规模巨大，被称为“The Whale”。

## 12.2 失败原因

2011年初，JPMorgan需要降低风险以满足监管资本要求，但CIO领导层没有减少空头押注，而是增加多头押注来“抵消”空头风险。该做法实际上扩大了风险暴露，并追求利润，而不是降低风险。

2012年初CIO策略出现亏损后，管理层不是调整交易经济影响，而是调整合成衍生品估值方法。最佳实践是使用每日交易区间中点作为估值基准，但CIO改用交易区间内任意有利价格，从而使头寸在内部控制中看起来风险更低。

## 12.3 VaR限额违规

2013年美国参议院对Whale Trade的审查称，CIO在2012年1月1日至4月30日期间忽视了330次VaR风险限额违规。违规被系统性忽视，或通过提高风险限额来处理。CIO还修改VaR假设，使计算VaR下降50%。

教训：当风险限额被突破或交易显得不盈利时，风险经理绝不能调整假设或估值模型，让错误决策看起来更好。

# 13. Barings Bank：流氓交易员与错误报告

## 13.1 案例背景

**Barings Bank** 成立于1762年，是全球第二古老的商人银行。1992年，Nick Leeson到新加坡担任本地运营主管，任务是执行客户在新加坡证券交易所的交易。其职责后来扩展到自营套利交易，利用Nikkei期货在日本最大证券交易所和Osaka Securities Exchange之间的定价差异。

## 13.2 失败原因

传统套利应同时做多低估资产并做空高估资产，但Leeson选择只买入一个资产而没有对应空头，变成方向性投机。更严重的是，Leeson还控制自己交易的后台会计，并通过隐藏的对账账户管理报告，该账户没有上报总部。

1994年看似有1.02亿英镑利润，实际却是2亿英镑损失。1995年1月，Leeson报告单月1,000万英镑异常利润，风险管理人员疑虑再次被忽视。最终，损失大到Barings Bank被迫清算，并被ING以1英镑收购。

## 13.3 Barings的教训

- 如果结果好到不像真的，就应被质疑。
- 异常高利润或异常稳定利润需要严格审查。
- 必须有独立验证机制。
- 后台（Back Office）绝不能由前台交易员（Front Office / Traders）控制。
- 交易应根据完整持有期最终结果评估，而不是根据孤立的短期片段评估。

# 14. 金融工程风险（Financial Engineering）

## 14.1 金融工程工具

金融工程（Financial Engineering）的基础工具包括远期（Forwards）、期货（Futures）、互换（Swaps）、期权（Options）和证券化产品（Securitized Products）。风险经理可用这些工具对冲单一风险敞口或一篮子风险敞口。

教材举例：美国共同基金投资日本固定收益工具时，可能同时暴露于日本利率风险和货币风险，可使用远期或互换对冲其中一种风险，也可使用**quanto swap**，即多币种利率互换。

FRM重点：风险经理必须明确对冲策略目标。纯粹对冲用于风险缓释，但一些企业将对冲策略用于增强收益，这通常会增加额外风险层次。

# 15. Bankers Trust：复杂杠杆互换与投机化对冲

## 15.1 案例背景

1990年代早期，**Bankers Trust (BT)** 被认为是熟练的风险管理者。Proctor & Gamble（P&G）找BT帮助管理美元和德国马克利率风险。P&G最终选择使用复杂杠杆互换押注利率下降。

P&G一度在一系列互换中使用20:1杠杆，由BT向P&G支付固定利率，P&G向BT支付浮动利率。

## 15.2 失败原因与教训

1994年，美联储加息250个基点，P&G遭受重大损失，因为它选择了投机而不只是对冲风险敞口，杠杆放大了损失。P&G最终起诉BT并获得7,800万美元判决。类似事件造成声誉损害，BT最终被Deutsche Bank收购。

教训：不要把风险缓释对冲变成投机策略，尤其在使用高杠杆复杂衍生品时。

# 16. Orange County：不了解工具导致破产

## 16.1 案例背景

回购协议（Repurchase Agreements, Repos）是短期借款机制，一方卖出证券并承诺未来以稍高价格买回，价格差相当于利息。1990年代初，California的Orange County拥有77亿美元资产，财务主管Robert Citron通过短期repo额外借入129亿美元。

Citron将资产和借入资金投资于复杂的反向浮动利率票据（Inverse Floating-Rate Notes），这类证券的票息在利率上升时下降。

## 16.2 失败原因与教训

Orange County完全依赖贷款人愿意滚动repo合约。初期策略收益比同业高2%，但1994年美联储加息250个基点后，复杂衍生品票息下降，投资者不再滚动repo，最终导致破产。Citron后来承认自己并不真正理解反向浮动利率票据的风险敞口。

教训：不要投资自己不理解的产品，否则损失可能是终局性的。

# 17. Sachsen Landesbank：表外实体与次贷资产

德国Landesbanks是公有银行，专门向地区中小企业贷款。2000年代中期，一些Landesbanks看到美国次级贷款市场利润潜力，并在其他国家建立表外实体持有证券化次级抵押贷款。**Sachsen Landesbank** 在Dublin, Ireland设立表外实体并购买大量次贷资产。

2007–2009年金融危机爆发后，Sachsen遭受重大损失，被迫出售给另一家未追逐证券化次贷产品利润的德国银行。

教训：表外结构不能消除真实风险；如果不理解或低估证券化资产风险，损失仍会回到机构本身。

# 18. 声誉风险：Volkswagen案例

## 18.1 声誉风险（Reputation Risk）

公司声誉是公众对公司公平性、伦理行为承诺以及对利益相关者待遇的认知。**声誉风险（Reputation Risk）** 是由于负面公众认知导致负面经营结果的可能性。三个重要利益相关者是客户、监管者和股东。

在互联网时代，事实和谣言都可以迅速传播；即使是不实谣言，也可能在数小时内暂时摧毁公司声誉。最严重的情况是传闻为真。

## 18.2 Volkswagen案例

2015年9月，美国Environmental Protection Agency（EPA）宣布Volkswagen（VW）在环境责任上存在不道德行为。VW通过车辆软件，使汽车只在监管测试时控制排放；车辆在日常使用中则停止控制排放。2009年至2015年，该软件影响全球超过1,000万辆汽车。

该丑闻迅速造成声誉损害，VW股价在事件发展过程中下跌三分之一。Volkswagen还面临数十亿美元潜在罚款和销售下降，因为消费者可能转向其他品牌。德国政府也曾担忧“Made in Germany”的声誉受到影响。

# 19. 公司治理：Enron案例

## 19.1 公司治理（Corporate Governance）

**公司治理（Corporate Governance）** 是指导企业如何运营的一套政策和程序。治理检查点包括透明度、问责、对高级领导和风险管理政策的监督、多元化、董事会独立性，以及具备公司所需技能的董事会成员。

## 19.2 Enron背景

**Enron** 于1985年由InterNorth和Houston Natural Gas高杠杆合并形成。后续放松监管使Enron成为天然气经纪商，买入天然气并以预定价格卖给客户。为管理天然气价格风险，Enron创建能源衍生品市场。Fortune杂志在1995年至2000年将Enron评为“America’s Most Innovative Company”。2000年底，Enron有20,000名员工和近1,010亿美元记录收入。

2001年12月，Enron成为当时美国历史上最大破产案，原因是大规模公司治理失败和代理风险。

## 19.3 Enron治理失败原因

1. **代理风险（Agency Risk）**  
    高级管理层将自身利益置于其他利益相关者之上，追求短期盈利以最大化个人财富，并牺牲整个公司。
    
2. **董事会监督不足（Lack of Board Oversight）**  
    Ken Lay同时担任董事会主席和CEO，相当于监督自己。董事会还允许CFO经营自己的私募股权公司作为副业，而该“业务”实际用于将更多资金导向CFO个人控制。
    
3. **会计欺诈（Accounting Fraud）**  
    Enron使用特殊目的实体（Special Purpose Vehicles, SPVs）和其他创意会计手法进行欺诈，制造虚假销售，并将亏损转移到SPV以隐藏公众审查。
    
4. **收入确认问题（Revenue Recognition Practices）**  
    Enron会在实体资产生产完成时立即确认收入，而不管是否实际销售；实际出售发生时的亏损则埋入表外SPV。
    
5. **审计失败（Auditor Failure）**  
    Arthur Andersen是Enron唯一审计师。国会调查后来发现Arthur Andersen参与该欺诈，并撤销其执业资格。Enron欺诈最终导致两家公司失败。
    

## 19.4 Enron的教训

Enron失败推动了2002年Sarbanes-Oxley Act（SOX）的出台。SOX要求关键公司高管对财务报表可靠性承担责任，并设立Public Company Accounting Oversight Board（PCAOB），促进更高标准的公司治理。

FRM重点：

- CEO和董事会主席应分离，以增强问责。
- 独立且有伦理的审计师是缓释代理风险的重要防线。
- 激进会计技术应被投资者高度审查。

# 20. 网络风险：SWIFT案例

## 20.1 网络风险（Cyber Risk）

**网络风险（Cyber Risk）** 是内部技术基础设施被攻破导致金融损失或声誉损失的风险。它涉及黑客进入系统并盗取资金、信息或身份数据，例如社会保障号码、电子邮件和密码。企业每年花费数十亿美元保护技术系统完整性，一些企业也购买网络保险以将损失风险转移给第三方。

## 20.2 SWIFT案例

**Society for Worldwide Interbank Financial Telecommunication (SWIFT)** 是金融机构之间电子转账的全球领导者，由来自Belgium、United States、England、European Central Bank、Japan等地的中央银行家组成的联盟监督。SWIFT每天处理2,500万至3,500万笔交易。

2016年2月，黑客进入SWIFT系统，从Bangladesh Bank盗取8,100万美元；这些资金存放在New York Federal Reserve Bank。黑客使用员工凭证发起一系列向亚洲不同地点转账的请求。

黑客目标是盗取10亿美元，但Bank of New York发现一份请求中存在拼写错误，将“foundations”写成“fandations”，于是停止所有转账。8,100万美元转到Philippines一家银行后，黑客使用恶意软件删除转账记录并关闭交易确认通知。Bangladesh Bank重启系统后收到大量转账通知，网络盗窃才暴露。资金最终未被追回，因为已从Philippines银行转入一系列赌场并被迅速提取。

## 20.3 SWIFT案例教训

SWIFT案例说明，网络攻击可以造成巨大金融损失，并影响机构声誉。网络风险不是银行专属风险，任何进行数字交易的企业都可能面临该风险；企业可通过内部资源、外部IT顾问或网络保险来处理该风险。

# 21. 本章重要公式与概念

本章没有复杂计算公式，但以下概念必须熟记：

- 利率风险（Interest Rate Risk）：由利率水平波动导致损失，可用久期衡量敏感性。
- 流动性风险（Liquidity Risk）：短期资金无法满足需求导致损失。
- 静态对冲（Static Hedge）：部署后不调整，成本低但不适应变化。
- 动态对冲（Dynamic Hedge）：频繁再平衡，适应性强但成本高，并可能产生流动性风险和模型风险。
- 模型风险（Model Risk）：错误模型、错误假设、不完整数据或错误估计造成风险。
- 流氓交易员风险（Rogue Trader Risk）：交易员绕过控制、隐藏损失并造成机构毁灭性损失。
- 声誉风险（Reputation Risk）：公众负面认知导致负面运营结果。
- 网络风险（Cyber Risk）：技术基础设施被攻破导致财务或声誉损失。

# 22. FRM考试高频易错点

1. **S&L危机核心是未管理的利率风险**：短期资金成本上升，而长期贷款收益固定或较低。
2. **Lehman、Continental Illinois和Northern Rock共同问题是用短期融资支持长期资产**，一旦短期融资消失，就会出现流动性危机。
3. **静态对冲不是“完全匹配”**，而是“尽量接近匹配”；动态对冲才适合快速变化市场，但需要监督并带来交易成本和模型风险。
4. **MGRM的根本问题是现金流/流动性问题**，不是单纯对冲理论错误；若能满足中间保证金需求，长期对冲现金流可能平衡。
5. **Niederhoffer说明尾部风险和错误正态假设会摧毁看似低风险策略。**
6. **LTCM说明短期VaR不足以管理极端市场冲击**，必须关注相关性上升、流动性和压力测试。
7. **London Whale说明不能通过修改模型假设或估值方法掩盖风险限额突破。**
8. **Barings说明前台交易与后台会计必须分离**，异常优秀或稳定的业绩应被怀疑。
9. **Bankers Trust和Orange County说明复杂衍生品若被误用，会从风险管理工具变成投机工具。**
10. **Enron说明公司治理失败、代理风险、SPV滥用和审计失败可导致企业崩溃。**
11. **SWIFT说明网络风险可造成重大金融损失，且资金转移速度极快。**

# 23. 一句话记忆

本章核心是：**金融灾难往往不是单一风险造成，而是利率风险、流动性风险、模型风险、杠杆、治理失败、错误激励、复杂产品误用和信息系统漏洞共同放大；FRM考试要能把每个案例与对应风险类型、失败原因和可预防控制措施一一对应。**

# Reading 10 2007–2009年全球金融危机剖析（Anatomy of the Great Financial Crisis of 2007–2009）

# 0. 本章考试重点（Exam Focus）

2007–2009年金融危机由多种因素共同造成，包括宽松贷款标准（Relaxed Lending Practices）、容易获得信贷（Easy Access to Credit）、房价泡沫（Inflated Housing Prices），以及高度互联的银行体系和全球金融体系。FRM考试重点是理解危机前的背景、主要成因、金融机构使用短期融资的危险，以及短期融资如何增加系统性风险（Systemic Risk）。此外，还要能评价政府和中央银行在危机中的应对措施。

# 1. 金融危机背景与总体概述

## 1.1 低利率与房地产泡沫

在2007–2009年金融危机之前，美国利率长期处于历史低位。低资金成本使个人更容易借钱购买房地产，从而推动房价快速且不可持续地上涨。

低利率环境也使投资者寻求更高收益的资产，而次级抵押贷款相关产品因收益较高而受到欢迎。

## 1.2 金融创新与证券化

金融创新，尤其是**证券化（Securitization）**，使抵押贷款可以由贷款人发放、重新包装并出售给寻求更高收益的投资者。这样做降低了贷款发起人自身承担的信用风险，因此发起银行对借款人信用质量的关注下降，贷款标准也随之放松。

FRM重点：证券化本身不是一定有害，但它改变了贷款发起人的激励，使贷款质量控制变弱。

## 1.3 从次贷问题扩散为全球危机

危机最初是美国次级抵押贷款问题，但它很快扩散到其他资产类别和地理区域，最终影响全球市场。许多银行，尤其是持有次贷敞口的银行，遭受大额损失和流动性问题。金融机构变得极度谨慎，囤积超额准备金，不愿把资金借给其他缺现金的机构。

各国政府通过降低利率和提供流动性支持进行干预，以鼓励贷款并支撑濒临失败的金融机构。

# 2. 资产负债期限错配与危机爆发

## 2.1 短期负债融资长期资产

危机前，银行越来越多地使用短期负债（Short-Term Liabilities）为长期资产（Long-Term Assets）融资。这造成资产和负债之间的期限错配（Maturity Mismatch），使银行暴露于显著的流动性风险（Liquidity Risk）。

当危机爆发、房价停止上涨时，这些短期负债无法续作（Roll Over），金融机构融资模式迅速失效。

## 2.2 Lehman Brothers破产与信心崩溃

2008年9月危机高峰时，美国大型投资银行Lehman Brothers宣布破产，引发市场信心巨大损失，并冻结银行间拆借市场（Interbank Lending Market）。其他投资银行若未直接倒闭，则被竞争对手收购，或转型为银行控股公司并接受美联储监管。

美国两大抵押贷款支持证券发行机构Fannie Mae和Freddie Mac被国有化；大型金融服务和保险公司American International Group（AIG）获得救助，以防止进一步系统性问题。

# 3. 次级抵押贷款（Subprime Mortgages）

## 3.1 次级抵押贷款定义

**次级抵押贷款（Subprime Mortgage）** 是以住宅房地产作抵押、发放给信用质量较差借款人的贷款。这类借款人可能有拖欠付款记录、高贷款价值比（Loan-to-Value, LTV）或高贷款收入比（Loan-to-Income Ratio）。

## 3.2 典型次贷产品：2-28 ARM

典型次级贷款可以设计为30年期**2-28可调利率抵押贷款（2-28 Adjustable-Rate Mortgage, ARM）**。这种产品前2年提供较低固定**诱惑利率（Teaser Rate）**，之后剩余28年转为更高且可能无法负担的浮动利率。

## 3.3 高风险次贷结构

危机前部分次级贷款具有以下特征：

- **100%贷款价值比（100% Loan-to-Value）**：无需首付，因此借款人没有权益缓冲，贷款人违约损失缓冲也较弱。
- **只付息贷款（Interest-Only Loans）**：贷款期间只支付利息，不减少本金余额。
- **NINJA贷款（NINJA Loans）**：发放给无收入、无工作、无资产的借款人，即 no income, no job, and no assets。
- **说谎贷款（Liar Loans）**：贷款机构几乎不收集证据来确认申请人的就业和收入声明。

## 3.4 房价下跌后的连锁反应

许多次级借款人，尤其是房地产投机者，希望房价继续上涨，这样他们可以在诱惑利率结束时再融资，或卖房获利。房价下跌后，很多借款人进入**负权益（Negative Equity）**状态，即抵押贷款余额超过房屋价值。许多人选择放弃房屋并违约，导致止赎数量增加，房屋供给过剩，并进一步压低房价。

# 4. OTD模式、SIV与证券化

## 4.1 发起并分销模式（Originate-to-Distribute, OTD）

贷款标准下降部分来自**发起并分销模式（Originate-to-Distribute, OTD Model）**。在该模式下，贷款人不再把抵押贷款留在自身资产负债表上，而是通过证券化将贷款转移到破产隔离的**结构化投资工具（Structured Investment Vehicles, SIVs）**中。

FRM重点：OTD模式使贷款人把信用风险转移给投资者，因此贷款人可能放松承销标准。

## 4.2 证券化（Securitization）

**证券化（Securitization）** 是把资产池组合起来，并出售针对这些资产现金流的权利要求。抵押贷款通过证券化形成复杂结构化产品，并出售给投资者。

# 5. CDO在危机中的作用

## 5.1 CDO定义与分层

**担保债务凭证（Collateralized Debt Obligation, CDO）** 是证券化结构的一种，资产池被切分为多个**分层（Tranches）**，例如高级层（Senior）、初级层（Junior）和权益层（Equity）。

现金流和违约损失按照**瀑布结构（Waterfall Structure）**分配：高级层最先获得现金流，但最后吸收损失。

## 5.2 AAA评级掩盖底层风险

高级CDO分层被认为非常安全，并被设计成获得AAA评级，即使其底层抵押贷款可能包含NINJA贷款和说谎贷款。

FRM重点：高级分层的AAA评级并不代表底层资产质量好，而可能只是结构设计和评级模型的结果。

## 5.3 CDO-squared

多个CDO结构中的初级分层经常被重新打包为**CDO-squared**。CDO-squared的现金流由其他CDO分层支持，而不是直接由抵押贷款支持。此类结构非常不透明，正常时期也难以估值，危机时期更难估值。

# 6. 不同机构在危机中的角色

## 6.1 银行与贷款机构

由于OTD模式，银行和贷款机构可以把抵押贷款转移到SIV中并出售给投资者，因此对贷款质量的关注下降，贷款标准被不合理地放宽。

## 6.2 抵押贷款经纪人与贷款人

部分贷款人和抵押贷款经纪人存在问题行为。贷款发起的薪酬结构通常基于数量而非质量，因此贷款是否适合借款人经常被忽视，导致许多次级贷款卖给无法负担贷款的人，或卖给本可获得更便宜产品的人。

## 6.3 评级机构（Rating Agencies）

评级机构给予CDO过高评级，尤其是高级CDO分层。评级常基于优质抵押贷款的历史数据，而没有充分考虑市场投机性越来越强的现实。评级机构也经常高度依赖发行人提供的数据，而没有进行独立检查。

此外，评级机构由发行人付费，存在利益冲突（Conflict of Interest），因此有激励提供有利评级。

# 7. 短期批发融资市场与系统性风险

## 7.1 SIV的短期融资

银行建立SIV后，SIV越来越多地通过短期负债为长期资产如抵押贷款融资。这样做的动机是降低融资成本，但造成资产负债期限错配。主要短期融资工具是：

1. 资产支持商业票据（Asset-Backed Commercial Paper, ABCP）
2. 回购协议（Repurchase Agreements, Repos）

## 7.2 商业票据与ABCP

**商业票据（Commercial Paper）** 是一种短期、无担保融资形式，主要由高质量发行人使用。**资产支持商业票据（Asset-Backed Commercial Paper, ABCP）** 是一种特殊商业票据，由抵押品支持，例如信用卡贷款或抵押贷款。由于商业票据期限短，市场默认发行人到期时可以续作债务。

## 7.3 回购协议（Repos）

**回购协议（Repurchase Agreement, Repo）** 是许多金融机构使用的短期融资来源。银行先卖出资产，同时约定未来以更高价格买回资产。买回价与卖出价之间的差额就是融资期间的利息成本，称为**回购利率（Repo Rate）**。

Repo被视为有担保短期借款，因为卖出的资产作为抵押品。如果借款人到期无法付款，贷款人可保留或出售抵押品，而无需经过破产法院。

## 7.4 折扣率（Haircut）

根据抵押品质量，交易开始时会设定**折扣率（Haircut）**来降低信用风险。例如，若价值100美元的资产，贷款人只愿支付90美元，则haircut为10%。

## 7.5 短期融资枯竭

由于持有抵押贷款的SIV主要通过ABCP和repo短期融资，它们高度依赖到期续作。房价和MBS价格下跌后，贷款人开始质疑SIV内部资产质量，不愿继续提供短期贷款。到2007年8月，ABCP和repo市场几乎完全关闭。

赞助SIV的银行也受到影响，因为它们经常向这些实体提供备用信用额度。货币市场基金也受到冲击，因为它们对ABCP有大量敞口。ABCP价格下跌引发机构和大型投资者挤兑货币市场基金，进一步加剧流动性危机。

## 7.6 去杠杆与价格下跌循环

许多对冲基金无法续作债务，被迫出售CDO投资和其他高质量资产以满足保证金追缴。这使次贷问题扩散到更广泛市场，市场参与者开始质疑所有金融机构的可行性。

Lehman违约后，haircuts从危机前的0%上升到2008年9月接近45%。LIBOR-OIS利差从危机前接近0%上升到危机高峰超过3.6%。更高的LIBOR-OIS利差表示更高的感知信用风险，以及银行间市场更不愿意放贷。

更高haircut和无法借款迫使机构通过出售头寸去杠杆，进一步压低价格、侵蚀机构权益，并迫使它们寻求政府或竞争对手帮助，或最终申请破产。

## 7.7 系统性风险（Systemic Risk）

危机事件体现了**系统性风险（Systemic Risk）**：由于资产负债期限错配等脆弱性，整个金融市场可能关闭或失灵。关键教训是，即使银行认为自己资本充足，过度依赖短期融资仍然非常危险，因为危机时期这种融资可能一夜之间消失。

# 8. 中央银行与政府干预

## 8.1 中央银行应对

为了防止进一步系统性问题，美联储和其他中央银行通过提供流动性支持和降低利率进行干预。美联储采取的措施包括：

- 提供由高质量抵押品担保的长期贷款。
- 允许投资银行和证券公司通过贴现窗口（Discount Window）直接向美联储借款；危机前投资银行不能使用该渠道。
- 针对高质量但缺乏流动性的资产提供流动性。
- 提供资金购买资产支持商业票据。
- 收购Fannie Mae和Freddie Mac发行的资产。

这些行动导致全球中央银行资产负债表大幅扩张。

## 8.2 美国具体干预工具

美国危机期间实施的具体政府干预包括：

- **Term Auction Facility, TAF**：向存款机构提供资金。
- **Primary Dealer Credit Facility, PDCF**：美联储通过repo向一级交易商放贷。
- 2008年9月对Fannie Mae和Freddie Mac进行政府救助。
- **Troubled Asset Relief Program, TARP**：从2008年10月开始从金融机构购买有毒资产（Toxic Assets）。

# 9. FRM考试必背概念

## 9.1 关键英文词汇

- 全球金融危机（Global Financial Crisis）
- 次级抵押贷款（Subprime Mortgage）
- 诱惑利率（Teaser Rate）
- 可调利率抵押贷款（Adjustable-Rate Mortgage, ARM）
- NINJA贷款（NINJA Loans）
- 说谎贷款（Liar Loans）
- 发起并分销模式（Originate-to-Distribute, OTD Model）
- 结构化投资工具（Structured Investment Vehicle, SIV）
- 证券化（Securitization）
- 担保债务凭证（Collateralized Debt Obligation, CDO）
- CDO-squared
- 分层（Tranches）
- 瀑布结构（Waterfall Structure）
- 资产支持商业票据（Asset-Backed Commercial Paper, ABCP）
- 回购协议（Repurchase Agreements, Repos）
- 折扣率（Haircut）
- LIBOR-OIS利差（LIBOR-OIS Spread）
- 系统性风险（Systemic Risk）
- 贴现窗口（Discount Window）
- Term Auction Facility, TAF
- Primary Dealer Credit Facility, PDCF
- Troubled Asset Relief Program, TARP

## 9.2 高频易错点

1. **OTD模式会降低贷款发起人保留信用风险的程度，因此可能导致贷款标准放松。**
2. **CDO高级分层获得AAA评级，并不说明底层抵押贷款质量高。** 评级机构可能过度依赖历史数据和发行人提供的数据，并存在利益冲突。
3. **ABCP和repo的问题不是“商业票据是否有担保”本身，而是短期融资支持长期资产造成融资流动性风险。**
4. **Repo虽然有抵押，但仍会在危机中出现haircut大幅上升和融资中断。**
5. **短期融资枯竭会迫使机构去杠杆卖资产，进一步压低资产价格，形成恶性循环。**
6. **危机不是单纯由次贷造成，而是次贷、证券化、评级失真、短期融资依赖和金融系统互联共同放大。**
7. **中央银行危机应对包括降低利率、提供流动性支持、扩展贴现窗口、购买或支持问题资产。**
8. **贴现窗口危机前已对商业银行开放；危机中新增的是允许投资银行和证券公司直接通过贴现窗口向美联储借款。**

# 10. 一句话记忆

本章核心是：**2007–2009年金融危机是低利率、房价泡沫、次贷扩张、OTD激励错配、CDO复杂证券化、评级失真和短期融资依赖共同作用的结果；当房价下跌、短期融资无法续作、Lehman破产引发信心崩溃时，局部次贷问题迅速演变为全球系统性危机。**

# Reading 11 GARP职业行为准则（GARP Code of Conduct）

# 0. 本章考试重点（Exam Focus）

本章介绍**GARP职业行为准则（GARP Code of Conduct）**，它规定了风险管理职业中的伦理行为原则。FRM考生需要掌握所有GARP会员责任，以及违反准则可能导致的处分。考试题通常不会只问定义，而是通过复杂情境判断是否违反伦理标准。

# 1. GARP Code of Conduct概述

**GARP Code of Conduct** 是一套支持金融风险管理实践的关键原则，适用于**Financial Risk Manager (FRM)** 项目以及由 **Global Association of Risk Professionals (GARP)** 管理的其他认证项目。所有GARP会员，包括FRM考生，都应遵守准则；违反准则可能导致暂停会员资格等后果。

GARP会员还应理解，高伦理行为不仅限于准则中明确列出的内容。遇到准则未具体说明的情形时，会员仍应以符合伦理的方式行动，并在与风险管理职业相关的所有情境中保持审慎，以维护风险管理领域和从业者的诚信。

# 2. GARP准则的两大部分

GARP Code of Conduct强调两个领域：

1. **原则（Principles）**
2. **专业标准（Professional Standards）**

## 2.1 Principles部分

Principles包括：

- 专业诚信与伦理行为（Professional Integrity and Ethical Conduct）
- 利益冲突（Conflicts of Interest）
- 保密（Confidentiality）

## 2.2 Professional Standards部分

Professional Standards包括：

- 基本责任（Fundamental Responsibilities）
- 遵守风险管理公认实践（Adherence to Generally Accepted Practices in Risk Management）

# 3. Professional Integrity and Ethical Conduct

## 专业诚信与伦理行为

GARP会员应在与雇主、现有或潜在客户、公众以及金融服务行业其他从业者的所有往来中，以专业、伦理和诚信方式行事。

会员应在提供风险服务时运用合理判断，并保持思想和行动方向上的独立性。会员不得提供、索取或接受任何可能被合理预期会损害自己或他人独立性与客观性的礼物、利益、报酬或其他对价。

会员还必须采取合理预防措施，确保其服务不被用于不当、欺诈或非法目的；不得故意歪曲与分析、建议、行动或其他专业活动相关的细节。

会员不得从事涉及不诚实或欺骗的专业行为，也不得从事任何会负面影响其诚信、品格、可信赖性、专业能力或风险管理职业声誉的行为。

会员不得从事任何损害GARP、FRM称号，或损害FRM考试及GARP其他认证考试完整性和有效性的行为。会员还应注意不同文化对伦理行为和习俗的差异，避免任何根据当地习俗可能显得不道德的行为；若标准冲突或重叠，应始终适用最高标准。

# 4. Conflict of Interest

## 利益冲突

GARP会员应在所有情境中公平行事，并向所有受影响方充分披露任何实际或潜在利益冲突。

会员还应充分、公平披露所有可能被合理预期会损害独立性和客观性，或干扰其对雇主、客户及潜在客户职责履行的事项。

# 5. Confidentiality

## 保密

GARP会员不得将保密信息用于不当目的；除非事先获得同意，否则应对其工作、雇主或客户的信息保持保密。

会员不得利用保密信息为个人谋取利益。

FRM考试易错点：如果客户涉及非法活动，保密义务可以被打破，但信息只能提供给适当主管机关，而不是任意告诉第三方。

# 6. Fundamental Responsibilities

## 基本责任

GARP会员应遵守适用于其专业活动的所有法律、规则和法规，包括GARP Code；不得故意参与或协助违反这些法律、规则或法规。

会员拥有伦理责任，不能将这些责任外包或委托给他人。

会员应理解雇主或客户的需求和复杂性，并提供适当、合适的风险管理服务和建议。会员还应谨慎避免夸大结果或结论的准确性或确定性，并清楚披露其在风险评估、行业实践以及适用法律法规方面具体知识和专业能力的限制。

# 7. Best Practices

## 最佳实践 / 公认风险管理实践

GARP会员应勤勉执行所有服务，并以独立于利益相关方的方式完成工作。会员应以最高专业客观性收集、分析和分发风险信息。

会员应熟悉当前公认风险管理实践，并清楚说明任何偏离这些实践的情况。

会员应确保沟通内容包含事实数据，且不包含虚假信息。会员在呈现分析和建议时，应区分事实（Fact）与意见（Opinion）。

# 8. 违反GARP Code的后果

所有GARP会员都应遵守GARP Code of Conduct以及与风险管理职业相关的当地法律法规。如果Code与某些法律冲突，则法律法规优先。

违反GARP Code可能导致：

- 暂时暂停GARP会员资格（Temporary Suspension）
- 永久取消GARP会员资格（Permanent Removal from GARP Membership）
- 撤销使用FRM称号的权利（Revocation of the Right to Use the FRM Designation）

处分会在GARP进行正式调查后作出。

# 9. FRM考试高频情境判断

## 9.1 正当交易不一定需要逐笔提前披露

如果基金客户已经了解基金一般投资策略，基金经理执行符合该策略的具体交易，通常不必在每笔交易前逐项通知客户。教材案例中，Lorraine Quigley购买Craeger Industrial Products股票并做空同股票看跌期权，被视为利用股票与期权之间套利机会，而非操纵证券价格，也未因未提前披露每笔交易而违反准则。

## 9.2 礼物和招待可能损害独立性

即使礼物已向雇主披露，若该礼物可能被合理预期会影响独立性或客观性，仍不得接受。教材案例中，Jack Schleifer拒绝ChemCo提供的豪华酒店和私人飞机，但接受纽约正式上流晚宴门票仍违反准则，因为该门票可能昂贵或难得，并可能影响其未来研究。

## 9.3 不得暗示无法合理保证的业绩

会员不得故意歪曲分析、建议或专业活动细节。教材案例中，Beth Bixby声称一个40至60只中盘股组合可跟踪S&P 500并获得2%至4%溢价，被认为不合理，因为S&P 500是大盘指数，且无法保证持续获得2%至4%溢价。

## 9.4 必须区分事实与意见

历史增长等已经发生的事项可作为事实陈述；对未来增长和盈利能力的预期属于意见。教材案例中，Gail Stefano说明南美公用事业行业历史订单快速增长，并表达其公司对未来增长和盈利的预期，同时披露政治与汇率不稳定等主要风险，因此未违反准则。

## 9.5 客户违法信息不能随意透露给第三方

会员必须维护客户信息保密。若客户信息涉及非法活动，信息只能提供给适当主管机关。教材案例中，Beth Anderson将客户Reuben Carlyle被IRS调查的信息告诉当地投资银行，使其撤回Carlyle Concrete上市提案，违反了保密义务。

# 10. FRM考试必背重点

## 10.1 关键英文词汇

- GARP职业行为准则（GARP Code of Conduct）
- 专业诚信与伦理行为（Professional Integrity and Ethical Conduct）
- 独立性与客观性（Independence and Objectivity）
- 利益冲突（Conflict of Interest）
- 保密（Confidentiality）
- 基本责任（Fundamental Responsibilities）
- 公认风险管理实践（Generally Accepted Practices in Risk Management）
- 事实与意见（Fact and Opinion）
- 暂停会员资格（Temporary Suspension）
- 永久取消会员资格（Permanent Removal from Membership）
- 撤销FRM称号使用权（Revocation of the Right to Use the FRM Designation）

## 10.2 高频易错点

1. **伦理责任不能外包或委托。** 会员不能说“这是别人负责的”来逃避伦理责任。
2. **披露礼物不一定足够。** 如果礼物可能损害独立性和客观性，即使披露也不能接受。
3. **不能夸大模型或策略的确定性。** 尤其不能暗示保证收益、稳定溢价或高度确定结果。
4. **必须区分事实和意见。** 历史数据是事实，未来预测是意见。
5. **保密信息不能为个人利益或不当目的使用，也不能随意给第三方。** 涉及违法时，只能向适当机关披露。
6. **若法律法规与GARP Code冲突，法律法规优先。**
7. **违反Code可能导致暂停、永久取消会员资格或撤销FRM称号使用权。**
8. **遇到准则未明确规定的情形，仍应以伦理和审慎方式行动。**

# 11. 一句话记忆

本章核心是：**GARP Code要求FRM从业者始终保持诚信、独立、客观、公平、保密和专业审慎；考试常通过礼物、利益冲突、业绩承诺、事实与意见区分、客户保密和违法披露等情境考查是否违反准则。**