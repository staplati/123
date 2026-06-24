# 第7章 数据可视化  
  
> 用一张图片。它胜过千言万语。  
> ——Arthur Brisbane (1911)  
  
本章主要介绍 matplotlib 和 plotly 包的基本可视化功能。  
  
尽管还有更多其他可视化包可用，但 matplotlib 已将其确立为基准，并且在许多情况下，它是一种健壮且可靠的可视化工具。对于标准绘图来说它既易于使用，在处理更复杂的绘图和自定义时又非常灵活。此外，它与 NumPy 和 pandas 及其提供的数据结构紧密集成。  
  
matplotlib 仅允许以位图的形式生成绘图（例如，PNG 或 JPG 格式）。另一方面，基于例如数据驱动文档（D3.js）标准的现代 Web 技术，允许生成漂亮的可交互且可嵌入的图表（例如，可交互是指可以放大以更详细地检查某些区域）。使用 Python 创建此类 D3.js 图表的便捷包是 plotly。一个名为 Cufflinks 的较小附加库将 plotly 与 pandas 的 DataFrame 对象紧密集成在一起，并允许创建流行的金融图表（例如K线图）。  
  
本章主要涵盖以下主题：  
  
第168页的“静态2D绘图”  
本节介绍 matplotlib 并展示了一系列典型的2D绘图，从最简单的绘图到带有双比例尺或不同子图的一些更高级的绘图。  
  
第191页的“静态3D绘图”  
基于 matplotlib，本节展示了一组对于某些金融应用很有用的3D绘图。  
  
第195页的“交互式2D绘图”  
本节介绍使用 plotly 和 Cufflinks 来创建交互式2D绘图。利用 Cufflinks 的 QuantFigure 特性，本节还介绍了例如在股票技术分析中使用的典型金融绘图。  
  
本章无法详尽地介绍使用 Python、matplotlib 或 plotly 进行数据可视化的所有内容，但它为这些包在金融领域的基本且重要的功能提供了一些示例。后面的章节中也可以找到其他示例。例如，第8章展示了如何使用 pandas 库更深入地可视化金融时间序列数据。  
  
## 静态2D绘图  
  
在创建样本数据并开始绘图之前，先进行一些导入和自定义：  
  
```python  
In [1]: import matplotlib as mpl # 导入matplotlib，并使用常用缩写mpl。  
In [2]: mpl.__version__ # 使用的matplotlib的版本。  
```  
  
```txt  
Out[2]: '3.0.0'  
```  
  
```python  
In [3]: import matplotlib.pyplot as plt # 导入主绘图（子）包，并使用常用缩写plt。  
In [4]: plt.style.use('seaborn') # 将绘图样式设置为seaborn。  
In [5]: mpl.rcParams['font.family'] = 'serif' # 在所有绘图中将字体设置为serif。  
In [6]: %matplotlib inline  
```  
  
### 一维数据集  
  
最基本但非常强大的绘图函数是 `plt.plot()`。原则上，它需要两组数字：  
  
x值  
包含x坐标（横坐标值）的列表或数组  
  
y值  
包含y坐标（纵坐标值）的列表或数组  
  
当然，提供的x和y值的数量必须匹配。考虑以下代码，其输出显示在图7-1中：  
  
```python  
In [7]: import numpy as np  
In [8]: np.random.seed(1000) # 固定随机数生成器的种子以保证可重复性。  
In [9]: y = np.random.standard_normal(20) # 抽取随机数（y值）。  
In [10]: x = np.arange(len(y)) # 固定整数（x值）。  
         plt.plot(x, y); # 使用x和y对象调用plt.plot()函数。  
```  
  
<img width="666" height="458" alt="python_for_finance 7 1" src="https://github.com/user-attachments/assets/0cf7afa2-ea7c-47db-be77-5ac34a2d90e0" />
  
图7-1。给定x和y值的绘图  
  
当传递 `ndarray` 对象时，`plt.plot()` 能够识别。在这种情况下，无需提供x值的“额外”信息。如果只提供y值，`plt.plot()` 会将索引值作为各自的x值。因此，以下单行代码生成完全相同的输出（见图7-2）：  
  
```python  
In [11]: plt.plot(y);  
```  
  
<img width="667" height="456" alt="python_for_finance 7 2" src="https://github.com/user-attachments/assets/b2144e9c-588b-48a2-8f5c-8992f27be377" />
  
图7-2。给定数据作为ndarray对象的绘图  
  
**NumPy数组和matplotlib**  
  
你可以简单地将 NumPy 的 `ndarray` 对象传递给 matplotlib 函数。matplotlib 能够解释数据结构以简化绘图。但是，请注意不要传递太大或太复杂的数组。  
  
由于大多数 `ndarray` 方法返回一个 `ndarray` 对象，我们也可以传递附加了方法（在某些情况下甚至多个方法）的对象。通过在具有样本数据的 `ndarray` 对象上调用 `cumsum()` 方法，我们可以获得该数据的累积和，并且不出所料，会得到不同的输出（见图7-3）：  
  
```python  
In [12]: plt.plot(y.cumsum());  
```  
  
<img width="665" height="456" alt="python_for_finance 7 3" src="https://github.com/user-attachments/assets/617da3af-298f-4187-8dee-0501ce93418f" />
  
图7-3。给定一个附带方法的ndarray对象的绘图  
  
通常，默认的绘图样式不能满足报告、出版物等的典型要求。例如，你可能想要自定义所使用的字体（例如，为了与LaTeX字体兼容），在轴上添加标签，或者为了更好的可读性绘制网格。这就是绘图样式发挥作用的地方。此外，matplotlib 提供了大量函数来自定义绘图样式。有些很容易访问；而另一些则需要稍微深入挖掘。例如，容易访问的是那些操作轴的函数以及与网格和标签相关的函数（见图7-4）：  
  
```python  
In [13]: plt.plot(y.cumsum())  
         plt.grid(False) # 关闭网格。  
         plt.axis('equal'); # 使两个轴进行等比例缩放。  
```  
  
<img width="666" height="467" alt="python_for_finance 7 4" src="https://github.com/user-attachments/assets/da7bd06f-b33d-4680-95ba-05f18a3da8da" />
  
图7-4。没有网格的绘图  
  
`plt.axis()` 的其他选项见表7-1，其中大多数必须作为 `str` 对象传递。  
  
<img width="455" height="245" alt="python_for_finance 7 1 1" src="https://github.com/user-attachments/assets/8439d6c5-989d-42c0-a139-a43ea8c89247" />
  
表7-1。plt.axis()的选项  
  
此外，可以通过使用 `plt.xlim()` 和 `plt.ylim()` 直接设置每个轴的最小值和最大值。以下代码提供了一个示例，其输出显示在图7-5中：  
  
```python  
In [14]: plt.plot(y.cumsum())  
         plt.xlim(-1, 20)  
         plt.ylim(np.min(y.cumsum()) - 1,  
                  np.max(y.cumsum()) + 1);  
```  
  
<img width="665" height="455" alt="python_for_finance 7 5" src="https://github.com/user-attachments/assets/d4bf9da0-ba27-429f-84e8-72e4a551739e" />
  
图7-5。带有自定义轴限制的绘图  
  
为了具有更好的可读性，图表通常包含一些标签——例如，标题以及描述x和y值性质的标签。这些分别由函数 `plt.title()`、`plt.xlabel()` 和 `plt.ylabel()` 添加。默认情况下，即使提供了离散的数据点，`plot()` 也会绘制连续的线。绘制离散点是通过选择不同的样式选项来完成的。图7-6叠加了（红色）点和（蓝色）线宽为1.5磅的线：  
  
```python  
In [15]: plt.figure(figsize=(10, 6)) # 增加图形的大小。  
         plt.plot(y.cumsum(), 'b', lw=1.5) # 将数据绘制为蓝色线条，线宽为1.5磅。  
         plt.plot(y.cumsum(), 'ro') # 将数据绘制为红色（粗）点。  
         plt.xlabel('index') # 在x轴上放置标签。  
         plt.ylabel('value') # 在y轴上放置标签。  
         plt.title('A Simple Plot'); # 放置标题。  
```  
  
<img width="665" height="441" alt="python_for_finance 7 6" src="https://github.com/user-attachments/assets/50b22dad-5326-4dd6-87ec-4db8a254f0cd" />
  
图7-6。带有典型标签的绘图  
  
默认情况下，`plt.plot()` 支持表7-2中的颜色缩写。  
  
<img width="294" height="272" alt="python_for_finance 7 2 1" src="https://github.com/user-attachments/assets/48fd55d5-0226-449f-a43c-8c1dd0aa9842" />
  
表7-2。标准颜色缩写  
  
在线条和/或点样式方面，`plt.plot()` 支持列在表7-3中的字符。  
  
<img width="264" height="747" alt="python_for_finance 7 3 1" src="https://github.com/user-attachments/assets/70ed039b-9a02-4903-bcd6-969abf6b3883" />
  
表7-3。标准样式字符  
  
任何颜色缩写都可以与任何样式字符组合。通过这种方式，可以确保不同的数据集很容易区分。绘图样式也反映在图例中。  
  
### 二维数据集  
  
绘制一维数据可以被视为一种特例。通常，数据集将由多个独立的数据子集组成。在 matplotlib 中处理此类数据集遵循与一维数据相同的规则。然而，在这样的背景下可能会出现许多其他问题。例如，两个数据集可能具有如此不同的比例尺，以至于它们不能使用相同的y轴和/或x轴比例尺来绘制。另一个问题可能是我们可能希望以不同的方式可视化两个不同的数据集，例如，一个通过折线图，另一个通过条形图。  
  
以下代码生成了一个二维样本数据集，作为形状为 20×2 的包含标准正态分布伪随机数的 NumPy `ndarray` 对象。在此数组上调用 `cumsum()` 方法来沿轴 0（即第一个维度）计算样本数据的累积和：  
  
```python  
In [16]: y = np.random.standard_normal((20, 2)).cumsum(axis=0)  
```  
  
通常，也可以将此类二维数组传递给 `plt.plot()`。然后它将自动把包含的数据解释为独立的数据集（沿轴1，即第二个维度）。相应的绘图如图7-7所示：  
  
```python  
In [17]: plt.figure(figsize=(10, 6))  
         plt.plot(y, lw=1.5)  
         plt.plot(y, 'ro')  
         plt.xlabel('index')  
         plt.ylabel('value')  
         plt.title('A Simple Plot');  
```  
  
<img width="665" height="448" alt="python_for_finance 7 7" src="https://github.com/user-attachments/assets/1ab57e16-ff9a-4cfe-aab0-372bdc65336b" />
  
图7-7。具有两个数据集的绘图  
  
在这种情况下，进一步的注释可能有助于更好地读取图表。你可以向每个数据集添加单独的标签，并将它们列在图例中。函数 `plt.legend()` 接受不同的位置参数。0 代表最佳位置，意思是尽量少隐藏数据。  
  
图7-8显示了两个数据集的图表，这次带有图例。在生成代码中，`ndarray` 对象没有作为一个整体传递，而是分别访问两个数据子集（`y[:, 0]` 和 `y[:, 1]`），这允许你为它们附加单独的标签：  
  
```python  
In [18]: plt.figure(figsize=(10, 6))  
         plt.plot(y[:, 0], lw=1.5, label='1st') # 为数据子集定义标签。  
         plt.plot(y[:, 1], lw=1.5, label='2nd') # 为数据子集定义标签。  
         plt.plot(y, 'ro')  
         plt.legend(loc=0) # 在“最佳”位置放置图例。  
         plt.xlabel('index')  
         plt.ylabel('value')  
         plt.title('A Simple Plot');  
```  
  
<img width="665" height="448" alt="python_for_finance 7 8" src="https://github.com/user-attachments/assets/567069dc-aa57-4046-8f60-41b5ceb9c2f3" />
  
图7-8。带标签数据集的绘图  
  
`plt.legend()` 的其他位置选项包括列在表7-4中的那些。  
  
<img width="250" height="376" alt="python_for_finance 7 4 1" src="https://github.com/user-attachments/assets/f1c24da1-d386-4bf9-ad81-38484e226fd5" />
  
表7-4。plt.legend()的选项  
  
具有类似比例尺的多个数据集（如同一金融风险因子的模拟路径）可以使用单个y轴绘制。然而，通常数据集显示出相当不同的缩放比例，用单个y比例尺绘制此类数据通常会导致重要的视觉信息丢失。为了说明这种效果，以下示例将两个数据子集中的第一个按100的因子缩放，并再次绘制数据（见图7-9）：  
  
```python  
In [19]: y[:, 0] = y[:, 0] * 100 # 重新缩放第一个数据子集。  
In [20]: plt.figure(figsize=(10, 6))  
         plt.plot(y[:, 0], lw=1.5, label='1st')  
         plt.plot(y[:, 1], lw=1.5, label='2nd')  
         plt.plot(y, 'ro')  
         plt.legend(loc=0)  
         plt.xlabel('index')  
         plt.ylabel('value')  
         plt.title('A Simple Plot');  
```  
  
<img width="666" height="440" alt="python_for_finance 7 9" src="https://github.com/user-attachments/assets/841b157b-4ef6-4f62-a7f3-7651c963981f" />
  
图7-9。带有两个比例尺不同数据集的绘图  
  
观察图7-9可以发现，第一个数据集仍然“视觉可读”，而随着新的y轴比例尺，第二个数据集现在看起来像一条直线。在某种意义上，关于第二个数据集的信息现在“在视觉上丢失了”。有两种基本方法可以通过绘图手段（而不是调整数据，例如通过重缩放）来解决这个问题：  
  
- 使用双y轴（左/右）  
  
- 使用两个子图（上/下，左/右）  
  
以下示例在图中引入了第二个y轴。图7-10现在有两个不同的y轴。左边的y轴对应第一个数据集，而右边的y轴对应第二个数据集。因此，也有两个图例：  
  
```python  
In [21]: fig, ax1 = plt.subplots() # 定义图形和坐标轴对象。  
         plt.plot(y[:, 0], 'b', lw=1.5, label='1st')  
         plt.plot(y[:, 0], 'ro')  
         plt.legend(loc=8)  
         plt.xlabel('index')  
         plt.ylabel('value 1st')  
         plt.title('A Simple Plot')  
         ax2 = ax1.twinx() # 创建一个共享x轴的第二个坐标轴对象。  
         plt.plot(y[:, 1], 'g', lw=1.5, label='2nd')  
         plt.plot(y[:, 1], 'ro')  
         plt.legend(loc=0)  
         plt.ylabel('value 2nd');  
```  
  
<img width="666" height="458" alt="python_for_finance 7 10" src="https://github.com/user-attachments/assets/ebfd7f4e-63f2-43ec-b331-d2fe1307905d" />
  
图7-10。带有两个数据集和双y轴的绘图  
  
帮助管理坐标轴的关键代码行是：  
  
```python  
fig, ax1 = plt.subplots()  
ax2 = ax1.twinx()  
```  
  
通过使用 `plt.subplots()` 函数，人们可以直接访问底层绘图对象（图形，子图等）。例如，它允许生成与第一个子图共享x轴的第二个子图。在图7-10中，两个子图实际上相互叠加。  
  
接下来，考虑两个独立子图的情况。该选项在处理这两个数据集时给予了更多自由，如图7-11所示：  
  
```python  
In [22]: plt.figure(figsize=(10, 6))  
         plt.subplot(211) # 定义上部子图1。  
         plt.plot(y[:, 0], lw=1.5, label='1st')  
         plt.plot(y[:, 0], 'ro')  
         plt.legend(loc=0)  
         plt.ylabel('value')  
         plt.title('A Simple Plot')  
         plt.subplot(212) # 定义下部子图2。  
         plt.plot(y[:, 1], 'g', lw=1.5, label='2nd')  
         plt.plot(y[:, 1], 'ro')  
         plt.legend(loc=0)  
         plt.xlabel('index')  
         plt.ylabel('value');  
```  
  
<img width="666" height="439" alt="python_for_finance 7 11" src="https://github.com/user-attachments/assets/90d8b430-5e87-4e87-addb-74d6f073496d" />
  
图7-11。带有两个子图的绘图  
  
在 matplotlib 的 figure 对象中放置子图是通过使用特定的坐标系统来完成的。`plt.subplot()` 接受三个整数参数，代表 numrows、numcols 和 fignum（这三个整数可以由逗号分隔，也可以不分隔）。numrows 指定行数，numcols 指定列数，fignum 指定子图的编号，从 1 开始到 numrows * numcols 结束。例如，一个包含九个大小相同的子图的图形应具有 numrows=3、numcols=3 和 fignum=1,2,...,9。右下角的子图应具有以下“坐标”：`plt.subplot(3, 3, 9)`。  
  
有时，可能需要或希望选择两种不同的图表类型来可视化这些数据。通过子图方法，我们可以自由组合 matplotlib 提供的任意类型的绘图。1 有关可用绘图类型的概述，请访问 matplotlib 画廊。  
  
图7-12结合了线/点图和条形图：  
  
```python  
In [23]: plt.figure(figsize=(10, 6))  
         plt.subplot(121)  
         plt.plot(y[:, 0], lw=1.5, label='1st')  
         plt.plot(y[:, 0], 'ro')  
         plt.legend(loc=0)  
         plt.xlabel('index')  
         plt.ylabel('value')  
         plt.title('1st Data Set')  
         plt.subplot(122)  
         plt.bar(np.arange(len(y)), y[:, 1], width=0.5,  
                 color='g', label='2nd') # 创建一个条形图子图。  
         plt.legend(loc=0)  
         plt.xlabel('index')  
         plt.title('2nd Data Set');  
```  
  
<img width="666" height="440" alt="python_for_finance 7 12" src="https://github.com/user-attachments/assets/6505434a-d00f-4b82-8147-479f57ef7efa" />
  
图7-12。将线/点图子图与条形图子图结合的图  
  
### 其他绘图样式  
  
在涉及二维绘图时，线图和点图可能是金融领域中最重要的；这是因为许多数据集体现了时间序列数据，而这些数据通常由这种图表可视化。第8章详细讨论了金融时间序列数据。然而，本节坚持使用随机数的二维数据集并展示了一些替代的、并且对金融应用有用的可视化方法。  
  
第一个是散点图，其中一个数据集的值充当另一个数据集的x值。图7-13显示了这样一个图表。例如，这种图表类型可用于绘制一个金融时间序列的收益与另一个时间序列的收益。本示例使用具有更多数据的新二维数据集：  
  
```python  
In [24]: y = np.random.standard_normal((1000, 2)) # 创建包含随机数的更大数据集。  
In [25]: plt.figure(figsize=(10, 6))  
         plt.plot(y[:, 0], y[:, 1], 'ro') # 通过plt.plot()函数生成的散点图。  
         plt.xlabel('1st')  
         plt.ylabel('2nd')  
         plt.title('Scatter Plot');  
```  
  
<img width="666" height="449" alt="python_for_finance 7 13" src="https://github.com/user-attachments/assets/8c841c1b-7629-451f-a4fd-da68d69e5aec" />
  
图7-13。通过plt.plot()函数绘制的散点图  
  
matplotlib 还提供了一个专门用于生成散点图的函数。它在基本工作原理上相同，但提供了一些附加功能。图7-14显示了与图7-13对应的散点图，这次使用 `plt.scatter()` 函数生成：  
  
```python  
In [26]: plt.figure(figsize=(10, 6))  
         plt.scatter(y[:, 0], y[:, 1], marker='o') # 通过plt.scatter()函数生成的散点图。  
         plt.xlabel('1st')  
         plt.ylabel('2nd')  
         plt.title('Scatter Plot');  
```  
  
<img width="664" height="447" alt="python_for_finance 7 14" src="https://github.com/user-attachments/assets/2cab16fe-0a6d-4c81-bc02-161d48895ac7" />
  
图7-14。通过plt.scatter()函数绘制的散点图  
  
除其他外，`plt.scatter()` 绘图函数允许添加第三个维度，这可以通过不同颜色来可视化，并由颜色条描述。图7-15显示了一个散点图，其中通过单个点的不同颜色以及作为颜色图例的颜色条，说明了第三个维度的存在。为此，以下代码生成了具有随机数据的第三个数据集，这次由 0 到 10 之间的整数组成：  
  
```python  
In [27]: c = np.random.randint(0, 10, len(y))  
In [28]: plt.figure(figsize=(10, 6))  
         plt.scatter(y[:, 0], y[:, 1],  
                     c=c, # 包含第三个数据集。  
                     cmap='coolwarm', # 选择颜色映射。  
                     marker='o') # 将标记定义为粗点。  
         plt.colorbar()  
         plt.xlabel('1st')  
         plt.ylabel('2nd')  
         plt.title('Scatter Plot');  
```  
  
<img width="665" height="488" alt="python_for_finance 7 15" src="https://github.com/user-attachments/assets/36780f0f-1fe2-4004-bac1-faa11ca97c84" />
  
图7-15。带有第三个维度的散点图  
  
另一种类型的图——直方图，也经常用于金融收益的背景下。图7-16将两个数据集的频率值并排放在同一个图表中：  
  
```python  
In [29]: plt.figure(figsize=(10, 6))  
         plt.hist(y, label=['1st', '2nd'], bins=25) # 通过plt.hist()函数生成的直方图。  
         plt.legend(loc=0)  
         plt.xlabel('value')  
         plt.ylabel('frequency')  
         plt.title('Histogram');  
```  
  
<img width="664" height="445" alt="python_for_finance 7 16" src="https://github.com/user-attachments/assets/14ee9696-3fa4-4149-aaeb-252db9846e5a" />
  
图7-16。两个数据集的直方图  
  
既然直方图对于金融应用来说是如此重要的一种绘图类型，让我们仔细看看 `plt.hist()` 的用法。下面的示例说明了支持的参数：  
  
```python  
plt.hist(x, bins=10, range=None, normed=False, weights=None, cumulative=False,  
bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None,  
log=False, color=None, label=None, stacked=False, hold=None, **kwargs)  
```  
  
表7-5描述了 `plt.hist()` 函数的主要参数。  
  
<img width="438" height="426" alt="python_for_finance 7 5 1" src="https://github.com/user-attachments/assets/cafcd924-cc58-4fca-884b-686f169b829f" />
  
表7-5。plt.hist()的参数  
  
图7-17展示了类似的绘图；这一次，两个数据集的数据在直方图中进行了堆叠：  
  
```python  
In [30]: plt.figure(figsize=(10, 6))  
         plt.hist(y, label=['1st', '2nd'], color=['b', 'g'],  
                  stacked=True, bins=20, alpha=0.5)  
         plt.legend(loc=0)  
         plt.xlabel('value')  
         plt.ylabel('frequency')  
         plt.title('Histogram');  
```  
  
<img width="665" height="446" alt="python_for_finance 7 17" src="https://github.com/user-attachments/assets/54e25d39-10b6-4885-ad49-9319d0029b23" />
  
图7-17。两个数据集的堆叠直方图  
  
另一种有用的绘图类型是箱形图。与直方图类似，箱形图允许简明地概述数据集的特征，并能够轻松比较多个数据集。图7-18展示了我们数据集的这样一种绘图：  
  
```python  
In [31]: fig, ax = plt.subplots(figsize=(10, 6))  
         plt.boxplot(y) # 通过plt.boxplot()函数生成的箱形图。  
         plt.setp(ax, xticklabels=['1st', '2nd']) # 设置单独的x轴标签。  
         plt.xlabel('data set')  
         plt.ylabel('value')  
         plt.title('Boxplot');  
```  
  
<img width="664" height="448" alt="python_for_finance 7 18" src="https://github.com/user-attachments/assets/dac53b1f-0545-40f9-aea4-e0987fc8bc99" />
  
图7-18。两个数据集的箱形图  
  
这最后一个示例使用了 `plt.setp()` 函数，该函数为（一组）绘图实例设置属性。例如，考虑由以下代码生成的折线图：  
  
```python  
line = plt.plot(data, 'r')  
```  
  
下面的代码将线条的样式更改为“虚线”：  
  
```python  
plt.setp(line, linestyle='--')  
```  
  
这样，我们就可以在生成绘图实例（“artist 对象”）之后轻松更改参数。  
  
作为本节的最后一个图例，考虑一个受数学启发的绘图，它也可以在 matplotlib 画廊中作为示例找到。它绘制了一个函数，并在图形上突出显示了函数从下限到上限下方的区域——换句话说，即函数在上下限之间的积分值。要演示的积分（值）是 $\int_a^b f(x)dx$ ，其中 $f(x)=\frac{1}{2}\cdot e^x+1$ ， $a=\frac{1}{2}$ ，且 $b=\frac{3}{2}$ 。图7-19显示了结果图，并演示了 matplotlib 在将数学公式包含在图形中时可以无缝处理 LaTeX 排版。  
  
首先是函数定义，以积分限作为变量，并为x和y值准备数据集：  
  
```python  
In [32]: def func(x):  
             return 0.5 * np.exp(x) + 1 # 函数定义。  
         a, b = 0.5, 1.5 # 积分限。  
         x = np.linspace(0, 2) # 要绘制该函数的x值。  
         y = func(x) # 要绘制该函数的y值。  
         Ix = np.linspace(a, b) # 积分限度内的x值。  
         Iy = func(Ix) # 积分限度内的y值。  
         verts = [(a, 0)] + list(zip(Ix, Iy)) + [(b, 0)] # 带有多个元组对象的列表对象，表示要绘制的多边形的坐标。  
```  
  
其次是绘图本身，由于要明确放置许多单一对象，因此有点复杂：  
  
```python  
In [33]: from matplotlib.patches import Polygon  
         fig, ax = plt.subplots(figsize=(10, 6))  
         plt.plot(x, y, 'b', linewidth=2) # 将函数值绘制成蓝线。  
         plt.ylim(bottom=0) # 定义纵坐标轴的最小y值。  
         poly = Polygon(verts, facecolor='0.7', edgecolor='0.5') # 将多边形（积分区域）绘制为灰色。  
         ax.add_patch(poly)   
         plt.text(0.5 * (a + b), 1, r'$\int_a^b f(x)\mathrm{d}x$',  
                  horizontalalignment='center', fontsize=20) # 在图表中放置积分公式。  
         plt.figtext(0.9, 0.075, '$x$') # 放置轴标签。  
         plt.figtext(0.075, 0.9, '$f(x)$') # 放置轴标签。  
         ax.set_xticks((a, b)) # 放置x标签。  
         ax.set_xticklabels(('$a$', '$b$'))   
         ax.set_yticks([func(a), func(b)]) # 放置y标签。  
         ax.set_yticklabels(('$f(a)$', '$f(b)$'));   
```  
  
<img width="665" height="437" alt="python_for_finance 7 19" src="https://github.com/user-attachments/assets/42dbc266-1efb-4057-a490-0ee16964ed40" />
  
图7-19。指数函数、积分区域以及LaTeX标签  
  
## 静态3D绘图  
  
金融领域中能够真正从三维可视化中受益的领域并不多。然而，一个应用领域是波动率曲面，它同时显示出所交易期权的各种到期时间和执行价格的隐含波动率。另见附录 B 中为欧式看涨期权可视化的价值和 vega 曲面的示例。在下文中，代码人为地生成了一个类似于波动率曲面的图形。为此，考虑以下参数：  
  
- 执行价格在50和150之间  
  
- 到期时间在0.5和2.5年之间  
  
这提供了一个二维坐标系统。NumPy的 `np.meshgrid()` 函数可以利用两个一维的 `ndarray` 对象生成这样一个系统：  
  
```python  
In [34]: strike = np.linspace(50, 150, 24) # 带有执行价格值的ndarray对象。  
In [35]: ttm = np.linspace(0.5, 2.5, 24) # 带有到期时间值的ndarray对象。  
In [36]: strike, ttm = np.meshgrid(strike, ttm) # 创建了两个二维ndarray对象（网格）。  
In [37]: strike[:2].round(1)   
```  
  
```txt  
Out[37]: array([[  50. ,   54.3,   58.7,   63. ,   67.4,   71.7,   76.1,   80.4,   84.8,  
           89.1,   93.5,   97.8,  102.2,  106.5,  110.9,  115.2,  119.6,  123.9,  
          128.3,  132.6,  137. ,  141.3,  145.7,  150. ],  
         [  50. ,   54.3,   58.7,   63. ,   67.4,   71.7,   76.1,   80.4,   84.8,  
           89.1,   93.5,   97.8,  102.2,  106.5,  110.9,  115.2,  119.6,  123.9,  
          128.3,  132.6,  137. ,  141.3,  145.7,  150. ]])  
```  
  
```python  
In [38]: iv = (strike - 100) ** 2 / (100 * strike) / ttm # 虚拟的隐含波动率值。  
In [39]: iv[:5, :3]   
```  
  
```txt  
Out[39]: array([[1.        , 0.76695652, 0.58132045],  
         [0.85185185, 0.65333333, 0.4951989 ],  
         [0.74193548, 0.56903226, 0.43130227],  
         [0.65714286, 0.504     , 0.38201058],  
         [0.58974359, 0.45230769, 0.34283001]])  
```  
  
由以下代码生成的图表显示在图7-20中：  
  
```python  
In [40]: from mpl_toolkits.mplot3d import Axes3D # 导入相关的3D绘图功能，虽然Axes3D没有被直接使用，但这是必需的。  
         fig = plt.figure(figsize=(10, 6)) # 设置3D绘图的画布。  
         ax = fig.gca(projection='3d') # 创建3D图。  
         surf = ax.plot_surface(strike, ttm, iv, rstride=2, cstride=2,  
                                cmap=plt.cm.coolwarm, linewidth=0.5,  
                                antialiased=True)   
         ax.set_xlabel('strike') # 设置x轴标签。  
         ax.set_ylabel('time-to-maturity') # 设置y轴标签。  
         ax.set_zlabel('implied volatility') # 设置z轴标签。  
         fig.colorbar(surf, shrink=0.5, aspect=5); # 创建颜色条。  
```  
  
<img width="665" height="444" alt="python_for_finance 7 20" src="https://github.com/user-attachments/assets/382685f1-af7b-4aec-983a-063488260a33" />
  
图7-20。（虚拟的）隐含波动率的3D曲面图  
  
表7-6提供了 `plt.plot_surface()` 函数可以采用的不同参数的描述。  
  
<img width="361" height="323" alt="python_for_finance 7 6 1" src="https://github.com/user-attachments/assets/6a48d548-2b42-4920-997a-f757b508cf48" />
  
表7-6。plot_surface()的参数  
  
与二维图一样，折线样式可以替换为单点，或者如下文所示，替换为单个三角形。图7-21将相同的数据绘制为3D散点图，但现在使用 `view_init()` 方法设置了不同的观看角度：  
  
```python  
In [41]: fig = plt.figure(figsize=(10, 6))  
         ax = fig.add_subplot(111, projection='3d')  
         ax.view_init(30, 60) # 设置观看角度。  
         ax.scatter(strike, ttm, iv, zdir='z', s=25,  
                    c='b', marker='^') # 创建3D散点图。  
         ax.set_xlabel('strike')  
         ax.set_ylabel('time-to-maturity')  
         ax.set_zlabel('implied volatility');  
```  
  
<img width="665" height="424" alt="python_for_finance 7 21" src="https://github.com/user-attachments/assets/3db4aae8-1e79-41f1-acf4-1b939db8a5c8" />
  
图7-21。（虚拟的）隐含波动率的3D散点图  
  
## 交互式2D绘图  
  
matplotlib 允许你创建静态位图对象或 PDF 格式的绘图。如今，有许多可用的库可以创建基于 D3.js 标准的交互式绘图。此类绘图支持放大和缩小、用于数据检查的悬停效果等。一般来说，它们也可以轻松嵌入到网页中。  
  
一个流行的平台和绘图库是 plotly。它致力于数据科学的可视化，并在数据科学界得到广泛使用。plotly 的主要优点在于它与 Python 生态系统的紧密集成和易用性——特别是当它与 pandas 的 DataFrame 对象和包装包 Cufflinks 结合使用时。  
  
对于某些功能，需要一个免费账户。获得凭据后，应将它们存储在本地以供永久使用。有关详细信息，请参阅“Getting Started with Plotly for Python”指南。  
  
本节仅关注特定方面，其中完全使用 Cufflinks 来从存储在 DataFrame 对象中的数据创建交互式绘图。  
  
### 基本绘图  
  
要在 Jupyter Notebook 环境中开始，需要进行一些导入并开启笔记本模式（notebook mode）：  
  
```python  
In [42]: import pandas as pd  
In [43]: import cufflinks as cf # 导入Cufflinks。  
In [44]: import plotly.offline as plyo # 导入plotly的离线绘图功能。  
In [45]: plyo.init_notebook_mode(connected=True) # 开启笔记本绘图模式。  
```  
  
**远程或本地渲染**  
  
使用 plotly，还可以选择在 plotly 服务器上渲染图表。然而，笔记本模式通常要快得多，尤其是在处理大型数据集时。话虽如此，一些功能，比如 plotly 的流式绘图服务，只能通过与服务器通信来使用。  
  
接下来的示例同样依赖于伪随机数，这次存储在具有 `DatetimeIndex` 的 `DataFrame` 对象中（即，作为时间序列数据）：  
  
```python  
In [46]: a = np.random.standard_normal((250, 5)).cumsum(axis=0) # 标准正态分布伪随机数。  
In [47]: index = pd.date_range('2019-1-1', # DatetimeIndex对象的起始日期。  
                               freq='B', # 频率（“工作日”）。  
                               periods=len(a)) # 所需的周期数。  
In [48]: df = pd.DataFrame(100 + 5 * a, # 对原始数据进行线性变换。  
                           columns=list('abcde'), # 将单字符作为列标题。  
                           index=index) # DatetimeIndex对象。  
In [49]: df.head() # 数据的前五行。  
```  
  
```txt  
Out[49]:                  a           b           c           d           e  
2019-01-01  109.037535   98.693865  104.474094   96.878857  100.621936  
2019-01-02  107.598242   97.005738  106.789189   97.966552  100.175313  
2019-01-03  101.639668  100.332253  103.183500   99.747869  107.902901  
2019-01-04   98.500363  101.208283  100.966242   94.023898  104.387256  
2019-01-07   93.941632  103.319168  105.674012   95.891062   86.547934  
```  
  
Cufflinks 向 `DataFrame` 类添加了一个新方法：`df.iplot()`。该方法在后端使用 plotly 创建交互式绘图。本节中的代码示例都使用了将交互式图表下载为静态位图的选项，然后将其嵌入到文本中。在 Jupyter Notebook 环境中，创建的图表都是交互式的。以下代码的结果如图7-22所示：  
  
```python  
In [50]: plyo.iplot( # 这使用了plotly的离线（笔记本模式）功能。  
             df.iplot(asFigure=True), # 使用参数asFigure=True调用df.iplot()方法，以允许本地绘图和嵌入。  
             image='png', # image选项提供绘图的额外静态位图版本。  
             filename='ply_01' # 指定要保存的位图的文件名（文件扩展名会自动添加）。  
         )  
```  
  
<img width="664" height="431" alt="python_for_finance 7 22" src="https://github.com/user-attachments/assets/0e18e808-a5d6-4a21-ba3e-d76f2161b321" />
  
图7-22。使用plotly、pandas和Cufflinks绘制的时间序列数据折线图  
  
通常与 matplotlib 和 pandas 绘图功能一样，有多个参数可用于自定义此类图表（见图7-23）：  
  
```python  
In [51]: plyo.iplot(  
             df[['a', 'b']].iplot(asFigure=True,  
                                  theme='polar', # 为绘图选择一个主题（绘图样式）。  
                                  title='A Time Series Plot', # 添加标题。  
                                  xTitle='date', # 添加x轴标签。  
                                  yTitle='value', # 添加y轴标签。  
                                  mode={'a': 'markers', 'b': 'lines+markers'}, # 按列定义绘图模式（线、标记等）。  
                                  symbol={'a': 'circle', 'b': 'diamond'}, # 按列定义用作标记的符号。  
                                  size=3.5, # 固定所有标记的大小。  
                                  colors={'a': 'blue', 'b': 'magenta'}, # 按列指定绘图颜色。  
                                  ),  
             image='png',  
             filename='ply_02'  
         )  
```  
  
<img width="665" height="505" alt="python_for_finance 7 23" src="https://github.com/user-attachments/assets/0387a691-a6ca-4f87-ae50-c53352ddb551" />
  
图7-23。带有自定义设置的DataFrame对象中两列数据的折线图  
  
与 matplotlib 类似，plotly 允许使用多种不同的绘图类型。通过 Cufflinks 可用的绘图类型有 chart, scatter, bar, box, spread, ratio, heat map, surface, histogram, bubble, bubble3d, scatter3d, scattergeo, ohlc, candle, pie, 以及 choropleth。作为折线图之外不同绘图类型的示例，考虑直方图（见图7-24）：  
  
```python  
In [52]: plyo.iplot(  
             df.iplot(kind='hist', # 指定绘图类型。  
                      subplots=True, # 需要为每列提供单独的子图。  
                      bins=15, # 设置bins参数（要使用的桶 = 要绘制的条形）。  
                      asFigure=True),  
             image='png',  
             filename='ply_03'  
         )  
```  
  
<img width="665" height="434" alt="python_for_finance 7 24" src="https://github.com/user-attachments/assets/41c76744-9f8e-4195-a28e-c00621b07b49" />
  
图7-24。DataFrame对象每列的直方图  
  
### 金融图表  
  
在处理金融时间序列数据时，plotly、Cufflinks 和 pandas 的组合显得尤其强大。Cufflinks 提供了专门的功能来创建典型的金融图表，并添加典型的金融图表元素，例如相对强弱指数（RSI），这只是其中的一个例子。为此，创建了一个持久的 `QuantFig` 对象，可以使用与 Cufflinks 中的 `DataFrame` 对象相同的方法绘制该对象。  
  
本小节使用了一个真实的金融数据集，即 EUR/USD 汇率的时间序列数据（来源：FXCM Forex Capital Markets Ltd.）：  
  
```python  
In [54]: raw = pd.read_csv('../../source/fxcm_eur_usd_eod_data.csv',  
                           index_col=0, parse_dates=True) # 从CSV文件读取金融数据。  
In [55]: raw.info() # 生成的DataFrame对象由多个列和超过1,500行数据组成。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 1547 entries, 2013-01-01 22:00:00 to 2017-12-31 22:00:00  
Data columns (total 8 columns):  
BidOpen      1547 non-null float64  
BidHigh      1547 non-null float64  
BidLow       1547 non-null float64  
BidClose     1547 non-null float64  
AskOpen      1547 non-null float64  
AskHigh      1547 non-null float64  
AskLow       1547 non-null float64  
AskClose     1547 non-null float64  
dtypes: float64(8)  
memory usage: 108.8 KB  
```  
  
```python  
In [56]: quotes = raw[['AskOpen', 'AskHigh', 'AskLow', 'AskClose']] # 从DataFrame对象中选择四列（开盘-最高-最低-收盘，或OHLC）。  
         quotes = quotes.iloc[-60:] # 可视化仅使用少量数据行。  
         quotes.tail() # 返回结果数据集quotes的最后五行。  
```  
  
```txt  
Out[56]:                      AskOpen  AskHigh   AskLow  AskClose  
2017-12-25 22:00:00  1.18667  1.18791  1.18467   1.18587  
2017-12-26 22:00:00  1.18587  1.19104  1.18552   1.18885  
2017-12-27 22:00:00  1.18885  1.19592  1.18885   1.19426  
2017-12-28 22:00:00  1.19426  1.20256  1.19369   1.20092  
2017-12-31 22:00:00  1.20092  1.20144  1.19994   1.20144  
```  
  
在实例化期间，`QuantFig` 对象将 `DataFrame` 对象作为输入并允许进行一些基本自定义。然后使用 `qf.iplot()` 方法绘制存储在 `QuantFig` 对象 `qf` 中的数据（见图7-25）：  
  
```python  
In [57]: qf = cf.QuantFig(  
             quotes, # 将DataFrame对象传递给QuantFig构造函数。  
             title='EUR/USD Exchange Rate', # 添加图形标题。  
             legend='top', # 将图例置于图表顶部。  
             name='EUR/USD' # 为数据集命名。  
         )  
In [58]: plyo.iplot(  
             qf.iplot(asFigure=True),  
             image='png',  
             filename='qf_01'  
         )  
```  
  
<img width="664" height="547" alt="python_for_finance 7 25" src="https://github.com/user-attachments/assets/b0d6ee73-5e3a-4ddc-87c9-7bf1b59174ef" />
  
图7-25。EUR/USD数据的OHLC图  
  
可以通过为 `QuantFig` 对象提供的不同方法添加典型的金融图表元素，例如布林带（Bollinger bands）（见图7-26）：  
  
```python  
In [59]: qf.add_bollinger_bands(periods=15, # 布林带的周期数。  
                                boll_std=2) # 用于带宽度（band width）的标准差乘数。  
In [60]: plyo.iplot(qf.iplot(asFigure=True),  
                    image='png',  
                    filename='qf_02'  
                    )  
```  
  
<img width="666" height="546" alt="python_for_finance 7 26" src="https://github.com/user-attachments/assets/aaebd85d-78b9-4a32-9026-7f9aa563a7ec" />
  
图7-26。带有布林带的EUR/USD数据的OHLC图  
  
某些财务指标（如RSI）可以作为子图添加（见图7-27）：  
  
```python  
In [61]: qf.add_rsi(periods=14, # 固定RSI周期。  
                    showbands=False) # 不显示上界或下界。  
In [62]: plyo.iplot(  
             qf.iplot(asFigure=True),  
             image='png',  
             filename='qf_03'  
         )  
```  
  
<img width="665" height="523" alt="python_for_finance 7 27" src="https://github.com/user-attachments/assets/0b59bb72-bf9c-4ff6-bc4c-c5c9b5a4e19c" />
  
图7-27。带有布林带和RSI的EUR/USD数据的OHLC图  
  
## 结论  
  
在 Python 中的数据可视化方面，matplotlib 既可以被视为基准，也可以被视为全能工具。它与 NumPy 和 pandas 紧密集成，其基本功能很容易并且很方便访问。然而，matplotlib 是一个庞大的库，其 API 稍微有些复杂。这使得在本章中无法全面概述 matplotlib 的所有功能。  
  
本章介绍了 matplotlib 在许多金融背景中有用的 2D 和 3D 绘图的基本功能。其他章节提供了有关如何使用该包进行可视化的更多示例。  
  
此外，本章探讨了 plotly 与 Cufflinks 的结合。这种结合使得创建交互式的 D3.js 图表变成了一件方便的事情，因为通常只需要对 DataFrame 对象进行单次方法调用即可。所有的技术细节都在后端处理好了。而且，Cufflinks 凭借 QuantFig 对象提供了一种简易途径来创建包含热门金融指标的典型金融图表。  
  
## 进一步的资源  
  
网上可以找到多种 matplotlib 的资源，包括：  
  
- 主页，这可能是最好的起点  
- 包含许多有用示例的画廊  
- 2D 绘图教程  
- 3D 绘图教程  
  
查阅画廊、在那里寻找合适的可视化示例，并从相应的示例代码开始，已经成为一种标准惯例。  
  
plotly 和 Cufflinks 包的主要资源也可在网上找到。这些包括：  
  
- plotly 主页  
- 开始使用 Python版 plotly 的教程  
- Cufflinks 的 GitHub 页面  
