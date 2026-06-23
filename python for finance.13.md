# 第13章 统计学  
  
> 我可以用统计数据证明除了真理以外的任何东西。  
> ——乔治·坎宁 (George Canning)  
  
统计学是一个广阔的领域，但它提供的工具和结果已成为金融领域不可或缺的一部分。这解释了像R这样的特定领域语言在金融行业的流行。统计模型变得越精细和复杂，拥有可用且易于使用的高性能计算解决方案就越重要。  
  
像本书这样的单一一章无法充分展现统计学领域的丰富性和深度。因此，与许多其他章节一样，这里的方法是重点关注一些看似重要或在为特定任务使用Python时提供良好起点的精选主题。本章有四个重点：  
  
第398页的“正态性检验”  
大量重要的金融模型，如现代或均值-方差投资组合理论（MPT）以及资本资产定价模型（CAPM），都建立在证券收益率呈正态分布的假设之上。因此，本章介绍了检验给定时间序列收益率正态性的方法。  
  
第415页的“投资组合优化”  
MPT可以被认为是统计学在金融领域最大的成功之一。从20世纪50年代初先驱哈里·马科维茨（Harry Markowitz）的工作开始，该理论在金融市场资金投资方面，开始用严格的数学和统计方法取代人们对判断和经验的依赖。从这个意义上说，它也许是金融领域第一个真正的量化模型和方法。  
  
第429页的“贝叶斯统计”  
在概念层面上，贝叶斯统计将代理人的信念以及信念的更新概念引入了统计学。当涉及线性回归时，这可能表现为回归参数具有统计分布，而不是单一的点估计（例如，回归线的截距和斜率）。如今，贝叶斯方法在金融中被广泛使用，这也是为什么本节将基于一些示例来说明贝叶斯方法。  
  
第444页的“机器学习”  
机器学习（或统计学习）基于高级统计方法，被认为是人工智能（AI）的一个子学科。与统计学本身一样，机器学习提供了丰富的推断和模型集，可以从数据集中学习并基于所学内容进行预测。学习算法有不同的分类，例如监督学习或无监督学习算法。算法解决的问题类型也有所不同，例如估计或分类。本章展示的示例属于用于分类的监督学习类别。  
  
本章中的许多方面涉及日期和/或时间信息。有关使用Python、NumPy和pandas处理此类数据的概述，请参阅附录A。  
  
## 正态性检验  
  
正态分布可以说是金融中最重要的分布，也是金融理论的主要统计构建模块之一。其中，以下金融理论的基石在很大程度上依赖于金融工具的收益率呈正态分布的假设：[^1]  
  
投资组合理论  
当股票收益率呈正态分布时，最优投资组合的选择可以转换为仅考虑收益率的（预期）平均收益和方差（或波动率）以及不同股票之间的协方差即可做出投资决策（即最优投资组合构成）的设定。  
  
资本资产定价模型  
同样，当股票收益率呈正态分布时，单只股票的价格可以优雅地表达为与广泛市场指数的线性关系；这种关系通常通过一个衡量单只股票与市场指数联动性的指标来表达，该指标称为beta或 $\beta$ 。  
  
有效市场假说  
有效市场是指价格反映所有可用信息的市场，其中“所有”可以被定义得更窄或更广（例如，“所有公开可用”信息与还包括“仅私人可用”信息）。如果这个假设成立，那么股票价格是随机波动的，且收益率呈正态分布。  
  
期权定价理论  
布朗运动是对金融工具价格随机运动进行建模的基准模型；著名的布莱克-斯科尔斯-默顿（Black-Scholes-Merton）期权定价公式使用几何布朗运动作为股票随时间随机价格波动的模型，从而得出对数正态分布的价格和正态分布的收益率。  
  
这个远非详尽无遗的列表凸显了正态性假设在金融中的重要性。  
  
### 基准案例  
  
为了为进一步的分析奠定基础，分析从几何布朗运动开始，这是金融建模中使用的典型随机过程之一。关于来自几何布朗运动 $S$ 的路径特征，可以说以下几点：  
  
正态对数收益率  
两个时间 $0<s<t$ 之间的对数收益率 $\log\frac{S_t}{S_s}=\log{S_t}-\log{S_s}$ 呈正态分布。  
  
对数正态值  
在任何时间 $t>0$ ，值 $S_t$ 呈对数正态分布。  
  
在接下来的内容中，首先处理绘图设置。然后导入许多Python包，包括scipy.stats和statsmodels.api：  
  
```python  
In [1]: import math  
        import numpy as np  
        import scipy.stats as scs  
        import statsmodels.api as sm  
        from pylab import mpl, plt  
  
In [2]: plt.style.use('seaborn')  
        mpl.rcParams['font.family'] = 'serif'  
        %matplotlib inline  
```  
  
下面使用函数 `gen_paths()` 为几何布朗运动生成样本蒙特卡洛路径（另见第12章）：  
  
```python  
In [3]: def gen_paths(S0, r, sigma, T, M, I):  
            ''' 生成几何布朗运动的蒙特卡洛路径。  
              
            参数  
            ==========  
            S0: float  
                初始股票/指数值  
            r: float  
                恒定短期利率  
            sigma: float  
                恒定波动率  
            T: float  
                最终时间跨度  
            M: int  
                时间步数/区间数  
            I: int  
                要模拟的路径数  
                  
            返回  
            =======  
            paths: ndarray, shape (M + 1, I)  
                给定参数的模拟路径  
            '''  
            dt = T / M  
            paths = np.zeros((M + 1, I))  
            paths[0] = S0  
            for t in range(1, M + 1):  
                rand = np.random.standard_normal(I)  
                rand = (rand - rand.mean()) / rand.std() # 匹配一阶和二阶矩。  
                paths[t] = paths[t - 1] * np.exp((r - 0.5 * sigma ** 2) * dt +  
                                                 sigma * math.sqrt(dt) * rand) # 几何布朗运动的向量化欧拉离散化。  
            return paths  
```  
  
这里的模拟基于蒙特卡洛模拟的参数化，结合函数 `gen_paths()` 生成了250,000条路径，每条路径有50个时间步。图13-1显示了前10条模拟路径：  
  
```python  
In [4]: S0 = 100. # 模拟过程的初始值。  
        r = 0.05 # 恒定短期利率。  
        sigma = 0.2 # 恒定波动率因子。  
        T = 1.0 # 以年为单位的时间跨度。  
        M = 50 # 时间区间数。  
        I = 250000 # 模拟过程数。  
        np.random.seed(1000)  
  
In [5]: paths = gen_paths(S0, r, sigma, T, M, I)  
  
In [6]: S0 * math.exp(r * T) # 预期值与平均模拟值。  
```  
  
```txt  
Out[6]: 105.12710963760242  
```  
  
```python  
In [7]: paths[-1].mean() # 预期值与平均模拟值。  
```  
  
```txt  
Out[7]: 105.12645392478755  
```  
  
```python  
In [8]: plt.figure(figsize=(10, 6))  
        plt.plot(paths[:, :10])  
        plt.xlabel('time steps')  
        plt.ylabel('index level');  
```  
  
<img width="663" height="431" alt="python_for_finance 13 1" src="https://github.com/user-attachments/assets/8dbdfec8-9de3-4897-ba89-63581c103cc0" />
  
图13-1. 几何布朗运动的十条模拟路径  
  
主要的兴趣在于对数收益率的分布。为此，根据模拟路径创建一个包含所有对数收益率的ndarray对象。这里显示了单条模拟路径及由此产生的对数收益率：  
  
```python  
In [9]: paths[:, 0].round(4)  
```  
  
```txt  
Out[9]: array([100.    ,  97.821 ,  98.5573, 106.1546, 105.899 ,  99.8363,  
       100.0145, 102.6589, 105.6643, 107.1107, 108.7943, 108.2449,  
       106.4105, 101.0575, 102.0197, 102.6052, 109.6419, 109.5725,  
       112.9766, 113.0225, 112.5476, 114.5585, 109.942 , 112.6271,  
       112.7502, 116.3453, 115.0443, 113.9586, 115.8831, 117.3705,  
       117.9185, 110.5539, 109.9687, 104.9957, 108.0679, 105.7822,  
       105.1585, 104.3304, 108.4387, 105.5963, 108.866 , 108.3284,  
       107.0077, 106.0034, 104.3964, 101.0637,  98.3776,  97.135 ,  
        95.4254,  96.4271,  96.3386])  
```  
  
```python  
In [10]: log_returns = np.log(paths[1:] / paths[:-1])  
  
In [11]: log_returns[:, 0].round(4)  
```  
  
```txt  
Out[11]: array([-0.022 ,  0.0075,  0.0743, -0.0024, -0.059 ,  0.0018,  0.0261,  
        0.0289,  0.0136,  0.0156, -0.0051, -0.0171, -0.0516,  0.0095,  
        0.0057,  0.0663, -0.0006,  0.0306,  0.0004, -0.0042,  0.0177,  
       -0.0411,  0.0241,  0.0011,  0.0314, -0.0112, -0.0095,  0.0167,  
        0.0128,  0.0047, -0.0645, -0.0053, -0.0463,  0.0288, -0.0214,  
       -0.0059, -0.0079,  0.0386, -0.0266,  0.0305, -0.0049, -0.0123,  
       -0.0094, -0.0153, -0.0324, -0.0269, -0.0127, -0.0178,  0.0104,  
       -0.0009])  
```  
  
这也是人们在金融市场中可能会体验到的事情：有的日子里投资获得正收益，而有的日子里相较于最近的财富状况却在亏钱。  
  
函数 `print_statistics()` 是 scipy.stats 子包中 `scs.describe()` 函数的包装函数。它主要用于为诸如均值、偏度或峰度等统计量生成更好读的（人类可读的）输出，针对给定的（历史或模拟）数据集：  
  
```python  
In [13]: def print_statistics(array):  
            ''' 打印选定的统计数据。  
              
            参数  
            ==========  
            array: ndarray  
                用于生成统计数据的对象  
            '''  
            sta = scs.describe(array)  
            print('%14s %15s' % ('statistic', 'value'))  
            print(30 * '-')  
            print('%14s %15.5f' % ('size', sta[0]))  
            print('%14s %15.5f' % ('min', sta[1][0]))  
            print('%14s %15.5f' % ('max', sta[1][1]))  
            print('%14s %15.5f' % ('mean', sta[2]))  
            print('%14s %15.5f' % ('std', np.sqrt(sta[3])))  
            print('%14s %15.5f' % ('skew', sta[4]))  
            print('%14s %15.5f' % ('kurtosis', sta[5]))  
  
In [14]: print_statistics(log_returns.flatten())  
```  
  
```txt  
     statistic           value  
------------------------------  
          size  12500000.00000  
           min        -0.15664  
           max         0.15371  
          mean         0.00060  
           std         0.02828  
          skew         0.00055  
      kurtosis         0.00085  
```  
  
```python  
In [15]: log_returns.mean() * M + 0.5 * sigma ** 2 # 伊藤项修正后的年化平均对数收益率。[^2]  
```  
  
```txt  
Out[15]: 0.05000000000000005  
```  
  
```python  
In [16]: log_returns.std() * math.sqrt(M) # 年化波动率；即对数收益率的年化标准差。  
```  
  
```txt  
Out[16]: 0.20000000000000015  
```  
  
本例中的数据集由12,500,000个数据点组成，值主要分布在 +/- 0.15 之间。人们会预期平均收益率（在修正了伊藤项之后）的年化值为0.05，标准差（波动率）为0.2。年化值几乎完美地匹配了这些值（将平均值乘以50并针对伊藤项进行修正；将标准差乘以 $\sqrt{50}$ ）。匹配度良好的原因之一是在提取随机数时使用矩匹配来减少方差（见第372页的“方差缩减”）。  
  
图13-2对比了给定 r 和 sigma 参数化情况下，模拟对数收益率的频率分布与正态分布的概率密度函数（PDF）。所使用的函数是scipy.stats子包中的 `norm.pdf()` 。显然存在相当好的拟合：  
  
```python  
In [17]: plt.figure(figsize=(10, 6))  
         plt.hist(log_returns.flatten(), bins=70, density=True,  
                  label='frequency', color='b')  
         plt.xlabel('log return')  
         plt.ylabel('frequency')  
         x = np.linspace(plt.axis()[0], plt.axis()[1])  
         plt.plot(x, scs.norm.pdf(x, loc=r / M, scale=sigma / np.sqrt(M)),  
                  'r', lw=2.0, label='pdf') # 绘制按照区间长度缩放的假定参数的PDF。  
         plt.legend();  
```  
  
<img width="664" height="460" alt="python_for_finance 13 2" src="https://github.com/user-attachments/assets/662af82b-92fa-405d-af10-71e40270e754" />
  
图13-2. 几何布朗运动的对数收益率直方图与正态密度函数  
  
将频率分布（直方图）与理论PDF进行比较并不是图形化“检验”正态性的唯一方法。所谓的QQ（分位数-分位数）图也非常适合这项任务。在这里，将样本分位数值与理论分位数值进行比较。对于呈正态分布的样本数据集，这种图可能如图13-3所示，绝大多数分位数值（点）位于一条直线上：  
  
```python  
In [18]: sm.qqplot(log_returns.flatten()[::500], line='s')  
         plt.xlabel('theoretical quantiles')  
         plt.ylabel('sample quantiles');  
```  
  
<img width="664" height="460" alt="python_for_finance 13 3" src="https://github.com/user-attachments/assets/6e7e6339-9967-44fe-b6f8-83718968a6e9" />
  
图13-3. 几何布朗运动对数收益率的QQ图  
  
尽管图形方法很有吸引力，但它们通常无法取代更严格的检验程序。在下一个示例中使用的函数 `normality_tests()` 结合了三种不同的统计检验：  
  
偏度检验 (skewtest())  
检验样本数据的偏度是否“正常”（即其值是否足够接近于零）。  
  
峰度检验 (kurtosistest())  
类似地，检验样本数据的峰度是否“正常”（同样，是否足够接近于零）。  
  
正态性检验 (normaltest())  
该方法结合了其他两种检验方法来进行正态性检验。  
  
检验值表明几何布朗运动的对数收益率确实呈正态分布——即它们的 $p$ 值显示为0.05或更高：  
  
```python  
In [19]: def normality_tests(arr):  
             ''' 对给定数据集进行正态分布检验。  
               
             参数  
             ==========  
             array: ndarray  
                 用于生成统计数据的对象  
             '''  
             print('Skew of data set %14.3f' % scs.skew(arr))  
             print('Skew test p-value %14.3f' % scs.skewtest(arr)[1])  
             print('Kurt of data set %14.3f' % scs.kurtosis(arr))  
             print('Kurt test p-value %14.3f' % scs.kurtosistest(arr)[1])  
             print('Norm test p-value %14.3f' % scs.normaltest(arr)[1])  
  
In [20]: normality_tests(log_returns.flatten()) # 所有p值都远高于0.05。  
```  
  
```txt  
Skew of data set          0.001  
Skew test p-value          0.430  
Kurt of data set          0.001  
Kurt test p-value          0.541  
Norm test p-value          0.607  
```  
  
最后，检查期末值是否确实呈现对数正态分布。这归结为一个正态性检验，因为只需将对数函数应用于数据即可对其进行转换，从而得出正态分布的值（也许不是）。图13-4同时绘制了对数正态分布的期末值以及转换后的期末值（“对数指数水平”）：  
  
```python  
In [21]: f, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 6))  
         ax1.hist(paths[-1], bins=30)  
         ax1.set_xlabel('index level')  
         ax1.set_ylabel('frequency')  
         ax1.set_title('regular data')  
         ax2.hist(np.log(paths[-1]), bins=30)  
         ax2.set_xlabel('log index level')  
         ax2.set_title('log data')  
```  
  
<img width="664" height="456" alt="python_for_finance 13 4" src="https://github.com/user-attachments/assets/cc611fc0-f052-4b83-9740-f22e2ad8a276" />
  
图13-4. 几何布朗运动的模拟期末指数水平直方图  
  
数据集的统计数据表现出预期的行为——例如，平均值接近105。对数指数水平值的偏度和峰度值接近于零，并且显示出很高的 $p$ 值，为正态分布假设提供了强有力的支持：  
  
```python  
In [22]: print_statistics(paths[-1])  
```  
  
```txt  
     statistic           value  
------------------------------  
          size   250000.00000  
           min       42.74870  
           max      233.58435  
          mean      105.12645  
           std       21.23174  
          skew        0.61116  
      kurtosis        0.65182  
```  
  
```python  
In [23]: print_statistics(np.log(paths[-1]))  
```  
  
```txt  
     statistic           value  
------------------------------  
          size   250000.00000  
           min        3.75534  
           max        5.45354  
          mean        4.63517  
           std        0.19998  
          skew       -0.00092  
      kurtosis       -0.00327  
```  
  
```python  
In [24]: normality_tests(np.log(paths[-1]))  
```  
  
```txt  
Skew of data set         -0.001  
Skew test p-value         0.851  
Kurt of data set         -0.003  
Kurt test p-value         0.744  
Norm test p-value         0.931  
```  
  
图13-5再次将频率分布与正态分布的PDF进行了比较，显示出非常好的拟合（正如现在理所当然预期的那样）：  
  
```python  
In [25]: plt.figure(figsize=(10, 6))  
         log_data = np.log(paths[-1])  
         plt.hist(log_data, bins=70, density=True,  
                  label='observed', color='b')  
         plt.xlabel('index levels')  
         plt.ylabel('frequency')  
         x = np.linspace(plt.axis()[0], plt.axis()[1])  
         plt.plot(x, scs.norm.pdf(x, log_data.mean(), log_data.std()),  
                  'r', lw=2.0, label='pdf')  
         plt.legend();  
```  
  
<img width="664" height="450" alt="python_for_finance 13 5" src="https://github.com/user-attachments/assets/443f5f45-b221-4e3e-82a0-fb94aef64ab4" />
  
图13-5. 几何布朗运动的对数指数水平直方图与正态密度函数  
  
图13-6也支持对数指数水平呈正态分布的假设：  
  
```python  
In [26]: sm.qqplot(log_data, line='s')  
         plt.xlabel('theoretical quantiles')  
         plt.ylabel('sample quantiles');  
```  
  
<img width="664" height="469" alt="python_for_finance 13 6" src="https://github.com/user-attachments/assets/b9cf15de-a341-4e57-b3c3-e6d8ea395a33" />
  
图13-6. 几何布朗运动的对数指数水平QQ图  
  
**正态性**  
关于金融工具不确定收益的正态性假设是许多金融理论的核心。Python提供了高效的统计和图形手段来检验时间序列数据是否呈正态分布。  
  
## 真实世界数据  
  
本节分析四个历史金融时间序列数据，两个用于科技股，两个用于交易所交易基金（ETF）：  
  
* AAPL.O: 苹果公司股票价格  
* MSFT.O: 微软公司股票价格  
* SPY: SPDR标普500 ETF信托  
* GLD: SPDR黄金信托  
  
数据管理的优选工具是pandas（见第8章）。图13-7显示了随时间推移的归一化价格：  
  
```python  
In [27]: import pandas as pd  
  
In [28]: raw = pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                           index_col=0, parse_dates=True).dropna()  
  
In [29]: symbols = ['SPY', 'GLD', 'AAPL.O', 'MSFT.O']  
  
In [30]: data = raw[symbols]  
         data = data.dropna()  
  
In [31]: data.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 2138 entries, 2010-01-04 to 2018-06-29  
Data columns (total 4 columns):  
SPY       2138 non-null float64  
GLD       2138 non-null float64  
AAPL.O    2138 non-null float64  
MSFT.O    2138 non-null float64  
dtypes: float64(4)  
memory usage: 83.5 KB  
```  
  
```python  
In [32]: data.head()  
```  
  
```txt  
Out[32]:              SPY     GLD     AAPL.O  MSFT.O  
Date  
2010-01-04  113.33  109.80  30.572827  30.950  
2010-01-05  113.63  109.70  30.625684  30.960  
2010-01-06  113.71  111.51  30.138541  30.770  
2010-01-07  114.19  110.82  30.082827  30.452  
2010-01-08  114.57  111.37  30.282827  30.660  
```  
  
```python  
In [33]: (data / data.iloc[0] * 100).plot(figsize=(10, 6))  
```  
  
<img width="666" height="419" alt="python_for_finance 13 7" src="https://github.com/user-attachments/assets/7b83eb25-ceda-45fd-b737-15de194aceb1" />
  
图13-7. 随时间推移金融工具的归一化价格  
  
图13-8以直方图的形式显示了这些金融工具的对数收益率：  
  
```python  
In [34]: log_returns = np.log(data / data.shift(1))  
         log_returns.head()  
```  
  
```txt  
Out[34]:                 SPY       GLD    AAPL.O    MSFT.O  
Date  
2010-01-04       NaN       NaN       NaN       NaN  
2010-01-05  0.002644 -0.000911  0.001727  0.000323  
2010-01-06  0.000704  0.016365 -0.016034 -0.006156  
2010-01-07  0.004212 -0.006207 -0.001850 -0.010389  
2010-01-08  0.003322  0.004951  0.006626  0.006807  
```  
  
```python  
In [35]: log_returns.hist(bins=50, figsize=(10, 8));  
```  
  
<img width="665" height="559" alt="python_for_finance 13 8" src="https://github.com/user-attachments/assets/b6c28973-9f96-4cc8-af34-7aa01d566bee" />
  
图13-8. 金融工具对数收益率的直方图  
  
接下来，考察这几个时间序列数据集的不同统计数据。所有四个数据集的峰度值似乎都远偏离正态分布：  
  
```python  
In [36]: for sym in symbols:  
             print('\nResults for symbol {}'.format(sym))  
             print(30 * '-')  
             log_data = np.array(log_returns[sym].dropna())  
             print_statistics(log_data) # 金融工具时间序列的统计数据。  
```  
  
```txt  
Results for symbol SPY  
------------------------------  
     statistic           value  
------------------------------  
          size      2137.00000  
           min        -0.06734  
           max         0.04545  
          mean         0.00041  
           std         0.00933  
          skew        -0.52189  
      kurtosis         4.52432  
  
Results for symbol GLD  
------------------------------  
     statistic           value  
------------------------------  
          size      2137.00000  
           min        -0.09191  
           max         0.04795  
          mean         0.00004  
           std         0.01020  
          skew        -0.59934  
      kurtosis         5.68423  
  
Results for symbol AAPL.O  
------------------------------  
     statistic           value  
------------------------------  
          size      2137.00000  
           min        -0.13187  
           max         0.08502  
          mean         0.00084  
           std         0.01591  
          skew        -0.23510  
      kurtosis         4.78964  
  
Results for symbol MSFT.O  
------------------------------  
     statistic           value  
------------------------------  
          size      2137.00000  
           min        -0.12103  
           max         0.09941  
          mean         0.00054  
           std         0.01421  
          skew        -0.09117  
      kurtosis         7.29106  
```  
  
图13-9显示了SPY ETF的QQ图。显然，样本分位数值并不在一条直线上，表明存在“非正态性”。在左侧和右侧，分别有许多值远低于该直线和远高于该直线。换句话说，时间序列数据表现出厚尾分布（fat tails）。该术语指的是这样一种（频率）分布：观察到极大负值和极大正值的次数比正态分布所暗示的要多。从图13-10（展示了微软股票的数据）中也可以得出相同的结论。那里似乎也有厚尾分布的证据：  
  
```python  
In [37]: sm.qqplot(log_returns['SPY'].dropna(), line='s')  
         plt.title('SPY')  
         plt.xlabel('theoretical quantiles')  
         plt.ylabel('sample quantiles');  
  
In [38]: sm.qqplot(log_returns['MSFT.O'].dropna(), line='s')  
         plt.title('MSFT.O')  
         plt.xlabel('theoretical quantiles')  
         plt.ylabel('sample quantiles');  
```  
  
<img width="666" height="488" alt="python_for_finance 13 9" src="https://github.com/user-attachments/assets/98689f3a-ce2a-4fc1-9368-c8c68885f3a1" />
  
图13-9. SPY对数收益率的QQ图  
  
<img width="667" height="487" alt="python_for_finance 13 10" src="https://github.com/user-attachments/assets/28c2f84b-c82d-41ee-b17e-2c17ef3eb823" />
  
图13-10. MSFT.O对数收益率的QQ图  
  
这最终引向统计正态性检验：  
  
```python  
In [39]: for sym in symbols:  
             print('\nResults for symbol {}'.format(sym))  
             print(32 * '-')  
             log_data = np.array(log_returns[sym].dropna())  
             normality_tests(log_data) # 金融工具时间序列的正态性检验结果。  
```  
  
```txt  
Results for symbol SPY  
--------------------------------  
Skew of data set         -0.522  
Skew test p-value         0.000  
Kurt of data set          4.524  
Kurt test p-value         0.000  
Norm test p-value         0.000  
  
Results for symbol GLD  
--------------------------------  
Skew of data set         -0.599  
Skew test p-value         0.000  
Kurt of data set          5.684  
Kurt test p-value         0.000  
Norm test p-value         0.000  
  
Results for symbol AAPL.O  
--------------------------------  
Skew of data set         -0.235  
Skew test p-value         0.000  
Kurt of data set          4.790  
Kurt test p-value         0.000  
Norm test p-value         0.000  
  
Results for symbol MSFT.O  
--------------------------------  
Skew of data set         -0.091  
Skew test p-value         0.085  
Kurt of data set          7.291  
Kurt test p-value         0.000  
Norm test p-value         0.000  
```  
  
不同检验的 $p$ 值全为零，强烈拒绝了不同样本数据集呈正态分布的检验假设。这表明股票市场收益和其他资产类别（例如体现在几何布朗运动模型中的资产）的正态假设在一般情况下是站不住脚的，并且人们可能必须使用能够生成厚尾分布的更丰富的模型（例如跳跃扩散模型或具有随机波动率的模型）。  
  
## 投资组合优化  
  
现代或均值-方差投资组合理论是金融理论的一个重要基石。基于这一理论突破，诺贝尔经济学奖于1990年授予了它的发明者哈里·马科维茨。尽管该理论是在20世纪50年代提出的，但它仍然是向金融专业学生传授并在当今实践中应用的理论（通常带有一些微小或重大的修改）。[^3]本节说明该理论的基本原理。  
  
Copeland、Weston和Shastri（2005）所著书的第5章介绍了与MPT相关的正式主题。如前所述，收益率呈正态分布的假设是该理论的基础：  
  
> 仅通过观察均值和方差，我们必然假设不需要其他统计数据来描述期末财富的分布。除非投资者有一种特殊类型的效用函数（二次效用函数），否则有必要假设收益率具有正态分布，这种分布可以完全用均值和方差来描述。  
  
### 数据  
  
接下来的分析和示例使用了与前面相同的金融工具。MPT的基本思想是利用分散化（diversification），在给定目标收益水平下实现最小的投资组合风险，或者在给定一定风险水平下实现最大的投资组合收益。人们会期望在正确组合大量资产并使资产具备一定多样性的情况下产生这种分散化效应。然而，为了传达基本思想并显示典型的效应，四种金融工具便足够了。图13-11显示了这些金融工具对数收益率的频率分布：  
  
```python  
In [40]: symbols = ['AAPL.O', 'MSFT.O', 'SPY', 'GLD'] # 定义用于投资组合构成的四种金融工具。  
  
In [41]: noa = len(symbols) # 定义金融工具的数量。  
  
In [42]: data = raw[symbols]  
  
In [43]: rets = np.log(data / data.shift(1))  
  
In [44]: rets.hist(bins=40, figsize=(10, 8));  
```  
  
待投资金融工具的协方差矩阵是投资组合选择过程的核心部分。pandas有一个内置的方法来生成协方差矩阵，上面应用了相同的缩放因子：  
  
```python  
In [45]: rets.mean() * 252 # 年化平均收益率。  
```  
  
```txt  
Out[45]: AAPL.O    0.212359  
MSFT.O    0.136648  
SPY       0.102928  
GLD       0.009141  
dtype: float64  
```  
  
```python  
In [46]: rets.cov() * 252 # 年化协方差矩阵。  
```  
  
```txt  
Out[46]:           AAPL.O    MSFT.O       SPY       GLD  
AAPL.O  0.063773  0.023427  0.021039  0.001513  
MSFT.O  0.023427  0.050917  0.022244 -0.000347  
SPY     0.021039  0.022244  0.021939  0.000062  
GLD     0.001513 -0.000347  0.000062  0.026209  
```  
  
<img width="667" height="559" alt="python_for_finance 13 11" src="https://github.com/user-attachments/assets/53138b70-8125-49cf-9f30-b4725368ee2b" />
  
图13-11. 金融工具对数收益率直方图  
  
### 基本理论  
  
在接下来的内容中，假设不允许投资者在金融工具中建立空头头寸。仅允许做多头寸，这意味着投资者的100%财富必须分配到可用工具中，使得所有头寸均为多头（正数），且头寸总和为100%。给定这四种工具，例如，人们可以在每种工具中投入相等的金额——即在每种工具中投入25%的可用财富。下面的代码生成四个0到1之间均匀分布的随机数，然后将值进行归一化，使得所有值的总和等于1：  
  
```python  
In [47]: weights = np.random.random(noa) # 随机的投资组合权重...  
         weights /= np.sum(weights) # ...归一化为1或100%。  
  
In [48]: weights  
```  
  
```txt  
Out[48]: array([0.07650728, 0.06021919, 0.63364218, 0.22963135])  
```  
  
```python  
In [49]: weights.sum()  
```  
  
```txt  
Out[49]: 1.0  
```  
  
正如这里验证的那样，这些权重的确加起来等于1；即 $\sum_I w_i = 1$ ，其中 $I$ 是金融工具的数量，且 $w_i > 0$ 是金融工具 $i$ 的权重。公式13-1提供了在给定单个工具权重的情况下计算预期投资组合收益率的公式。这是一个意义上的预期投资组合收益率：即假定历史平均表现是对未来（预期）表现的最佳估计。这里， $r_i$ 是状态依赖的未来收益率（具有假设为正态分布的收益率值的向量）， $\mu_i$ 是工具 $i$ 的预期收益率。最后， $w^T$ 是权重向量的转置， $\mu$ 是预期证券收益率的向量。  
  
公式 13-1. 预期投资组合收益率的通用公式  
$$\mu_p=\text{E}(\sum_I w_i r_i)=\sum_I w_i \text{E}(r_i)=\sum_I w_i \mu_i=w^T\mu$$  
  
转化为Python后，归结为一行包含年化的代码：  
  
```python  
In [50]: np.sum(rets.mean() * weights) * 252 # 给定投资组合权重计算出的年化投资组合收益率。  
```  
  
```txt  
Out[50]: 0.09179459482057793  
```  
  
MPT中第二个重要的对象是预期的投资组合方差。两种证券之间的协方差定义为 $\sigma_{ij}=\sigma_{ji}=\text{E}(r_i-\mu_i)(r_j-\mu_j)$ 。一种证券的方差是与自身协方差的特例： $\sigma_i^2=\text{E}((r_i-\mu_i)^2)$ 。图13-12提供了证券组合的协方差矩阵（假设每种证券的权重相等为1）。  
  
<img width="666" height="209" alt="python_for_finance 13 12" src="https://github.com/user-attachments/assets/c573aa00-f897-4334-9dc8-887b7b8f385f" />
  
图13-12. 投资组合协方差矩阵  
  
配备了投资组合协方差矩阵后，公式13-2随后提供了预期投资组合方差的公式。  
  
公式 13-2. 预期投资组合方差的通用公式  
$$\sigma_p^2=\text{E}((r-\mu)^2) \\ = \sum_{i\in I}\sum_{j\in I}w_i w_j \sigma_{ij} \\ = w^T \Sigma w$$  
  
在Python中，这再次归结为一行代码，大量使用了NumPy的向量化能力。 `np.dot()` 函数给出两个向量/矩阵的点积。 `T` 属性或 `transpose()` 方法给出了向量或矩阵的转置。给定投资组合方差，（预期的）投资组合标准差或波动率 $\sigma_p=\sqrt{\sigma_p^2}$ 就只需再开一次平方根即可：  
  
```python  
In [51]: np.dot(weights.T, np.dot(rets.cov() * 252, weights)) # 给定投资组合权重计算出的年化投资组合方差。  
```  
  
```txt  
Out[51]: 0.014763288666485574  
```  
  
```python  
In [52]: math.sqrt(np.dot(weights.T, np.dot(rets.cov() * 252, weights))) # 给定投资组合权重计算出的年化投资组合波动率。  
```  
  
```txt  
Out[52]: 0.12150427427249452  
```  
  
**Python 和向量化**  
MPT示例展示了使用Python将诸如投资组合收益率或投资组合方差等数学概念转化为可执行的向量化代码是多么高效（在第1章中提出了这个论点）。  
  
这基本完成了用于均值-方差投资组合选择的工具集。对于投资者来说，最重要的是：对于给定的一组金融工具，可以有哪些风险-收益曲线，以及它们的统计特征是什么。为此，下面实现了一个蒙特卡洛模拟（见第12章），以更大规模地生成随机投资组合权重向量。对于每次模拟分配，代码都会记录产生的预期投资组合收益率和方差。为了简化代码，定义了两个函数 `port_ret()` 和 `port_vol()` ：  
  
```python  
In [53]: def port_ret(weights):  
             return np.sum(rets.mean() * weights) * 252  
  
In [54]: def port_vol(weights):  
             return np.sqrt(np.dot(weights.T, np.dot(rets.cov() * 252, weights)))  
  
In [55]: prets = []  
         pvols = []  
         for p in range (2500): # 投资组合权重的蒙特卡洛模拟。  
             weights = np.random.random(noa)   
             weights /= np.sum(weights)   
             prets.append(port_ret(weights))   
             pvols.append(port_vol(weights))   
         prets = np.array(prets) # 将结果统计数据收集到列表对象中。  
         pvols = np.array(pvols) # 将结果统计数据收集到列表对象中。  
```  
  
图13-13展示了蒙特卡洛模拟的结果。此外，它还提供了夏普比率（Sharpe ratio）的结果，定义为 $\text{SR}\equiv\frac{\mu_p-r_f}{\sigma_p}$ ——即投资组合超过无风险短期利率 $r_f$ 的预期超额收益，除以投资组合的预期标准差。为简单起见，假设 $r_f\equiv 0$ ：  
  
```python  
In [56]: plt.figure(figsize=(10, 6))  
         plt.scatter(pvols, prets, c=prets / pvols,  
                     marker='o', cmap='coolwarm')  
         plt.xlabel('expected volatility')  
         plt.ylabel('expected return')  
         plt.colorbar(label='Sharpe ratio');  
```  
  
<img width="665" height="447" alt="python_for_finance 13 13" src="https://github.com/user-attachments/assets/433733e4-01b2-40a5-9cff-a8234d6ede75" />
  
图13-13. 随机投资组合权重的预期收益与波动率  
  
通过观察图13-13可以清楚地看出，在按照均值和波动率衡量时，并非所有的权重分布表现都很好。例如，对于固定的风险水平（比如15%），存在多个收益率不同的投资组合。作为投资者，人们通常感兴趣的是在固定风险水平下的最大收益，或在固定预期收益下的最小风险。这组投资组合便构成了所谓的有效前沿。这将在本节稍后推导得出。  
  
### 最优投资组合  
  
这种最小化函数具有很强的通用性，并允许为参数设置等式约束、不等式约束以及数值边界。  
  
首先是最大化夏普比率。形式上，最小化夏普比率的负值以得出最大值和最优投资组合构成。约束条件是所有参数（权重）的总和为1。使用 `minimize()` 函数的约定，可以按如下公式化表达。[^4] 参数值（权重）的边界也被限制在0和1之间。这些值作为元组的元组提供给最小化函数。  
  
要调用优化函数，唯一缺少的输入就是启动参数列表（对于权重向量的初始猜测）。权重的平均分配就可以：  
  
```python  
In [57]: import scipy.optimize as sco  
  
In [58]: def min_func_sharpe(weights): # 待最小化的函数。  
             return -port_ret(weights) / port_vol(weights)   
  
In [59]: cons = ({'type': 'eq', 'fun': lambda x: np.sum(x) - 1}) # 等式约束。  
  
In [60]: bnds = tuple((0, 1) for x in range(noa)) # 参数的边界。  
  
In [61]: eweights = np.array(noa * [1. / noa,]) # 等权重向量。  
         eweights   
```  
  
```txt  
Out[61]: array([0.25, 0.25, 0.25, 0.25])  
```  
  
```python  
In [62]: min_func_sharpe(eweights)  
```  
  
```txt  
Out[62]: -0.8436203363155397  
```  
  
调用该函数返回的不仅仅是最优参数值。结果存储在一个名为 `opts` 的对象中。主要的兴趣在于获得最优的投资组合构成。为此，我们可以通过提供感兴趣的键（在这个例子中是 `x`）来访问结果对象：  
  
```python  
In [63]: %%time  
         opts = sco.minimize(min_func_sharpe, eweights,  
                             method='SLSQP', bounds=bnds,  
                             constraints=cons) # 1  
         CPU times: user 67.6 ms, sys: 1.94 ms, total: 69.6 ms  
         Wall time: 75.2 ms  
  
In [64]: opts # 2  
```  
  
```txt  
Out[64]:      fun: -0.8976673894052725  
     jac: array([ 8.96826386e-05,  8.30739737e-05, -2.45958567e-04,  
        1.92895532e-05])  
 message: 'Optimization terminated successfully.'  
    nfev: 36  
     nit: 6  
    njev: 6  
  status: 0  
 success: True  
       x: array([0.51191354, 0.19126414, 0.25454109, 0.04228123])  
```  
  
```python  
In [65]: opts['x'].round(3) # 3  
```  
  
```txt  
Out[65]: array([0.512, 0.191, 0.255, 0.042])  
```  
  
```python  
In [66]: port_ret(opts['x']).round(3) # 4  
```  
  
```txt  
Out[66]: 0.161  
```  
  
```python  
In [67]: port_vol(opts['x']).round(3) # 5  
```  
  
```txt  
Out[67]: 0.18  
```  
  
```python  
In [68]: port_ret(opts['x']) / port_vol(opts['x']) # 6  
```  
  
```txt  
Out[68]: 0.8976673894052725  
```  
  
(在上述代码中：1 表示优化（即最小化 `min_func_sharpe()` 函数）；2 表示优化的结果；3 表示最优投资组合权重；4 表示由此产生的投资组合收益；5 表示由此产生的投资组合波动率；6 表示最大夏普比率。)  
  
接下来，是投资组合方差的最小化。这等同于最小化波动率：  
  
```python  
In [69]: optv = sco.minimize(port_vol, eweights,  
                             method='SLSQP', bounds=bnds,  
                             constraints=cons) # 1  
  
In [70]: optv  
```  
  
```txt  
Out[70]:      fun: 0.1094215526341138  
     jac: array([0.11098004, 0.10948556, 0.10939826, 0.10944918])  
 message: 'Optimization terminated successfully.'  
    nfev: 54  
     nit: 9  
    njev: 9  
  status: 0  
 success: True  
       x: array([1.62630326e-18, 1.06170720e-03, 5.43263079e-01,  
       4.55675214e-01])  
```  
  
```python  
In [71]: optv['x'].round(3)  
```  
  
```txt  
Out[71]: array([0.   , 0.001, 0.543, 0.456])  
```  
  
```python  
In [72]: port_vol(optv['x']).round(3)  
```  
  
```txt  
Out[72]: 0.109  
```  
  
```python  
In [73]: port_ret(optv['x']).round(3)  
```  
  
```txt  
Out[73]: 0.06  
```  
  
```python  
In [74]: port_ret(optv['x']) / port_vol(optv['x'])  
```  
  
```txt  
Out[74]: 0.5504173653075624  
```  
  
（上面的 `In [69]` 中的 1 代表投资组合波动率的最小化。）  
  
这一次，投资组合仅由三种金融工具组成。这种投资组合的组合导致了所谓的最小波动率或最小方差投资组合。  
  
### 有效前沿  
  
对于所有最优投资组合（即，对于给定目标收益水平下具有最小波动率的所有投资组合，或者对于给定风险水平下具有最大收益率的所有投资组合）的推导与先前的优化类似。唯一的区别是人们必须遍历多个起始条件。  
  
所采取的方法是固定目标收益水平，然后针对每个水平推导出能导致最小波动率值的投资组合权重。对于该优化过程，这导致了两个约束条件：一个针对目标收益水平 `tret` ，另一个与之前一样针对投资组合权重的总和。每个参数的边界值保持不变。当遍历不同目标收益水平（`trets`）时，最小化函数中的一个约束条件发生改变。这就是为什么每次循环都会更新约束字典的原因：  
  
```python  
In [75]: cons = ({'type': 'eq', 'fun': lambda x:  port_ret(x) - tret},  
                 {'type': 'eq', 'fun': lambda x:  np.sum(x) - 1}) # 有效前沿的两个具有约束力的条件。  
  
In [76]: bnds = tuple((0, 1) for x in weights)  
  
In [77]: %%time  
         trets = np.linspace(0.05, 0.2, 50)  
         tvols = []  
         for tret in trets:  
             res = sco.minimize(port_vol, eweights, method='SLSQP',  
                                bounds=bnds, constraints=cons) # 不同目标收益下的投资组合波动率最小化。  
             tvols.append(res['fun'])  
         tvols = np.array(tvols)  
         CPU times: user 2.6 s, sys: 13.1 ms, total: 2.61 s  
         Wall time: 2.66 s  
```  
  
图13-14显示了优化结果。粗线表示给定特定目标收益率下的最优投资组合；与以前一样，那些点代表随机投资组合。此外，该图还显示了两颗较大的星号，一颗代表最小波动率/方差投资组合（最左边的投资组合），另一颗代表具有最大夏普比率的投资组合：  
  
```python  
In [78]: plt.figure(figsize=(10, 6))  
         plt.scatter(pvols, prets, c=prets / pvols,  
                     marker='.', alpha=0.8, cmap='coolwarm')  
         plt.plot(tvols, trets, 'b', lw=4.0)  
         plt.plot(port_vol(opts['x']), port_ret(opts['x']),  
                  'y*', markersize=15.0)  
         plt.plot(port_vol(optv['x']), port_ret(optv['x']),  
                  'r*', markersize=15.0)  
         plt.xlabel('expected volatility')  
         plt.ylabel('expected return')  
         plt.colorbar(label='Sharpe ratio')  
```  
  
<img width="667" height="445" alt="python_for_finance 13 14" src="https://github.com/user-attachments/assets/dda00525-5ba7-4c61-9b01-95434bfe209d" />
  
图13-14. 给定收益水平下的最小风险投资组合（有效前沿）  
  
有效前沿包含所有预期收益高于绝对最小方差投资组合的最优投资组合。在给定一定风险水平的情况下，这些投资组合在预期收益方面支配了所有其他投资组合。  
  
### 资本市场线  
  
除了股票或大宗商品（如黄金）等有风险的金融工具外，通常还存在一种普遍的、无风险的投资机会：现金或现金账户。在一个理想的世界中，大型银行现金账户中的资金可以被认为是无风险的（例如，通过公共存款保险计划）。缺点是这种无风险投资通常只产生极小的回报，有时接近于零。  
  
然而，考虑到这种无风险资产，极大地增强了投资者的有效投资机会集。基本理念是，投资者首先确定有风险资产的有效投资组合，然后将无风险资产加入组合中。通过调整投资者要投资在无风险资产中的财富比例，有可能实现位于风险-收益空间中代表无风险资产的点和有效投资组合这两点连成直线上的任何风险-收益曲线。  
  
应选择哪个有效投资组合（在众多选项中）以实现最优投资？即选择那个其有效前沿的切线恰好经过无风险投资组合在风险-收益图上的那个有效投资组合。例如，考虑无风险利率 $r_f=0.01$ 。该投资组合将出现在有效前沿上，其切线穿过风险-收益空间中的点 $(\sigma_f, r_f)=(0, 0.01)$ 。  
  
在接下来的计算中，将使用有效前沿的函数近似以及它的一阶导数。三次样条插值提供了这种可微函数的近似（见第11章）。对于样条插值，仅使用来自有效前沿的投资组合。通过这种数值方法，可以为有效前沿定义一个连续可微函数 `f(x)` 以及相应的一阶导数函数 `df(x)`：  
  
```python  
In [79]: import scipy.interpolate as sci  
  
In [80]: ind = np.argmin(tvols) # 最小波动率投资组合的索引位置。  
         evols = tvols[ind:] # 相关投资组合的波动率和收益率值。  
         erets = trets[ind:] # 相关投资组合的波动率和收益率值。  
  
In [81]: tck = sci.splrep(evols, erets) # 对这些值进行三次样条插值。  
  
In [82]: def f(x):  
             ''' 有效前沿函数（样条近似）。 '''  
             return sci.splev(x, tck, der=0)  
         def df(x):  
             ''' 有效前沿函数的一阶导数。 '''  
             return sci.splev(x, tck, der=1)  
```  
  
现在要推导的是一个线性函数 $t(x)=a+b\cdot x$ ，代表一条在风险-收益空间中穿过无风险资产点并且与有效前沿相切的线。公式13-3描述了该函数 $t(x)$ 必须满足的三个条件。  
  
公式 13-3. 资本市场线的数学条件  
$$t(x)=a+b\cdot x \\ t(0)=r_f \Leftrightarrow a=r_f \\ t(x)=f(x) \Leftrightarrow a+b\cdot x=f(x) \\ t'(x)=f'(x) \Leftrightarrow b=f'(x)$$  
  
由于有效前沿或其一阶导数没有封闭公式，人们必须对公式13-3中的方程组进行数值求解。为此，定义一个Python函数，该函数在给定参数集 $p=(a, b, x)$ 的情况下返回所有三个方程式的值。  
  
来自scipy.optimize的 `sco.fsolve()` 函数能够求解这样的方程组。除了提供函数 `equations()` 之外，还提供了一个初始参数化设置。请注意，优化成功或失败可能取决于该初始参数化，因此必须仔细选择该初始化——通常是通过经验猜测与试错相结合。  
  
```python  
In [83]: def equations(p, rf=0.01):  
             eq1 = rf - p[0] # 描述资本市场线（CML）的方程式。  
             eq2 = rf + p[1] * p[2] - f(p[2]) # 描述资本市场线（CML）的方程式。  
             eq3 = p[1] - df(p[2]) # 描述资本市场线（CML）的方程式。  
             return eq1, eq2, eq3  
  
In [84]: opt = sco.fsolve(equations, [0.01, 0.5, 0.15]) # 求解这些方程式的给定初始值。  
  
In [85]: opt # 最优参数值。  
```  
  
```txt  
Out[85]: array([0.01      , 0.84470952, 0.19525391])  
```  
  
```python  
In [86]: np.round(equations(opt), 6) # 所有方程式值均为零。  
```  
  
```txt  
Out[86]: array([ 0.,  0., -0.])  
```  
  
图13-15以图形方式显示了结果；星号代表来自有效前沿的最优投资组合，其切线穿过无风险资产点 $(0, r_f=0.01)$ ：  
  
```python  
In [87]: plt.figure(figsize=(10, 6))  
         plt.scatter(pvols, prets, c=(prets - 0.01) / pvols,  
                     marker='.', cmap='coolwarm')  
         plt.plot(evols, erets, 'b', lw=4.0)  
         cx = np.linspace(0.0, 0.3)  
         plt.plot(cx, opt[0] + opt[1] * cx, 'r', lw=1.5)  
         plt.plot(opt[2], f(opt[2]), 'y*', markersize=15.0)  
         plt.grid(True)  
         plt.axhline(0, color='k', ls='--', lw=2.0)  
         plt.axvline(0, color='k', ls='--', lw=2.0)  
         plt.xlabel('expected volatility')  
         plt.ylabel('expected return')  
         plt.colorbar(label='Sharpe ratio')  
```  
  
<img width="665" height="450" alt="python_for_finance 13 15" src="https://github.com/user-attachments/assets/1a2260f7-2429-4c12-9dc1-f280f86cbefb" />
  
图13-15. 资本市场线与无风险利率为1%时的切点投资组合（星号）  
  
最优（相切）投资组合的权重如下。只有三种资产被选中进行组合：  
  
```python  
In [88]: cons = ({'type': 'eq', 'fun': lambda x:  port_ret(x) - f(opt[2])},  
                 {'type': 'eq', 'fun': lambda x:  np.sum(x) - 1}) # 切点投资组合的约束条件（图13-15中的金星）。  
         res = sco.minimize(port_vol, eweights, method='SLSQP',  
                            bounds=bnds, constraints=cons)  
  
In [89]: res['x'].round(3) # 这个特定投资组合的权重。  
```  
  
```txt  
Out[89]: array([0.59 , 0.221, 0.189, 0.   ])  
```  
  
```python  
In [90]: port_ret(res['x'])  
```  
  
```txt  
Out[90]: 0.1749328414905194  
```  
  
```python  
In [91]: port_vol(res['x'])  
```  
  
```txt  
Out[91]: 0.19525371793918325  
```  
  
```python  
In [92]: port_ret(res['x']) / port_vol(res['x'])  
```  
  
```txt  
Out[92]: 0.8959257899765407  
```  
  
## 贝叶斯统计  
  
如今，贝叶斯统计在实证金融学中非常流行。本章当然无法为该领域所有概念奠定基础。如果需要，读者应查阅像Geweke（2005）这样进行一般性介绍的教科书，或者Rachev（2008）这样基于金融动机背景介绍的教科书。  
  
### 贝叶斯公式  
  
金融中对贝叶斯公式最常见的解释是历时性的解释。这主要指出随着时间的推移，人们能够学习关于感兴趣的特定变量或参数（例如时间序列的平均收益）的新信息。公式13-4正式阐述了该定理。  
  
公式 13-4. 贝叶斯公式  
$$p(H|D)=\frac{p(H)\cdot p(D|H)}{p(D)}$$  
  
在这里， $H$ 代表一个事件，即假设，而 $D$ 代表实验或现实世界可能呈现的数据。[^5]在这些基本概念的基础上，有：  
  
$p(H)$  
先验概率  
  
$p(D)$  
在任何假设下数据的概率，称为归一化常数  
  
$p(D|H)$  
在假设 $H$ 之下，数据的似然度（即概率）  
  
$p(H|D)$  
后验概率；即，在看到数据之后  
  
考虑一个简单的例子。有两个盒子 $B_1$ 和 $B_2$ 。盒子 $B_1$ 包含30个黑球和60个红球，而盒子 $B_2$ 包含60个黑球和30个红球。随机从其中一个盒子中抽出一个球。假设这个球是黑色的。那么假设“ $H_1$ ：球来自盒子 $B_1$ ”和假设“ $H_2$ ：球来自盒子 $B_2$ ”的概率分别是多少？  
  
在随机抽球之前，这两种假设的可能性是相等的。当明确球是黑色后，人们必须根据贝叶斯公式更新这两个假设的概率。考虑假设 $H_1$ ：  
  
* 先验概率： $p(H_1)=\frac{1}{2}$  
* 归一化常数： $p(D)=\frac{1}{2}\cdot\frac{1}{3}+\frac{1}{2}\cdot\frac{2}{3}=\frac{1}{2}$  
* 似然度： $p(D|H_1)=\frac{1}{3}$  
  
由此得出更新后关于 $H_1$ 的概率： $p(H_1|D)=\frac{\frac{1}{2}\cdot\frac{1}{3}}{\frac{1}{2}}=\frac{1}{3}$ 。  
  
这个结果在直觉上也是说得通的。从盒子 $B_2$ 中抽到黑球的概率是从盒子 $B_1$ 中发生相同事件的概率的两倍。因此，在抽到一个黑球之后，假设 $H_2$ 更新后的概率 $p(H_2|D)=\frac{2}{3}$ 也是假设 $H_1$ 更新后概率的两倍。  
  
### 贝叶斯回归  
  
通过PyMC3，Python生态系统为从技术上实现贝叶斯统计和概率编程提供了一个全面的库。  
  
考虑以下基于在直线周围具有噪声的数据所做的示例。[^6]首先，在这个数据集上实现了一个线性普通最小二乘法（OLS）回归（见第11章），其结果可视化显示在图13-16中：  
  
```python  
In [1]: import numpy as np  
        import pandas as pd  
        import datetime as dt  
        from pylab import mpl, plt  
  
In [2]: plt.style.use('seaborn')  
        mpl.rcParams['font.family'] = 'serif'  
        np.random.seed(1000)  
        %matplotlib inline  
  
In [3]: x = np.linspace(0, 10, 500)  
        y = 4 + 2 * x + np.random.standard_normal(len(x)) * 2  
  
In [4]: reg = np.polyfit(x, y, 1)  
  
In [5]: reg  
```  
  
```txt  
Out[5]: array([2.03384161, 3.77649234])  
```  
  
```python  
In [6]: plt.figure(figsize=(10, 6))  
        plt.scatter(x, y, c=y, marker='v', cmap='coolwarm')  
        plt.plot(x, reg[1] + reg[0] * x, lw=2.0)  
        plt.colorbar()  
        plt.xlabel('x')  
        plt.ylabel('y')  
```  
  
<img width="665" height="469" alt="python_for_finance 13 16" src="https://github.com/user-attachments/assets/4d4da567-b446-4578-be77-fa76bceee788" />
  
图13-16. 样本数据点和回归线  
  
OLS回归方法的结果是针对回归线两个参数（截距和斜率）的固定值。请注意，最高阶单项式的因子（在本例中为回归线的斜率）位于索引层级0处，而截距位于索引层级1处。原始参数2和4没有被完美恢复，这当然是由于数据中包含了噪声。  
  
其次是利用PyMC3包进行的贝叶斯回归。这里，假设参数以某种特定的方式分布。例如，考虑描述回归线的方程 $\hat{y}(x)=\alpha+\beta\cdot x$ 。现在假设有以下先验概率：  
  
* $\alpha$ 服从正态分布，其均值为0，标准差为20。  
* $\beta$ 服从正态分布，其均值为0，标准差为10。  
  
关于似然度，假设服从正态分布，其均值为 $\hat{y}(x)$ ，其标准差在0到10之间均匀分布。  
  
贝叶斯回归的一个主要元素是马尔可夫链蒙特卡洛（MCMC）采样。[^7]从原理上讲，这与在上一节简单示例中多次从盒子里抽球相同——只是采用了一种更为系统且自动化的方式。  
  
为了技术上的采样计算，有三个不同的函数需要被调用：  
  
* `find_MAP()` 通过推导局部最大后验点来寻找采样算法的起始点。  
* `NUTS()` 给定了假设先验的情况下，实现了用于MCMC采样的所谓的“带双重平均的无U转（NUTS）高效采样器”算法。  
* `sample()` 在给定来自 `find_MAP()` 的起始值以及来自 NUTS 算法的最优步长的情况下，提取数量特定的样本。  
  
这一切需要被包装进一个PyMC3 Model对象中，并在 `with` 语句中执行：  
  
```python  
In [8]: import pymc3 as pm  
  
In [9]: %%time  
        with pm.Model() as model:  
            # model  
            alpha = pm.Normal('alpha', mu=0, sd=20) # 定义先验。  
            beta = pm.Normal('beta', mu=0, sd=10) # 定义先验。  
            sigma = pm.Uniform('sigma', lower=0, upper=10) # 定义先验。  
            y_est = alpha + beta * x # 指定线性回归。  
            likelihood = pm.Normal('y', mu=y_est, sd=sigma,  
                                   observed=y) # 定义似然度。  
              
            # inference  
            start = pm.find_MAP() # 通过优化找到起始值。  
            step = pm.NUTS() # 实例化MCMC算法。  
            trace = pm.sample(100, tune=1000, start=start,  
                              progressbar=True) # 使用NUTS提取后验样本。  
        logp = -1,067.8, ||grad|| = 60.354: 100%|██████████| 28/28 [00:00<00:00,  
        474.70it/s]  
```  
  
```txt  
Only 100 samples in chain.  
Auto-assigning NUTS sampler...  
Initializing NUTS using jitter+adapt_diag...  
Multiprocess sampling (2 chains in 2 jobs)  
NUTS: [sigma, beta, alpha]  
Sampling 2 chains: 100%|██████████| 2200/2200 [00:03<00:00,  
690.96draws/s]  
  
CPU times: user 6.2 s, sys: 1.72 s, total: 7.92 s  
Wall time: 1min 28s  
```  
  
```python  
In [10]: pm.summary(trace) # 显示采样的汇总统计信息。  
```  
  
```txt  
Out[10]:  
           mean        sd  mc_error     hpd_2.5    hpd_97.5       n_eff      Rhat  
alpha  3.764027  0.174796  0.013177    3.431739    4.070091  152.446951  0.996281  
beta   2.036318  0.030519  0.002230    1.986874    2.094008  106.505590  0.999155  
sigma  2.010398  0.058663  0.004517    1.904395    2.138187  188.643293  0.998547  
```  
  
```python  
In [11]: trace[0] # 第一个样本的估计值。  
```  
  
```txt  
Out[11]: {'alpha': 3.9303300798212444,  
 'beta': 2.0020264758995463,  
 'sigma_interval__': -1.3519315719461853,  
 'sigma': 2.0555476283253156}  
```  
  
显示的三个估计值都相当接近于原始值（4，2，2）。但是，整个过程产生的估计值还要更多。借助于迹线图可以最清晰地展示这些信息，如图13-17所示——即展示了不同参数产生的后验分布以及每次采样的所有单个估计值。后验分布可以让人直观地感受到关于估计值的不确定性：  
  
```python  
In [12]: pm.traceplot(trace, lines={'alpha': 4, 'beta': 2, 'sigma': 2});  
```  
  
<img width="665" height="361" alt="python_for_finance 13 17" src="https://github.com/user-attachments/assets/9ae20cc7-e32e-4282-9063-d27c1bb32eff" />
  
图13-17. 后验分布与迹线图  
  
如果仅从回归中获取alpha和beta值，我们就可以绘制出所有推导出的回归线，如图13-18所示：  
  
```python  
In [13]: plt.figure(figsize=(10, 6))  
         plt.scatter(x, y, c=y, marker='v', cmap='coolwarm')  
         plt.colorbar()  
         plt.xlabel('x')  
         plt.ylabel('y')  
         for i in range(len(trace)):  
             plt.plot(x, trace['alpha'][i] + trace['beta'][i] * x) # 绘制单一回归线。  
```  
  
<img width="665" height="469" alt="python_for_finance 13 18" src="https://github.com/user-attachments/assets/90d8ab50-4fd5-45c8-8e25-cce27857d987" />
  
图13-18. 基于不同估计值的多条回归线  
  
### 两种金融工具  
  
介绍了利用PyMC3基于模拟虚拟数据进行贝叶斯回归后，将其应用到真实的金融数据上就相当直接了。该示例使用了两种交易所交易基金（ETF），即 GLD 和 GDX 的金融时间序列数据（见图13-19）：  
  
```python  
In [14]: raw = pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                           index_col=0, parse_dates=True)  
  
In [15]: data = raw[['GDX', 'GLD']].dropna()  
  
In [16]: data = data / data.iloc[0] # 将起始数据值归一化为1。  
  
In [17]: data.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 2138 entries, 2010-01-04 to 2018-06-29  
Data columns (total 2 columns):  
GDX    2138 non-null float64  
GLD    2138 non-null float64  
dtypes: float64(2)  
memory usage: 50.1 KB  
```  
  
```python  
In [18]: data.iloc[-1] / data.iloc[0] - 1 # 计算相对表现。  
```  
  
```txt  
Out[18]: GDX   -0.532383  
GLD    0.080601  
dtype: float64  
```  
  
```python  
In [19]: data.corr() # 计算两种工具之间的相关性。  
```  
  
```txt  
Out[19]:          GDX       GLD  
GDX  1.00000   0.71539  
GLD  0.71539   1.00000  
```  
  
```python  
In [20]: data.plot(figsize=(10, 6));  
```  
  
<img width="665" height="419" alt="python_for_finance 13 19" src="https://github.com/user-attachments/assets/d2291b28-a7ec-46ef-8193-c3738b31f1ef" />
  
图13-19. GLD 和 GDX 随时间的归一化价格  
  
接下来的内容中，各个数据点的日期被可视化显示在散点图中。为此，DataFrame对象的 `DatetimeIndex` 被转换为 matplotlib 格式的日期。图13-20展示了时间序列数据的散点图，将GLD值对应于GDX值绘制，并通过不同颜色呈现了各对数据对的日期：[^8]  
  
```python  
In [21]: data.index[:3]  
```  
  
```txt  
Out[21]: DatetimeIndex(['2010-01-04', '2010-01-05', '2010-01-06'],  
              dtype='datetime64[ns]', name='Date', freq=None)  
```  
  
```python  
In [22]: mpl_dates = mpl.dates.date2num(data.index.to_pydatetime()) # 将 DatetimeIndex 对象转换为 matplotlib 的日期。  
         mpl_dates[:3]  
```  
  
```txt  
Out[22]: array([733776., 733777., 733778.])  
```  
  
```python  
In [23]: plt.figure(figsize=(10, 6))  
         plt.scatter(data['GDX'], data['GLD'], c=mpl_dates,  
                     marker='o', cmap='coolwarm')  
         plt.xlabel('GDX')  
         plt.ylabel('GLD')  
         plt.colorbar(ticks=mpl.dates.DayLocator(interval=250),  
                      format=mpl.dates.DateFormatter('%d %b %y')); # 为日期自定颜色栏。  
```  
  
<img width="665" height="438" alt="python_for_finance 13 20" src="https://github.com/user-attachments/assets/14a4bd28-e6e9-41f7-a094-528af7d77807" />
  
图13-20. GLD 价格相对于 GDX 价格的散点图  
  
接下来的代码基于这两个时间序列实现了一个贝叶斯回归。它的参数化实质上与前面带有虚拟数据的例子相同。图13-21显示了在使用MCMC采样程序后，根据三个参数关于先验概率分布的假设而产生的结果：  
  
```python  
In [24]: with pm.Model() as model:  
             alpha = pm.Normal('alpha', mu=0, sd=20)  
             beta = pm.Normal('beta', mu=0, sd=20)  
             sigma = pm.Uniform('sigma', lower=0, upper=50)  
               
             y_est = alpha + beta * data['GDX'].values  
               
             likelihood = pm.Normal('GLD', mu=y_est, sd=sigma,  
                                    observed=data['GLD'].values)  
               
             start = pm.find_MAP()  
             step = pm.NUTS()  
             trace = pm.sample(250, tune=2000, start=start,  
                               progressbar=True)  
         logp = 1,493.7, ||grad|| = 188.29: 100%|██████████| 27/27 [00:00<00:00,  
         1609.34it/s]  
```  
  
```txt  
Only 250 samples in chain.  
Auto-assigning NUTS sampler...  
Initializing NUTS using jitter+adapt_diag...  
Multiprocess sampling (2 chains in 2 jobs)  
NUTS: [sigma, beta, alpha]  
Sampling 2 chains: 100%|██████████| 4500/4500 [00:09<00:00,  
465.07draws/s]  
The estimated number of effective samples is smaller than 200 for some  
parameters.  
```  
  
```python  
In [25]: pm.summary(trace)  
```  
  
```txt  
Out[25]:  
           mean        sd  mc_error     hpd_2.5    hpd_97.5       n_eff      Rhat  
alpha  0.913335  0.005983  0.000356    0.901586    0.924714  184.264900  1.001855  
beta   0.385394  0.007746  0.000461    0.369154    0.398291  215.477738  1.001570  
sigma  0.119484  0.001964  0.000098    0.115305    0.123315  312.260213  1.005246  
```  
  
```python  
In [26]: fig = pm.traceplot(trace)  
```  
  
<img width="665" height="361" alt="python_for_finance 13 21" src="https://github.com/user-attachments/assets/1a00c34c-e82f-4216-b49c-4200a125d1ea" />
  
图13-21. GDX 和 GLD 数据的后验分布与迹线图  
  
图13-22将所有的回归线加入到了之前构建的散点图中。然而，所有的回归线彼此之间都贴得非常近：  
  
```python  
In [27]: plt.figure(figsize=(10, 6))  
         plt.scatter(data['GDX'], data['GLD'], c=mpl_dates,  
                     marker='o', cmap='coolwarm')  
         plt.xlabel('GDX')  
         plt.ylabel('GLD')  
         for i in range(len(trace)):  
             plt.plot(data['GDX'],  
                      trace['alpha'][i] + trace['beta'][i] * data['GDX'])  
         plt.colorbar(ticks=mpl.dates.DayLocator(interval=250),  
                      format=mpl.dates.DateFormatter('%d %b %y'));  
```  
  
<img width="665" height="437" alt="python_for_finance 13 22" src="https://github.com/user-attachments/assets/ce9d3eab-1ccd-4fc0-ad33-a73dd35128c9" />
  
图13-22. 穿过 GDX 和 GLD 数据的多个贝叶斯回归线  
  
这幅图揭示了所用回归方法的一个主要缺陷：该方法没有考虑随时间的演变演进。也就是说，将最新的数据采用与最老的数据相同的方式去对待处理。  
  
### 随时间更新估计  
  
如前所述，由于将金融中的贝叶斯方法看作是“历时的”——即由于新数据随时间揭示出来，人们便能够通过更新或学习进行更好的回归并作出更好的估计——这样才是金融领域应用贝叶斯方法最有用的时候。  
  
为了在当前的例子中融入这一概念，我们不再仅仅假设回归参数是随机的并以某种方式分布，而是假设它们遵循某种随时间的随机游走。这与在金融理论中完成从随机变量到随机过程（它们本质上是随机变量的有序序列）过渡时所采用的是一样的推广化处理思想。  
  
为此，定义一个新的PyMC3模型，这次将参数值指定为随机游走。指定了随机游走参数的分布之后，便继续指定 alpha 和 beta 的随机游走。为了使整个过程更加高效，采用每次共享相同系数的50个数据点分段作为跨度：  
  
```python  
In [28]: from pymc3.distributions.timeseries import GaussianRandomWalk  
  
In [29]: subsample_alpha = 50  
         subsample_beta = 50  
  
In [30]: model_randomwalk = pm.Model()  
         with model_randomwalk:  
             sigma_alpha = pm.Exponential('sig_alpha', 1. / .02, testval=.1) # 定义随机游走参数的先验。  
             sigma_beta = pm.Exponential('sig_beta', 1. / .02, testval=.1) # 定义随机游走参数的先验。  
             alpha = GaussianRandomWalk('alpha', sigma_alpha ** -2,  
                                        shape=int(len(data) / subsample_alpha)) # 随机游走模型。  
             beta = GaussianRandomWalk('beta', sigma_beta ** -2,  
                                       shape=int(len(data) / subsample_beta)) # 随机游走模型。  
             alpha_r = np.repeat(alpha, subsample_alpha) # 把参数向量变为区间长度。  
             beta_r = np.repeat(beta, subsample_beta) # 把参数向量变为区间长度。  
             regression = alpha_r + beta_r * data['GDX'].values[:2100] # 定义回归模型。  
             sd = pm.Uniform('sd', 0, 20) # 用于标准差的先验。  
             likelihood = pm.Normal('GLD', mu=regression, sd=sd,  
                                    observed=data['GLD'].values[:2100]) # 采用通过回归结果得出的 mu 定义似然度。  
```  
  
这些定义比起以前稍微复杂一些，因为它是使用了随机游走而不是单个随机变量。然而，MCMC采样的推断步骤本质上还是相同的。不过请注意，由于算法必须计算每个随机游走样本的参数——在本例中就是 1,950 / 50 = 39 种参数组合（而以前只有 1 种）——所以运算负担大幅增加了：  
  
```python  
In [31]: %%time  
         import scipy.optimize as sco  
         with model_randomwalk:  
             start = pm.find_MAP(vars=[alpha, beta],  
                                 fmin=sco.fmin_l_bfgs_b)  
             step = pm.NUTS(scaling=start)  
             trace_rw = pm.sample(250, tune=1000, start=start,  
                                  progressbar=True)  
         logp = -6,657:   2%|▏         | 82/5000 [00:00<00:08, 550.29it/s]  
```  
  
```txt  
Only 250 samples in chain.  
Auto-assigning NUTS sampler...  
Initializing NUTS using jitter+adapt_diag...  
Multiprocess sampling (2 chains in 2 jobs)  
NUTS: [sd, beta, alpha, sig_beta, sig_alpha]  
Sampling 2 chains: 100%|██████████| 2500/2500 [02:48<00:00,  8.59draws/s]  
  
CPU times: user 27.5 s, sys: 3.68 s, total: 31.2 s  
Wall time: 5min 3s  
```  
  
```python  
In [32]: pm.summary(trace_rw).head() # 各个区间的汇总统计信息（只展示前5个并且只有 alpha 的数据）。  
```  
  
```txt  
Out[32]:  
               mean        sd  mc_error     hpd_2.5  hpd_97.5        n_eff \  
alpha__0   0.673846  0.040224  0.001376    0.592655  0.753034  1004.616544  
alpha__1   0.424819  0.041257  0.001618    0.348102  0.509757   804.760648  
alpha__2   0.456817  0.057200  0.002011    0.321125  0.553173   800.225916  
alpha__3   0.268148  0.044879  0.001725    0.182744  0.352197   724.967532  
alpha__4   0.651465  0.057472  0.002197    0.544076  0.761216   978.073246  
  
               Rhat  
alpha__0   0.998637  
alpha__1   0.999540  
alpha__2   0.998075  
alpha__3   0.998995  
alpha__4   0.998060  
```  
  
图13-23通过绘制一部分估计值来说明回归参数 alpha 和 beta 随时间的演变情况：  
  
```python  
In [33]: sh = np.shape(trace_rw['alpha']) # 获取含参数估计值的对象形状。  
         sh   
```  
  
```txt  
Out[33]: (500, 42)  
```  
  
```python  
In [34]: part_dates = np.linspace(min(mpl_dates),  
                                  max(mpl_dates), sh[1]) # 创建匹配区间数量的日期列表。  
  
In [35]: index = [dt.datetime.fromordinal(int(date)) for  
                  date in part_dates] # 创建匹配区间数量的日期列表。  
  
In [36]: alpha = {'alpha_%i' % i: v for i, v in  
                  enumerate(trace_rw['alpha']) if i < 20} # 将相关参数时间序列收集到两个DataFrame对象中。  
  
In [37]: beta = {'beta_%i' % i: v for i, v in  
                 enumerate(trace_rw['beta']) if i < 20} # 将相关参数时间序列收集到两个DataFrame对象中。  
  
In [38]: df_alpha = pd.DataFrame(alpha, index=index) # 将相关参数时间序列收集到两个DataFrame对象中。  
  
In [39]: df_beta = pd.DataFrame(beta, index=index) # 将相关参数时间序列收集到两个DataFrame对象中。  
  
In [40]: ax = df_alpha.plot(color='b', style='-.', legend=False,  
                            lw=0.7, figsize=(10, 6))  
         df_beta.plot(color='r', style='-.', legend=False,  
                      lw=0.7, ax=ax)  
         plt.ylabel('alpha/beta');  
```  
  
<img width="665" height="394" alt="python_for_finance 13 23" src="https://github.com/user-attachments/assets/1fbbd0f6-a920-428c-8edd-9c294db3cab1" />
  
图13-23. 随时间推移的选定参数估计值  
  
**绝对价格数据与相对收益数据**  
本节中的分析均基于归一化的价格数据。这仅作说明用途，因为其相应的图形结果更易于理解和诠释（它们在视觉上“更具吸引力”）。但在针对现实世界的金融应用中，研究通常应该转而依赖于诸如收益率等数据，以确保时间序列数据的平稳性。  
  
使用alpha和beta的平均值，图13-24展示了回归模型随时间是如何更新的。由alpha和beta平均值所得出的39条不同回归线显示在图中。显而易见，随时间更新使得回归对（当前或最新的）数据的拟合有了显著提高——换句话说就是，“每个时间段都需要它自己的回归”。  
  
```python  
In [41]: plt.figure(figsize=(10, 6))  
         plt.scatter(data['GDX'], data['GLD'], c=mpl_dates,  
                     marker='o', cmap='coolwarm')  
         plt.colorbar(ticks=mpl.dates.DayLocator(interval=250),  
                      format=mpl.dates.DateFormatter('%d %b %y'))  
         plt.xlabel('GDX')  
         plt.ylabel('GLD')  
         x = np.linspace(min(data['GDX']), max(data['GDX']))  
         for i in range(sh[1]): # 为长度等于50的所有时间区间绘制回归线。  
             alpha_rw = np.mean(trace_rw['alpha'].T[i])  
             beta_rw = np.mean(trace_rw['beta'].T[i])  
             plt.plot(x, alpha_rw + beta_rw * x, '--', lw=0.7,  
                      color=plt.cm.coolwarm(i / sh[1]))  
```  
  
<img width="666" height="434" alt="python_for_finance 13 24" src="https://github.com/user-attachments/assets/dc1eed33-0bb4-44de-b810-f3bcb82d00cd" />
  
图13-24. 带有随时间变化的回归线（采用已更新参数估计）的散点图  
  
有关贝叶斯统计的内容到此结束。Python与PyMC3搭配使用，提供了一个全面性的功能包，可以用来实现关于贝叶斯统计以及概率编程的各种方法。贝叶斯回归正逐渐流行开来，它是量化金融里相当重要的一个工具。  
  
[^1]: 另一个核心假设是线性假设。例如，一般认为金融市场表现出关于对某股票份额的需求与其所需支付价格之间的线性关系。换句话说，市场一般被假设为处于完全流动状态的，这意味着不管需求如何波动，都不会对某一金融工具的单位价格产生任何影响。  
  
[^2]: 有关此背景下所需随机过程与伊藤微积分（Itô calculus）的基础知识，请参阅 Glasserman（2004）。  
  
[^3]: 参见 Markowitz（1952）。  
  
[^4]: `np.sum(x) - 1` 的一个替代方法可以写为 `np.sum(x) == 1` ，需要考虑在Python中布尔值 `True` 的值为 1 ，而 `False` 值为 0。  
  
[^5]: 关于在Python环境中对这些以及贝叶斯统计其他基本概念的入门介绍，请参考 Downey (2013)。  
  
[^6]: 这里的例子最初由 Thomas Wiecki 提供，他是 PyMC3 包的主要作者之一。  
  
[^7]: 例如，整本书中所采用并在第12章被详细分析过的所有蒙特卡洛算法全都能产生所谓的马尔可夫链，因为紧随其后的下一步/值只取决于该过程的当前状态，而跟历史上其他任何状态或值无关。  
  
[^8]: 请注意，此处的所有可视化都是基于经过了归一化的价格数据而不是收益率数据的（由于使用收益率数据可能会在现实应用中产生更好效果）。  
  
## 机器学习  
  
如今在金融及许多其他领域，“制胜关键”就是机器学习（ML）。正如以下引文所述：  
  
> 计量经济学在金融学术界取得成功可能已经足够好了（目前而言），但在实践中取得成功则需要机器学习。  
>  
> ——Marcos López de Prado (2018)  
  
机器学习包含了不同类型的算法，这些算法基本上能够自行从原始数据中学习特定的关系、模式等。第463页的“进一步获取资源的途径”列出了许多可以参考的书籍，涵盖机器学习方法和算法的数学和统计方面，以及与其实现和实际使用相关的主题。例如，Alpaydin (2016) 对该领域进行了温和的介绍，并对通常使用的算法类型进行了非技术的概述。  
  
本节采取严格实用的方法，仅侧重于选定的实现方面——旨在为第15章中使用的技术提供参考。然而，所介绍的算法和技术当然可以用于许多不同的金融领域，而不仅仅是算法交易。本节涵盖两种类型的算法：无监督学习和有监督学习算法。  
  
Python中最受欢迎的机器学习包之一是 `scikit-learn`。它不仅提供了种类繁多的机器学习算法的实现，还提供了大量用于与机器学习任务相关的预处理和后处理活动的有用工具。本节主要依赖于这个包。它还在深度神经网络（DNNs）的背景下使用了 `TensorFlow`。  
  
VanderPlas (2016) 提供了基于Python和 `scikit-learn` 的不同机器学习算法的简明介绍。Albon (2018) 提供了机器学习中典型任务的许多秘诀，同样主要使用Python和 `scikit-learn`。  
  
### 无监督学习  
  
无监督学习体现了这样一种理念，即机器学习算法在没有任何进一步指导的情况下从原始数据中发现洞察。其中一种算法是k-means聚类算法，它将原始数据集聚类为多个子集，并为这些子集分配标签（“cluster 0”、“cluster 1”等）。另一个是高斯混合。9  
  
9 有关 `scikit-learn` 中更多可用的无监督学习算法，请参阅文档。  
  
#### 数据  
  
除其他功能外，`scikit-learn` 允许为不同类型的机器学习问题创建样本数据集。以下代码创建了一个适合说明k-means聚类的样本数据集。  
  
首先，一些标准的导入和配置：  
  
```python  
In [1]: import numpy as np  
 import pandas as pd  
 import datetime as dt  
 from pylab import mpl, plt  
  
In [2]: plt.style.use('seaborn')  
 mpl.rcParams['font.family'] = 'serif'  
 np.random.seed(1000)  
 np.set_printoptions(suppress=True, precision=4)  
 %matplotlib inline  
```  
  
其次，创建样本数据集。图13-25可视化了样本数据：  
  
```python  
In [3]: from sklearn.datasets.samples_generator import make_blobs  
  
In [4]: X, y = make_blobs(n_samples=250, centers=4,  
 random_state=500, cluster_std=1.25) # ❶ 创建带有250个样本和4个中心的聚类示例数据集。  
  
In [5]: plt.figure(figsize=(10, 6))  
 plt.scatter(X[:, 0], X[:, 1], s=50);  
```  
  
<img width="666" height="424" alt="python_for_finance 13 25" src="https://github.com/user-attachments/assets/ec7b308f-c0b6-4df2-b589-f02476c03ec8" />
  
图13-25. 聚类算法应用示例数据  
  
#### k-means聚类  
  
`scikit-learn` 的便利特征之一是它提供了一个标准化的API来应用不同种类的算法。以下代码显示了k-means聚类的基本步骤，这些步骤随后也会在其​​他模型中重复使用：  
  
- 导入模型类  
- 实例化模型对象  
- 将模型对象拟合到某些数据  
- 给定拟合模型，针对某些数据预测结果  
  
图13-26显示了结果：  
  
```python  
In [6]: from sklearn.cluster import KMeans # ❶ 从scikit-learn导入模型类。  
  
In [7]: model = KMeans(n_clusters=4, random_state=0) # ❷ 给定某些参数，实例化一个模型对象；利用关于样本数据的知识来指导实例化。  
  
In [8]: model.fit(X) # ❸ 将模型对象拟合到原始数据。  
```  
  
```txt  
Out[8]: KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,  
 n_clusters=4, n_init=10, n_jobs=None, precompute_distances='auto',  
 random_state=0, tol=0.0001, verbose=0)  
```  
  
```python  
In [9]: y_kmeans = model.predict(X) # ❹ 给定原始数据，预测簇（编号）。  
  
In [10]: y_kmeans[:12] # ❺ 显示一些预测的簇编号。  
```  
  
```txt  
Out[10]: array([1, 1, 0, 3, 0, 1, 3, 3, 3, 0, 2, 2], dtype=int32)  
```  
  
```python  
In [11]: plt.figure(figsize=(10, 6))  
 plt.scatter(X[:, 0], X[:, 1], c=y_kmeans, cmap='coolwarm');  
```  
  
<img width="665" height="422" alt="python_for_finance 13 26" src="https://github.com/user-attachments/assets/91fbbed4-dfd8-43d1-bdac-09b8b0c25daf" />
  
图13-26. 示例数据和识别出的簇  
  
#### 高斯混合  
  
作为另一种聚类方法，可以考虑高斯混合。其应用是相同的，在适当的参数化下，结果也是相同的：  
  
```python  
In [12]: from sklearn.mixture import GaussianMixture  
  
In [13]: model = GaussianMixture(n_components=4, random_state=0)  
  
In [14]: model.fit(X)  
```  
  
```txt  
Out[14]: GaussianMixture(covariance_type='full', init_params='kmeans',  
 max_iter=100,  
 means_init=None, n_components=4, n_init=1, precisions_init=None,  
 random_state=0, reg_covar=1e-06, tol=0.001, verbose=0,  
 verbose_interval=10, warm_start=False, weights_init=None)  
```  
  
```python  
In [15]: y_gm = model.predict(X)  
  
In [16]: y_gm[:12]  
```  
  
```txt  
Out[16]: array([1, 1, 0, 3, 0, 1, 3, 3, 3, 0, 2, 2])  
```  
  
```python  
In [17]: (y_gm == y_kmeans).all() # ❶ k-means聚类和高斯混合的结果是相同的。  
```  
  
```txt  
Out[17]: True  
```  
  
### 有监督学习  
  
有监督学习是一种在已知结果或观测数据形式的指导下进行的机器学习。这意味着原始数据已经包含了机器学习算法应该学习的内容。在接下来的内容中，重点是分类问题，而不是估计问题。虽然估计问题通常是关于实值数量的估计，但分类问题的特征在于致力于将某种特征组合分配给来自相对较小的类别集合（整数值）中的特定类别（整数值）。  
  
上一小节中的示例表明，在无监督学习中，算法会为识别出的簇提出自己的分类标签。有四个簇时，标签为0、1、2和3。在有监督学习中，这样的分类标签已经给定，以便算法能够学习特征和类别（类）之间的关系。换句话说，在拟合步骤中，算法知道给定特征值组合的正确类别。  
  
本小节说明以下分类算法的应用：高斯朴素贝叶斯、逻辑回归、决策树、深度神经网络和支持向量机。10  
  
10 有关 `scikit-learn` 中可用的有监督学习分类算法的概述，请参阅文档。请注意，其中许多算法也可用于估计而不是分类。  
  
#### 数据  
  
同样，`scikit-learn` 允许创建一个适当的样本数据集来应用分类算法。为了能够可视化结果，样本数据仅包含两个包含信息的实值特征和一个单一的二元标签（二元标签的特征是仅有两个不同的类别，0和1）。以下代码创建了样本数据，显示了数据的某些提取部分，并对数据进行了可视化（见图13-27）：  
  
```python  
In [18]: from sklearn.datasets import make_classification  
  
In [19]: n_samples = 100  
  
In [20]: X, y = make_classification(n_samples=n_samples, n_features=2,  
 n_informative=2, n_redundant=0,  
 n_repeated=0, random_state=250)  
  
In [21]: X[:5]  
```  
  
```txt  
Out[21]: array([[ 1.6876, -0.7976],  
 [-0.4312, -0.7606],  
 [-1.4393, -1.2363],  
 [ 1.118 , -1.8682],  
 [ 0.0502, 0.659 ]])  
```  
  
```python  
In [22]: X.shape # ❶ 两个包含信息的实值特征。  
```  
  
```txt  
Out[22]: (100, 2)  
```  
  
```python  
In [23]: y[:5]  
```  
  
```txt  
Out[23]: array([1, 0, 0, 1, 1])  
```  
  
```python  
In [24]: y.shape # ❷ 单一的二元标签。  
```  
  
```txt  
Out[24]: (100,)  
```  
  
```python  
plt.figure(figsize=(10, 6))  
plt.hist(X);  
  
In [25]: plt.figure(figsize=(10, 6))  
 plt.scatter(x=X[:, 0], y=X[:, 1], c=y, cmap='coolwarm');  
```  
  
<img width="666" height="428" alt="python_for_finance 13 27" src="https://github.com/user-attachments/assets/6b461696-137e-4e54-9ce3-8c5d2a5480ff" />
  
图13-27. 分类算法应用的示例数据  
  
#### 高斯朴素贝叶斯  
  
高斯朴素贝叶斯（GNB）通常被认为是解决众多不同分类问题的一个良好基准算法。其应用与第446页上“k-means聚类”中概述的步骤一致：  
  
```python  
In [26]: from sklearn.naive_bayes import GaussianNB  
 from sklearn.metrics import accuracy_score  
  
In [27]: model = GaussianNB()  
  
In [28]: model.fit(X, y)  
```  
  
```txt  
Out[28]: GaussianNB(priors=None, var_smoothing=1e-09)  
```  
  
```python  
In [29]: model.predict_proba(X).round(4)[:5] # ❶ 显示拟合后算法分配给每个类的概率。  
```  
  
```txt  
Out[29]: array([[0.0041, 0.9959],  
 [0.8534, 0.1466],  
 [0.9947, 0.0053],  
 [0.0182, 0.9818],  
 [0.5156, 0.4844]])  
```  
  
```python  
In [30]: pred = model.predict(X) # ❷ 基于概率，预测数据集的二元类。  
  
In [31]: pred  
```  
  
```txt  
Out[31]: array([1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 1,  
 0,  
 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0,  
 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0,  
 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1,  
 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0])  
```  
  
```python  
In [32]: pred == y # ❸ 将预测的类与真实类进行比较。  
```  
  
```txt  
Out[32]: array([ True, True, True, True, False, True, True, True, True,  
 True, False, True, True, True, True, True, True, True,  
 True, True, True, True, False, False, False, True, True,  
 True, True, True, True, True, True, False, True, True,  
 True, True, True, True, True, True, True, True, True,  
 True, True, True, True, True, True, False, True, False,  
 True, True, True, True, True, True, True, True, True,  
 True, True, False, True, True, True, True, True, True,  
 True, True, True, True, True, True, False, True, False,  
 True, True, True, True, True, True, True, True, True,  
 True, True, False, True, False, True, True, True, True,  
 True])  
```  
  
```python  
In [33]: accuracy_score(y, pred) # ❹ 给定预测值，计算准确度得分。  
```  
  
```txt  
Out[33]: 0.87  
```  
  
图13-28可视化了GNB的正确和错误预测：  
  
```python  
In [34]: Xc = X[y == pred] # ❶ 选择正确的预测并绘制它们。  
 Xf = X[y != pred] # ❷ 选择错误的预测并绘制它们。  
  
In [35]: plt.figure(figsize=(10, 6))  
 plt.scatter(x=Xc[:, 0], y=Xc[:, 1], c=y[y == pred],  
 marker='o', cmap='coolwarm')  
 plt.scatter(x=Xf[:, 0], y=Xf[:, 1], c=y[y != pred],  
 marker='x', cmap='coolwarm')  
```  
  
<img width="666" height="428" alt="python_for_finance 13 28" src="https://github.com/user-attachments/assets/9793f5c9-9076-4e0e-8404-71c751702d7d" />
  
图13-28. GNB的正确（圆点）和错误预测（叉号）  
  
#### 逻辑回归  
  
逻辑回归（LR）是一种快速且可扩展的分类算法。在这种特定情况下的准确度比GNB略好：  
  
```python  
In [36]: from sklearn.linear_model import LogisticRegression  
  
In [37]: model = LogisticRegression(C=1, solver='lbfgs')  
  
In [38]: model.fit(X, y)  
```  
  
```txt  
Out[38]: LogisticRegression(C=1, class_weight=None, dual=False,  
 fit_intercept=True,  
 intercept_scaling=1, max_iter=100, multi_class='warn',  
 n_jobs=None, penalty='l2', random_state=None, solver='lbfgs',  
 tol=0.0001, verbose=0, warm_start=False)  
```  
  
```python  
In [39]: model.predict_proba(X).round(4)[:5]  
```  
  
```txt  
Out[39]: array([[0.011 , 0.989 ],  
 [0.7266, 0.2734],  
 [0.971 , 0.029 ],  
 [0.04 , 0.96 ],  
 [0.4843, 0.5157]])  
```  
  
```python  
In [40]: pred = model.predict(X)  
  
In [41]: accuracy_score(y, pred)  
```  
  
```txt  
Out[41]: 0.9  
```  
  
```python  
In [42]: Xc = X[y == pred]  
 Xf = X[y != pred]  
  
In [43]: plt.figure(figsize=(10, 6))  
 plt.scatter(x=Xc[:, 0], y=Xc[:, 1], c=y[y == pred],  
 marker='o', cmap='coolwarm')  
 plt.scatter(x=Xf[:, 0], y=Xf[:, 1], c=y[y != pred],  
 marker='x', cmap='coolwarm');  
```  
  
#### 决策树  
  
决策树（DTs）是另一种扩展性很好的分类算法。在最大深度为1的情况下，该算法的表现已经略好于GNB和LR（另见图13-29）：  
  
```python  
In [44]: from sklearn.tree import DecisionTreeClassifier  
  
In [45]: model = DecisionTreeClassifier(max_depth=1)  
  
In [46]: model.fit(X, y)  
```  
  
```txt  
Out[46]: DecisionTreeClassifier(class_weight=None, criterion='gini',  
 max_depth=1,  
 max_features=None, max_leaf_nodes=None,  
 min_impurity_decrease=0.0, min_impurity_split=None,  
 min_samples_leaf=1, min_samples_split=2,  
 min_weight_fraction_leaf=0.0, presort=False, random_state=None,  
 splitter='best')  
```  
  
```python  
In [47]: model.predict_proba(X).round(4)[:5]  
```  
  
```txt  
Out[47]: array([[0.08, 0.92],  
 [0.92, 0.08],  
 [0.92, 0.08],  
 [0.08, 0.92],  
 [0.08, 0.92]])  
```  
  
```python  
In [48]: pred = model.predict(X)  
  
In [49]: accuracy_score(y, pred)  
```  
  
```txt  
Out[49]: 0.92  
```  
  
```python  
In [50]: Xc = X[y == pred]  
 Xf = X[y != pred]  
  
In [51]: plt.figure(figsize=(10, 6))  
 plt.scatter(x=Xc[:, 0], y=Xc[:, 1], c=y[y == pred],  
 marker='o', cmap='coolwarm')  
 plt.scatter(x=Xf[:, 0], y=Xf[:, 1], c=y[y != pred],  
 marker='x', cmap='coolwarm');  
```  
  
<img width="665" height="428" alt="python_for_finance 13 29" src="https://github.com/user-attachments/assets/caa8820d-1d0f-4104-8945-ba00d72a2dce" />
  
图13-29. DT (max_depth=1)的正确（圆点）和错误预测（叉号）  
  
然而，增加决策树的最大深度参数可以使其达到完美的结果：  
  
```python  
In [52]: print('{:>8s} | {:8s}'.format('depth', 'accuracy'))  
 print(20 * '-')  
 for depth in range(1, 7):  
 model = DecisionTreeClassifier(max_depth=depth)  
 model.fit(X, y)  
 acc = accuracy_score(y, model.predict(X))  
 print('{:8d} | {:8.2f}'.format(depth, acc))  
```  
  
```txt  
 depth | accuracy  
 --------------------  
 1 | 0.92  
 2 | 0.92  
 3 | 0.94  
 4 | 0.97  
 5 | 0.99  
 6 | 1.00  
```  
  
#### 深度神经网络  
  
深度神经网络（DNNs）被认为是用于估计和分类的最强大——同时计算要求也最高——的算法之一。谷歌开源的 `TensorFlow` 包及其相关的成功案例是其受欢迎的部分原因。DNN能够学习和模拟复杂的非线性关系。虽然它们的起源可以追溯到1970年代，但直到最近由于硬件（CPUs、GPUs、TPUs）、数值算法及相关软件实现的进步，它们才在大规模上变得可行。  
  
虽然其他机器学习算法（如LR类型的线性模型）可以基于标准的优化问题有效拟合，但DNN依赖于深度学习，这通常需要大量重复的步骤来调整某些参数（权重）并将结果与​​数据进行比较。在这个意义上，深度学习可以与数理金融中的蒙特卡洛模拟进行比较，比如欧式看涨期权的价格可以基于标的资产的100,000条模拟路径来估计。另一方面，Black-Scholes-Merton期权定价公式以封闭形式提供，可以进行解析评估。  
  
虽然蒙特卡洛模拟是数理金融中最灵活、最强大的数值技术之一，但代价是高昂的计算负担和庞大的内存占用。深度学习也是如此，它通常比许多其他机器学习算法更灵活，但也需要更强大的计算能力。  
  
**使用scikit-learn的DNN。** 虽然本质上截然不同，但 `scikit-learn` 为其 `MLPClassifier` 算法类11（这是一个DNN模型）提供了与之前使用的其他机器学习算法相同的API。仅用两个所谓的隐藏层，它在测试数据上就达到了完美的结果（隐藏层是将深度学习区别于简单学习的要素——例如，在线性回归的背景下“学习”权重，而不是使用OLS回归直接推导它们）：  
  
11 有关更多详细信息和可用参数，请参阅有关多层感知器分类器的文档。  
  
```python  
In [53]: from sklearn.neural_network import MLPClassifier  
  
In [54]: model = MLPClassifier(solver='lbfgs', alpha=1e-5,  
 hidden_layer_sizes=2 * [75], random_state=10)  
  
In [55]: %time model.fit(X, y)  
```  
  
```txt  
 CPU times: user 537 ms, sys: 14.2 ms, total: 551 ms  
 Wall time: 340 ms  
Out[55]: MLPClassifier(activation='relu', alpha=1e-05, batch_size='auto',  
 beta_1=0.9,  
 beta_2=0.999, early_stopping=False, epsilon=1e-08,  
 hidden_layer_sizes=[75, 75], learning_rate='constant',  
 learning_rate_init=0.001, max_iter=200, momentum=0.9,  
 n_iter_no_change=10, nesterovs_momentum=True, power_t=0.5,  
 random_state=10, shuffle=True, solver='lbfgs', tol=0.0001,  
 validation_fraction=0.1, verbose=False, warm_start=False)  
```  
  
```python  
In [56]: pred = model.predict(X)  
 pred  
```  
  
```txt  
Out[56]: array([1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 1,  
 0,  
 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0,  
 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1,  
 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1,  
 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0])  
```  
  
```python  
In [57]: accuracy_score(y, pred)  
```  
  
```txt  
Out[57]: 1.0  
```  
  
**使用TensorFlow的DNN。** `TensorFlow` 的API与 `scikit-learn` 标准不同。然而，`DNNClassifier` 类的应用同样直截了当：  
  
```python  
In [58]: import tensorflow as tf  
 tf.logging.set_verbosity(tf.logging.ERROR) # ❶ 设置TensorFlow日志记录的详细程度。  
  
In [59]: fc = [tf.contrib.layers.real_valued_column('features')] # ❷ 抽象地定义实值特征。  
  
In [60]: model = tf.contrib.learn.DNNClassifier(hidden_units=5 * [250],  
 n_classes=2,  
 feature_columns=fc) # ❸ 实例化模型对象。  
  
In [61]: def input_fn(): # ❹ 特征和标签数据将由函数提供。  
 fc = {'features': tf.constant(X)}  
 la = tf.constant(y)  
 return fc, la  
  
In [62]: %time model.fit(input_fn=input_fn, steps=100) # ❺ 通过学习拟合模型并对其进行评估。  
```  
  
```txt  
 CPU times: user 7.1 s, sys: 1.35 s, total: 8.45 s  
 Wall time: 4.71 s  
Out[62]: DNNClassifier(params={'head':  
 <tensorflow.contrib.learn.python.learn ... head._BinaryLogisticHead  
 object at 0x1a3ee692b0>, 'hidden_units': [250, 250, 250, 250, 250],  
 'feature_columns': (_RealValuedColumn(column_name='features',  
 dimension=1, default_value=None, dtype=tf.float32, normalizer=None),),  
 'optimizer': None, 'activation_fn': <function relu at 0x1a3aa75b70>,  
 'dropout': None, 'gradient_clip_norm': None,  
 'embedding_lr_multipliers': None, 'input_layer_min_slice_size': None})  
```  
  
```python  
In [63]: model.evaluate(input_fn=input_fn, steps=1)  
```  
  
```txt  
Out[63]: {'loss': 0.18724777,  
 'accuracy': 0.91,  
 'labels/prediction_mean': 0.5003989,  
 'labels/actual_label_mean': 0.5,  
 'accuracy/baseline_label_mean': 0.5,  
 'auc': 0.9782,  
 'auc_precision_recall': 0.97817385,  
 'accuracy/threshold_0.500000_mean': 0.91,  
 'precision/positive_threshold_0.500000_mean': 0.9019608,  
 'recall/positive_threshold_0.500000_mean': 0.92,  
 'global_step': 100}  
```  
  
```python  
In [64]: pred = np.array(list(model.predict(input_fn=input_fn))) # ❻ 基于特征值预测标签值。  
 pred[:10]   
```  
  
```txt  
Out[64]: array([1, 0, 0, 1, 1, 0, 1, 1, 1, 1])  
```  
  
```python  
In [65]: %time model.fit(input_fn=input_fn, steps=750) # ❼ 基于更多学习步骤重新训练模型；先前的结果作为起点。  
```  
  
```txt  
 CPU times: user 29.8 s, sys: 7.51 s, total: 37.3 s  
 Wall time: 13.6 s  
Out[65]: DNNClassifier(params={'head':  
 <tensorflow.contrib.learn.python.learn ... head._BinaryLogisticHead  
 object at 0x1a3ee692b0>, 'hidden_units': [250, 250, 250, 250, 250],  
 'feature_columns': (_RealValuedColumn(column_name='features',  
 dimension=1, default_value=None, dtype=tf.float32, normalizer=None),),  
 'optimizer': None, 'activation_fn': <function relu at 0x1a3aa75b70>,  
 'dropout': None, 'gradient_clip_norm': None,  
 'embedding_lr_multipliers': None, 'input_layer_min_slice_size': None})  
```  
  
```python  
In [66]: model.evaluate(input_fn=input_fn, steps=1) # ❽ 重新训练后准确度提高。  
```  
  
```txt  
Out[66]: {'loss': 0.09271307,  
 'accuracy': 0.94,  
 'labels/prediction_mean': 0.5274486,  
 'labels/actual_label_mean': 0.5,  
 'accuracy/baseline_label_mean': 0.5,  
 'auc': 0.99759996,  
 'auc_precision_recall': 0.9977609,  
 'accuracy/threshold_0.500000_mean': 0.94,  
 'precision/positive_threshold_0.500000_mean': 0.9074074,  
 'recall/positive_threshold_0.500000_mean': 0.98,  
 'global_step': 850}  
```  
  
这只是稍微触及了 `TensorFlow` 的皮毛，它被用于许多要求苛刻的用例中，例如Alphabet Inc. 开发自动驾驶汽车的努力。在速度方面，训练 `TensorFlow` 模型通常会因为使用专用硬件（如GPU和TPU）而不是CPU而获得显著的好处。  
  
#### 特征变换  
  
出于许多原因，转换实值特征可能是有益的，甚至是必要的。以下代码展示了一些典型的转换，并在图13-30中可视化了用于比较的结果：  
  
```python  
In [67]: from sklearn import preprocessing  
  
In [68]: X[:5]  
```  
  
```txt  
Out[68]: array([[ 1.6876, -0.7976],  
 [-0.4312, -0.7606],  
 [-1.4393, -1.2363],  
 [ 1.118 , -1.8682],  
 [ 0.0502, 0.659 ]])  
```  
  
```python  
In [69]: Xs = preprocessing.StandardScaler().fit_transform(X) # ❶ 将特征数据转换为均值为零、方差为1的标准正态分布数据。  
 Xs[:5]  
```  
  
```txt  
Out[69]: array([[ 1.2881, -0.5489],  
 [-0.3384, -0.5216],  
 [-1.1122, -0.873 ],  
 [ 0.8509, -1.3399],  
 [ 0.0312, 0.5273]])  
```  
  
```python  
In [70]: Xm = preprocessing.MinMaxScaler().fit_transform(X) # ❷ 按照每个特征的最小和最大值的定义，将特征数据转换为给定范围。  
 Xm[:5]  
```  
  
```txt  
Out[70]: array([[0.7262, 0.3563],  
 [0.3939, 0.3613],  
 [0.2358, 0.2973],  
 [0.6369, 0.2122],  
 [0.4694, 0.5523]])  
```  
  
```python  
In [71]: Xn1 = preprocessing.Normalizer(norm='l1').transform(X) # ❸ 将特征数据单独缩放为单位范数（L1或L2）。  
 Xn1[:5]  
```  
  
```txt  
Out[71]: array([[ 0.6791, -0.3209],  
 [-0.3618, -0.6382],  
 [-0.5379, -0.4621],  
 [ 0.3744, -0.6256],  
 [ 0.0708, 0.9292]])  
```  
  
```python  
In [72]: Xn2 = preprocessing.Normalizer(norm='l2').transform(X)   
 Xn2[:5]  
```  
  
```txt  
Out[72]: array([[ 0.9041, -0.4273],  
 [-0.4932, -0.8699],  
 [-0.7586, -0.6516],  
 [ 0.5135, -0.8581],  
 [ 0.076 , 0.9971]])  
```  
  
```python  
In [73]: plt.figure(figsize=(10, 6))  
 markers = ['o', '.', 'x', '^', 'v']  
 data_sets = [X, Xs, Xm, Xn1, Xn2]  
 labels = ['raw', 'standard', 'minmax', 'norm(1)', 'norm(2)']  
 for x, m, l in zip(data_sets, markers, labels):  
 plt.scatter(x=x[:, 0], y=x[:, 1], c=y,  
 marker=m, cmap='coolwarm', label=l)  
 plt.legend();  
```  
  
<img width="666" height="428" alt="python_for_finance 13 30" src="https://github.com/user-attachments/assets/98f1fdc9-86c1-44b8-b0f9-286942695de7" />
  
图13-30. 原始数据与变换后数据的比较  
  
在模式识别任务方面，转换为分类特征通常是有帮助的，甚至需要这种转换以达到可接受的结果。为此，特征的实值被映射为有限且固定数量的可能整数值（类别、类）：  
  
```python  
In [74]: X[:5]  
```  
  
```txt  
Out[74]: array([[ 1.6876, -0.7976],  
 [-0.4312, -0.7606],  
 [-1.4393, -1.2363],  
 [ 1.118 , -1.8682],  
 [ 0.0502, 0.659 ]])  
```  
  
```python  
In [75]: Xb = preprocessing.Binarizer().fit_transform(X) # ❶ 将特征转换为二元特征。  
 Xb[:5]  
```  
  
```txt  
Out[75]: array([[1., 0.],  
 [0., 0.],  
 [0., 0.],  
 [1., 0.],  
 [1., 1.]])  
```  
  
```python  
In [76]: 2 ** 2 # ❷ 两个二元特征的可能特征值组合的数量。  
```  
  
```txt  
Out[76]: 4  
```  
  
```python  
In [77]: Xd = np.digitize(X, bins=[-1, 0, 1]) # ❸ 基于用于分箱的值列表将特征转换为分类特征。  
 Xd[:5]  
```  
  
```txt  
Out[77]: array([[3, 1],  
 [1, 1],  
 [0, 0],  
 [3, 0],  
 [2, 2]])  
```  
  
```python  
In [78]: 4 ** 2 # ❹ 在两个特征使用三个分箱值的情况下，可能的特征值组合的数量。  
```  
  
```txt  
Out[78]: 16  
```  
  
#### 训练-测试拆分：支持向量机  
  
此时，阅读本文的每一位经验丰富的机器学习研究人员和从业者可能都会对本节中的实现产生担忧：它们全都依赖于相同的数据进行训练、学习和预测。当然，当一方面使用不同的数据（子）集进行训练和学习，另一方面进行测试时，就能更好地判断机器学习算法的质量。这更接近真实世界的应用场景。  
  
同样，`scikit-learn` 提供了一个函数来高效地实现这种方法。特别是，`train_test_split()` 函数允许以随机化但可重复的方式将数据集拆分为训练和测试数据。  
  
以下代码使用了另一种分类算法：支持向量机（SVM）。它首先基于训练数据拟合SVM模型：  
  
```python  
In [79]: from sklearn.svm import SVC  
 from sklearn.model_selection import train_test_split  
  
In [80]: train_x, test_x, train_y, test_y = train_test_split(X, y, test_size=0.33,  
 random_state=0)  
  
In [81]: model = SVC(C=1, kernel='linear')  
  
In [82]: model.fit(train_x, train_y) # ❶ 基于训练数据拟合模型。  
```  
  
```txt  
Out[82]: SVC(C=1, cache_size=200, class_weight=None, coef0=0.0,  
 decision_function_shape='ovr', degree=3, gamma='auto_deprecated',  
 kernel='linear', max_iter=-1, probability=False, random_state=None,  
 shrinking=True, tol=0.001, verbose=False)  
```  
  
```python  
In [83]: pred_train = model.predict(train_x) # ❷ 预测训练数据的标签值。  
  
In [84]: accuracy_score(train_y, pred_train) # ❸ 训练数据预测的准确度（“样本内”）。  
```  
  
```txt  
Out[84]: 0.9402985074626866  
```  
  
接下来，基于测试数据测试拟合后的模型。图13-31显示了测试数据的正确和错误预测。可以自然预料到，测试数据上的准确度要低于训练数据：  
  
```python  
In [85]: pred_test = model.predict(test_x) # ❶ 基于测试数据预测测试数据的标签值。  
  
In [86]: test_y == pred_test   
```  
  
```txt  
Out[86]: array([ True, True, True, True, True, True, True, True, True,  
 True, False, False, False, True, True, True, False, False,  
 False, True, True, True, True, True, True, True, True,  
 True, True, True, True, False, True])  
```  
  
```python  
In [87]: accuracy_score(test_y, pred_test) # ❷ 评估测试数据的拟合模型的准确度（“样本外”）。  
```  
  
```txt  
Out[87]: 0.7878787878787878  
```  
  
```python  
In [88]: test_c = test_x[test_y == pred_test]  
 test_f = test_x[test_y != pred_test]  
  
In [89]: plt.figure(figsize=(10, 6))  
 plt.scatter(x=test_c[:, 0], y=test_c[:, 1], c=test_y[test_y == pred_test],  
 marker='o', cmap='coolwarm')  
 plt.scatter(x=test_f[:, 0], y=test_f[:, 1], c=test_y[test_y != pred_test],  
 marker='x', cmap='coolwarm');  
```  
  
<img width="666" height="424" alt="python_for_finance 13 31" src="https://github.com/user-attachments/assets/00fb6e5e-a0a5-43a7-b686-195881197384" />
  
图13-31. SVM对测试数据的正确（圆点）和错误预测（叉号）  
  
SVM分类算法提供了许多可选的核（kernel）选项。正如以下分析所示，根据手头的问题，不同的核可能会导致截然不同的结果（即准确度得分）。该代码首先将实值特征转换为分类特征：  
  
```python  
In [90]: bins = np.linspace(-4.5, 4.5, 50)  
  
In [91]: Xd = np.digitize(X, bins=bins)  
  
In [92]: Xd[:5]  
```  
  
```txt  
Out[92]: array([[34, 21],  
 [23, 21],  
 [17, 18],  
 [31, 15],  
 [25, 29]])  
```  
  
```python  
In [93]: train_x, test_x, train_y, test_y = train_test_split(Xd, y, test_size=0.33,  
 random_state=0)  
  
In [94]: print('{:>8s} | {:8s}'.format('kernel', 'accuracy'))  
 print(20 * '-')  
 for kernel in ['linear', 'poly', 'rbf', 'sigmoid']:  
 model = SVC(C=1, kernel=kernel, gamma='auto')  
 model.fit(train_x, train_y)  
 acc = accuracy_score(test_y, model.predict(test_x))  
 print('{:>8s} | {:8.3f}'.format(kernel, acc))  
```  
  
```txt  
 kernel | accuracy  
 --------------------  
 linear | 0.848  
 poly | 0.758  
 rbf | 0.788  
 sigmoid | 0.455  
```  
  
## 结论  
  
统计学不仅本身是一门重要的学科，而且还为许多其他学科（如金融学和社会科学）提供了不可或缺的工具。在单单一章中给出这样一门庞大主题的广泛概述是不可能的。因此，本章重点关注四个重要主题，并通过现实案例说明Python和几个统计学库的使用：  
  
### 正态性  
  
关于金融市场收益率的正态性假设对于许多金融理论和应用而言都是至关重要的；因此，能够测试某些时间序列数据是否符合这一假设非常重要。正如第398页“正态性测试”中——通过图表和统计手段——所看到的那样，现实世界中的收益数据通常不呈正态分布。  
  
### 投资组合优化  
  
MPT（现代投资组合理论）侧重于收益的均值和方差/波动率，不仅可以被认为是金融统计学的首批概念性成功之一，而且也是主要的概念性成功之一；在这方面，投资多元化的重要概念得到了完美的说明。  
  
### 贝叶斯统计  
  
一般而言的贝叶斯统计（特别是贝叶斯回归）已成为金融界流行的一种工具，因为这种方法克服了正如第11章中介绍的一些其他方法的缺点；即使数学和形式主义更为复杂，其基本思想——如随着时间的推移更新概率/分布信念——也是很容易掌握的（至少在直觉上）。  
  
### 机器学习  
  
如今，机器学习已经与传统的统计方法和技术一起，在金融领域确立了自己的地位。本章介绍了用于无监督学习（如k-means聚类）和有监督学习（如DNN分类器）的机器学习算法，并说明了相关的特定主题，如特征变换和训练-测试拆分。  
  
## 进一步获取资源的途径  
  
有关本章涵盖的主题和包的更多信息，请参考以下在线资源：  
  
- `SciPy` 统计函数的文档  
- `statsmodels` 库的文档  
- 本章使用的优化函数的详细信息  
- `PyMC3` 的文档  
- `scikit-learn` 的文档  
  
有关更多背景信息的有用书籍参考如下：  
  
- Albon, Chris (2018). Machine Learning with Python Cookbook. Sebastopol, CA: O’Reilly.  
- Alpaydin, Ethem (2016). Machine Learning. Cambridge, MA: MIT Press.  
- Copeland, Thomas, Fred Weston, and Kuldeep Shastri (2005). Financial Theory and Corporate Policy. Boston, MA: Pearson.  
- Downey, Allen (2013). Think Bayes. Sebastopol, CA: O’Reilly.  
- Geweke, John (2005). Contemporary Bayesian Econometrics and Statistics. Hoboken, NJ: John Wiley & Sons.  
- Hastie, Trevor, Robert Tibshirani, and Jerome Friedman (2009). The Elements of Statistical Learning: Data Mining, Inference, and Prediction. New York: Springer.  
- James, Gareth, et al. (2013). An Introduction to Statistical Learning—With Applications in R. New York: Springer.  
- López de Prado, Marcos (2018). Advances in Financial Machine Learning. Hoboken, NJ: John Wiley & Sons.  
- Rachev, Svetlozar, et al. (2008). Bayesian Methods in Finance. Hoboken, NJ: John Wiley & Sons.  
- VanderPlas, Jake (2016). Python Data Science Handbook. Sebastopol, CA: O’Reilly.  
  
介绍现代投资组合理论的论文是：  
  
Markowitz, Harry (1952). “Portfolio Selection.” Journal of Finance, Vol. 7, pp. 77–91.  
