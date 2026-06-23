# 第15章 交易策略  
  
> 他们愚蠢地认为你可以看看过去就能预测未来。  
> ——The Economist  
  
本章讲述的是算法交易策略的向量化回测。算法交易策略这一术语用于描述任何类型的基于算法的金融交易策略，该算法被设计为能够在没有人类干预的情况下自行在金融工具中建立多头、空头或中立头寸。一个简单的算法，例如“每五分钟在苹果公司股票的多头和中立头寸之间切换一次”，就符合这个定义。就本章的目的而言，更具技术性地说，算法交易策略由一些 Python 代码表示，当新数据可用时，该代码决定是否买入或卖出金融工具，以便在其中建立多头、空头或中立头寸。  
  
本章不提供算法交易策略的概述（有关更详细地涵盖算法交易策略的参考文献，请参阅第519页的“进一步的资源”）。它侧重于针对少数此类策略的向量化回测方法的物理技术层面。使用这种方法，通常将被测试策略的金融数据作为一个整体进行操作，对存储金融数据的 NumPy ndarray 和 pandas DataFrame 对象应用向量化操作。  
  
本章的另一个重点是应用机器学习和深度学习算法来制定算法交易策略。为此，分类算法在历史数据上进行训练，以预测未来的市场方向变动。这通常需要将金融数据从实数值转换为相对较少数量的类别值。这使我们能够利用此类算法的模式识别能力。  
  
本章分为以下几个部分：  
  
“简单移动平均线” 第484页  
本节重点介绍基于简单移动平均线的算法交易策略以及如何回测此类策略。  
  
“随机游走假说” 第491页  
本节介绍随机游走假说。  
  
“线性 OLS 回归” 第494页  
本节着眼于使用 OLS 回归来推导算法交易策略。  
  
“聚类” 第499页  
在本节中，我们探索使用无监督学习算法来推导算法交易策略。  
  
“频率方法” 第501页  
本节介绍一种用于算法交易的简单频率方法。  
  
“分类” 第504页  
在这里，我们研究用于算法交易的机器学习中的分类算法。  
  
“深度神经网络” 第512页  
本节重点介绍深度神经网络以及如何将它们用于算法交易。  
  
## 简单移动平均线  
  
基于简单移动平均线 (SMA) 的交易是一种有几十年历史的交易方法（例如，参见 Brock 等人 (1992) 的论文）。虽然许多交易员将 SMA 用于他们的主观交易，但它们也可以用于制定简单的算法交易策略。本节使用 SMA 来介绍算法交易策略的向量化回测。它建立在第8章中的技术分析示例之上。  
  
### 数据导入  
  
首先，进行一些导入：  
  
```python  
import numpy as np  
import pandas as pd  
import datetime as dt  
from pylab import mpl, plt  
  
plt.style.use('seaborn')  
mpl.rcParams['font.family'] = 'serif'  
%matplotlib inline  
```  
  
其次，读取原始数据并选择单个代码（苹果公司股票 AAPL.O）的金融时间序列。本节中的分析基于日终数据；后续小节将使用盘中数据：  
  
```python  
raw = pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                  index_col=0, parse_dates=True)  
  
raw.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 2216 entries, 2010-01-01 to 2018-06-29  
Data columns (total 12 columns):  
AAPL.O      2138 non-null float64  
MSFT.O      2138 non-null float64  
INTC.O      2138 non-null float64  
AMZN.O      2138 non-null float64  
GS.N        2138 non-null float64  
SPY         2138 non-null float64  
.SPX        2138 non-null float64  
.VIX        2138 non-null float64  
EUR=        2216 non-null float64  
XAU=        2211 non-null float64  
GDX         2138 non-null float64  
GLD         2138 non-null float64  
dtypes: float64(12)  
memory usage: 225.1 KB  
```  
  
```python  
symbol = 'AAPL.O'  
  
data = (  
    pd.DataFrame(raw[symbol])  
    .dropna()  
)  
```  
  
### 交易策略  
  
第三步，计算两个不同滚动窗口大小的 SMA 值。图15-1直观地展示了这三个时间序列：  
  
```python  
SMA1 = 42  
SMA2 = 252  
  
data['SMA1'] = data[symbol].rolling(SMA1).mean() # ❶ 计算较短 SMA 的值。  
data['SMA2'] = data[symbol].rolling(SMA2).mean() # ❷ 计算较长 SMA 的值。  
  
data.plot(figsize=(10, 6));  
```  
  
图15-1. 苹果公司股票价格和两条简单移动平均线  
  
<img width="667" height="419" alt="python_for_finance 15 1" src="https://github.com/user-attachments/assets/3917762a-ea32-422e-99a1-b8fd1fce7c33" />
  
第四步，推导头寸。交易规则如下：  
  
- 当较短的 SMA 高于较长的 SMA 时，做多 (= +1)。  
- 当较短的 SMA 低于较长的 SMA 时，做空 (= -1)。  
  
头寸在图15-2中进行了可视化：  
  
```python  
data.dropna(inplace=True)  
  
data['Position'] = np.where(data['SMA1'] > data['SMA2'], 1, -1) # ❶ np.where(cond, a, b) 逐元素计算条件 cond，当为 True 时放置 a，否则放置 b。  
  
data.tail()  
```  
  
```txt  
                  AAPL.O        SMA1        SMA2  Position  
Date  
2018-06-25      182.17  185.606190  168.265556         1  
2018-06-26      184.43  186.087381  168.418770         1  
2018-06-27      184.16  186.607381  168.579206         1  
2018-06-28      185.50  187.089286  168.736627         1  
2018-06-29      185.11  187.470476  168.901032         1  
```  
  
```python  
ax = data.plot(secondary_y='Position', figsize=(10, 6))  
ax.get_legend().set_bbox_to_anchor((0.25, 0.85));  
```  
  
图15-2. 苹果公司股票价格、两条 SMA 及产生的头寸  
  
<img width="666" height="395" alt="python_for_finance 15 2" src="https://github.com/user-attachments/assets/9868c6bb-9d98-4f87-98df-609a3fd0c389" />
  
这复现了第8章中得出的结果。那里没有讨论的是，遵循交易规则——即实施算法交易策略——是否优于在整个期间简单地做多苹果股票的基准情况。鉴于该策略仅导致两个做空苹果股票的时期，业绩上的差异只能是由这两个时期产生的。  
  
### 向量化回测  
  
向量化回测现在可以实现如下。首先，计算对数收益率。然后，将表示为 +1 或 -1 的头寸乘以相关的对数收益率。这种简单的计算是可行的，因为多头头寸赚取苹果股票的收益，而空头头寸赚取苹果股票的负收益。最后，需要将苹果股票的对数收益率和基于 SMA 的算法交易策略的对数收益率加起来，并应用指数函数得出绝对表现值：  
  
```python  
data['Returns'] = np.log(data[symbol] / data[symbol].shift(1)) # ❶ 计算苹果公司股票（即基准投资）的对数收益率。  
  
data['Strategy'] = data['Position'].shift(1) * data['Returns'] # ❷ 将平移一天的头寸值乘以苹果股票的对数收益率；平移操作是为了避免前瞻性偏差。  
  
data.round(4).head()  
```  
  
```txt  
                  AAPL.O     SMA1     SMA2  Position  Returns  Strategy  
Date  
2010-12-31     46.0800  45.2810  37.1207         1      NaN       NaN  
2011-01-03     47.0814  45.3497  37.1862         1   0.0215    0.0215  
2011-01-04     47.3271  45.4126  37.2525         1   0.0052    0.0052  
2011-01-05     47.7142  45.4661  37.3223         1   0.0081    0.0081  
2011-01-06     47.6757  45.5226  37.3921         1  -0.0008   -0.0008  
```  
  
```python  
data.dropna(inplace=True)  
  
np.exp(data[['Returns', 'Strategy']].sum()) # ❸ 将策略和基准投资的对数收益率求和，并计算指数值以得出绝对表现。  
```  
  
```txt  
Returns     4.017148  
Strategy    5.811299  
dtype: float64  
```  
  
```python  
data[['Returns', 'Strategy']].std() * 252 ** 0.5 # ❹ 计算策略和基准投资的年化波动率。  
```  
  
```txt  
Returns     0.250571  
Strategy    0.250407  
dtype: float64  
```  
  
这些数字表明，算法交易策略确实跑赢了被动持有苹果股票的基准投资。由于策略的类型和特点，年化波动率相同，因此在风险调整后的基础上，它也跑赢了基准投资。  
  
为了更好地了解整体表现，图15-3展示了苹果股票和算法交易策略随时间推移的表现：  
  
```python  
ax = data[['Returns', 'Strategy']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6))  
data['Position'].plot(ax=ax, secondary_y='Position', style='--')  
ax.get_legend().set_bbox_to_anchor((0.25, 0.85));  
```  
  
<img width="665" height="404" alt="python_for_finance 15 3" src="https://github.com/user-attachments/assets/0781bd97-81ff-480e-a8e4-df16824a239b" />
  
图15-3. 苹果股票和基于 SMA 的交易策略随时间的表现  
  
**简化假设**  
  
本小节中介绍的向量化回测方法基于许多简化假设。其中，未包括交易成本（固定费用、买卖价差、借贷成本等）。对于在多年内仅产生几笔交易的交易策略来说，这可能是合理的。还假设所有交易都以苹果股票的日终收盘价进行。更现实的回测方法会将这些和其他（市场微观结构）因素考虑在内。  
  
### 优化  
  
自然会产生一个问题：所选参数 SMA1=42 和 SMA2=252 是否是“正确”的。在其他条件相同的情况下，投资者通常更喜欢较高的收益而不是较低的收益。因此，人们可能会倾向于寻找在相关时期内使收益最大化的参数。为此，可以使用暴力方法，针对不同的参数组合简单地重复整个向量化回测过程，记录结果并随后进行排名。这就是以下代码所做的事情：  
  
```python  
from itertools import product  
  
sma1 = range(20, 61, 4) # ❶ 指定 SMA1 的参数值。  
sma2 = range(180, 281, 10) # ❷ 指定 SMA2 的参数值。  
  
results = pd.DataFrame()  
for SMA1, SMA2 in product(sma1, sma2): # ❸ 将 SMA1 的所有值与 SMA2 的那些值进行组合。  
    data = pd.DataFrame(raw[symbol])  
    data.dropna(inplace=True)  
    data['Returns'] = np.log(data[symbol] / data[symbol].shift(1))  
    data['SMA1'] = data[symbol].rolling(SMA1).mean()  
    data['SMA2'] = data[symbol].rolling(SMA2).mean()  
    data.dropna(inplace=True)  
    data['Position'] = np.where(data['SMA1'] > data['SMA2'], 1, -1)  
    data['Strategy'] = data['Position'].shift(1) * data['Returns']  
    data.dropna(inplace=True)  
    perf = np.exp(data[['Returns', 'Strategy']].sum())  
    results = results.append(pd.DataFrame(  
        {'SMA1': SMA1, 'SMA2': SMA2,  
         'MARKET': perf['Returns'],  
         'STRATEGY': perf['Strategy'],  
         'OUT': perf['Strategy'] - perf['Returns']},  
        index=[0]), ignore_index=True) # ❹ 将向量化回测结果记录在 DataFrame 对象中。  
```  
  
以下代码给出了结果的概览，并显示了所有被回测的组合中表现最好的七个参数组合。排名是根据算法交易策略相较于基准投资的超额表现进行的。由于 SMA2 参数的选择影响了向量化回测执行所在的时间间隔和数据集的长度，基准投资的表现也会有所不同：  
  
```python  
results.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 121 entries, 0 to 120  
Data columns (total 5 columns):  
SMA1        121 non-null int64  
SMA2        121 non-null int64  
MARKET      121 non-null float64  
STRATEGY    121 non-null float64  
OUT         121 non-null float64  
dtypes: float64(3), int64(2)  
memory usage: 4.8 KB  
```  
  
```python  
results.sort_values('OUT', ascending=False).head(7)  
```  
  
```txt  
      SMA1   SMA2    MARKET  STRATEGY       OUT  
56      40    190  4.650342  7.175173  2.524831  
39      32    240  4.045619  6.558690  2.513071  
59      40    220  4.220272  6.544266  2.323994  
46      36    200  4.074753  6.389627  2.314874  
55      40    180  4.574979  6.857989  2.283010  
70      44    220  4.220272  6.469843  2.249571  
101     56    200  4.074753  6.319524  2.244772  
```  
  
根据基于暴力的优化结果，SMA1=40 和 SMA2=190 是最优参数，导致了约 230 个百分点的超额表现。然而，这个结果严重依赖于所使用的数据集，并且容易出现过拟合。更严谨的方法是在一个数据集（样本内或训练数据集）上进行优化，并在另一个数据集（样本外或测试数据集）上进行测试。  
  
**过拟合**  
  
通常，算法交易策略背景下的任何类型的优化、拟合或训练都容易产生所谓的过拟合问题。这意味着所选取的参数可能在所使用的数据集上表现（极其）良好，但在其他数据集或实际操作中可能表现（极其）糟糕。  
  
## 随机游走假说  
  
上一节引入了向量化回测作为回测算法交易策略的有效工具。基于单个金融时间序列（即苹果股票的历史日终价格）回测的单一策略，在同一时期内跑赢了仅仅做多苹果股票的基准投资。  
  
尽管性质相当具体，但这些结果与随机游走假说 (RWH) 的预测相悖，该假说指出，此类预测性方法根本不应产生任何超额收益。RWH 假设金融市场中的价格遵循随机游走，或者在连续时间中，遵循没有漂移的算术布朗运动。没有漂移的算术布朗运动在未来任意点的期望值等于它今天的值。因此，在最小二乘意义上，如果 RWH 适用，明天价格的最佳预测因素就是今天的价格。  
  
这些影响在以下引文中得到了总结：  
  
> 多年来，经济学家、统计学家和金融学教师一直对开发和测试股票价格行为的模型感兴趣。从这项研究中演变出来的一个重要模型是随机游走理论。该理论对许多其他用于描述和预测股票价格行为的方法——这些方法在学术界之外相当受欢迎——提出了严重质疑。例如，我们稍后将看到，如果随机游走理论是对现实的准确描述，那么用于预测股票价格的各种“技术”或“图表”程序就完全没有价值了。  
> ——Eugene F. Fama (1965)  
  
RWH 与有效市场假说 (EMH) 是一致的，非技术性地讲，该假说指出市场价格反映了“所有可用信息”。通常会区分不同程度的效率，如弱式、半强式和强式，从而更具体地定义“所有可用信息”包含的内容。在形式上，这种定义在理论上可以基于信息集的概念，在编程目的上可以基于数据集，如下面的引文所示：  
  
> 如果不可能通过基于信息集 S 的交易获得经济利润，那么相对于信息集 S，市场是有效的。  
> ——Michael Jensen (1978)  
  
使用 Python，可以针对特定情况测试 RWH，方法如下：使用历史市场价格的金融时间序列，并创建若干个其滞后版本——例如五个。然后，使用 OLS 回归基于先前创建的滞后市场价格来预测市场价格。基本思想是，昨天以及更早四天的市场价格可以用来预测今天的市场价格。  
  
以下 Python 代码实现了这一想法，并创建了标普 500 股票指数历史日终收盘水平的五个滞后版本：  
  
```python  
symbol = '.SPX'  
  
data = pd.DataFrame(raw[symbol])  
  
lags = 5  
cols = []  
for lag in range(1, lags + 1):  
    col = 'lag_{}'.format(lag) # ❶ 定义当前滞后值的列名。  
    data[col] = data[symbol].shift(lag) # ❷ 为当前滞后值创建市场价格的滞后版本。  
    cols.append(col) # ❸ 收集列名以供后续参考。  
  
data.head(7)  
```  
  
```txt  
              .SPX   lag_1   lag_2    lag_3    lag_4    lag_5  
Date  
2010-01-01      NaN     NaN     NaN      NaN      NaN      NaN  
2010-01-04  1132.99     NaN     NaN      NaN      NaN      NaN  
2010-01-05  1136.52  1132.99     NaN      NaN      NaN      NaN  
2010-01-06  1137.14  1136.52  1132.99     NaN      NaN      NaN  
2010-01-07  1141.69  1137.14  1136.52  1132.99      NaN      NaN  
2010-01-08  1144.98  1141.69  1137.14  1136.52  1132.99      NaN  
2010-01-11  1146.98  1144.98  1141.69  1137.14  1136.52  1132.99  
```  
  
```python  
data.dropna(inplace=True)  
```  
  
使用 NumPy，OLS 回归的实现非常简单。正如最优回归参数所示，lag_1 确实是基于 OLS 回归预测市场价格时最重要的参数。它的值接近于 1。其他四个值则相当接近于 0。图15-4可视化了最优回归参数值。  
  
<img width="666" height="428" alt="python_for_finance 15 4" src="https://github.com/user-attachments/assets/c96ba431-ca86-46db-a2be-3ec42b153213" />
  
图15-4. 用于价格预测的 OLS 回归最优回归参数  
  
当使用最优结果将预测值与标普 500 指数的原始指数值进行比较时，从图15-5中可以明显看出，lag_1 基本上就是用来得出预测值的指标。图形上来看，图15-5中的预测线就是原始时间序列向右平移了一天（带有一些微小的调整）。  
  
<img width="666" height="432" alt="python_for_finance 15 5" src="https://github.com/user-attachments/assets/987d7fcf-63b1-48f7-b2f8-3a131a666bb9" />
  
图15-5. 标普 500 指数水平与 OLS 回归预测值的比较  
  
总而言之，本节中的简要分析显示出对 RWH 和 EMH 的一些支持。诚然，该分析仅针对单个股票指数进行，并且使用了相当特定的参数设置——但可以很容易地将其扩展到合并跨多个资产类别的多种金融工具、不同的滞后数量值等。总的来说，你会发现结果在定性上基本相同。毕竟，RWH 和 EMH 属于具有广泛实证支持的金融理论。从这个意义上讲，任何算法交易策略都必须通过证明 RWH 在一般情况下不适用来证明其价值。这确实是一个艰难的障碍。  
  
## 线性 OLS 回归  
  
本节应用线性 OLS 回归，基于历史对数收益率来预测市场变动的方向。为了保持简单，只使用两个特征。第一个特征 (lag_1) 代表滞后一天的金融时间序列的对数收益率。第二个特征 (lag_2) 将对数收益率滞后两天。与价格相反，对数收益率在通常情况下是平稳的，这通常是应用统计和机器学习算法的必要条件。  
  
使用滞后对数收益率作为特征的基本思想是，它们可能对预测未来收益具有信息价值。例如，人们可能假设在两次向下移动之后，向上移动的可能性更大（“均值回归”），或者相反，再次向下移动的可能性更大（“动量”或“趋势”）。回归技术的应用允许将此类非正式的推理形式化。  
  
### 数据  
  
首先是数据集的导入和准备。图15-6显示了 EUR/USD 汇率历史日对数收益率的频率分布。它们是作为后续所用的特征以及标签的基础：  
  
```python  
raw = pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                  index_col=0, parse_dates=True).dropna()  
  
raw.columns  
```  
  
```txt  
Index(['AAPL.O', 'MSFT.O', 'INTC.O', 'AMZN.O', 'GS.N', 'SPY', '.SPX',  
       '.VIX', 'EUR=', 'XAU=', 'GDX', 'GLD'],  
      dtype='object')  
```  
  
```python  
symbol = 'EUR='  
  
data = pd.DataFrame(raw[symbol])  
  
data['returns'] = np.log(data / data.shift(1))  
  
data.dropna(inplace=True)  
  
data['direction'] = np.sign(data['returns']).astype(int)  
  
data.head()  
```  
  
```txt  
              EUR=   returns  direction  
Date  
2010-01-05  1.4368 -0.002988         -1  
2010-01-06  1.4412  0.003058          1  
2010-01-07  1.4318 -0.006544         -1  
2010-01-08  1.4412  0.006544          1  
2010-01-11  1.4513  0.006984          1  
```  
  
```python  
data['returns'].hist(bins=35, figsize=(10, 6));  
```  
  
<img width="666" height="427" alt="python_for_finance 15 6" src="https://github.com/user-attachments/assets/d1796023-182c-44f9-be37-08f451aebf2f" />
  
图15-6. EUR/USD 汇率对数收益率的直方图  
  
其次，通过将对数收益率滞后来创建特征数据并结合收益率数据将其可视化的代码（见图15-7）：  
  
```python  
lags = 2  
  
def create_lags(data):  
    global cols  
    cols = []  
    for lag in range(1, lags + 1):  
        col = 'lag_{}'.format(lag)  
        data[col] = data['returns'].shift(lag)  
        cols.append(col)  
  
create_lags(data)  
  
data.head()  
```  
  
```txt  
              EUR=   returns  direction     lag_1     lag_2  
Date  
2010-01-05  1.4368 -0.002988         -1       NaN       NaN  
2010-01-06  1.4412  0.003058          1 -0.002988       NaN  
2010-01-07  1.4318 -0.006544         -1  0.003058 -0.002988  
2010-01-08  1.4412  0.006544          1 -0.006544  0.003058  
2010-01-11  1.4513  0.006984          1  0.006544 -0.006544  
```  
  
```python  
data.dropna(inplace=True)  
  
data.plot.scatter(x='lag_1', y='lag_2', c='returns',  
                  cmap='coolwarm', figsize=(10, 6), colorbar=True)  
plt.axvline(0, c='r', ls='--')  
plt.axhline(0, c='r', ls='--');  
```  
  
<img width="665" height="404" alt="python_for_finance 15 7" src="https://github.com/user-attachments/assets/ba39ddde-62de-46e5-a4cd-2191fe68d4f9" />
  
图15-7. 基于特征和标签数据的散点图  
  
### 回归  
  
数据集准备完毕后，可以应用线性 OLS 回归来了解任何潜在的（线性）关系，基于特征预测市场变动，并回测基于预测的交易策略。这里有两种基本方法可用：在回归过程中使用对数收益率或仅使用方向数据作为因变量。无论哪种情况，预测结果都是实数值，因此会转换为 +1 或 -1 以仅使用预测的方向：  
  
```python  
from sklearn.linear_model import LinearRegression # ❶ 使用 scikit-learn 的线性 OLS 回归实现。  
  
model = LinearRegression()  
  
data['pos_ols_1'] = model.fit(data[cols],  
     data['returns']).predict(data[cols]) # ❷ 回归直接在对数收益率上实现...  
  
data['pos_ols_2'] = model.fit(data[cols],  
     data['direction']).predict(data[cols]) # ❸ ...以及在主要关注的方向数据上实现。  
  
data[['pos_ols_1', 'pos_ols_2']].head()  
```  
  
```txt  
            pos_ols_1  pos_ols_2  
Date  
2010-01-07  -0.000166  -0.000086  
2010-01-08   0.000017   0.040404  
2010-01-11  -0.000244  -0.011756  
2010-01-12  -0.000139  -0.043398  
2010-01-13  -0.000022   0.002237  
```  
  
```python  
data[['pos_ols_1', 'pos_ols_2']] = np.where(  
            data[['pos_ols_1', 'pos_ols_2']] > 0, 1, -1) # ❹ 实值预测被转换为方向值（+1, -1）。  
  
data['pos_ols_1'].value_counts() # ❺ 这两种方法通常会产生不同的方向预测。  
```  
  
```txt  
-1    1847  
 1     288  
Name: pos_ols_1, dtype: int64  
```  
  
```python  
data['pos_ols_2'].value_counts() # ❺   
```  
  
```txt  
 1    1377  
-1     758  
Name: pos_ols_2, dtype: int64  
```  
  
```python  
(data['pos_ols_1'].diff() != 0).sum() # ❻ 然而，两者都会导致随着时间推移产生相对大量的交易。  
```  
  
```txt  
555  
```  
  
```python  
(data['pos_ols_2'].diff() != 0).sum() # ❻   
```  
  
```txt  
762  
```  
  
配备了方向预测之后，就可以应用向量化回测来评判由此产生的交易策略的表现。在这个阶段，分析基于许多简化假设，例如“零交易成本”，以及训练和测试使用相同的数据集。然而，在这些假设下，两种基于回归的策略都跑赢了基准的被动投资，尽管只有在市场方向上训练的策略显示出整体表现为正（图15-8）：  
  
```python  
data['strat_ols_1'] = data['pos_ols_1'] * data['returns']  
  
data['strat_ols_2'] = data['pos_ols_2'] * data['returns']  
  
data[['returns', 'strat_ols_1', 'strat_ols_2']].sum().apply(np.exp)  
```  
  
```txt  
returns        0.810644  
strat_ols_1    0.942422  
strat_ols_2    1.339286  
dtype: float64  
```  
  
```python  
(data['direction'] == data['pos_ols_1']).value_counts() # ❶ 显示策略做出正确和错误预测的数量。  
```  
  
```txt  
False    1093  
True     1042  
dtype: int64  
```  
  
```python  
(data['direction'] == data['pos_ols_2']).value_counts() # ❶   
```  
  
```txt  
True     1096  
False    1039  
dtype: int64  
```  
  
```python  
data[['returns', 'strat_ols_1', 'strat_ols_2']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="666" height="419" alt="python_for_finance 15 8" src="https://github.com/user-attachments/assets/e0e22478-5587-47e9-9933-f8dfd1aecab7" />
  
图15-8. EUR/USD 与基于回归的策略随时间的表现  
  
## 聚类  
  
本节将第444页“机器学习”中介绍的 k 均值聚类应用于金融时间序列数据，以自动得出用于制定交易策略的簇。其思想是该算法能识别出预测向上变动或向下变动的两个特征值簇。  
  
以下代码将 k 均值算法应用于之前使用的两个特征。图15-9将这两个簇进行了可视化：  
  
```python  
from sklearn.cluster import KMeans  
  
model = KMeans(n_clusters=2, random_state=0) # ❶ 为算法选择了两个簇。  
  
model.fit(data[cols])  
```  
  
```txt  
KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,  
       n_clusters=2, n_init=10, n_jobs=None, precompute_distances='auto',  
       random_state=0, tol=0.0001, verbose=0)  
```  
  
```python  
data['pos_clus'] = model.predict(data[cols])  
  
data['pos_clus'] = np.where(data['pos_clus'] == 1, -1, 1) # ❷ 给定簇值，选择头寸。  
  
data['pos_clus'].values  
```  
  
```txt  
array([-1,  1, -1, ...,  1,  1, -1])  
```  
  
```python  
plt.figure(figsize=(10, 6))  
plt.scatter(data[cols].iloc[:, 0], data[cols].iloc[:, 1],  
            c=data['pos_clus'], cmap='coolwarm');  
```  
  
<img width="666" height="411" alt="python_for_finance 15 9" src="https://github.com/user-attachments/assets/70652c4c-103e-449c-a008-4a4d23e03f90" />
  
图15-9. 由 k 均值算法识别的两个簇  
  
诚然，在这种背景下，这种方法相当武断——毕竟，算法如何知道我们要寻找什么？然而，最终产生的交易策略与基准被动投资相比，确实显示出了轻微的超额表现（见图15-10）。值得注意的是，没有给出任何指导（监督），而且命中率（即正确预测的数量占做出所有预测数量的比例）不到 50%：  
  
```python  
data['strat_clus'] = data['pos_clus'] * data['returns']  
  
data[['returns', 'strat_clus']].sum().apply(np.exp)  
```  
  
```txt  
returns       0.810644  
strat_clus    1.277133  
dtype: float64  
```  
  
```python  
(data['direction'] == data['pos_clus']).value_counts()  
```  
  
```txt  
True     1077  
False    1058  
dtype: int64  
```  
  
```python  
data[['returns', 'strat_clus']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="666" height="420" alt="python_for_finance 15 10" src="https://github.com/user-attachments/assets/f7b27934-70b8-4123-b7b4-871a64e7ef52" />
  
图15-10. EUR/USD 和基于 k 均值的策略随时间的表现  
  
## 频率方法  
  
超越更复杂的算法和技术，人们可能会想出利用频率方法来预测金融市场方向变动的主意。为此，人们可以将两个实值特征转化为二元特征，并在给定两个二元特征的四种可能组合 ((0, 0), (0, 1), (1, 0), (1, 1)) 的情况下，从历史观察结果中评估向上和向下移动的概率。  
  
利用 pandas 的数据分析能力，这种方法相对容易实现：  
  
```python  
def create_bins(data, bins=[0]):  
    global cols_bin  
    cols_bin = []  
    for col in cols:  
        col_bin = col + '_bin'  
        data[col_bin] = np.digitize(data[col], bins=bins) # ❶ 根据 bins 参数对特征值进行数字化。  
        cols_bin.append(col_bin)  
  
create_bins(data)  
  
data[cols_bin + ['direction']].head() # ❷ 显示数字化的特征值和标签值。  
```  
  
```txt  
            lag_1_bin  lag_2_bin  direction  
Date  
2010-01-07          1          0         -1  
2010-01-08          0          1          1  
2010-01-11          1          0          1  
2010-01-12          1          1         -1  
2010-01-13          0          1          1  
```  
  
```python  
grouped = data.groupby(cols_bin + ['direction'])  
grouped.size() # ❸ 显示在特征值组合条件下可能变动的频率。  
```  
  
```txt  
lag_1_bin  lag_2_bin  direction  
0          0          -1           239  
                       1           258  
           1          -1           262  
                       1           288  
1          0          -1           272  
                       1           278  
           1          -1           278  
                       1           251  
dtype: int64  
```  
  
```python  
res = grouped['direction'].size().unstack(fill_value=0) # ❹ 转换 DataFrame 对象，使其频率在列中。  
  
def highlight_max(s):  
    is_max = s == s.max()  
    return ['background-color: yellow' if v else '' for v in is_max] # ❺ 突出显示每个特征值组合的最高频率值。  
  
res.style.apply(highlight_max, axis=1) # ❺   
```  
  
```txt  
<pandas.io.formats.style.Styler at 0x1a194216a0>  
```  
  
给定频率数据，三个特征值组合暗示向下移动，而一个则让向上移动显得更有可能。这可以转化为一种交易策略，其表现显示在图15-11中：  
  
```python  
data['pos_freq'] = np.where(data[cols_bin].sum(axis=1) == 2, -1, 1) # ❶ 将给定频率的发现转化为交易策略。  
  
(data['direction'] == data['pos_freq']).value_counts()  
```  
  
```txt  
True     1102  
False    1033  
dtype: int64  
```  
  
```python  
data['strat_freq'] = data['pos_freq'] * data['returns']  
  
data[['returns', 'strat_freq']].sum().apply(np.exp)  
```  
  
```txt  
returns       0.810644  
strat_freq    0.989513  
dtype: float64  
```  
  
```python  
data[['returns', 'strat_freq']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="667" height="437" alt="python_for_finance 15 11" src="https://github.com/user-attachments/assets/0a6af1e6-3db5-4340-8b56-d0fc677b9b59" />
  
图15-11. EUR/USD 和基于频率的交易策略随时间的表现  
  
## 分类  
  
本节将机器学习中的分类算法（正如第444页“机器学习”中所介绍的）应用于预测金融市场价格变动方向的问题中。凭借该背景和前几节中的示例，应用逻辑回归 (logistic regression)、高斯朴素贝叶斯 (Gaussian Naive Bayes) 和支持向量机 (support vector machine) 方法就像将它们应用于更小的样本数据集一样简单。  
  
### 两个二元特征  
  
首先，基于二元特征值进行模型拟合以及随后的头寸值推导：  
  
```python  
from sklearn import linear_model  
from sklearn.naive_bayes import GaussianNB  
from sklearn.svm import SVC  
  
C = 1  
  
models = {  
    'log_reg': linear_model.LogisticRegression(C=C),  
    'gauss_nb': GaussianNB(),  
    'svm': SVC(C=C)  
}  
  
def fit_models(data): # ❶ 一个拟合所有模型的函数。  
    mfit = {model: models[model].fit(data[cols_bin],  
                                     data['direction'])  
            for model in models.keys()}  
  
fit_models(data)  
  
def derive_positions(data): # ❷ 一个从拟合模型推导所有头寸值的函数。  
    for model in models.keys():  
        data['pos_' + model] = models[model].predict(data[cols_bin])  
  
derive_positions(data)  
```  
  
其次，所得交易策略的向量化回测。图15-12可视化了随时间推移的表现：  
  
```python  
def evaluate_strats(data): # ❶ 一个评估所有产生的交易策略的函数。  
    global sel  
    sel = []  
    for model in models.keys():  
        col = 'strat_' + model  
        data[col] = data['pos_' + model] * data['returns']  
        sel.append(col)  
    sel.insert(0, 'returns')  
  
evaluate_strats(data)  
  
sel.insert(1, 'strat_freq')  
  
data[sel].sum().apply(np.exp) # ❷ 某些策略可能表现出完全相同的性能。  
```  
  
```txt  
returns           0.810644  
strat_freq        0.989513  
strat_log_reg     1.243322  
strat_gauss_nb    1.243322  
strat_svm         0.989513  
dtype: float64  
```  
  
```python  
data[sel].cumsum().apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="443" alt="python_for_finance 15 12" src="https://github.com/user-attachments/assets/89edbd1c-2882-4040-b00a-652f97a1dcf8" />
  
图15-12. EUR/USD 和基于分类的交易策略（两个二元滞后）随时间的表现  
  
### 五个二元特征  
  
为了试图提高策略的表现，以下代码使用五个二元滞后值代替两个。特别是，基于 SVM 的策略的表现得到了显著提升（见图15-13）。另一方面，基于 LR 和 GNB 的策略的表现变得更差：  
  
```python  
data = pd.DataFrame(raw[symbol])  
  
data['returns'] = np.log(data / data.shift(1))  
  
data['direction'] = np.sign(data['returns'])  
  
lags = 5 # ❶ 现在使用对数收益率序列的五个滞后值。  
create_lags(data)  
data.dropna(inplace=True)  
  
create_bins(data) # ❷ 实值特征数据被转换为二元数据。  
cols_bin  
```  
  
```txt  
['lag_1_bin', 'lag_2_bin', 'lag_3_bin', 'lag_4_bin', 'lag_5_bin']  
```  
  
```python  
data[cols_bin].head()  
```  
  
```txt  
            lag_1_bin  lag_2_bin  lag_3_bin  lag_4_bin  lag_5_bin  
Date  
2010-01-12          1          1          0          1          0  
2010-01-13          0          1          1          0          1  
2010-01-14          1          0          1          1          0  
2010-01-15          0          1          0          1          1  
2010-01-19          0          0          1          0          1  
```  
  
```python  
data.dropna(inplace=True)  
  
fit_models(data)  
  
derive_positions(data)  
  
evaluate_strats(data)  
  
data[sel].sum().apply(np.exp)  
```  
  
```txt  
returns           0.805002  
strat_log_reg     0.971623  
strat_gauss_nb    0.986420  
strat_svm         1.452406  
dtype: float64  
```  
  
```python  
data[sel].cumsum().apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="442" alt="python_for_finance 15 13" src="https://github.com/user-attachments/assets/14ef3fc4-d328-415e-bb6d-4d62f3f53a87" />
  
图15-13. EUR/USD 和基于分类的交易策略（五个二元滞后）随时间的表现  
  
### 五个数字化特征  
  
最后，以下代码使用历史对数收益率的第一和第二矩来数字化特征数据，从而允许更多的特征值组合。这改善了所使用的所有分类算法的性能，但对于 SVM 的改善再次最为明显（见图15-14）：  
  
```python  
mu = data['returns'].mean() # ❶ 使用平均对数收益率和...  
v = data['returns'].std() # ❷ ...标准差...  
  
bins = [mu - v, mu, mu + v] # ❸ ...来对特征数据进行数字化。  
bins # ❸   
```  
  
```txt  
[-0.006033537040418665, -0.00010174015279231306, 0.005830056734834039]  
```  
  
```python  
create_bins(data, bins)  
  
data[cols_bin].head()  
```  
  
```txt  
            lag_1_bin  lag_2_bin  lag_3_bin  lag_4_bin  lag_5_bin  
Date  
2010-01-12          3          3          0          2          1  
2010-01-13          1          3          3          0          2  
2010-01-14          2          1          3          3          0  
2010-01-15          1          2          1          3          3  
2010-01-19          0          1          2          1          3  
```  
  
```python  
fit_models(data)  
  
derive_positions(data)  
  
evaluate_strats(data)  
  
data[sel].sum().apply(np.exp)  
```  
  
```txt  
returns           0.805002  
strat_log_reg     1.431120  
strat_gauss_nb    1.815304  
strat_svm         5.653433  
dtype: float64  
```  
  
```python  
data[sel].cumsum().apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="666" height="447" alt="python_for_finance 15 14" src="https://github.com/user-attachments/assets/72af2d2e-9e44-40f7-9edc-40c2feacb62d" />
  
图15-14. EUR/USD 和基于分类的交易策略（五个数字化滞后）随时间的表现  
  
**特征的类型**  
  
本章完全使用滞后收益数据作为特征数据，且主要以二值化或数字化形式呈现。这样做主要是为了方便，因为此类特征数据可以从金融时间序列本身中推导出来。然而，在实际应用中，特征数据可以从丰富多样的不同数据源中获取，并且可能包括其他金融时间序列及其衍生的统计数据、宏观经济数据、公司财务指标或新闻文章。有关此主题的深入讨论，请参阅 López de Prado (2018)。也有可用的自动化时间序列特征提取的 Python 包，例如 tsfresh。  
  
### 顺序训练-测试拆分  
  
为了更好地评估分类算法的性能，下面的代码实现了一个顺序训练-测试拆分。这里的思路是模拟这样一种情况：只有截至某个时间点的数据可用于训练机器学习算法。在实盘交易中，该算法将面临它以前从未见过的数据。这正是算法必须证明其价值的地方。在这个特定案例中，所有分类算法都跑赢了（在之前的简化假设下）被动基准投资，但只有 GNB 和 LR 算法实现了正的绝对收益（图15-15）：  
  
```python  
split = int(len(data) * 0.5)  
  
train = data.iloc[:split].copy() # ❶ 在训练数据上训练所有分类算法。  
  
fit_models(train) # ❶   
  
test = data.iloc[split:].copy() # ❷ 在测试数据上测试所有分类算法。  
  
derive_positions(test) # ❷   
  
evaluate_strats(test) # ❷   
  
test[sel].sum().apply(np.exp)  
```  
  
```txt  
returns           0.850291  
strat_log_reg     0.962989  
strat_gauss_nb    0.941172  
strat_svm         1.048966  
dtype: float64  
```  
  
```python  
test[sel].cumsum().apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="664" height="447" alt="python_for_finance 15 15" src="https://github.com/user-attachments/assets/ff1af7bc-962f-40d0-870e-0f59cbb9523c" />
  
图15-15. EUR/USD 和基于分类的交易策略表现（顺序训练-测试拆分）  
  
### 随机训练-测试拆分  
  
分类算法在二元或数字化特征数据上进行训练和测试。其理念是特征值模式使得预测未来市场变动的命中率超过 50%。它隐含地假设这些模式的预测能力随时间持续存在。从这个意义上说，算法在数据的哪一部分进行训练以及在哪一部分进行测试应该不会产生（太大的）差异——这意味着为了进行训练和测试，我们可以打破数据的时间序列顺序。  
  
执行此操作的一种典型方法是随机训练-测试拆分以测试分类算法在样本外的性能——再次试图模拟现实，在现实中交易期间算法持续面临新数据。所使用的方法与第459页“训练-测试拆分：支持向量机”中应用于样本数据的方法相同。基于这种方法，SVM 算法在样本外再次表现出最佳性能（见图15-16）：  
  
```python  
from sklearn.model_selection import train_test_split  
  
train, test = train_test_split(data, test_size=0.5,  
                               shuffle=True, random_state=100)  
  
train = train.copy().sort_index() # ❶ 复制训练和测试数据集，并按时间顺序排回。  
  
train[cols_bin].head()  
```  
  
```txt  
            lag_1_bin  lag_2_bin  lag_3_bin  lag_4_bin  lag_5_bin  
Date  
2010-01-12          3          3          0          2          1  
2010-01-13          1          3          3          0          2  
2010-01-14          2          1          3          3          0  
2010-01-15          1          2          1          3          3  
2010-01-20          1          0          1          2          1  
```  
  
```python  
test = test.copy().sort_index() # ❶   
  
fit_models(train)  
  
derive_positions(test)  
  
evaluate_strats(test)  
  
test[sel].sum().apply(np.exp)  
```  
  
```txt  
returns           0.878078  
strat_log_reg     0.735893  
strat_gauss_nb    0.765009  
strat_svm         0.695428  
dtype: float64  
```  
  
```python  
test[sel].cumsum().apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="439" alt="python_for_finance 15 16" src="https://github.com/user-attachments/assets/904e97f6-a39c-4f84-ab7b-9a624c7da32f" />
  
图15-16. EUR/USD 和基于分类的交易策略表现（随机训练-测试拆分）  
  
## 深度神经网络  
  
深度神经网络 (DNN) 试图模拟人脑的运作方式。它们通常由输入层（特征）、输出层（标签）以及若干隐藏层组成。隐藏层的存在使得神经网络称之为“深”。这允许它学习更复杂的关系，并在许多问题类型上表现更好。当应用 DNN 时，人们通常会提到“深度学习”而不是“机器学习”。关于这一领域的介绍，请参阅 Géron (2017) 或 Gibson 和 Patterson (2017)。  
  
### 使用 scikit-learn 构建 DNN  
  
本节应用 scikit-learn 中的 MLPClassifier 算法，如第454页“深度神经网络”中所介绍的。首先，使用数字化特征在整个数据集上对其进行训练和测试。该算法在样本内实现了异常出色的性能（见图15-17），这说明了 DNN 在此类问题上的威力。它也暗示存在严重的过拟合现象，因为这种性能表现确实好得不切实际：  
  
```python  
from sklearn.neural_network import MLPClassifier  
  
model = MLPClassifier(solver='lbfgs', alpha=1e-5,  
                      hidden_layer_sizes=2 * [250],  
                      random_state=1)  
  
%time model.fit(data[cols_bin], data['direction'])  
```  
  
```txt  
CPU times: user 16.1 s, sys: 156 ms, total: 16.2 s  
Wall time: 9.85 s  
```  
  
```txt  
MLPClassifier(activation='relu', alpha=1e-05, batch_size='auto',  
              beta_1=0.9,  
              beta_2=0.999, early_stopping=False, epsilon=1e-08,  
              hidden_layer_sizes=[250, 250], learning_rate='constant',  
              learning_rate_init=0.001, max_iter=200, momentum=0.9,  
              n_iter_no_change=10, nesterovs_momentum=True, power_t=0.5,  
              random_state=1, shuffle=True, solver='lbfgs', tol=0.0001,  
              validation_fraction=0.1, verbose=False, warm_start=False)  
```  
  
```python  
data['pos_dnn_sk'] = model.predict(data[cols_bin])  
  
data['strat_dnn_sk'] = data['pos_dnn_sk'] * data['returns']  
  
data[['returns', 'strat_dnn_sk']].sum().apply(np.exp)  
```  
  
```txt  
returns          0.805002  
strat_dnn_sk    35.156677  
dtype: float64  
```  
  
```python  
data[['returns', 'strat_dnn_sk']].cumsum().apply(  
    np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="445" alt="python_for_finance 15 17" src="https://github.com/user-attachments/assets/dc855d53-94c5-4796-8302-b7daf8a724dd" />
  
图15-17. EUR/USD 和基于 DNN 的交易策略表现（scikit-learn，样本内）  
  
为了避免 DNN 模型的过拟合，接下来应用随机训练-测试拆分。该算法再次跑赢了被动基准投资，并获得了正的绝对表现（图15-18）。不过，现在的看起来要更现实一些：  
  
```python  
train, test = train_test_split(data, test_size=0.5,  
                               random_state=100)  
  
train = train.copy().sort_index()  
  
test = test.copy().sort_index()  
  
model = MLPClassifier(solver='lbfgs', alpha=1e-5, max_iter=500,  
                      hidden_layer_sizes=3 * [500], random_state=1) # ❶ 增加隐藏层和隐藏单元的数量。  
  
%time model.fit(train[cols_bin], train['direction'])  
```  
  
```txt  
CPU times: user 2min 26s, sys: 1.02 s, total: 2min 27s  
Wall time: 1min 31s  
```  
  
```txt  
MLPClassifier(activation='relu', alpha=1e-05, batch_size='auto',  
              beta_1=0.9,  
              beta_2=0.999, early_stopping=False, epsilon=1e-08,  
              hidden_layer_sizes=[500, 500, 500], learning_rate='constant',  
              learning_rate_init=0.001, max_iter=500, momentum=0.9,  
              n_iter_no_change=10, nesterovs_momentum=True, power_t=0.5,  
              random_state=1, shuffle=True, solver='lbfgs', tol=0.0001,  
              validation_fraction=0.1, verbose=False, warm_start=False)  
```  
  
```python  
test['pos_dnn_sk'] = model.predict(test[cols_bin])  
  
test['strat_dnn_sk'] = test['pos_dnn_sk'] * test['returns']  
  
test[['returns', 'strat_dnn_sk']].sum().apply(np.exp)  
```  
  
```txt  
returns         0.878078  
strat_dnn_sk    1.242042  
dtype: float64  
```  
  
```python  
test[['returns', 'strat_dnn_sk']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="443" alt="python_for_finance 15 18" src="https://github.com/user-attachments/assets/5c554f2f-8284-49bd-a167-69b85e05a0a3" />
  
图15-18. EUR/USD 和基于 DNN 的交易策略表现（scikit-learn，随机训练-测试拆分）  
  
### 使用 TensorFlow 构建 DNN  
  
TensorFlow 已成为一个深受欢迎的深度学习包。它由 Google Inc. 开发和支持，并在那里被应用于各种各样的机器学习问题。Zedah 和 Ramsundar (2018) 深入探讨了用于深度学习的 TensorFlow。  
  
正如在 scikit-learn 中那样，有了第454页“深度神经网络”的背景知识，应用 TensorFlow 中的 DNNClassifier 算法来推导算法交易策略是直截了当的。训练和测试数据与之前相同。首先是模型的训练。在样本内，该算法跑赢被动基准投资并显示出可观的绝对收益（见图15-19），同样这也暗示了过拟合的可能：  
  
```python  
import tensorflow as tf  
tf.logging.set_verbosity(tf.logging.ERROR)  
  
fc = [tf.contrib.layers.real_valued_column('lags', dimension=lags)]  
  
model = tf.contrib.learn.DNNClassifier(hidden_units=3 * [500],  
                                       n_classes=len(bins) + 1,  
                                       feature_columns=fc)  
  
def input_fn():  
    fc = {'lags': tf.constant(data[cols_bin].values)}  
    la = tf.constant(data['direction'].apply(  
        lambda x: 0 if x < 0 else 1).values,  
                     shape=[data['direction'].size, 1])  
    return fc, la  
  
%time model.fit(input_fn=input_fn, steps=250) # ❶ 训练所需的时间可能相当可观。  
```  
  
```txt  
CPU times: user 2min 7s, sys: 8.85 s, total: 2min 16s  
Wall time: 49 s  
```  
  
```txt  
DNNClassifier(params={'head':  
<tensorflow.contrib.learn.python.learn.estimators.head._MultiClassHead  
object at 0x1a19acf898>, 'hidden_units': [500, 500, 500],  
'feature_columns': (_RealValuedColumn(column_name='lags', dimension=5,  
default_value=None, dtype=tf.float32, normalizer=None),), 'optimizer':  
None, 'activation_fn': <function relu at 0x1161441e0>, 'dropout':  
None, 'gradient_clip_norm': None, 'embedding_lr_multipliers': None,  
'input_layer_min_slice_size': None})  
```  
  
```python  
model.evaluate(input_fn=input_fn, steps=1)  
```  
  
```txt  
{'loss': 0.6879357, 'accuracy': 0.5379925, 'global_step': 250}  
```  
  
```python  
pred = np.array(list(model.predict(input_fn=input_fn)))  
pred[:10] # ❷ 二进制预测 (0, 1)...  
```  
  
```txt  
array([0, 0, 0, 0, 0, 1, 0, 1, 1, 0])  
```  
  
```python  
data['pos_dnn_tf'] = np.where(pred > 0, 1, -1) # ❸ ...需要转换为市场头寸 (-1, +1)。  
  
data['strat_dnn_tf'] = data['pos_dnn_tf'] * data['returns']  
  
data[['returns', 'strat_dnn_tf']].sum().apply(np.exp)  
```  
  
```txt  
returns         0.805002  
strat_dnn_tf    2.437222  
dtype: float64  
```  
  
```python  
data[['returns', 'strat_dnn_tf']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="666" height="440" alt="python_for_finance 15 19" src="https://github.com/user-attachments/assets/45a02f49-1bae-412b-b055-407a0407038d" />
  
图15-19. EUR/USD 和基于 DNN 的交易策略表现（TensorFlow，样本内）  
  
以下代码再次实现随机训练-测试拆分，以获得对基于 DNN 的算法交易策略性能的更真实的视图。正如预期，样本外的表现变差了（见图15-20）。此外，在特定的参数化设置下，TensorFlow DNNClassifier 算法的表现比 scikit-learn MLPClassifier 算法差了几个百分点：  
  
```python  
model = tf.contrib.learn.DNNClassifier(hidden_units=3 * [500],  
                                       n_classes=len(bins) + 1,  
                                       feature_columns=fc)  
  
data = train  
  
%time model.fit(input_fn=input_fn, steps=2500)  
```  
  
```txt  
CPU times: user 11min 7s, sys: 1min 7s, total: 12min 15s  
Wall time: 4min 27s  
```  
  
```txt  
DNNClassifier(params={'head':  
<tensorflow.contrib.learn.python.learn.estimators.head._MultiClassHead  
object at 0x116828cc0>, 'hidden_units': [500, 500, 500],  
'feature_columns': (_RealValuedColumn(column_name='lags', dimension=5,  
default_value=None, dtype=tf.float32, normalizer=None),), 'optimizer':  
None, 'activation_fn': <function relu at 0x1161441e0>, 'dropout':  
None, 'gradient_clip_norm': None, 'embedding_lr_multipliers': None,  
'input_layer_min_slice_size': None})  
```  
  
```python  
data = test  
  
model.evaluate(input_fn=input_fn, steps=1)  
```  
  
```txt  
{'loss': 0.82882184, 'accuracy': 0.48968107, 'global_step': 2500}  
```  
  
```python  
pred = np.array(list(model.predict(input_fn=input_fn)))  
  
test['pos_dnn_tf'] = np.where(pred > 0, 1, -1)  
  
test['strat_dnn_tf'] = test['pos_dnn_tf'] * test['returns']  
  
test[['returns', 'strat_dnn_sk', 'strat_dnn_tf']].sum().apply(np.exp)  
```  
  
```txt  
returns         0.878078  
strat_dnn_sk    1.242042  
strat_dnn_tf    1.063968  
dtype: float64  
```  
  
```python  
test[['returns', 'strat_dnn_sk', 'strat_dnn_tf']].cumsum(  
    ).apply(np.exp).plot(figsize=(10, 6));  
```  
  
<img width="665" height="440" alt="python_for_finance 15 20" src="https://github.com/user-attachments/assets/1a4a2ef4-b3c5-40f9-bdf2-8cf25d0a6325" />
  
图15-20. EUR/USD 和基于 DNN 的交易策略表现（TensorFlow，随机训练-测试拆分）  
  
**表现结果**  
  
迄今为止展示的不同算法交易策略从向量化回测中得出的所有表现结果都仅供说明参考。除了零交易成本这一简化假设之外，结果还取决于许多其他（主要也是任意选择的）参数。它们还取决于全篇所使用的相对较小的 EUR/USD 汇率日终价格数据集。本章的重点在于说明将不同方法和机器学习算法应用于金融数据，而不是在于推导出能在实践中部署的稳健的算法交易策略。下一章将解决其中一些问题。  
  
## 结论  
  
本章讨论的是算法交易策略以及如何基于向量化回测来评估其表现。它从基于双简单移动平均线的相当简单的算法交易策略开始，这是几十年来在实践中广为人知并被使用的一种策略类型。利用该策略说明了向量化回测的方法，其在数据分析上大量利用了 NumPy 和 pandas 的向量化能力。  
  
本章还使用 OLS 回归在真实金融时间序列的基础上说明了随机游走假说。这是任何算法交易策略都必须证明其价值的基准。  
  
本章的核心是应用“机器学习”（第444页）中介绍的机器学习算法。我们使用和应用了多种主要属于分类类型的算法，基本遵循了相同的“节奏”。在特征选择上，虽然没必要自我设限，但本章在多种变体中一直使用滞后的对数收益率数据。这主要是为了方便和简单。此外，分析基于一些简化的假设，因为我们的重点主要在于将机器学习算法应用于金融时间序列数据以预测金融市场方向变动的物理技术层面。  
  
## 进一步的资源  
  
本章提及的论文如下：  
  
- Brock, William, Josef Lakonishok, and Blake LeBaron (1992). “Simple Technical Trading Rules and the Stochastic Properties of Stock Returns.” Journal of Finance, Vol. 47, No. 5, pp. 1731–1764.  
- Fama, Eugene (1965). “Random Walks in Stock Market Prices.” Selected Papers, No. 16, Graduate School of Business, University of Chicago.  
- Jensen, Michael (1978). “Some Anomalous Evidence Regarding Market Efficiency.” Journal of Financial Economics, Vol. 6, No. 2/3, pp. 95–101.  
  
涉及本章相关主题的金融书籍包括：  
  
- Baxter, Martin, and Andrew Rennie (1996). Financial Calculus. Cambridge, England: Cambridge University Press.  
- Chan, Ernest (2009). Quantitative Trading. Hoboken, NJ: John Wiley & Sons.  
- Chan, Ernest (2013). Algorithmic Trading. Hoboken, NJ: John Wiley & Sons.  
- Chan, Ernest (2017). Machine Trading. Hoboken, NJ: John Wiley & Sons.  
- López de Prado, Marcos (2018). Advances in Financial Machine Learning. Hoboken, NJ: John Wiley & Sons.  
  
涉及本章相关主题的技术书籍包括：  
  
- Albon, Chris (2018). Machine Learning with Python Cookbook. Sebastopol, CA: O’Reilly.  
- Géron, Aurélien (2017). Hands-On Machine Learning with Scikit-Learn and Tensorflow. Sebastopol, CA: O’Reilly.  
- Gibson, Adam, and Josh Patterson (2017). Deep Learning. Sebastopol, CA: O’Reilly.  
- VanderPlas, Jake (2016). Python Data Science Handbook. Sebastopol, CA: O’Reilly.  
- Zadeh, Reza Bosagh, and Bharath Ramsundar (2018). TensorFlow for Deep Learning. Sebastopol, CA: O’Reilly.  
  
有关涵盖面向算法交易 Python 的综合在线培训计划，请访问 http://certificate.tpq.io。  
