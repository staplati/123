# 12  
  
本篇阅读材料系统地介绍了概率论的基础知识，这对于量化分析至关重要。以下是全面且易于理解的总结：  
  
当一个结果未知时（例如抛硬币的结果或明天的气温），我们称其为随机变量 (random variable)。我们可以用可能结果的概率 (probability) 来描述一个随机变量。概率代表了一个结果发生的可能性，其数值必须介于0（表示绝对不会发生）和1（表示必定会发生）之间。  
  
无条件概率 (unconditional probability)，也称为边际概率 (marginal probability)，是指一个事件在没有任何前提条件下的绝对发生概率，例如某个城市的最高气温在特定范围内的概率，通常记作 $P(A)$。  
  
条件概率 (conditional probability) 是指在另一个事件已经发生的前提下，某事件发生的概率。例如，在已知今天是阴天的情况下，气温在特定范围内的概率。它记作 $P(A|B)$，表示在事件 B 发生的前提下事件 A 发生的概率。  
  
联合概率 (joint probability) 是指两个事件同时发生的概率，记作 $P(AB)$ 或 $P(A \text{ and } B)$。  
  
事件 (event) 是指随机变量的一个单一结果或几个结果的组合（例如掷骰子得到3，或者得到一个偶数）。  
  
事件空间 (event space) 是指一个随机变量所有可能结果及其组合的集合（包括什么都不发生的空集）。在数学符号中，$P(A \cup B)$ 通常用来表示事件 A 或 B 发生的概率，$P(A \cap B)$ 用来表示事件 A 和 B 同时发生的概率。  
  
独立事件 (independent events) 是指一个事件的结果完全不会影响另一个事件发生的概率。如果事件 A 和 B 是相互独立的，则必须满足以下两个等式：  
1. $P(A) \times P(B) = P(AB)$：A 和 B 同时发生的联合概率等于它们各自无条件概率的乘积。  
2. $P(A|B) = P(A)$：在已知 B 发生的情况下，A 发生的条件概率依然等于 A 的无条件概率。  
  
互斥事件 (mutually exclusive events) 是指两个事件绝不可能同时发生。例如，掷一次骰子不可能同时得到偶数和数字3。对于互斥事件，它们的联合概率 $P(AB) = 0$。  
  
在计算两个事件中至少有一个发生的概率时，通用公式为 $P(A \text{ or } B) = P(A) + P(B) - P(AB)$。我们需要减去联合概率以避免重复计算。但如果事件是互斥的，因为 $P(AB) = 0$，公式可以直接简化为 $P(A) + P(B)$。  
  
条件独立事件 (conditionally independent events) 探讨的是在引入第三个条件事件 C 后，A 和 B 是否独立。如果满足 $P(A|C) \times P(B|C) = P(AB|C)$，则它们是条件独立的。值得注意的是，两个事件可能本身是不独立的，但在给定某个条件后变得独立；反之，本身独立的事件也可能在给定某条件后变得不独立。  
  
离散概率函数 (discrete probability function) 适用于结果数量有限的随机变量。该函数给出了每一种可能结果的具体概率。一个有效的概率函数，其所有可能结果的概率总和必须等于 100%（即 1）。  
  
在计算条件概率和联合概率时，我们可以将公式进行转换：$P(AB) = P(A|B) \times P(B)$。这也意味着我们可以通过联合概率来求条件概率：$P(A|B) = \frac{P(AB)}{P(B)}$。  
  
当我们有一组互斥且详尽的条件事件 $B_i$ 时（详尽意味着这些条件涵盖了所有可能的情况），我们可以使用全概率法则 (total probability rule) 来计算事件 A 的无条件概率。公式为所有联合概率的加总：  
$P(A) = P(A|B_1)P(B_1) + P(A|B_2)P(B_2) + \dots + P(A|B_n)P(B_n)$。  
  
贝叶斯法则 (Bayes' rule) 是一个非常重要的工具，它允许我们利用一个事件刚刚发生的新信息，来“更新”我们对另一个事件发生的无条件概率的估计。  
  
根据概率论的基本规则，我们知道 $P(A|B) \times P(B) = P(AB)$ 且 $P(B|A) \times P(A) = P(AB)$。通过重新排列这些项，我们可以得出贝叶斯法则的核心公式：  
$P(A|B) = \frac{P(B|A) \times P(A)}{P(B)}$。  
  
<img width="50%" src="https://github.com/user-attachments/assets/995f1826-284d-4f4d-a32c-061686024a26" />
  
在实际应用贝叶斯法则（如上图中的例子）时，公式的分子实际上就是两个事件的联合概率，而分母通常是利用全概率法则计算出来的更新目标事件的无条件概率总和。  
  
# 13  
  
这份文档系统全面地回顾了定量分析中关于随机变量的核心原则。以下是使用通俗易懂的简体中文进行的详细总结。  
  
**第一部分：随机变量与概率函数 (Random Variables and Probability Functions)**  
  
离散随机变量 (Discrete random variable) 是指只能取可数个可能结果的变量。例如抛硬币的结果（0或1）属于伯努利随机变量。连续随机变量 (Continuous random variable) 包含不可数个可能结果，例如降雨量。因为连续随机变量有无数个结果，所以取任何单一具体值的概率都为零，我们只能测量它在某个区间内发生的概率。  
  
概率质量函数 (Probability mass function, PMF) 记作 $f(x) = P(X = x)$，它给出了离散随机变量结果等于某个特定值 $x$ 的概率。对于一个有效的概率质量函数，所有可能结果的概率总和必须等于 100%。  
  
累积分布函数 (Cumulative distribution function, CDF) 记作 $F(x) = P(X \le x)$，它给出了随机变量取值小于或等于 $x$ 的概率。对于离散型变量，累积分布函数就是将所有小于或等于 $x$ 的可能结果的概率相加。累积分布函数对所有实数都有定义。  
  
期望值 (Expected value) 是随机变量所有可能结果的加权平均值，其中权重就是这些结果发生的概率。其数学表达式为 $E(X) = \sum P(x_i)x_i$。期望值代表了我们在统计学上对随机变量结果的“最佳猜测”。如果所有结果发生的概率相等，期望值就是所有结果的简单平均数。期望值具有两个重要性质：一是对于常数 $c$，$E(cX) = cE(X)$；二是对于任意随机变量 $X$ 和 $Y$，$E(X + Y) = E(X) + E(Y)$。  
  
**第二部分：均值、方差、偏度与峰度 (Mean, Variance, Skewness, and Kurtosis)**  
  
统计分布的四个常用总体矩 (Population moments) 分别是均值、方差、偏度和峰度。第一矩是均值 (Mean)，记作 $\mu$ 或 $E(X)$，即前面提到的期望值。后三个被称为中心矩 (Central moments)，因为它们测量的是随机变量偏离其均值 ($X - \mu$) 的程度，这排除了均值位置的影响，从而反映分布的形状。  
  
方差 (Variance) 是第二中心矩，记作 $\sigma^2$。其公式为 $\sigma^2 = E[(X - \mu)^2]$。对方差开平方即可得到标准差 (Standard deviation)，记作 $\sigma$。方差和标准差都用于衡量变量围绕均值分散的程度（即离散程度）。标准差通常更常用，因为它的单位与随机变量的单位相同。  
  
偏度 (Skewness) 是标准化的第三中心矩，用于衡量分布的对称性。其公式为 $E[(X - \mu)^3] / \sigma^3$。偏度为 0 表示分布是完美对称的。  
  
<img width="50%" src="https://github.com/user-attachments/assets/821e3db3-4bc0-47fa-8bb5-a5518a63ac61" />
  
峰度 (Kurtosis) 是标准化的第四中心矩，用于衡量分布的形状。其公式为 $E[(X - \mu)^4] / \sigma^4$。峰度衡量的是分布尾部的总概率相对于其他部分概率的比例。峰度越高，极端结果（尾部）发生的概率就越大，这种分布常被称为肥尾分布 (Fat-tailed distributions)。  
  
<img width="50%" src="https://github.com/user-attachments/assets/0b33199c-993c-419d-a590-77f5fc479d79" />
  
**第三部分：概率密度函数、分位数与线性变换 (Probability Density Functions, Quantiles, and Linear Transformations)**  
  
概率密度函数 (Probability density function, PDF) 用于描述连续随机变量的分布。由于连续变量取单一值的概率为零，概率密度函数允许我们通过计算曲线下方的面积（即积分）来求出结果落在某两个值（区间）之间的概率。  
  
分位数函数 (Quantile function) 记作 $Q(\alpha)$，它是累积分布函数 (CDF) 的逆函数。它告诉我们，有百分之几的结果会小于或等于某个特定的返回值。  
  
中位数 (Median) 是 $50\%$ 处的分位数，即 $Q(50\%)$。平均有 $50\%$ 的结果在它之下，$50\%$ 在它之上。对于对称分布，均值等于中位数；对于正（右）偏态，中位数小于均值；对于负（左）偏态，中位数大于均值。  
  
四分距 (Interquartile range, IQR) 是包含中间 $50\%$ 概率分布的结果区间，计算公式为 $Q(75\%) - Q(25\%)$。与标准差类似，四分距也是衡量随机变量变异性（离散程度）的指标。  
  
线性变换 (Linear transformations) 的形式为 $Y = a + bX$，其中 $a$ 改变分布的位置，$b$ 缩放分布的数值。当随机变量 $X$ 进行线性变换后：  
  
变换后的均值：$E(Y) = a + bE(X)$，位置和缩放比例都会受到影响。  
  
变换后的方差：$\sigma_Y^2 = b^2\sigma_X^2$。常数 $a$ 不影响变异程度，标准差则变为 $\sigma_Y = |b|\sigma_X$。  
  
变换后的偏度：如果 $b > 0$（递增变换），偏度不受影响；如果 $b < 0$（递减变换），偏度的绝对值不变，但正负号改变（即 $Y$ 的偏度 $= -X$ 的偏度）。  
  
变换后的峰度：线性变换完全不影响峰度，即 $Y$ 的峰度 $= X$ 的峰度。  
  
# 14  
  
本文是对常见单变量随机变量（Common Univariate Random Variables）相关概念的系统性、全面性总结。以下内容使用通俗易懂的简体中文编写，并保留了关键的英文术语以便于对照学习。  
  
**第一部分：均匀分布、伯努利分布、二项分布与泊松分布**  
  
连续均匀分布（Continuous Uniform Distribution）定义在下限 a 和上限 b 之间。在这个区间外，事件发生的概率为零。因为是连续分布，所以出现任意一个精确单点的概率都为零。连续均匀分布的概率密度函数表示为结果落在一个特定区间内的概率：$P(x_1 \le X \le x_2) = (x_2 - x_1) / (b - a)$。它的均值为 $(a+b)/2$，方差为 $(b-a)^2/12$。  
  
<img width="50%" src="https://github.com/user-attachments/assets/cdce5d3f-d2b5-4940-941f-197d4ca29f96" />
  
伯努利分布（Bernoulli Distribution）的随机变量只有两个可能的结果：成功（用1表示）或失败（用0表示）。假设成功的概率为 p，那么失败的概率就是 1-p。伯努利变量的均值为 p，方差为 $p(1-p)$。当 p 接近 0 或 1 时，方差很小；当 p=0.5 时，方差达到最大。这种分布常用于评估二元结果，例如公司是否会违约。  
  
二项分布（Binomial Distribution）可以定义为在 n 次独立的伯努利试验中，成功发生次数的概率分布。每次试验成功的概率 p 是恒定的。计算在 n 次试验中恰好成功 x 次的概率公式为：$P(X = x) = \frac{n!}{(n - x)!x!} p^x (1 - p)^{n - x}$。二项分布的期望值（即均值）为 np，方差为 $np(1-p)$。二项分布在投资领域应用广泛，例如用于构建资产估值的二叉树模型。  
  
泊松分布（Poisson Distribution）是一种离散概率分布，通常用于衡量在一定时间或空间单位内事件发生的次数（例如每小时拨打911的电话数量）。参数拉姆达（$\lambda$）代表单位时间内预期或平均发生的成功次数。在已知预期次数为 $\lambda$ 的情况下，实际发生 x 次的概率公式为：$P(X = x) = \frac{\lambda^x e^{-\lambda}}{x!}$。泊松分布的一个有趣且重要的特征是，它的均值和方差都等于参数 $\lambda$。  
  
**第二部分：正态分布与对数正态分布**  
  
正态分布（Normal Distribution）在金融和投资组合理论中占据核心地位。它完全由其均值（$\mu$）和方差（$\sigma^2$）来描述。正态分布具有以下关键属性：偏度为0（意味着它围绕均值完全对称），均值、中位数和众数相等；峰度为3（衡量分布尾部肥瘦的指标）；独立正态随机变量的线性组合依然是正态分布。此外，越远离均值的结果发生概率越小，但永远不会归零（尾部无限延伸）。  
  
<img width="50%" src="https://github.com/user-attachments/assets/76d28c83-2e38-415c-a57e-3498637e6ebd" />
  
置信区间（Confidence Interval）是指我们预期实际结果在一定概率下会落入的一个数值范围。对于正态分布，68%的结果落在均值加减一个标准差的范围内，大约95%的结果落在均值加减两个标准差的范围内。在实际操作中，最常用的三个置信区间是：90%置信区间对应的乘数是 1.65，95%置信区间对应 1.96，99%置信区间对应 2.58。  
  
<img width="50%" src="https://github.com/user-attachments/assets/ff955350-4be8-4856-b7a6-031ab47896c2" />
  
标准正态分布（Standard Normal Distribution，也称 z-distribution）是经过标准化处理的正态分布，其均值为0，标准差为1。通过标准化公式 $z = (x - \mu) / \sigma$，可以将任何正态随机变量转化为 z 值。z 值代表某个观测值距离均值有多少个标准差。  
  
我们可以使用 z 表（z-table）来查找标准正态分布的累积概率。z 表提供的是随机变量小于某个给定 z 值的概率。由于正态分布是对称的，通过查表和简单的减法即可计算出大于某值或落在某个区间内的概率。  
  
<img width="50%" src="https://github.com/user-attachments/assets/fc6c07e7-4b4b-4498-b843-9c2fb35a62ed" />
  
对数正态分布（Lognormal Distribution）由函数 $e^x$ 生成，其中 x 是正态分布的。对数正态分布有明显的右偏（Skewed to the right）特征，并且其下限被截断在零点（即变量值不能为负）。这一特性使其非常适合用于对资产价格进行建模，因为资产的实际价格不可能低于零，这意味着投资的最大亏损幅度是 -100%。  
  
<img width="50%" src="https://github.com/user-attachments/assets/70ebe72c-9880-4980-a342-85b373878b71" />
  
**第三部分：学生t分布、卡方分布与F分布及其他分布**  
  
学生t分布（Student’s t-Distribution）形状类似于正态分布，但拥有“更肥的尾部”（Fatter tails），这意味着极端结果发生的概率相对更高。当我们要基于未知总体方差的小样本（样本量 n < 30）构建置信区间时，t 分布是最合适的选择。它是对称的，由单一参数——自由度（Degrees of freedom, df）来定义，对于样本均值而言，自由度等于 $n - 1$。随着自由度（样本量）的增加，t 分布的形状会越来越接近标准正态分布。  
  
<img width="50%" src="https://github.com/user-attachments/assets/993f4b65-384a-46a6-bdd8-ca978b8dc636" />
  
卡方分布（Chi-Squared Distribution，记作 $\chi^2$）常用于对总是为正数的随机变量或总体方差进行假设检验。卡方分布是不对称的，且下限为零。随着自由度的增加，它的形状逐渐趋向于正态分布。其检验统计量的计算依赖于样本方差与假设总体方差的比例。  
  
<img width="50%" src="https://github.com/user-attachments/assets/7a516735-f6fa-4fab-b32b-cee6641db95e" />

  
F分布（F-Distribution）用于检验两个独立正态总体方差的相等性。F 检验的统计量是两个样本方差的比值：$F = s_1^2 / s_2^2$。F 分布呈现右偏态，并在左侧截断于零点。它的形状由两个单独的自由度决定：分子自由度（$df_1$）和分母自由度（$df_2$）。当分母的观测值数量趋于无限大时，F 分布和卡方分布存在数学上的转换关系。  
  
<img width="50%" src="https://github.com/user-attachments/assets/6d289154-ac45-493f-8f2e-dc8cfbd40447" />
  
指数分布（Exponential Distribution）常用于模拟等待时间，例如员工服务一位客户需要的时间或公司发生违约的时间。其概率密度函数包含尺度参数（$\beta$），它大于零并且是速率参数（$\lambda$）的倒数。速率参数衡量事件发生的速度，在评估违约时称为风险率（Hazard rate）。指数分布用于评估到达某次事件的时间，而其相关的泊松分布用于评估在特定期间内事件发生的总次数。  
  
贝塔分布（Beta Distribution）可用于模拟违约概率和回收率，常被应用于如 CreditMetrics 等信用风险模型中。贝塔分布的取值范围严格限制在0到1之间，根据其形状参数的取值不同，该分布既可以是对称的，也可以是偏斜的。  
  
**第四部分：混合分布**  
  
混合分布（Mixture Distributions）通过将多种现有分布结合在一起，创造出独特的概率密度函数。如果当前手头的数据无法完美契合任何单一的预设分布，构建一个混合分布往往能更好地解释实际数据。  
  
例如，假设某只股票在 75% 的时间里波动率较低，而在 25% 的时间里波动率较高。为了模拟这种情况，我们可以将两个拥有相同均值但风险水平（方差）不同的正态分布结合起来。以 75% 的概率抽取低波动率分布的随机结果，以 25% 的概率抽取高波动率分布的结果，经过多次重复，就能产生一个反映双重波动率特征的新分布。  
  
混合分布结合了参数分布（作为输入的组件分布）和非参数权重（基于历史数据的概率占比）。引入的组件分布越多，混合分布就越贴近历史真实数据。这种方法可以人为改变分布的偏度和峰度，甚至创造出拥有多个众数（例如双峰分布）的模型。对于风险管理者来说，混合分布非常有价值，因为它能更好地将低频、高损（Low-frequency, high-severity）的极端风险事件纳入模型之中。  
  
# 15  
  
本文是对“多元随机变量 (Multivariate Random Variables)”章节内容的系统性、全面且通俗易懂的中文总结。  
  
多元随机变量是包含多个随机变量的向量。本章重点研究双变量随机变量 (Bivariate Random Variables)，即包含两个成分的多元分布特例，主要探讨它们之间的依赖关系、分布特征以及统计矩 (Moments) 的计算。  
  
### 边缘分布与条件分布 (Marginal and Conditional Distributions)  
  
概率质量函数 (Probability Mass Function, PMF) 用于描述双变量随机变量同时取特定数值的概率。其公式表示为：$f_{X_1,X_2}(x_1,x_2) = P(X_1=x_1, X_2=x_2)$。  
  
概率矩阵 (Probability Matrix) 常用来直观展示离散型双变量的 PMF。概率矩阵必须满足三个条件：所有概率值必须大于等于0且小于等于1；矩阵中所有可能结果的概率总和等于1。  
  
<img width="50%" src="https://github.com/user-attachments/assets/5e22a705-9262-4ce7-b56d-8c8022835a19" />
  
边缘分布 (Marginal Distribution) 定义了双变量中单一随机变量（即单变量随机变量）的分布。在概率矩阵中，通过对整行求和可以得到列变量的边缘分布，对整列求和可以得到行变量的边缘分布。  
  
<img width="50%" src="https://github.com/user-attachments/assets/bdd2bfe8-cbdf-44ff-b8b5-1902efae295d" />
  
条件分布 (Conditional Distribution) 表示在已知一个变量取特定值的前提下，另一个变量的概率分布。计算方法是将联合概率（矩阵内部的数值）除以该前提条件发生的边缘概率。公式为：$f_{X_1|X_2}(x_1|X_2=x_2) = \frac{f_{X_1,X_2}(x_1,x_2)}{f_{X_2}(x_2)}$。  
  
### 双变量随机分布的矩 (Moments of Bivariate Random Distributions)  
  
双变量随机函数的期望 (Expectation of a Bivariate Random Function) 是双变量离散随机变量的一阶矩。它是基于联合概率的加权平均值，公式为：$E[g(x_1, x_2)] = \sum_{x_1} \sum_{x_2} g(x_1, x_2) f_{X_1,X_2}(x_1, x_2)$。  
  
协方差 (Covariance) 是二阶矩，衡量两个变量共同波动的程度或依赖关系。它是两个变量偏离各自期望值的乘积的期望值。公式为：$Cov[X_1,X_2] = E[(X_1 - E[X_1])(X_2 - E[X_2])] = E[X_1 X_2] - E[X_1]E[X_2]$。  
  
双变量随机变量有两个方差和一个协方差，它们通常被表达为一个 2x2 的协方差矩阵 (Covariance Matrix)，主对角线上是方差，另一条对角线上是协方差。  
  
相关系数 (Correlation) 旨在解决协方差因数据尺度 (Scale) 不同而难以解释的问题。通过将协方差除以两个变量标准差的乘积，可以得到标准化的相关系数。公式为：$Corr(X_1,X_2) = \frac{Cov[X_1,X_2]}{\sqrt{Var[X_1]}\sqrt{Var[X_2]}} = \frac{\sigma_{12}}{\sigma_1 \sigma_2}$。  
  
相关系数的取值范围在 -1 到 +1 之间，用于衡量线性关系的强度。相关系数为0代表没有线性关系，但这并不必然意味着两个变量完全独立。  
  
### 双变量随机变量矩的特性 (Behavior of Moments)  
  
对双变量随机变量进行线性变换 (Linear Transformations) 会产生四个重要影响。假设存在线性关系 $X_2 = a + bX_1$：  
  
第一，乘数 $b$ 的符号决定了相关系数。如果 $b>0$，相关系数为 1；如果 $b=0$，相关系数为 0；如果 $b<0$，相关系数为 -1。  
  
第二，常数项 $a$（位置移动）对方差没有影响。乘数 $b$ 会使方差按 $b^2$ 的比例缩放，即方差变为 $b^2 Var[X_1]$。  
  
第三，在计算协方差时，常数项加减不影响结果，但乘数会产生乘法效应。例如：$Cov[a+bX_1, c+dX_2] = bdCov[X_1,X_2]$。  
  
第四，这会延伸出协偏度 (Coskewness) 和协峰度 (Cokurtosis) 的概念，用于衡量一个变量的一次方受另一个变量高次方影响的方向。  
  
在计算双变量随机变量加权和的方差 (Variance of Weighted Sum) 时，协方差是一个关键组成部分。在两资产投资组合 (Two-asset portfolio) 中，如果权重分别为 $w$ 和 $(1-w)$，组合方差公式为：$\sigma_P^2 = w^2 \sigma_1^2 + (1-w)^2 \sigma_2^2 + 2w(1-w)\sigma_{12}$。  
  
在此基础上，可以通过公式找到使风险最小的最优权重，即最小方差投资组合 (Minimum variance portfolio)。  
  
<img width="50%" src="https://github.com/user-attachments/assets/2da13ecb-d031-4ec7-8b7d-cfd78b4a16c0" />
  
条件期望 (Conditional Expectations) 在投资组合风险管理中非常有用。它是基于特定事件（如特定财报公布）发生的情况下，利用条件分布算出的概率加权平均回报。  
  
### 独立同分布随机变量 (Independent and Identically Distributed, i.i.d.)  
  
独立同分布随机变量 (i.i.d. random variables) 是指由同一个单一分布生成的一组变量。它们具有以下核心特征：与其他所有变量互相独立；全部来自同一个单变量分布；所有变量具有相同的统计矩（如均值 $\mu$ 和方差 $\sigma^2$）。  
  
$n$ 个独立同分布随机变量之和的期望值等于 $n\mu$。  
  
$n$ 个独立同分布随机变量之和的方差等于 $n\sigma^2$。这意味着和的方差是随着 $n$ 线性增长的（因为变量之间独立，协方差均为0）。  
  
多个独立同分布随机变量平均值的方差，会随着 $n$ 的增加而减小。这一特性在估计未知参数时意义重大，它意味着样本量 $n$ 越大，预期的平均值就会越接近真实的未知均值 $\mu$。  
  
# 16  
  
本文总结了关于样本矩（Sample Moments）的核心量化分析原理，主要探讨如何使用样本数据来估计独立同分布（i.i.d.）随机变量的真实总体矩，并介绍了大数定律、中心极限定理以及分位数等概念。  
  
**一、 估计均值、方差与标准差 (Estimating Mean, Variance, and Standard Deviation)**  
  
样本均值（Sample mean）是通过将样本中所有观测值相加并除以样本量 $n$ 来计算的，用于推断未知的总体均值（Population mean）。算术均值是唯一一个使所有观测值与均值偏差之和为零的集中趋势指标。其公式表示为：$\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} X_i$。  
  
样本方差（Sample variance）通过计算观测值与均值偏差的平方和得出。为了得到无偏估计，标准做法是除以 $n-1$ 而不是 $n$。方差的平方根即为标准差（Standard deviation），两者均用于衡量随机变量偏离均值的离散程度。无偏样本方差的公式为：$s^2 = \frac{1}{n-1} \sum_{i=1}^{n} (X_i - \hat{\mu})^2$。  
  
总体矩与样本矩（Population and Sample Moments）有着本质区别。总体均值 $\mu$ 是基于所有可能的观测值计算的，通常是未知且唯一的。而样本均值 $\hat{\mu}$ 则是基于已知数据集的估计值。在使用大样本时，由于包含了更多信息，样本均值会更接近真实的总体均值。需要注意的是，极端值（异常值）会对算术均值产生不成比例的重大影响。  
  
**二、 估计量与点估计 (Estimators and Point Estimates)**  
  
点估计（Point estimates）是用于估计总体参数的单个样本值，而用于计算点估计的公式被称为估计量（Estimator）。例如，样本均值公式就是一个将观察到的数据转化为对真实总体均值估计的估计量。  
  
有偏估计量（Biased estimators）的偏差衡量的是估计量的期望值与其试图估计的真实总体参数之间的差异。公式为：$Bias(\hat{\theta}) = E[\hat{\theta}] - \theta$。由于样本均值的期望值等于总体均值，因此样本均值是无偏估计量。相反，如果计算样本方差时分母使用 $n$，会系统性地低估总体方差，从而产生已知偏差；将其修正为除以 $n-1$ 后，即可成为无偏估计量（Unbiased estimator）。  
  
最佳线性无偏估计量（Best Linear Unbiased Estimator, BLUE）是指在所有线性无偏估计量中具有最小方差的估计量。当数据是独立同分布时，样本均值被认为是最佳线性无偏估计量。  
  
**三、 分布矩的估计 (Estimating Moments of the Distribution)**  
  
大数定律（Law of Large Numbers, LLN）说明了估计量的一致性（Consistency）。一致估计量（Consistent estimator）有两个基本属性：随着样本量增加，有限样本偏差趋近于零；估计量的方差也趋近于零。这意味着在样本量极大时，样本均值和方差会非常接近真实的总体均值和方差。  
  
中心极限定理（Central Limit Theorem, CLT）指出，对于均值为 $\mu$、方差为 $\sigma^2$（方差有限）的总体，当样本量 $n$ 足够大（通常 $n \ge 30$）时，样本均值的抽样分布会趋近于均值为 $\mu$、方差为 $\sigma^2/n$ 的正态分布。该定理极其重要，因为它不需要对总体数据的原始分布做出任何假设，仅凭大样本即可使用正态分布进行假设检验和构建置信区间。  
  
偏度（Skewness）是分布的标准化三阶中心矩，用于衡量分布围绕其均值的不对称程度。非对称分布可能是由于异常值的出现引起的。正偏态分布（右偏分布）在右侧尾部有大量异常值，导致众数小于中位数，中位数小于均值（均值被异常值拉高）。负偏态分布（左偏分布）在左侧尾部有较多异常值，导致均值小于中位数，中位数小于众数。  
  
<img width="50%" src="https://github.com/user-attachments/assets/52e3f079-1fd3-4759-a7d4-efba292528e9" />
  
峰度（Kurtosis）是分布的标准化四阶中心矩，用于衡量数据分布尾部的厚薄程度。正态分布的峰度等于3。如果分布的峰度大于3，则称为尖峰厚尾分布（Leptokurtic distribution）。大多数软件报告的是超额峰度（Excess kurtosis = Kurtosis - 3）。在风险管理中，峰度至关重要，因为实际证券收益率往往具有厚尾特征；如果仅假设其为正态分布，将无法充分考虑发生极端负面损失的可能性。  
  
<img width="50%" src="https://github.com/user-attachments/assets/20329265-165e-4b7e-8988-1190c2088d9f" />
  
**四、 中位数与分位数估计 (Median and Quantile Estimates)**  
  
中位数（Median）是数据集排序后的第50个百分位数。当极值（异常值）存在时，算术均值容易受到影响，而中位数不受极差数值的影响，因此在这种情况下中位数是比均值更好的集中趋势衡量指标。  
  
四分位数（Quartiles）是最常用的分位数，包括第25和第75百分位数。四分位距（Interquartile range, IQR）是第75分位数与第25分位数之差，这是一种与标准差类似但用于衡量偏离中位数的离散程度的指标。分位数的优势在于其单位与样本数据相同，且对异常值具有极强的鲁棒性（Robustness）。  
  
**五、 两个随机变量的均值、协方差与相关性 (Mean, Covariance, and Correlation of Two Random Variables)**  
  
两个随机变量的均值估计方法与单个变量相同。协方差（Covariance）是一种统计指标，衡量两个随机变量一起变动的程度，捕捉两者之间的线性关系。正协方差表示同向变动，负协方差表示反向变动。样本协方差的计算公式分母为 $n-1$。  
  
由于协方差的绝对数值对数据的尺度极为敏感且不易解释，通常需要计算相关系数（Correlation coefficient），将协方差标准化。相关系数的取值范围始终介于 -1 和 +1 之间：$Corr(X,Y) = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}$。  
  
**六、 共偏度与共峰度 (Coskewness and Cokurtosis)**  
  
如同单变量的偏度和峰度，一对随机变量也可以计算交叉的三阶和四阶矩，即共偏度（Coskewness）和共峰度（Cokurtosis）。  
  
共偏度衡量的是当一个变量出现大数值时，另一个变量发生大幅定向变动的可能性。在双变量正态分布样本中，由于数据对称且呈正态分布，共偏度始终为零。  
  
共峰度有三种衡量指标，其值取决于相关性。对称情况下的共峰度 $k(X,X,Y,Y)$ 衡量一个序列的绝对幅度对另一个序列绝对幅度的敏感性。当两个随机变量的相关性从 -1 变化到 +1 时，对称共峰度在 1 到 3 之间变化，且在相关性为零时取得最小值 1。非对称情况下的共峰度表现为在 -3 到 +3 之间向上的线性关系。  
  
<img width="50%" src="https://github.com/user-attachments/assets/6751938d-52b3-4182-a814-a83575e3963d" />
  
# 17  
  
这里为您对提供的《假设检验 (Hypothesis Testing)》阅读材料进行系统、全面且易于理解的中文总结：  
  
**第17章阅读：假设检验 (Hypothesis Testing)**  
  
**模块 17.1：假设检验基础 (Hypothesis Testing Basics)**  
  
假设检验 (Hypothesis testing) 是对关于某个总体的陈述或观点进行统计评估的过程。通过相关的样本数据，假设检验程序可以在给定的显著性水平下测试该陈述的有效性。  
  
任何一个假设检验都包含以下六个基本组成部分：  
1. 原假设 (Null hypothesis)：指定一个假设为真的总体参数值。  
2. 备择假设 (Alternative hypothesis)：指定在拒绝原假设时应接受的测试统计量的值的范围。  
3. 检验统计量 (Test statistic)：从样本数据中计算得出的数值。  
4. 检验规模或显著性水平 (Size of the test / Significance level)：指定在原假设为真时却错误地拒绝它的概率。  
5. 临界值 (Critical value)：用于与检验统计量进行比较，以决定是否拒绝原假设的数值。  
6. 决策规则 (Decision rule)：基于检验统计量和临界值的比较，决定是否拒绝原假设的规则。  
  
原假设 (Null Hypothesis, $H_0$) 是研究者试图拒绝的假设。它通常是关于总体参数的一个简单陈述，且必须包含“等于” (equal to) 的条件（如 $\mu = \mu_0$、$\mu \le \mu_0$ 或 $\mu \ge \mu_0$）。  
  
备择假设 (Alternative Hypothesis, $H_A$) 是当有充分证据拒绝原假设时得出的结论。通常这是研究者真正想要证实的假设。因为统计学永远无法绝对证明任何事情，所以通过否定原假设，我们从而推断备择假设是有效的。  
  
检验统计量 (Test statistic) 是通过比较总体参数的点估计值与假设值来计算的。其计算公式如下，即样本统计量与假设值之差，再除以样本统计量的标准误 (Standard error)：  
  
$$ \text{Test Statistic} = \frac{\text{Sample Statistic} - \text{Hypothesized Value}}{\text{Standard Error of the Sample Statistic}} $$  
  
样本均值的标准误 (Standard error of the sample statistic) 计算方式取决于总体标准差 $\sigma$ 是否已知。如果已知，则为 $\sigma_{\bar{x}} = \frac{\sigma}{\sqrt{n}}$；如果未知，则用样本标准差 $s$ 替代，即 $s_{\bar{x}} = \frac{s}{\sqrt{n}}$。  
  
假设检验可以是单尾检验 (One-tailed test) 或双尾检验 (Two-tailed test)。双尾检验用于测试参数是否“不同于”某个特定值（允许偏大或偏小），而单尾检验用于测试参数是否“大于”或“小于”某个特定值。  
  
<img width="50%" src="https://github.com/user-attachments/assets/529a5dde-86c0-4c16-a2c9-8184c955a02e" />
  
<img width="50%" src="https://github.com/user-attachments/assets/f6b3e808-b004-45c4-9c37-69ec3df0099c" />
  
在根据假设检验得出推论时，可能会犯两类错误：  
第一类错误 (Type I error)：当原假设实际上为真时，却错误地拒绝了它。  
第二类错误 (Type II error)：当原假设实际上为假时，却没有拒绝它。  
  
显著性水平 (Significance level, $\alpha$) 就是犯第一类错误的概率。而检验效力 (Power of a test) 是在原假设为假时正确拒绝它的概率，其值等于 $1 - P(\text{Type II error})$。为了降低第二类错误的概率并提高检验效力，唯一的方法是增加样本量 (Sample size)。  
  
<img width="50%" src="https://github.com/user-attachments/assets/1c04bc8b-de78-48ff-8fa8-202273e97892" />
  
置信区间 (Confidence interval) 是一个值域范围，研究者相信真实的总体参数落在该范围内。假设检验与置信区间通过临界值 (Critical value) 联系在一起。如果样本统计量落在假设值构建的置信区间之外，则会拒绝原假设。  
  
统计显著性 (Statistical significance) 并不一定意味着实际显著性 (Practical significance)。即使通过假设检验发现某种投资策略的收益在统计上显著大于零，在实际操作中，交易成本 (Transactions costs)、税收 (Taxes) 以及潜在的风险 (Risk) 因素也可能使该策略无利可图。  
  
**模块 17.2：假设检验结果 (Hypothesis Testing Results)**  
  
p值 (The p-Value) 是指在假设原假设为真的前提下，获得导致拒绝原假设的检验统计量的概率。它也是能够拒绝原假设的最小显著性水平。如果计算出的 p值小于我们设定的显著性水平，我们就可以拒绝原假设。  
  
<img width="50%" src="https://github.com/user-attachments/assets/d75a0c04-ab2d-4feb-a768-6422b61c1c69" />
  
t检验 (The t-Test) 是一种广泛使用的假设检验方法，采用服从 t分布 (t-distribution) 的检验统计量。当总体方差未知 (Population variance is unknown)，并且样本量很大 ($n \ge 30$) 或者样本量较小但总体近似正态分布时，应当使用 t检验。t统计量 (t-statistic) 的计算公式为：  
  
$$ t_{n-1} = \frac{\bar{x} - \mu_0}{s / \sqrt{n}} $$  
  
z检验 (The z-Test) 则适用于总体服从正态分布且总体方差已知 (Known variance) 的情况。z统计量 (z-statistic) 的计算公式为：  
  
$$ z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}} $$  
  
如果样本量很大 ($n \ge 30$) 且总体方差未知，虽然严格意义上应使用 t分布，但由于大样本下 t分布和 z分布的临界值几乎完全相同，此时也可以使用 z分布进行检验，并将样本标准差 $s$ 代入公式计算。  
  
<img width="50%" src="https://github.com/user-attachments/assets/a293460e-fe72-491c-a25f-32a542d32ce7" />
  
在金融领域，我们经常需要测试两个总体的均值是否相等 (Testing the Equality of Means)，这等同于检验两个均值之差是否为零。假设两个序列相互独立同分布 (i.i.d.)，其检验统计量为：  
  
$$ T = \frac{\bar{X} - \bar{Y}}{\sqrt{\frac{s_X^2 + s_Y^2 - 2Cov(X,Y)}{n}}} $$  
  
多重假设检验 (Multiple Hypothesis Testing) 指的是在同一数据集上测试多个不同的假设。这种做法的问题在于，显著性水平仅针对单一假设检验是准确的。如果不断尝试测试越来越多的策略，发生第一类错误 (Type I error) 的实际概率会越来越大，从而导致结果出现偏差。  
  
# 18  
  
第十八章：线性回归 (Linear regression)  
  
本章主要介绍线性回归的基本原理、普通最小二乘法估计以及相关的假设检验。线性回归旨在用一个或多个自变量来解释一个因变量的变化关系。  
  
模块 18.1：回归分析 (Regression analysis)  
  
回归分析旨在衡量一个变量（称为因变量或被解释变量，dependent or explained variable）的变化如何被另一个或多个变量（称为自变量或解释变量，independent or explanatory variables）的变化所解释。这种关系可以通过估计一个线性方程来捕获。  
  
简单的双变量线性方程通常表示为：  
  
$E(Y) = \alpha + \beta \times X$  
  
要使用线性回归，需要满足三个基本条件。第一，Y和X之间的关系必须是线性的。第二，误差项必须是可加的，即误差项的方差与观测数据无关。第三，所有的X变量都应该是可观测的，如果存在缺失数据，则该模型不适用。  
  
“线性”一词有两个层面的含义。首先，它不仅适用于自变量，也适用于未知的系数（参数）。如果因变量和自变量之间的真实关系是非线性的，分析师可以通过适当的数学转换（例如取对数）将其转化为线性关系，然后再代入线性模型。其次，因变量必须是未知系数的线性函数，这意味着未知系数不能以乘法或指数等非线性形式出现在模型中。  
  
模块 18.2：普通最小二乘法估计 (Ordinary least squares estimation)  
  
普通最小二乘法 (OLS) 是一种估计参数 $\alpha$ 和 $\beta$ 的方法，其核心目标是使样本数据中残差（即误差项，squared residuals）的平方和最小化。  
  
斜率系数 (Slope coefficient) $\beta$ 描述了X每变化一个单位时，Y的预期变化量。其计算公式为：  
  
$\beta = \frac{Cov(X,Y)}{Var(X)}$  
  
截距项 (Intercept term) $\alpha$ 是回归线在 $X = 0$ 时与Y轴的交点。其计算公式为：  
  
$\alpha = \overline{Y} - \beta \overline{X}$  
  
这个公式也表明，回归线必然经过因变量和自变量的样本均值点。在多元回归模型中，斜率系数衡量的是在保持其他自变量不变的情况下，某一个自变量变动一个单位对因变量的影响，因此有时也被称为偏斜率系数 (Partial slope coefficients)。  
  
虚拟变量 (Dummy variables) 用于量化定性变量的影响。当自变量本质上是二元属性（例如“是”或“否”，“一月”或“其他月份”）时，我们可以为其赋值为0或1，将其引入回归模型中进行分析。  
  
回归的决定系数 (Coefficient of determination)，即 $R^2$，用于衡量模型的拟合优度。它代表了因变量的变异中能够被自变量解释的比例。在单变量回归模型中，$R^2$ 等于自变量和因变量之间相关系数的平方，即：  
  
$R^2 = r_{X,Y}^2$  
  
OLS回归依赖于几个核心假设。大部分关键假设都与模型的残差项（误差项）有关：  
  
第一，在给定自变量的条件下，误差项的期望值为零，即 $E(\varepsilon_i | X_i) = 0$。这意味着X不包含关于误差项位置的任何信息。如果这一假设被违反，通常会表现为以下几种偏差：生存偏差或样本选择偏差 (Survivorship or sample selection bias)，即观察结果是事后收集的或取决于特定结果；联立性偏差 (Simultaneity bias)，即X和Y的值是同时决定的；遗漏变量 (Omitted variables)，即模型排除了重要的解释变量，导致回归系数产生偏差；衰减偏差 (Attenuation bias)，即X变量的测量存在误差，导致回归系数被低估。  
  
第二，所有的 (X, Y) 观测值必须是独立同分布的 (independent and identically distributed, i.i.d.)。  
  
第三，X的方差必须严格大于零，否则无法估计斜率 $\beta$。  
  
第四，误差项的方差是恒定的，这被称为同方差性 (Homoskedasticity)。  
  
第五，数据中不太可能观察到巨大的异常值 (Outliers)。OLS估计对异常值非常敏感，巨大的异常值可能会产生误导性的回归结果。  
  
上述假设共同确保了回归估计量是无偏的 (unbiased)，并且呈正态分布，从而允许我们进行假设检验。  
  
OLS估计量的性质受到中心极限定理 (Central limit theorem, CLT) 的支持。由于OLS估计量来自随机样本，它们本身也是随机变量，具有自己的抽样分布 (Sampling distributions)。  
  
一个无偏估计量 (Unbiased estimator) 的期望值等于我们要估计的总体参数。在样本量很大的情况下，抽样分布会趋于正态分布，这意味着该估计量也是一个一致估计量 (Consistent estimator)。一致估计量的准确性会随着样本量的增加而提高。  
  
斜率系数 $\beta$ 的方差会随着误差方差的增加而增加，并随着解释变量方差的增加而减少。这意味着，模型误差越大，系数估计的可靠性越低；而自变量样本的多样性越丰富，系数估计的变异性越低，置信度越高。  
  
模块 18.3：假设检验 (Hypothesis testing)  
  
对回归系数进行假设检验的步骤如下：首先，设定要检验的假设；其次，计算检验统计量；最后，将检验统计量与临界值进行比较，得出拒绝或无法拒绝原假设的结论。  
  
假设前面讨论的OLS回归假设成立，估计的斜率系数 $\beta$ 将呈正态分布，其标准差被称为回归系数的标准误差 (Standard error of the regression coefficient, $S_b$)。  
  
如果我们想检验斜率系数等于某一特定值 $\beta_0$ 的假设，我们使用以下 t-统计量 (t-statistic)：  
  
$t = \frac{\beta - \beta_0}{S_b}$  
  
如果该检验统计量的绝对值超过了临界t值（来自t分布表，自由度为 $n - 2$），我们就会拒绝原假设。  
  
置信区间 (Confidence intervals) 的计算公式为：  
  
置信区间 = $\beta \pm (t_c \times S_b)$  
  
其中 $t_c$ 是给定显著性水平和自由度下的临界t值。如果假设的斜率系数值落在置信区间之外，我们可以拒绝原假设。如果它落在置信区间之内，我们则无法拒绝原假设。  
  
p值 (p-value) 是可以拒绝原假设的最小显著性水平。这是检验回归系数的另一种直接方法：如果p值小于选定的显著性水平，则可以拒绝原假设；如果p值大于显著性水平，则无法拒绝原假设。通常，回归模型的软件输出结果会直接提供用于检验系数是否为0的p值。  
  
# 19  
  
这是一份关于《包含多个解释变量的回归（Regression with Multiple Explanatory Variables）》的系统、全面且易于理解的中文总结：  
  
**1. 多元回归模型简介与核心假设 (Multiple Regression Model and Assumptions)**  
  
在实际应用中，我们通常需要将简单回归模型进行扩展，引入多个解释变量（Explanatory Variables），这就形成了多元回归模型（Multiple Regression Model）。其一般数学表达式为：  
$$Y = \alpha + \beta_1X_1 + \beta_2X_2 + ... + \beta_kX_k + \varepsilon$$  
其中，$Y$ 是因变量，$\alpha$ 是截距，$X$ 是各个独立变量，$\beta$ 是斜率系数，$\varepsilon$ 是误差项。  
  
多元回归模型在继承简单单变量回归假设的基础上，进行了一些修改并增加了一个关键假设：  
  
第一，在给定独立变量的条件下，误差项的期望值为零。  
  
第二，所有关于独立变量 $X$ 和因变量 $Y$ 的观测值都是独立同分布的（i.i.d.）。  
  
第三，$X$ 的方差必须大于零（否则无法估计 $\beta$）。  
  
第四，误差的方差是恒定的，即同方差性（Homoskedasticity）。  
  
第五，数据中没有观察到异常值（Outliers）。  
  
第六（多元回归新增假设），$X$ 变量之间不能完全相关，即它们不能完全线性相关（Not perfectly linearly dependent）。换句话说，模型中的每个 $X$ 变量都必须具有一些不能被其他 $X$ 变量完全解释的变异性。  
  
**2. 解释多元回归系数 (Interpreting Regression Coefficients)**  
  
在多元回归中，斜率系数（Slope Coefficient）的含义是：在保持其他所有独立变量不变（Holding the other independent variables constant）的情况下，某一个独立变量发生一个单位的变化所引起的因变量的变化量。因此，多元回归中的斜率系数有时被称为偏斜率系数（Partial Slope Coefficients）。  
  
多元回归的普通最小二乘法（Ordinary Least Squares, OLS）估算过程采用的是逐步法。简而言之，它通过将个体解释变量与其他解释变量进行回归，利用残差（Residuals）来确保在计算某个变量的斜率系数时，已经剔除了其他独立变量带来的影响。  
  
**3. 线性回归的拟合度衡量 (Measures of Fit in Linear Regression)**  
  
回归的标准误（Standard Error of the Regression, SER）衡量了因变量预测值准确性的不确定度。数据点越靠近回归线（误差越小），这种关系就越强。  
  
回归模型旨在解释因变量 $Y$ 的变异（Variation）。因变量的总变异（Total Sum of Squares, TSS）可以分解为模型可以解释的变异（Explained Sum of Squares, ESS）和无法解释的残差变异（Residual Sum of Squares, RSS）。其数学关系为：  
$$TSS = ESS + RSS$$  
  
<img width="50%" src="https://github.com/user-attachments/assets/a56a4b54-e5c5-486f-b69b-e787ed638851" />
  
决定系数（Coefficient of Determination, $R^2$）是多元回归中衡量拟合优度的重要指标，它代表了回归模型所能解释的变异占总变异的百分比：  
$$R^2 = \frac{ESS}{TSS}$$  
  
然而，在多元回归中，$R^2$ 本身可能不是衡量模型解释能力的可靠指标，主要面临“高估回归（Overestimating the regression）”的问题。因为只要向模型中添加独立的变量，即使这些新变量的边际贡献在统计学上并不显著，$R^2$ 也几乎总是会增加。  
  
为了克服由于增加变量而高估模型解释能力的问题，研究人员通常使用调整后的决定系数（Adjusted $R^2$）。调整后的 $R^2$ 对自变量的数量进行了惩罚，其公式为：  
$$Adjusted R^2 = 1 - \left[ \frac{n - 1}{n - k - 1} \right] \times (1 - R^2)$$  
其中 $n$ 是观察值的数量，$k$ 是独立变量的数量。  
  
关于调整后的 $R^2$ 有几个关键点需要注意：它永远小于或等于 $R^2$；当向模型中添加新变量时，如果新变量对模型的贡献很小，调整后的 $R^2$ 可能会下降；如果原始 $R^2$ 足够低，调整后的 $R^2$ 甚至可能小于零。此外，$R^2$ 不能用于比较具有不同因变量（$Y$）的模型。  
  
**4. 联合假设检验与置信区间 (Joint Hypothesis Tests and Confidence Intervals)**  
  
与单一回归一样，多元回归中系数的绝对大小并不能告诉我们该变量在解释因变量时的重要性。我们必须对估计的斜率系数进行假设检验（Hypothesis Testing），以确定独立变量是否做出了显著贡献。  
  
个体系数的统计显著性检验使用 t检验（t-test）。它主要用来检验零假设（即该系数等于零，说明该变量无显著影响）。t统计量的自由度（Degrees of freedom）为 $n - k - 1$。其计算公式与置信区间（Confidence interval）的构建方法与单一回归类似。  
  
当需要测试涉及多个变量影响的复杂假设时，单变量的 t检验就不再适用，我们需要使用 F检验（F-Test）。  
  
F检验通常用于评估一个完整模型与去除了某些变量的“部分模型”之间的优劣。它可以检验被移除的这些额外变量是否对解释 $Y$ 的变异做出了有意义的贡献。F检验始终是单尾检验。  
  
一种更通用的 F检验用于测试模型中包含的“所有”变量是否共同对解释 $Y$ 的变异没有做出贡献（零假设为所有斜率系数均等于0）。如果计算出的 F统计量大于临界 F值，我们就可以拒绝零假设，得出结论：至少有一个独立变量与零有显著差异。公式如下：  
$$F = \frac{ESS / k}{RSS / (n - k - 1)}$$  
  
# 20  
  
本文是对《回归诊断》（Regression Diagnostics）核心内容的系统性、全面性总结。主要探讨了多元回归模型在基本假设被违反时的表现、检测方法以及修正手段。  
  
### 一、 异方差性 (Heteroskedasticity)  
  
如果样本中所有观测值的残差方差保持不变，该回归被称为同方差的 (Homoskedastic)。反之，如果残差的方差在样本中不一致，则存在异方差性 (Heteroskedasticity)。这通常发生在样本中的某些子样本比其他部分更加分散时。  
  
无条件异方差性 (Unconditional heteroskedasticity) 是指异方差性与自变量的水平无关，它不会随着自变量值的变化而系统性地增减。虽然这违反了方差相等的假设，但通常不会对回归造成重大问题。  
  
条件异方差性 (Conditional heteroskedasticity) 是指与自变量水平相关的异方差性。例如，当自变量的值增大时，残差项的方差也随之增大。条件异方差性会给统计推断带来严重的问题。  
  
<img width="50%" src="https://github.com/user-attachments/assets/70ac97bb-7b12-43cd-8126-116b13695982" />
  
条件异方差性对回归分析 (Regression Analysis) 有以下几个显著影响：标准误差 (Standard errors) 的估计通常变得不可靠；尽管如此，系数估计值 (Coefficient estimates) 仍然是一致且无偏的；由于标准误差不可靠，相关的假设检验 (Hypothesis testing) 也会失去可靠性。  
  
为了检测异方差性，可以通过绘制残差与某个自变量的散点图来观察规律。此外，还可以使用卡方检验 (Chi-squared test) 进行正式计算，步骤如下：  
1. 使用标准的普通最小二乘法 (OLS) 估计回归，得出残差并将其平方。  
2. 将步骤1中的平方残差作为因变量，与原来的解释变量进行新的回归。  
3. 计算步骤2新模型的决定系数 ($R^2$)，并计算卡方检验统计量：$\chi^2 = nR^2$。  
4. 将该卡方统计量与其临界值进行比较，自由度为 $[k \times (k + 3) / 2]$（其中k为自变量数量）。如果计算出的 $\chi^2$ 大于临界值，则拒绝无条件异方差性的原假设（即认为存在条件异方差性）。  
  
如果检测到条件异方差性，说明模型系数未受影响，但标准误差不可靠。此时，在假设检验中不应使用 OLS 程序的标准误差，而应使用修正后的怀特标准误差 (White standard errors，也称稳健标准误差)。  
  
### 二、 多重共线性 (Multicollinearity)  
  
在多元回归中，一个重要的假设是自变量之间不能完全相关。如果自变量之间完全相关，例如一个自变量可以由其他自变量的线性组合完美表示，这被称为完全共线性 (Perfect collinearity)。  
  
多重共线性 (Multicollinearity) 是指在一个多元回归中，两个或多个自变量（或其线性组合）之间高度相关。虽然多重共线性本身并不违反回归的基本假设，但它的存在会严重损害参数估计的可靠性。  
  
多重共线性会导致我们更容易错误地得出某个变量在统计上不显著的结论（即增加发生第二类错误 / Type II error 的概率）。在大多数经济模型中，多重共线性都在一定程度上存在，关键在于它是否对回归结果产生了重大影响。  
  
检测多重共线性的最常见方法是观察以下矛盾现象：t检验 (t-tests) 显示所有的单独系数都不显著异于零，但整个模型的 $R^2$ 很高，且F检验 (F-test) 拒绝了原假设。这意味着变量作为一个整体能很好地解释因变量的变动，但单独看却无法解释。这通常是因为自变量高度相关，其共同特征解释了因变量，但也“抵消”了各自的独立效应。  
  
<img width="50%" src="https://github.com/user-attachments/assets/60b8747b-ae7d-4cd5-a316-36ece4e717b8" />
  
另一种识别多重共线性的方法是计算每个解释变量的方差膨胀因子 (Variance inflation factor, VIF)。具体做法是将某个解释变量 ($X_j$) 作为因变量，与其他自变量进行回归求得 $R_j^2$，然后代入以下公式：  
$$VIF_j = \frac{1}{1 - R_j^2}$$  
如果 VIF > 10（即 $R^2$ > 90%），则认为该变量存在严重的多重共线性问题。  
  
修正多重共线性最常用的方法是剔除一个或多个高度相关的自变量。由于很难精准找出问题的源头变量，通常可以使用逐步回归 (Stepwise regression) 等统计程序，系统性地移除变量，直到多重共线性降至最低。  
  
### 三、 模型设定 (Model Specification)  
  
模型设定是一门艺术，需要对解释因变量行为的底层经济理论有透彻的理解。分析师需要决定在模型中包含或排除哪些因素。  
  
如果在模型中包含了不相关或多余的变量 (Irrelevant/extraneous variables)，通常不会带来严重的挑战，但会降低模型的调整后决定系数 (Adjusted $R^2$)。  
  
然而，如果从 OLS 回归中遗漏了相关因素，则会产生误导性或有偏差的结果。当满足以下两个条件时，就会出现遗漏变量偏差 (Omitted variable bias)：（1）被遗漏的变量与模型中已有的其他自变量相关；（2）被遗漏的变量是因变量的一个决定因素。  
  
遗漏变量偏差无论样本大小如何都会存在，它会使得 OLS 估计量变得不一致。已有变量由于与遗漏变量存在相关性，会部分吸收遗漏变量的影响，从而导致已有变量的系数估计产生偏差。此外，遗漏变量未被吸收的影响会进入误差项并将其放大。  
  
### 四、 偏差与方差的权衡 (Bias-Variance Tradeoff)  
  
选择合适的解释变量是模型设定的最终目标。如果模型包含过多的解释变量（即过度拟合 / Overfit models），虽然在样本内对因变量的解释度很高，但在样本外的表现往往很差。复杂的大模型由于包含了过多的变量，往往具有较低的偏差 (Low bias) 和较高的方差误差 (High variance)。相反，变量较少的简单模型则具有较高的偏差和较低的方差误差。  
  
解决这种偏差-方差权衡的常用方法有两种：  
1. 从一般到特定的模型 (General-to-specific model)：从包含最多变量的模型开始，依次剔除绝对t统计量最小的自变量。  
2. m折交叉验证 (m-fold cross-validation)：将样本分为m份，使用其中(m-1)份（称为训练集 / Training set）来拟合模型，使用剩下的一份（称为验证集 / Validation set）进行样本外验证。通过此方法测试候选模型，找出样本外误差最小的最优模型。  
  
### 五、 残差分析与异常值识别  
  
基本的残差图 (Residual plots) 将残差显示在y轴上，因变量的预测值显示在x轴上。理想情况下，残差的幅度应该很小，且与任何解释变量都无关。也可以使用标准化残差 (Standardized residuals，即残差除以其标准差) 来绘图。任何超过 ±4 个标准差的残差都被认为是有问题的。  
  
线性回归的一个假设是样本数据中没有异常值 (Outliers)，因为异常值的存在会扭曲回归参数的估计。识别异常值的一个常用指标是库克距离 (Cook's distance)，计算公式如下：  
$$D_j = \frac{\sum_{i=1}^n (\hat{y}_i^{(-j)} - \hat{y}_i)^2}{kS^2}$$  
公式中，$\hat{y}_i^{(-j)}$ 是剔除第j个观测值后算出的预测值，$\hat{y}_i$ 是未剔除任何数据的预测值，$k$ 是自变量数量，$S^2$ 是模型的平方残差。当库克距离较大（$D_j > 1$）时，表明被剔除的观测值确实是一个异常值。  
  
### 六、 最佳线性无偏估计量 (The Best Linear Unbiased Estimator)  
  
要使 OLS 产生最佳线性无偏估计量 (BLUE)，必须满足线性回归的底层假设。具体来说：因变量Y和自变量X之间必须是线性关系；残差必须是同方差的、相互独立的，且期望值为零，通常记为 $\varepsilon_i \sim N(i.i.d.)$。如果确认数据中没有异常值且残差期望值为零，我们可以放宽对残差分布必须呈正态分布的要求。  
  
# 21  
  
本文档对“平稳时间序列（Stationary Time Series）”的相关核心原则进行了系统且全面的总结。为了基于过去的值来预测未来的时间序列，了解并应用自回归（AR）、移动平均（MA）以及自回归移动平均（ARMA）模型至关重要。  
  
**一、 协方差平稳（Covariance Stationary）**  
  
时间序列（Time series）是指定期收集的数据序列。通常时间序列具有趋势（随着时间变化的成分）、季节性（在一年中特定时间发生的系统性变化）和周期性（随时间周期发生的变化）。要基于时间序列的过去值来准确预测未来值，其现值与过去值之间的关系必须随时间保持稳定，具备这种特性的时间序列被称为协方差平稳（Covariance stationary）。  
  
要成为协方差平稳，时间序列必须具备以下三个属性：  
1. 均值（Mean）必须随时间保持稳定。  
2. 方差（Variance）必须是有限的，且随时间保持稳定。  
3. 协方差结构（Covariance structure）必须随时间保持稳定。  
  
**二、 自协方差与自相关函数（Autocovariance and Autocorrelation Functions）**  
  
协方差结构是指时间序列在不同滞后期（Lags，用希腊字母 $\tau$ 表示）值之间的协方差。时间序列的当前值与滞后 $\tau$ 期之前的过去值之间的协方差，被称为滞后 $\tau$ 期的自协方差（Autocovariance）。所有 $\tau$ 期的自协方差构成了自协方差函数（Autocovariance function）。如果序列是平稳的，该函数将不随观察时间而变，只取决于滞后期 $\tau$。  
  
为了更好地解释关联强度，我们将每个 $\tau$ 处的自协方差除以时间序列的方差，即可转化为自相关函数（Autocorrelation function, ACF），其值被缩放限制在 -1 到 +1 之间。对于协方差平稳序列，当 $\tau$ 变大时，自相关系数总是趋近于零。  
  
<img width="50%" src="https://github.com/user-attachments/assets/04cd2c0e-e3c2-4163-9e81-63e9b0b5bd8a" />
  
偏自相关函数（Partial autocorrelation function, PACF）则是指在控制了滞后间隔之间的所有值的影响后，特定滞后期的相关性。与自相关系数的逐渐衰减不同，偏自相关系数通常会经历急剧下降（Steep decline），这能帮助我们筛选并确定自回归（AR）模型需要包含的滞后项。  
  
**三、 白噪声（White Noise）**  
  
如果一个时间序列的任何滞后值之间相关性为零，则称该序列为序列不相关（Serially uncorrelated）。其中，如果一个序列不仅序列不相关，且均值为零、方差恒定，则被称为白噪声（White noise）。  
  
如果白噪声过程中的观测值相互独立，则称为独立白噪声（Independent white noise）。如果该独立过程还服从正态分布，则称为正态白噪声或高斯白噪声（Gaussian white noise）。注意：所有正态白噪声都是独立白噪声，但并非所有独立白噪声都是正态分布的。  
  
<img width="50%" src="https://github.com/user-attachments/assets/5caa7bfd-2f5b-4000-b603-8af6e5fa8562" />
  
在预测模型中，白噪声概念非常重要，因为一个优秀模型的预测误差（Forecast errors）应当服从白噪声过程。如果预测误差不服从白噪声，意味着误差本身可以被预测，从而说明原模型不充分，需要修改（例如增加滞后项）。  
  
沃尔德定理（Wold's theorem）指出，协方差平稳过程可以被建模为白噪声过程的无限分布滞后（Infinite distributed lag），即一般线性过程（General linear process），公式如下：  
$$y_t = \epsilon_t + b_1 \epsilon_{t-1} + b_2 \epsilon_{t-2} + ... = \sum_{i=0}^{\infty} b_i \epsilon_{t-i}$$  
（其中 $b$ 是常数变量，$\epsilon_t$ 是白噪声过程）  
  
**四、 自回归模型（Autoregressive Processes - AR）**  
  
自回归（AR）模型是金融领域最广泛应用的时间序列模型之一。一阶自回归过程 AR(1) 将变量对其自身的滞后一期数据进行回归估算，公式为：  
$$y_t = d + \Phi y_{t-1} + \epsilon_t$$  
为了确保 AR 过程具备协方差平稳性，滞后项系数 $\Phi$ 的绝对值必须小于 1（即 $|\Phi| < 1$）。对于包含多个滞后项的 AR(p) 过程，所有系数的总和必须小于 1。  
  
AR(1) 序列具有长期或无条件的均值回归水平（Mean reverting level），计算公式为 $\mu = \frac{d}{1 - \Phi}$。对于 AR(p) 过程，公式扩展为：  
$$\mu = \frac{d}{1 - \Phi_1 - \Phi_2 ... - \Phi_p}$$  
均值回归水平起着吸引子的作用，随着时间的推移，时间序列总是倾向于向其均值移动。  
  
分析师使用尤尔-沃克方程（Yule-Walker equation）来估算自回归的自相关性。其关键意义在于，对于 AR 过程，自相关系数会随着时间递增而呈现几何级数衰减（Decay geometrically to zero）。如果系数 $\Phi$ 是负数，那么它的衰减表现为在正负数之间来回震荡，但绝对值依然逐渐减小。  
  
**五、 移动平均模型（Moving Average Processes - MA）**  
  
概念上，移动平均（MA）过程是将时间序列的当前值与当前及前期未观测到的白噪声误差项（即随机冲击）进行线性回归。所有的 MA 过程天然都是协方差平稳的。一阶移动平均过程 MA(1) 定义为：  
$$y_t = \mu + \theta \epsilon_{t-1} + \epsilon_t$$  
MA 过程的一个关键特征是自相关截断（Autocorrelation cutoff）。对于 MA(1) 过程，超过一期的滞后误差项的相关性将直接等于零，这种突然截断帮助分析师将其与 AR 过程区分开来。此外，MA 模型在实际预测中存在难度，因为其包含不可被直接观测的过去随机冲击误差项。  
  
一般的 MA(q) 过程包含了 $q$ 个滞后期：  
$$y_t = \mu + \epsilon_t + \theta_1\epsilon_{t-1} + ... + \theta_q\epsilon_{t-q}$$  
  
**六、 滞后算子（Lag Operators）**  
  
滞后算子（Lag operator，简写为 $L$）是时间序列建模中常用的数学符号表达。将其应用于时间序列值 $y_t$ 时，会得出其前一期的值 $y_{t-1}$：  
$$y_{t-1} = L y_t$$  
  
滞后算子有六个关键属性：  
1. 它将时间索引向后推移一个时期。  
2. 跨多期应用：$L^m y_t = y_{t-m}$。  
3. 应用于常数时，常数保持不变。  
4. 可将分布式滞后权重写成滞后多项式（Lag polynomial）格式，如 $(1 + 0.7L + 0.4L^2 + 0.2L^3)y$。  
5. 滞后多项式之间可以相乘。  
6. 在系数满足一定条件时，多项式可以被求逆（Inverted）。AR 过程只有在其滞后多项式可逆时，才是协方差平稳的。  
  
**七、 自回归移动平均模型（ARMA Models）**  
  
在现实中，时间序列常常同时受到未观测到的冲击（MA部分）及其自身历史行为（AR部分）的影响。这种更复杂的关系被称为自回归移动平均（ARMA）过程，其一阶 ARMA(1,1) 公式合并了两种模型的核心：  
$$y_t = d + \Phi y_{t-1} + \epsilon_t + \theta \epsilon_{t-1}$$  
为保证 ARMA 过程满足用于预测所需的协方差平稳性条件，它依然必须满足 $|\Phi| < 1$。其自相关性表现与 AR 模型相似，呈现逐渐衰减的特征。通过将其参数扩展，可得到更为通用和极其灵活的 ARMA(p, q) 模型，它包含 $p$ 个 AR 滞后项和 $q$ 个 MA 滞后项。  
  
**八、 模型测试与自相关性检验（Testing Autocorrelations）**  
  
分析师可以通过观察数据的自相关图来选择模型形式：如果自相关图突然截断（Cuts off abruptly），应该考虑使用 MA 过程；如果表现为逐渐衰减（Decay gradually），则应该考虑使用 AR 或 ARMA 过程。  
  
构建模型后，需要利用残差自相关（Residual autocorrelations）来评估模型的拟合优度。如果模型对数据拟合得很好，那么残差中不应该遗留任何规律信息，即所有的残差自相关系数都不应该与零有统计学上的显著差异。  
  
一种判定所有残差自相关系数是否联合等于零的检验方法是计算博克斯-皮尔斯统计量（Box-Pierce (BP) statistic）：  
$$Q_{BP} = T \sum_{i=1}^h r_i^2$$  
  
针对样本量较小（$T \le 100$）的数据集，建议采用容-博克斯统计量（Ljung-Box (LB) statistic），它具有更好的表现：  
$$Q_{LB} = T \sum_{i=1}^h \left(\frac{T+2}{T-i}\right) r_i^2$$  
  
**九、 在 ARMA 模型中对季节性进行建模（Modeling Seasonality）**  
  
当数据序列中出现随着固定周期复现的模式时（例如第四季度的零售额普遍较高），这说明存在季节性（Seasonality）。对于这种现象，可以在 AR 或 ARMA 模型中显式地包含一个与该季节性相对应的滞后项（例如对于季度数据引入第4期滞后项，对应月度数据则引入第12期滞后项）。带有季节性的 ARMA 模型表示为 ARMA (p, q) × (ps, qs)，其中 ps 和 qs 受限于取值 1（真） 或 0（假），用来专门捕捉周期性特征。  
  
# 22  
  
本文是对“非平稳时间序列 (Non-Stationary Time Series)”内容的系统性总结。非平稳性主要有三个来源：时间趋势 (time trends)、季节性 (seasonality) 以及单位根/随机游走 (unit roots/random walks)。  
  
**模块 22.1：时间趋势 (Time Trends)**  
  
非平稳时间序列可能表现出确定性趋势 (deterministic trends)、随机趋势 (stochastic trends) 或两者兼有。确定性趋势包括时间趋势和确定性季节性；随机趋势包括单位根过程。  
  
线性时间趋势 (linear time trend) 是指变量每期倾向于以相同的数量发生变化。  
  
<img width="50%" src="https://github.com/user-attachments/assets/78f4a155-211f-4987-a95e-005fe52cbd96" />
  
线性模型可以简单表示为 $y_t = \delta_0 + \delta_1 t + \varepsilon_t$（其中 $\varepsilon_t$ 是白噪声）。但它在金融和经济学中存在局限性：如果趋势向下，模型最终会产生无意义的负值；如果趋势向上，恒定的增长量意味着随着时间的推移，实际的增长率是在递减的。  
  
非线性时间趋势 (nonlinear time trend) 或多项式时间趋势 (polynomial time trend) 可以解决上述问题，例如二次多项式模型：$y_t = \delta_0 + \delta_1 t + \delta_2 t^2 + \varepsilon_t$。  
  
对数线性模型 (log-linear model) 常用于模拟金融中以恒定增长率 (constant growth rate) 增长的过程。其模型表示为 $\ln(y_t) = \delta_0 + \delta_1 t + \varepsilon_t$。  
  
<img width="50%" src="https://github.com/user-attachments/assets/c3e3a14d-3799-488a-9575-226721ae47a3" />
  
在进行预测时，如果假设误差项是正态分布的白噪声 (normally distributed white noise)，则未来 $h$ 期的 95% 置信区间 (confidence interval) 可以表示为：预测值 $y_{t+h} \pm 1.96 \times$ 回归残差的标准差 (standard deviation of the regression residuals)。  
  
对数据进行建模并去除趋势成分后，会得到一个去趋势时间序列 (detrended time series)。如果该序列是协方差平稳的 (covariance stationary)，我们可以进一步使用自回归 (AR)、移动平均 (MA) 或自回归移动平均 (ARMA) 技术来进行预测。  
  
**模块 22.2：季节性 (Seasonality)**  
  
季节性 (seasonality) 是指在时间序列中每年倾向于重复出现的模式（例如零售业的假日销售激增或夏季汽油销量上升）。如果这种周期短于一年（如一周内的特定日子），通常被称为日历效应 (calendar effects)。  
  
在回归分析中，处理季节性的有效方法是引入季节性虚拟变量 (seasonal dummy variables)。虚拟变量的取值为1或0，代表某个季节的“开启”或“关闭”。  
  
为了避免多重共线性 (multicollinearity)，回归模型中包含的虚拟变量数量必须比数据的频率少一个。例如，对于季度数据，最多只能使用3个虚拟变量；月度数据最多使用11个。未被包含的那个“额外”时期会被归入回归的截距项 (intercept) 中。  
  
另一种处理季节性的方法是季节性差分 (seasonal differencing)，即计算当前水平与一年前同期水平之间的差异。  
  
在对带有季节性的序列进行 $h$ 步向前点预测 (h-step-ahead point forecast) 时，只需确定 $T+h$ 时刻的时间趋势值，并将虚拟变量设置为该时期对应的 0 或 1 即可。  
  
**模块 22.3：单位根 (Unit Roots)**  
  
随机游走 (random walk) 是指某一时期的值等于其上一期的值加上或减去一个随机的“冲击” (shock)。公式表示为 $y_t = y_{t-1} + \varepsilon_t$。如果将其不断向前推导，任何一个观测值都可以看作是初始值加上所有历史冲击的总和。  
  
随机游走的一个关键特性是其方差 (variance) 会随时间增加。因此，随机游走不是协方差平稳的 (covariance stationary)，不能直接使用 AR, MA 或 ARMA 技术建模。随机游走是更广泛的时间序列类别——单位根过程 (unit root processes) 的一个特例。  
  
对含有单位根的时间序列直接建模会面临三个主要挑战：  
1. 包含单位根的序列不会均值复归 (revert to a mean)。  
2. 单位根序列之间通常会表现出伪关系 (spurious relationships)。  
3. 如果使用 ARMA 模型，其估计参数服从迪基-福勒分布 (Dickey-Fuller distribution)，这会降低我们选择正确模型和进行有效预测的能力。  
  
解决这些问题的方法是对单位根时间序列进行一阶差分 (first differences)，即计算相邻两期之间的变化量。一阶差分通常也可以解决时间趋势和季节性问题。如果差分一次后仍不平稳，可以进行二次差分；但如果对已经平稳的序列进行差分，会导致过度差分 (overdifferencing)，从而不必要地增加预测模型的复杂性。  
  
检验时间序列是否包含单位根的最常用方法是迪基-福勒增强检验 (augmented Dickey-Fuller test)。该测试本质上是检验序列的滞后项是否是回归模型中的显著统计因子。  
该检验的零假设 (null hypothesis) 是滞后项系数 $\gamma = 0$（即序列没有预测价值，是一个随机游走/存在单位根）。备择假设 (alternative hypothesis) 是 $\gamma < 0$（即序列是协方差平稳的）。需要注意的是，备择假设必须是小于零，而不能仅仅是不等于零。  
  
# 23  
  
以下是对《测量收益率、波动率和相关性》这一章内容的系统、全面且易于理解的中文总结：  
  
本章的核心在于准确估计波动率以理解潜在的风险暴露。主要内容涵盖了收益率与波动率的计算、金融数据分布的非正态特征检验，以及变量之间相关性与依赖性的度量方法。  
  
**模块 23.1：定义收益率与波动率 (Defining Returns and Volatility)**  
  
投资回报通常有两种表达方式：简单收益率 (Simple Returns) 和连续复利收益率 (Continuously Compounded Returns)，后者也称为对数收益率 (Log Returns)。  
  
简单收益率是在一定时间跨度内的直接回报。如果是跨越多个时期的总回报，可以通过将每个单期的简单收益率相乘来计算。假设资产在 t-1 时刻买入，t 时刻卖出，价格为 P，简单收益率 R 的计算公式为：  
$R_t = \frac{P_t - P_{t-1}}{P_{t-1}}$  
  
连续复利收益率 (Log Returns) 则常用于较短的时间周期。对于多期回报，对数收益率可以直接相加求和，这比简单收益率的连乘更方便计算。计算公式为：  
$r_t = \ln(P_t) - \ln(P_{t-1})$  
  
这两种收益率之间可以互相转换，公式为：$1 + R_t = \exp(r_t)$。在任何情况下，简单收益率总是大于对数收益率。  
  
波动率 (Volatility)，通常用符号 $\sigma$ 表示，是收益率的标准差。资产的方差 (Variance) 则是标准差的平方，即 $\sigma^2$。  
  
在将短期波动率转化为长期波动率时（例如年化），需要乘以时间的平方根。假设一年有252个交易日，年化波动率的计算公式为：  
$\sigma_{annual} = \sqrt{252} \times \sigma_{daily}$  
  
隐含波动率 (Implied Volatility) 是一种前瞻性的年化波动率指标，它是通过期权价格反推出来的。例如，在布莱克-斯科尔斯-墨顿 (BSM) 模型中，只要知道期权的市场价格以及其他已知变量（资产价格、执行价格、到期时间、无风险利率），就可以反推出隐含的方差。该方法的一个缺点是它假设方差随时间保持不变。VIX指数就是衡量标准普尔500指数未来30天隐含波动率的常用指标。  
  
**模块 23.2：正态与非正态分布 (Normal and Nonnormal Distributions)**  
  
概率密度函数的前四个矩 (Moments) 分别是：均值 (Mean)、方差 (Variance)、偏度 (Skewness) 和 峰度 (Kurtosis)。  
  
正态分布 (Normal Distribution) 具有对称性且尾部较薄，其偏度为零，峰度为3（即超额峰度为0）。然而，金融资产的实际收益率通常表现出非正态分布 (Nonnormal Distribution)，往往带有偏度并且峰度大于3（即存在正的超额峰度）。  
  
雅克-贝拉检验 (Jarque-Bera Test) 是一种用于检验数据是否符合正态分布的统计方法。其零假设 (Null Hypothesis) 是分布具有零偏度 (S=0) 且峰度为3 (K=3)。检验统计量的公式为（T代表样本量）：  
$JB = (T-1) \left( \frac{S^2}{6} + \frac{(K-3)^2}{24} \right)$  
  
JB统计量服从卡方分布。如果计算出的JB值较小，我们通常无法拒绝零假设，意味着数据接近正态分布；如果JB值很大（超过临界值），则拒绝零假设，表明数据呈非正态分布。  
  
因为金融收益率通常表现为非正态分布，研究其尾部特征非常重要。许多金融数据呈现厚尾 (Fat tails)，即极端值出现的概率比正态分布预期的高。这种特征可以通过幂律 (The Power Law) 来解释，在幂律分布（如学生t分布）下，尾部概率下降的速度非常缓慢。  
  
**模块 23.3：相关性与依赖性 (Correlations and Dependence)**  
  
随机变量可以是独立的 (Independent) 或依赖的 (Dependent)。当变量独立时，风险分散的好处会增加，尾部风险会降低。然而，金融资产往往在横向和纵向上表现出高度的线性或非线性依赖性。  
  
皮尔逊相关系数 (Pearson's Correlation) 用于衡量两个变量之间的线性依赖关系 (Linear Dependence)，而协方差 (Covariance) 衡量的是它们之间的方向性关系。如果两个变量的方差均被标准化为1（单位方差），那么相关系数就等于回归方程中的斜率。  
  
当变量之间存在非线性依赖 (Nonlinear Dependence) 时，传统的皮尔逊相关系数可能不适用。此时可以使用斯皮尔曼等级相关系数 (Spearman's Rank Correlation) 和肯德尔系数 (Kendall's tau)。这两者的取值范围都在 -1 到 1 之间，并且不易受极端异常值的影响。  
  
斯皮尔曼等级相关系数 (Spearman's Rank Correlation) 并不直接使用变量的绝对数值，而是将数值从小到大排序，基于“排名 (Ranks)”来计算线性关系。如果变量自身存在很强的单调关系（非线性），该指标依然能准确捕捉。其公式为（d代表排名差，n代表观察次数）：  
$\rho_s = 1 - \frac{6\sum d_i^2}{n(n^2-1)}$  
  
肯德尔系数 (Kendall's tau) 的原理是通过计算数据对中的一致对 (Concordant pairs) 和不一致对 (Discordant pairs) 的相对频率来衡量相关性。如果所有对都是一致的，结果为1；如果是完全相反的，结果为-1。公式为（$n_c$代表一致对数量，$n_d$代表不一致对数量）：  
$\tau = \frac{n_c - n_d}{n(n-1)/2}$  
  
最后，相关系数矩阵必须满足正定性 (Positive Definiteness)。这意味着任何随机变量的加权平均组合其方差都必须是非负的。为了确保相关性矩阵的正定性，实践中通常采用结构化的方法，比如假设所有资产相关性相等的等相关 (Equicorrelation) 结构，或者假设相关性是由共同因子敞口 (Common factor exposure) 所驱动的结构。  
  
# 24  
  
这是一份关于“模拟与自举法 (Simulation and Bootstrapping)”的系统性、全面且通俗易懂的中文总结：  
  
蒙特卡洛模拟 (Monte Carlo simulation) 常用于对复杂问题进行建模，或者在样本量较小的情况下估计变量。其在金融领域的实际应用包括：为奇异期权定价、估计宏观经济变量变化对金融市场的影响，以及在压力测试情景下审查资本要求。  
  
进行蒙特卡洛模拟有五个基本步骤：  
  
1. 生成随机抽取的数据 $x_i = [x_{1i}, x_{2i}, \dots, x_{ni}]$。对于蒙特卡洛过程，这些数据是从假设的数据生成过程 (Data Generating Process, DGP) 中抽取的。  
  
2. 计算感兴趣的统计量或函数，即 $g_i = g(x_i)$。  
  
3. 重复步骤1和步骤2，以产生 $N$ 次复制 (replications)。  
  
4. 根据生成的集合 $\{g_1, g_2, \dots, g_b\}$ 估计感兴趣的量。  
  
5. 通过计算标准误 (standard error) 来评估准确性。应不断增加复制次数 $N$，直到达到所需的准确度水平。  
  
蒙特卡洛模拟的抽样误差 (sampling error) 可以量化为标准误估计值。真实期望值的标准误计算公式为 $s / \sqrt{N}$，其中 $s$ 是输出变量的标准差，$N$ 是模拟中的情景或复制次数。根据这个公式，如果分析师想将标准误估计值缩小10倍，就必须将样本量 $N$ 增加100倍。  
  
然而，对于复杂的多期模拟来说，大幅增加生成情景的数量会导致极高的成本。方差缩减技术 (variance reduction techniques) 提供了一种替代方法来减少蒙特卡洛模拟的抽样误差。两种最常用的技术是对立变量 (antithetic variates) 和控制变量 (control variates)。  
  
对立变量技术 (antithetic variate technique) 通过使用原始随机变量集的补集 (complement set) 重新运行模拟来减少误差。如果每次复制的原始随机抽样集记为 $u_t$，那么就使用记为 $-u_t$ 的随机数补集再运行一次模拟。由于这两个集合是完全负相关的，即 $\text{corr}(u_t, -u_t) = -1$，这种负相关关系意味着使用对立变量所产生的蒙特卡洛抽样误差必定总是更小。  
  
控制变量技术 (control variate technique) 是用具有已知属性的类似变量 $y$（控制变量）来替换具有未知属性的模拟变量 $x$。假设对 $x$ 和 $y$ 使用同一组随机数进行估计，估计值分别记为 $\hat{x}$ 和 $\hat{y}$。变量 $x$ 的新估计值可以定义为 $x^* = y + (\hat{x} - \hat{y})$。  
  
如果控制统计量和感兴趣的统计量高度相关，新的 $x^*$ 变量的抽样误差将小于原始的 $x$ 变量。因为控制变量 $y$ 属性已知，所以没有抽样误差，即 $\text{var}(y) = 0$。只有当 $\text{var}(x^*) < \text{var}(\hat{x})$ 时，控制变量法才能成功减少抽样误差。  
  
自举法 (bootstrapping method) 是另一种生成随机数的方法。与蒙特卡洛模拟不同，自举法不依赖假设的概率分布，而是直接从历史数据样本中抽取实际的历史收益数据。此外，自举法采用有放回抽样，抽出的数据会被放回原数据集，因此可以被再次抽取。自举法直接从未知分布的观测数据中进行抽样，不对数据的分布做任何假设。  
  
第一种常用的自举法是独立同分布自举法 (independent and identically distributed, i.i.d.)。这种方法简单地从观测数据中逐一抽取样本并放回。然而，这种方法只有在观测数据相互独立时才有效。  
  
在金融领域，数据往往在时间上存在依赖性（例如波动率聚集）。当观测数据不独立时，需要使用循环块自举法 (circular block bootstrap, CBB)。该方法不再抽取单个观测值，而是有放回地抽取由多个观测值组成的“数据块” (blocks)，直到达到所需的样本量。对于样本量为 $n$ 的数据集，数据块的大小通常设定为 $\sqrt{n}$ 较为合适。  
  
自举法虽然有用，但也存在局限性：当当前金融市场状况偏离常态时，使用整个历史数据集可能不可靠（例如在金融危机期间使用历史低波动率数据会低估风险）。此外，如果市场发生了根本性的结构性变化 (structural changes)（如长期的零利率政策），使用旧的历史数据将无法有效重现当前时期的真实情况。  
  
伪随机数生成器 (pseudo-random number generators, PRNGs) 是一种用于生成不规则数值序列的算法。“伪”一词意味着这些由计算机生成的数字并不是真正随机的，而是通过特定的公式生成的，通常产生在0和1之间均匀分布的随机数。  
  
要生成伪随机数，必须首先选择一个初始种子值 (seed value)。种子值决定了将生成的随机数序列。任何特定的种子值每次都会生成完全相同的一组数值。这种特性带来了两个好处：第一是可重复性 (Repeatability)，相同的种子值可以确保在不同实验中或监管审查时重现模拟结果；第二是便于计算集群 (Computing Clusters) 协同工作，使用相同的种子值可以让集群中的不同计算机在模拟复杂投资组合时使用相同的一组随机数。  
  
无论是蒙特卡洛还是自举法，模拟方法在解决金融问题时都存在以下缺点：  
  
1. 数据生成过程 (DGP) 设定不当：如果模型输入或数据生成过程的假设不切实际，即使模拟迭代次数再多也会导致结果不准确。例如，期权价格通常具有肥尾分布特征，如果模型错误地假设其服从正态分布，必然会导致错误的结论。  
  
2. 计算成本 (Computational cost) 高昂：为了获得可接受的结果，通常需要大量的复制（蒙特卡洛模拟通常至少需要10,000次复制）。随着金融市场和被检验问题的复杂性不断增加，相关的计算成本和时间消耗可能会非常巨大。  
  
# 25  
  
**机器学习方法 (Machine Learning Methods)**  
  
本阅读材料全面系统地介绍了机器学习模型及其在金融和运营场景中的应用。核心内容涵盖机器学习与传统计量经济学的本质区别、机器学习的三大主要分类、数据降维与聚类算法、模型训练中的拟合问题及数据验证方法，以及强化学习和自然语言处理技术。  
  
**机器学习与传统计量经济学 (Machine Learning vs. Classical Econometrics)**  
  
机器学习 (Machine Learning) 代表了一系列依靠模型自主识别数据模式以解决实际应用问题的技术。与传统统计和计量经济学相比，机器学习专门为处理海量数据 (Big data) 而设计，具有极高的灵活性，并能捕捉标准线性模型无法处理的复杂非线性交互关系。  
  
在传统计量经济学中，经济或金融理论驱动整个数据生成过程，分析师需要人为设定假设、选择模型和变量。然而在机器学习中，完全由数据本身决定模型应包含的内容，分析师无需在流程中测试特定的预设假设。传统模型极度依赖变量独立性、正态分布和统计显著性，而监督式机器学习 (Supervised learning) 的唯一核心目标是预测的准确性 (Prediction accuracy)。  
  
<img width="50%" src="https://github.com/user-attachments/assets/4f1b9e83-67ca-446b-8028-99b46614019a" />
  
**机器学习的三大分类 (Categories of Machine Learning)**  
  
监督学习 (Supervised learning)：利用“带标签的数据” (Labeled data) 让算法进行学习，主要用于预测连续变量的值（如预测房价）或对观察结果进行分类（如预测比赛胜负）。  
  
无监督学习 (Unsupervised learning)：在没有设定特定目标变量的情况下对数据进行模式识别。它不用于直接预测，而是用于数据聚类 (Data clustering) 和识别数据集的深层结构特征。  
  
强化学习 (Reinforcement learning)：在不断变化的环境中，结合试错法 (Trial-and-error) 采取决策，核心目标是使获得的奖励最大化。  
  
**数据准备与清洗 (Data Preparation and Cleaning)**  
  
为了满足机器学习模型对数据维度的要求，通常需要使用以下两种主要方法对特征变量进行缩放：  
  
标准化 (Standardization)：这是处理包含异常值 (Outliers) 的广泛数据的首选方法。该过程通过减去均值并除以标准差，使变量转化为均值为0、方差为1的标准尺度。公式如下：  
$$ \tilde{x}_{i,j} = \frac{x_{i,j} - \hat{\mu}_i}{\hat{\sigma}_i} $$  
  
归一化/最小最大变换 (Normalization / Min-max transformation)：该方法将变量严格缩放到0到1之间，处理后的数据通常不再具有零均值或单位方差。公式如下：  
$$ \tilde{x}_{i,j} = \frac{x_{i,j} - x_{i,min}}{x_{i,max} - x_{i,min}} $$  
  
数据清洗 (Data cleaning) 是极其耗时且关键的一步，必须妥善处理缺失数据 (Missing data)、异常值、重复记录以及不相关的无效观测值。  
  
**主成分分析 (Principal Components Analysis, PCA)**  
  
主成分分析是一种在无监督学习中广泛使用的降维 (Dimension reduction) 技术。它的根本目的是通过提取少量毫无相关性的组件 (Uncorrelated components) 来替代原有大量存在相关性的特征变量，同时保证输出几乎相同数量的有效信息。在金融中，PCA常用于分析收益率曲线的变动情况（如平行移动和扭曲）。  
  
**K均值聚类 (K-Means Clustering)**  
  
K均值算法 (K-means algorithm) 是一种无监督学习模型，用于探究数据集的结构并将观察结果划分为 K 个聚类 (Clusters)。K值由分析师提前设定，每个数据聚类的中心点被称为质心 (Centroids)。  
  
分配数据点时，需要计算距离，常用的距离测量方法有两种：  
欧几里得距离 (Euclidean distance)：两点之间的直线对角线距离。  
$$ d_E = \sqrt{\sum_{i=1}^{m} (x_{iQ} - x_{iP})^2} $$  
曼哈顿距离 (Manhattan distance)：两点之间沿着横纵轴网格路径走的直角距离。  
$$ d_M = \sum_{i=1}^{m} |x_{iQ} - x_{iP}| $$  
  
<img width="50%" src="https://github.com/user-attachments/assets/3ca9b961-1a51-4587-95e7-84ab1649bc6d" />
  
K均值算法的目标是让每个数据点尽可能靠近其所属的质心。惯性 (Inertia) 是衡量这种距离的指标，惯性越低代表聚类拟合度越好。分析师通常使用碎石图 (Scree plot) 来寻找最优的K值，图中惯性下降速度开始显著放缓的“肘部” (Elbow) 即为最佳质心数量。此外，也可以通过计算轮廓系数 (Silhouette coefficient) 来辅助判断，得分最高时对应的K值即为最优选择。  
  
<img width="50%" src="https://github.com/user-attachments/assets/8d40d318-c57b-48b1-afaf-5a6a6b54f867" />
  
**欠拟合与过拟合 (Underfitting and Overfitting)**  
  
过拟合 (Overfitting) 发生在模型过于复杂、参数过多时。过拟合的模型在训练集上错误率极低，但在面对外部新数据时表现非常糟糕（表现为低偏差、高方差）。这是机器学习模型面临的主要风险。  
  
欠拟合 (Underfitting) 发生在模型过于简单，遗漏了关键影响因素，导致根本无法捕捉到数据中的相关模式。欠拟合会导致预测结果产生系统性偏差（表现为高偏差、低方差）。  
  
模型设计必须在偏差 (Bias) 和方差 (Variance) 之间做出权衡，以找到最佳的模型复杂度。  
  
<img width="50%" src="https://github.com/user-attachments/assets/3d770b7b-56c5-47c1-9387-106e6096d5f8" />
  
**数据集的划分与交叉验证 (Data Sub-Samples and Cross-Validation)**  
  
为了评估机器学习模型预测未知数据的真实能力，通常将完整的数据样本划分为三个独立的子集：  
  
训练集 (Training set)：通常占数据的2/3，用于让模型学习并估计出模型的各个参数。  
  
验证集 (Validation set)：通常占数据的1/6，用于在两个或多个备选模型之间进行对比和决定。  
  
测试集 (Test set)：通常占数据的1/6，用于评估最终选定模型在从未见过的数据上的泛化能力 (Model generalization)。  
  
如果可用的数据集较小，则可以采用 K折交叉验证 (k-fold cross-validation)。这种方法将训练集和验证集合并后分成 k 等份，轮流留下其中一份用于验证，其余用于训练，最后计算平均性能以降低误差风险。  
  
**强化学习 (Reinforcement Learning)**  
  
强化学习致力于构建一套决策系统，目标是最大化所能获得的奖励 (Maximize reward)。算法在初始阶段表现通常较弱，但通过在环境中不断试错，性能会随着时间推移呈指数级提升。  
  
强化学习的三大核心要素包括：定义环境的 状态 (States, S)、做出的 动作 (Actions, A) 以及由此产生的 奖励 (Rewards, R)。  
  
算法在每一步都需要权衡：是采用目前已知最优的动作（称为利用，Exploitation），还是尝试全新的未知动作（称为探索，Exploration）。随着试验次数的增加，算法越来越确信最佳策略，选择“利用”的概率将不断上升。  
  
蒙特卡洛法 (Monte Carlo method) 常用于评估在特定状态下采取动作的期望价值 (Q-value)。其更新公式如下：  
$$ Q^{new}(S, A) = Q^{old}(S, A) + \alpha[R - Q^{old}(S, A)] $$  
  
**自然语言处理 (Natural Language Processing, NLP)**  
  
自然语言处理 (NLP) 也就是文本挖掘 (Text mining)，致力于让计算机理解和分析人类的口头与书面语言。它极大地提升了文件审查的速度，并消除了人工审查中固有的偏见和不一致性。在预处理步骤完成后，NLP算法通常将文本视为“词袋” (Bag of words)，即完全忽略单词在文中的先后顺序。  
  
将非结构化文本转化为机器学习可用的数据，需要经过以下关键的预处理步骤：  
1. 分词 (Tokenization)：剔除标点符号、特殊符号和空格，只保留纯单词，并将所有字母转换为小写。  
2. 剔除停用词 (Removing stopwords)：删掉诸如“the”、“has”、“a”等只为句子通顺但没有实际分析价值的常用连接词。  
3. 词干提取 (Stemming)：强行截断单词后缀，将其替换为词干（如将 arguing 统一映射为 augu）。  
4. 词形还原 (Lemmatization)：与词干提取类似，但它会将其还原为字典里的真实基础词（如将 worse 还原为 bad）。  
5. N元语法 (N-grams)：将若干个一起出现才有特殊意义的相邻单词打包作为一个整体处理，避免拆分后语意丢失。  
  
# 26  
  
这份文档系统且全面地总结了关于机器学习与预测（Machine Learning and Prediction）的核心知识点。以下为核心内容的简体中文总结。  
  
**模块 26.1：分类变量、正则化与逻辑回归 (Categorical Variables, Regularization, and Logistic Regression)**  
  
**分类变量 (Categorical Variables)**  
  
在机器学习或回归模型中使用定性信息时，必须通过映射（Mapping）或编码（Encoding）将非数值数据转化为数值。  
  
对于没有自然顺序的分类（如不同的居住县），应使用独热编码（One-hot encoding），即为每个类别设立一个独立的虚拟变量（Dummy variable），符合条件的类别赋值为1，其他为0。  
  
对于有自然顺序的分类（如收入范围），虚拟变量可以采用顺序数值。例如，低收入为0，中等收入为1，高收入为2。  
  
**正则化 (Regularization)**  
  
正则化用于防止模型变得过于庞大或复杂，从而简化模型并降低过拟合（Overfitting）的概率。主要方法是通过在目标函数或损失函数（Loss function）中添加惩罚项（Penalty term）来实现。  
  
岭回归（Ridge Regression / L2 Regularization）：其目标是减小斜率系数的幅度，使其逼近于零。它使用解析法求解，其惩罚项是斜率系数的平方和。公式为：  
$L = RSS + \lambda \sum_{j=1}^m \beta_j^2$  
其中 RSS 为残差平方和，$\lambda$ 为超参数（Hyperparameter）。  
  
套索回归（LASSO / L1 Regularization）：与岭回归类似，但其惩罚项是斜率系数的绝对值之和。它使用数值法求解。LASSO 会将不重要的斜率系数精确缩减为零，因此被视为一种特征选择（Feature selection）方法。随着 $\lambda$ 的增大，被移除的特征越多。公式为：  
$L = RSS + \lambda \sum_{j=1}^m |\beta_j|$  
  
弹性网络（Elastic Net）：这是岭回归和 LASSO 的混合体，其损失函数将两种方法的惩罚项相加。  
  
**逻辑回归 (Logistic Regression)**  
  
金融领域的模型输出通常是二元的（如是否违约），分别赋值为“1”和“0”。线性模型不适用此类情况，应使用逻辑回归（Logit）模型。它通过累积逻辑函数产生介于0到1之间的输出（也称为S型曲线 Sigmoid curve）。  
  
功能形式和概率公式如下：  
$y_j = \alpha + \beta_1 x_{1j} + \beta_2 x_{2j} + ... + \beta_m x_{mj}$  
$f(y_j) = \frac{1}{1 + e^{-y_j}}$  
  
由于逻辑模型是非线性的，无法使用普通最小二乘法，通常采用最大似然法（Maximum likelihood method）来估计模型参数。  
  
生成预测时，需要设定一个阈值（Threshold, Z）。Z 的设定取决于犯错的成本。如果将违约预测为不违约的成本远高于将不违约预测为违约的成本，Z 应该设定为一个较低的值（如0.1）。  
  
**模块 26.2：决策树、集成学习、K近邻与支持向量机 (Decision Trees, Ensemble Learning, K-Nearest Neighbors, and Support Vector Machines)**  
  
**决策树 (Decision Trees)**  
  
决策树是一种监督式机器学习技术（Supervised machine learning），通过倒置的树状图视觉化地展示连续的输入特征。它们通常被称为分类与回归树（CARTs），属于易于解释的“白盒模型”（White-box models）。  
  
<img width="50%" src="https://github.com/user-attachments/assets/2962c69e-3e50-43f8-bf70-2c1712d910a8" />
  
信息增益（Information gain）衡量获取某特征信息后能减少多少不确定性。决策树在每个节点寻找最大化信息增益的特征。常用的衡量标准包括熵（Entropy）和基尼系数（Gini coefficient）。  
  
熵是数据集中无序程度的衡量，值在0到1之间：  
$\text{entropy} = -\sum_{i=1}^n p_i \log_2(p_i)$  
  
基尼系数（Gini impurity）的公式为：  
$Gini = 1 - \sum_{i=1}^n p_i^2$  
  
决策树的主要风险是过拟合。可以通过设置停止规则来缓解，包括预剪枝（Pre-pruning，当样本数低于某数值时停止分裂）和后剪枝（Post-pruning，建好大树后移除较弱的节点）。  
  
**集成学习 (Ensemble Learning)**  
  
集成学习将多种不同模型的输出组合成单一模型，利用“群体智慧”（Wisdom of crowds）平均多个预测，并防止过拟合。构建集成学习的三种技术包括：  
  
装袋法（Bootstrap aggregation / Bagging）：从训练集中通过有放回抽样（Sampled with replacement）创建多棵决策树，并取其平均值以降低模型方差。  
  
随机森林（Random forests）：应用了装袋法，但进一步通过减少决策树之间的相关性来改进。每次分裂只随机选取一部分特征（通常是总特征数的平方根）。  
  
提升法（Boosting）：基于之前生成的树来提升模型表现。例如梯度提升（Gradient boosting）利用前一模型的残差构建新模型，而自适应提升（AdaBoost）则对分类错误的输出顺序增加权重。  
  
**K近邻 (K-Nearest Neighbors, KNN) 与 支持向量机 (Support Vector Machines, SVMs)**  
  
K近邻（KNN）是一种不预先学习数据集关系的“惰性学习者”（Lazy learner）。它通过计算距离，找到距离待预测数据点最近的 $K$ 个训练样本，并取其平均值作为预测值。$K$ 的值很关键：过大的 $K$ 会导致高偏差（Bias）和低方差（Variance），反之亦然。通常 $K$ 设为训练样本数 $n$ 的平方根。  
  
支持向量机（SVMs）适用于特征数量庞大的情况。其目标是利用特征开发出一条路径，以图形方式分离不同的类别（如违约与不违约）。SVM 创建由两条平行线组成的最宽路径，落在路径边缘的数据点称为支持向量（Support vectors）。  
  
<img width="50%" src="https://github.com/user-attachments/assets/7ca3bc46-0a00-4083-82d7-4fb058de92f0" />
  
**模块 26.3：神经网络与模型表现 (Neural Networks and Model Performance)**  
  
**神经网络 (Neural Networks)**  
  
人工神经网络（ANNs）模仿人脑的工作方式。带有反向传播（Backpropagation）的前馈网络（Feedforward network）是最常见的类型，反向传播描述了偏置和权重在迭代中不断更新的过程。  
  
<img width="50%" src="https://github.com/user-attachments/assets/d1df0a14-4326-4b65-97f2-caa6a30e099b" />
  
网络通过将权重（Weights, $w$）应用于输入，并加上常数偏置（Bias）来决定节点输出。激活函数（Activation function，如逻辑函数）用于在输入和输出之间引入非线性关系。  
  
因为没有最佳参数的解析公式，通常使用梯度下降算法（Gradient descent algorithm）来最小化目标（损失）函数。学习率（Learning rate）决定了梯度下降步长的大小。  
  
神经网络容易发生过拟合。为防止过拟合，应同时对训练集和验证集进行计算。当目标函数值在验证集上开始上升（即使在训练集上仍在下降）时，就是停止梯度下降算法的最佳时机。  
  
**模型预测表现 (Model Predictive Performance)**  
  
当模型有二元分类输出时，可使用混淆矩阵（Confusion matrix）来评估。它包含四个元素：真阳性（True positive, TP）、假阳性（False positive, FP）、真阴性（True negative, TN）和假阴性（False negative, FN）。  
  
基于混淆矩阵可以衍生出以下性能指标：  
准确率（Accuracy） = $\frac{TP + TN}{TP + TN + FP + FN}$  
精确率（Precision） = $\frac{TP}{TP + FP}$  
召回率（Recall） = $\frac{TP}{TP + FN}$  
错误率（Error Rate） = $\frac{FP + FN}{TP + TN + FP + FN}$  
  
接收者操作特征曲线（ROC curve）展示了真阳性率与假阳性率之间的联系。ROC曲线下面积（AUC）越大，预测越好。AUC为1表示完美预测，AUC为0.5表示没有预测价值（随机模型）。  
  
<img width="50%" src="https://github.com/user-attachments/assets/92b78fb4-9816-4c89-9d35-a151a58cc56a" />
  
# 公式  
  
以下是根据您提供的文档系统、全面总结的公式汇总表。为了清晰易懂，数学公式已使用标准格式排版，核心术语已保留英文对照，段落间保留了空行。  
  
**Reading 12**  
  
联合概率 (joint probability)：  
$$P(AB) = P(A|B) \times P(B)$$  
  
条件概率 (conditional probability)：  
$$P(A|B) = \frac{P(AB)}{P(B)}$$  
  
贝叶斯公式 (Bayes' formula)：  
$$P(A|B) = \frac{P(B|A) \times P(A)}{P(B)}$$  
  
**Reading 13**  
  
期望值 (expected value)：  
$$E(X) = \Sigma P(x_i)x_i = P(x_1)x_1 + P(x_2)x_2 + \dots + P(x_n)x_n$$  
  
方差 (variance)：  
$$\sigma^2 = E\{[X - E(X)]^2\} = E[(X - \mu)^2]$$  
  
偏度 (skewness)：  
$$skewness = \frac{E[(X - \mu)^3]}{\sigma^3}$$  
  
峰度 (kurtosis)：  
$$kurtosis = \frac{E[(X - \mu)^4]}{\sigma^4}$$  
  
**Reading 14**  
  
均匀分布范围 (uniform distribution range)：  
$$P(x_1 \le X \le x_2) = \frac{x_2 - x_1}{b - a}$$  
  
连续均匀分布的概率密度函数 (PDF of continuous uniform distribution)：  
$$f(x) = \frac{1}{b - a} \quad (\text{当 } a \le x \le b \text{ 时，否则 } f(x) = 0)$$  
  
均匀分布的均值 (mean of uniform distribution)：  
$$E(x) = \frac{a + b}{2}$$  
  
均匀分布的方差 (variance of uniform distribution)：  
$$Var(x) = \frac{(b - a)^2}{12}$$  
  
二项概率函数 (binomial probability function)：  
$$p(x) = \frac{n!}{(n - x)!x!} p^x (1 - p)^{n - x}$$  
  
二项随机变量的期望值 (expected value of binomial random variable)：  
$$E(X) = np$$  
  
二项随机变量的方差 (variance of binomial random variable)：  
$$variance = np(1 - p)$$  
  
泊松分布 (Poisson distribution)：  
$$P(X = x) = \frac{\lambda^x e^{-\lambda}}{x!}$$  
  
卡方检验统计量 (chi-squared test statistic)：  
$$\chi_{n-1}^2 = \frac{(n - 1)s^2}{\sigma_0^2}$$  
其中：n = 样本量 (sample size)，s² = 样本方差 (sample variance)，σ₀² = 总体方差的假设值 (hypothesized value for the population variance)  
  
F检验 (F-test)：  
$$F = \frac{s_1^2}{s_2^2}$$  
其中：s₁² = 来自总体1的n₁个观察值的样本方差 (variance of the sample of n₁ observations drawn from Population 1)，s₂² = 来自总体2的n₂个观察值的样本方差 (variance of the sample of n₂ observations drawn from Population 2)  
  
**Reading 15**  
  
协方差 (covariance)：  
$$Cov[X_1, X_2] = E[(X_1 - E[X_1])(X_2 - E[X_2])]$$  
$$Cov[X_1, X_2] = E[X_1X_2] - E[X_1]E[X_2]$$  
  
相关系数 (correlation)：  
$$Corr(X_1, X_2) = \frac{Cov[X_1, X_2]}{\sqrt{Var[X_1]} \sqrt{Var[X_2]}} = \frac{\sigma_{12}}{\sqrt{\sigma_1^2} \sqrt{\sigma_2^2}} = \frac{\sigma_{12}}{\sigma_1 \sigma_2}$$  
  
两种资产组合的方差 (variance of a two-asset portfolio)：  
$$\sigma_p^2 = w_1^2 \sigma_1^2 + (1 - w)^2 \sigma_2^2 + 2w_1(1 - w)\sigma_{12}$$  
  
**Reading 16**  
  
样本均值 (sample mean)：  
$$\hat{\mu} = \frac{\sum_{i=1}^n X_i}{n}$$  
  
样本方差 (sample variance)：  
$$s^2 = \frac{n}{n - 1} \hat{\sigma}^2 = \frac{1}{n - 1} \sum_{i=1}^n (X_i - \hat{\mu})^2$$  
  
样本协方差 (sample covariance)：  
$$sample \ Cov_{XY} = \frac{\sum_{i=1}^n (X_i - \hat{\mu}_X)(Y_i - \hat{\mu}_Y)}{n - 1}$$  
  
**Reading 17**  
  
检验统计量 (test statistic)：  
$$test \ statistic = \frac{sample \ statistic - hypothesized \ value}{standard \ error \ of \ the \ sample \ statistic}$$  
  
样本均值的标准误 (standard error of sample mean)：  
$$\sigma_{\bar{x}} = \frac{\sigma}{\sqrt{n}}$$  
  
置信区间 (confidence interval)：  
$$[sample \ statistic - (critical \ value)(standard \ error)] \le population \ parameter \le [sample \ statistic + (critical \ value)(standard \ error)]$$  
  
z统计量 (z-statistic)：  
$$z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}}$$  
其中：x̄ = 样本均值 (sample mean)，μ₀ = 假设的总体均值 (hypothesized population mean)，σ = 总体标准差 (standard deviation of the population)，n = 样本量 (sample size)  
  
均值相等检验统计量 (equality of means test statistic)：  
$$T = \frac{\bar{X} - \bar{Y}}{\sqrt{\frac{s_X^2 + s_Y^2 - 2Cov(X,Y)}{n}}}$$  
  
**Reading 18**  
  
回归方程 (regression equation)：  
$$Y = \alpha + \beta X + \varepsilon$$  
其中：β = 回归或斜率系数；Y对X变化的敏感度 (regression or slope coefficient; sensitivity of Y to changes in X)，α = 当X=0时Y的值 (value of Y when X = 0)，ε = 随机误差或冲击；Y中未被X解释的部分 (random error or shock; unexplained component of Y)  
  
回归斜率系数 (regression slope coefficient)：  
$$\beta = \frac{\sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})}{\sum_{i=1}^n (X_i - \bar{X})^2} = \frac{Cov(X,Y)}{Var(X)}$$  
  
回归截距 (regression intercept)：  
$$\alpha = \bar{Y} - \beta \bar{X}$$  
其中：Ȳ = Y的均值 (mean of Y)，X̄ = X的均值 (mean of X)  
  
斜率系数的置信区间 (confidence interval of the slope coefficient)：  
$$\beta \pm (t_c \times S_b)$$  
  
**Reading 19**  
  
多元回归 (multiple regression)：  
$$Y = \alpha + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_k X_k + \varepsilon$$  
其中：β_j = 回归或斜率系数；在控制其他所有X的情况下，Y对X_j变化的敏感度 (slope coefficients; sensitivity of Y to changes in X_j controlling for all other Xs)  
  
总平方和 (total sum of squares)：  
$$\sum (Y_i - \bar{Y})^2 = \sum (\hat{Y}_i - \bar{Y})^2 + \sum (Y_i - \hat{Y}_i)^2$$  
或者：  
$$TSS = ESS + RSS$$  
其中：TSS = 总平方和 (total sum of squares)，ESS = 解释平方和 (explained sum of squares)，RSS = 残差平方和 (residual sum of squares)  
  
决定系数 (coefficient of determination)：  
$$R^2 = \frac{ESS}{TSS}$$  
  
调整后的决定系数 (adjusted R²)：  
$$R_a^2 = 1 - \left[ \left( \frac{n - 1}{n - k - 1} \right) \times (1 - R^2) \right]$$  
其中：n = 观察值数量 (number of observations)，k = 自变量数量 (number of independent variables)  
  
F检验统计量 (FF-statistic)：  
$$F = \frac{(RSS_P - RSS_F) / q}{RSS_F / (n - k_F - 1)} = \frac{(R_F^2 - R_P^2) / q}{(1 - R_F^2) / (n - k_F - 1)}$$  
其中：RSS_F = 全模型的残差平方和 (residual sum of squares of the full model)，RSS_P = 偏模型的残差平方和 (residual sum of squares of the partial model)，R²_F = 全模型的决定系数 (coefficient of determination of the full model)，R²_P = 偏模型的决定系数 (coefficient of determination of the partial model)，q = 施加的限制数量 (number of restrictions imposed)  
  
**Reading 20**  
  
方差膨胀因子 (variance inflation factor, VIF)：  
$$VIF_j = \frac{1}{1 - R_j^2}$$  
  
库克距离 (Cook's distance)：  
$$D_j = \frac{\sum_{i=1}^n (\hat{y}_i^{(-j)} - \hat{y}_i)^2}{k S^2}$$  
其中：ŷ_i^{(-j)} = 删除异常观察值j后的y预测值 (predicted value of y after dropping outlier observation j)，ŷ_i = 未删除任何观察值时的y预测值 (predicted value of y without dropping any observation)  
  
**Reading 21**  
  
一阶自回归过程 (first-order autoregressive [AR(1)] process)：  
注：原文图片中此处公式与移动平均模型公式重复，按原图记录如下。  
$$y_t = \mu + \theta \varepsilon_{t-1} + \varepsilon_t$$  
其中：μ = 时间序列的均值 (mean of the time series)，ε_t = 当前随机白噪声冲击 (current random white noise shock)，ε_{t-1} = 一期滞后的随机白噪声冲击 (one-period lagged random white noise shock)  
  
一阶滑动平均过程 (first-order moving average [MA(1)] process)：  
$$y_t = \mu + \theta \varepsilon_{t-1} + \varepsilon_t$$  
  
滞后算子 (lag operator)：  
$$y_{t-1} = L y_t$$  
  
自回归滑动平均过程 (autoregressive moving average (ARMA) process)：  
$$y_t = d + \Phi y_{t-1} + \varepsilon_t + \theta \varepsilon_{t-1}$$  
其中：d = 截距项 (intercept term)，Φ = 滞后观察值的系数 (coefficient for the lagged observations)  
  
**Reading 22**  
  
线性时间趋势 (linear time trend)：  
$$y_t = \delta_0 + \delta_1 t + \varepsilon_t$$  
  
非线性时间趋势 (nonlinear time trend)：  
$$y_t = \delta_0 + \delta_1 t + \delta_2 t^2 + \varepsilon_t$$  
  
对数二次模型 (log-quadratic model)：  
$$\ln(y_t) = \delta_0 + \delta_1 t + \delta_2 t^2 + \varepsilon_t$$  
  
纯季节性虚拟变量模型 (pure seasonal dummy variable model)：  
$$y_t = \sum_{i=1}^s \gamma_i (D_{i,t}) + \varepsilon_t$$  
  
具有季节性的趋势模型 (trend model with seasonality)：  
$$y_t = \beta_1(t) + \sum_{i=1}^s \gamma_i (D_{i,t}) + \varepsilon_t$$  
  
**Reading 23**  
  
资产回报率 (asset return)：  
$$R_t = \frac{P_t - P_{t-1}}{P_{t-1}}$$  
  
连续复利资产回报率 (continuously compounded asset return)：  
$$r_t = \ln P_t - \ln P_{t-1}$$  
  
年化波动率 (annualized volatility)：  
$$\sigma_{annual} = \sqrt{252 \times \sigma_{daily}^2}$$  
  
Jarque-Bera检验统计量 (Jarque-Bera test statistic)：  
$$JB = (T - 1) \left[ \frac{\hat{S}^2}{6} + \frac{(\hat{K} - 3)^2}{24} \right]$$  
  
幂律 (power law)：  
$$P(X > x) = k x^{-\alpha}$$  
  
斯皮尔曼秩相关系数 (Spearman's rank correlation)：  
$$\hat{\rho}_s = 1 - \frac{6 \sum_{i=1}^n (d_i)^2}{n(n^2 - 1)}$$  
  
肯德尔τ相关系数 (Kendall's tau)：  
$$\hat{\tau} = \frac{n_c - n_d}{n(n-1)/2} = \frac{n_c}{n_c + n_d + n_t} - \frac{n_d}{n_c + n_d + n_t}$$  
其中：n_c = 一致对的数量 (number of concordant pairs)，n_d = 不一致对的数量 (number of discordant pairs)，n_t = 平局/结的数量 (number of ties)  
  
**Reading 25**  
  
标准化 (Standardization)：  
$$\tilde{x}_{ij} = \frac{x_{ij} - \hat{\mu}_i}{\hat{\sigma}_i}$$  
  
归一化 (Normalization)：  
$$\tilde{x}_{ij} = \frac{x_{ij} - x_{i,min}}{x_{i,max} - x_{i,min}}$$  
  
欧氏距离 (Euclidean distance)：  
两个特征 (Two features)：  
$$d_E = \sqrt{(x_{1Q} - x_{1P})^2 + (x_{2Q} - x_{2P})^2}$$  
m个特征 (m features)：  
$$d_E = \sqrt{\sum_{i=1}^m (x_{iQ} - x_{iP})^2}$$  
  
曼哈顿距离 (Manhattan distance)：  
两个特征 (Two features)：  
$$d_M = |x_{1Q} - x_{1P}| + |x_{2Q} - x_{2P}|$$  
m个特征 (m features)：  
$$d_M = \sum_{i=1}^m |x_{iQ} - x_{iP}|$$  
  
惯性 (inertia)：  
$$inertia = \sum_{j=1}^n d_j^2$$  
  
**Reading 26**  
  
岭回归 (Ridge regression)：  
$$L = RSS + \lambda \sum_{i=1}^m \beta_i^2$$  
其中：  
$$RSS = \sum_{i=1}^n \left( y_i - \hat{\alpha} - \sum_{j=1}^m \hat{\beta}_j x_{ij} \right)^2$$  
  
LASSO回归 (LASSO regression)：  
$$L = RSS + \lambda \sum_{i=1}^m |\beta_i|$$  
  
逻辑回归函数 (Logistic regression function)：  
$$f(y_j) = \frac{1}{1 + e^{-y_j}}$$  
  
熵 (Entropy)：  
$$entropy = - \sum_{i=1}^n p_i \log_2(p_i)$$  
  
基尼系数 (Gini coefficient)：  
$$Gini = 1 - \sum_{i=1}^n p_i^2$$  
  
准确率 (Accuracy)：  
$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$  
  
精确率 (Precision)：  
$$Precision = \frac{TP}{TP + FP}$$  
  
召回率 (Recall)：  
$$Recall = \frac{TP}{TP + FN}$$  
  
错误率 (Error Rate)：  
$$Error \ Rate = \frac{FP + FN}{TP + TN + FP + FN}$$
