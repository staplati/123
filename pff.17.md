# 第17章 估值框架  
  
> 复利是有史以来最伟大的数学发现。  
> ——阿尔伯特·爱因斯坦  
  
本章通过介绍开发DX库所需的最基本概念，为该库的开发提供了框架。它简要回顾了资产定价基本定理，该定理为模拟和估值提供了理论背景。然后，本章继续探讨日期处理和风险中性贴现的基本概念。本章仅考虑用于贴现的恒定短期利率这一最简单的情况，但更复杂和现实的模型可以很容易地添加到库中。本章还介绍了市场环境的概念，即几乎所有其他类在后续章节实例化时所需的一组常量、列表和曲线的集合。  
  
本章包括以下部分：  
  
“资产定价基本定理”  
本部分介绍了资产定价基本定理，为待开发的库提供了理论背景。  
  
“风险中性贴现”  
本部分开发了一个类，用于期权和其他衍生工具未来收益的风险中性贴现。  
  
“市场环境”  
本部分开发了一个类，用于管理单一工具以及由多个工具组成的投资组合定价的市场环境。  
  
## 资产定价基本定理  
  
资产定价基本定理是现代金融理论和数学的基石和成功案例之一。1 该定理的核心概念是鞅测度；即一种消除了贴现风险因子（随机过程）漂移的概率测度。换句话说，在鞅测度下，所有风险因子都以无风险短期利率漂移，而不是以包含某种风险溢价的任何其他市场利率漂移。  
  
### 一个简单的例子  
  
考虑一个简单的经济体，在今天和明天这两个日期，拥有一种风险资产（“股票”）和一种无风险资产（“债券”）。该债券今天价值10美元，并在明天支付10美元（零利率）。该股票今天价值10美元，并且在明天分别以60%和40%的概率支付20美元或0美元。债券的无风险回报率为0。股票的预期回报率为 $\frac{0.6\cdot20+0.4\cdot0}{10}-1=0.2$ ，即20%。这是股票为其风险性支付的风险溢价。  
  
现在考虑执行价格为15美元的看涨期权。这种以60%的概率支付5美元，在其他情况下支付0美元的或有索取权，其公允价值是多少？例如，我们可以取期望值，并将结果价值贴现回来（这里是零利率）。这种方法得出的价值为 $0.6\cdot5=3$ 美元，因为期权在股票价格上涨至20美元时支付5美元，在其他情况下支付0美元。  
  
然而，还有另一种成功应用于此类期权定价问题的方法：通过交易证券的投资组合来复制期权的收益。很容易验证，购买0.25份股票可以完美复制期权的收益（在60%的情况发生时，将获得 $0.25\cdot20=5$ 美元）。四分之一的股票只需2.5美元，而不是3美元。在真实世界的概率测度下取期望值高估了期权。  
  
为什么会这样？真实世界的测度意味着股票有20%的风险溢价，因为股票所涉及的风险（赚取100%或损失100%）是“真实的”，意味着它无法被分散或对冲掉。另一方面，我们拥有一个无需任何风险即可复制期权收益的可用投资组合。这也意味着，卖出这种期权的人可以完全对冲掉任何风险。2 这样一个由期权和对冲头寸组成的完美对冲投资组合必须产生无风险利率，以避免套利机会（即以正概率无本金赚钱的机会）。  
  
我们还能保留取期望值的方法来为看涨期权估值吗？是的，这是可能的。人们“只”需要以这样一种方式改变概率，使得风险资产（股票）以零无风险短期利率漂移。显然，赋予两种情况各50%等权重的（鞅）测度实现了这一点；计算过程为 $\frac{0.5\cdot20+0.5\cdot0}{10}-1=0$ 。现在，在新的鞅测度下获取期权收益的期望值，得出正确的（无套利）公允价值： $0.5\cdot5+0.5\cdot0=2.5$ 美元。  
  
### 一般结果  
  
这种方法的美妙之处在于它甚至可以延续应用到最复杂的经济体中，例如连续时间建模（即需要考虑时间轴上的连续点）、大量风险资产、复杂的衍生品收益等。  
  
因此，考虑离散时间下的一般市场模型：3  
  
离散时间下的一般市场模型 $\mathcal{M}$ 是以下各项的集合：  
  
* 有限状态空间 $\Omega$  
  
* 域流 $\mathbb{F}$  
  
* 定义在 $\wp(\Omega)$ 上的严格正概率测度 $P$  
  
* 终止日期 $T\in\mathbb{N}$ ， $T<\infty$  
  
* 一组 $\mathbb{S}\equiv\{(S_t^k)_{t\in\{0,...,T\}}:k\in\{0,...,K\}\}$ 构成的 $K+1$ 个严格正的证券价格过程  
  
综合起来有 $\mathcal{M}=\{(\Omega,\wp(\Omega),\mathbb{F},P),T,\mathbb{S}\}$ 。  
  
基于这样一个一般市场模型，可以如下表述资产定价基本定理：4  
  
考虑一般市场模型 $\mathcal{M}$ 。根据资产定价基本定理，以下三种陈述是等价的：  
  
* 市场模型 $\mathcal{M}$ 中不存在套利机会。  
  
* $P$ 等价鞅测度的集合 $\mathbb{Q}$ 非空。  
  
* 一致线性价格系统的集合 $\mathbb{P}$ 非空。  
  
当涉及或有索取权（即期权、衍生品、期货、远期、掉期等）的估值和定价时，该定理的重要性由以下推论说明：  
  
如果市场模型 $\mathcal{M}$ 是无套利的，那么对于任何可达到的（即可复制的）或有索取权（期权、衍生品等） $V_T$ ，存在一个相关的唯一价格 $V_0$ 。它满足 $\forall{Q}\in\mathbb{Q}:V_0=E_0^Q(e^{-rT}V_T)$ ，其中 $e^{-rT}$ 是恒定短期利率 $r$ 下相关的风险中性贴现因子。  
  
这一结果说明了该定理的重要性，并表明我们早期的简单推理确实可以延续应用到一般市场模型中。  
  
由于鞅测度在其中发挥的作用，这种估值方法也常被称为鞅方法，或者——由于在鞅测度下所有风险资产都以无风险短期利率漂移——风险中性估值方法。对于我们的目的而言，第二个术语可能是更好的选择，因为在数值应用中，人们“只是”让风险因子（随机过程）以风险中性短期利率漂移。我们不必在应用中直接处理概率测度——然而，它们在理论上证明了所应用的核心理论结果和所实施的技术方法的合理性。  
  
最后，考虑一般市场模型中的市场完备性：  
  
如果市场模型 $\mathcal{M}$ 是无套利的，并且每个或有索取权（期权、衍生品等）都是可达到的（即可复制的），那么该市场模型就是完备的。  
  
假设市场模型 $\mathcal{M}$ 是无套利的。当且仅当 $\mathcal{M}$ 是单元素集合时，即存在唯一的 $P$ 等价鞅测度时，该市场模型是完备的。  
  
这主要完成了对后续内容理论背景的讨论。有关概念、记号、定义和结果的详细阐述，请参阅Hilpisch (2015)的第4章。  
  
1 有关数学机制的综合回顾和详细信息，请参阅Delbaen和Schachermayer (2004)。关于较短的介绍，特别是离散时间版本，请参见Hilpisch (2015)的第4章。  
  
2 该策略包括以2.5美元的价格出售期权并以2.5美元的价格购买0.25份股票。无论在这个简单经济体中发生何种情况，该投资组合的收益均为0。  
  
3 关于概率概念，请参见Williams (1991)。  
  
4 参见Delbaen和Schachermayer (2004)。  
  
## 风险中性贴现  
  
显然，风险中性贴现是风险中性估值方法的核心。因此，本节开发了一个用于风险中性贴现的Python类。然而，在进行估值之前，先仔细看看相关日期的建模和处理是有好处的。  
  
### 建模与处理日期  
  
贴现的一个必要先决条件是日期建模。对于估值目的，人们通常将今天与一般市场模型的最终日期 $T$ 之间的时间间隔划分为离散的时间段。这些时间段可以是同构的（即长度相等），也可以是异构的（即长度不等）。一个估值库应该能够处理更一般情况下的异构时间段，因为较简单的情况会自动包含在内。因此，代码使用日期列表进行工作，假设最小的相关时间间隔是一天。这意味着日内事件被认为是不相关的，否则人们必须对时间进行建模（除了日期之外）。5  
  
为了编制一份相关日期的列表，基本上可以采用两种方法之一：构建具体日期的列表（例如，在Python中作为datetime对象）或年化分数的列表（作为十进制数，正如在理论著作中经常做的那样）。  
  
首先进行一些导入：  
  
```python  
In [1]: import numpy as np # 导入numpy库作为np  
        import pandas as pd # 导入pandas库作为pd  
        import datetime as dt # 导入datetime库作为dt  
  
In [2]: from pylab import mpl, plt # 从pylab导入mpl和plt  
        plt.style.use('seaborn') # 设置绘图样式为seaborn  
        mpl.rcParams['font.family'] = 'serif' # 设置字体族为serif  
        %matplotlib inline # 使得matplotlib图表在notebook中内联显示  
  
In [3]: import sys # 导入sys模块  
        sys.path.append('../dx') # 将'../dx'目录添加到系统路径中以方便导入  
```  
  
例如，以下关于日期和分数的两种定义（大致上）是等价的：  
  
```python  
In [4]: dates = [dt.datetime(2020, 1, 1), dt.datetime(2020, 7, 1),  
                 dt.datetime(2021, 1, 1)] # 定义一个包含三个特定日期的列表  
  
In [5]: (dates[1] - dates[0]).days / 365. # 计算第二个日期与第一个日期之间的天数并除以365得到年化分数  
```  
  
```txt  
Out[5]: 0.4986301369863014  
```  
  
```python  
In [6]: (dates[2] - dates[1]).days / 365. # 计算第三个日期与第二个日期之间的天数并除以365得到年化分数  
```  
  
```txt  
Out[6]: 0.5041095890410959  
```  
  
```python  
In [7]: fractions = [0.0, 0.5, 1.0] # 直接定义年化分数列表  
```  
  
5 添加时间组件实际上是一项简单的工作，但为了便于说明，此处未执行此操作。  
  
它们之所以只是大致等价，是因为年化分数很少位于某一天的一开始（凌晨0点）。只需考虑将一年除以50的结果。  
  
有时有必要从日期列表中获取年化分数。函数 `get_year_deltas()` 完成了这项工作：  
  
```python  
#  
# DX Package (DX包)  
#  
# Frame -- Helper Function (框架 -- 辅助函数)  
#  
# get_year_deltas.py  
#  
# Python for Finance, 2nd ed. (Python金融大数据分析，第二版)  
# (c) Dr. Yves J. Hilpisch  
#  
import numpy as np # 导入numpy库作为np  
  
def get_year_deltas(date_list, day_count=365.):  
    ''' Return vector of floats with day deltas in year fractions.  
    (返回包含以年为单位的日期差值的浮点数向量。)  
    Initial value normalized to zero.  
    (初始值归一化为零。)  
  
    Parameters (参数)  
    ==========  
    date_list: list or array (列表或数组)  
        collection of datetime objects (datetime对象的集合)  
    day_count: float (浮点数)  
        number of days for a year (一年的天数)  
        (to account for different conventions) (用于处理不同的计算惯例)  
  
    Results (返回结果)  
    =======  
    delta_list: array (数组)  
        year fractions (年化分数)  
    '''  
    start = date_list[0] # 获取起始日期  
    delta_list = [(date - start).days / day_count  
                  for date in date_list] # 循环计算列表中每个日期与起始日期的差值，转换为年化分数  
    return np.array(delta_list) # 将列表转换为numpy数组并返回  
```  
  
然后可以按如下方式应用此函数：  
  
```python  
In [8]: from get_year_deltas import get_year_deltas # 从模块中导入该函数  
  
In [9]: get_year_deltas(dates) # 将日期列表传入函数计算年化分数  
```  
  
```txt  
Out[9]: array([0.        , 0.49863014, 1.00273973])  
```  
  
在对短期利率进行建模时，这种转换的好处就显而易见了。  
  
### 恒定短期利率  
  
接下来的阐述集中于短期利率贴现的最简单情况；即短期利率随时间保持恒定的情况。许多期权定价模型，如Black-Scholes-Merton (1973)、Merton (1976)或Cox-Ross-Rubinstein (1979)的模型，都做出了这一假设。6 假设采用连续贴现，这在期权定价应用中很常见。在这种情况下，给定未来日期 $t$ 和恒定短期利率 $r$ ，截至今天的通用贴现因子由 $D_0(t)=e^{-rt}$ 给出。当然，对于经济体的结束，特殊情况 $D_0(T)=e^{-rT}$ 成立。请注意，此处的 $t$ 和 $T$ 均以年化分数表示。  
  
这些贴现因子也可以被解释为今天价值为一个单位的零息债券（ZCB）在分别到期于 $t$ 和 $T$ 时的价值。7 给定两个日期 $t\geq{s}\geq0$ ，从 $t$ 贴现到 $s$ 的相关贴现因子则由以下等式给出 $D_s(t)=D_0(t)/D_0(s)=e^{-rt}/e^{-rs}=e^{-rt}\cdot{e^{rs}}=e^{-r(t-s)}$ 。  
  
以下将这些思考转化为类的形式的Python代码：8  
  
```python  
#  
# DX Library (DX库)  
#  
# Frame -- Constant Short Rate Class (框架 -- 恒定短期利率类)  
#  
# constant_short_rate.py  
#  
# Python for Finance, 2nd ed. (Python金融大数据分析，第二版)  
# (c) Dr. Yves J. Hilpisch  
#  
from get_year_deltas import * # 导入get_year_deltas模块的所有内容  
  
class constant_short_rate(object):  
    ''' Class for constant short rate discounting.  
    (用于恒定短期利率贴现的类。)  
  
    Attributes (属性)  
    ==========  
    name: string (字符串)  
        name of the object (对象的名称)  
    short_rate: float (positive) (浮点数(正数))  
        constant rate for discounting (用于贴现的恒定利率)  
  
    Methods (方法)  
    =======  
    get_discount_factors:  
        get discount factors given a list/array of datetime objects  
        or year fractions  
        (给定datetime对象或年化分数的列表/数组，获取贴现因子)  
    '''  
  
    def __init__(self, name, short_rate):  
        self.name = name # 初始化名称  
        self.short_rate = short_rate # 初始化短期利率  
        if short_rate < 0: # 如果短期利率小于0  
            raise ValueError('Short rate negative.') # 抛出负利率错误  
            # this is debatable given recent market realities  
            # (鉴于最近的市场现实，这值得商榷)  
  
    def get_discount_factors(self, date_list, dtobjects=True):  
        if dtobjects is True: # 如果传入的是datetime对象  
            dlist = get_year_deltas(date_list) # 转换为年化分数  
        else:  
            dlist = np.array(date_list) # 否则直接将列表转换为numpy数组  
        dflist = np.exp(self.short_rate * np.sort(-dlist)) # 计算贴现因子序列  
        return np.array((date_list, dflist)).T # 组合日期和贴现因子，转置后返回  
```  
  
6 对于例如短期期权的定价，这一假设在许多情况下似乎是满足的。  
  
7 单位零息债券在到期时精确支付一个货币单位，而在今天和到期之间没有息票。  
  
8 关于Python中面向对象编程（OOP）的基础知识，请参见第6章。在这里以及本部分的其余内容中，在Python类命名方面偏离了标准PEP 8惯例。PEP 8建议对Python类名一般使用“CapWords”或“CamelCase”惯例。本部分的代码更倾向于使用PEP 8中提到的函数命名约定，作为“在接口被文档化且主要用作可调用对象的情况下”的一种有效替代方案。  
  
类 `dx.constant_short_rate` 的应用可以通过一个简单具体的例子得到最好的说明。主要结果是一个二维的ndarray对象，其中包含datetime对象和相关贴现因子的配对。该类本身，特别是对象 `csr`，也能处理年化分数：  
  
```python  
In [10]: from constant_short_rate import constant_short_rate # 导入该类  
  
In [11]: csr = constant_short_rate('csr', 0.05) # 实例化一个短期利率为0.05的类对象  
  
In [12]: csr.get_discount_factors(dates) # 获取日期列表对应的贴现因子  
```  
  
```txt  
Out[12]: array([[datetime.datetime(2020, 1, 1, 0, 0), 0.9510991280247174],  
                [datetime.datetime(2020, 7, 1, 0, 0), 0.9753767163648953],  
                [datetime.datetime(2021, 1, 1, 0, 0), 1.0]], dtype=object)  
```  
  
```python  
In [13]: deltas = get_year_deltas(dates) # 计算日期的年化分数  
         deltas # 打印查看  
```  
  
```txt  
Out[13]: array([0.        , 0.49863014, 1.00273973])  
```  
  
```python  
In [14]: csr.get_discount_factors(deltas, dtobjects=False) # 直接使用年化分数获取贴现因子，需设置dtobjects为False  
```  
  
```txt  
Out[14]: array([[0.        , 0.95109913],  
                [0.49863014, 0.97537672],  
                [1.00273973, 1.        ]])  
```  
  
这个类将负责处理其他类中所需的所有贴现操作。  
  
## 市场环境  
  
市场环境“仅仅”是其他数据和Python对象集合的名称。然而，使用这种抽象非常方便，因为它简化了许多操作，并且允许对重复出现的方面进行一致的建模。9 市场环境主要由三个字典组成，用于存储以下类型的数据和Python对象：  
  
常量 (Constants)  
例如，这些可以是模型参数或期权到期日。  
  
列表 (Lists)  
这些通常是对象的集合，比如一组用来模拟（风险）证券的对象列表。  
  
曲线 (Curves)  
这些是用于贴现的对象；例如， `dx.constant_short_rate` 类的实例。  
  
以下是 `dx.market_environment` 类的代码。有关处理字典对象的详细信息，请参阅第3章：  
  
```python  
#  
# DX Package (DX包)  
#  
# Frame -- Market Environment Class (框架 -- 市场环境类)  
#  
# market_environment.py  
#  
# Python for Finance, 2nd ed. (Python金融大数据分析，第二版)  
# (c) Dr. Yves J. Hilpisch  
#  
class market_environment(object):  
    ''' Class to model a market environment relevant for valuation.  
    (用于为估值相关的市场环境建模的类。)  
  
    Attributes (属性)  
    ==========  
    name: string (字符串)  
        name of the market environment (市场环境的名称)  
    pricing_date: datetime object (datetime对象)  
        date of the market environment (市场环境的日期)  
  
    Methods (方法)  
    =======  
    add_constant:  
        adds a constant (e.g. model parameter)  
        (添加一个常量（例如模型参数）)  
    get_constant:  
        gets a constant  
        (获取一个常量)  
    add_list:  
        adds a list (e.g. underlyings)  
        (添加一个列表（例如标的资产）)  
    get_list:  
        gets a list  
        (获取一个列表)  
    add_curve:  
        adds a market curve (e.g. yield curve)  
        (添加一条市场曲线（例如收益率曲线）)  
    get_curve:  
        gets a market curve  
        (获取一条市场曲线)  
    add_environment:  
        adds and overwrites whole market environments  
        with constants, lists, and curves  
        (通过常量、列表和曲线添加并覆盖整个市场环境)  
    '''  
  
    def __init__(self, name, pricing_date):  
        self.name = name # 初始化名称  
        self.pricing_date = pricing_date # 初始化定价日期  
        self.constants = {} # 初始化常量字典  
        self.lists = {} # 初始化列表字典  
        self.curves = {} # 初始化曲线字典  
  
    def add_constant(self, key, constant):  
        self.constants[key] = constant # 将常量存入字典  
  
    def get_constant(self, key):  
        return self.constants[key] # 从字典获取常量  
  
    def add_list(self, key, list_object):  
        self.lists[key] = list_object # 将列表存入字典  
  
    def get_list(self, key):  
        return self.lists[key] # 从字典获取列表  
  
    def add_curve(self, key, curve):  
        self.curves[key] = curve # 将曲线存入字典  
  
    def get_curve(self, key):  
        return self.curves[key] # 从字典获取曲线  
  
    def add_environment(self, env):  
        # overwrites existing values, if they exist  
        # (如果存在现有值，则将其覆盖)  
        self.constants.update(env.constants) # 更新常量字典  
        self.lists.update(env.lists) # 更新列表字典  
        self.curves.update(env.curves) # 更新曲线字典  
```  
  
9 关于这个概念，另请参阅Fletcher和Gardner (2009)，他们广泛地使用了市场环境。  
  
虽然 `dx.market_environment` 类本身并没有什么特别之处，但一个简单的示例将说明使用该类的实例进行工作是多么方便：  
  
```python  
In [15]: from market_environment import market_environment # 导入市场环境类  
  
In [16]: me = market_environment('me_gbm', dt.datetime(2020, 1, 1)) # 实例化一个市场环境对象  
  
In [17]: me.add_constant('initial_value', 36.) # 添加初始值常量  
  
In [18]: me.add_constant('volatility', 0.2) # 添加波动率常量  
  
In [19]: me.add_constant('final_date', dt.datetime(2020, 12, 31)) # 添加最后日期常量  
  
In [20]: me.add_constant('currency', 'EUR') # 添加货币常量  
  
In [21]: me.add_constant('frequency', 'M') # 添加频率常量  
  
In [22]: me.add_constant('paths', 10000) # 添加路径数量常量  
  
In [23]: me.add_curve('discount_curve', csr) # 添加贴现曲线  
  
In [24]: me.get_constant('volatility') # 获取波动率常量  
```  
  
```txt  
Out[24]: 0.2  
```  
  
```python  
In [25]: me.get_curve('discount_curve').short_rate # 获取贴现曲线中的短期利率属性  
```  
  
```txt  
Out[25]: 0.05  
```  
  
这说明了对这个相当通用的“存储”类的基本处理。在实际应用中，首先收集市场数据和其他数据以及Python对象，然后实例化一个 `dx.market_environment` 对象，并用相关数据和对象对其进行填充。随后只需单步操作，即可将其传递给需要提取并存储在各自的 `dx.market_environment` 对象中的数据和对象的其他类。  
  
这种面向对象的建模方法的一个主要优点是，例如， `dx.constant_short_rate` 类的实例可以存在于多个环境中（见第6章关于聚合的主题）。一旦实例被更新——例如，当设置了新的恒定短期利率时——包含该贴现类特定实例的所有 `dx.market_environment` 类的实例都会自动更新。  
  
**灵活性**  
  
本节介绍的市场环境类是一种灵活的手段，可以对期权和衍生品以及由其组成的投资组合定价相关的任何数量和输入数据进行建模和存储。然而，这种灵活性也会带来操作风险，因为在实例化期间很容易向类传递无意义的数据、对象等，而这些可能会或可能不会在实例化期间被捕获。在生产环境中，需要添加大量检查来至少捕获明显错误的情况。  
  
## 结论  
  
本章为一个通过蒙特卡洛模拟对期权和其他衍生品进行估值的更大Python包的构建项目提供了基本框架。本章介绍了资产定价基本定理，并通过一个相当简单的数值示例对其进行了说明。在此方面，提供了针对离散时间下一般市场模型的重要结果。  
  
本章还出于风险中性贴现的目的开发了一个Python类，以便能够将资产定价基本定理的数学机制用于数值计算。基于一个由Python datetime对象或表示年化分数的浮点对象组成的列表对象，类 `dx.constant_short_rate` 的实例能提供合适的贴现因子（单位零息债券的现值）。  
  
本章最后给出了相当通用的 `dx.market_environment` 类，该类允许收集相关数据和Python对象，用于建模、模拟、估值和其他目的。  
  
为了简化未来的导入，此处使用了名为 `dx_frame.py` 的包装模块：  
  
```python  
#  
# DX Analytics Package (DX分析包)  
#  
# Frame Functions & Classes (框架函数与类)  
#  
# dx_frame.py  
#  
# Python for Finance, 2nd ed. (Python金融大数据分析，第二版)  
# (c) Dr. Yves J. Hilpisch  
#  
import datetime as dt # 导入datetime库作为dt  
  
from get_year_deltas import get_year_deltas # 导入get_year_deltas函数  
from constant_short_rate import constant_short_rate # 导入恒定短期利率类  
from market_environment import market_environment # 导入市场环境类  
```  
  
像下面这样的单一导入语句，就可以在一步操作中提供所有的框架组件：  
  
```python  
import dx_frame # 导入整合模块  
```  
  
在考虑构建包含多个模块的Python包时，还可以选择将所有相关Python模块存储在一个（子）文件夹中，并在该文件夹中放置一个特殊的 `__init__.py` 文件来执行所有导入。例如，当把所有模块都存入一个名为dx的文件夹中时，下面呈现的文件就能完成这项任务。不过，请注意这个特定文件的命名约定：  
  
```python  
#  
# DX Package (DX包)  
# packaging file (打包文件)  
# __init__.py  
#  
import datetime as dt # 导入datetime库作为dt  
  
from get_year_deltas import get_year_deltas # 从同目录导入对应模块函数  
from constant_short_rate import constant_short_rate # 从同目录导入对应模块类  
from market_environment import market_environment # 从同目录导入对应模块类  
```  
  
在那种情况下，您就可以直接使用文件夹名称一次性完成所有导入：  
  
```python  
from dx import * # 导入dx包下的所有组件  
```  
  
或者，通过替代方法：  
  
```python  
import dx # 直接导入dx包  
```  
  
## 进一步阅读  
  
对于本章涵盖的主题，以下是以书籍形式提供的有用参考资料：  
  
* Bittman, James (2009). 作为专业人士交易期权 (Trading Options as a Professional). 纽约: McGraw Hill.  
  
* Delbaen, Freddy, 以及 Walter Schachermayer (2004). 套利的数学 (The Mathematics of Arbitrage). 柏林, 海德堡: Springer-Verlag.  
  
* Fletcher, Shayne, 以及 Christopher Gardner (2009). Python中的金融建模 (Financial Modelling in Python). 奇切斯特, 英国: Wiley Finance.  
  
* Hilpisch, Yves (2015). 使用Python的衍生品分析 (Derivatives Analytics with Python). 奇切斯特, 英国: Wiley Finance.  
  
* Williams, David (1991). 结合鞅的概率论 (Probability with Martingales). 剑桥, 英国: 剑桥大学出版社.  
  
有关定义本章引用模型的原始研究论文，请参阅后续章节中的“进一步阅读”部分。  
