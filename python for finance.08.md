# 第8章 金融时间序列  
  
> 时间使所有事情不会同时发生。  
> ——Ray Cummings  
  
金融时间序列数据是金融中最重要的数据类型之一。这是按日期和/或时间索引的数据。例如，随时间变化的股票价格代表金融时间序列数据。同样，随时间变化的欧元/美元汇率代表一个金融时间序列；汇率在极短的时间间隔内报价，这些报价的集合便构成了汇率的时间序列。  
  
没有任何一门金融学科可以不把时间作为一个重要因素来考虑。这主要与物理学和其他科学相同。在Python中处理时间序列数据的主要工具是pandas。pandas的最初和主要作者Wes McKinney在AQR Capital Management（一家大型对冲基金）担任分析师时开始开发这个库。可以肯定地说，pandas从一开始就被设计为处理金融时间序列数据。  
  
本章主要基于两个逗号分隔值（CSV）文件格式的金融时间序列数据集。本章将按照以下思路展开：  
  
“金融数据”见第206页  
本节介绍使用pandas处理金融时间序列数据的基础知识：数据导入、推导汇总统计信息、计算随时间的变化以及重采样。  
  
“滚动统计”见第217页  
在金融分析中，滚动统计起着重要作用。这些统计信息通常是在一个固定的时间间隔内计算的，并在整个数据集上向前滚动。一个常见的例子是简单移动平均。本节说明了pandas如何支持计算这些统计信息。  
  
“相关性分析”见第222页  
本节提供了一个基于标准普尔500股票指数和VIX波动率指数的金融时间序列数据的案例研究。它为这两个指数呈负相关这一典型（经验）事实提供了一些支持。  
  
“高频数据”见第228页  
本节处理高频数据或分笔（tick）数据，这在金融界已变得司空见惯。pandas再次证明了在处理此类数据集时的强大功能。  
  
### 金融数据  
  
本节处理以CSV文件格式本地存储的金融数据集。从技术上讲，此类文件只是文本文件，其数据行结构的特征是用逗号分隔单个值。在导入数据之前，进行一些包导入和自定义：  
  
```python  
In [1]: import numpy as np  
        import pandas as pd  
        from pylab import mpl, plt  
        plt.style.use('seaborn')  
        mpl.rcParams['font.family'] = 'serif'  
        %matplotlib inline  
```  
  
#### 数据导入  
  
pandas提供了许多不同的函数和DataFrame方法来导入以不同格式（CSV、SQL、Excel等）存储的数据，并将数据导出为不同格式（更多详细信息参见第9章）。以下代码使用`pd.read_csv()`函数从CSV文件导入时间序列数据集：1  
  
1 该文件包含从Thomson Reuters Eikon Data API检索的不同金融工具的日终（EOD）数据。  
  
```python  
In [2]: filename = '../../source/tr_eikon_eod_data.csv' # 指定路径和文件名  
  
In [3]: f = open(filename, 'r') # 打开文件用于读取  
        f.readlines()[:5] # 显示原始数据的前五行（Linux/Mac）  
```  
  
```txt  
Out[3]: ['Date,AAPL.O,MSFT.O,INTC.O,AMZN.O,GS.N,SPY,.SPX,.VIX,EUR=,XAU=,GDX,  
 ,GLD\n',  
 '2010-01-01,,,,,,,,,1.4323,1096.35,,\n',  
 '2010-01-04,30.57282657,30.95,20.88,133.9,173.08,113.33,1132.99,20.04,  
 ,1.4411,1120.0,47.71,109.8\n',  
 '2010-01-05,30.625683660000004,30.96,20.87,134.69,176.14,113.63,1136.52,  
 ,19.35,1.4368,1118.65,48.17,109.7\n',  
 '2010-01-06,30.138541290000003,30.77,20.8,132.25,174.26,113.71,1137.14,  
 ,19.16,1.4412,1138.5,49.34,111.51\n']  
```  
  
```python  
In [4]: data = pd.read_csv(filename, # 传递给pd.read_csv()函数的文件名。  
                           index_col=0, # 指定第一列应作为索引处理。  
                           parse_dates=True) # 指定索引值的类型为datetime。  
  
In [5]: data.info() # 生成的DataFrame对象。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 2216 entries, 2010-01-01 to 2018-06-29  
Data columns (total 12 columns):  
AAPL.O    2138 non-null float64  
MSFT.O    2138 non-null float64  
INTC.O    2138 non-null float64  
AMZN.O    2138 non-null float64  
GS.N      2138 non-null float64  
SPY       2138 non-null float64  
.SPX      2138 non-null float64  
.VIX      2138 non-null float64  
EUR=      2216 non-null float64  
XAU=      2211 non-null float64  
GDX       2138 non-null float64  
GLD       2138 non-null float64  
dtypes: float64(12)  
memory usage: 225.1 KB  
```  
  
在这个阶段，金融分析师可能会初步查看数据，无论是通过检查还是可视化（见图8-1）：  
  
```python  
In [6]: data.head() # 显示前五行数据...  
```  
  
```txt  
Out[6]:  
               AAPL.O   MSFT.O  INTC.O  AMZN.O    GS.N     SPY     .SPX   .VIX \  
Date  
2010-01-01        NaN      NaN     NaN     NaN     NaN     NaN      NaN    NaN  
2010-01-04  30.572827   30.950   20.88  133.90  173.08  113.33  1132.99  20.04  
2010-01-05  30.625684   30.960   20.87  134.69  176.14  113.63  1136.52  19.35  
2010-01-06  30.138541   30.770   20.80  132.25  174.26  113.71  1137.14  19.16  
2010-01-07  30.082827   30.452   20.60  130.00  177.67  114.19  1141.69  19.06  
  
              EUR=     XAU=    GDX     GLD  
Date  
2010-01-01  1.4323  1096.35    NaN     NaN  
2010-01-04  1.4411  1120.00  47.71  109.80  
2010-01-05  1.4368  1118.65  48.17  109.70  
2010-01-06  1.4412  1138.50  49.34  111.51  
2010-01-07  1.4318  1131.90  49.10  110.82  
```  
  
```python  
In [7]: data.tail() # ...并显示最后五行数据。  
```  
  
```txt  
Out[7]:  
            AAPL.O  MSFT.O  INTC.O   AMZN.O    GS.N     SPY     .SPX   .VIX \  
Date  
2018-06-25  182.17   98.39   50.71  1663.15  221.54  271.00  2717.07  17.33  
2018-06-26  184.43   99.08   49.67  1691.09  221.58  271.60  2723.06  15.92  
2018-06-27  184.16   97.54   48.76  1660.51  220.18  269.35  2699.63  17.91  
2018-06-28  185.50   98.63   49.25  1701.45  223.42  270.89  2716.31  16.85  
2018-06-29  185.11   98.61   49.71  1699.80  220.57  271.28  2718.37  16.09  
  
              EUR=     XAU=    GDX     GLD  
Date  
2018-06-25  1.1702  1265.00  22.01  119.89  
2018-06-26  1.1645  1258.64  21.95  119.26  
2018-06-27  1.1552  1251.62  21.81  118.58  
2018-06-28  1.1567  1247.88  21.93  118.22  
2018-06-29  1.1683  1252.25  22.31  118.65  
```  
  
```python  
In [8]: data.plot(figsize=(10, 12), subplots=True); # 通过多个子图可视化完整数据集。  
```  
  
<img width="665" height="731" alt="python_for_finance 8 1" src="https://github.com/user-attachments/assets/b2e61339-53ad-4dc0-a84d-274c054c8c8d" />
  
图8-1. 绘制为线图的金融时间序列数据  
  
所使用的数据来自Thomson Reuters (TR) Eikon Data API。在TR领域，金融工具的符号被称为路透工具代码（RIC）。单一RIC代表的金融工具包括：  
  
```python  
In [9]: instruments = ['Apple Stock', 'Microsoft Stock',  
                       'Intel Stock', 'Amazon Stock', 'Goldman Sachs Stock',  
                       'SPDR S&P 500 ETF Trust', 'S&P 500 Index',  
                       'VIX Volatility Index', 'EUR/USD Exchange Rate',  
                       'Gold Price', 'VanEck Vectors Gold Miners ETF',  
                       'SPDR Gold Trust']  
  
In [10]: for ric, name in zip(data.columns, instruments):  
             print('{:8s} | {}'.format(ric, name))  
```  
  
```txt  
AAPL.O   | Apple Stock  
MSFT.O   | Microsoft Stock  
INTC.O   | Intel Stock  
AMZN.O   | Amazon Stock  
GS.N     | Goldman Sachs Stock  
SPY      | SPDR S&P 500 ETF Trust  
.SPX     | S&P 500 Index  
.VIX     | VIX Volatility Index  
EUR=     | EUR/USD Exchange Rate  
XAU=     | Gold Price  
GDX      | VanEck Vectors Gold Miners ETF  
GLD      | SPDR Gold Trust  
```  
  
#### 汇总统计  
  
金融分析师可能采取的下一步是查看数据集的不同汇总统计信息，以“感受”数据的总体情况：  
  
```python  
In [11]: data.info() # 给出一些关于DataFrame对象的元信息。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 2216 entries, 2010-01-01 to 2018-06-29  
Data columns (total 12 columns):  
AAPL.O    2138 non-null float64  
MSFT.O    2138 non-null float64  
INTC.O    2138 non-null float64  
AMZN.O    2138 non-null float64  
GS.N      2138 non-null float64  
SPY       2138 non-null float64  
.SPX      2138 non-null float64  
.VIX      2138 non-null float64  
EUR=      2216 non-null float64  
XAU=      2211 non-null float64  
GDX       2138 non-null float64  
GLD       2138 non-null float64  
dtypes: float64(12)  
memory usage: 225.1 KB  
```  
  
```python  
In [12]: data.describe().round(2) # 提供每列有用的标准统计信息。  
```  
  
```txt  
Out[12]:  
         AAPL.O   MSFT.O   INTC.O   AMZN.O     GS.N      SPY     .SPX     .VIX \  
count   2138.00  2138.00  2138.00  2138.00  2138.00  2138.00  2138.00  2138.00  
mean      93.46    44.56    29.36   480.46   170.22   180.32  1802.71    17.03  
std       40.55    19.53     8.17   372.31    42.48    48.19   483.34     5.88  
min       27.44    23.01    17.66   108.61    87.70   102.20  1022.58     9.14  
25%       60.29    28.57    22.51   213.60   146.61   133.99  1338.57    13.07  
50%       90.55    39.66    27.33   322.06   164.43   186.32  1863.08    15.58  
75%      117.24    54.37    34.71   698.85   192.13   210.99  2108.94    19.07  
max      193.98   102.49    57.08  1750.08   273.38   286.58  2872.87    48.00  
  
           EUR=     XAU=      GDX      GLD  
count   2216.00  2211.00  2138.00  2138.00  
mean       1.25  1349.01    33.57   130.09  
std        0.11   188.75    15.17    18.78  
min        1.04  1051.36    12.47   100.50  
25%        1.13  1221.53    22.14   117.40  
50%        1.27  1292.61    25.62   124.00  
75%        1.35  1428.24    48.34   139.00  
max        1.48  1898.99    66.63   184.59  
```  
  
> **快速洞察**  
> pandas提供了许多方法来快速概览新导入的金融时间序列数据集，例如`info()`和`describe()`。它们还允许快速检查导入过程是否按预期工作（例如，`DataFrame`对象是否确实具有`DatetimeIndex`类型的索引）。  
  
当然，还有一些选项可以自定义要推导和显示的统计信息类型：  
  
```python  
In [13]: data.mean() # 每列的平均值  
```  
  
```txt  
Out[13]: AAPL.O      93.455973  
         MSFT.O      44.561115  
         INTC.O      29.364192  
         AMZN.O     480.461251  
         GS.N       170.216221  
         SPY        180.323029  
         .SPX      1802.713106  
         .VIX        17.027133  
         EUR=         1.248587  
         XAU=      1349.014130  
         GDX         33.566525  
         GLD        130.086590  
         dtype: float64  
```  
  
```python  
In [14]: data.aggregate([min, # 每列的最小值。  
                         np.mean, # 每列的平均值。  
                         np.std, # 每列的标准差。  
                         np.median, # 每列的中位数。  
                         max] # 每列的最大值。  
                        ).round(2)  
```  
  
```txt  
Out[14]:  
        AAPL.O  MSFT.O  INTC.O   AMZN.O    GS.N     SPY     .SPX   .VIX  EUR= \  
min      27.44   23.01   17.66   108.61   87.70  102.20  1022.58   9.14  1.04  
mean     93.46   44.56   29.36   480.46  170.22  180.32  1802.71  17.03  1.25  
std      40.55   19.53    8.17   372.31   42.48   48.19   483.34   5.88  0.11  
median   90.55   39.66   27.33   322.06  164.43  186.32  1863.08  15.58  1.27  
max     193.98  102.49   57.08  1750.08  273.38  286.58  2872.87  48.00  1.48  
  
           XAU=    GDX     GLD  
min     1051.36  12.47  100.50  
mean    1349.01  33.57  130.09  
std      188.75  15.17   18.78  
median  1292.61  25.62  124.00  
max     1898.99  66.63  184.59  
```  
  
使用`aggregate()`方法还允许传入自定义函数。  
  
#### 随时间的变化  
  
统计分析方法通常基于随时间的变化，而不是绝对值本身。有多种选项可以计算时间序列随时间的变化，包括绝对差值、百分比变化和对数（log）收益率。  
  
首先是绝对差值，pandas为此提供了一个特殊的方法：  
  
```python  
In [15]: data.diff().head() # 提供两个索引值之间的绝对差值。  
```  
  
```txt  
Out[15]:  
              AAPL.O  MSFT.O  INTC.O  AMZN.O  GS.N   SPY  .SPX  .VIX    EUR= \  
Date  
2010-01-01       NaN     NaN     NaN     NaN   NaN   NaN   NaN   NaN     NaN  
2010-01-04       NaN     NaN     NaN     NaN   NaN   NaN   NaN   NaN  0.0088  
2010-01-05  0.052857   0.010   -0.01    0.79  3.06  0.30  3.53 -0.69 -0.0043  
2010-01-06 -0.487142  -0.190   -0.07   -2.44 -1.88  0.08  0.62 -0.19  0.0044  
2010-01-07 -0.055714  -0.318   -0.20   -2.25  3.41  0.48  4.55 -0.10 -0.0094  
  
             XAU=   GDX   GLD  
Date  
2010-01-01    NaN   NaN   NaN  
2010-01-04  23.65   NaN   NaN  
2010-01-05  -1.35  0.46 -0.10  
2010-01-06  19.85  1.17  1.81  
2010-01-07  -6.60 -0.24 -0.69  
```  
  
```python  
In [16]: data.diff().mean() # 当然，也可以额外应用聚合操作。  
```  
  
```txt  
Out[16]: AAPL.O      0.064737  
         MSFT.O      0.031246  
         INTC.O      0.013540  
         AMZN.O      0.706608  
         GS.N        0.028224  
         SPY         0.072103  
         .SPX        0.732659  
         .VIX       -0.019583  
         EUR=       -0.000119  
         XAU=        0.041887  
         GDX        -0.015071  
         GLD        -0.003455  
         dtype: float64  
```  
  
从统计学的角度来看，绝对变化并不是最佳的，因为它们依赖于时间序列数据本身的规模。因此，通常首选百分比变化。以下代码推导了金融背景下的百分比变化或百分比收益率（也称：简单收益率），并可视化了每列的平均值（见图8-2）：  
  
```python  
In [17]: data.pct_change().round(3).head() # 计算两个索引值之间的百分比变化。  
```  
  
```txt  
Out[17]:  
            AAPL.O  MSFT.O  INTC.O  AMZN.O   GS.N    SPY   .SPX    .VIX   EUR= \  
Date  
2010-01-01     NaN     NaN     NaN     NaN    NaN    NaN    NaN     NaN    NaN  
2010-01-04     NaN     NaN     NaN     NaN    NaN    NaN    NaN     NaN  0.006  
2010-01-05   0.002   0.000  -0.000   0.006  0.018  0.003  0.003  -0.034 -0.003  
2010-01-06  -0.016  -0.006  -0.003  -0.018 -0.011  0.001  0.001  -0.010  0.003  
2010-01-07  -0.002  -0.010  -0.010  -0.017  0.020  0.004  0.004  -0.005 -0.007  
  
              XAU=    GDX     GLD  
Date  
2010-01-01     NaN    NaN     NaN  
2010-01-04   0.022    NaN     NaN  
2010-01-05  -0.001  0.010  -0.001  
2010-01-06   0.018  0.024   0.016  
2010-01-07  -0.006 -0.005  -0.006  
```  
  
```python  
In [18]: data.pct_change().mean().plot(kind='bar', figsize=(10, 6)); # 结果的平均值作为条形图可视化。  
```  
  
<img width="666" height="454" alt="python_for_finance 8 2" src="https://github.com/user-attachments/assets/c1eef75e-c9ef-4b7d-8d8e-3de96daa90dc" />
  
图8-2. 作为条形图的百分比变化平均值  
  
作为百分比收益率的替代方案，可以使用对数收益率。在某些情况下，它们更容易处理，因此通常在金融背景下首选。2 图8-3显示了单个金融时间序列的累积对数收益率。这种类型的图表会导致某种形式的归一化：  
  
2 优点之一是随时间推移的加法性，而这对于简单的百分比变化/收益率是不成立的。  
  
```python  
In [19]: rets = np.log(data / data.shift(1)) # 以向量化方式计算对数收益率。  
  
In [20]: rets.head().round(3) # 结果的一个子集。  
```  
  
```txt  
Out[20]:  
            AAPL.O  MSFT.O  INTC.O  AMZN.O   GS.N    SPY   .SPX    .VIX   EUR= \  
Date  
2010-01-01     NaN     NaN     NaN     NaN    NaN    NaN    NaN     NaN    NaN  
2010-01-04     NaN     NaN     NaN     NaN    NaN    NaN    NaN     NaN  0.006  
2010-01-05   0.002   0.000  -0.000   0.006  0.018  0.003  0.003  -0.035 -0.003  
2010-01-06  -0.016  -0.006  -0.003  -0.018 -0.011  0.001  0.001  -0.010  0.003  
2010-01-07  -0.002  -0.010  -0.010  -0.017  0.019  0.004  0.004  -0.005 -0.007  
  
              XAU=    GDX     GLD  
Date  
2010-01-01     NaN    NaN     NaN  
2010-01-04   0.021    NaN     NaN  
2010-01-05  -0.001  0.010  -0.001  
2010-01-06   0.018  0.024   0.016  
2010-01-07  -0.006 -0.005  -0.006  
```  
  
```python  
In [21]: rets.cumsum().apply(np.exp).plot(figsize=(10, 6)); # 绘制随时间推移的累积对数收益率；首先调用cumsum()方法，然后将np.exp()应用于结果。  
```  
  
<img width="666" height="446" alt="python_for_finance 8 3" src="https://github.com/user-attachments/assets/274f9415-c5bb-4e0f-a020-917c13a014eb" />
  
图8-3. 随时间推移的累积对数收益率  
  
#### 重采样  
  
重采样是金融时间序列数据的一项重要操作。通常采用向下采样的形式，这意味着，例如，将分笔数据系列重采样为一分钟间隔，或将每日观测时间序列重采样为具有每周或每月观测结果的时间序列（如图8-4所示）：  
  
```python  
In [22]: data.resample('1w', label='right').last().head() # 将日终（EOD）数据重采样为每周时间间隔...  
```  
  
```txt  
Out[22]:  
               AAPL.O  MSFT.O  INTC.O  AMZN.O    GS.N     SPY     .SPX   .VIX \  
Date  
2010-01-03        NaN     NaN     NaN     NaN     NaN     NaN      NaN    NaN  
2010-01-10  30.282827   30.66   20.83  133.52  174.31  114.57  1144.98  18.13  
2010-01-17  29.418542   30.86   20.80  127.14  165.21  113.64  1136.03  17.91  
2010-01-24  28.249972   28.96   19.91  121.43  154.12  109.21  1091.76  27.31  
2010-01-31  27.437544   28.18   19.40  125.41  148.72  107.39  1073.87  24.62  
  
              EUR=     XAU=    GDX     GLD  
Date  
2010-01-03  1.4323  1096.35    NaN     NaN  
2010-01-10  1.4412  1136.10  49.84  111.37  
2010-01-17  1.4382  1129.90  47.42  110.86  
2010-01-24  1.4137  1092.60  43.79  107.17  
2010-01-31  1.3862  1081.05  40.72  105.96  
```  
  
```python  
In [23]: data.resample('1m', label='right').last().head() # ...以及每月时间间隔。  
```  
  
```txt  
Out[23]:  
               AAPL.O   MSFT.O  INTC.O  AMZN.O    GS.N       SPY     .SPX \  
Date  
2010-01-31  27.437544  28.1800   19.40  125.41  148.72  107.3900  1073.87  
2010-02-28  29.231399  28.6700   20.53  118.40  156.35  110.7400  1104.49  
2010-03-31  33.571395  29.2875   22.29  135.77  170.63  117.0000  1169.43  
2010-04-30  37.298534  30.5350   22.84  137.10  145.20  118.8125  1186.69  
2010-05-31  36.697106  25.8000   21.42  125.46  144.26  109.3690  1089.41  
  
             .VIX    EUR=     XAU=    GDX      GLD  
Date  
2010-01-31  24.62  1.3862  1081.05  40.72  105.960  
2010-02-28  19.50  1.3625  1116.10  43.89  109.430  
2010-03-31  17.59  1.3510  1112.80  44.41  108.950  
2010-04-30  22.05  1.3295  1178.25  50.51  115.360  
2010-05-31  32.07  1.2305  1215.71  49.86  118.881  
```  
  
```python  
In [24]: rets.cumsum().apply(np.exp). resample('1m', label='right').last(  
         ).plot(figsize=(10, 6)); # 这绘制了随时间推移的累积对数收益率：首先调用cumsum()方法，然后将np.exp()应用于结果；最后进行重采样。  
```  
  
<img width="665" height="445" alt="python_for_finance 8 4" src="https://github.com/user-attachments/assets/124e65db-7886-45da-a215-80a77f1cb4ff" />
  
图8-4. 随时间推移的重采样累积对数收益率（月度）  
  
> **避免预见偏差**  
> 在进行重采样时，pandas在许多情况下默认采用区间的左侧标签（或索引值）。为了保持财务一致性，请务必使用右侧标签（索引值），并且通常使用区间内的最后一个可用数据点。否则，预见偏差可能会潜入金融分析中。3  
  
3 预见偏差——或者，就其最强形式而言，完美预见——意味着在财务分析的某个阶段，使用了只有在稍后阶段才能获得的数据。其结果可能是产生“好得令人难以置信”的结果，例如，在回测交易策略时。  
  
### 滚动统计  
  
使用滚动统计（通常也称为财务指标或财务研究）是一种财务传统。例如，此类滚动统计是金融图表分析师和技术交易员的基本工具。本节仅处理单个金融时间序列：  
  
```python  
In [25]: sym = 'AAPL.O'  
  
In [26]: data = pd.DataFrame(data[sym]).dropna()  
  
In [27]: data.tail()  
```  
  
```txt  
Out[27]:                 AAPL.O  
Date  
2018-06-25  182.17  
2018-06-26  184.43  
2018-06-27  184.16  
2018-06-28  185.50  
2018-06-29  185.11  
```  
  
#### 概述  
  
使用pandas推导标准滚动统计信息非常简单：  
  
```python  
In [28]: window = 20 # 定义窗口大小；即要包含的索引值数量。  
  
In [29]: data['min'] = data[sym].rolling(window=window).min() # 计算滚动最小值。  
  
In [30]: data['mean'] = data[sym].rolling(window=window).mean() # 计算滚动平均值。  
  
In [31]: data['std'] = data[sym].rolling(window=window).std() # 计算滚动标准差。  
  
In [32]: data['median'] = data[sym].rolling(window=window).median() # 计算滚动中位数。  
  
In [33]: data['max'] = data[sym].rolling(window=window).max() # 计算滚动最大值。  
  
In [34]: data['ewma'] = data[sym].ewm(halflife=0.5, min_periods=window).mean() # 计算指数加权移动平均值，衰减半衰期为0.5。  
```  
  
要推导更专业的财务指标，通常需要额外的包（例如，请参见第195页“交互式2D绘图”中使用Cufflinks的财务图表）。自定义指标也可以通过`apply()`方法轻松应用。  
  
以下代码显示了结果的子集，并将计算出的某些滚动统计信息进行了可视化（见图8-5）：  
  
```python  
In [35]: data.dropna().head()  
```  
  
```txt  
Out[35]:  
               AAPL.O        min       mean       std     median        max \  
Date  
2010-02-01  27.818544  27.437544  29.580892  0.933650  29.821542  30.719969  
2010-02-02  27.979972  27.437544  29.451249  0.968048  29.711113  30.719969  
2010-02-03  28.461400  27.437544  29.343035  0.950665  29.685970  30.719969  
2010-02-04  27.435687  27.435687  29.207892  1.021129  29.547113  30.719969  
2010-02-05  27.922829  27.435687  29.099892  1.037811  29.419256  30.719969  
  
                 ewma  
Date  
2010-02-01  27.805432  
2010-02-02  27.936337  
2010-02-03  28.330134  
2010-02-04  27.659299  
2010-02-05  27.856947  
```  
  
```python  
In [36]: ax = data[['min', 'mean', 'max']].iloc[-200:].plot(  
             figsize=(10, 6), style=['g--', 'r--', 'g--'], lw=0.8) # 为最后200个数据行绘制三个滚动统计量。  
         data[sym].iloc[-200:].plot(ax=ax, lw=2.0); # 将原始时间序列数据添加到图表中。  
```  
  
<img width="666" height="418" alt="python_for_finance 8 5" src="https://github.com/user-attachments/assets/6ce70db7-ca2b-47b0-be8f-5caf96f31952" />
  
图8-5. 最小值、平均值、最大值的滚动统计  
  
#### 技术分析示例  
  
滚动统计是所谓的股票技术分析的主要工具，相较之下，基本面分析的重点在于，例如财务报告以及正在被分析股票的公司的战略地位。  
  
一种基于技术分析的具有几十年历史的交易策略是使用两条简单移动平均线（SMA）。其思想是，当短期SMA高于长期SMA时，交易者应该做多股票（或一般意义上的金融工具），当情况相反时应做空。这些概念可以通过pandas和`DataFrame`对象的功能来精确实现。  
  
滚动统计通常只在给定窗口参数规格有足够数据时才计算。如图8-6所示，SMA时间序列仅从具有足够数据满足特定参数化的那天开始：  
  
```python  
In [37]: data['SMA1'] = data[sym].rolling(window=42).mean() # 计算短期SMA的值。  
  
In [38]: data['SMA2'] = data[sym].rolling(window=252).mean() # 计算长期SMA的值。  
  
In [39]: data[[sym, 'SMA1', 'SMA2']].tail()  
```  
  
```txt  
Out[39]:             AAPL.O        SMA1        SMA2  
Date  
2018-06-25  182.17  185.606190  168.265556  
2018-06-26  184.43  186.087381  168.418770  
2018-06-27  184.16  186.607381  168.579206  
2018-06-28  185.50  187.089286  168.736627  
2018-06-29  185.11  187.470476  168.901032  
```  
  
```python  
In [40]: data[[sym, 'SMA1', 'SMA2']].plot(figsize=(10, 6)); # 可视化股票价格数据加上两个SMA时间序列。  
```  
  
<img width="665" height="418" alt="python_for_finance 8 6" src="https://github.com/user-attachments/assets/0b696334-59c5-4c80-9bac-4ea30897c0e8" />
  
图8-6. 苹果股票价格和两条简单移动平均线  
  
在这种情况下，SMA只是达到目的的手段。它们用于推导头寸以实施交易策略。图8-7通过值为1来可视化多头头寸，通过值为-1来可视化空头头寸。头寸的变化是由代表SMA时间序列的两条线的交叉（在视觉上）触发的：  
  
```python  
In [41]: data.dropna(inplace=True) # 仅保留完整的数据行。  
  
In [42]: data['positions'] = np.where(data['SMA1'] > data['SMA2'], # 如果短期SMA值大于长期SMA值...  
                                      1, # ...做多股票（填入1）。  
                                      -1) # 否则，做空股票（填入-1）。  
  
In [43]: ax = data[[sym, 'SMA1', 'SMA2', 'positions']].plot(figsize=(10, 6),  
                                                            secondary_y='positions')  
         ax.get_legend().set_bbox_to_anchor((0.25, 0.85));  
```  
  
<img width="666" height="396" alt="python_for_finance 8 7" src="https://github.com/user-attachments/assets/7591f4a0-63ba-46a1-937c-fa79e6764408" />
  
图8-7. 苹果股票价格、两条简单移动平均线及头寸  
  
隐含推导出的交易策略本身仅会导致几笔交易：只有当头寸价值发生变化（即发生交叉）时，才会进行交易。算上开仓和平仓交易，总共只有六笔交易。  
  
### 相关性分析  
  
作为如何使用pandas和金融时间序列数据的进一步说明，请考虑标准普尔500股票指数和VIX波动率指数的案例。一个典型事实是，当标准普尔500指数上涨时，VIX通常会下跌，反之亦然。这与相关性有关，而与因果关系无关。本节演示了如何为标准普尔500指数和VIX呈（高度）负相关的典型事实提供一些支持性的统计证据。4  
  
4 这背后的一个原因是，当股票指数下跌时——例如在危机期间——交易量上升，波动率也随之上升。当股票指数处于上升通道时，投资者通常保持冷静，没有太多动力进行大量交易。特别是，只做多的投资者会试图进一步顺势而为。  
  
#### 数据  
  
数据集现在由两个金融时间序列组成，均在图8-8中可视化：  
  
```python  
In [44]: raw = pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                           index_col=0, parse_dates=True) # 从CSV文件读取EOD数据（最初来自Thomson Reuters Eikon Data API）。  
  
In [45]: data = raw[['.SPX', '.VIX']].dropna()  
```  
  
```python  
In [46]: data.tail()  
```  
  
```txt  
Out[46]:                .SPX   .VIX  
Date  
2018-06-25  2717.07  17.33  
2018-06-26  2723.06  15.92  
2018-06-27  2699.63  17.91  
2018-06-28  2716.31  16.85  
2018-06-29  2718.37  16.09  
```  
  
```python  
In [47]: data.plot(subplots=True, figsize=(10, 6));  
```  
  
<img width="666" height="414" alt="python_for_finance 8 8" src="https://github.com/user-attachments/assets/e75c84c8-8c29-4c18-830d-9debbbac500b" />
  
图8-8. S&P 500和VIX时间序列数据（不同子图）  
  
当在单一图表中绘制这两个时间序列的（部分）并调整缩放比例时，通过简单的视觉检查，两个指数之间呈负相关的典型事实变得显而易见（图8-9）：  
  
```python  
In [48]: data.loc[:'2012-12-31'].plot(secondary_y='.VIX', figsize=(10, 6)); # .loc[:DATE] 选择直到给定值 DATE 的数据。  
```  
  
<img width="666" height="411" alt="python_for_finance 8 9" src="https://github.com/user-attachments/assets/ec43e448-baf4-4d3c-bfb0-4be44e3da962" />
  
图8-9. S&P 500和VIX时间序列数据（同一子图）  
  
#### 对数收益率  
  
如前所述，统计分析通常依赖于收益率而不是绝对变化甚至是绝对值。因此，在进行任何进一步分析之前，我们将首先计算对数收益率。图8-10显示了随时间推移对数收益率的高变异性。在这两个指数中都可以发现所谓的“波动率聚类”。总的来说，股票指数的高波动期伴随着波动率指数的相同现象：  
  
```python  
In [49]: rets = np.log(data / data.shift(1))  
  
In [50]: rets.head()  
```  
  
```txt  
Out[50]:                .SPX      .VIX  
Date  
2010-01-04       NaN       NaN  
2010-01-05  0.003111 -0.035038  
2010-01-06  0.000545 -0.009868  
2010-01-07  0.003993 -0.005233  
2010-01-08  0.002878 -0.050024  
```  
  
```python  
In [51]: rets.dropna(inplace=True)  
  
In [52]: rets.plot(subplots=True, figsize=(10, 6));  
```  
  
<img width="666" height="408" alt="python_for_finance 8 10" src="https://github.com/user-attachments/assets/63613077-02b0-4949-8693-476ee05eedde" />
  
图8-10. S&P 500和VIX随时间推移的对数收益率  
  
在这种情况下，pandas的`scatter_matrix()`绘图函数派上了用场，方便用于可视化。它将两个序列的对数收益率相互绘制成散点图，并且可以在对角线上添加直方图或核密度估计图（KDE）（见图8-11）：  
  
```python  
In [53]: pd.plotting.scatter_matrix(rets, # 要绘制的数据集。  
                                    alpha=0.2, # 圆点不透明度的alpha参数。  
                                    diagonal='hist', # 放置在对角线上的内容；此处为：列数据的直方图。  
                                    hist_kwds={'bins': 35}, # 传递给直方图绘制函数的关键字。  
                                    figsize=(10, 6));  
```  
  
<img width="666" height="416" alt="python_for_finance 8 11" src="https://github.com/user-attachments/assets/5aa43a5b-0f57-4779-b8cf-47bdfd3e3c07" />
  
图8-11. S&P 500和VIX对数收益率的散点矩阵  
  
#### OLS回归  
  
有了所有这些准备工作，实现普通最小二乘法（OLS）回归分析就很方便了。图8-12显示了对数收益率的散点图和穿过点云的线性回归线。斜率显然是负的，这为两个指数之间负相关的典型事实提供了支持：  
  
```python  
In [54]: reg = np.polyfit(rets['.SPX'], rets['.VIX'], deg=1) # 这实现了线性OLS回归。  
  
In [55]: ax = rets.plot(kind='scatter', x='.SPX', y='.VIX', figsize=(10, 6)) # 绘制对数收益率的散点图...  
         ax.plot(rets['.SPX'], np.polyval(reg, rets['.SPX']), 'r', lw=2); # ...并将线性回归线添加到其中。  
```  
  
<img width="665" height="428" alt="python_for_finance 8 12" src="https://github.com/user-attachments/assets/de608053-3a99-432f-b803-d290b8d23514" />
  
图8-12. S&P 500和VIX对数收益率的散点图  
  
#### 相关性  
  
最后，我们直接考虑相关性度量。考虑两种此类度量方法：一种是考虑完整数据集的静态度量，另一种是显示一段时间内固定窗口的相关性的滚动度量。图8-13说明相关性确实随时间而变化，但在给定参数化的情况下，它始终为负。这为S&P 500和VIX指数呈（强）负相关的典型事实提供了强有力的支持：  
  
```python  
In [56]: rets.corr() # 整个DataFrame的相关矩阵。  
```  
  
```txt  
Out[56]:           .SPX      .VIX  
         .SPX  1.000000 -0.804382  
         .VIX -0.804382  1.000000  
```  
  
```python  
In [57]: ax = rets['.SPX'].rolling(window=252).corr(  
              rets['.VIX']).plot(figsize=(10, 6)) # 绘制随时间推移的滚动相关性...  
         ax.axhline(rets.corr().iloc[0, 1], c='r'); # ...并向图中添加一条表示静态值的水平线。  
```  
  
<img width="666" height="407" alt="python_for_finance 8 13" src="https://github.com/user-attachments/assets/7aee43bf-2c67-4a4d-9036-d92f13319a02" />
  
图8-13. S&P 500和VIX之间的相关性（静态和滚动）  
  
### 高频数据  
  
本章内容为使用pandas进行金融时间序列分析。分笔数据集是金融时间序列的一个特例。坦率地说，它们几乎可以用与目前本章中使用的日终（EOD）数据集相同的方法来处理。在pandas中，导入此类数据集通常也非常快。所使用的数据集包含17,352个数据行（另见图8-14）：  
  
```python  
In [59]: %%time  
         # data from FXCM Forex Capital Markets Ltd.  
         tick = pd.read_csv('../../source/fxcm_eur_usd_tick_data.csv',  
                            index_col=0, parse_dates=True)  
```  
  
```txt  
CPU times: user 1.07 s, sys: 149 ms, total: 1.22 s  
Wall time: 1.16 s  
```  
  
```python  
In [60]: tick.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 461357 entries, 2018-06-29 00:00:00.082000 to 2018-06-29  
 20:59:00.607000  
Data columns (total 2 columns):  
Bid    461357 non-null float64  
Ask    461357 non-null float64  
dtypes: float64(2)  
memory usage: 10.6 MB  
```  
  
```python  
In [61]: tick['Mid'] = tick.mean(axis=1) # 计算每个数据行的中间价。  
  
In [62]: tick['Mid'].plot(figsize=(10, 6));  
```  
  
<img width="665" height="405" alt="python_for_finance 8 14" src="https://github.com/user-attachments/assets/53b702b1-4640-4386-93f9-f466b6f47b8d" />
  
图8-14. 欧元/美元汇率的分笔数据  
  
处理分笔数据通常属于需要对金融时间序列数据进行重采样的场景。以下代码将分笔数据重采样为五分钟K线数据（见图8-15），然后可以使用该数据进行例如算法交易策略的回测或实施技术分析：  
  
```python  
In [63]: tick_resam = tick.resample(rule='5min', label='right').last()  
  
In [64]: tick_resam.head()  
```  
  
```txt  
Out[64]:                        Bid      Ask       Mid  
2018-06-29 00:05:00  1.15649  1.15651  1.156500  
2018-06-29 00:10:00  1.15671  1.15672  1.156715  
2018-06-29 00:15:00  1.15725  1.15727  1.157260  
2018-06-29 00:20:00  1.15720  1.15722  1.157210  
2018-06-29 00:25:00  1.15711  1.15712  1.157115  
```  
  
```python  
In [65]: tick_resam['Mid'].plot(figsize=(10, 6));  
```  
  
<img width="665" height="434" alt="python_for_finance 8 15" src="https://github.com/user-attachments/assets/ad0549bb-e605-47ca-87da-8114802a62c2" />
  
图8-15. 欧元/美元汇率的五分钟K线数据  
  
### 结论  
  
本章讨论金融时间序列，这可能是金融领域最重要的数据类型。pandas是一个处理此类数据集的强大包，它不仅可以进行高效的数据分析，还可以轻松进行可视化。pandas还有助于从不同来源读取此类数据集，并将数据集导出为不同的技术文件格式。这将在随后的章节中进行说明。  
  
### 进一步阅读的资源  
  
本章涵盖主题的书面形式的良好参考资料有：  
  
* McKinney, Wes (2017). Python for Data Analysis. Sebastopol, CA: O’Reilly.  
* VanderPlas, Jake (2016). Python Data Science Handbook. Sebastopol, CA: O’Reilly.  
