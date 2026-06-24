# 第9章 输入/输出操作  
  
> 在拥有数据之前进行理论化是一个致命的错误。  
> ——夏洛克·福尔摩斯  
  
作为一般规则，大多数数据，无论是在金融领域还是任何其他应用领域，都存储在硬盘驱动器（HDD）或某种其他形式的永久存储设备上，例如固态硬盘（SSD）或混合磁盘驱动器。多年来，存储容量一直在稳步增加，而每个存储单元的成本（例如，每兆字节）却在稳步下降。  
  
与此同时，存储数据量的增长速度远远快于即使是最大型机器上可用的典型随机存取存储器（RAM）。这不仅需要将数据存储到磁盘以进行永久存储，还需要通过在 RAM 和磁盘之间来回交换数据来弥补 RAM 不足的情况。  
  
因此，在涉及金融应用和一般的数据密集型应用时，输入/输出（I/O）操作是重要的任务。通常，它们代表了性能关键型计算的瓶颈，因为 I/O 操作通常无法足够快地将数据传输到 RAM（这里，不区分不同级别的 RAM 和处理器缓存。当前内存架构的最佳使用本身就是一个独立的话题。）和从 RAM 传输到磁盘。从某种意义上说，由于缓慢的 I/O 操作，CPU 经常处于“饥饿”状态。  
  
尽管当今大多数的金融和企业分析工作都面临着大数据（例如，PB 级），但单个分析任务通常使用属于“中等”数据类别的数据子集。微软研究院的一项研究得出结论：  
  
> 我们的测量以及最近的其他工作表明，大多数真实的分析任务处理的输入少于 100 GB，但诸如 Hadoop/MapReduce 等流行的基础架构最初是为处理 PB 级数据而设计的。  
> ——Appuswamy 等人（2013）  
  
就频率而言，单个金融分析任务通常处理大小不超过几千兆字节（GB）的数据——这正是 Python 及其科学技术栈的库（例如 NumPy、pandas 和 PyTables）的绝佳用武之地。这样规模的数据集也可以在内存中进行分析，利用当今的 CPU 和 GPU 通常能获得很高的速度。然而，必须将数据读入 RAM，并且将结果写入磁盘，同时还要确保满足当今的性能要求。  
  
本章讨论以下主题：  
  
第 232 页的“使用 Python 进行基本 I/O”  
Python 具有内置函数来序列化对象并将其存储在磁盘上，以及将其从磁盘读取到 RAM 中；除此之外，Python 在处理文本文件和 SQL 数据库时也很强大。NumPy 还提供了专用函数，用于快速的二进制存储和检索 `ndarray` 对象。  
  
第 244 页的“使用 pandas 进行 I/O”  
pandas 库提供了大量便捷的函数和方法，用于读取以不同格式（例如，CSV、JSON）存储的数据，并将数据以各种格式写入文件。  
  
第 252 页的“使用 PyTables 进行 I/O”  
PyTables 使用具有层次型数据库结构和二进制存储的 HDF5 标准来完成大型数据集的快速 I/O 操作；速度通常仅受所用硬件的限制。  
  
第 267 页的“使用 TsTables 进行 I/O”  
TsTables 是一个构建在 PyTables 之上的包，允许快速存储和检索时间序列数据。  
  
## 使用 Python 进行基本 I/O  
  
Python 本身带有很多 I/O 功能，有些针对性能进行了优化，有些则更注重灵活性。然而，一般来说，它们无论在交互式还是生产环境中都易于使用。  
  
### 将对象写入磁盘  
  
为了后续使用、记录文档或与他人共享，我们可能希望将 Python 对象存储在磁盘上。一种选择是使用 `pickle` 模块。该模块可以序列化大多数的 Python 对象。序列化是指将对象（层次结构）转换为字节流；反序列化则是相反的操作。  
  
像往常一样，首先是一些关于绘图的导入和自定义配置：  
  
```python  
In [1]: from pylab import plt, mpl  
        plt.style.use('seaborn')  
        mpl.rcParams['font.family'] = 'serif'  
        %matplotlib inline  
```  
  
接下来的例子使用的是（伪）随机数据，这次存储在一个 `list` 对象中：  
  
```python  
In [2]: import pickle # ❶ 从标准库导入 pickle 模块。  
        import numpy as np  
        from random import gauss # ❷ 导入 gauss 以生成正态分布的随机数。  
  
In [3]: a = [gauss(1.5, 2) for i in range(1000000)] # ❸ 创建一个具有随机数的较大 list 对象。  
  
In [4]: path = '/Users/yves/Temp/data/' # ❹ 指定存储数据文件的路径。  
  
In [5]: pkl_file = open(path + 'data.pkl', 'wb') # ❺ 以二进制模式 (wb) 打开一个文件进行写入。  
```  
  
用于序列化和反序列化 Python 对象的两个主要函数是 `pickle.dump()`（用于写入对象）和 `pickle.load()`（用于将它们加载到内存中）：  
  
```python  
In [6]: %time pickle.dump(a, pkl_file) # ❶ 序列化对象 a 并将其保存到文件中。  
```  
  
```txt  
 CPU times: user 37.2 ms, sys: 15.3 ms, total: 52.5 ms  
 Wall time: 50.8 ms  
```  
  
```python  
In [7]: pkl_file.close() # ❷ 关闭文件。  
  
In [8]: ll $path* # ❸ 显示磁盘上的文件及其大小 (Mac/Linux)。  
```  
  
```txt  
 -rw-r--r-- 1 yves staff 9002006 Oct 19 12:11  
 /Users/yves/Temp/data/data.pkl  
```  
  
```python  
In [9]: pkl_file = open(path + 'data.pkl', 'rb') # ❹ 以二进制模式 (rb) 打开文件以进行读取。  
  
In [10]: %time b = pickle.load(pkl_file) # ❺ 从磁盘读取对象并对其进行反序列化。  
```  
  
```txt  
 CPU times: user 34.1 ms, sys: 16.7 ms, total: 50.8 ms  
 Wall time: 48.7 ms  
```  
  
```python  
In [11]: a[:3]  
```  
  
```txt  
Out[11]: [6.517874180585469, -0.5552400459507827, 2.8488946310833096]  
```  
  
```python  
In [12]: b[:3]  
```  
  
```txt  
Out[12]: [6.517874180585469, -0.5552400459507827, 2.8488946310833096]  
```  
  
```python  
In [13]: np.allclose(np.array(a), np.array(b)) # ❻ 将 a 和 b 转换为 ndarray 对象，np.allclose() 验证两者是否包含相同的数据（数字）。  
```  
  
```txt  
Out[13]: True  
```  
  
使用 `pickle` 存储和检索单个对象显然非常简单。那么两个对象呢？  
  
```python  
In [14]: pkl_file = open(path + 'data.pkl', 'wb')  
  
In [15]: %time pickle.dump(np.array(a), pkl_file) # ❶ 序列化 a 的 ndarray 版本并保存它。  
```  
  
```txt  
 CPU times: user 58.1 ms, sys: 6.09 ms, total: 64.2 ms  
 Wall time: 32.5 ms  
```  
  
```python  
In [16]: %time pickle.dump(np.array(a) ** 2, pkl_file) # ❷ 序列化 a 的平方的 ndarray 版本并保存它。  
```  
  
```txt  
 CPU times: user 66.7 ms, sys: 7.22 ms, total: 73.9 ms  
 Wall time: 39.3 ms  
```  
  
```python  
In [17]: pkl_file.close()  
  
In [18]: ll $path* # ❸ 现在的文件大小大约是以前的两倍。  
```  
  
```txt  
 -rw-r--r-- 1 yves staff 16000322 Oct 19 12:11  
 /Users/yves/Temp/data/data.pkl  
```  
  
那么将这两个 `ndarray` 对象读回内存又如何呢？  
  
```python  
In [19]: pkl_file = open(path + 'data.pkl', 'rb')  
  
In [20]: x = pickle.load(pkl_file) # ❶ 检索第一个存储的对象。  
         x[:4]  
```  
  
```txt  
Out[20]: array([ 6.51787418, -0.55524005, 2.84889463, 5.94489175])  
```  
  
```python  
In [21]: y = pickle.load(pkl_file) # ❷ 检索第二个存储的对象。  
         y[:4]  
```  
  
```txt  
Out[21]: array([42.48268383,  0.30829151,  8.11620062, 35.34173791])  
```  
  
```python  
In [22]: pkl_file.close()  
```  
  
显然，`pickle` 根据先进先出（FIFO）的原则存储对象。这有一个主要问题：用户没有可用的元信息来预先知道 `pickle` 文件中存储了什么。  
  
一个有时会有用的变通方法是不存储单个对象，而是存储一个包含所有其他对象的 `dict` 对象：  
  
```python  
In [23]: pkl_file = open(path + 'data.pkl', 'wb')  
         pickle.dump({'x': x, 'y': y}, pkl_file) # ❶ 存储一个包含两个 ndarray 对象的 dict 对象。  
         pkl_file.close()  
  
In [24]: pkl_file = open(path + 'data.pkl', 'rb')  
         data = pickle.load(pkl_file) # ❷ 检索 dict 对象。  
         pkl_file.close()  
         for key in data.keys():  
             print(key, data[key][:4])  
```  
  
```txt  
 x [ 6.51787418 -0.55524005  2.84889463  5.94489175]  
 y [42.48268383  0.30829151  8.11620062 35.34173791]  
```  
  
```python  
In [25]: !rm -f $path*  
```  
  
这种方法需要一次性写入和读取所有对象，但考虑到它带来的更高便利性，这可能是一个在许多情况下可以接受的折衷方案。  
  
> **兼容性问题**  
>  
> 使用 `pickle` 序列化对象通常非常简单。然而，当例如 Python 包升级后，新版本的包无法再处理由旧版本序列化得到的对象时，这可能会导致问题。在跨平台和操作系统共享此类对象时，也可能导致问题。因此，一般建议使用后续部分中讨论的诸如 NumPy 和 pandas 等包内置的读写功能。  
  
### 读写文本文件  
  
文本处理可以被视为 Python 的一个强项。事实上，许多企业和科学用户使用 Python 正是为了这个任务。通过 Python，我们有多种选项来处理 `str` 对象，以及处理一般的文本文件。  
  
假设我们需要将非常大的一组数据作为 CSV 文件共享。虽然这类文件具有特殊的内部结构，但它们基本上是纯文本文件。下面的代码创建了一个作为 `ndarray` 对象的虚拟数据集，创建了一个 `DatetimeIndex` 对象，将两者结合起来，并将数据存储为 CSV 文本文件：  
  
```python  
In [26]: import pandas as pd  
  
In [27]: rows = 5000 # ❶ 定义数据集的行数。  
         a = np.random.standard_normal((rows, 5)).round(4) # ❷ 使用随机数创建 ndarray 对象。  
  
In [28]: a  
```  
  
```txt  
Out[28]: array([[-0.0892, -1.0508, -0.5942,  0.3367,  1.508 ],  
       [ 2.1046,  3.2623,  0.704 , -0.2651,  0.4461],  
       [-0.0482, -0.9221,  0.1332,  0.1192,  0.7782],  
       ...,  
       [ 0.3026, -0.2005, -0.9947,  1.0203, -0.6578],  
       [-0.7031, -0.6989, -0.8031, -0.4271,  1.9963],  
       [ 2.4573,  2.2151,  0.158 , -0.7039, -1.0337]])  
```  
  
```python  
In [29]: t = pd.date_range(start='2019/1/1', periods=rows, freq='H') # ❸ 创建适当长度的 DatetimeIndex 对象（每小时间隔）。  
  
In [30]: t  
```  
  
```txt  
Out[30]: DatetimeIndex(['2019-01-01 00:00:00', '2019-01-01 01:00:00',  
               '2019-01-01 02:00:00', '2019-01-01 03:00:00',  
               '2019-01-01 04:00:00', '2019-01-01 05:00:00',  
               '2019-01-01 06:00:00', '2019-01-01 07:00:00',  
               '2019-01-01 08:00:00', '2019-01-01 09:00:00',  
               ...  
               '2019-07-27 22:00:00', '2019-07-27 23:00:00',  
               '2019-07-28 00:00:00', '2019-07-28 01:00:00',  
               '2019-07-28 02:00:00', '2019-07-28 03:00:00',  
               '2019-07-28 04:00:00', '2019-07-28 05:00:00',  
               '2019-07-28 06:00:00', '2019-07-28 07:00:00'],  
              dtype='datetime64[ns]', length=5000, freq='H')  
```  
  
```python  
In [31]: csv_file = open(path + 'data.csv', 'w') # ❹ 打开文件以便写入 (w)。  
  
In [32]: header = 'date,no1,no2,no3,no4,no5\n' # ❺ 定义标题行（列标签）并将其作为第一行写入。  
  
In [33]: csv_file.write(header) # ❺ 定义标题行（列标签）并将其作为第一行写入。  
```  
  
```txt  
Out[33]: 25  
```  
  
```python  
In [34]: for t_, (no1, no2, no3, no4, no5) in zip(t, a): # ❻ 逐行组合数据...  
             s = '{},{},{},{},{},{}\n'.format(t_, no1, no2, no3, no4, no5) # ❼ ...变成 str 对象...  
             csv_file.write(s) # ❽ ...并逐行写入（追加到 CSV 文本文件中）。  
  
In [35]: csv_file.close()  
  
In [36]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  284757 Oct 19 12:11  
 /Users/yves/Temp/data/data.csv  
```  
  
反向操作的工作方式也非常相似。首先，打开现有的 CSV 文件。其次，使用文件对象的 `.readline()` 或 `.readlines()` 方法逐行读取其内容：  
  
```python  
In [37]: csv_file = open(path + 'data.csv', 'r') # ❶ 打开文件以便读取 (r)。  
  
In [38]: for i in range(5):  
             print(csv_file.readline(), end='') # ❷ 逐行读取文件内容并打印它们。  
```  
  
```txt  
 date,no1,no2,no3,no4,no5  
 2019-01-01 00:00:00,-0.0892,-1.0508,-0.5942,0.3367,1.508  
 2019-01-01 01:00:00,2.1046,3.2623,0.704,-0.2651,0.4461  
 2019-01-01 02:00:00,-0.0482,-0.9221,0.1332,0.1192,0.7782  
 2019-01-01 03:00:00,-0.359,-2.4955,0.6164,0.712,-1.4328  
```  
  
```python  
In [39]: csv_file.close()  
  
In [40]: csv_file = open(path + 'data.csv', 'r') # ❶ 打开文件以便读取 (r)。  
  
In [41]: content = csv_file.readlines() # ❸ 一步读取文件内容...  
  
In [42]: content[:5] # ❹ ...其结果是一个将所有行作为独立 str 对象的 list 对象。  
```  
  
```txt  
Out[42]: ['date,no1,no2,no3,no4,no5\n',  
 '2019-01-01 00:00:00,-0.0892,-1.0508,-0.5942,0.3367,1.508\n',  
 '2019-01-01 01:00:00,2.1046,3.2623,0.704,-0.2651,0.4461\n',  
 '2019-01-01 02:00:00,-0.0482,-0.9221,0.1332,0.1192,0.7782\n',  
 '2019-01-01 03:00:00,-0.359,-2.4955,0.6164,0.712,-1.4328\n']  
```  
  
```python  
In [43]: csv_file.close()  
```  
  
CSV 文件非常重要且普遍，因此 Python 标准库中有一个 `csv` 模块可简化这些文件的处理。`csv` 模块的两个有用的 reader（迭代器）对象分别返回 `list` 对象的列表或 `dict` 对象的列表：  
  
```python  
In [44]: import csv  
  
In [45]: with open(path + 'data.csv', 'r') as f:  
             csv_reader = csv.reader(f) # ❶ csv.reader() 将每一行作为一个 list 对象返回。  
             lines = [line for line in csv_reader]  
  
In [46]: lines[:5]  
```  
  
```txt  
Out[46]: [['date', 'no1', 'no2', 'no3', 'no4', 'no5'],  
 ['2019-01-01 00:00:00', '-0.0892', '-1.0508', '-0.5942', '0.3367',  
  '1.508'],  
 ['2019-01-01 01:00:00', '2.1046', '3.2623', '0.704', '-0.2651',  
  '0.4461'],  
 ['2019-01-01 02:00:00', '-0.0482', '-0.9221', '0.1332', '0.1192',  
  '0.7782'],  
 ['2019-01-01 03:00:00', '-0.359', '-2.4955', '0.6164', '0.712',  
  '-1.4328']]  
```  
  
```python  
In [47]: with open(path + 'data.csv', 'r') as f:  
             csv_reader = csv.DictReader(f) # ❷ csv.DictReader() 将每一行作为 OrderedDict 返回，这是 dict 对象的一个特例。  
             lines = [line for line in csv_reader]  
  
In [48]: lines[:3]  
```  
  
```txt  
Out[48]: [OrderedDict([('date', '2019-01-01 00:00:00'),  
               ('no1', '-0.0892'),  
               ('no2', '-1.0508'),  
               ('no3', '-0.5942'),  
               ('no4', '0.3367'),  
               ('no5', '1.508')]),  
  OrderedDict([('date', '2019-01-01 01:00:00'),  
               ('no1', '2.1046'),  
               ('no2', '3.2623'),  
               ('no3', '0.704'),  
               ('no4', '-0.2651'),  
               ('no5', '0.4461')]),  
  OrderedDict([('date', '2019-01-01 02:00:00'),  
               ('no1', '-0.0482'),  
               ('no2', '-0.9221'),  
               ('no3', '0.1332'),  
               ('no4', '0.1192'),  
               ('no5', '0.7782')])]  
```  
  
```python  
In [49]: !rm -f $path*  
```  
  
### 使用 SQL 数据库  
  
Python 可以使用任何类型的结构化查询语言 (SQL) 数据库，通常也支持任何类型的 NoSQL 数据库。Python 默认附带的一个 SQL 数据库（或关系型数据库）是 SQLite3。有了它，可以很容易地说明操作 SQL 数据库的基本 Python 方法（有关适用于 Python 的数据库连接器概述，请访问 https://wiki.python.org/moin/DatabaseInterfaces。与其直接操作关系型数据库，诸如 SQLAlchemy 之类的对象关系映射器通常很有用。它们引入了一个抽象层，允许使用更具 Python 风格、面向对象的代码。它们还允许你在后端更轻松地将一个关系型数据库替换为另一个关系型数据库。）：  
  
```python  
In [50]: import sqlite3 as sq3  
  
In [51]: con = sq3.connect(path + 'numbs.db') # ❶ 打开数据库连接；如果文件不存在则创建一个文件。  
  
In [52]: query = 'CREATE TABLE numbs (Date date, No1 real, No2 real)' # ❷ 创建一个具有三列的表的 SQL 查询（有关 SQLite3 语言方言的概述，请参见 https://www.sqlite.org/lang.html）。  
  
In [53]: con.execute(query) # ❸ 执行该查询...  
```  
  
```txt  
Out[53]: <sqlite3.Cursor at 0x102655f10>  
```  
  
```python  
In [54]: con.commit() # ❹ ...并提交更改。  
  
In [55]: q = con.execute # ❺ 为 con.execute() 方法定义一个简短的别名。  
  
In [56]: q('SELECT * FROM sqlite_master').fetchall() # ❻ 获取关于数据库的元信息，将刚创建的表显示为单一对象。  
```  
  
```txt  
Out[56]: [('table',  
  'numbs',  
  'numbs',  
  2,  
  'CREATE TABLE numbs (Date date, No1 real, No2 real)')]  
```  
  
现在有了一个带有数据表的数据库文件，这个表可以用数据来填充了。每一行由一个 `datetime` 对象和两个 `float` 对象组成：  
  
```python  
In [57]: import datetime  
  
In [58]: now = datetime.datetime.now()  
         q('INSERT INTO numbs VALUES(?, ?, ?)', (now, 0.12, 7.3)) # ❶ 写入单行（或记录）到 numbs 表中。  
```  
  
```txt  
Out[58]: <sqlite3.Cursor at 0x102655f80>  
```  
  
```python  
In [59]: np.random.seed(100)  
  
In [60]: data = np.random.standard_normal((10000, 2)).round(4) # ❷ 创建一个较大的伪数据集作为 ndarray 对象。  
  
In [61]: %%time  
         for row in data: # ❸ 迭代该 ndarray 对象的行。  
             now = datetime.datetime.now()  
             q('INSERT INTO numbs VALUES(?, ?, ?)', (now, row[0], row[1]))  
         con.commit()  
```  
  
```txt  
 CPU times: user 115 ms, sys: 6.69 ms, total: 121 ms  
 Wall time: 124 ms  
```  
  
```python  
In [62]: q('SELECT * FROM numbs').fetchmany(4) # ❹ 从表中检索多行数据。  
```  
  
```txt  
Out[62]: [('2018-10-19 12:11:15.564019', 0.12, 7.3),  
 ('2018-10-19 12:11:15.592956', -1.7498, 0.3427),  
 ('2018-10-19 12:11:15.593033', 1.153, -0.2524),  
 ('2018-10-19 12:11:15.593051', 0.9813, 0.5142)]  
```  
  
```python  
In [63]: q('SELECT * FROM numbs WHERE no1 > 0.5').fetchmany(4) # ❺ 操作相同，但增加了对 No1 列中数值的条件。  
```  
  
```txt  
Out[63]: [('2018-10-19 12:11:15.593033', 1.153, -0.2524),  
 ('2018-10-19 12:11:15.593051', 0.9813, 0.5142),  
 ('2018-10-19 12:11:15.593104', 0.6727, -0.1044),  
 ('2018-10-19 12:11:15.593134', 1.619, 1.5416)]  
```  
  
```python  
In [64]: pointer = q('SELECT * FROM numbs') # ❻ 定义一个 pointer 对象...  
  
In [65]: for i in range(3):  
             print(pointer.fetchone()) # ❼ ...该对象的行为类似于生成器对象。  
```  
  
```txt  
 ('2018-10-19 12:11:15.564019', 0.12, 7.3)  
 ('2018-10-19 12:11:15.592956', -1.7498, 0.3427)  
 ('2018-10-19 12:11:15.593033', 1.153, -0.2524)  
```  
  
```python  
In [66]: rows = pointer.fetchall() # ❽ 检索所有剩余的行。  
         rows[:3]  
```  
  
```txt  
Out[66]: [('2018-10-19 12:11:15.593051', 0.9813, 0.5142),  
 ('2018-10-19 12:11:15.593063', 0.2212, -1.07),  
 ('2018-10-19 12:11:15.593073', -0.1895, 0.255)]  
```  
  
最后，如果不再需要，可能想要删除数据库中的表对象：  
  
```python  
In [67]: q('DROP TABLE IF EXISTS numbs') # ❶ 从数据库中删除表。  
```  
  
```txt  
Out[67]: <sqlite3.Cursor at 0x1187a7420>  
```  
  
```python  
In [68]: q('SELECT * FROM sqlite_master').fetchall() # ❷ 执行此操作后将不再留下任何表对象。  
```  
  
```txt  
Out[68]: []  
```  
  
```python  
In [69]: con.close() # ❸ 关闭数据库连接。  
  
In [70]: !rm -f $path* # ❹ 从磁盘中删除数据库文件。  
```  
  
SQL 数据库是一个相当宽泛的话题；事实上，它过于宽泛和复杂，以至于无法在本章进行深入的探讨。其基本信息是：  
  
* Python 可以与几乎任何数据库技术很好地集成。  
* 基本的 SQL 语法主要由正在使用的数据库决定；其余部分则是所谓的“Pythonic”。  
  
本章后面还会包含一些基于 SQLite3 的示例。  
  
### 读写 NumPy 数组  
  
NumPy 本身就有一些函数能以方便且高性能的方式读写 `ndarray` 对象。这在某些情况下省去了不少麻烦，例如在将 NumPy `dtype` 对象转换为特定的数据库数据类型时（比如用于 SQLite3）。为了说明 NumPy 可以作为基于 SQL 方法的有效替代方案，下面的代码使用 NumPy 重现了上一节的示例。  
  
代码没有使用 pandas，而是使用 NumPy 的 `np.arange()` 函数来生成一个存储了 `datetime` 对象的 `ndarray` 对象：  
  
```python  
In [71]: dtimes = np.arange('2019-01-01 10:00:00', '2025-12-31 22:00:00',  
                            dtype='datetime64[m]') # ❶ 创建一个将 datetime 作为 dtype 的 ndarray 对象。  
  
In [72]: len(dtimes)  
```  
  
```txt  
Out[72]: 3681360  
```  
  
```python  
In [73]: dty = np.dtype([('Date', 'datetime64[m]'),  
                         ('No1', 'f'), ('No2', 'f')]) # ❷ 为结构化数组定义特殊的 dtype 对象。  
  
In [74]: data = np.zeros(len(dtimes), dtype=dty) # ❸ 使用特定的 dtype 实例化 ndarray 对象。  
  
In [75]: data['Date'] = dtimes # ❹ 填充 Date 列。  
  
In [76]: a = np.random.standard_normal((len(dtimes), 2)).round(4) # ❺ 虚拟的数据集...  
  
In [77]: data['No1'] = a[:, 0] # ❻ ...用于填充 No1 和 No2 列。  
         data['No2'] = a[:, 1] # ❻ ...用于填充 No1 和 No2 列。  
  
In [78]: data.nbytes # ❼ 结构化数组的字节大小。  
```  
  
```txt  
Out[78]: 58901760  
```  
  
保存 `ndarray` 对象的过程经过了高度优化，因此非常快。将近 60 MB 的数据保存到磁盘上只需要不到一秒钟（这里使用的是 SSD）。一个包含 480 MB 数据的较大 `ndarray` 对象保存到磁盘上大约需要半秒钟（请注意，即使在同一台机器上多次重复，这类耗时也可能存在显著差异，因为它们取决于很多因素，比如机器在同一时间执行的 CPU 和 I/O 相关的操作。）：  
  
```python  
In [79]: %time np.save(path + 'array', data) # ❶ 将结构化的 ndarray 对象保存在磁盘上。  
```  
  
```txt  
 CPU times: user 37.4 ms, sys: 58.9 ms, total: 96.4 ms  
 Wall time: 77.9 ms  
```  
  
```python  
In [80]: ll $path* # ❷ 磁盘上的大小几乎没有比在内存中大（归功于二进制存储）。  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  58901888 Oct 19 12:11  
 /Users/yves/Temp/data/array.npy  
```  
  
```python  
In [81]: %time np.load(path + 'array.npy') # ❸ 从磁盘加载结构化的 ndarray 对象。  
```  
  
```txt  
 CPU times: user 1.67 ms, sys: 44.8 ms, total: 46.5 ms  
 Wall time: 44.6 ms  
Out[81]: array([('2019-01-01T10:00',  1.5131,  0.6973),  
       ('2019-01-01T10:01', -1.722 , -0.4815),  
       ('2019-01-01T10:02',  0.8251,  0.3019), ...,  
       ('2025-12-31T21:57',  1.372 ,  0.6446),  
       ('2025-12-31T21:58', -1.2542,  0.1612),  
       ('2025-12-31T21:59', -1.1997, -1.097 )],  
      dtype=[('Date', '<M8[m]'), ('No1', '<f4'), ('No2', '<f4')])  
```  
  
```python  
In [82]: %time data = np.random.standard_normal((10000, 6000)).round(4) # ❹ 一个更大的常规 ndarray 对象。  
```  
  
```txt  
 CPU times: user 2.69 s, sys: 391 ms, total: 3.08 s  
 Wall time: 2.78 s  
```  
  
```python  
In [83]: data.nbytes # ❹ 一个更大的常规 ndarray 对象。  
```  
  
```txt  
Out[83]: 480000000  
```  
  
```python  
In [84]: %time np.save(path + 'array', data) # ❶  
```  
  
```txt  
 CPU times: user 42.9 ms, sys: 300 ms, total: 343 ms  
 Wall time: 481 ms  
```  
  
```python  
In [85]: ll $path* # ❷  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  480000128 Oct 19 12:11  
 /Users/yves/Temp/data/array.npy  
```  
  
```python  
In [86]: %time np.load(path + 'array.npy') # ❸  
```  
  
```txt  
 CPU times: user 2.32 ms, sys: 363 ms, total: 365 ms  
 Wall time: 363 ms  
Out[86]: array([[ 0.3066,  0.5951,  0.5826, ...,  1.6773,  0.4294, -0.2216],  
       [ 0.8769,  0.7292, -0.9557, ...,  0.5084,  0.9635, -0.4443],  
       [-1.2202, -2.5509, -0.0575, ..., -1.6128,  0.4662, -1.3645],  
       ...,  
       [-0.5598,  0.2393, -2.3716, ...,  1.7669,  0.2462,  1.035 ],  
       [ 0.273 ,  0.8216, -0.0749, ..., -0.0552, -0.8396,  0.3077],  
       [-0.6305,  0.8331,  1.3702, ...,  0.3493,  0.1981,  0.2037]])  
```  
  
```python  
In [87]: !rm -f $path*  
```  
  
这些例子说明在这种情况下，写入磁盘主要受限于硬件，因为所观察到的速度大致相当于撰写本书时标准 SSD 所宣称的写入速度（约 500 MB/s）。  
  
无论如何，与 SQL 数据库或使用 `pickle` 模块进行序列化相比，这种数据存储和检索形式有望更快。有两个原因：首先，数据主要是数值型的；其次，NumPy 使用的是二进制存储，这几乎将开销降低到了零。当然，这种方法并不具备 SQL 数据库那样的功能，但是 PyTables 将在这方面提供帮助，这在随后的几节中将会展示。  
  
## 使用 pandas 进行 I/O  
  
pandas 的主要优势之一是它能够原生读写不同的数据格式，包括：  
  
* CSV（逗号分隔值）  
* SQL（结构化查询语言）  
* XLS/XSLX（微软 Excel 文件）  
* JSON（JavaScript 对象简谱）  
* HTML（超文本标记语言）  
  
表 9-1 列出了 pandas 和 `DataFrame` 类分别支持的格式以及相应的导入和导出函数/方法。例如，像 `pd.read_csv()` 这个导入函数所采用的参数，在 `pandas.read_csv` 的文档中就有描述。  
  
<img width="561" height="351" alt="python_for_finance 9 1 1" src="https://github.com/user-attachments/assets/b2f170c9-9c98-4a5f-b7bf-4b912d166d1f" />
  
测试用例仍然是较大的一组 `float` 对象：  
  
```python  
In [88]: data = np.random.standard_normal((1000000, 5)).round(4)  
  
In [89]: data[:3]  
```  
  
```txt  
Out[89]: array([[ 0.4918,  1.3707,  0.137 ,  0.3981, -1.0059],  
       [ 0.4516,  1.4445,  0.0555, -0.0397,  0.44  ],  
       [ 0.1629, -0.8473, -0.8223, -0.4621, -0.5137]])  
```  
  
为此，本节也重新审视了 SQLite3，并使用 pandas 比较了其他可选格式的性能。  
  
### 使用 SQL 数据库  
  
以下关于 SQLite3 的所有操作现在应该很熟悉了：  
  
```python  
In [90]: filename = path + 'numbers'  
  
In [91]: con = sq3.Connection(filename + '.db')  
  
In [92]: query = 'CREATE TABLE numbers (No1 real, No2 real,\  
                  No3 real, No4 real, No5 real)' # ❶ 创建一个有五列的表来存放实数（float 对象）。  
  
In [93]: q = con.execute  
         qm = con.executemany  
  
In [94]: q(query)  
```  
  
```txt  
Out[94]: <sqlite3.Cursor at 0x1187a76c0>  
```  
  
这次由于数据存在于单个 `ndarray` 对象中，所以可以应用 `.executemany()` 方法。读取和处理数据的方法与以前一样。查询结果也很容易被可视化（参见图 9-1）：  
  
```python  
In [95]: %%time  
         qm('INSERT INTO numbers VALUES (?, ?, ?, ?, ?)', data) # ❶ 在单个步骤中将整个数据集插入表中。  
         con.commit()  
```  
  
```txt  
 CPU times: user 7.3 s, sys: 195 ms, total: 7.49 s  
 Wall time: 7.71 s  
```  
  
```python  
In [96]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  52633600 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.db  
```  
  
```python  
In [97]: %%time  
         temp = q('SELECT * FROM numbers').fetchall() # ❷ 在单个步骤中检索表中的所有行。  
         print(temp[:3])  
```  
  
```txt  
 [(0.4918, 1.3707, 0.137, 0.3981, -1.0059), (0.4516, 1.4445, 0.0555,  
 -0.0397, 0.44), (0.1629, -0.8473, -0.8223, -0.4621, -0.5137)]  
 CPU times: user 1.7 s, sys: 124 ms, total: 1.82 s  
 Wall time: 1.9 s  
```  
  
```python  
In [98]: %%time  
         query = 'SELECT * FROM numbers WHERE No1 > 0 AND No2 < 0'  
         res = np.array(q(query).fetchall()).round(3) # ❸ 检索多行的选定部分并将其转换为一个 ndarray 对象。  
```  
  
```txt  
 CPU times: user 639 ms, sys: 64.7 ms, total: 704 ms  
 Wall time: 702 ms  
```  
  
```python  
In [99]: res = res[::100] # ❹ 绘制查询结果的子集。  
         plt.figure(figsize=(10, 6))  
         plt.plot(res[:, 0], res[:, 1], 'ro') # ❹ 绘制查询结果的子集。  
```  
  
<img width="666" height="423" alt="python_for_finance 9 1" src="https://github.com/user-attachments/assets/532b3f94-545b-419a-9693-e173b618ce12" />
  
图 9-1. 查询结果的散点图（所选）  
  
### 从 SQL 到 pandas  
  
然而，通常更高效的方法是使用 pandas 来读取整个表或查询结果。当人们能够将一整个表读入内存时，通常执行分析查询的速度会比使用基于磁盘的 SQL 方法（内存外）快得多。  
  
使用 pandas 读取整张表所花费的时间与将其读入 NumPy `ndarray` 对象的时间大致相同。在那边和在这里一样，性能方面的瓶颈在于 SQL 数据库：  
  
```python  
In [100]: %time data = pd.read_sql('SELECT * FROM numbers', con) # ❶ 检索表的所有行存入名为 data 的 DataFrame 对象中。  
```  
  
```txt  
 CPU times: user 2.17 s, sys: 180 ms, total: 2.35 s  
 Wall time: 2.32 s  
```  
  
```python  
In [101]: data.head()  
```  
  
```txt  
Out[101]:       No1     No2     No3     No4     No5  
 0  0.4918  1.3707  0.1370  0.3981 -1.0059  
 1  0.4516  1.4445  0.0555 -0.0397  0.4400  
 2  0.1629 -0.8473 -0.8223 -0.4621 -0.5137  
 3  1.3064  0.9125  0.5142 -0.7868 -0.3398  
 4 -0.1148 -1.5215 -0.7045 -1.0042 -0.0600  
```  
  
现在数据已经进入了内存，这允许进行快得多的分析。速度的提升通常在一个数量级或以上。pandas 还能处理更复杂的查询，尽管在处理复杂的关系型数据结构时，它既不是用来替代 SQL 数据库的，也无法做到这一点。将多个条件组合起来的查询结果显示在图 9-2 中：  
  
```python  
In [102]: %time data[(data['No1'] > 0) & (data['No2'] < 0)].head() # ❶ 两个条件发生逻辑组合。  
```  
  
```txt  
 CPU times: user 47.1 ms, sys: 12.3 ms, total: 59.4 ms  
 Wall time: 33.4 ms  
Out[102]:        No1     No2     No3     No4     No5  
 2   0.1629 -0.8473 -0.8223 -0.4621 -0.5137  
 5   0.1893 -0.0207 -0.2104  0.9419  0.2551  
 8   1.4784 -0.3333 -0.7050  0.3586 -0.3937  
 10  0.8092 -0.9899  1.0364 -1.0453  0.0579  
 11  0.9065 -0.7757 -0.9267  0.7797  0.0863  
```  
  
```python  
In [103]: %%time  
          q = '(No1 < -0.5 | No1 > 0.5) & (No2 < -1 | No2 > 1)' # ❷ 四个条件发生逻辑组合。  
          res = data[['No1', 'No2']].query(q) # ❷ 四个条件发生逻辑组合。  
```  
  
```txt  
 CPU times: user 95.4 ms, sys: 22.4 ms, total: 118 ms  
 Wall time: 56.4 ms  
```  
  
```python  
In [104]: plt.figure(figsize=(10, 6))  
          plt.plot(res['No1'], res['No2'], 'ro');  
```  
  
<img width="666" height="427" alt="python_for_finance 9 2" src="https://github.com/user-attachments/assets/5ff2e14a-769a-437d-aca9-7238373701cd" />
  
图 9-2. 查询结果的散点图（所选）  
  
正如预期的那样，使用 pandas 的内存分析能力可以显著加快速度，前提是 pandas 能够复现对应的 SQL 语句。  
  
这还不是使用 pandas 的唯一优势，因为 pandas 与许多其他包（包括随后的章节将要介绍的 PyTables）都紧密集成。在这里，只需知道两者的结合可以大幅提升 I/O 操作的速度就足够了。这在以下内容中得到了展示：  
  
```python  
In [105]: h5s = pd.HDFStore(filename + '.h5s', 'w') # ❶ 打开一个 HDF5 数据库文件以进行写入；在 pandas 中创建了一个 HDFStore 对象。  
  
In [106]: %time h5s['data'] = data # ❷ 整个 DataFrame 对象通过二进制存储保存在了数据库文件中。  
```  
  
```txt  
 CPU times: user 46.7 ms, sys: 47.1 ms, total: 93.8 ms  
 Wall time: 99.7 ms  
```  
  
```python  
In [107]: h5s # ❸ 有关 HDFStore 对象的信息。  
```  
  
```txt  
Out[107]: <class 'pandas.io.pytables.HDFStore'>  
 File path: /Users/yves/Temp/data/numbers.h5s  
```  
  
```python  
In [108]: h5s.close() # ❹ 数据库文件关闭了。  
```  
  
与在 SQLite3 中使用相同的过程相比，将包含原始 SQL 表中所有数据的整个 `DataFrame` 写入的速度要快得多。读取速度甚至更快：  
  
```python  
In [109]: %%time  
          h5s = pd.HDFStore(filename + '.h5s', 'r') # ❶ 打开 HDF5 数据库文件以进行读取。  
          data_ = h5s['data'] # ❷ 读入并存入内存，成为名为 data_ 的 DataFrame。  
          h5s.close() # ❸ 数据库文件关闭了。  
```  
  
```txt  
 CPU times: user 11 ms, sys: 18.3 ms, total: 29.3 ms  
 Wall time: 29.4 ms  
```  
  
```python  
In [110]: data_ is data # ❹ 这两个 DataFrame 对象不是同一个...  
```  
  
```txt  
Out[110]: False  
```  
  
```python  
In [111]: (data_ == data).all() # ❺ ...但它们现在包含相同的数据。  
```  
  
```txt  
Out[111]: No1    True  
 No2    True  
 No3    True  
 No4    True  
 No5    True  
 dtype: bool  
```  
  
```python  
In [112]: np.allclose(data_, data) # ❺ ...但它们现在包含相同的数据。  
```  
  
```txt  
Out[112]: True  
```  
  
```python  
In [113]: ll $path* # ❻ 二进制存储通常比像 SQL 表那样的形式尺寸开销更小。  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  52633600 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.db  
 -rw-r--r--  1 yves  staff  48007240 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.h5s  
```  
  
### 使用 CSV 文件  
  
交换金融数据时最广泛使用的格式之一是 CSV 格式。虽然它没有真正的标准化，但任何平台以及绝大多数涉及数据和金融分析的应用程序都可以处理它。在前面，我们看到了如何使用标准的 Python 功能向 CSV 文件读写数据（请参阅第 236 页的“读写文本文件”）。pandas 使整个过程变得更加方便，代码更加简洁，并且执行速度总体上更快（另见图 9-3）：  
  
```python  
In [114]: %time data.to_csv(filename + '.csv') # ❶ .to_csv() 方法以 CSV 格式将 DataFrame 数据写入磁盘。  
```  
  
```txt  
 CPU times: user 6.44 s, sys: 139 ms, total: 6.58 s  
 Wall time: 6.71 s  
```  
  
```python  
In [115]: ll $path  
```  
  
```txt  
 total 283672  
 -rw-r--r--  1 yves  staff  43834157 Oct 19 12:11 numbers.csv  
 -rw-r--r--  1 yves  staff  52633600 Oct 19 12:11 numbers.db  
 -rw-r--r--  1 yves  staff  48007240 Oct 19 12:11 numbers.h5s  
```  
  
```python  
In [116]: %time df = pd.read_csv(filename + '.csv') # ❷ 然后 pd.read_csv() 方法将其作为一个新的 DataFrame 对象读回内存。  
```  
  
```txt  
 CPU times: user 1.12 s, sys: 111 ms, total: 1.23 s  
 Wall time: 1.23 s  
```  
  
```python  
In [117]: df[['No1', 'No2', 'No3', 'No4']].hist(bins=20, figsize=(10, 6));  
```  
  
<img width="664" height="428" alt="python_for_finance 9 3" src="https://github.com/user-attachments/assets/6eca60f8-aa70-4519-94d9-92194218924f" />
  
图 9-3. 所选列的直方图  
  
### 使用 Excel 文件  
  
以下代码简要演示了 pandas 如何以 Excel 格式写入数据以及从 Excel 电子表格中读取数据。在本例中，数据集被限制为 100,000 行（另见图 9-4）：  
  
```python  
In [118]: %time data[:100000].to_excel(filename + '.xlsx') # ❶ .to_excel() 方法以 XLSX 格式将 DataFrame 数据写入磁盘。  
```  
  
```txt  
 CPU times: user 25.9 s, sys: 520 ms, total: 26.4 s  
 Wall time: 27.3 s  
```  
  
```python  
In [119]: %time df = pd.read_excel(filename + '.xlsx', 'Sheet1') # ❷ 然后 pd.read_excel() 方法会将其作为一个新的 DataFrame 对象读回内存，这里还指定了要读取的表格。  
```  
  
```txt  
 CPU times: user 5.78 s, sys: 70.1 ms, total: 5.85 s  
 Wall time: 5.91 s  
```  
  
```python  
In [120]: df.cumsum().plot(figsize=(10, 6));  
  
In [121]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  43834157 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.csv  
 -rw-r--r--  1 yves  staff  52633600 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.db  
 -rw-r--r--  1 yves  staff  48007240 Oct 19 12:11  
 /Users/yves/Temp/data/numbers.h5s  
 -rw-r--r--  1 yves  staff   4032725 Oct 19 12:12  
 /Users/yves/Temp/data/numbers.xlsx  
```  
  
```python  
In [122]: rm -f $path*  
```  
  
利用较小的数据子集生成 Excel 电子表格文件需要相当长的时间。这说明了电子表格结构带来了怎样的开销。  
  
检查生成的文件可以发现，`DataFrame` 和 `HDFStore` 组合是目前最为紧凑的替代方案（使用下一节将介绍的压缩功能还能进一步增加优势）。与 CSV 文件（即文本文件）相比，同等数据量的数据文件在体积上还要稍大一些。这也是在操作 CSV 文件时性能较慢的一个原因，另一个原因则是它们“仅仅”只是一般的文本文件。  
  
<img width="665" height="418" alt="python_for_finance 9 4" src="https://github.com/user-attachments/assets/8e19aec0-4846-41f9-84ea-daafcf2afff4" />
  
图 9-4. 所有列的线图  
  
## 使用 PyTables 进行 I/O  
  
PyTables 是针对 HDF5 数据库标准的 Python 绑定。它经过专门设计以优化 I/O 操作的性能，并充分利用可用的硬件。该库的导入名称为 `tables`。与 pandas 类似，当涉及内存分析时，PyTables 既无法也不意味着能完全替代 SQL 数据库（许多其他数据库需要服务器/客户端的架构。对于交互式的数据与金融分析来说，基于文件的数据库被证明更加方便并且对于大多数目标也是足够的。）。然而，它带来的一些功能进一步缩小了这一差距。例如，一个 PyTables 数据库可以包含多个表，并且支持压缩、索引，以及对这些表的非简单（nontrivial）查询。此外，它能高效地存储 NumPy 数组，并且拥有自己独特风格的类数组数据结构。  
  
首先，进行一些导入：  
  
```python  
In [123]: import tables as tb # ❶ 该包的名字叫 PyTables，导入名字叫 tables。  
          import datetime as dt  
```  
  
### 使用 Tables  
  
PyTables 提供了一种基于文件的数据库格式，类似于 SQLite3。下面打开了一个数据库文件并创建了一个表：  
  
```python  
In [124]: filename = path + 'pytab.h5'  
  
In [125]: h5 = tb.open_file(filename, 'w') # ❶ 在 HDF5 二进制存储格式下打开一个数据库文件。  
  
In [126]: row_des = {  
              'Date': tb.StringCol(26, pos=1), # ❷ Date 列用于 datetime 信息（作为一个 str 对象）。  
              'No1': tb.IntCol(pos=2), # ❸ 存储 int 对象的两列。  
              'No2': tb.IntCol(pos=3), # ❸ 存储 int 对象的两列。  
              'No3': tb.Float64Col(pos=4), # ❹ 存储 float 对象的两列。  
              'No4': tb.Float64Col(pos=5) # ❹ 存储 float 对象的两列。  
              }  
  
In [127]: rows = 2000000  
  
In [128]: filters = tb.Filters(complevel=0) # ❺ 例如，可以通过 Filters 对象指定压缩等级。  
  
In [129]: tab = h5.create_table('/', 'ints_floats', # ❻ 该表的节点（路径）和技术名称。  
                                row_des, # ❼ 行数据结构的描述。  
                                title='Integers and Floats', # ❽ 该表的名字（标题）。  
                                expectedrows=rows, # ❾ 期望的行数；用于进行优化。  
                                filters=filters) # ❿ 该表使用的 Filters 对象。  
  
In [130]: type(tab)  
```  
  
```txt  
Out[130]: tables.table.Table  
```  
  
```python  
In [131]: tab  
```  
  
```txt  
Out[131]: /ints_floats (Table(0,)) 'Integers and Floats'  
  description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
  byteorder := 'little'  
  chunkshape := (2621,)  
```  
  
要用数值数据来填充表格，生成了两个包含随机数的 `ndarray` 对象：一个包含随机整数，另一个包含随机浮点数。表格的填充是通过一个简单的 Python 循环进行的：  
  
```python  
In [132]: pointer = tab.row # ❶ 创建一个 pointer 对象。   
  
In [133]: ran_int = np.random.randint(0, 10000, size=(rows, 2)) # ❷ 用随机的 int 对象创建一个 ndarray 对象。  
  
In [134]: ran_flo = np.random.standard_normal((rows, 2)).round(4) # ❸ 用随机的 float 对象创建一个 ndarray 对象。  
  
In [135]: %%time  
          for i in range(rows):  
              pointer['Date'] = dt.datetime.now() # ❹ 逐行写入 datetime 对象以及两个 int 和两个 float 对象。  
              pointer['No1'] = ran_int[i, 0] # ❹ 逐行写入 datetime 对象以及两个 int 和两个 float 对象。  
              pointer['No2'] = ran_int[i, 1] # ❹ 逐行写入 datetime 对象以及两个 int 和两个 float 对象。  
              pointer['No3'] = ran_flo[i, 0] # ❹ 逐行写入 datetime 对象以及两个 int 和两个 float 对象。  
              pointer['No4'] = ran_flo[i, 1] # ❹ 逐行写入 datetime 对象以及两个 int 和两个 float 对象。  
              pointer.append() # ❺ 追加新行。  
          tab.flush() # ❻ 刷新所有已写的行；也就是，提交为永久性更改。  
```  
  
```txt  
 CPU times: user 8.16 s, sys: 78.7 ms, total: 8.24 s  
 Wall time: 8.25 s  
```  
  
```python  
In [136]: tab # ❼ 该更改会反映在该 Table 对象的描述中。  
```  
  
```txt  
Out[136]: /ints_floats (Table(2000000,)) 'Integers and Floats'  
  description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
  byteorder := 'little'  
  chunkshape := (2621,)  
```  
  
```python  
In [137]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  100156248 Oct 19 12:12  
 /Users/yves/Temp/data/pytab.h5  
```  
  
在这种情况下，Python 循环相当慢。还有一种更高效、更 Pythonic 的方法可以达到相同的结果，即使用 NumPy 结构化数组。有了存储在结构化数组中的完整数据集，创建表的过程就可以归结为一行代码了。请注意，不再需要行描述；PyTables 使用结构化数组的 `dtype` 对象来推断数据类型：  
  
```python  
In [138]: dty = np.dtype([('Date', 'S26'), ('No1', '<i4'), ('No2', '<i4'),  
                          ('No3', '<f8'), ('No4', '<f8')]) # ❶ 这就定义了这个特殊的 dtype 对象。  
  
In [139]: sarray = np.zeros(len(ran_int), dtype=dty) # ❷ 这就创建了一个由零（和空字符串）填充的结构化数组。  
  
In [140]: sarray[:4] # ❸ 来自该 ndarray 对象的一些记录。  
```  
  
```txt  
Out[140]: array([(b'', 0, 0, 0., 0.), (b'', 0, 0, 0., 0.), (b'', 0, 0, 0., 0.),  
       (b'', 0, 0, 0., 0.)],  
      dtype=[('Date', 'S26'), ('No1', '<i4'), ('No2', '<i4'), ('No3', '<f8'),  
      ('No4', '<f8')])  
```  
  
```python  
In [141]: %%time  
          sarray['Date'] = dt.datetime.now() # ❹ ndarray 对象的列会被一次性填满。  
          sarray['No1'] = ran_int[:, 0] # ❹ ndarray 对象的列会被一次性填满。  
          sarray['No2'] = ran_int[:, 1] # ❹ ndarray 对象的列会被一次性填满。  
          sarray['No3'] = ran_flo[:, 0] # ❹ ndarray 对象的列会被一次性填满。  
          sarray['No4'] = ran_flo[:, 1] # ❹ ndarray 对象的列会被一次性填满。  
```  
  
```txt  
 CPU times: user 161 ms, sys: 42.7 ms, total: 204 ms  
 Wall time: 207 ms  
```  
  
```python  
In [142]: %%time  
          h5.create_table('/', 'ints_floats_from_array', sarray,  
                          title='Integers and Floats',  
                          expectedrows=rows, filters=filters) # ❺ 创建了该 Table 对象并用数据填充它。  
```  
  
```txt  
 CPU times: user 42.9 ms, sys: 51.4 ms, total: 94.3 ms  
 Wall time: 96.6 ms  
Out[142]: /ints_floats_from_array (Table(2000000,)) 'Integers and Floats'  
  description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
  byteorder := 'little'  
  chunkshape := (2621,)  
```  
  
这种方法不仅速度快了一个数量级，代码更简洁，而且实现了相同的结果：  
  
```python  
In [143]: type(h5)  
```  
  
```txt  
Out[143]: tables.file.File  
```  
  
```python  
In [144]: h5 # ❶ 拥有两个 Table 对象的该 File 对象的描述。  
```  
  
```txt  
Out[144]: File(filename=/Users/yves/Temp/data/pytab.h5, title='', mode='w',  
  root_uep='/', filters=Filters(complevel=0, shuffle=False,  
  bitshuffle=False, fletcher32=False, least_significant_digit=None))  
 / (RootGroup) ''  
 /ints_floats (Table(2000000,)) 'Integers and Floats'  
   description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
   byteorder := 'little'  
   chunkshape := (2621,)  
 /ints_floats_from_array (Table(2000000,)) 'Integers and Floats'  
   description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
   byteorder := 'little'  
   chunkshape := (2621,)  
```  
  
```python  
In [145]: h5.remove_node('/', 'ints_floats_from_array') # ❷ 这个方法删除了带有多余数据的第二个 Table 对象。  
```  
  
在绝大多数情况下，`Table` 对象的行为与 NumPy 的结构化 `ndarray` 对象非常相似（另见图 9-5）：  
  
```python  
In [146]: tab[:3] # ❶ 通过索引提取多行。   
```  
  
```txt  
Out[146]: array([(b'2018-10-19 12:12:28.227771', 8576, 5991, -0.0528,  0.2468),  
       (b'2018-10-19 12:12:28.227858', 2990, 9310, -0.0261,  0.3932),  
       (b'2018-10-19 12:12:28.227868', 4400, 4823,  0.9133,  0.2579)],  
      dtype=[('Date', 'S26'), ('No1', '<i4'), ('No2', '<i4'), ('No3', '<f8'),  
      ('No4', '<f8')])  
```  
  
```python  
In [147]: tab[:4]['No4'] # ❷ 通过索引仅提取出列的数值。   
```  
  
```txt  
Out[147]: array([ 0.2468,  0.3932,  0.2579, -0.5582])  
```  
  
```python  
In [148]: %time np.sum(tab[:]['No3']) # ❸ 运用 NumPy 这个通用的函数。   
```  
  
```txt  
 CPU times: user 76.7 ms, sys: 74.8 ms, total: 151 ms  
 Wall time: 152 ms  
Out[148]: 88.8542999999997  
```  
  
```python  
In [149]: %time np.sum(np.sqrt(tab[:]['No1'])) # ❸ 运用 NumPy 这个通用的函数。  
```  
  
```txt  
 CPU times: user 91 ms, sys: 57.9 ms, total: 149 ms  
 Wall time: 164 ms  
Out[149]: 133349920.3689251  
```  
  
```python  
In [150]: %%time  
          plt.figure(figsize=(10, 6))  
          plt.hist(tab[:]['No3'], bins=30); # ❹ 利用该 Table 对象绘制一列。   
```  
  
```txt  
 CPU times: user 328 ms, sys: 72.1 ms, total: 400 ms  
 Wall time: 456 ms  
```  
  
<img width="664" height="414" alt="python_for_finance 9 5" src="https://github.com/user-attachments/assets/605135e1-279d-4611-a5f7-4bc1873f9b24" />
  
图 9-5. 列数据的直方图  
  
PyTables 也提供灵活的工具通过类似于典型的 SQL 语句来查询数据，如下面的示例所示（其结果如图 9-6 所示；将其与基于 pandas 查询的图 9-2 进行比较）：  
  
```python  
In [151]: query = '((No3 < -0.5) | (No3 > 0.5)) & ((No4 < -1) | (No4 > 1))' # ❶ 以 str 对象表达的查询功能，包含这由逻辑运算符结合在一起的四个条件。  
  
In [152]: iterator = tab.where(query) # ❷ 根据上面的查询得到的迭代对象。   
  
In [153]: %time res = [(row['No3'], row['No4']) for row in iterator] # ❸ 该查询的结果行将通过列表解析式归集在一起...   
```  
  
```txt  
 CPU times: user 269 ms, sys: 64.4 ms, total: 333 ms  
 Wall time: 294 ms  
```  
  
```python  
In [154]: res = np.array(res) # ❹ ...并转化为一个 ndarray 对象。   
          res[:3]  
```  
  
```txt  
Out[154]: array([[0.7694, 1.4866],  
       [0.9201, 1.3346],  
       [1.4701, 1.8776]])  
```  
  
```python  
In [155]: plt.figure(figsize=(10, 6))  
          plt.plot(res.T[0], res.T[1], 'ro');  
```  
  
<img width="666" height="428" alt="python_for_finance 9 6" src="https://github.com/user-attachments/assets/73a80d0e-3b81-413f-b4a5-7334b59898b9" />
  
图 9-6. 列数据的散点图  
  
### 快速查询  
  
pandas 和 PyTables 都能处理相对复杂的、类似 SQL 的查询和选择。两者在涉及此类操作时都针对速度进行了优化。尽管与关系数据库相比，这些方法存在一定的局限性，但在大多数数值和金融应用中，这些局限性通常并不相关。  
  
正如以下例子所示，对于存储在 PyTables 中作为 `Table` 对象的数据，不论是在语法方面还是从性能的角度来看，操作它都会给人一种像是在内存中操作 NumPy 或是 pandas 对象的错觉：  
  
```python  
In [156]: %%time  
          values = tab[:]['No3']  
          print('Max %18.3f' % values.max())  
          print('Ave %18.3f' % values.mean())  
          print('Min %18.3f' % values.min())  
          print('Std %18.3f' % values.std())  
```  
  
```txt  
 Max              5.224  
 Ave              0.000  
 Min             -5.649  
 Std              1.000  
 CPU times: user 163 ms, sys: 70.4 ms, total: 233 ms  
 Wall time: 234 ms  
```  
  
```python  
In [157]: %%time  
          res = [(row['No1'], row['No2']) for row in  
                 tab.where('((No1 > 9800) | (No1 < 200)) \  
                           & ((No2 > 4500) & (No2 < 5500))')]  
```  
  
```txt  
 CPU times: user 165 ms, sys: 52.5 ms, total: 218 ms  
 Wall time: 155 ms  
```  
  
```python  
In [158]: for r in res[:4]:  
              print(r)  
```  
  
```txt  
 (91, 4870)  
 (9803, 5026)  
 (9846, 4859)  
 (9823, 5069)  
```  
  
```python  
In [159]: %%time  
          res = [(row['No1'], row['No2']) for row in  
                 tab.where('(No1 == 1234) & (No2 > 9776)')]  
```  
  
```txt  
 CPU times: user 58.9 ms, sys: 40.5 ms, total: 99.4 ms  
 Wall time: 81 ms  
```  
  
```python  
In [160]: for r in res:  
              print(r)  
```  
  
```txt  
 (1234, 9841)  
 (1234, 9821)  
 (1234, 9867)  
 (1234, 9987)  
 (1234, 9849)  
 (1234, 9800)  
```  
  
### 使用压缩表  
  
使用 PyTables 的一个主要优势是它采用了数据压缩的方法。它使用压缩不仅仅是为了节约磁盘上的空间，同时也是在某些硬件场景下用来提升 I/O 运算性能的。它是如何做到的呢？当 I/O 成了性能瓶颈而 CPU 能够快速完成数据（解）压缩的工作，那么压缩在处理速度上的净效果可能就是积极正面的。既然下面的这些例子都是建立在一块标准的 SSD 之上的，压缩的速度优势就观察不到。然而，使用该压缩方法也几乎没什么坏处：  
  
```python  
In [161]: filename = path + 'pytabc.h5'  
  
In [162]: h5c = tb.open_file(filename, 'w')  
  
In [163]: filters = tb.Filters(complevel=5, # ❶ complevel (压缩级别) 参数的取值范围为 0 (无压缩) 到 9 (最大压缩级别)。  
                               complib='blosc') # ❷ 采用对性能进行了专门优化的 Blosc 压缩引擎。  
  
In [164]: tabc = h5c.create_table('/', 'ints_floats', sarray,  
                                  title='Integers and Floats',  
                                  expectedrows=rows, filters=filters)  
  
In [165]: query = '((No3 < -0.5) | (No3 > 0.5)) & ((No4 < -1) | (No4 > 1))'  
  
In [166]: iteratorc = tabc.where(query) # ❸ 根据前面的查询创建迭代器对象。  
  
In [167]: %time res = [(row['No3'], row['No4']) for row in iteratorc] # ❹ 由该查询得到的多行内容由列表推导式收集到一起。  
```  
  
```txt  
 CPU times: user 300 ms, sys: 50.8 ms, total: 351 ms  
 Wall time: 311 ms  
```  
  
```python  
In [168]: res = np.array(res)  
          res[:3]  
```  
  
```txt  
Out[168]: array([[0.7694, 1.4866],  
       [0.9201, 1.3346],  
       [1.4701, 1.8776]])  
```  
  
生成带原始数据的经过压缩了的 `Table` 对象，并对其进行分析，相比于未经过压缩的 `Table` 对象稍微要慢一些。要是读取数据转化为一个 `ndarray` 对象结果又会如何呢？让我们一探究竟：  
  
```python  
In [169]: %time arr_non = tab.read() # ❶ 从尚未压缩的 Table 对象 tab 中读取数据。  
```  
  
```txt  
 CPU times: user 63 ms, sys: 78.5 ms, total: 142 ms  
 Wall time: 149 ms  
```  
  
```python  
In [170]: tab.size_on_disk  
```  
  
```txt  
Out[170]: 100122200  
```  
  
```python  
In [171]: arr_non.nbytes  
```  
  
```txt  
Out[171]: 100000000  
```  
  
```python  
In [172]: %time arr_com = tabc.read() # ❷ 从压缩了的 Table 对象 tabc 中读取数据。  
```  
  
```txt  
 CPU times: user 106 ms, sys: 55.5 ms, total: 161 ms  
 Wall time: 173 ms  
```  
  
```python  
In [173]: tabc.size_on_disk # ❸ 比较两者的体积——被压缩的表其体积明显地缩减了。  
```  
  
```txt  
Out[173]: 41306140  
```  
  
```python  
In [174]: arr_com.nbytes  
```  
  
```txt  
Out[174]: 100000000  
```  
  
```python  
In [175]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  200312336 Oct 19 12:12  
 /Users/yves/Temp/data/pytab.h5  
 -rw-r--r--  1 yves  staff   41341436 Oct 19 12:12  
 /Users/yves/Temp/data/pytabc.h5  
```  
  
```python  
In [176]: h5c.close() # ❹ 关闭数据库文件。  
```  
  
这些例子表明，在处理经过压缩的 `Table` 对象时，处理速度上对比未压缩过的对象简直毫无区别。但是，位于磁盘上的文件体积——它取决于数据的质量——却明显减小了，这将会带来许多益处：  
  
* 降低存储成本。  
* 降低备份成本。  
* 减少网络流量。  
* 提升网络速度（提高存取位于远程服务器上数据的速度）。  
* 为了克服 I/O 瓶颈问题从而提升了 CPU 的利用率。  
  
### 使用数组  
  
第 232 页的“使用 Python 进行基本 I/O”表明，NumPy 内置有针对 `ndarray` 对象的高速读写能力。在使用 PyTables 处理存取 `ndarray` 对象的任务时也能做到非常快、非常高效，同时它又是基于一种层次数据库结构的，因此便具备了许多其他便利特性：  
  
```python  
In [177]: %%time  
          arr_int = h5.create_array('/', 'integers', ran_int) # ❶ 存储 ran_int 这个 ndarray 对象。  
          arr_flo = h5.create_array('/', 'floats', ran_flo) # ❷ 存储 ran_flo 这个 ndarray 对象。  
```  
  
```txt  
 CPU times: user 4.26 ms, sys: 37.2 ms, total: 41.5 ms  
 Wall time: 46.2 ms  
```  
  
```python  
In [178]: h5 # ❸ 相关的变动会立刻在对象的描述中反映出来。  
```  
  
```txt  
Out[178]: File(filename=/Users/yves/Temp/data/pytab.h5, title='', mode='w',  
  root_uep='/', filters=Filters(complevel=0, shuffle=False,  
  bitshuffle=False, fletcher32=False, least_significant_digit=None))  
 / (RootGroup) ''  
 /floats (Array(2000000, 2)) ''  
   atom := Float64Atom(shape=(), dflt=0.0)  
   maindim := 0  
   flavor := 'numpy'  
   byteorder := 'little'  
   chunkshape := None  
 /integers (Array(2000000, 2)) ''  
   atom := Int64Atom(shape=(), dflt=0)  
   maindim := 0  
   flavor := 'numpy'  
   byteorder := 'little'  
   chunkshape := None  
 /ints_floats (Table(2000000,)) 'Integers and Floats'  
   description := {  
   "Date": StringCol(itemsize=26, shape=(), dflt=b'', pos=0),  
   "No1": Int32Col(shape=(), dflt=0, pos=1),  
   "No2": Int32Col(shape=(), dflt=0, pos=2),  
   "No3": Float64Col(shape=(), dflt=0.0, pos=3),  
   "No4": Float64Col(shape=(), dflt=0.0, pos=4)}  
   byteorder := 'little'  
   chunkshape := (2621,)  
```  
  
```python  
In [179]: ll $path*  
```  
  
```txt  
 -rw-r--r--  1 yves  staff  262344490 Oct 19 12:12  
 /Users/yves/Temp/data/pytab.h5  
 -rw-r--r--  1 yves  staff   41341436 Oct 19 12:12  
 /Users/yves/Temp/data/pytabc.h5  
```  
  
```python  
In [180]: h5.close()  
  
In [181]: !rm -f $path*  
```  
  
将这些对象直接写入 HDF5 数据库要比循环遍历对象并将数据逐行写入 `Table` 对象或采用结构化的 `ndarray` 对象的方法来得快。  
  
> **基于 HDF5 存储数据**  
>  
> 当涉及结构化的数值型和金融数据时，HDF5 层次型数据库（文件）格式相较于关系数据库是一项强大的替代方案。无论是单独直接使用 PyTables，还是结合使用 pandas，都可以获得当前硬件能够支持的近乎极致的 I/O 性能。  
  
### 内存外（Out-of-Memory）计算  
  
PyTables 支持在内存外进行操作，这使得执行那些由于数据量大而内存放不下的基于数组的计算成为可能。为了达成这个目标，请参考下面的基于 `EArray` 类生成的代码。这类对象可以在一个维度（行）上进行扩展，不过列的数量（每行的元素数）则是固定不变的：  
  
```python  
In [182]: filename = path + 'earray.h5'  
  
In [183]: h5 = tb.open_file(filename, 'w')  
  
In [184]: n = 500 # ❶ 列的数量被设定为固定值。   
  
In [185]: ear = h5.create_earray('/', 'ear', # ❷ 这个 EArray 对象的路径以及其技术名称。   
                                 atom=tb.Float64Atom(), # ❸ 每个独立数据值的原子 dtype 对象。   
                                 shape=(0, n)) # ❹ 进行初始化时的对象的维度形状 (0 行, n 列)。   
  
In [186]: type(ear)  
```  
  
```txt  
Out[186]: tables.earray.EArray  
```  
  
```python  
In [187]: rand = np.random.standard_normal((n, n)) # ❺ 用于存储随机数的 ndarray 对象...   
          rand[:4, :4]  
```  
  
```txt  
Out[187]: array([[-1.25983231,  1.11420699,  0.1667485 ,  0.7345676 ],  
       [-0.13785424,  1.22232417,  1.36303097,  0.13521042],  
       [ 1.45487119, -1.47784078,  0.15027672,  0.86755989],  
       [-0.63519366,  0.1516327 , -0.64939447, -0.45010975]])  
```  
  
```python  
In [188]: %%time  
          for _ in range(750):  
              ear.append(rand) # ❻ ...被多次加以追加使用。   
          ear.flush()  
```  
  
```txt  
 CPU times: user 814 ms, sys: 1.18 s, total: 1.99 s  
 Wall time: 2.53 s  
```  
  
```python  
In [189]: ear  
```  
  
```txt  
Out[189]: /ear (EArray(375000, 500)) ''  
   atom := Float64Atom(shape=(), dflt=0.0)  
   maindim := 0  
   flavor := 'numpy'  
   byteorder := 'little'  
   chunkshape := (16, 500)  
```  
  
```python  
In [190]: ear.size_on_disk  
```  
  
```txt  
Out[190]: 1500032000  
```  
  
对于那些不需要聚合计算操作参与的内存外运算任务，则需要提供另外一个拥有相同的维度形状（体量大小）的 `EArray` 对象。PyTables 内含一个名叫 `Expr` 的针对数值表达式进行了专门优化的特殊模块。它是基于一个名为 `numexpr` 的数值运算表达式库产生的。后面的代码中便使用了 `Expr` 以便对前文产生的完整的 `EArray` 对象运用公式 9-1 中提供的数学表达式进行计算。  
  
公式 9-1. 数学表达式示例  
  
$$y = 3 \sin (x) + \sqrt{| x |}$$  
  
生成的结果被存入了向外部输出内容的名为 `out` 的 `EArray` 对象中，并且这部分公式的推导计算是按区块划分（chunk-wise）执行完成的：  
  
```python  
In [191]: out = h5.create_earray('/', 'out',  
                                 atom=tb.Float64Atom(),  
                                 shape=(0, n))  
  
In [192]: out.size_on_disk  
```  
  
```txt  
Out[192]: 0  
```  
  
```python  
In [193]: expr = tb.Expr('3 * sin(ear) + sqrt(abs(ear))') # ❶ 把一个基于 str 对象的运算表达式转化为 Expr 对象。  
  
In [194]: expr.set_output(out, append_mode=True) # ❷ 将输出结果定义为一个名为 out 的 EArray 对象。  
  
In [195]: %time expr.eval() # ❸ 初始化表达式的评估操作。  
```  
  
```txt  
 CPU times: user 3.08 s, sys: 1.7 s, total: 4.78 s  
 Wall time: 4.03 s  
Out[195]: /out (EArray(375000, 500)) ''  
   atom := Float64Atom(shape=(), dflt=0.0)  
   maindim := 0  
   flavor := 'numpy'  
   byteorder := 'little'  
   chunkshape := (16, 500)  
```  
  
```python  
In [196]: out.size_on_disk  
```  
  
```txt  
Out[196]: 1500032000  
```  
  
```python  
In [197]: out[0, :10]  
```  
  
```txt  
Out[197]: array([-1.73369462,  3.74824436,  0.90627898,  2.86786818,  
         1.75424957,  
        -0.91108973, -1.68313885,  1.29073295, -1.68665599, -1.71345309])  
```  
  
```python  
In [198]: %time out_ = out.read() # ❹ 把完整的 EArray 读入内存。  
```  
  
```txt  
 CPU times: user 1.03 s, sys: 1.1 s, total: 2.13 s  
 Wall time: 2.22 s  
```  
  
```python  
In [199]: out_[0, :10]  
```  
  
```txt  
Out[199]: array([-1.73369462,  3.74824436,  0.90627898,  2.86786818,  
         1.75424957,  
        -0.91108973, -1.68313885,  1.29073295, -1.68665599, -1.71345309])  
```  
  
鉴于所有的计算工作都是在内存外进行的，其执行速度算是非常的快了，更何况它还仅仅是在一台拥有标准化配置的普通硬件上执行出来的结果。作为对比参考，在内存内通过调用 `numexpr` 模块执行的结果（也可见于第 10 章）可以用来进行一下比对。后者的处理速度自然是会更快一些的，但也并没有展现出什么惊人的优势：  
  
```python  
In [200]: import numexpr as ne # ❶ 导入一个专门提供在内存中运行那些涉及复杂数学表达式功能模块。  
  
In [201]: expr = '3 * sin(out_) + sqrt(abs(out_))' # ❷ 要被计算的数学表达式，被包裹在一个 str 对象之中。  
  
In [202]: ne.set_num_threads(1) # ❸ 把所用的线程数量设定为一。  
```  
  
```txt  
Out[202]: 4  
```  
  
```python  
In [203]: %time ne.evaluate(expr)[0, :10] # ❹ 当在执行内存计算数学表达式评估的任务时使用了单线程进行计算。  
```  
  
```txt  
 CPU times: user 2.51 s, sys: 1.54 s, total: 4.05 s  
 Wall time: 4.94 s  
```  
  
```txt  
Out[203]: array([-1.64358578,  0.22567882,  3.31363043,  2.50443549,  
         4.27413965,  
        -1.41600606, -1.68373023,  4.01921805, -1.68117412, -1.66053597])  
```  
  
```python  
In [204]: ne.set_num_threads(4) # ❺ 把所需的线程数量设为四。  
```  
  
```txt  
Out[204]: 1  
```  
  
```python  
In [205]: %time ne.evaluate(expr)[0, :10] # ❻ 开启四个线程针对这道在内存里的复杂数值运算应用执行评估计算。  
```  
  
```txt  
 CPU times: user 3.39 s, sys: 1.94 s, total: 5.32 s  
 Wall time: 2.96 s  
```  
  
```txt  
Out[205]: array([-1.64358578,  0.22567882,  3.31363043,  2.50443549,  
         4.27413965,  
        -1.41600606, -1.68373023,  4.01921805, -1.68117412, -1.66053597])  
```  
  
```python  
In [206]: h5.close()  
  
In [207]: !rm -f $path*  
```  
  
## 使用 TsTables 进行 I/O  
  
`TsTables` 库利用 PyTables 开发出了针对处理时间序列数据的性能优异的数据存储处理工具。其主要的一个运用应用场情是所谓的“一次写好，在之后便能够支持无限次数被随手读取利用”。这一需求可以说是很多被归在金融分析大类当中的非常具备特性的一个应用场景了，那就是数据一旦产生就能够随时从市场中、或者是依照不同步的方式采集而来，被放存在磁盘以便应对将来使用的调用。这一应用的场景经常可以是在面对一套架构相对复杂的对某个商业模型执行验证性质的工作时使用到，那时，一个有着时间戳特性的财务类数据序列资料库经常就需要被拿来对其中的各个不同时期内存在的部分一遍又一遍地提取利用。这也就是说使得能够被极快速度读取的数据提取表现变得非常地具着至关重要性了。  
  
### 样本数据  
  
一如既往地，这最起头的第一个需要去完成的事情自然就是制造一整套规模足够被拿来用以解释 `TsTables` 的强项的虚拟数据。跟随其后的这样一小段代码借助在采用以几何布朗运动的手段基础下进行数据仿真，制作出包含了三个表现长度的金融类带有时间序列特性的数值系列表单：  
  
```python  
In [208]: no = 5000000 # ❶ 时间序列的步长数目。  
          co = 3 # ❷ 时间序列表的序列数据线列的数量。  
          interval = 1. / (12 * 30 * 24 * 60) # ❸ 把一个单位时间长度转为由一段段的年构成的一组分数数据单位序列。  
          vol = 0.2 # ❹ 金融波动参数设定。  
  
In [209]: %%time  
          rn = np.random.standard_normal((no, co)) # ❺ 生成标准的自然态下随机常态分布排列数列数据。  
          rn[0] = 0.0 # ❻ 把所有的初始随机数归零重新做设定。  
          paths = 100 * np.exp(np.cumsum(-0.5 * vol ** 2 * interval +  
                               vol * np.sqrt(interval) * rn, axis=0)) # ❼ 这里产生的一个以基于带有某种离散状态运算基础之上的仿真数列模型。  
          paths[0] = 100 # ❽ 设定这一系列的路径参数初始位均定在了100。  
```  
  
```txt  
 CPU times: user 869 ms, sys: 175 ms, total: 1.04 s  
 Wall time: 812 ms  
```  
  
由于发现这一个名为 `TsTables` 的包配合在调用使用了那些来自于 `pandas` 所产生出的 `DataFrame` 对象作为配合上表现得异常优异，故而这一套产生完成的资料库便将被转化成这么一种特殊数据处理对象的形貌格式被运用（参见下面的这个图片示意 图9-7）：  
  
```python  
In [210]: dr = pd.date_range('2019-1-1', periods=no, freq='1s')  
  
In [211]: dr[-6:]  
```  
  
```txt  
Out[211]: DatetimeIndex(['2019-02-27 20:53:14', '2019-02-27 20:53:15',  
               '2019-02-27 20:53:16', '2019-02-27 20:53:17',  
               '2019-02-27 20:53:18', '2019-02-27 20:53:19'],  
              dtype='datetime64[ns]', freq='S')  
```  
  
```python  
In [212]: df = pd.DataFrame(paths, index=dr, columns=['ts1', 'ts2', 'ts3'])  
  
In [213]: df.info()  
```  
  
```txt  
 <class 'pandas.core.frame.DataFrame'>  
 DatetimeIndex: 5000000 entries, 2019-01-01 00:00:00 to 2019-02-27  
 20:53:19  
 Freq: S  
 Data columns (total 3 columns):  
 ts1    float64  
 ts2    float64  
 ts3    float64  
 dtypes: float64(3)  
 memory usage: 152.6 MB  
```  
  
```python  
In [214]: df.head()  
```  
  
```txt  
Out[214]:                             ts1         ts2         ts3  
 2019-01-01 00:00:00  100.000000  100.000000  100.000000  
 2019-01-01 00:00:01  100.018443   99.966644   99.998255  
 2019-01-01 00:00:02  100.069023  100.004420   99.986646  
 2019-01-01 00:00:03  100.086757  100.000246   99.992042  
 2019-01-01 00:00:04  100.105448  100.036033   99.950618  
```  
  
```python  
In [215]: df[::100000].plot(figsize=(10, 6));  
```  
  
<img width="666" height="457" alt="python_for_finance 9 7" src="https://github.com/user-attachments/assets/e66fdda7-eab4-427e-a651-0c3bcd66b73c" />
  
图 9-7. 此一金融数字带有时间性质的序列化表现分布取点标的制图示意图  
  
### 数据存储  
  
`TsTables` 这个工具包基于一套使用块作为区分标准的专属独特构造的方式来储存具备时间特性的序列性财务数据资料，所以它可以支持用户能根据不同的时段选取所定范围以实现任意提取对应的一组具备分段性质数据内容的超高速截取表现效果。出于这样的目的它便在自己的里面为调用使用 `PyTables` 功能的过程中加设配置好了一个被起名为 `create_ts()` 的特定函数。在这个基础上接下来的这一步演示的内容就拿一套依托于来自被存留在 `PyTables` 这个模块中所设定的一套关于如何设定这个类被称为 `tb.IsDescription` 进行表内各列项目分别使用格式定义工作进行表项目创建处理操作：  
  
```python  
In [216]: import tstables as tstab  
  
In [217]: class ts_desc(tb.IsDescription):  
              timestamp = tb.Int64Col(pos=0) # ❶ 用来作为时间标识符存放的列设定。   
              ts1 = tb.Float64Col(pos=1) # ❷ 三列用于存放以数字呈现数据的定义列。   
              ts2 = tb.Float64Col(pos=2) # ❷ 三列用于存放以数字呈现数据的定义列。  
              ts3 = tb.Float64Col(pos=3) # ❷ 三列用于存放以数字呈现数据的定义列。  
  
In [218]: h5 = tb.open_file(path + 'tstab.h5', 'w') # ❸ 开启用一套名叫作 HDF5 的用于处理数据库系统形式的文件执行书写写入的操作功能。   
  
In [219]: ts = h5.create_ts('/', 'ts', ts_desc) # ❹ 这一个过程所生成的是通过由使用上面定义好的一个有着 ts_desc 的数据格式特性的名叫 TsTable 这个特殊的管理数据呈现形式物件。   
  
In [220]: %time ts.append(df) # ❺ 然后再向在刚刚产生出来的叫做 TsTable 的那个里面追加那些一开始被保留装载到名叫 DataFrame 中的所有那些资料的。   
```  
  
```txt  
 CPU times: user 1.36 s, sys: 497 ms, total: 1.86 s  
 Wall time: 1.29 s  
```  
  
```python  
In [221]: type(ts)  
```  
  
```txt  
Out[221]: tstables.tstable.TsTable  
```  
  
```python  
In [222]: ls -n $path  
```  
  
```txt  
 total 328472  
 -rw-r--r--  1 501  20  157037368 Oct 19 12:13 tstab.h5  
```  
  
### 数据检索提取  
  
既然说借助于利用好这个被起名为 `TsTables` 的这一工具在向当中做数据资料执行书写写入这个环节在执行操作的表现速度毫无疑问极其优秀了的话，虽然说速度表现它仍然还是被依赖到机器硬体的规格等级上才能够获得表现出来的。然而，这就说明使用这样的方式它在用于想要读写截取位于资料当中的各个被分别划好区分区块中的被取部分重新导送进入运行记忆体内的环节表现自然同样也就是快到没话说的一种极致程度了。非常简单地方便使用的就是这个在运用过它所产生的 `TsTables` 可以通过处理返回以一个名为 `DataFrame` 当对象的返回（同样可以见诸于图片 9-8）：  
  
```python  
In [223]: read_start_dt = dt.datetime(2019, 2, 1, 0, 0) # ❶ 这便是用来起取在限定区域时间内那个开头位置时点的。  
          read_end_dt = dt.datetime(2019, 2, 5, 23, 59) # ❷ 同上在作为定义所取间隔位置的这个就是它的终止截止结束时间。   
  
In [224]: %time rows = ts.read_range(read_start_dt, read_end_dt) # ❸ 在其使用被作为返回一个被指明包含在那被限制范围在区内的一个由处理产生的称呼为 DataFrame 当作结果的返回方法是借用这一条以叫做 ts.read_range() 被做命名的。   
```  
  
```txt  
 CPU times: user 182 ms, sys: 73.5 ms, total: 255 ms  
 Wall time: 163 ms  
```  
  
```python  
In [225]: rows.info() # ❹ 该返回的生成产物就是一个囊括含包含了有一百甚至以千及至在到了上万行列级的数据数列的称呼作为名叫 DataFrame 如此对象的这一个结果来。   
```  
  
```txt  
 <class 'pandas.core.frame.DataFrame'>  
 DatetimeIndex: 431941 entries, 2019-02-01 00:00:00 to 2019-02-05  
 23:59:00  
 Data columns (total 3 columns):  
 ts1    431941 non-null float64  
 ts2    431941 non-null float64  
 ts3    431941 non-null float64  
 dtypes: float64(3)  
 memory usage: 13.2 MB  
```  
  
```python  
In [226]: rows.head()  
```  
  
```txt  
Out[226]:                             ts1         ts2         ts3  
 2019-02-01 00:00:00   52.063640   40.474580  217.324713  
 2019-02-01 00:00:01   52.087455   40.471911  217.250070  
 2019-02-01 00:00:02   52.084808   40.458013  217.228712  
 2019-02-01 00:00:03   52.073536   40.451408  217.302912  
 2019-02-01 00:00:04   52.056133   40.450951  217.207481  
```  
  
```python  
In [227]: h5.close()  
  
In [228]: (rows[::500] / rows.iloc[0]).plot(figsize=(10, 6));  
```  
  
<img width="664" height="445" alt="python_for_finance 9 8" src="https://github.com/user-attachments/assets/09b94131-1210-45e8-9b77-c4aaee3f31d0" />
  
图 9-8. 将这一含有金融时段在特别区间选取时间被取出这以标准化的一截时段制图的分布示意图。  
  
如果要想能能将关于基于对被命名了叫作 `TsTables` 的这么一个做抽取并抓取出一段截取自被处理的数据这在作为处理执行这个部分能力上取得的表现能够以更显明地表达清楚，则在这里可以以一个采用了拿相当于连续被设定是为 3 天时间的，其在内容上的每一次跳动都是 1 秒这样高刷新规格的由足足有 100 个这样的段截被抽出组合起来的内容当作对它能力水平考量比较基准。而在这一次关于它的数据被截取的工作被完成以后，提取这样一个能够拥包含了以拥有达有 345,600 行数据内容名为叫 `DataFrame` 数据资料却仅只连十分之一秒都不用的超绝提取提取读取运算耗时表现了：  
  
```python  
In [229]: import random  
  
In [230]: h5 = tb.open_file(path + 'tstab.h5', 'r')  
  
In [231]: ts = h5.root.ts._f_get_timeseries() # ❶ 这里则负责能够直接建立到和这个产生叫做 TsTable 的这样一个对象做接触读取获取动作的操作。   
  
In [232]: %%time  
          for _ in range(100): # ❷ 这个把进行提取检索的工作又一遍又遍多次再不停进行下去。   
              d = random.randint(1, 24) # ❸ 而把那选取被做开端的开始值都采取用着完全不确定以随意抓到的形式的被弄做了。   
              read_start_dt = dt.datetime(2019, 2, d, 0, 0, 0)  
              read_end_dt = dt.datetime(2019, 2, d + 3, 23, 59, 59)  
              rows = ts.read_range(read_start_dt, read_end_dt)  
```  
  
```txt  
 CPU times: user 7.17 s, sys: 1.65 s, total: 8.81 s  
 Wall time: 4.78 s  
```  
  
```python  
In [233]: rows.info() # ❹ 把所有处理完成后获取在最为结果返回获取那个最终获取这个名字有着产生出一个命名做为是叫做叫 DataFrame 的东西对象被弄出来呈现出了。  
```  
  
```txt  
 <class 'pandas.core.frame.DataFrame'>  
 DatetimeIndex: 345600 entries, 2019-02-04 00:00:00 to 2019-02-07  
 23:59:59  
 Data columns (total 3 columns):  
 ts1    345600 non-null float64  
 ts2    345600 non-null float64  
 ts3    345600 non-null float64  
 dtypes: float64(3)  
 memory usage: 10.5 MB  
```  
  
```python  
In [234]: !rm $path/tstab.h5  
```  
  
## 结论  
  
当涉及到展现单个对象/表之间大量关系的复杂数据结构时，基于 SQL 或关系型数据库具有优势。在某些情况下，这或许能证明其相对于纯粹基于 NumPy `ndarray` 或基于 pandas `DataFrame` 的方法在性能上处于劣势是合理的。  
  
总体而言，在金融或科学领域的许多应用中，主要基于数组的数据建模方法能够取得成功。在这些情况下，通过利用原生 NumPy 的 I/O 功能、NumPy 与 PyTables 的组合功能，或通过基于 HDF5 存储的 pandas 方法，可以实现巨大的性能提升。在处理大型（金融）时间序列数据集时，特别是在“一次写入，多次检索”的场景中，TsTables 尤其有用。  
  
尽管最近的趋势是使用基于云的解决方案——云是由大量基于商品硬件的计算节点组成的——但人们应该仔细考虑，尤其是在金融环境中，哪种硬件架构最能满足分析需求。微软的一项研究为这个话题提供了一些启示：  
  
> 我们声称，一台横向扩展（“scale-up”）的单一服务器可以处理所有这些作业，并且在性能、成本、功耗和服务器密度方面，其表现等同甚至优于一个集群。  
> ——Appuswamy 等人（2013）  
  
因此，参与数据分析的公司、研究机构等应首先分析总体上必须完成哪些具体任务，然后再根据以下方面决定硬件/软件架构：  
  
横向扩展（Scaling out）  
使用一个由许多带有标准 CPU 和相对较低内存的商品节点组成的集群  
  
纵向扩展（Scaling up）  
使用一台或几台强大的服务器，配备多核 CPU（在涉及机器学习和深度学习时可能还配备 GPU 甚至 TPU），以及大容量内存  
  
纵向扩展硬件并采用合适的实施方法可能会显着影响性能，这是下一章的重点。  
  
## 进一步的资源  
  
本章开头和结尾引用的论文是一篇好文章，也是思考金融分析硬件架构的一个很好的起点：  
  
* Appuswamy, Raja 等人（2013）。“Nobody Ever Got Fired for Buying a Cluster”。微软技术报告。  
  
像往常一样，互联网提供了大量关于本章涵盖的主题和 Python 包的有价值的资源：  
  
* 关于使用 `pickle` 序列化 Python 对象，请参阅文档。  
* NumPy I/O 功能的概述可在其网站上找到。  
* 关于使用 pandas 进行 I/O，请参阅在线文档中的相应部分。  
* PyTables 主页提供了教程和详细文档。  
* 有关 TsTables 的更多信息，请访问其 GitHub 页面。  
  
可以在 http://github.com/yhilpisch/tstables 找到一个关于 TsTables 的友好分叉（fork）。使用 `pip install git+git://github.com/yhilpisch/tstables` 从该分叉安装包，该分叉的维护是为了兼容较新版本的 pandas 和其他 Python 包。  
