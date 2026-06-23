# 第12章 随机学  
  
> 预测性不是事情将如何发展，而是事情可以如何发展。  
> ——拉希尔·法鲁克 (Raheel Farooq)  
  
如今，随机学是金融学中最重要的数学和数值学科之一。在现代金融时代的初期（主要是在20世纪70年代和80年代），金融研究的主要目标是为特定的金融模型提出闭式解，例如期权价格。近年来，要求发生了极大的变化，不仅对单一金融工具的正确定价对金融市场参与者很重要，而且对整个衍生品簿的一致性定价也很重要。同样，为了在整个金融机构中得出一致的风险衡量标准（如风险价值和信用估值调整），需要考虑到该机构及其所有交易对手的整个簿本。这类艰巨的任务只能通过灵活高效的数值方法来解决。因此，一般的随机学特别是蒙特卡洛模拟在金融领域声名鹊起。  
  
本章从Python的角度介绍以下主题：  
  
“随机数”（第346页）  
一切从伪随机数开始，伪随机数构成了所有模拟工作的基础；尽管准随机数（例如基于Sobol序列）在金融界获得了一些普及，但伪随机数似乎仍然是基准。  
  
“模拟”（第352页）  
在金融学中，两项模拟任务尤为重要：随机变量模拟和随机过程模拟。  
  
“估值”（第375页）  
涉及估值的两大主要学科是对具有欧式行权（在特定日期）和美式行权（在特定时间区间内）的衍生品进行估值；此外还有具有百慕大行权或在一组有限的特定日期行权的工具。  
  
“风险测度”（第383页）  
模拟非常适合计算风险价值、信用风险价值和信用估值调整等风险指标。  
  
## 随机数  
  
在整个本章中，为了生成随机数（1 为了简单起见，我们将谈论随机数，同时知道使用的所有数字都将是伪随机的。），使用了 `numpy.random` 子包提供的函数：  
  
```python  
import math  
import numpy as np  
import numpy.random as npr # ❶ 导入NumPy中的随机数生成子包。  
from pylab import plt, mpl  
  
plt.style.use('seaborn')  
mpl.rcParams['font.family'] = 'serif'  
%matplotlib inline  
```  
  
例如， `rand()` 函数从开区间 $[0,1)$ 中返回以函数参数形式提供的形状的随机数。返回的对象是一个 `ndarray` 对象。这样的数字很容易进行转换，以涵盖实线的其他区间。例如，如果希望从区间 $[a,b)=[5,10)$ 生成随机数，可以像下一个例子那样转换 `npr.rand()` 返回的数字——由于NumPy广播机制，这在多维情况下同样适用：  
  
```python  
npr.seed(100) # ❶ 固定种子值以确保可重现性，并固定打印输出的数字位数。  
np.set_printoptions(precision=4) # ❶ 固定种子值以确保可重现性，并固定打印输出的数字位数。  
  
npr.rand(10) # ❷ 均匀分布的随机数作为一维 ndarray 对象。  
```  
  
```txt  
Out[4]: array([0.5434, 0.2784, 0.4245, 0.8448, 0.0047, 0.1216, 0.6707, 0.8259,  
 0.1367, 0.5751])  
```  
  
```python  
npr.rand(5, 5) # ❸ 均匀分布的随机数作为二维 ndarray 对象。  
```  
  
```txt  
Out[5]: array([[0.8913, 0.2092, 0.1853, 0.1084, 0.2197],  
 [0.9786, 0.8117, 0.1719, 0.8162, 0.2741],  
 [0.4317, 0.94  , 0.8176, 0.3361, 0.1754],  
 [0.3728, 0.0057, 0.2524, 0.7957, 0.0153],  
 [0.5988, 0.6038, 0.1051, 0.3819, 0.0365]])  
```  
  
```python  
a = 5. # ❹ 下限...  
b = 10. # ❺ ...及上限...  
npr.rand(10) * (b - a) + a # ❻ ...用于转换为另一个区间。  
```  
  
```txt  
Out[6]: array([9.4521, 9.9046, 5.2997, 9.4527, 7.8845, 8.7124, 8.1509, 7.9092,  
 5.1022, 6.0501])  
```  
  
```python  
npr.rand(5, 5) * (b - a) + a # ❼ 针对二维的相同转换。  
```  
  
```txt  
Out[7]: array([[7.7234, 8.8456, 6.2535, 6.4295, 9.262 ],  
 [9.875 , 9.4243, 6.7975, 7.9943, 6.774 ],  
 [6.701 , 5.8904, 6.1885, 5.2243, 7.5272],  
 [6.8813, 7.964 , 8.1497, 5.713 , 9.6692],  
 [9.7319, 8.0115, 6.9388, 6.8159, 6.0217]])  
```  
  
表 12-1 列出了用于生成简单随机数的函数。  
  
<img width="640" height="328" alt="python_for_finance 12 1 1" src="https://github.com/user-attachments/assets/0d854e3d-5ecf-4846-a045-57145c31dd6d" />
  
可以直接对表 12-1 中选定函数生成的一些随机抽取进行可视化。图 12-1 以图形方式展示了两个连续分布和两个离散分布的结果：  
  
```python  
sample_size = 500  
rn1 = npr.rand(sample_size, 3) # ❶ 均匀分布的随机数。  
rn2 = npr.randint(0, 10, sample_size) # ❷ 给定区间的随机整数。  
rn3 = npr.sample(size=sample_size) # ❶ 均匀分布的随机数。  
a = [0, 25, 50, 75, 100]   
rn4 = npr.choice(a, size=sample_size) # ❸ 从有限列表对象中随机抽样值。  
  
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(nrows=2, ncols=2,  
                                             figsize=(10, 8))  
  
ax1.hist(rn1, bins=25, stacked=True)  
ax1.set_title('rand')  
ax1.set_ylabel('frequency')  
ax2.hist(rn2, bins=25)  
ax2.set_title('randint')  
ax3.hist(rn3, bins=25)  
ax3.set_title('sample')  
ax3.set_ylabel('frequency')  
ax4.hist(rn4, bins=25)  
ax4.set_title('choice');  
```  
  
<img width="668" height="551" alt="python_for_finance 12 1" src="https://github.com/user-attachments/assets/8fa5c7ad-32a6-4bf5-bb5a-3384f8862a94" />
  
图 12-1. 简单随机数的直方图  
  
表 12-2 列出了根据不同分布规律生成随机数的函数。  
  
<img width="664" height="1128" alt="python_for_finance 12 2 1" src="https://github.com/user-attachments/assets/dd8e7e7c-a41c-4cb7-81a0-9f0a54a9291a" />
  
尽管在金融领域使用（标准）正态分布存在许多批评，但它们是不可或缺的工具，仍然是分析和数值应用中最广泛使用的分布类型。原因之一是许多金融模型直接以某种方式依赖于正态分布或对数正态分布。另一个原因是，许多不直接依赖于（对数）正态假设的金融模型可以通过使用正态分布进行离散化，从而近似用于模拟目的。  
  
作为说明，图 12-2 可视化了从以下分布中随机抽取的结果：  
  
* 均值为 0、标准差为 1 的标准正态分布  
* 均值为 100、标准差为 20 的正态分布  
* 自由度为 0.5 的卡方分布  
* $ \lambda $ 为 1 的泊松分布  
  
图 12-2 显示了三个连续分布和一个离散分布（泊松分布）的结果。例如，泊松分布用于模拟（罕见）外部事件的到来，如金融工具价格的跳跃或外生冲击。以下是生成它的代码：  
  
```python  
sample_size = 500  
rn1 = npr.standard_normal(sample_size) # ❶ 标准正态分布的随机数。  
rn2 = npr.normal(100, 20, sample_size) # ❷ 正态分布的随机数。  
rn3 = npr.chisquare(df=0.5, size=sample_size) # ❸ 卡方分布的随机数。  
rn4 = npr.poisson(lam=1.0, size=sample_size) # ❹ 泊松分布的随机数。  
  
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(nrows=2, ncols=2,  
                                             figsize=(10, 8))  
  
ax1.hist(rn1, bins=25)  
ax1.set_title('standard normal')  
ax1.set_ylabel('frequency')  
ax2.hist(rn2, bins=25)  
ax2.set_title('normal(100, 20)')  
ax3.hist(rn3, bins=25)  
ax3.set_title('chi square')  
ax3.set_ylabel('frequency')  
ax4.hist(rn4, bins=25)  
ax4.set_title('Poisson');  
```  
  
<img width="665" height="543" alt="python_for_finance 12 2" src="https://github.com/user-attachments/assets/29a80cae-8cf7-493a-9076-49e75c3f359b" />
  
图 12-2. 不同分布随机样本的直方图  
  
**NumPy 和随机数**  
  
本节表明，在 Python 中生成伪随机数时，NumPy 是一个强大（甚至不可或缺）的工具。使用此类数字创建小型或大型 `ndarray` 对象不仅方便，而且性能卓越。  
  
## 模拟  
  
蒙特卡洛模拟 (MCS) 是金融领域最重要的数值技术之一，如果不是最重要和最广泛使用的技术的话。这主要源于这样一个事实：在评估数学表达式（例如积分），特别是评估金融衍生品时，它是最灵活的数值方法。然而，这种灵活性是以相对较高的计算负担为代价的，因为通常必须执行数十万甚至数百万次复杂计算才能得出一个单一的价值估计。  
  
### 随机变量  
  
以 Black-Scholes-Merton 期权定价模型为例。在他们的设定中，给定今天的水平 $S_0$ ，在未来日期 $T$ 的股票指数水平 $S_T$ 根据公式 12-1 给出。  
  
公式 12-1. 在 Black-Scholes-Merton 设定中模拟未来指数水平  
$$S_T=S_0\exp\left(\left(r-\frac{1}{2}\sigma^2\right)T+\sigma\sqrt{T}z\right)$$  
  
变量和参数具有以下含义：  
  
$S_T$  
在日期 $T$ 的指数水平  
  
$r$  
恒定的无风险短期利率  
  
$\sigma$  
$S$ 的恒定波动率（= 收益率的标准差）  
  
$z$  
标准正态分布随机变量  
  
该金融模型被参数化并模拟如下。此模拟代码的输出如图 12-3 所示：  
  
```python  
S0 = 100 # ❶ 初始指数水平。  
r = 0.05 # ❷ 恒定无风险短期利率。  
sigma = 0.25 # ❸ 恒定波动率因子。  
T = 2.0 # ❹ 以年为单位的期限。  
I = 10000 # ❺ 模拟次数。  
ST1 = S0 * np.exp((r - 0.5 * sigma ** 2) * T +  
                  sigma * math.sqrt(T) * npr.standard_normal(I)) # ❻ 模拟本身通过向量化表达式进行；离散化方案利用了 npr.standard_normal() 函数。  
  
plt.figure(figsize=(10, 6))  
plt.hist(ST1, bins=50)  
plt.xlabel('index level')  
plt.ylabel('frequency');  
```  
  
<img width="665" height="452" alt="python_for_finance 12 3" src="https://github.com/user-attachments/assets/edab2c8e-88e1-437f-bfc7-2dcdcb4af22c" />
  
图 12-3. 静态模拟的几何布朗运动（通过 `npr.standard_normal()`）  
  
图 12-3 表明公式 12-1 中定义的随机变量的分布是对数正态分布。因此，人们也可以尝试使用 `npr.lognormal()` 函数直接导出随机变量的值。在这种情况下，必须向函数提供均值和标准差：  
  
```python  
ST2 = S0 * npr.lognormal((r - 0.5 * sigma ** 2) * T,  
                         sigma * math.sqrt(T), size=I) # ❶ 模拟通过向量化表达式进行；离散化方案利用了 npr.lognormal() 函数。  
  
plt.figure(figsize=(10, 6))  
plt.hist(ST2, bins=50)  
plt.xlabel('index level')  
plt.ylabel('frequency');  
```  
  
结果如图 12-4 所示。  
  
<img width="666" height="430" alt="python_for_finance 12 4" src="https://github.com/user-attachments/assets/bec7b13a-784d-4f3a-a7b3-6ac0bf901228" />
  
图 12-4. 静态模拟的几何布朗运动（通过 `npr.lognormal()`）  
  
通过目测，图 12-3 和 12-4 确实看起来非常相似。这可以通过比较结果分布的统计矩来进行更严格的验证。为了比较模拟结果的分布特征， `scipy.stats` 子包和此处定义的辅助函数 `print_statistics()` 被证明非常有用：  
  
```python  
import scipy.stats as scs  
  
def print_statistics(a1, a2):  
    ''' 打印选定的统计数据。  
      
    参数  
    ==========  
    a1, a2: ndarray 对象  
        模拟结果对象  
    '''  
    sta1 = scs.describe(a1) # ❶ scs.describe() 函数返回数据集的重要统计信息。  
    sta2 = scs.describe(a2) # ❶  
    print('%14s %14s %14s' % ('statistic', 'data set 1', 'data set 2'))  
    print(45 * "-")  
    print('%14s %14.3f %14.3f' % ('size', sta1[0], sta2[0]))  
    print('%14s %14.3f %14.3f' % ('min', sta1[1][0], sta2[1][0]))  
    print('%14s %14.3f %14.3f' % ('max', sta1[1][1], sta2[1][1]))  
    print('%14s %14.3f %14.3f' % ('mean', sta1[2], sta2[2]))  
    print('%14s %14.3f %14.3f' % ('std', np.sqrt(sta1[3]),  
                                  np.sqrt(sta2[3])))  
    print('%14s %14.3f %14.3f' % ('skew', sta1[4], sta2[4]))  
    print('%14s %14.3f %14.3f' % ('kurtosis', sta1[5], sta2[5]))  
  
print_statistics(ST1, ST2)  
```  
  
```txt  
      statistic     data set 1     data set 2  
---------------------------------------------  
           size      10000.000      10000.000  
            min         32.327         28.230  
            max        414.825        409.110  
           mean        110.730        110.431  
            std         40.300         39.878  
           skew          1.122          1.115  
       kurtosis          2.438          2.217  
```  
  
显然，两种模拟结果的统计数据非常相似。差异主要是由于模拟中的采样误差所致。当对连续随机过程进行离散模拟时，还可能引入另一种误差——即离散化误差，但由于此处采用静态模拟方法，该误差在这里没有影响。  
  
### 随机过程  
  
粗略地说，随机过程是一系列随机变量。从这个意义上说，在模拟过程时，人们应该期待类似于随机变量重复模拟序列的东西。这在很大程度上是正确的，除了抽取通常不是独立的，而是依赖于前一次或前几次抽取的结果。然而，一般来说，金融中使用的随机过程表现出马尔可夫性，其主要含义是过程明天的值仅取决于过程今天的状态，而不取决于任何其他更“历史”的状态甚至整个路径历史。因此，该过程也被称为无记忆的。  
  
**几何布朗运动**  
  
现在考虑动态形式的 Black-Scholes-Merton 模型，如公式 12-2 中的随机微分方程 (SDE) 所示。这里， $Z_t$ 是标准布朗运动。该 SDE 称为几何布朗运动。 $S_t$ 的值呈对数正态分布，而（边际）收益率 $\frac{dS_t}{S_t}$ 呈正态分布。  
  
公式 12-2. Black-Scholes-Merton 设定中的随机微分方程  
$$dS_t=rS_tdt+\sigma S_tdZ_t$$  
  
公式 12-2 中的 SDE 可以通过欧拉格式精确离散化。公式 12-3 展示了这种格式，其中 $\Delta t$ 是固定的离散化区间，且 $z_t$ 是一个标准正态分布的随机变量。  
  
公式 12-3. 在 Black-Scholes-Merton 设定中动态模拟指数水平  
$$S_t=S_{t-\Delta t}\exp\left(\left(r-\frac{1}{2}\sigma^2\right)\Delta t+\sigma\sqrt{\Delta t}z_t\right)$$  
  
和以前一样，将其转换为 Python 和 NumPy 代码非常简单。最终得出的指数水平值再次呈对数正态分布，如图 12-5 所示。前四个矩也非常接近静态模拟方法产生的结果：  
  
```python  
I = 10000 # ❶ 要模拟的路径数。  
M = 50 # ❷ 离散化的时间区间数。  
dt = T / M # ❸ 以年为单位的时间区间长度。  
S = np.zeros((M + 1, I)) # ❹ 用于指数水平的二维 ndarray 对象。  
S[0] = S0 # ❺ 时间 t=0 时初始点的初始值。  
for t in range(1, M + 1):  
    S[t] = S[t - 1] * np.exp((r - 0.5 * sigma ** 2) * dt +  
                             sigma * math.sqrt(dt) * npr.standard_normal(I)) # ❻ 通过半向量化表达式进行模拟；循环遍及从 t=1 开始到 t=T 结束的时间点。  
  
plt.figure(figsize=(10, 6))  
plt.hist(S[-1], bins=50)  
plt.xlabel('index level')  
plt.ylabel('frequency');  
```  
  
<img width="664" height="425" alt="python_for_finance 12 5" src="https://github.com/user-attachments/assets/30f4ca1b-d452-4af7-b7d7-9a3ae34d588c" />
  
图 12-5. 到期时动态模拟的几何布朗运动  
  
以下是对动态模拟和静态模拟产生的统计数据的比较。图 12-6 显示了前 10 条模拟路径：  
  
```python  
print_statistics(S[-1], ST2)  
```  
  
```txt  
      statistic     data set 1     data set 2  
---------------------------------------------  
           size      10000.000      10000.000  
            min         27.746         28.230  
            max        382.096        409.110  
           mean        110.423        110.431  
            std         39.179         39.878  
           skew          1.069          1.115  
       kurtosis          2.028          2.217  
```  
  
```python  
plt.figure(figsize=(10, 6))  
plt.plot(S[:, :10], lw=1.5)  
plt.xlabel('time')  
plt.ylabel('index level');  
```  
  
<img width="664" height="430" alt="python_for_finance 12 6" src="https://github.com/user-attachments/assets/c969c2b3-0501-4653-ac0f-37444e642946" />
  
图 12-6. 动态模拟的几何布朗运动路径  
  
使用动态模拟方法不仅可以让我们可视化如图 12-6 所示的路径，而且还可以对具有美式/百慕大行权的期权或其收益依赖于路径的期权进行估值。可以说，这提供了一幅随时间变化的完整动态图景。  
  
**平方根扩散**  
  
另一类重要的金融过程是均值回归过程，例如用于对短期利率或波动率过程进行建模。Cox、Ingersoll 和 Ross (1985) 提出的平方根扩散模型就是一个广受欢迎且被广泛应用的模型。公式 12-4 提供了相应的 SDE。  
  
公式 12-4. 平方根扩散的随机微分方程  
$$dx_t=\kappa(\theta-x_t)dt+\sigma\sqrt{x_t}dZ_t$$  
  
变量和参数具有以下含义：  
  
$x_t$  
在日期 $t$ 的过程水平  
  
$\kappa$  
均值回归因子  
  
$\theta$  
过程的长期均值  
  
$\sigma$  
恒定波动率参数  
  
$Z_t$  
标准布朗运动  
  
众所周知， $x_t$ 的值服从卡方分布。然而，如前所述，许多金融模型可以通过使用正态分布进行离散化和近似（即所谓的欧拉离散化格式）。虽然欧拉格式对于几何布朗运动是精确的，但它对于大多数其他随机过程是有偏的。即使有一个精确的格式可用——稍后将介绍平方根扩散的一种精确格式——出于数值和/或计算方面的原因，使用欧拉格式可能是合乎需要的。定义 $s=t-\Delta t$ 和 $x^+\equiv\max(x,0)$ ，公式 12-5 展示了这样一种欧拉格式。这种特定的格式在文献中通常被称为完全截断（更多细节和其他格式请参见 Hilpisch (2015)）。  
  
公式 12-5. 平方根扩散的欧拉离散化  
$$\tilde{x}_t=\tilde{x}_s+\kappa(\theta-\tilde{x}_s^+)\Delta t+\sigma\sqrt{\tilde{x}_s^+}\sqrt{\Delta t}z_t$$  
$$x_t=\tilde{x}_t^+$$  
  
平方根扩散具有一个方便且现实的特征，即 $x_t$ 的值始终保持严格为正。当用欧拉格式对其进行离散化时，不能排除负值。这就是为什么人们始终使用原始模拟过程的正值版本的原因。因此，在模拟代码中，需要两个 `ndarray` 对象，而不是仅仅一个。图 12-7 以直方图的形式图形化地展示了模拟结果：  
  
```python  
x0 = 0.05 # ❶ 初始值（例如，对于短期利率）。  
kappa = 3.0 # ❷ 均值回归因子。  
theta = 0.02 # ❸ 长期均值。  
sigma = 0.1 # ❹ 波动率因子。  
I = 10000  
M = 50  
dt = T / M  
  
def srd_euler():  
    xh = np.zeros((M + 1, I))  
    x = np.zeros_like(xh)  
    xh[0] = x0  
    x[0] = x0  
    for t in range(1, M + 1):  
        xh[t] = (xh[t - 1] +  
                 kappa * (theta - np.maximum(xh[t - 1], 0)) * dt +  
                 sigma * np.sqrt(np.maximum(xh[t - 1], 0)) *  
                 math.sqrt(dt) * npr.standard_normal(I)) # ❺ 基于欧拉格式的模拟。  
        x = np.maximum(xh, 0)  
    return x  
  
x1 = srd_euler()  
  
plt.figure(figsize=(10, 6))  
plt.hist(x1[-1], bins=50)  
plt.xlabel('value')  
plt.ylabel('frequency');  
```  
  
<img width="665" height="427" alt="python_for_finance 12 7" src="https://github.com/user-attachments/assets/1f74d5cf-22c4-43b7-a11e-4f92bbb06226" />
  
图 12-7. 到期时动态模拟的平方根扩散（欧拉格式）  
  
接着图 12-8 显示了前 10 条模拟路径，说明了由此产生的负平均漂移（因为 $x_0>\theta$ ）并收敛至 $\theta=0.02$ ：  
  
```python  
plt.figure(figsize=(10, 6))  
plt.plot(x1[:, :10], lw=1.5)  
plt.xlabel('time')  
plt.ylabel('index level');  
```  
  
<img width="665" height="426" alt="python_for_finance 12 8" src="https://github.com/user-attachments/assets/d39d946f-00db-4248-9124-e779532c3289" />
  
图 12-8. 动态模拟的平方根扩散路径（欧拉格式）  
  
公式 12-6 展示了基于非中心卡方分布 $\chi_d^{\prime 2}$ 的平方根扩散的精确离散化格式，其中自由度为：  
$$df=\frac{4\theta\kappa}{\sigma^2}$$  
并且非中心性参数为：  
$$nc=\frac{4\kappa e^{-\kappa\Delta t}}{\sigma^2(1-e^{-\kappa\Delta t})}x_s$$  
  
公式 12-6. 平方根扩散的精确离散化  
$$x_t=\frac{\sigma^2(1-e^{-\kappa\Delta t})}{4\kappa}\chi_d^{\prime 2}\left(\frac{4\kappa e^{-\kappa\Delta t}}{\sigma^2(1-e^{-\kappa\Delta t})}x_s\right)$$  
  
这种离散化格式的 Python 实现稍显复杂，但仍然非常简洁。图 12-9 以直方图形式显示了使用精确格式模拟的到期输出：  
  
```python  
def srd_exact():  
    x = np.zeros((M + 1, I))  
    x[0] = x0  
    for t in range(1, M + 1):  
        df = 4 * theta * kappa / sigma ** 2   
        c = (sigma ** 2 * (1 - np.exp(-kappa * dt))) / (4 * kappa)   
        nc = np.exp(-kappa * dt) / c * x[t - 1]   
        x[t] = c * npr.noncentral_chisquare(df, nc, size=I) # ❶ 精确的离散化格式，利用了 npr.noncentral_chisquare()。  
    return x  
  
x2 = srd_exact()  
  
plt.figure(figsize=(10, 6))  
plt.hist(x2[-1], bins=50)  
plt.xlabel('value')  
plt.ylabel('frequency');  
```  
  
<img width="665" height="429" alt="python_for_finance 12 9" src="https://github.com/user-attachments/assets/16593457-a5f1-4ff3-bc98-e4602be54858" />
  
图 12-9. 到期时动态模拟的平方根扩散（精确格式）  
  
和之前一样，图 12-10 展示了前 10 条模拟路径，再次显示了负平均漂移并收敛至 $\theta$ ：  
  
```python  
plt.figure(figsize=(10, 6))  
plt.plot(x2[:, :10], lw=1.5)  
plt.xlabel('time')  
plt.ylabel('index level');  
```  
  
<img width="664" height="427" alt="python_for_finance 12 10" src="https://github.com/user-attachments/assets/db2d1d95-97b0-4606-a041-117efc8ab9f7" />
  
图 12-10. 动态模拟的平方根扩散路径（精确格式）  
  
比较不同方法的主要统计数据表明，在期望的统计特性方面，有偏的欧拉格式实际上表现得相当不错：  
  
```python  
print_statistics(x1[-1], x2[-1])  
```  
  
```txt  
      statistic     data set 1     data set 2  
---------------------------------------------  
           size      10000.000      10000.000  
            min          0.003          0.005  
            max          0.049          0.047  
           mean          0.020          0.020  
            std          0.006          0.006  
           skew          0.529          0.532  
       kurtosis          0.289          0.273  
```  
  
```python  
I = 250000  
%time x1 = srd_euler()  
```  
  
```txt  
CPU times: user 1.62 s, sys: 184 ms, total: 1.81 s  
Wall time: 1.08 s  
```  
  
```python  
%time x2 = srd_exact()  
```  
  
```txt  
CPU times: user 3.29 s, sys: 39.8 ms, total: 3.33 s  
Wall time: 1.98 s  
```  
  
```python  
print_statistics(x1[-1], x2[-1])  
```  
  
```txt  
      statistic     data set 1     data set 2  
---------------------------------------------  
           size     250000.000     250000.000  
            min          0.002          0.003  
            max          0.071          0.055  
           mean          0.020          0.020  
            std          0.006          0.006  
           skew          0.563          0.579  
       kurtosis          0.492          0.520  
```  
  
然而，在执行速度方面可以观察到很大的差异，因为从非中心卡方分布中抽样在计算上的要求比从标准正态分布中抽样要高。精确格式大致需要两倍的时间才能得到几乎与欧拉格式相同的结果。  
  
**随机波动率**  
  
Black-Scholes-Merton 模型的主要简化假设之一是恒定的波动率。然而，通常来说波动率既不是恒定的也不是确定性的——它是随机的。因此，在20世纪90年代初，引入了所谓的随机波动率模型，这是金融建模方面的一项重大进步。Heston（1993）的模型是属于这一类的最受欢迎的模型之一，如公式 12-7 所示。  
  
公式 12-7. Heston 随机波动率模型的随机微分方程  
$$dS_t=rS_tdt+\sqrt{v_t}S_tdZ_t^1$$  
$$dv_t=\kappa_v(\theta_v-v_t)dt+\sigma_v\sqrt{v_t}dZ_t^2$$  
$$dZ_t^1dZ_t^2=\rho$$  
  
现在可以很容易地从关于几何布朗运动和平方根扩散的讨论中推断出变量和参数的含义。参数 $\rho$ 代表两个标准布朗运动 $Z_t^1, Z_t^2$ 之间的瞬时相关性。这使我们能够解释被称为杠杆效应的典型事实，其本质上是指波动率在压力时期（市场下跌）上升，在牛市时期（市场上涨）下降。  
  
考虑模型的以下参数化。为了考虑两个随机过程之间的相关性，需要确定相关矩阵的乔尔斯基（Cholesky）分解：  
  
```python  
S0 = 100.  
r = 0.05  
v0 = 0.1 # ❶ 初始（瞬时）波动率值。  
kappa = 3.0  
theta = 0.25  
sigma = 0.1  
rho = 0.6 # ❷ 两个布朗运动之间的固定相关性。  
T = 1.0  
  
corr_mat = np.zeros((2, 2))  
corr_mat[0, :] = [1.0, rho]  
corr_mat[1, :] = [rho, 1.0]  
cho_mat = np.linalg.cholesky(corr_mat) # ❸ 乔尔斯基分解及结果矩阵。  
  
cho_mat  
```  
  
```txt  
Out[36]: array([[1. , 0. ],  
 [0.6, 0.8]])  
```  
  
在随机过程模拟开始之前，为这两个过程生成了全套随机数，准备将集合 0 用于指数过程，并将集合 1 用于波动率过程。对于由平方根扩散建模的波动率过程，我们选择了欧拉格式，并通过乔尔斯基矩阵考虑了相关性：  
  
```python  
M = 50  
I = 10000  
dt = T / M  
  
ran_num = npr.standard_normal((2, M + 1, I)) # ❶ 生成三维随机数数据集。  
  
v = np.zeros_like(ran_num[0])  
vh = np.zeros_like(v)  
  
v[0] = v0  
vh[0] = v0  
  
for t in range(1, M + 1):  
    ran = np.dot(cho_mat, ran_num[:, t, :]) # ❷ 提取相关的随机数子集并通过乔尔斯基矩阵对其进行转换。  
    vh[t] = (vh[t - 1] +  
             kappa * (theta - np.maximum(vh[t - 1], 0)) * dt +  
             sigma * np.sqrt(np.maximum(vh[t - 1], 0)) *  
             math.sqrt(dt) * ran[1]) # ❸ 基于欧拉格式模拟路径。  
  
v = np.maximum(vh, 0)  
```  
  
指数水平过程的模拟也考虑了相关性，并对几何布朗运动使用了（在这种情况下）精确的欧拉格式。图 12-11 以直方图形式显示了指数水平过程和波动率过程在到期时的模拟结果：  
  
```python  
S = np.zeros_like(ran_num[0])  
S[0] = S0  
for t in range(1, M + 1):  
    ran = np.dot(cho_mat, ran_num[:, t, :])  
    S[t] = S[t - 1] * np.exp((r - 0.5 * v[t]) * dt +  
                             np.sqrt(v[t]) * ran[0] * np.sqrt(dt))  
  
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 6))  
ax1.hist(S[-1], bins=50)  
ax1.set_xlabel('index level')  
ax1.set_ylabel('frequency')  
ax2.hist(v[-1], bins=50)  
ax2.set_xlabel('volatility');  
```  
  
<img width="664" height="428" alt="python_for_finance 12 11" src="https://github.com/user-attachments/assets/2f352a71-721b-4c56-b708-3cad995a8fa6" />
  
图 12-11. 到期时动态模拟的随机波动率过程  
  
这说明了对平方根扩散使用欧拉格式的另一个优点：因为只需抽取标准正态分布的随机数，所以可以轻松且一致地考虑相关性。没有一种简单的方法可以通过混合方法（即指数过程使用欧拉格式，波动率过程使用基于非中心卡方的精确方法）来实现这一点。  
  
对每个过程的前 10 条模拟路径的检查（参见图 12-12）显示，波动率过程平均呈正向漂移，并且正如预期的那样，收敛至 $\theta=0.25$ ：  
  
```python  
print_statistics(S[-1], v[-1])  
```  
  
```txt  
      statistic     data set 1     data set 2  
---------------------------------------------  
           size      10000.000      10000.000  
            min         20.556          0.174  
            max        517.798          0.328  
           mean        107.843          0.243  
            std         51.341          0.020  
           skew          1.577          0.124  
       kurtosis          4.306          0.048  
```  
  
```python  
fig, (ax1, ax2) = plt.subplots(2, 1, sharex=True, figsize=(10, 6))  
ax1.plot(S[:, :10], lw=1.5)  
ax1.set_ylabel('index level')  
ax2.plot(v[:, :10], lw=1.5)  
ax2.set_xlabel('time')  
ax2.set_ylabel('volatility');  
```  
  
<img width="664" height="429" alt="python_for_finance 12 12" src="https://github.com/user-attachments/assets/bfda332e-d620-4afb-8466-db3db172af8b" />
  
图 12-12. 动态模拟的随机波动率过程路径  
  
简要查看这两个数据集在到期日的统计数据，会发现指数水平过程的最大值相当高。事实上，在其他条件不变的情况下，这比常数波动率的几何布朗运动所能攀升的水平要高得多。  
  
**跳跃扩散**  
  
随机波动率和杠杆效应是在许多市场中发现的程式化（经验）事实。另一个重要的程式化事实是资产价格以及波动率中存在跳跃。在 1976 年，Merton 发表了他的跳跃扩散模型，通过一个生成对数正态分布跳跃的模型组件增强了 Black-Scholes-Merton 设定。公式 12-8 展示了风险中性 SDE。  
  
公式 12-8. Merton 跳跃扩散模型的随机微分方程  
$$dS_t=(r-r_J)S_tdt+\sigma S_tdZ_t+J_tS_tdN_t$$  
  
为了完整起见，这里是变量和参数含义的概述：  
  
$S_t$  
在日期 $t$ 的指数水平  
  
$r$  
恒定的无风险短期利率  
  
$r_J\equiv\lambda\cdot(e^{\mu_J+\delta^2/2}-1)$  
为了保持风险中性对跳跃进行的漂移修正  
  
$\sigma$  
$S$ 的恒定波动率  
  
$Z_t$  
标准布朗运动  
  
$J_t$  
日期 $t$ 的跳跃，其分布为...  
  
* ... $\log(1+J_t)\approx \text{N}(\log(1+\mu_J)-\frac{\delta^2}{2},\delta^2)$ 其中...  
* ... $\text{N}$ 是标准正态随机变量的累积分布函数  
  
$N_t$  
强度为 $\lambda$ 的泊松过程  
  
公式 12-9 展示了跳跃扩散的欧拉离散化，其中 $z_t^n$ 呈标准正态分布，而 $y_t$ 呈强度为 $\lambda$ 的泊松分布。  
  
公式 12-9. Merton 跳跃扩散模型的欧拉离散化  
$$S_t=S_{t-\Delta t}\left(e^{(r-r_J-\sigma^2/2)\Delta t+\sigma\sqrt{\Delta t}z_t^1}+(e^{\mu_J+\delta z_t^2}-1)y_t\right)$$  
  
给定离散化格式，考虑以下数值参数化：  
  
```python  
S0 = 100.  
r = 0.05  
sigma = 0.2  
lamb = 0.75 # ❶ 跳跃强度。  
mu = -0.6 # ❷ 平均跳跃大小。  
delta = 0.25 # ❸ 跳跃波动率。  
rj = lamb * (math.exp(mu + 0.5 * delta ** 2) - 1) # ❹ 漂移修正。  
  
T = 1.0  
M = 50  
I = 10000  
dt = T / M  
```  
  
这一次需要三组随机数。注意图 12-13 中的第二个峰值（双峰频率分布），这是由跳跃引起的：  
  
```python  
S = np.zeros((M + 1, I))  
S[0] = S0  
sn1 = npr.standard_normal((M + 1, I)) # ❶ 标准正态分布随机数。  
sn2 = npr.standard_normal((M + 1, I)) # ❶   
poi = npr.poisson(lamb * dt, (M + 1, I)) # ❷ 泊松分布随机数。  
for t in range(1, M + 1, 1):  
    S[t] = S[t - 1] * (np.exp((r - rj - 0.5 * sigma ** 2) * dt +  
                       sigma * math.sqrt(dt) * sn1[t]) +  
                       (np.exp(mu + delta * sn2[t]) - 1) *  
                       poi[t]) # ❸ 基于精确欧拉格式的模拟。  
    S[t] = np.maximum(S[t], 0)  
  
plt.figure(figsize=(10, 6))  
plt.hist(S[-1], bins=50)  
plt.xlabel('value')  
plt.ylabel('frequency');  
```  
  
<img width="664" height="429" alt="python_for_finance 12 13" src="https://github.com/user-attachments/assets/e9f24034-8fbd-4dfe-9c51-4f9ba66bac56" />
  
图 12-13. 到期时动态模拟的跳跃扩散过程  
  
在图 12-14 所示的前 10 条模拟指数水平路径中也可以看到负跳跃：  
  
```python  
plt.figure(figsize=(10, 6))  
plt.plot(S[:, :10], lw=1.5)  
plt.xlabel('time')  
plt.ylabel('index level');  
```  
  
<img width="664" height="430" alt="python_for_finance 12 14" src="https://github.com/user-attachments/assets/0e769c0d-99f5-4b0f-b0ce-3177cba3af8e" />
  
图 12-14. 动态模拟的跳跃扩散过程路径  
  
### 方差缩减  
  
由于到目前为止使用的 Python 函数生成的是伪随机数，并且由于提取的样本大小不同，生成的一组数字可能无法显示出足够接近预期或所需统计数据的特征。例如，人们期望一组标准正态分布随机数显示出均值为 0，标准差为 1。让我们检查不同组的随机数显示出什么样的统计数据。为了进行符合实际的比较，随机数生成器的种子值是固定的：  
  
```python  
print('%15s %15s' % ('Mean', 'Std. Deviation'))  
print(31 * '-')  
for i in range(1, 31, 2):  
    npr.seed(100)  
    sn = npr.standard_normal(i ** 2 * 10000)  
    print('%15.12f %15.12f' % (sn.mean(), sn.std()))  
```  
  
```txt  
           Mean  Std. Deviation  
-------------------------------  
 0.001150944833  1.006296354600  
 0.002841204001  0.995987967146  
 0.001998082016  0.997701714233  
 0.001322322067  0.997771186968  
 0.000592711311  0.998388962646  
-0.000339730751  0.998399891450  
-0.000228109010  0.998657429396  
 0.000295768719  0.998877333340  
 0.000257107789  0.999284894532  
-0.000357870642  0.999456401088  
-0.000528443742  0.999617831131  
-0.000300171536  0.999445228838  
-0.000162924037  0.999516059328  
 0.000135778889  0.999611052522  
 0.000182006048  0.999619405229  
```  
  
```python  
i ** 2 * 10000  
```  
  
```txt  
Out[53]: 8410000  
```  
  
结果表明，提取的数量越大，统计数据“在某种程度上”会变得更好。2 但它们仍然无法达到所需的结果，即使在我们包含超过 8,000,000 个随机数的最大样本中也是如此。  
（2 这里的灵感来自大数定律。）  
  
幸运的是，有一些易于实现、通用的方差缩减技术可以用来改善与（标准）正态分布的前两个矩的匹配程度。第一种技术是使用对偶变量。这种方法只需进行预期随机提取数量的一半，然后添加同一组具有相反符号的随机数。3 例如，如果随机数生成器（即相应的 Python 函数）提取出 0.5，则将另一个值为 -0.5 的数字添加到该集合中。根据这种构造，此类数据集的平均值必然等于零。  
（3 所述方法仅适用于对称均值为0的随机变量，例如标准正态分布随机变量，这种变量在贯穿全文的应用中几乎是唯一被使用的。）  
  
在 NumPy 中，可以使用 `np.concatenate()` 函数简明扼要地实现这一点。下面重复之前的练习，这次使用对偶变量：  
  
```python  
sn = npr.standard_normal(int(10000 / 2))  
sn = np.concatenate((sn, -sn)) # ❶ 将两个 ndarray 对象连接起来...  
  
np.shape(sn) # ❷ ...从而得出所需数量的随机数。  
```  
  
```txt  
Out[55]: (10000,)  
```  
  
```python  
sn.mean() # ❸ 得到的均值为零（在标准的浮点运算误差范围内）。  
```  
  
```txt  
Out[56]: 2.842170943040401e-18  
```  
  
```python  
print('%15s %15s' % ('Mean', 'Std. Deviation'))  
print(31 * "-")  
for i in range(1, 31, 2):  
    npr.seed(1000)  
    sn = npr.standard_normal(i ** 2 * int(10000 / 2))  
    sn = np.concatenate((sn, -sn))  
    print("%15.12f %15.12f" % (sn.mean(), sn.std()))  
```  
  
```txt  
           Mean  Std. Deviation  
-------------------------------  
 0.000000000000  1.009653753942  
-0.000000000000  1.000413716783  
 0.000000000000  1.002925061201  
-0.000000000000  1.000755212673  
 0.000000000000  1.001636910076  
-0.000000000000  1.000726758438  
-0.000000000000  1.001621265149  
 0.000000000000  1.001203722778  
-0.000000000000  1.000556669784  
-0.000000000000  1.000113464185  
-0.000000000000  0.999435175324  
-0.000000000000  0.999356961431  
-0.000000000000  0.999641436845  
-0.000000000000  0.999642768905  
-0.000000000000  0.999638303451  
```  
  
可以立即注意到，这种方法完美地纠正了一阶矩——鉴于数据集的构造方式，这并不令人惊讶。然而，这种方法对二阶矩（即标准差）没有任何影响。使用另一种方差缩减技术（称为矩匹配），可以帮助一步同时纠正一阶矩和二阶矩：  
  
```python  
sn = npr.standard_normal(10000)  
  
sn.mean()  
```  
  
```txt  
Out[59]: -0.001165998295162494  
```  
  
```python  
sn.std()  
```  
  
```txt  
Out[60]: 0.991255920204605  
```  
  
```python  
sn_new = (sn - sn.mean()) / sn.std() # ❶ 一步校正一阶和二阶矩。  
  
sn_new.mean()  
```  
  
```txt  
Out[62]: -2.3803181647963357e-17  
```  
  
```python  
sn_new.std()  
```  
  
```txt  
Out[63]: 0.9999999999999999  
```  
  
通过从每个随机数中减去均值并将每个随机数除以标准差，该技术确保这组随机数（几乎）完美地匹配了标准正态分布期望的一阶和二阶矩。  
  
以下函数利用有关方差缩减技术的见解，并使用两种、一种或不使用方差缩减技术来生成用于过程模拟的标准正态随机数：  
  
```python  
def gen_sn(M, I, anti_paths=True, mo_match=True):  
    ''' 生成用于模拟的随机数的函数。  
      
    参数  
    ==========  
    M: int  
        离散化的时间区间数  
    I: int  
        要模拟的路径数  
    anti_paths: boolean  
        是否使用对偶变量  
    mo_match: boolean  
        是否使用矩匹配  
    '''  
    if anti_paths is True:  
        sn = npr.standard_normal((M + 1, int(I / 2)))  
        sn = np.concatenate((sn, -sn), axis=1)  
    else:  
        sn = npr.standard_normal((M + 1, I))  
    if mo_match is True:  
        sn = (sn - sn.mean()) / sn.std()  
    return sn  
```  
  
**向量化和模拟**  
  
使用 NumPy 的向量化是在 Python 中实现蒙特卡洛模拟算法的一种自然、简洁且高效的方法。然而，使用 NumPy 向量化通常会带来更大的内存占用。有关可能同样快速的替代方案，请参阅第 10 章。  
  
## 估值  
  
蒙特卡洛模拟最重要的应用之一是对或有要求权（期权、衍生品、混合工具等）的估值。简而言之，在风险中性世界中，或有要求权的价值是风险中性（鞅）测度下的贴现预期收益。这是一种概率测度，它使得所有风险因素（股票、指数等）以无风险短期利率漂移，从而使得贴现过程成为鞅。根据资产定价基本定理，这种概率测度的存在等价于不存在套利。  
  
金融期权体现了在给定的到期日（欧式期权），或在指定的一段时间内（美式期权），以给定价格（执行价格）买入（看涨期权）或卖出（看跌期权）特定金融工具的权利。我们首先考虑估值方面更简单的欧式期权情况。  
  
### 欧式期权  
  
欧式指数看涨期权在到期时的收益由 $h(S_T)\equiv\max(S_T-K,0)$ 给出，其中 $S_T$ 是在到期日 $T$ 的指数水平， $K$ 是执行价格。给定相关随机过程（例如，几何布朗运动）的（或在完备市场中的唯一的）风险中性测度，此类期权的价格由公式 12-10 中的公式给出。  
  
公式 12-10. 风险中性期望定价  
$$C_0=e^{-rT}\textbf{E}_0^\textbf{Q}(h(S_T))=e^{-rT}\int_0^\infty h(s)q(s)ds$$  
  
第 11 章概述了如何通过蒙特卡洛模拟对积分进行数值计算。以下使用了这种方法，并将其应用于公式 12-10。公式 12-11 提供了各自欧式期权的蒙特卡洛估计量，其中 $\tilde{S}_T^i$ 是到期时第 $i$ 次模拟的指数水平。  
  
公式 12-11. 风险中性蒙特卡洛估计量  
$$\tilde{C}_0=e^{-rT}\frac{1}{I}\sum_{i=1}^I h(\tilde{S}_T^i)$$  
  
现在考虑以下几何布朗运动的参数化和估值函数 `gbm_mcs_stat()` ，仅将执行价格作为参数。这里，仅模拟到期时的指数水平。作为参考，考虑执行价格为 $K=105$ 的情况：  
  
```python  
S0 = 100.  
r = 0.05  
sigma = 0.25  
T = 1.0  
I = 50000  
  
def gbm_mcs_stat(K):  
    ''' 通过蒙特卡洛模拟（到期指数水平）计算 Black-Scholes-Merton 模型下的欧式看涨期权估值。  
      
    参数  
    ==========  
    K: float  
        （正的）期权执行价格  
          
    返回  
    =======  
    C0: float  
        欧式看涨期权的估计现值  
    '''  
    sn = gen_sn(1, I)  
    # 模拟到期指数水平  
    ST = S0 * np.exp((r - 0.5 * sigma ** 2) * T  
                     + sigma * math.sqrt(T) * sn[1])  
    # 计算到期收益  
    hT = np.maximum(ST - K, 0)  
    # 计算 MCS 估计量  
    C0 = math.exp(-r * T) * np.mean(hT)  
    return C0  
  
gbm_mcs_stat(K=105.) # ❶ 欧式看涨期权的蒙特卡洛估计量。  
```  
  
```txt  
Out[67]: 10.044221852841922  
```  
  
接下来，考虑动态模拟方法，除了看涨期权之外还允许欧式看跌期权。函数 `gbm_mcs_dyna()` 实现了该算法。该代码还比较了相同执行水平下看涨期权和看跌期权的期权价格估计值：  
  
```python  
M = 50 # ❶ 离散化的时间区间数。  
  
def gbm_mcs_dyna(K, option='call'):  
    ''' 通过蒙特卡洛模拟（指数水平路径）计算 Black-Scholes-Merton 模型下的欧式期权估值。  
      
    参数  
    ==========  
    K: float  
        （正的）期权执行价格  
    option : string  
        待估值的期权类型 ('call', 'put')  
          
    返回  
    =======  
    C0: float  
        欧式期权的估计现值  
    '''  
    dt = T / M  
    # 模拟指数水平路径  
    S = np.zeros((M + 1, I))  
    S[0] = S0  
    sn = gen_sn(M, I)  
    for t in range(1, M + 1):  
        S[t] = S[t - 1] * np.exp((r - 0.5 * sigma ** 2) * dt  
                                 + sigma * math.sqrt(dt) * sn[t])  
    # 基于情况计算收益  
    if option == 'call':  
        hT = np.maximum(S[-1] - K, 0)  
    else:  
        hT = np.maximum(K - S[-1], 0)  
    # 计算 MCS 估计量  
    C0 = math.exp(-r * T) * np.mean(hT)  
    return C0  
  
gbm_mcs_dyna(K=110., option='call') # ❷ 欧式看涨期权的蒙特卡洛估计量。  
```  
  
```txt  
Out[70]: 7.950008525028434  
```  
  
```python  
gbm_mcs_dyna(K=110., option='put') # ❸ 欧式看跌期权的蒙特卡洛估计量。  
```  
  
```txt  
Out[71]: 12.629934942682004  
```  
  
问题是，这些基于模拟的估值方法相对于 Black-Scholes-Merton 估值公式的基准值表现如何。为了找出答案，下面的代码使用模块 `bsm_functions.py` 中的欧式看涨期权分析期权定价公式（参见第 392 页的“Python 脚本”），为一系列执行价格生成各自的期权值/估计量。  
  
首先，我们将静态模拟方法的结果与精确的分析值进行比较：  
  
```python  
from bsm_functions import bsm_call_value  
  
stat_res = [] # ❶ 实例化空列表对象以收集结果。  
dyna_res = [] # ❶   
anal_res = [] # ❶   
k_list = np.arange(80., 120.1, 5.) # ❷ 创建一个包含执行价格范围的 ndarray 对象。  
np.random.seed(100)  
  
for K in k_list:  
    stat_res.append(gbm_mcs_stat(K)) # ❸ 模拟/计算并收集所有执行价格的期权值。  
    dyna_res.append(gbm_mcs_dyna(K)) # ❸   
    anal_res.append(bsm_call_value(S0, K, T, r, sigma)) # ❸   
  
stat_res = np.array(stat_res) # ❹ 将列表对象转换为 ndarray 对象。  
dyna_res = np.array(dyna_res) # ❹   
anal_res = np.array(anal_res) # ❹   
```  
  
图 12-15 显示了结果。所有估值绝对差异都小于 1%。既有负值差异也有正值差异：  
  
```python  
plt.figure(figsize=(10, 6))  
fig, (ax1, ax2) = plt.subplots(2, 1, sharex=True, figsize=(10, 6))  
ax1.plot(k_list, anal_res, 'b', label='analytical')  
ax1.plot(k_list, stat_res, 'ro', label='static')  
ax1.set_ylabel('European call option value')  
ax1.legend(loc=0)  
ax1.set_ylim(bottom=0)  
wi = 1.0  
ax2.bar(k_list - wi / 2, (anal_res - stat_res) / anal_res * 100, wi)  
ax2.set_xlabel('strike')  
ax2.set_ylabel('difference in %')  
ax2.set_xlim(left=75, right=125);  
```  
  
```txt  
Out[76]: <Figure size 720x432 with 0 Axes>  
```  
  
<img width="664" height="427" alt="python_for_finance 12 15" src="https://github.com/user-attachments/assets/2049d74e-b677-4204-a4fa-19d0d6d5b6b6" />
  
图 12-15. 分析期权价值与蒙特卡洛估计量对比（静态模拟）  
  
动态模拟和估值方法也呈现出类似的情况，其结果在图 12-16 中报告。同样，所有的绝对估值差异都小于 1%，既有正偏差也有负偏差。作为一般规则，蒙特卡洛估计量的质量可以通过调整使用的时间区间数量 $M$ 和/或模拟的路径数量 $I$ 来控制：  
  
```python  
fig, (ax1, ax2) = plt.subplots(2, 1, sharex=True, figsize=(10, 6))  
ax1.plot(k_list, anal_res, 'b', label='analytical')  
ax1.plot(k_list, dyna_res, 'ro', label='dynamic')  
ax1.set_ylabel('European call option value')  
ax1.legend(loc=0)  
ax1.set_ylim(bottom=0)  
wi = 1.0  
ax2.bar(k_list - wi / 2, (anal_res - dyna_res) / anal_res * 100, wi)  
ax2.set_xlabel('strike')  
ax2.set_ylabel('difference in %')  
ax2.set_xlim(left=75, right=125);  
```  
  
<img width="664" height="444" alt="python_for_finance 12 16" src="https://github.com/user-attachments/assets/2c9dd2b4-dda7-44be-98f1-8e3361fa7a58" />
  
图 12-16. 分析期权价值与蒙特卡洛估计量对比（动态模拟）  
  
### 美式期权  
  
与欧式期权相比，美式期权的估值更为复杂。在这种情况下，必须解决一个最优停止问题才能得出期权的公允价值。公式 12-12 将美式期权的估值公式化为这样一个问题。问题公式化已经是基于用于数值模拟的离散时间网格。因此，在某种意义上，说具有百慕大行权特征的期权价值更为正确。当时间区间长度趋于零时，百慕大期权价值将收敛于美式期权价值。  
  
公式 12-12. 美式期权价格作为最优停止问题  
$$V_0=\sup_{\tau\in\{0,\Delta t,2\Delta t,\dots,T\}} e^{-rT}\textbf{E}_0^\textbf{Q}(h_\tau(S_\tau))$$  
  
以下描述的算法称为最小二乘蒙特卡洛 (LSM)，来源于 Longstaff 和 Schwartz (2001) 的论文。可以证明，美式（百慕大）期权在任何给定日期 $t$ 的价值由 $V_t(s)=\max(h_t(s), C_t(s))$ 给出，其中 $C_t(s)=\textbf{E}_t^\textbf{Q}(e^{-r\Delta t}V_{t+\Delta t}(S_{t+\Delta t})\mid S_t=s)$ 是在给定指数水平 $S_t=s$ 时所谓的期权延续价值。  
  
现在假设我们在 $M$ 个大小相等的时间区间 $\Delta t$ 上模拟了 $I$ 条指数水平路径。定义 $Y_{t,i}\equiv e^{-r\Delta t}V_{t+\Delta t,i}$ 为时间 $t$ 路径 $i$ 的模拟延续价值。我们不能直接使用这个数字，因为这暗示着完美的预见。然而，我们可以使用所有此类模拟延续价值的截面数据，通过最小二乘回归来估计（预期的）延续价值。  
  
给定一组基函数 $b_d, d=1,\dots,D$ ，延续价值然后由回归估计值给出 $\hat{C}_{t,i}=\sum_{d=1}^D\alpha_{d,t}^*\cdot b_d(S_{t,i})$ ，其中最优回归参数 $\alpha^*$ 是公式 12-13 所述最小二乘问题的解。  
  
公式 12-13. 美式期权估值的最小二乘回归  
$$\min_{\alpha_{1,t},\dots,\alpha_{D,t}} \frac{1}{I} \sum_{i=1}^I \left(Y_{t,i}-\sum_{d=1}^D\alpha_{d,t}\cdot b_d(S_{t,i})\right)^2$$  
  
函数 `gbm_mcs_amer()` 实现了用于美式看涨和看跌期权的 LSM 算法：4  
（4 有关算法细节，请参阅 Hilpisch (2015)。）  
  
```python  
def gbm_mcs_amer(K, option='call'):  
    ''' 通过 LSM 算法的蒙特卡洛模拟计算 Black-Scholes-Merton 模型下的美式期权估值  
      
    参数  
    ==========  
    K: float  
        （正的）期权执行价格  
    option: string  
        待估值的期权类型 ('call', 'put')  
          
    返回  
    =======  
    C0: float  
        美式看涨期权的估计现值  
    '''  
    dt = T / M  
    df = math.exp(-r * dt)  
    # 模拟指数水平  
    S = np.zeros((M + 1, I))  
    S[0] = S0  
    sn = gen_sn(M, I)  
    for t in range(1, M + 1):  
        S[t] = S[t - 1] * np.exp((r - 0.5 * sigma ** 2) * dt  
                                 + sigma * math.sqrt(dt) * sn[t])  
    # 基于情况计算收益  
    if option == 'call':  
        h = np.maximum(S - K, 0)  
    else:  
        h = np.maximum(K - S, 0)  
    # LSM 算法  
    V = np.copy(h)  
    for t in range(M - 1, 0, -1):  
        reg = np.polyfit(S[t], V[t + 1] * df, 7)  
        C = np.polyval(reg, S[t])  
        V[t] = np.where(C > h[t], V[t + 1] * df, h[t])  
    # MCS 估计量  
    C0 = df * np.mean(V[1])  
    return C0  
  
gbm_mcs_amer(110., option='call')  
```  
  
```txt  
Out[79]: 7.721705606305352  
```  
  
```python  
gbm_mcs_amer(110., option='put')  
```  
  
```txt  
Out[80]: 13.609997625418051  
```  
  
欧式期权的价值代表了美式期权价值的下限。这种差异通常被称为提前行权溢价。接下来比较具有相同执行价格范围的欧式和美式期权价值以估计提前行权溢价，这次使用的是看跌期权：5  
（5 由于假设没有红利支付（以指数为例），看涨期权通常没有提前行权溢价（即没有动机提前行权）。）  
  
```python  
euro_res = []  
amer_res = []  
  
k_list = np.arange(80., 120.1, 5.)  
  
for K in k_list:  
    euro_res.append(gbm_mcs_dyna(K, 'put'))  
    amer_res.append(gbm_mcs_amer(K, 'put'))  
  
euro_res = np.array(euro_res)  
amer_res = np.array(amer_res)  
```  
  
图 12-17 显示，对于选定的执行价格范围，提前行权溢价最高可达 10%：  
  
```python  
fig, (ax1, ax2) = plt.subplots(2, 1, sharex=True, figsize=(10, 6))  
ax1.plot(k_list, euro_res, 'b', label='European put')  
ax1.plot(k_list, amer_res, 'ro', label='American put')  
ax1.set_ylabel('call option value')  
ax1.legend(loc=0)  
wi = 1.0  
ax2.bar(k_list - wi / 2, (amer_res - euro_res) / euro_res * 100, wi)  
ax2.set_xlabel('strike')  
ax2.set_ylabel('early exercise premium in %')  
ax2.set_xlim(left=75, right=125);  
```  
  
<img width="665" height="433" alt="python_for_finance 12 17" src="https://github.com/user-attachments/assets/243ff4af-7b2c-4329-bb8a-265ca5cb5ff4" />
  
图 12-17. 欧式对比美式蒙特卡洛估计量  
  
## 风险测度  
  
除了估值之外，风险管理是随机方法和模拟的另一个重要应用领域。本节展示了当今金融业应用的两种最常见风险指标的计算/估计。  
  
### 风险价值 (Value-at-Risk)  
  
风险价值 (VaR) 是最广泛使用的风险指标之一，也是备受争议的指标。因其直观吸引力而受到从业者的喜爱，但也被许多人广泛讨论和批评——主要在理论基础上，因为其捕捉所谓尾部风险的能力有限（稍后会详细介绍）。简而言之，VaR 是一个以货币单位（例如，美元、欧元、日元）表示的数字，指示在给定时间段内具有一定置信水平（概率）不会被超过的损失（对于投资组合、单一头寸等而言）。  
  
考虑一个今天价值 100 万美元的股票头寸，其在 30 天（一个月）内，99% 置信水平下的 VaR 为 50,000 美元。该 VaR 数字表明，在 30 天的期间内，有 99% 的概率（即在 100 种情况中的 99 种情况），预期损失不会超过 50,000 美元。然而，它并没有说明一旦发生超过 50,000 美元的损失时损失的规模——也就是说，如果最大损失是 100,000 或 500,000 美元，发生这种特定的“高于 VaR 的损失”的概率是多少。它所说的只是有 1% 的概率会发生至少 50,000 美元或以上的损失。  
  
假设在 Black-Scholes-Merton 设定下，考虑以下参数化以及对未来日期 $T=30/365$ （为期 30 天）指数水平的模拟。VaR 数据的估计需要根据相对于今天头寸价值的绝对损益进行排序，即从最严重的亏损到最大的利润。图 12-18 显示了模拟的绝对业绩值的直方图：  
  
```python  
S0 = 100  
r = 0.05  
sigma = 0.25  
T = 30 / 365.  
I = 10000  
  
ST = S0 * np.exp((r - 0.5 * sigma ** 2) * T +  
                 sigma * np.sqrt(T) * npr.standard_normal(I)) # ❶ 模拟几何布朗运动的期末值。  
  
R_gbm = np.sort(ST - S0) # ❷ 计算每次模拟运行的绝对损益，并对这些值进行排序。  
  
plt.figure(figsize=(10, 6))  
plt.hist(R_gbm, bins=50)  
plt.xlabel('absolute return')  
plt.ylabel('frequency');  
```  
  
<img width="665" height="431" alt="python_for_finance 12 18" src="https://github.com/user-attachments/assets/bef284e8-ada1-47ed-9c76-bf0ef35ad0c5" />
  
图 12-18. 模拟的绝对损益（几何布朗运动）  
  
拥有了排序结果的 `ndarray` 对象后， `scs.scoreatpercentile()` 函数就可以实现该技巧。只需定义感兴趣的百分位数（以百分比值为单位）。在列表对象 `percs` 中， `0.1` 转化为 $100\%-0.1\%=99.9\%$ 的置信水平。在这种情况下，给定 99.9% 置信水平的 30 天 VaR 为 18.8 个货币单位，而在 90% 置信水平下则为 8.5：  
  
```python  
percs = [0.01, 0.1, 1., 2.5, 5.0, 10.0]  
var = scs.scoreatpercentile(R_gbm, percs)  
print('%16s %16s' % ('Confidence Level', 'Value-at-Risk'))  
print(33 * '-')  
for pair in zip(percs, var):  
    print('%16.2f %16.3f' % (100 - pair[0], -pair[1]))  
```  
  
```txt  
 Confidence Level    Value-at-Risk  
---------------------------------  
           99.99           21.814  
           99.90           18.837  
           99.00           15.230  
           97.50           12.816  
           95.00           10.824  
           90.00            8.504  
```  
  
作为第二个示例，回想一下通过动态模拟产生的 Merton 的跳跃扩散设定。在这种情况下，由于跳跃分量具有负均值，人们可以在图 12-19 中看到模拟损益具有类似双峰分布的情况。从正态分布的角度来看，会观察到一个显著的左侧肥尾（fat tail）：  
  
```python  
dt = 30. / 365 / M  
rj = lamb * (math.exp(mu + 0.5 * delta ** 2) - 1)  
  
S = np.zeros((M + 1, I))  
S[0] = S0  
sn1 = npr.standard_normal((M + 1, I))  
sn2 = npr.standard_normal((M + 1, I))  
poi = npr.poisson(lamb * dt, (M + 1, I))  
for t in range(1, M + 1, 1):  
    S[t] = S[t - 1] * (np.exp((r - rj - 0.5 * sigma ** 2) * dt  
                       + sigma * math.sqrt(dt) * sn1[t])  
                       + (np.exp(mu + delta * sn2[t]) - 1)  
                       * poi[t])  
    S[t] = np.maximum(S[t], 0)  
  
R_jd = np.sort(S[-1] - S0)  
  
plt.figure(figsize=(10, 6))  
plt.hist(R_jd, bins=50)  
plt.xlabel('absolute return')  
plt.ylabel('frequency');  
```  
  
<img width="664" height="427" alt="python_for_finance 12 19" src="https://github.com/user-attachments/assets/c759f453-ed0c-4a13-8f7c-36f92a3fc439" />
  
图 12-19. 模拟的绝对损益（跳跃扩散）  
  
对于该过程及其参数化设置，在 90% 置信水平下，30 天的 VaR 与几何布朗运动的 VaR 几乎相同，而在 99.9% 置信水平下，前者是后者的三倍多（70 货币单位对比 18.8 货币单位）：  
  
```python  
percs = [0.01, 0.1, 1., 2.5, 5.0, 10.0]  
var = scs.scoreatpercentile(R_jd, percs)  
print('%16s %16s' % ('Confidence Level', 'Value-at-Risk'))  
print(33 * '-')  
for pair in zip(percs, var):  
    print('%16.2f %16.3f' % (100 - pair[0], -pair[1]))  
```  
  
```txt  
 Confidence Level    Value-at-Risk  
---------------------------------  
           99.99           76.520  
           99.90           69.396  
           99.00           55.974  
           97.50           46.405  
           95.00           24.198  
           90.00            8.836  
```  
  
这说明了在金融市场中经常遇到的、通过标准的 VaR 指标来捕捉尾部风险时所产生的问题。  
  
为了进一步说明这一点，最后图 12-20 以图形方式展示了这两种情况下的 VaR 测度的直接比较。正如该图所揭示的那样，在一系列典型的置信水平下，VaR 测度表现得截然不同：  
  
```python  
percs = list(np.arange(0.0, 10.1, 0.1))  
gbm_var = scs.scoreatpercentile(R_gbm, percs)  
jd_var = scs.scoreatpercentile(R_jd, percs)  
  
plt.figure(figsize=(10, 6))  
plt.plot(percs, gbm_var, 'b', lw=1.5, label='GBM')  
plt.plot(percs, jd_var, 'r', lw=1.5, label='JD')  
plt.legend(loc=4)  
plt.xlabel('100 - confidence level [%]')  
plt.ylabel('value-at-risk')  
plt.ylim(ymax=0.0);  
```  
  
<img width="665" height="434" alt="python_for_finance 12 20" src="https://github.com/user-attachments/assets/d8b1b7a9-a3f2-4175-83f7-84d04c5c6dc9" />
  
图 12-20. 几何布朗运动和跳跃扩散的风险价值  
  
### 信用估值调整 (Credit Valuation Adjustments)  
  
其他重要的风险测度是信用风险价值 (CVaR) 以及从 CVaR 派生而来的信用估值调整 (CVA)。粗略地说，CVaR 衡量的是交易对手可能无法履行其义务（例如，如果交易对手破产）所带来的风险。在这样的情况下，必须做出两个主要的假设：违约概率和（平均）损失水平。  
  
具体来说，再次考虑在以下代码中具有参数化的 Black-Scholes-Merton 基准设定。在最简单的情况下，考虑交易对手有一个固定（平均）的损失水平 $L$ 和一个固定的违约概率 $p$ （每年）。使用泊松分布生成如下违约场景，并考虑到违约只能发生一次：  
  
```python  
S0 = 100.  
r = 0.05  
sigma = 0.2  
T = 1.  
I = 100000  
  
ST = S0 * np.exp((r - 0.5 * sigma ** 2) * T  
                 + sigma * np.sqrt(T) * npr.standard_normal(I))  
  
L = 0.5 # ❶ 定义损失水平。  
p = 0.01 # ❷ 定义违约概率。  
D = npr.poisson(p * T, I) # ❸ 模拟违约事件。  
D = np.where(D > 1, 1, D) # ❹ 将违约限制发生为一次。  
```  
  
在没有发生违约的情况下，未来指数水平的风险中性价值应该等于资产今天的当前价值（仅相差数值误差）。CVaR 和针对信用风险调整后的资产现值如下给出：  
  
```python  
math.exp(-r * T) * np.mean(ST) # ❶ 资产在 T 时的贴现平均模拟价值。  
```  
  
```txt  
Out[105]: 99.94767178982691  
```  
  
```python  
CVaR = math.exp(-r * T) * np.mean(L * D * ST) # ❷ 作为违约情况下的未来损失贴现平均值的 CVaR。  
CVaR  
```  
  
```txt  
Out[106]: 0.4883560258963962  
```  
  
```python  
S0_CVA = math.exp(-r * T) * np.mean((1 - L * D) * ST) # ❸ 资产在 T 时的贴现平均模拟价值，针对因违约引起的模拟损失进行了调整。  
S0_CVA  
```  
  
```txt  
Out[107]: 99.45931576393053  
```  
  
```python  
S0_adj = S0 - CVaR # ❹ 通过模拟的 CVaR 调整后的资产当前价格。  
S0_adj  
```  
  
```txt  
Out[108]: 99.5116439741036  
```  
  
在这个特定的模拟示例中，观察到大约 1,000 次由信用风险造成的损失。在假定 1% 违约概率以及 100,000 条模拟路径的情况下，这也是符合预期的。图 12-21 显示了违约造成损失的完整频率分布。当然，在绝大多数情况下（即在 100,000 个情况中约 99,000 个），没有观察到损失：  
  
```python  
np.count_nonzero(L * D * ST) # ❶ 违约事件的数量，以及因此造成的损失事件数量。  
```  
  
```txt  
Out[109]: 978  
```  
  
```python  
plt.figure(figsize=(10, 6))  
plt.hist(L * D * ST, bins=50)  
plt.xlabel('loss')  
plt.ylabel('frequency')  
plt.ylim(ymax=175);  
```  
  
<img width="665" height="430" alt="python_for_finance 12 21" src="https://github.com/user-attachments/assets/77fa4151-c19a-4871-b029-370098821b03" />
  
图 12-21. 风险中性预期违约（股票）带来的损失  
  
现在考虑欧式看涨期权的情况。在执行价格为 100 时，其价值约为 10.4 个货币单位。在有关违约概率和损失水平的假设相同的情况下，CVaR 约为 5 美分：  
  
```python  
K = 100.  
hT = np.maximum(ST - K, 0)  
  
C0 = math.exp(-r * T) * np.mean(hT) # ❶ 欧式看涨期权的蒙特卡洛估计量。  
C0  
```  
  
```txt  
Out[112]: 10.396916492839354  
```  
  
```python  
CVaR = math.exp(-r * T) * np.mean(L * D * hT) # ❷ 违约情况下的未来损失贴现平均值的 CVaR。  
CVaR  
```  
  
```txt  
Out[113]: 0.05159099858923533  
```  
  
```python  
C0_CVA = math.exp(-r * T) * np.mean((1 - L * D) * hT) # ❸ 欧式看涨期权的蒙特卡洛估计量，并对违约带来的模拟损失进行了调整。  
C0_CVA  
```  
  
```txt  
Out[114]: 10.34532549425012  
```  
  
与常规资产的情况相比，期权情况具有一些不同的特征。尽管总共仍然有 1,000 次违约，但只有 500 多一点是因为违约造成的损失。这是由于期权在到期时的收益有很大概率为零。图 12-22 显示，与常规资产情况相比，期权的 CVaR 具有非常不同的频率分布：  
  
```python  
np.count_nonzero(L * D * hT) # ❶ 违约带来的损失数量。  
```  
  
```txt  
Out[115]: 538  
```  
  
```python  
np.count_nonzero(D) # ❷ 违约数量。  
```  
  
```txt  
Out[116]: 978  
```  
  
```python  
I - np.count_nonzero(hT) # ❸ 期权无价值到期的数量。  
```  
  
```txt  
Out[117]: 44123  
```  
  
```python  
plt.figure(figsize=(10, 6))  
plt.hist(L * D * hT, bins=50)  
plt.xlabel('loss')  
plt.ylabel('frequency')  
plt.ylim(ymax=350);  
```  
  
<img width="664" height="434" alt="python_for_finance 12 22" src="https://github.com/user-attachments/assets/2fd372e8-5322-4e04-abd3-a8b5121c4076" />
  
图 12-22. 风险中性预期违约（看涨期权）带来的损失  
  
## Python 脚本  
  
下文介绍了与 Black-Scholes-Merton 模型相关的核心功能的实现，这些函数用于欧式（看涨）期权的分析定价。有关模型的详细信息，请参阅 Black 和 Scholes (1973) 以及 Merton (1973)。有关基于 Python 类的替代实现，请参见附录 B。  
  
```python  
#  
# 在 Black-Scholes-Merton 模型中  
# 对欧式看涨期权进行估值  
# 包括 vega 函数和隐含波动率估计  
# bsm_functions.py  
#  
# (c) Dr. Yves J. Hilpisch  
# Python for Finance, 2nd ed.  
#  
  
def bsm_call_value(S0, K, T, r, sigma):  
    ''' 在 BSM 模型中对欧式看涨期权进行估值。  
    分析公式。  
      
    参数  
    ==========  
    S0: float  
        初始股票/指数水平  
    K: float  
        执行价格  
    T: float  
        到期日（年为单位的小数）  
    r: float  
        恒定无风险短期利率  
    sigma: float  
        扩散项中的波动率因子  
          
    返回  
    =======  
    value: float  
        欧式看涨期权的现值  
    '''  
    from math import log, sqrt, exp  
    from scipy import stats  
      
    S0 = float(S0)  
    d1 = (log(S0 / K) + (r + 0.5 * sigma ** 2) * T) / (sigma * sqrt(T))  
    d2 = (log(S0 / K) + (r - 0.5 * sigma ** 2) * T) / (sigma * sqrt(T))  
    # stats.norm.cdf --> 累积分布函数  
    # 针对正态分布  
    value = (S0 * stats.norm.cdf(d1, 0.0, 1.0) -  
             K * exp(-r * T) * stats.norm.cdf(d2, 0.0, 1.0))  
    return value  
  
def bsm_vega(S0, K, T, r, sigma):  
    ''' BSM 模型中欧式期权的 Vega。  
      
    参数  
    ==========  
    S0: float  
        初始股票/指数水平  
    K: float  
        执行价格  
    T: float  
        到期日（年为单位的小数）  
    r: float  
        恒定无风险短期利率  
    sigma: float  
        扩散项中的波动率因子  
          
    返回  
    =======  
    vega: float  
        BSM 公式相对于 sigma 的偏导数，即 vega  
    '''  
    from math import log, sqrt  
    from scipy import stats  
  
    S0 = float(S0)  
    d1 = (log(S0 / K) + (r + 0.5 * sigma ** 2) * T) / (sigma * sqrt(T))  
    vega = S0 * stats.norm.pdf(d1, 0.0, 1.0) * sqrt(T)  
    return vega  
  
# 隐含波动率函数  
  
def bsm_call_imp_vol(S0, K, T, r, C0, sigma_est, it=100):  
    ''' BSM 模型中欧式看涨期权的隐含波动率。  
      
    参数  
    ==========  
    S0: float  
        初始股票/指数水平  
    K: float  
        执行价格  
    T: float  
        到期日（年为单位的小数）  
    r: float  
        恒定无风险短期利率  
    sigma_est: float  
        隐含波动率估计值  
    it: integer  
        迭代次数  
          
    返回  
    =======  
    simga_est: float  
        通过数值估计得出的隐含波动率  
    '''  
    for i in range(it):  
        sigma_est -= ((bsm_call_value(S0, K, T, r, sigma_est) - C0) /  
                      bsm_vega(S0, K, T, r, sigma_est))  
    return sigma_est  
```  
  
## 结论  
  
本章探讨了对于将蒙特卡洛模拟应用在金融领域中至关重要的方法和技术。具体而言，首先展示了如何基于不同的分布规律生成伪随机数。随后进行了随机变量和随机过程的模拟，这在许多金融领域中很重要。本章还深入讨论了两个应用领域：具有欧式和美式行权期权的估值，以及风险价值和信用估值调整等风险指标的估计。  
  
本章说明了 Python 与 NumPy 结合，非常适合实现诸如通过蒙特卡洛模拟进行美式期权估值这种计算要求极高的任务。这主要归功于大多数 NumPy 的函数和类都是用 C 语言实现的这一事实，这通常比纯 Python 代码带来相当大的速度优势。另一个好处是由于向量化操作，最终产生的代码紧凑并且具有良好的可读性。  
  
## 进一步的资源  
  
将蒙特卡洛模拟引入金融领域的最初文章是：  
  
* Boyle, Phelim (1977). “Options: A Monte Carlo Approach.” Journal of Financial Economics, Vol. 4, No. 4, pp. 322–338.  
  
本章中引用的其他原始论文有（另见第 18 章）：  
  
* Black, Fischer, and Myron Scholes (1973). “The Pricing of Options and Corporate Liabilities.” Journal of Political Economy, Vol. 81, No. 3, pp. 638–659.  
* Cox, John, Jonathan Ingersoll, and Stephen Ross (1985). “A Theory of the Term Structure of Interest Rates.” Econometrica, Vol. 53, No. 2, pp. 385–407.  
* Heston, Steven (1993). “A Closed-Form Solution for Options with Stochastic Volatility with Applications to Bond and Currency Options.” The Review of Financial Studies, Vol. 6, No. 2, pp. 327–343.  
* Merton, Robert (1973). “Theory of Rational Option Pricing.” Bell Journal of Economics and Management Science, Vol. 4, pp. 141–183.  
* Merton, Robert (1976). “Option Pricing When the Underlying Stock Returns Are Discontinuous.” Journal of Financial Economics, Vol. 3, No. 3, pp. 125–144.  
  
以下书籍更深入地涵盖了本章的主题（然而，第一本书没有涵盖技术实现细节）：  
  
* Glasserman, Paul (2004). Monte Carlo Methods in Financial Engineering. New York: Springer.  
* Hilpisch, Yves (2015). Derivatives Analytics with Python. Chichester, England: Wiley Finance.  
  
直到世纪之交，一种通过蒙特卡洛模拟对美式期权进行估值的高效方法才最终发表：  
  
* Longstaff, Francis, and Eduardo Schwartz (2001). “Valuing American Options by Simulation: A Simple Least Squares Approach.” Review of Financial Studies, Vol. 14, No. 1, pp. 113–147.  
  
对信用风险的广泛而深入的讨论可以在以下文献中找到：  
  
* Duffie, Darrell, and Kenneth Singleton (2003). Credit Risk—Pricing, Measurement, and Management. Princeton, NJ: Princeton University Press.  
