# 第14章 FXCM交易平台  
  
> 金融机构喜欢将他们从事的工作称为交易。老实说，那不是交易，那是赌博。  
> ——格雷顿·卡特 (Graydon Carter)  
  
本章介绍了FXCM Group, LLC（以下简称“FXCM”）的交易平台，以及其RESTful和流式应用程序编程接口（API），还有Python封装包 `fxcmpy`。FXCM为散户和机构交易者提供许多既可以通过传统交易应用程序交易，也可以通过API以程序化方式进行交易的金融产品。这些产品的重点在于货币对，以及主要股票指数、大宗商品等资产的差价合约（CFDs）。  
  
**风险免责声明**  
  
保证金外汇/差价合约交易具有极高的风险，可能不适合所有投资者，因为您可能会承受超过存款金额的损失。杠杆可能会对您不利。这些产品面向散户和专业客户。由于当地法律法规的某些限制，德国居民散户客户可能会承受存款资金的全部损失，但不承担超出存款资金的后续支付义务。请了解并充分理解与市场和交易相关的所有风险。在交易任何产品之前，请仔细考虑您的财务状况和经验水平。所提供的任何意见、新闻、研究、分析、价格或其他信息均作为一般市场评论，并不构成投资建议。市场评论并未根据旨在促进投资研究独立性的法律要求进行编写，因此不受发布前禁止交易的限制。FXCM及作者对直接或间接使用或依赖此类信息而可能产生的任何损失或损害（包括但不限于任何利润损失）概不负责。  
  
FXCM的交易平台允许即便是资本头寸较小的个人交易者也能实施和部署算法交易策略。  
  
本章涵盖了通过程序化方式实现自动化算法交易策略所需的FXCM交易API和 `fxcmpy` Python包的基本功能。本章的结构如下：  
  
“入门指南”（第469页）  
本节将展示如何设置好一切，以便使用FXCM的REST API进行算法交易。  
  
“获取数据”（第469页）  
本节将展示如何检索和处理金融数据（精确到Tick级别）。  
  
“使用API”（第474页）  
本节阐述了使用REST API实现的典型任务，例如检索历史数据和流数据、下达订单以及查询账户信息。  
  
## 入门指南  
  
关于FXCM API的详细文档可以在 https://fxcm.github.io/rest-api-docs 找到。要安装Python封装包 `fxcmpy`，请在shell中执行以下命令：  
  
```python  
pip install fxcmpy  
```  
  
`fxcmpy` 包的文档可以在 http://fxcmpy.tpq.io 找到。  
  
为了开始使用FXCM交易API和 `fxcmpy` 包，注册一个免费的FXCM模拟账户就足够了。[^1] 下一步是在模拟账户内生成一个独特的API令牌（Token）——例如，`YOUR_FXCM_API_TOKEN`。随后可以通过以下方式打开与API的连接：  
  
```python  
import fxcmpy  
api = fxcmpy.fxcmpy(access_token=YOUR_FXCM_API_TOKEN, log_level='error')  
```  
  
或者，也可以使用配置文件（例如 `fxcm.cfg`）连接到API。该文件的内容应如下所示：  
  
```python  
[FXCM]  
log_level = error  
log_file = PATH_TO_AND_NAME_OF_LOG_FILE  
access_token = YOUR_FXCM_API_TOKEN  
```  
  
然后即可通过以下方式连接到API：  
  
```python  
import fxcmpy  
api = fxcmpy.fxcmpy(config_file='fxcm.cfg')  
```  
  
默认情况下，`fxcmpy` 类会连接到模拟服务器。然而，通过使用 `server` 参数，也可以建立与实盘交易服务器的连接（如果存在这样的实盘账户）：  
  
```python  
api = fxcmpy.fxcmpy(config_file='fxcm.cfg', server='demo') # 连接到模拟服务器。  
api = fxcmpy.fxcmpy(config_file='fxcm.cfg', server='real') # 连接到实盘交易服务器。  
```  
  
## 获取数据  
  
FXCM提供了对预打包的历史市场价格数据集（如Tick数据）的访问。这意味着人们可以从FXCM服务器检索压缩文件，例如，其中包含2018年第26周EUR/USD汇率的Tick数据，如以下小节所述。从API检索历史K线数据的操作将在随后的部分中解释。  
  
### 获取Tick数据  
  
对于许多货币对，FXCM提供了历史Tick数据。`fxcmpy` 封装包使得获取这些Tick数据并对其进行处理变得非常方便。首先，进行一些导入操作：  
  
```python  
In [1]: import time  
 import numpy as np  
 import pandas as pd  
 import datetime as dt  
 from pylab import mpl, plt  
  
In [2]: plt.style.use('seaborn')  
 mpl.rcParams['font.family'] = 'serif'  
 %matplotlib inline  
```  
  
其次，查看一下可获得Tick数据的可用代码（货币对）：  
  
```python  
In [3]: from fxcmpy import fxcmpy_tick_data_reader as tdr  
  
In [4]: print(tdr.get_available_symbols())  
```  
  
```txt  
('AUDCAD', 'AUDCHF', 'AUDJPY', 'AUDNZD', 'CADCHF', 'EURAUD', 'EURCHF',  
 'EURGBP', 'EURJPY', 'EURUSD', 'GBPCHF', 'GBPJPY', 'GBPNZD', 'GBPUSD',  
 'GBPCHF', 'GBPJPY', 'GBPNZD', 'NZDCAD', 'NZDCHF', 'NZDJPY', 'NZDUSD',  
 'USDCAD', 'USDCHF', 'USDJPY')  
```  
  
以下代码检索了单一代码的一周Tick数据。生成的 `pandas DataFrame` 对象具有超过150万行数据：  
  
```python  
In [5]: start = dt.datetime(2018, 6, 25)   
 stop = dt.datetime(2018, 6, 30)   
  
In [6]: td = tdr('EURUSD', start, stop) # 这将检索数据文件，将其解压，并将原始数据存储在DataFrame对象中（作为结果对象的属性）。  
  
In [7]: td.get_raw_data().info() # td.get_raw_data()方法返回包含原始数据的DataFrame对象；即索引值仍为str对象。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
Index: 1963779 entries, 06/24/2018 21:00:12.290 to 06/29/2018  
20:59:00.607  
Data columns (total 2 columns):  
Bid float64  
Ask float64  
dtypes: float64(2)  
memory usage: 44.9+ MB  
```  
  
```python  
In [8]: td.get_data().info() # td.get_data()方法返回一个DataFrame对象，其索引已转换为DatetimeIndex。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 1963779 entries, 2018-06-24 21:00:12.290000 to 2018-06-29  
20:59:00.607000  
Data columns (total 2 columns):  
Bid float64  
Ask float64  
dtypes: float64(2)  
memory usage: 44.9 MB  
```  
  
```python  
In [9]: td.get_data().head()  
```  
  
```txt  
Out[9]: Bid Ask  
 2018-06-24 21:00:12.290 1.1662 1.16660  
 2018-06-24 21:00:16.046 1.1662 1.16650  
 2018-06-24 21:00:22.846 1.1662 1.16658  
 2018-06-24 21:00:22.907 1.1662 1.16660  
 2018-06-24 21:00:23.441 1.1662 1.16663  
```  
  
既然Tick数据存储在 `DataFrame` 对象中，那么从中选取一个数据子集并在其上实现典型的金融分析任务就非常直接了。图14-1展示了为该子集推导出的中间价以及一条简单移动平均线（SMA）的图表：  
  
```python  
In [10]: sub = td.get_data(start='2018-06-29 12:00:00',  
 end='2018-06-29 12:15:00') # 选取完整数据集的一个子集。  
  
In [11]: sub.head()  
```  
  
```txt  
Out[11]: Bid Ask  
 2018-06-29 12:00:00.011 1.16497 1.16498  
 2018-06-29 12:00:00.071 1.16497 1.16497  
 2018-06-29 12:00:00.079 1.16497 1.16498  
 2018-06-29 12:00:00.091 1.16495 1.16498  
 2018-06-29 12:00:00.205 1.16496 1.16498  
```  
  
```python  
In [12]: sub['Mid'] = sub.mean(axis=1) # 通过买入和卖出价计算中间价。  
  
In [13]: sub['SMA'] = sub['Mid'].rolling(1000).mean() # 在1000个tick的间隔上计算SMA值。  
  
In [14]: sub[['Mid', 'SMA']].plot(figsize=(10, 6), lw=0.75);  
```  
  
<img width="667" height="413" alt="python_for_finance 14 1" src="https://github.com/user-attachments/assets/9a8cbeb7-97b5-4bbf-a8b9-4f6a5c3dcce7" />
  
图14-1. EUR/USD的历史Tick中间价与SMA  
  
### 获取K线数据  
  
FXCM还提供访问历史K线数据（API之外）的功能——即获取特定时间间隔（“柱”）的数据，包含买入价和卖出价的开盘、最高、最低以及收盘值。  
  
首先，查看提供了K线数据的可用代码：  
  
```python  
In [15]: from fxcmpy import fxcmpy_candles_data_reader as cdr  
  
In [16]: print(cdr.get_available_symbols())  
```  
  
```txt  
('AUDCAD', 'AUDCHF', 'AUDJPY', 'AUDNZD', 'CADCHF', 'EURAUD', 'EURCHF',  
 'EURGBP', 'EURJPY', 'EURUSD', 'GBPCHF', 'GBPJPY', 'GBPNZD', 'GBPUSD',  
 'GBPCHF', 'GBPJPY', 'GBPNZD', 'NZDCAD', 'NZDCHF', 'NZDJPY', 'NZDUSD',  
 'USDCAD', 'USDCHF', 'USDJPY')  
```  
  
其次是数据检索操作本身。这与Tick数据的检索类似。唯一的区别在于需要指定一个 `period` 值——即K线的长度（例如，`m1` 表示一分钟，`H1` 表示一小时，或 `D1` 表示一天）：  
  
```python  
In [17]: start = dt.datetime(2018, 5, 1)  
 stop = dt.datetime(2018, 6, 30)  
  
In [18]: period = 'H1' # 指定周期（period）值。  
  
In [19]: candles = cdr('EURUSD', start, stop, period)  
```  
  
```python  
In [20]: data = candles.get_data()  
  
In [21]: data.info()  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 1080 entries, 2018-04-29 21:00:00 to 2018-06-29 20:00:00  
Data columns (total 8 columns):  
BidOpen 1080 non-null float64  
BidHigh 1080 non-null float64  
BidLow 1080 non-null float64  
BidClose 1080 non-null float64  
AskOpen 1080 non-null float64  
AskHigh 1080 non-null float64  
AskLow 1080 non-null float64  
AskClose 1080 non-null float64  
dtypes: float64(8)  
memory usage: 75.9 KB  
```  
  
```python  
In [22]: data[data.columns[:4]].tail() # 买入价的开盘、最高、最低、收盘值。  
```  
  
```txt  
Out[22]: BidOpen BidHigh BidLow BidClose  
 2018-06-29 16:00:00 1.16768 1.16820 1.16731 1.16769  
 2018-06-29 17:00:00 1.16769 1.16826 1.16709 1.16781  
 2018-06-29 18:00:00 1.16781 1.16816 1.16668 1.16684  
 2018-06-29 19:00:00 1.16684 1.16792 1.16638 1.16774  
 2018-06-29 20:00:00 1.16774 1.16904 1.16758 1.16816  
```  
  
```python  
In [23]: data[data.columns[4:]].tail() # 卖出价的开盘、最高、最低、收盘值。  
```  
  
```txt  
Out[23]: AskOpen AskHigh AskLow AskClose  
 2018-06-29 16:00:00 1.16769 1.16820 1.16732 1.16771  
 2018-06-29 17:00:00 1.16771 1.16827 1.16711 1.16782  
 2018-06-29 18:00:00 1.16782 1.16817 1.16669 1.16686  
 2018-06-29 19:00:00 1.16686 1.16794 1.16640 1.16775  
 2018-06-29 20:00:00 1.16775 1.16907 1.16760 1.16861  
```  
  
作为本节的结尾，以下代码计算了中间收盘价以及两条SMA线，并绘制了结果（见图14-2）：  
  
```python  
In [24]: data['MidClose'] = data[['BidClose', 'AskClose']].mean(axis=1) # 通过买卖收盘价计算中间收盘价。  
  
In [25]: data['SMA1'] = data['MidClose'].rolling(30).mean() # 计算两条SMA线，一条用于较短的时间区间，另一条用于较长的时间区间。  
 data['SMA2'] = data['MidClose'].rolling(100).mean() # 计算两条SMA线，一条用于较短的时间区间，另一条用于较长的时间区间。  
  
In [26]: data[['MidClose', 'SMA1', 'SMA2']].plot(figsize=(10, 6));  
```  
  
<img width="666" height="413" alt="python_for_finance 14 2" src="https://github.com/user-attachments/assets/e6b0f0e7-930f-418d-87ca-22028cf10007" />
  
图14-2. EUR/USD的历史小时中间收盘价与两条SMA  
  
## 使用API  
  
前面的小节演示了从FXCM服务器获取预打包的历史Tick数据和K线数据，而本节将展示如何通过API获取历史数据。为此，需要一个连接到FXCM API的连接对象。因此，首先导入 `fxcmpy` 封装包，建立API连接（基于唯一的API令牌），并查看可用的交易工具：  
  
```python  
In [27]: import fxcmpy  
  
In [28]: fxcmpy.__version__  
```  
  
```txt  
Out[28]: '1.1.33'  
```  
  
```python  
In [29]: api = fxcmpy.fxcmpy(config_file='../fxcm.cfg') # 此处连接到API；请根据情况调整路径/文件名。  
  
In [30]: instruments = api.get_instruments()  
  
In [31]: print(instruments)  
```  
  
```txt  
['EUR/USD', 'XAU/USD', 'GBP/USD', 'UK100', 'USDOLLAR', 'XAG/USD', 'GER30',  
 'FRA40', 'USD/CNH', 'EUR/JPY', 'USD/JPY', 'CHN50', 'GBP/JPY', 'AUD/JPY',  
 'CHF/JPY', 'USD/CHF', 'GBP/CHF', 'AUD/USD', 'EUR/AUD', 'EUR/CHF',  
 'EUR/CAD', 'EUR/GBP', 'AUD/CAD', 'NZD/USD', 'USD/CAD', 'CAD/JPY',  
 'GBP/AUD', 'NZD/JPY', 'US30', 'GBP/CAD', 'SOYF', 'GBP/NZD', 'AUD/NZD',  
 'USD/SEK', 'EUR/SEK', 'EUR/NOK', 'USD/NOK', 'USD/MXN', 'AUD/CHF',  
 'EUR/NZD', 'USD/ZAR', 'USD/HKD', 'ZAR/JPY', 'BTC/USD', 'USD/TRY',  
 'EUR/TRY', 'NZD/CHF', 'CAD/CHF', 'NZD/CAD', 'TRY/JPY', 'AUS200',  
 'ESP35', 'HKG33', 'JPN225', 'NAS100', 'SPX500', 'Copper', 'EUSTX50',  
 'USOil', 'UKOil', 'NGAS', 'Bund']  
```  
  
### 获取历史数据  
  
建立连接后，获取特定时间区间的数据即可通过单次方法调用来完成。当使用 `get_candles()` 方法时，参数 `period` 可以是 `m1`、`m5`、`m15`、`m30`、`H1`、`H2`、`H3`、`H4`、`H6`、`H8`、`D1`、`W1` 或 `M1`。以下代码提供了一些示例。图14-3展示了EUR/USD交易工具（货币对）的1分钟柱卖出收盘价：  
  
```python  
In [32]: candles = api.get_candles('USD/JPY', period='D1', number=10) # 检索最近10天的日终价格。  
  
In [33]: candles[candles.columns[:4]]   
```  
  
```txt  
Out[33]: bidopen bidclose bidhigh bidlow  
 date  
 2018-10-08 21:00:00 113.760 113.219 113.937 112.816  
 2018-10-09 21:00:00 113.219 112.946 113.386 112.863  
 2018-10-10 21:00:00 112.946 112.267 113.281 112.239  
 2018-10-11 21:00:00 112.267 112.155 112.528 111.825  
 2018-10-12 21:00:00 112.155 112.200 112.491 111.873  
 2018-10-14 21:00:00 112.163 112.130 112.270 112.109  
 2018-10-15 21:00:00 112.130 111.758 112.230 111.619  
 2018-10-16 21:00:00 112.151 112.238 112.333 111.727  
 2018-10-17 21:00:00 112.238 112.636 112.670 112.009  
 2018-10-18 21:00:00 112.636 112.168 112.725 111.942  
```  
  
```python  
In [34]: candles[candles.columns[4:]]   
```  
  
```txt  
Out[34]: askopen askclose askhigh asklow tickqty  
 date  
 2018-10-08 21:00:00 113.840 113.244 113.950 112.827 184835  
 2018-10-09 21:00:00 113.244 112.970 113.399 112.875 321755  
 2018-10-10 21:00:00 112.970 112.287 113.294 112.265 329174  
 2018-10-11 21:00:00 112.287 112.175 112.541 111.835 568231  
 2018-10-12 21:00:00 112.175 112.243 112.504 111.885 363233  
 2018-10-14 21:00:00 112.219 112.181 112.294 112.145 581  
 2018-10-15 21:00:00 112.181 111.781 112.243 111.631 322304  
 2018-10-16 21:00:00 112.163 112.271 112.345 111.740 253420  
 2018-10-17 21:00:00 112.271 112.664 112.682 112.022 542166  
 2018-10-18 21:00:00 112.664 112.237 112.738 111.955 369012  
```  
  
```python  
In [35]: start = dt.datetime(2017, 1, 1)   
 end = dt.datetime(2018, 1, 1)   
  
In [36]: candles = api.get_candles('EUR/GBP', period='D1',  
 start=start, stop=end) # 检索一整年的日终价格。  
  
In [37]: candles.info()   
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 309 entries, 2017-01-03 22:00:00 to 2018-01-01 22:00:00  
Data columns (total 9 columns):  
bidopen 309 non-null float64  
bidclose 309 non-null float64  
bidhigh 309 non-null float64  
bidlow 309 non-null float64  
askopen 309 non-null float64  
askclose 309 non-null float64  
askhigh 309 non-null float64  
asklow 309 non-null float64  
tickqty 309 non-null int64  
dtypes: float64(8), int64(1)  
memory usage: 24.1 KB  
```  
  
```python  
In [38]: candles = api.get_candles('EUR/USD', period='m1', number=250) # 检索最新可用的1分钟柱价格。  
  
In [39]: candles['askclose'].plot(figsize=(10, 6))  
```  
  
<img width="666" height="425" alt="python_for_finance 14 3" src="https://github.com/user-attachments/assets/b2dd182d-eff2-4350-afe7-683bc2e62747" />
  
图14-3. EUR/USD的历史卖出收盘价（分钟K线图）  
  
### 获取流数据  
  
虽然历史数据很重要（例如用于回测算法交易策略），但在部署和自动化算法交易策略时，还需要持续访问实时或流数据（在交易时段内）。FXCM API允许订阅所有交易工具的实时数据流。`fxcmpy` 封装包支持这一功能，其中包括允许用户提供自定义函数（即所谓的回调函数）以处理实时数据流。  
  
以下代码展示了一个简单的回调函数——它仅仅是打印出检索到的数据集中的特定元素——并在订阅所需的交易工具（此处为 EUR/USD）之后使用它来实时处理检索到的数据：  
  
```python  
In [40]: def output(data, dataframe):  
 print('%3d | %s | %s | %6.5f, %6.5f'  
 % (len(dataframe), data['Symbol'],  
 pd.to_datetime(int(data['Updated']), unit='ms'),  
 data['Rates'][0], data['Rates'][1])) # 打印出检索数据集部分特定元素的回调函数。  
  
In [41]: api.subscribe_market_data('EUR/USD', (output,)) # 订阅特定的实时数据流；只要没有“取消订阅”事件，数据就会被异步处理。  
```  
  
```txt  
 1 | EUR/USD | 2018-10-19 11:36:39.735000 | 1.14694, 1.14705  
 2 | EUR/USD | 2018-10-19 11:36:39.776000 | 1.14694, 1.14706  
 3 | EUR/USD | 2018-10-19 11:36:40.714000 | 1.14695, 1.14707  
 4 | EUR/USD | 2018-10-19 11:36:41.646000 | 1.14696, 1.14708  
 5 | EUR/USD | 2018-10-19 11:36:41.992000 | 1.14696, 1.14709  
 6 | EUR/USD | 2018-10-19 11:36:45.131000 | 1.14696, 1.14708  
 7 | EUR/USD | 2018-10-19 11:36:45.247000 | 1.14696, 1.14709  
```  
  
```python  
In [42]: api.get_last_price('EUR/USD') # 在订阅期间，.get_last_price()方法返回最后可用的数据集。  
```  
  
```txt  
Out[42]: Bid 1.14696  
 Ask 1.14709  
 High 1.14775  
 Low 1.14323  
 Name: 2018-10-19 11:36:45.247000, dtype: float64  
```  
  
```python  
In [43]: api.unsubscribe_market_data('EUR/USD') # 此操作取消订阅实时数据流。  
```  
  
```txt  
 8 | EUR/USD | 2018-10-19 11:36:48.239000 | 1.14696, 1.14708  
```  
  
**回调函数**  
  
回调函数是一种基于Python函数处理实时流数据的灵活手段，您甚至可以使用多个回调函数。它们既可以用于执行简单的任务，比如打印传入的数据；也可以用于复杂的任务，比如根据在线交易算法生成交易信号（详见第16章）。  
  
## 下单  
  
FXCM API允许下达和管理所有类型的订单，这些订单也同样可在FXCM交易应用程序中找到（例如挂单或追踪止损订单）。[^2] 然而，以下代码仅演示了基本的市价买入和卖出订单，因为一般而言，它们对于开启算法交易之旅已经足够了。代码首先验证当前是否有未平仓的头寸，随后开启不同的头寸（通过 `create_market_buy_order()` 方法）：  
  
```python  
In [44]: api.get_open_positions() # 显示已连接（默认）账户的未平仓头寸。  
```  
  
```txt  
Out[44]: Empty DataFrame  
 Columns: []  
 Index: []  
```  
  
```python  
In [45]: order = api.create_market_buy_order('EUR/USD', 10) # 建立100,000规模的EUR/USD货币对头寸。[^3]  
  
In [46]: sel = ['tradeId', 'amountK', 'currency',  
 'grossPL', 'isBuy'] # 仅显示选定元素的未平仓头寸。  
  
In [47]: api.get_open_positions()[sel] # 仅显示选定元素的未平仓头寸。  
```  
  
```txt  
Out[47]: tradeId amountK currency grossPL isBuy  
 0 132607899 10 EUR/USD 0.17436 True  
```  
  
```python  
In [48]: order = api.create_market_buy_order('EUR/GBP', 5) # 建立另一个50,000规模的EUR/GBP货币对头寸。  
  
In [49]: api.get_open_positions()[sel]  
```  
  
```txt  
Out[49]: tradeId amountK currency grossPL isBuy  
 0 132607899 10 EUR/USD 0.17436 True  
 1 132607928 5 EUR/GBP -1.53367 True  
```  
  
如果说 `create_market_buy_order()` 函数用于开仓或增加头寸，那么 `create_market_sell_order()` 函数则允许平仓或减少头寸。同样还有更通用的方法可以用于平仓，以下代码将进行演示：  
  
```python  
In [50]: order = api.create_market_sell_order('EUR/USD', 3) # 减少EUR/USD货币对的头寸。  
  
In [51]: order = api.create_market_buy_order('EUR/GBP', 5) # 增加EUR/GBP货币对的头寸。  
  
In [52]: api.get_open_positions()[sel] # 对于EUR/GBP现在有两个未平仓的多头头寸；与EUR/USD的头寸情况不同，它们没有被轧差。  
```  
  
```txt  
Out[52]: tradeId amountK currency grossPL isBuy  
 0 132607899 10 EUR/USD 0.17436 True  
 1 132607928 5 EUR/GBP -1.53367 True  
 2 132607930 3 EUR/USD -1.33369 False  
 3 132607932 5 EUR/GBP -1.64728 True  
```  
  
```python  
In [53]: api.close_all_for_symbol('EUR/GBP') # close_all_for_symbol()方法关闭指定代码品种的所有头寸。  
  
In [54]: api.get_open_positions()[sel]  
```  
  
```txt  
Out[54]: tradeId amountK currency grossPL isBuy  
 0 132607899 10 EUR/USD 0.17436 True  
 1 132607930 3 EUR/USD -1.33369 False  
```  
  
```python  
In [55]: api.close_all() # close_all()方法关闭所有的未平仓头寸。  
  
In [56]: api.get_open_positions()  
```  
  
```txt  
Out[56]: Empty DataFrame  
 Columns: []  
 Index: []  
```  
  
## 账户信息  
  
除开仓头寸等信息外，FXCM API还允许检索更通用的账户信息。例如，您可以查询默认账户（如果存在多个账户），或者获得有关净值和保证金状况的概览：  
  
```python  
In [57]: api.get_default_account() # 显示默认的accountId值。  
```  
  
```txt  
Out[57]: 1090495  
```  
  
```python  
In [58]: api.get_accounts().T # 显示所有账户的财务状况和部分参数。  
```  
  
```txt  
Out[58]: 0  
 accountId 1090495  
 accountName 01090495  
 balance 4915.2  
 dayPL -41.97  
 equity 4915.2  
 grossPL 0  
 hedging Y  
 mc N  
 mcDate  
 ratePrecision 0  
 t 6  
 usableMargin 4915.2  
 usableMargin3 4915.2  
 usableMargin3Perc 100  
 usableMarginPerc 100  
 usdMr 0  
 usdMr3 0  
```  
  
## 结论  
  
本章讲述了用于算法交易的FXCM REST API，并涵盖了以下主题：  
  
- 设置并准备使用API  
- 获取历史Tick数据  
- 获取历史K线数据  
- 实时获取流数据  
- 下达市价买单和卖单  
- 查询账户信息  
  
FXCM API和 `fxcmpy` 封装包当然提供了更多的功能，但这些都是开展算法交易之旅所需的基本构建模块。  
  
## 进一步的资源  
  
有关FXCM交易API和Python封装包的进一步详细信息，请参阅文档：  
  
- 交易 API  
- `fxcmpy` 包  
  
欲了解有关Python在算法交易中应用的全面在线培训计划，请参阅 http://certificate.tpq.io。  
  
---  
[^1]: 请注意，FXCM的模拟账户仅向特定国家/地区提供。  
[^2]: 详情请参阅官方文档。  
[^3]: 对于货币对，该数量单位为千。另请注意，不同的账户可能具有不同的杠杆率。这意味着相同的头寸由于杠杆率不同可能需要更多或更少的净值（保证金）。如有必要，请将示例数量调整为更小的值。  
