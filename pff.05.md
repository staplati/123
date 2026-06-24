# 第5章 使用pandas进行数据分析  
  
数据！数据！数据！没有黏土我怎么做砖！  
——夏洛克·福尔摩斯  
  
本章是关于pandas的，这是一个专注于表格数据的数据分析库。pandas是一个强大的工具，它不仅提供了许多有用的类和函数，而且在封装其他包的功能方面也做得非常出色。其结果是一个用户界面，使得数据分析（特别是财务分析）成为一项方便而高效的任务。  
  
本章涵盖以下基本数据结构：  
  
<img width="482" height="86" alt="python_for_finance 5 0 1" src="https://github.com/user-attachments/assets/3b2ca6db-5f41-4de0-aab6-c2e0ac74f18a" />
  
本章的组织结构如下：  
  
“DataFrame类”在第114页  
本节从使用简单且小型的数据集探索pandas的DataFrame类的基本特征和功能开始；然后展示了如何将NumPy的ndarray对象转换为DataFrame对象。  
  
“基本分析”在第123页和“基本可视化”在第126页  
这些小节介绍了基本分析和可视化功能（后续章节将深入探讨这些主题）。  
  
“Series类”在第128页  
这个相对简短的小节涵盖了pandas的Series类，从某种意义上说，它代表了仅具有单列数据的DataFrame类的特殊情况。  
  
“GroupBy操作”在第130页  
DataFrame类的优势之一在于根据单个或多个列对数据进行分组。本节探讨了pandas的分组功能。  
  
“复杂选择”在第132页  
本节说明了如何使用（复杂）条件，以便从DataFrame对象中轻松选择数据。  
  
“拼接、连接和合并”在第135页  
将不同的数据集组合成一个数据集是数据分析中的一个重要操作。pandas提供了不同的选项来完成这项任务，正如本节所描述的。  
  
“性能方面”在第141页  
像Python本身一样，pandas通常提供多种选项来实现同一目标。本节简要介绍了潜在的性能差异。  
  
## DataFrame类  
  
pandas（以及本章）的核心是DataFrame，这是一个设计用于有效处理表格形式的数据（即以列状组织为特征的数据）的类。为此，DataFrame类提供了一些功能，例如列标签以及数据集中行（记录）的灵活索引功能，类似于关系型数据库或Excel电子表格中的表。  
  
本节涵盖了pandas DataFrame类的一些基本方面。由于该类非常复杂且强大，此处只能展示其部分功能。后续章节提供了更多示例，并从不同方面进行了阐释。  
  
### DataFrame类的初步步骤  
  
从根本上说，DataFrame类的设计目的是管理带有索引和标签的数据，这与SQL数据库表或电子表格应用程序中的工作表没有太大的不同。考虑以下创建DataFrame对象的过程：  
  
```python  
import pandas as pd # ❶ 导入pandas。  
  
df = pd.DataFrame([10, 20, 30, 40], # ❷ 将数据定义为list对象。  
                  columns=['numbers'], # ❸ 指定列标签。  
                  index=['a', 'b', 'c', 'd']) # ❹ 指定索引值/标签。  
  
df # ❺ 显示DataFrame对象的数据以及列和索引标签。  
```  
  
```txt  
Out[3]:   numbers  
      a        10  
      b        20  
      c        30  
      d        40  
```  
  
这个简单的例子在涉及存储数据时，已经展示了DataFrame类的一些主要特性：  
  
- 数据本身可以以不同的形状和类型提供（list、tuple、ndarray和dict对象都是候选对象）。  
- 数据以列的形式组织，列可以有自定义名称（标签）。  
- 有一个可以采用不同格式的索引（例如，数字、字符串、时间信息）。  
  
总的来说，在对象的处理方面，使用DataFrame对象非常方便和高效。例如，与常规的ndarray对象相比，如果想要（比如）扩大现有对象，ndarray对象会更专门化且受限更多。与此同时，DataFrame对象在计算效率上通常与ndarray对象一样高。以下是一些简单的示例，展示了在DataFrame对象上典型操作的工作方式：  
  
```python  
df.index # ❶ index属性和Index对象。  
```  
  
```txt  
Out[4]: Index(['a', 'b', 'c', 'd'], dtype='object')  
```  
  
```python  
df.columns # ❷ columns属性和Index对象。  
```  
  
```txt  
Out[5]: Index(['numbers'], dtype='object')  
```  
  
```python  
df.loc['c'] # ❸ 选择与索引c对应的值。  
```  
  
```txt  
Out[6]: numbers    30  
Name: c, dtype: int64  
```  
  
```python  
df.loc[['a', 'd']] # ❹ 选择与索引a和d对应的两个值。  
```  
  
```txt  
Out[7]:   numbers  
      a        10  
      d        40  
```  
  
```python  
df.iloc[1:3] # ❺ 通过索引位置选择第二和第三行。  
```  
  
```txt  
Out[8]:   numbers  
      b        20  
      c        30  
```  
  
```python  
df.sum() # ❻ 计算单列的总和。  
```  
  
```txt  
Out[9]: numbers    100  
dtype: int64  
```  
  
```python  
df.apply(lambda x: x ** 2) # ❼ 使用apply()方法以向量化方式计算平方。  
```  
  
```txt  
Out[10]:   numbers  
       a       100  
       b       400  
       c       900  
       d      1600  
```  
  
```python  
df ** 2 # ❽ 与ndarray对象一样直接应用向量化。  
```  
  
```txt  
Out[11]:   numbers  
       a       100  
       b       400  
       c       900  
       d      1600  
```  
  
与NumPy的ndarray对象相反，在两个维度上扩大DataFrame对象是可能的：  
  
```python  
df['floats'] = (1.5, 2.5, 3.5, 4.5) # ❶ 添加包含作为tuple对象提供的float对象的新列。  
  
df  
```  
  
```txt  
Out[13]:   numbers  floats  
       a        10     1.5  
       b        20     2.5  
       c        30     3.5  
       d        40     4.5  
```  
  
```python  
df['floats'] # ❷ 选择此列并显示其数据和索引标签。  
```  
  
```txt  
Out[14]: a    1.5  
         b    2.5  
         c    3.5  
         d    4.5  
Name: floats, dtype: float64  
```  
  
也可以获取完整的DataFrame对象来定义新列。在这种情况下，索引将自动对齐：  
  
```python  
df['names'] = pd.DataFrame(['Yves', 'Sandra', 'Lilli', 'Henry'],  
                           index=['d', 'a', 'b', 'c']) # ❶ 基于DataFrame对象创建另一个新列。  
  
df  
```  
  
```txt  
Out[16]:   numbers  floats   names  
       a        10     1.5  Sandra  
       b        20     2.5   Lilli  
       c        30     3.5   Henry  
       d        40     4.5    Yves  
```  
  
追加数据的工作方式类似。然而，在下面的示例中可以看到一个通常要避免的副作用——即索引被简单的范围索引替换：  
  
```python  
df.append({'numbers': 100, 'floats': 5.75, 'names': 'Jil'},  
          ignore_index=True) # ❶ 通过dict对象追加新行；这是一个临时操作，在此期间索引信息会丢失。  
```  
  
```txt  
Out[17]:   numbers  floats   names  
       0        10    1.50  Sandra  
       1        20    2.50   Lilli  
       2        30    3.50   Henry  
       3        40    4.50    Yves  
       4       100    5.75     Jil  
```  
  
```python  
df = df.append(pd.DataFrame({'numbers': 100, 'floats': 5.75,  
                             'names': 'Jil'}, index=['y',])) # ❷ 基于包含索引信息的DataFrame对象追加行；保留了原始索引信息。  
  
df  
```  
  
```txt  
Out[19]:   numbers  floats   names  
       a        10    1.50  Sandra  
       b        20    2.50   Lilli  
       c        30    3.50   Henry  
       d        40    4.50    Yves  
       y       100    5.75     Jil  
```  
  
```python  
df = df.append(pd.DataFrame({'names': 'Liz'}, index=['z',]),  
               sort=False) # ❸ 向DataFrame对象追加不完整的数据行，导致出现NaN值。  
  
df  
```  
  
```txt  
Out[21]:   numbers  floats   names  
       a      10.0    1.50  Sandra  
       b      20.0    2.50   Lilli  
       c      30.0    3.50   Henry  
       d      40.0    4.50    Yves  
       y     100.0    5.75     Jil  
       z       NaN     NaN     Liz  
```  
  
```python  
df.dtypes # ❹ 返回各个列的不同dtypes；这类似于结构化ndarray对象的功能。  
```  
  
```txt  
Out[22]: numbers    float64  
         floats     float64  
         names       object  
dtype: object  
```  
  
尽管现在有缺失值，但大多数方法调用仍将正常工作：  
  
```python  
df[['numbers', 'floats']].mean() # ❶ 计算指定两列的平均值（忽略包含NaN值的行）。  
```  
  
```txt  
Out[23]: numbers    40.00  
         floats      3.55  
dtype: float64  
```  
  
```python  
df[['numbers', 'floats']].std() # ❷ 计算指定两列的标准差（忽略包含NaN值的行）。  
```  
  
```txt  
Out[24]: numbers    35.355339  
         floats      1.662077  
dtype: float64  
```  
  
### DataFrame类的第二步  
  
本小节中的示例基于包含标准正态分布随机数的ndarray对象。它进一步探索了诸如使用DatetimeIndex管理时间序列数据等功能：  
  
```python  
import numpy as np  
  
np.random.seed(100)  
  
a = np.random.standard_normal((9, 4))  
  
a  
```  
  
```txt  
Out[28]: array([[-1.74976547,  0.3426804 ,  1.1530358 , -0.25243604],  
                [ 0.98132079,  0.51421884,  0.22117967, -1.07004333],  
                [-0.18949583,  0.25500144, -0.45802699,  0.43516349],  
                [-0.58359505,  0.81684707,  0.67272081, -0.10441114],  
                [-0.53128038,  1.02973269, -0.43813562, -1.11831825],  
                [ 1.61898166,  1.54160517, -0.25187914, -0.84243574],  
                [ 0.18451869,  0.9370822 ,  0.73100034,  1.36155613],  
                [-0.32623806,  0.05567601,  0.22239961, -1.443217  ],  
                [-0.75635231,  0.81645401,  0.75044476, -0.45594693]])  
```  
  
虽然可以直接构建DataFrame对象（如前所示），但使用ndarray对象通常是一个不错的选择，因为pandas将保留基本结构并“仅”添加元信息（例如，索引值）。这也是一般的科学研究及金融应用程序中的典型用例。例如：  
  
```python  
df = pd.DataFrame(a) # ❶ 从ndarray对象创建DataFrame对象。  
  
df  
```  
  
```txt  
Out[30]:           0         1         2         3  
       0 -1.749765  0.342680  1.153036 -0.252436  
       1  0.981321  0.514219  0.221180 -1.070043  
       2 -0.189496  0.255001 -0.458027  0.435163  
       3 -0.583595  0.816847  0.672721 -0.104411  
       4 -0.531280  1.029733 -0.438136 -1.118318  
       5  1.618982  1.541605 -0.251879 -0.842436  
       6  0.184519  0.937082  0.731000  1.361556  
       7 -0.326238  0.055676  0.222400 -1.443217  
       8 -0.756352  0.816454  0.750445 -0.455947  
```  
  
表5-1列出了DataFrame()函数接受的参数。在表中，“类数组（array-like）”指的是类似于ndarray对象的数据结构——例如list。Index是pandas中Index类的实例。  
  
<img width="629" height="193" alt="python_for_finance 5 1 1" src="https://github.com/user-attachments/assets/015d20e5-1c86-45fc-8b8d-cc6bb88e17a4" />
  
正如结构化数组那样，也如之前所见，DataFrame对象具有列名，可以通过分配一个具有正确元素数量的list对象来直接定义列名。这说明人们可以轻松地定义/更改DataFrame对象的属性：  
  
```python  
df.columns = ['No1', 'No2', 'No3', 'No4'] # ❶ 通过list对象指定列标签。  
  
df  
```  
  
```txt  
Out[32]:         No1       No2       No3       No4  
       0 -1.749765  0.342680  1.153036 -0.252436  
       1  0.981321  0.514219  0.221180 -1.070043  
       2 -0.189496  0.255001 -0.458027  0.435163  
       3 -0.583595  0.816847  0.672721 -0.104411  
       4 -0.531280  1.029733 -0.438136 -1.118318  
       5  1.618982  1.541605 -0.251879 -0.842436  
       6  0.184519  0.937082  0.731000  1.361556  
       7 -0.326238  0.055676  0.222400 -1.443217  
       8 -0.756352  0.816454  0.750445 -0.455947  
```  
  
```python  
df['No2'].mean() # ❷ 现在可以轻松选取一列。  
```  
  
```txt  
Out[33]: 0.7010330941456459  
```  
  
为了有效地处理金融时间序列数据，必须能够很好地处理时间索引。这也可以被认为是pandas的一个主要优势。例如，假设我们四列中的九个数据条目对应于从2019年1月开始的月末数据。然后，使用date_range()函数生成一个DatetimeIndex对象，如下所示：  
  
```python  
dates = pd.date_range('2019-1-1', periods=9, freq='M') # ❶ 创建一个DatetimeIndex对象。  
  
dates  
```  
  
```txt  
Out[35]: DatetimeIndex(['2019-01-31', '2019-02-28', '2019-03-31', '2019-04-30',  
                        '2019-05-31', '2019-06-30', '2019-07-31', '2019-08-31',  
                        '2019-09-30'],  
                       dtype='datetime64[ns]', freq='M')  
```  
  
表5-2列出了date_range()函数接受的参数。  
  
<img width="484" height="247" alt="python_for_finance 5 2 1" src="https://github.com/user-attachments/assets/6c7f990d-caf1-4f8b-a414-45f9db069f2a" />
  
以下代码将刚刚生成的DatetimeIndex对象定义为相关的索引对象，使原始数据集变成了时间序列：  
  
```python  
df.index = dates  
  
df  
```  
  
```txt  
Out[37]:                  No1       No2       No3       No4  
       2019-01-31 -1.749765  0.342680  1.153036 -0.252436  
       2019-02-28  0.981321  0.514219  0.221180 -1.070043  
       2019-03-31 -0.189496  0.255001 -0.458027  0.435163  
       2019-04-30 -0.583595  0.816847  0.672721 -0.104411  
       2019-05-31 -0.531280  1.029733 -0.438136 -1.118318  
       2019-06-30  1.618982  1.541605 -0.251879 -0.842436  
       2019-07-31  0.184519  0.937082  0.731000  1.361556  
       2019-08-31 -0.326238  0.055676  0.222400 -1.443217  
       2019-09-30 -0.756352  0.816454  0.750445 -0.455947  
```  
  
在生成借助于date_range()函数的DatetimeIndex对象时，频率参数freq有多种选择。表5-3列出了所有选项。  
  
<img width="473" height="615" alt="python_for_finance 5 3 1" src="https://github.com/user-attachments/assets/5ef1a2c7-e8e5-41f4-a914-bfc45e0b9561" />
  
在某些情况下，以ndarray对象的形式访问原始数据集是值得的。values属性提供了直接访问它的方式：  
  
```python  
df.values  
```  
  
```txt  
Out[38]: array([[-1.74976547,  0.3426804 ,  1.1530358 , -0.25243604],  
                [ 0.98132079,  0.51421884,  0.22117967, -1.07004333],  
                [-0.18949583,  0.25500144, -0.45802699,  0.43516349],  
                [-0.58359505,  0.81684707,  0.67272081, -0.10441114],  
                [-0.53128038,  1.02973269, -0.43813562, -1.11831825],  
                [ 1.61898166,  1.54160517, -0.25187914, -0.84243574],  
                [ 0.18451869,  0.9370822 ,  0.73100034,  1.36155613],  
                [-0.32623806,  0.05567601,  0.22239961, -1.443217  ],  
                [-0.75635231,  0.81645401,  0.75044476, -0.45594693]])  
```  
  
```python  
np.array(df)  
```  
  
```txt  
Out[39]: array([[-1.74976547,  0.3426804 ,  1.1530358 , -0.25243604],  
                [ 0.98132079,  0.51421884,  0.22117967, -1.07004333],  
                [-0.18949583,  0.25500144, -0.45802699,  0.43516349],  
                [-0.58359505,  0.81684707,  0.67272081, -0.10441114],  
                [-0.53128038,  1.02973269, -0.43813562, -1.11831825],  
                [ 1.61898166,  1.54160517, -0.25187914, -0.84243574],  
                [ 0.18451869,  0.9370822 ,  0.73100034,  1.36155613],  
                [-0.32623806,  0.05567601,  0.22239961, -1.443217  ],  
                [-0.75635231,  0.81645401,  0.75044476, -0.45594693]])  
```  
  
**数组与DataFrame**  
可以从ndarray对象生成DataFrame对象，但也可以通过使用DataFrame类的values属性或NumPy的np.array()函数轻松地从DataFrame对象生成ndarray对象。  
  
## 基本分析  
  
就像NumPy的ndarray类一样，pandas的DataFrame类内置了大量方便的方法。作为入门，请考虑info()和describe()方法：  
  
```python  
df.info() # ❶ 提供关于数据、列和索引的元信息。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
DatetimeIndex: 9 entries, 2019-01-31 to 2019-09-30  
Freq: M  
Data columns (total 4 columns):  
No1    9 non-null float64  
No2    9 non-null float64  
No3    9 non-null float64  
No4    9 non-null float64  
dtypes: float64(4)  
memory usage: 360.0 bytes  
```  
  
```python  
df.describe() # ❷ 提供每列有用的摘要统计信息（针对数值数据）。  
```  
  
```txt  
Out[41]:             No1       No2       No3       No4  
       count   9.000000  9.000000  9.000000  9.000000  
       mean   -0.150212  0.701033  0.289193 -0.387788  
       std     0.988306  0.457685  0.579920  0.877532  
       min    -1.749765  0.055676 -0.458027 -1.443217  
       25%    -0.583595  0.342680 -0.251879 -1.070043  
       50%    -0.326238  0.816454  0.222400 -0.455947  
       75%     0.184519  0.937082  0.731000 -0.104411  
       max     1.618982  1.541605  1.153036  1.361556  
```  
  
此外，人们还可以轻松获取按列或按行的总和、平均值和累积和：  
  
```python  
df.sum() # ❶ 按列求和。  
```  
  
```txt  
Out[43]: No1   -1.351906  
         No2    6.309298  
         No3    2.602739  
         No4   -3.490089  
dtype: float64  
```  
  
```python  
df.mean() # ❷ 按列求平均值。  
```  
  
```txt  
Out[44]: No1   -0.150212  
         No2    0.701033  
         No3    0.289193  
         No4   -0.387788  
dtype: float64  
```  
  
```python  
df.mean(axis=0) # ❷ 按列求平均值。  
```  
  
```txt  
Out[45]: No1   -0.150212  
         No2    0.701033  
         No3    0.289193  
         No4   -0.387788  
dtype: float64  
```  
  
```python  
df.mean(axis=1) # ❸ 按行求平均值。  
```  
  
```txt  
Out[46]: 2019-01-31   -0.126621  
         2019-02-28    0.161669  
         2019-03-31    0.010661  
         2019-04-30    0.200390  
         2019-05-31   -0.264500  
         2019-06-30    0.516568  
         2019-07-31    0.803539  
         2019-08-31   -0.372845  
         2019-09-30    0.088650  
Freq: M, dtype: float64  
```  
  
```python  
df.cumsum() # ❹ 按列计算累积和（从第一个索引位置开始）。  
```  
  
```txt  
Out[47]:                  No1       No2       No3       No4  
       2019-01-31 -1.749765  0.342680  1.153036 -0.252436  
       2019-02-28 -0.768445  0.856899  1.374215 -1.322479  
       2019-03-31 -0.957941  1.111901  0.916188 -0.887316  
       2019-04-30 -1.541536  1.928748  1.588909 -0.991727  
       2019-05-31 -2.072816  2.958480  1.150774 -2.110045  
       2019-06-30 -0.453834  4.500086  0.898895 -2.952481  
       2019-07-31 -0.269316  5.437168  1.629895 -1.590925  
       2019-08-31 -0.595554  5.492844  1.852294 -3.034142  
       2019-09-30 -1.351906  6.309298  2.602739 -3.490089  
```  
  
DataFrame对象也如预期那样能够理解NumPy的通用函数：  
  
```python  
np.mean(df) # ❶ 按列求平均值。  
```  
  
```txt  
Out[48]: No1   -0.150212  
         No2    0.701033  
         No3    0.289193  
         No4   -0.387788  
dtype: float64  
```  
  
```python  
np.log(df) # ❷ 逐元素计算自然对数；会引发警告，但计算会继续运行，导致出现多个NaN值。  
```  
  
```txt  
Out[49]:                  No1       No2       No3       No4  
       2019-01-31       NaN -1.070957  0.142398       NaN  
       2019-02-28 -0.018856 -0.665106 -1.508780       NaN  
       2019-03-31       NaN -1.366486       NaN -0.832033  
       2019-04-30       NaN -0.202303 -0.396425       NaN  
       2019-05-31       NaN  0.029299       NaN       NaN  
       2019-06-30  0.481797  0.432824       NaN       NaN  
       2019-07-31 -1.690005 -0.064984 -0.313341  0.308628  
       2019-08-31       NaN -2.888206 -1.503279       NaN  
       2019-09-30       NaN -0.202785 -0.287089       NaN  
```  
  
```python  
np.sqrt(abs(df)) # ❸ 逐元素计算绝对值的平方根……  
```  
  
```txt  
Out[50]:                  No1       No2       No3       No4  
       2019-01-31  1.322787  0.585389  1.073795  0.502430  
       2019-02-28  0.990616  0.717091  0.470297  1.034429  
       2019-03-31  0.435311  0.504977  0.676777  0.659669  
       2019-04-30  0.763934  0.903796  0.820196  0.323127  
       2019-05-31  0.728890  1.014757  0.661918  1.057506  
       2019-06-30  1.272392  1.241614  0.501876  0.917843  
       2019-07-31  0.429556  0.968030  0.854986  1.166857  
       2019-08-31  0.571173  0.235958  0.471593  1.201340  
       2019-09-30  0.869685  0.903578  0.866282  0.675238  
```  
  
```python  
np.sqrt(abs(df)).sum() # ❹ ……以及结果的按列平均值。  
```  
  
```txt  
Out[51]: No1    7.384345  
         No2    7.075190  
         No3    6.397719  
         No4    7.538440  
dtype: float64  
```  
  
```python  
100 * df + 100 # ❺ 数值数据的线性变换。  
```  
  
```txt  
Out[52]:                  No1         No2         No3         No4  
       2019-01-31  -74.976547  134.268040  215.303580   74.756396  
       2019-02-28  198.132079  151.421884  122.117967   -7.004333  
       2019-03-31   81.050417  125.500144   54.197301  143.516349  
       2019-04-30   41.640495  181.684707  167.272081   89.558886  
       2019-05-31   46.871962  202.973269   56.186438  -11.831825  
       2019-06-30  261.898166  254.160517   74.812086   15.756426  
       2019-07-31  118.451869  193.708220  173.100034  236.155613  
       2019-08-31   67.376194  105.567601  122.239961  -44.321700  
       2019-09-30   24.364769  181.645401  175.044476   54.405307  
```  
  
**NumPy通用函数**  
通常，当NumPy通用函数可以应用于包含相同类型数据的ndarray对象时，就可以将它们应用于pandas的DataFrame对象。  
  
pandas相当具有容错性，从某种意义上说，它会捕获错误，并仅在相应的数学运算失败的地方放置一个NaN值。不仅如此，正如前面简要展示的那样，在许多情况下，人们也可以像处理完整数据集一样处理这些不完整的数据集。这很方便，因为现实情况往往以不完整的数据集为特征，其频率超过了人们的期望。  
  
## 基本可视化  
  
一般来说，一旦数据存储在DataFrame对象中，距离绘制数据就只差一行代码了（见图5-1）：  
  
```python  
from pylab import plt, mpl # ❶ 自定义绘图样式。  
plt.style.use('seaborn')   
mpl.rcParams['font.family'] = 'serif'   
%matplotlib inline  
```  
  
```python  
df.cumsum().plot(lw=2.0, figsize=(10, 6)); # ❷ 将四列的累积和绘制为折线图。  
```  
  
基本上，pandas提供了一个围绕matplotplib（见第7章）的包装器，专为DataFrame对象设计。表5-4列出了plot()方法接受的参数。  
  
<img width="667" height="742" alt="python_for_finance 5 4 1" src="https://github.com/user-attachments/assets/33e03b9a-71ec-4fec-b0d6-0f9197b93d13" />
  
图5-1. DataFrame对象的折线图  
  
<img width="665" height="439" alt="python_for_finance 5 1" src="https://github.com/user-attachments/assets/d0dfc34d-451c-4b53-8cb8-59e5e536880f" />
  
作为另一个例子，考虑相同数据的条形图（见图5-2）：  
  
```python  
df.plot.bar(figsize=(10, 6), rot=15); # ❶ 通过.plot.bar()绘制柱状图。  
# df.plot(kind='bar', figsize=(10, 6)) # ❷ 替代语法：使用kind参数更改绘图类型。  
```  
  
<img width="666" height="437" alt="python_for_finance 5 2" src="https://github.com/user-attachments/assets/6729d629-ed50-405f-8b5e-0fbef5af80e8" />
  
图5-2. DataFrame对象的条形图  
  
## Series类  
  
到目前为止，本章主要使用了pandas的DataFrame类。Series是pandas附带的另一个重要类。它的特征在于它只包含一列数据。在这个意义上，它是DataFrame类的特化，它们共享了许多（但不是全部）特征和功能。当从多列DataFrame对象中选择单个列时，就会获得一个Series对象：  
  
```python  
type(df)  
```  
  
```txt  
Out[56]: pandas.core.frame.DataFrame  
```  
  
```python  
S = pd.Series(np.linspace(0, 15, 7), name='series')  
  
S  
```  
  
```txt  
Out[58]: 0     0.0  
         1     2.5  
         2     5.0  
         3     7.5  
         4    10.0  
         5    12.5  
         6    15.0  
Name: series, dtype: float64  
```  
  
```python  
type(S)  
```  
  
```txt  
Out[59]: pandas.core.series.Series  
```  
  
```python  
s = df['No1']  
  
s  
```  
  
```txt  
Out[61]: 2019-01-31   -1.749765  
         2019-02-28    0.981321  
         2019-03-31   -0.189496  
         2019-04-30   -0.583595  
         2019-05-31   -0.531280  
         2019-06-30    1.618982  
         2019-07-31    0.184519  
         2019-08-31   -0.326238  
         2019-09-30   -0.756352  
Freq: M, Name: No1, dtype: float64  
```  
  
```python  
type(s)  
```  
  
```txt  
Out[62]: pandas.core.series.Series  
```  
  
主要的DataFrame方法也适用于Series对象。为了说明，请考虑mean()和plot()方法（见图5-3）：  
  
```python  
s.mean()  
```  
  
```txt  
Out[63]: -0.15021177307319458  
```  
  
```python  
s.plot(lw=2.0, figsize=(10, 6));  
```  
  
<img width="668" height="432" alt="python_for_finance 5 3" src="https://github.com/user-attachments/assets/4488d56d-49d9-402e-b533-2df637247d29" />
  
图5-3. Series对象的折线图  
  
## GroupBy操作  
  
pandas具有强大且灵活的分组功能。它们的工作方式类似于SQL中的分组以及Microsoft Excel中的数据透视表。为了有一个可以分组的依据，人们可以例如添加一列，以指示索引的各个数据属于哪个季度：  
  
```python  
df['Quarter'] = ['Q1', 'Q1', 'Q1', 'Q2', 'Q2',  
                 'Q2', 'Q3', 'Q3', 'Q3']  
  
df  
```  
  
```txt  
Out[65]:                  No1       No2       No3       No4 Quarter  
       2019-01-31 -1.749765  0.342680  1.153036 -0.252436      Q1  
       2019-02-28  0.981321  0.514219  0.221180 -1.070043      Q1  
       2019-03-31 -0.189496  0.255001 -0.458027  0.435163      Q1  
       2019-04-30 -0.583595  0.816847  0.672721 -0.104411      Q2  
       2019-05-31 -0.531280  1.029733 -0.438136 -1.118318      Q2  
       2019-06-30  1.618982  1.541605 -0.251879 -0.842436      Q2  
       2019-07-31  0.184519  0.937082  0.731000  1.361556      Q3  
       2019-08-31 -0.326238  0.055676  0.222400 -1.443217      Q3  
       2019-09-30 -0.756352  0.816454  0.750445 -0.455947      Q3  
```  
  
以下代码按Quarter列进行分组，并输出单个组的统计信息：  
  
```python  
groups = df.groupby('Quarter') # ❶ 根据Quarter列进行分组。  
```  
  
```python  
groups.size() # ❷ 给出每个组的行数。  
```  
  
```txt  
Out[67]: Quarter  
         Q1    3  
         Q2    3  
         Q3    3  
dtype: int64  
```  
  
```python  
groups.mean() # ❸ 给出每列的平均值。  
```  
  
```txt  
Out[68]:               No1       No2       No3       No4  
         Quarter  
         Q1      -0.319314  0.370634  0.305396 -0.295772  
         Q2       0.168035  1.129395 -0.005765 -0.688388  
         Q3      -0.299357  0.603071  0.567948 -0.179203  
```  
  
```python  
groups.max() # ❹ 给出每列的最大值。  
```  
  
```txt  
Out[69]:               No1       No2       No3       No4  
         Quarter  
         Q1       0.981321  0.514219  1.153036  0.435163  
         Q2       1.618982  1.541605  0.672721 -0.104411  
         Q3       0.184519  0.937082  0.750445  1.361556  
```  
  
```python  
groups.aggregate([min, max]).round(2) # ❺ 给出每列的最小值和最大值。  
```  
  
```txt  
Out[70]:           No1          No2          No3          No4  
                 min   max    min   max    min   max    min   max  
         Quarter  
         Q1    -1.75  0.98   0.26  0.51  -0.46  1.15  -1.07  0.44  
         Q2    -0.58  1.62   0.82  1.54  -0.44  0.67  -1.12 -0.10  
         Q3    -0.76  0.18   0.06  0.94   0.22  0.75  -1.44  1.36  
```  
  
也可以使用多个列进行分组。为此，引入了另一列，以指示索引日期的月份是奇数还是偶数：  
  
```python  
df['Odd_Even'] = ['Odd', 'Even', 'Odd', 'Even', 'Odd', 'Even',  
                  'Odd', 'Even', 'Odd']  
  
groups = df.groupby(['Quarter', 'Odd_Even'])  
  
groups.size()  
```  
  
```txt  
Out[73]: Quarter  Odd_Even  
         Q1       Even        1  
                  Odd         2  
         Q2       Even        2  
                  Odd         1  
         Q3       Even        1  
                  Odd         2  
dtype: int64  
```  
  
```python  
groups[['No1', 'No4']].aggregate([sum, np.mean])  
```  
  
```txt  
Out[74]:                           No1                  No4  
                           sum      mean        sum      mean  
         Quarter Odd_Even  
         Q1      Even     0.981321  0.981321  -1.070043 -1.070043  
                 Odd     -1.939261 -0.969631   0.182727  0.091364  
         Q2      Even     1.035387  0.517693  -0.946847 -0.473423  
                 Odd     -0.531280 -0.531280  -1.118318 -1.118318  
         Q3      Even    -0.326238 -0.326238  -1.443217 -1.443217  
                 Odd     -0.571834 -0.285917   0.905609  0.452805  
```  
  
## 复杂选择  
  
通常，数据选择是通过对列值制定条件来实现的，并且可能在逻辑上组合多个此类条件。考虑以下数据集：  
  
```python  
data = np.random.standard_normal((10, 2)) # ❶ 包含标准正态分布随机数的ndarray对象。  
  
df = pd.DataFrame(data, columns=['x', 'y']) # ❷ 包含相同随机数的DataFrame对象。  
  
df.info()   
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 10 entries, 0 to 9  
Data columns (total 2 columns):  
x       10 non-null float64  
y       10 non-null float64  
dtypes: float64(2)  
memory usage: 240.0 bytes  
```  
  
```python  
df.head() # ❸ 通过head()方法获取前五行。  
```  
  
```txt  
Out[78]:           x         y  
       0  1.189622 -1.690617  
       1 -1.356399 -1.232435  
       2 -0.544439 -0.668172  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
```  
  
```python  
df.tail() # ❹ 通过tail()方法获取最后五行。  
```  
  
```txt  
Out[79]:           x         y  
       5 -0.983310  0.357508  
       6 -1.613579  1.470714  
       7 -1.188018 -0.549746  
       8 -0.940046 -0.827932  
       9  0.108863  0.507810  
```  
  
以下代码说明了Python的比较运算符和逻辑运算符在两列值上的应用：  
  
```python  
df['x'] > 0.5 # ❶ 检查列x中的值是否大于0.5。  
```  
  
```txt  
Out[80]: 0     True  
         1    False  
         2    False  
         3    False  
         4     True  
         5    False  
         6    False  
         7    False  
         8    False  
         9    False  
Name: x, dtype: bool  
```  
  
```python  
(df['x'] > 0) & (df['y'] < 0) # ❷ 检查列x中的值是否为正数，并且列y中的值是否为负数。  
```  
  
```txt  
Out[81]: 0     True  
         1    False  
         2    False  
         3     True  
         4     True  
         5    False  
         6    False  
         7    False  
         8    False  
         9    False  
dtype: bool  
```  
  
```python  
(df['x'] > 0) | (df['y'] < 0) # ❸ 检查列x中的值是否为正数，或者列y中的值是否为负数。  
```  
  
```txt  
Out[82]: 0     True  
         1     True  
         2     True  
         3     True  
         4     True  
         5    False  
         6    False  
         7     True  
         8     True  
         9     True  
dtype: bool  
```  
  
使用得到的布尔Series对象，复杂数据（行）选择变得非常简单。或者，人们可以使用query()方法，并将条件作为str对象传递：  
  
```python  
df[df['x'] > 0] # ❶ 列x中的值大于0的所有行。  
```  
  
```txt  
Out[83]:           x         y  
       0  1.189622 -1.690617  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
       9  0.108863  0.507810  
```  
  
```python  
df.query('x > 0') # ❶ 列x中的值大于0的所有行。  
```  
  
```txt  
Out[84]:           x         y  
       0  1.189622 -1.690617  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
       9  0.108863  0.507810  
```  
  
```python  
df[(df['x'] > 0) & (df['y'] < 0)] # ❷ 列x中的值为正数且列y中的值为负数的所有行。  
```  
  
```txt  
Out[85]:           x         y  
       0  1.189622 -1.690617  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
```  
  
```python  
df.query('x > 0 & y < 0') # ❷ 列x中的值为正数且列y中的值为负数的所有行。  
```  
  
```txt  
Out[86]:           x         y  
       0  1.189622 -1.690617  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
```  
  
```python  
df[(df.x > 0) | (df.y < 0)] # ❸ 列x中的值为正数或列y中的值为负数的所有行（此处通过各自的属性访问列）。  
```  
  
```txt  
Out[87]:           x         y  
       0  1.189622 -1.690617  
       1 -1.356399 -1.232435  
       2 -0.544439 -0.668172  
       3  0.007315 -0.612939  
       4  1.299748 -1.733096  
       7 -1.188018 -0.549746  
       8 -0.940046 -0.827932  
       9  0.108863  0.507810  
```  
  
比较运算符也可以一次性应用于完整的DataFrame对象：  
  
```python  
df > 0 # ❶ DataFrame对象中有哪些值为正数？  
```  
  
```txt  
Out[88]:        x      y  
       0   True  False  
       1  False  False  
       2  False  False  
       3   True  False  
       4   True  False  
       5  False   True  
       6  False   True  
       7  False  False  
       8  False  False  
       9   True   True  
```  
  
```python  
df[df > 0] # ❷ 选择所有此类值，并在所有其他位置放置NaN。  
```  
  
```txt  
Out[89]:           x         y  
       0  1.189622       NaN  
       1       NaN       NaN  
       2       NaN       NaN  
       3  0.007315       NaN  
       4  1.299748       NaN  
       5       NaN  0.357508  
       6       NaN  1.470714  
       7       NaN       NaN  
       8       NaN       NaN  
       9  0.108863  0.507810  
```  
  
## 拼接、连接和合并  
  
本节逐步介绍了通过不同方法将两个简单的数据集以DataFrame对象的形式组合在一起。这两个简单的数据集是：  
  
```python  
df1 = pd.DataFrame(['100', '200', '300', '400'],  
                   index=['a', 'b', 'c', 'd'],  
                   columns=['A',])  
  
df1  
```  
  
```txt  
Out[91]:      A  
       a   100  
       b   200  
       c   300  
       d   400  
```  
  
```python  
df2 = pd.DataFrame(['200', '150', '50'],  
                   index=['f', 'b', 'd'],  
                   columns=['B',])  
  
df2  
```  
  
```txt  
Out[93]:      B  
       f   200  
       b   150  
       d    50  
```  
  
### 拼接  
  
拼接或追加基本上意味着将行从一个DataFrame对象添加到另一个对象中。这可以通过append()方法或pd.concat()函数来实现。一个主要的考虑因素是如何处理索引值：  
  
```python  
df1.append(df2, sort=False) # ❶ 将数据从df2作为新行追加到df1。  
```  
  
```txt  
Out[94]:      A    B  
       a   100  NaN  
       b   200  NaN  
       c   300  NaN  
       d   400  NaN  
       f   NaN  200  
       b   NaN  150  
       d   NaN   50  
```  
  
```python  
df1.append(df2, ignore_index=True, sort=False) # ❷ 执行相同的操作，但忽略索引。  
```  
  
```txt  
Out[95]:      A    B  
       0   100  NaN  
       1   200  NaN  
       2   300  NaN  
       3   400  NaN  
       4   NaN  200  
       5   NaN  150  
       6   NaN   50  
```  
  
```python  
pd.concat((df1, df2), sort=False) # ❸ 与第一次append操作的效果相同。  
```  
  
```txt  
Out[96]:      A    B  
       a   100  NaN  
       b   200  NaN  
       c   300  NaN  
       d   400  NaN  
       f   NaN  200  
       b   NaN  150  
       d   NaN   50  
```  
  
```python  
pd.concat((df1, df2), ignore_index=True, sort=False) # ❹ 与第二次append操作的效果相同。  
```  
  
```txt  
Out[97]:      A    B  
       0   100  NaN  
       1   200  NaN  
       2   300  NaN  
       3   400  NaN  
       4   NaN  200  
       5   NaN  150  
       6   NaN   50  
```  
  
### 连接  
  
在连接两个数据集时，DataFrame对象的顺序也很重要，但方式不同。只使用了第一个DataFrame对象的索引值。这种默认行为被称为左连接：  
  
```python  
df1.join(df2) # ❶ df1的索引值起作用。  
```  
  
```txt  
Out[98]:      A    B  
       a   100  NaN  
       b   200  150  
       c   300  NaN  
       d   400   50  
```  
  
```python  
df2.join(df1) # ❷ df2的索引值起作用。  
```  
  
```txt  
Out[99]:      B    A  
       f   200  NaN  
       b   150  200  
       d    50  400  
```  
  
总共有四种不同的连接方法可用，每种方法在处理索引值和相应数据行的方式上都会导致不同的行为：  
  
```python  
df1.join(df2, how='left') # ❶ 左连接是默认操作。  
```  
  
```txt  
Out[100]:      A    B  
        a   100  NaN  
        b   200  150  
        c   300  NaN  
        d   400   50  
```  
  
```python  
df1.join(df2, how='right') # ❷ 右连接与反转DataFrame对象的顺序相同。  
```  
  
```txt  
Out[101]:      A    B  
        f   NaN  200  
        b   200  150  
        d   400   50  
```  
  
```python  
df1.join(df2, how='inner') # ❸ 内连接仅保留在两个索引中都找到的索引值。  
```  
  
```txt  
Out[102]:      A    B  
        b   200  150  
        d   400   50  
```  
  
```python  
df1.join(df2, how='outer') # ❹ 外连接保留两个索引中的所有索引值。  
```  
  
```txt  
Out[103]:      A    B  
        a   100  NaN  
        b   200  150  
        c   300  NaN  
        d   400   50  
        f   NaN  200  
```  
  
连接也可以基于一个空的DataFrame对象发生。在这种情况下，列是顺序创建的，导致行为类似于左连接：  
  
```python  
df = pd.DataFrame()  
  
df['A'] = df1['A'] # ❶ 将df1作为第一列A。  
  
df  
```  
  
```txt  
Out[106]:      A  
        a   100  
        b   200  
        c   300  
        d   400  
```  
  
```python  
df['B'] = df2 # ❷ 将df2作为第二列B。  
  
df  
```  
  
```txt  
Out[108]:      A    B  
        a   100  NaN  
        b   200  150  
        c   300  NaN  
        d   400   50  
```  
  
利用字典组合数据集会产生类似于外连接的结果，因为这些列是同时创建的：  
  
```python  
df = pd.DataFrame({'A': df1['A'], 'B': df2['B']}) # ❶ DataFrame对象的列用作dict对象中的值。  
  
df  
```  
  
```txt  
Out[110]:      A    B  
        a   100  NaN  
        b   200  150  
        c   300  NaN  
        d   400   50  
        f   NaN  200  
```  
  
### 合并  
  
虽然join操作基于要连接的DataFrame对象的索引发生，但merge操作通常发生在两个数据集共享的列上。为此，将新列C添加到两个原始DataFrame对象中：  
  
```python  
c = pd.Series([250, 150, 50], index=['b', 'd', 'c'])  
  
df1['C'] = c  
  
df2['C'] = c  
  
df1  
```  
  
```txt  
Out[112]:      A      C  
        a   100    NaN  
        b   200  250.0  
        c   300   50.0  
        d   400  150.0  
```  
  
```python  
df2  
```  
  
```txt  
Out[113]:      B      C  
        f   200    NaN  
        b   150  250.0  
        d    50  150.0  
```  
  
默认情况下，在这种情况下合并操作基于单个共享列C发生。然而，也可以使用其他选项，例如外合并：  
  
```python  
pd.merge(df1, df2) # ❶ 在列C上的默认合并。  
```  
  
```txt  
Out[114]:      A      C    B  
        0   100    NaN  200  
        1   200  250.0  150  
        2   400  150.0   50  
```  
  
```python  
pd.merge(df1, df2, on='C') # ❶ 在列C上的默认合并。  
```  
  
```txt  
Out[115]:      A      C    B  
        0   100    NaN  200  
        1   200  250.0  150  
        2   400  150.0   50  
```  
  
```python  
pd.merge(df1, df2, how='outer') # ❷ 外合并也是可行的，会保留所有数据行。  
```  
  
```txt  
Out[116]:      A      C    B  
        0   100    NaN  200  
        1   200  250.0  150  
        2   300   50.0  NaN  
        3   400  150.0   50  
```  
  
还有许多其他类型的合并操作可用，其中一些在以下代码中进行了说明：  
  
```python  
pd.merge(df1, df2, left_on='A', right_on='B')  
```  
  
```txt  
Out[117]:      A    C_x    B  C_y  
        0   200  250.0  200  NaN  
```  
  
```python  
pd.merge(df1, df2, left_on='A', right_on='B', how='outer')  
```  
  
```txt  
Out[118]:      A    C_x    B    C_y  
        0   100    NaN  NaN    NaN  
        1   200  250.0  200    NaN  
        2   300   50.0  NaN    NaN  
        3   400  150.0  NaN    NaN  
        4   NaN    NaN  150  250.0  
        5   NaN    NaN   50  150.0  
```  
  
```python  
pd.merge(df1, df2, left_index=True, right_index=True)  
```  
  
```txt  
Out[119]:      A    C_x    B    C_y  
        b   200  250.0  150  250.0  
        d   400  150.0   50  150.0  
```  
  
```python  
pd.merge(df1, df2, on='C', left_index=True)  
```  
  
```txt  
Out[120]:      A      C    B  
        f   100    NaN  200  
        b   200  250.0  150  
        d   400  150.0   50  
```  
  
```python  
pd.merge(df1, df2, on='C', right_index=True)  
```  
  
```txt  
Out[121]:      A      C    B  
        a   100    NaN  200  
        b   200  250.0  150  
        d   400  150.0   50  
```  
  
```python  
pd.merge(df1, df2, on='C', left_index=True, right_index=True)  
```  
  
```txt  
Out[122]:      A      C    B  
        b   200  250.0  150  
        d   400  150.0   50  
```  
  
## 性能方面  
  
本章中的许多示例表明，pandas经常有多个选项可以达到相同的目标。本节比较了逐元素相加两列的此类选项。首先是用NumPy生成的数据集：  
  
```python  
data = np.random.standard_normal((1000000, 2)) # ❶ 包含随机数的ndarray对象。  
  
data.nbytes # ❶ 包含随机数的ndarray对象。  
```  
  
```txt  
Out[124]: 16000000  
```  
  
```python  
df = pd.DataFrame(data, columns=['x', 'y']) # ❷ 包含随机数的DataFrame对象。  
  
df.info() # ❷ 包含随机数的DataFrame对象。  
```  
  
```txt  
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 1000000 entries, 0 to 999999  
Data columns (total 2 columns):  
x       1000000 non-null float64  
y       1000000 non-null float64  
dtypes: float64(2)  
memory usage: 15.3 MB  
```  
  
其次，手头有一些完成该任务且包含性能值的选项：  
  
```python  
%time res = df['x'] + df['y'] # ❶ 直接使用列（Series对象）是最快的方法。  
```  
  
```txt  
CPU times: user 7.35 ms, sys: 7.43 ms, total: 14.8 ms  
Wall time: 7.48 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[128]: 0    0.387242  
          1   -0.969343  
          2   -0.863159  
dtype: float64  
```  
  
```python  
%time res = df.sum(axis=1) # ❷ 这通过调用DataFrame对象上的sum()方法来计算总和。  
```  
  
```txt  
CPU times: user 130 ms, sys: 30.6 ms, total: 161 ms  
Wall time: 101 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[130]: 0    0.387242  
          1   -0.969343  
          2   -0.863159  
dtype: float64  
```  
  
```python  
%time res = df.values.sum(axis=1) # ❸ 这通过调用ndarray对象上的sum()方法来计算总和。  
```  
  
```txt  
CPU times: user 50.3 ms, sys: 2.75 ms, total: 53.1 ms  
Wall time: 27.9 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[132]: array([ 0.3872424 , -0.96934273, -0.86315944])  
```  
  
```python  
%time res = np.sum(df, axis=1) # ❹ 这通过在DataFrame对象上使用函数np.sum()来计算总和。  
```  
  
```txt  
CPU times: user 127 ms, sys: 15.1 ms, total: 142 ms  
Wall time: 73.7 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[134]: 0    0.387242  
          1   -0.969343  
          2   -0.863159  
dtype: float64  
```  
  
```python  
%time res = np.sum(df.values, axis=1) # ❺ 这通过在ndarray对象上使用函数np.sum()来计算总和。  
```  
  
```txt  
CPU times: user 49.3 ms, sys: 2.36 ms, total: 51.7 ms  
Wall time: 26.9 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[136]: array([ 0.3872424 , -0.96934273, -0.86315944])  
```  
  
最后，还有两个基于eval()和apply()方法的选项，分别如下：1 应用程序的eval()方法需要安装numexpr包。  
  
```python  
%time res = df.eval('x + y') # ❶ eval()是专门用于计算（复杂的）数值表达式的方法；可以直接定位列。  
```  
  
```txt  
CPU times: user 25.5 ms, sys: 17.7 ms, total: 43.2 ms  
Wall time: 22.5 ms  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[138]: 0    0.387242  
          1   -0.969343  
          2   -0.863159  
dtype: float64  
```  
  
```python  
%time res = df.apply(lambda row: row['x'] + row['y'], axis=1) # ❷ 最慢的选项是逐行使用apply()方法；这就像在Python层面上循环遍历所有行。  
```  
  
```txt  
CPU times: user 19.6 s, sys: 83.3 ms, total: 19.7 s  
Wall time: 19.9 s  
```  
  
```python  
res[:3]  
```  
  
```txt  
Out[140]: 0    0.387242  
          1   -0.969343  
          2   -0.863159  
dtype: float64  
```  
  
**明智选择**  
pandas通常提供多种选项来实现相同的目标。如果不确定使用哪一种，请在时间紧迫时比较这些选项，以验证是否达到了最佳可能的性能。在这个简单的例子中，执行时间的差异达几个数量级。  
  
## 结论  
  
pandas是用于数据分析的强大工具，并已成为所谓的PyData栈中的核心包。它的DataFrame类特别适合处理各种类型的表格数据。对此类对象的大多数操作都是向量化的，这不仅导致了如NumPy中那样简洁的代码，而且在总体上带来了高性能。此外，pandas使得处理不完整的数据集变得方便（例如，NumPy就不是这样）。pandas和DataFrame类将在本书后面许多章节中占据核心地位，在必要时将使用并引入附加功能。  
  
## 延伸阅读  
  
pandas是一个开源项目，可提供在线文档和供下载的PDF版本。2 网站提供了两者的链接以及其他资源：  
  
- http://pandas.pydata.org/  
  
2 在撰写本文时，PDF版本的总页数超过2500页。  
  
至于NumPy，有关pandas的书籍形式推荐参考资料有：  
  
- McKinney, Wes (2017). Python for Data Analysis. Sebastopol, CA: O’Reilly.  
- VanderPlas, Jake (2016). Python Data Science Handbook. Sebastopol, CA: O’Reilly.  
