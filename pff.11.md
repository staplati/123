# 第11章 数学工具  
  
> 现代世界的牧师是数学家。  
> ——比尔·盖德 (Bill Gaede)  
  
自从20世纪80年代和90年代所谓的“火箭科学家”来到华尔街以来，金融已经演变成一门应用数学学科。早期的金融研究论文包含大量文字，几乎没有数学表达式和方程式，而现在的论文主要由数学表达式和方程式组成，周围有一些解释性文字。  
  
本章介绍了金融中一些有用的数学工具，但并未提供每个工具的详细背景。关于这个主题有许多有用的书籍可供参考，因此本章侧重于如何使用Python来应用这些工具和技术。具体来说，它涵盖了：  
  
**“逼近” 见第312页**  
回归和插值是金融中最常用的数值技术之一。  
  
**“凸优化” 见第328页**  
许多金融学科需要凸优化工具（例如，在衍生品分析涉及模型校准时）。  
  
**“积分” 见第334页**  
特别是，金融（衍生）资产的估值通常归结为积分的计算。  
  
**“符号计算” 见第337页**  
Python通过SymPy提供了一个强大的符号数学包，例如，用于求解（方程）组。  
  
## 逼近  
  
首先，常规的导入：  
  
```python  
import numpy as np  
from pylab import plt, mpl  
  
plt.style.use('seaborn')  
mpl.rcParams['font.family'] = 'serif'  
%matplotlib inline  
```  
  
在整个本节中，主要的示例函数如下，它由一个三角项和一个线性项组成：  
  
```python  
def f(x):  
    return np.sin(x) + 0.5 * x  
```  
  
主要重点是通过回归和插值技术在给定区间上逼近这个函数。首先，绘制该函数的图形，以更好地了解逼近到底要实现什么。我们感兴趣的区间应为 $[–2\pi, 2\pi]$ 。图11-1显示了通过 `np.linspace()` 函数定义的固定区间上的该函数。 `create_plot()` 函数是一个辅助函数，用于创建本章中多次需要的相同类型的图形：  
  
```python  
def create_plot(x, y, styles, labels, axlabels):  
    plt.figure(figsize=(10, 6))  
    for i in range(len(x)):  
        plt.plot(x[i], y[i], styles[i], label=labels[i])  
    plt.xlabel(axlabels[0])  
    plt.ylabel(axlabels[1])  
    plt.legend(loc=0)  
  
x = np.linspace(-2 * np.pi, 2 * np.pi, 50) # 用于绘图和计算的x值。  
create_plot([x], [f(x)], ['b'], ['f(x)'], ['x', 'f(x)'])  
```  
  
<img width="665" height="433" alt="python_for_finance 11 1" src="https://github.com/user-attachments/assets/2394d9de-2a8a-40f8-96f9-7af0f73779e5" />
  
图11-1. 示例函数图  
  
### 回归  
  
在函数逼近方面，回归是一个相当有效的工具。它不仅适用于逼近一维函数，在更高维度中也同样适用。得出回归结果所需的数值技术易于实现且执行迅速。基本上，回归的任务是在给定一组所谓的基函数 $b_d$ （其中 $d \in \{1, \dots, D\}$ ）的情况下，根据等式11-1找到最佳参数 $\alpha_1^*$ , $\dots$ , $\alpha_D^*$ ，其中对于 $i \in \{1, \dots, I\}$ 个观测点，有 $y_i \equiv f(x_i)$ 。 $x_i$ 被视为独立观测值，而 $y_i$ 被视为因变量观测值（在函数或统计意义上）。  
  
等式11-1. 回归的最小化问题  
  
$$\min_{\alpha_1, \dots, \alpha_D} \frac{1}{I} \sum_{i=1}^I (y_i - \sum_{d=1}^D \alpha_d \cdot b_d(x_i))^2$$  
  
#### 单项式作为基函数  
  
最简单的情况之一是使用单项式作为基函数——即 $b_1 = 1, b_2 = x, b_3 = x^2, b_4 = x^3, \dots$ 。在这种情况下，NumPy具有内置函数，可用于确定最佳参数（即 `np.polyfit()` ）以及在给定一组输入值时评估逼近结果（即 `np.polyval()` ）。  
  
表11-1列出了 `np.polyfit()` 函数接受的参数。给定从 `np.polyfit()` 返回的最佳回归系数 `p` ， `np.polyval(p, x)` 然后返回 $x$ 坐标处的回归值。  
  
<img width="352" height="219" alt="python_for_finance 11 1 1" src="https://github.com/user-attachments/assets/e0619342-d30c-4d5b-b838-e697c69fae1a" />
  
表11-1. polyfit()函数的参数  
  
在典型的向量化风格中， `np.polyfit()` 和 `np.polyval()` 的应用对于线性回归（即 `deg=1` ）采用以下形式。给定存储在 `ry` 数组中的回归估计值，我们可以将回归结果与图11-2中显示的原始函数进行比较。当然，线性回归无法解释示例函数的sin部分：  
  
```python  
res = np.polyfit(x, f(x), deg=1, full=True) # 线性回归步骤。  
res # 完整结果：回归参数、残差、有效秩、奇异值和相对条件数。  
```  
  
```txt  
Out[8]: (array([ 4.28841952e-01, -1.31499950e-16]),  
 array([21.03238686]),  
 2,  
 array([1., 1.]),  
 1.1102230246251565e-14)  
```  
  
```python  
ry = np.polyval(res[0], x) # 使用回归参数进行评估。  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="432" alt="python_for_finance 11 2" src="https://github.com/user-attachments/assets/93cb252c-e195-47d7-ac74-8ae2e4a474fd" />
  
图11-2. 线性回归  
  
为了解释示例函数的sin部分，必须使用更高阶的单项式。下一次回归尝试采用最高至5阶的单项式作为基函数。正如图11-3所示，回归结果现在看起来更接近原始函数，这并不太令人惊讶。然而，它远非完美：  
  
```python  
reg = np.polyfit(x, f(x), deg=5)  
ry = np.polyval(reg, x)  
  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="433" alt="python_for_finance 11 3" src="https://github.com/user-attachments/assets/0465dac6-e486-4fcf-8b9a-258c970c7d9b" />
  
图11-3. 最高5阶单项式的回归  
  
最后一次尝试采用最高至7阶的单项式来逼近示例函数。在图11-4所示的这种情况下，结果相当令人信服：  
  
```python  
reg = np.polyfit(x, f(x), 7)  
ry = np.polyval(reg, x)  
  
np.allclose(f(x), ry) # 检查函数值和回归值是否相同（或至少接近）。  
```  
  
```txt  
Out[14]: False  
```  
  
```python  
np.mean((f(x) - ry) ** 2) # 给定函数值，计算回归值的均方误差 (MSE)。  
```  
  
```txt  
Out[15]: 0.0017769134759517689  
```  
  
```python  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="432" alt="python_for_finance 11 4" src="https://github.com/user-attachments/assets/f9be3e57-83fd-4b7c-b060-993e4e4f8f00" />
  
图11-4. 最高7阶单项式的回归  
  
#### 单个基函数  
  
一般而言，通过选择更好的基函数集，例如利用有关要逼近的函数的知识，可以获得更好的回归结果。在这种情况下，必须通过矩阵方法（即使用NumPy的 `ndarray` 对象）来定义各个基函数。首先是带有最高3阶单项式的情况（图11-5）。这里的核心函数是 `np.linalg.lstsq()` ：  
  
```python  
matrix = np.zeros((3 + 1, len(x))) # 基函数值（矩阵）的ndarray对象。  
matrix[3, :] = x ** 3 # 从常数到立方的基函数值。  
matrix[2, :] = x ** 2   
matrix[1, :] = x   
matrix[0, :] = 1   
  
reg = np.linalg.lstsq(matrix.T, f(x), rcond=None)[0] # 回归步骤。  
reg.round(4) # 最佳回归参数。  
```  
  
```txt  
Out[19]: array([ 0.    ,  0.5628, -0.    , -0.0054])  
```  
  
```python  
ry = np.dot(reg, matrix) # 函数值的回归估计。  
  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="434" alt="python_for_finance 11 5" src="https://github.com/user-attachments/assets/2c3b2d01-63ad-4089-a10d-c9b18ed98fa5" />
  
图11-5. 使用单个基函数的回归  
  
图11-5中的结果并没有基于我们以前使用单项式的经验所预期的那么好。使用更一般的方法使我们能够利用关于示例函数的知识——即函数中存在一个sin部分。因此，在基函数集中包含正弦函数是有道理的。为了简单起见，最高阶的单项式被替换了。现在的拟合是完美的，正如下面的数字和图11-6所示：  
  
```python  
matrix[3, :] = np.sin(x) # 利用关于示例函数的知识的新基函数。  
reg = np.linalg.lstsq(matrix.T, f(x), rcond=None)[0]  
  
reg.round(4) # 最佳回归参数恢复了原始参数。  
```  
  
```txt  
Out[24]: array([0. , 0.5, 0. , 1. ])  
```  
  
```python  
ry = np.dot(reg, matrix)  
np.allclose(f(x), ry) # 回归现在实现了完美的拟合。  
```  
  
```txt  
Out[26]: True  
```  
  
```python  
np.mean((f(x) - ry) ** 2)   
```  
  
```txt  
Out[27]: 3.404735992885531e-31  
```  
  
```python  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="434" alt="python_for_finance 11 6" src="https://github.com/user-attachments/assets/56dcf088-aafd-4100-8cb9-3d39cb320836" />
  
图11-6. 使用正弦基函数的回归  
  
#### 噪声数据  
  
回归同样可以很好地处理噪声数据，无论是来自模拟的数据还是来自（非完美的）测量的数。为了说明这一点，我们生成了带有噪声的独立观测值和带有噪声的因变量观测值。图11-7表明，回归结果比含噪声的数据点更接近原始函数。在某种意义上，回归在一定程度上平均了噪声：  
  
```python  
xn = np.linspace(-2 * np.pi, 2 * np.pi, 50) # 新的确定的x值。  
xn = xn + 0.15 * np.random.standard_normal(len(xn)) # 引入噪声到x值。  
yn = f(xn) + 0.25 * np.random.standard_normal(len(xn)) # 引入噪声到y值。  
  
reg = np.polyfit(xn, yn, 7)  
ry = np.polyval(reg, xn)  
  
create_plot([x, x], [f(x), ry], ['b', 'r.'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
<img width="666" height="434" alt="python_for_finance 11 7" src="https://github.com/user-attachments/assets/f22f54c0-fd88-4a88-b42c-50d2ec0e50c2" />
  
图11-7. 噪声数据的回归  
  
#### 未排序数据  
  
回归的另一个重要方面是，该方法也可以无缝地处理未排序的数据。前面的示例都依赖于排序好的 $x$ 数据。这并非必须如此。为了说明这一点，让我们看看 $x$ 值的另一种随机化方法。在这种情况下，通过目视检查原始数据很难识别任何结构：  
  
```python  
xu = np.random.rand(50) * 4 * np.pi - 2 * np.pi # 随机化x值。  
yu = f(xu)  
  
print(xu[:10].round(2))   
print(yu[:10].round(2))   
```  
  
```txt  
[-4.17 -0.11 -1.91  2.33  3.34 -0.96  5.81  4.92 -4.56 -5.42]  
[-1.23 -0.17 -1.9   1.89  1.47 -1.29  2.45  1.48 -1.29 -1.95]  
```  
  
```python  
reg = np.polyfit(xu, yu, 5)  
ry = np.polyval(reg, xu)  
  
create_plot([xu, xu], [yu, ry], ['b.', 'ro'],  
            ['f(x)', 'regression'], ['x', 'f(x)'])  
```  
  
正如使用噪声数据一样，回归方法并不关心观测点的顺序。这在检查等式11-1中最小化问题的结构时变得很明显。图11-8中给出的结果也很明显说明了这一点。  
  
<img width="664" height="433" alt="python_for_finance 11 8" src="https://github.com/user-attachments/assets/45e8cf3d-ba9a-4266-a724-0978d91bef74" />
  
图11-8. 未排序数据的回归  
  
#### 多维  
  
最小二乘回归方法的另一个方便特征是，它可以在不需要太多修改的情况下转移到多维情况中。以 `fm()` 为例，如下所示：  
  
```python  
def fm(p):  
    x, y = p  
    return np.sin(x) + 0.25 * x + np.sqrt(y) + 0.05 * y ** 2  
```  
  
为了正确地可视化此函数，需要独立数据点的（二维）网格。基于独立和结果相关的因变量数据点的二维网格（在下面由 `X` 、 `Y` 和 `Z` 具体体现），图11-9呈现了 `fm()` 函数的形状：  
  
```python  
x = np.linspace(0, 10, 20)  
y = np.linspace(0, 10, 20)  
X, Y = np.meshgrid(x, y) # 从一维ndarray对象生成二维ndarray对象（“网格”）。  
  
Z = fm((X, Y))  
x = X.flatten() # 从二维ndarray对象产生一维ndarray对象。  
y = Y.flatten()   
  
from mpl_toolkits.mplot3d import Axes3D # 根据需要从matplotlib导入3D绘图功能。  
  
fig = plt.figure(figsize=(10, 6))  
ax = fig.gca(projection='3d')  
surf = ax.plot_surface(X, Y, Z, rstride=2, cstride=2,  
                       cmap='coolwarm', linewidth=0.5,  
                       antialiased=True)  
ax.set_xlabel('x')  
ax.set_ylabel('y')  
ax.set_zlabel('f(x, y)')  
fig.colorbar(surf, shrink=0.5, aspect=5)  
```  
  
<img width="668" height="450" alt="python_for_finance 11 9" src="https://github.com/user-attachments/assets/900ee663-741f-44d7-baae-0643b02fed40" />
  
图11-9. 具有两个参数的函数  
  
为了获得良好的回归结果，基函数集是至关重要的。因此，结合对函数 `fm()` 本身的了解，同时包含 `np.sin()` 和 `np.sqrt()` 函数。图11-10直观地展示了完美的回归结果：  
  
```python  
matrix = np.zeros((len(x), 6 + 1))  
matrix[:, 6] = np.sqrt(y) # y参数的np.sqrt()函数。  
matrix[:, 5] = np.sin(x) # x参数的np.sin()函数。  
matrix[:, 4] = y ** 2  
matrix[:, 3] = x ** 2  
matrix[:, 2] = y  
matrix[:, 1] = x  
matrix[:, 0] = 1  
  
reg = np.linalg.lstsq(matrix, fm((x, y)), rcond=None)[0]  
  
RZ = np.dot(matrix, reg).reshape((20, 20)) # 将回归结果转换为网格结构。  
  
fig = plt.figure(figsize=(10, 6))  
ax = fig.gca(projection='3d')  
surf1 = ax.plot_surface(X, Y, Z, rstride=2, cstride=2,  
                        cmap=mpl.cm.coolwarm, linewidth=0.5,  
                        antialiased=True) # 绘制原始函数曲面。  
surf2 = ax.plot_wireframe(X, Y, RZ, rstride=2, cstride=2,  
                          label='regression') # 绘制回归曲面。  
ax.set_xlabel('x')  
ax.set_ylabel('y')  
ax.set_zlabel('f(x, y)')  
ax.legend()  
fig.colorbar(surf, shrink=0.5, aspect=5)  
```  
  
<img width="666" height="451" alt="python_for_finance 11 10" src="https://github.com/user-attachments/assets/59099a9a-06b8-49e7-b352-f5293e51c2de" />
  
图11-10. 具有两个参数的函数回归曲面  
  
> **回归**  
> 最小二乘回归方法有多个应用领域，包括基于噪声或未排序数据的简单函数逼近和函数逼近。这些方法既可以应用于一维问题，也可以应用于多维问题。由于基础数学原理，其应用“几乎总是一样的”。  
  
### 插值  
  
与回归相比，插值（例如，使用三次样条）在数学上更复杂。它也仅限于低维问题。给定一组有序的观测点（按 $x$ 维度排序），基本思想是在两个相邻数据点之间进行回归，不仅使结果分段定义的插值函数完美匹配数据点，而且使函数在数据点处连续可微。连续可微性至少需要进行3次插值——即使用三次样条。然而，该方法通常也适用于二次甚至线性样条。  
  
以下代码实现了线性样条插值，其结果如图11-11所示：  
  
```python  
import scipy.interpolate as spi # 从SciPy导入所需的子包。  
  
x = np.linspace(-2 * np.pi, 2 * np.pi, 25)  
  
def f(x):  
    return np.sin(x) + 0.5 * x  
  
ipo = spi.splrep(x, f(x), k=1) # 实现线性样条插值。  
iy = spi.splev(x, ipo) # 推导插值值。  
  
np.allclose(f(x), iy) # 检查插值值是否（足够）接近函数值。  
```  
  
```txt  
Out[50]: True  
```  
  
```python  
create_plot([x, x], [f(x), iy], ['b', 'ro'],  
            ['f(x)', 'interpolation'], ['x', 'f(x)'])  
```  
  
<img width="666" height="433" alt="python_for_finance 11 11" src="https://github.com/user-attachments/assets/93009f18-cad0-4e38-9551-2b03fd70a15e" />
  
图11-11. 线性样条插值（完整数据集）  
  
给定一组按 $x$ 排序的数据点，其应用本身就像应用 `np.polyfit()` 和 `np.polyval()` 一样简单。这里的相应函数是 `sci.splrep()` 和 `sci.splev()` 。表11-2列出了 `sci.splrep()` 函数采用的主要参数。  
  
<img width="394" height="272" alt="python_for_finance 11 2 1" src="https://github.com/user-attachments/assets/fc34f661-5715-463b-b422-f76e8eeb0037" />
  
表11-2. splrep()函数的参数  
  
表11-3列出了 `sci.splev()` 函数接受的参数。  
  
<img width="565" height="168" alt="python_for_finance 11 3 1" src="https://github.com/user-attachments/assets/8f3b0cc4-207f-4502-9cd7-bdd3eb7ccecb" />
  
表11-3. splev()函数的参数  
  
样条插值在金融中经常用于为原始观测中未包含的自变量数据点生成因变量估计值。为此，下一个示例选择了一个小得多的区间，并仔细研究了具有线性样条的插值值。图11-12揭示了插值函数确实在两个观测点之间进行线性插值。对于某些应用，这可能不够精确。此外，显然该函数在原始数据点处不是连续可微的——这是另一个缺点：  
  
```python  
xd = np.linspace(1.0, 3.0, 50) # 点数更多但区间更小。  
iyd = spi.splev(xd, ipo)  
  
create_plot([xd, xd], [f(xd), iyd], ['b', 'ro'],  
            ['f(x)', 'interpolation'], ['x', 'f(x)'])  
```  
  
<img width="665" height="433" alt="python_for_finance 11 12" src="https://github.com/user-attachments/assets/c37d58ef-ce04-4ad9-ab60-acac482d7f44" />
  
图11-12. 线性样条插值（数据子集）  
  
重复整个练习，这次使用三次样条，结果会显着改善（见图11-13）：  
  
```python  
ipo = spi.splrep(x, f(x), k=3) # 在完整数据集上的三次样条插值。  
iyd = spi.splev(xd, ipo) # 将结果应用于较小的区间。  
  
np.allclose(f(xd), iyd) # 插值仍不完美……  
```  
  
```txt  
Out[55]: False  
```  
  
```python  
np.mean((f(xd) - iyd) ** 2) # ……但比以前更好。  
```  
  
```txt  
Out[56]: 1.1349319851436892e-08  
```  
  
```python  
create_plot([xd, xd], [f(xd), iyd], ['b', 'ro'],  
            ['f(x)', 'interpolation'], ['x', 'f(x)'])  
```  
  
<img width="666" height="431" alt="python_for_finance 11 13" src="https://github.com/user-attachments/assets/24c26529-f627-44b7-927b-bb9dabeba770" />
  
图11-13. 三次样条插值（数据子集）  
  
> **插值**  
> 在那些可以应用样条插值的情况下，与最小二乘回归方法相比，人们可以期望得到更好的逼近结果。但是，请记住，该方法需要有序的（并且“无噪声”）数据，并且该方法仅限于低维问题。它的计算要求也更高，因此在某些用例中可能比回归花费（更长）的时间。  
  
## 凸优化  
  
在金融和经济学中，凸优化起着重要作用。例如，选项定价模型对市场数据的校准，或代理人效用函数的优化。例如，以 `fm()` 函数为例：  
  
```python  
def fm(p):  
    x, y = p  
    return (np.sin(x) + 0.05 * x ** 2  
            + np.sin(y) + 0.05 * y ** 2)  
```  
  
图11-14以图形方式显示了该函数在 $x$ 和 $y$ 既定区间的状态。直观观察已发现此函数有多个局部极小值。全局极小值的存在无法被此特定的图形表示真正证实，但它似乎存在：  
  
```python  
x = np.linspace(-10, 10, 50)  
y = np.linspace(-10, 10, 50)  
X, Y = np.meshgrid(x, y)  
Z = fm((X, Y))  
  
fig = plt.figure(figsize=(10, 6))  
ax = fig.gca(projection='3d')  
surf = ax.plot_surface(X, Y, Z, rstride=2, cstride=2,  
                       cmap='coolwarm', linewidth=0.5,  
                       antialiased=True)  
ax.set_xlabel('x')  
ax.set_ylabel('y')  
ax.set_zlabel('f(x, y)')  
fig.colorbar(surf, shrink=0.5, aspect=5)  
```  
  
<img width="665" height="456" alt="python_for_finance 11 14" src="https://github.com/user-attachments/assets/1305521d-9c0d-4709-baa5-ffe98dcbbfb1" />
  
图11-14. 线性样条插值（数据子集）  
  
### 全局优化  
  
在下文中，我们将实施全局最小化方法和局部最小化方法。应用到的 `sco.brute()` 和 `sco.fmin()` 函数来自 `scipy.optimize` 。  
  
为了在最小化过程期间近距离观察幕后情况，以下代码在原始函数上增加了一个输出当前参数值以及函数值的选项。这使我们能够跟踪过程中的所有相关信息：  
  
```python  
import scipy.optimize as sco # 从SciPy导入所需的子包。  
  
def fo(p):  
    x, y = p  
    z = np.sin(x) + 0.05 * x ** 2 + np.sin(y) + 0.05 * y ** 2  
    if output == True:  
        print('%8.4f | %8.4f | %8.4f' % (x, y, z)) # 如果output = True则打印输出信息。  
    return z  
  
output = True  
sco.brute(fo, ((-10, 10.1, 5), (-10, 10.1, 5)), finish=None) # 蛮力优化。  
```  
  
```txt  
-10.0000 | -10.0000 |  11.0880  
-10.0000 | -10.0000 |  11.0880  
-10.0000 |  -5.0000 |   7.7529  
-10.0000 |   0.0000 |   5.5440  
-10.0000 |   5.0000 |   5.8351  
-10.0000 |  10.0000 |  10.0000  
 -5.0000 | -10.0000 |   7.7529  
 -5.0000 |  -5.0000 |   4.4178  
 -5.0000 |   0.0000 |   2.2089  
 -5.0000 |   5.0000 |   2.5000  
 -5.0000 |  10.0000 |   6.6649  
  0.0000 | -10.0000 |   5.5440  
  0.0000 |  -5.0000 |   2.2089  
  0.0000 |   0.0000 |   0.0000  
  0.0000 |   5.0000 |   0.2911  
  0.0000 |  10.0000 |   4.4560  
  5.0000 | -10.0000 |   5.8351  
  5.0000 |  -5.0000 |   2.5000  
  5.0000 |   0.0000 |   0.2911  
  5.0000 |   5.0000 |   0.5822  
  5.0000 |  10.0000 |   4.7471  
 10.0000 | -10.0000 |  10.0000  
 10.0000 |  -5.0000 |   6.6649  
 10.0000 |   0.0000 |   4.4560  
 10.0000 |   5.0000 |   4.7471  
 10.0000 |  10.0000 |   8.9120  
  
Out[63]: array([0., 0.])  
```  
  
给定函数的初始参数化，最优参数值为 $x = y = 0$ 。正如前面输出内容的快速回顾所揭示的，产生的函数值也是0。人们可能倾向于接受它作为全局极小值。然而，此处的第一个参数化非常粗糙，因为对两个输入参数使用了步长为5。这当然可以大幅改进，从而在此处获得更好的结果——并表明先前的解不是最优解：  
  
```python  
output = False  
opt1 = sco.brute(fo, ((-10, 10.1, 0.1), (-10, 10.1, 0.1)), finish=None)  
  
opt1  
```  
  
```txt  
Out[65]: array([-1.4, -1.4])  
```  
  
```python  
fm(opt1)  
```  
  
```txt  
Out[66]: -1.7748994599769203  
```  
  
此时的最优参数值为 $x = y = -1.4$ ，全局最小化的最小函数值约为–1.7749。  
  
### 局部优化  
  
接下来的局部凸优化利用了来自全局优化的结果。 `sco.fmin()` 函数将要最小化的函数和起始参数值作为输入。可选的参数值包含输入参数容差和函数值容差，以及最大迭代次数和最大函数调用次数。局部优化进一步改善了结果：  
  
```python  
output = True  
opt2 = sco.fmin(fo, opt1, xtol=0.001, ftol=0.001,  
                maxiter=15, maxfun=20) # 局部凸优化。  
```  
  
```txt  
 -1.4000 |  -1.4000 |  -1.7749  
 -1.4700 |  -1.4000 |  -1.7743  
 -1.4000 |  -1.4700 |  -1.7743  
 -1.3300 |  -1.4700 |  -1.7696  
 -1.4350 |  -1.4175 |  -1.7756  
 -1.4350 |  -1.3475 |  -1.7722  
 -1.4088 |  -1.4394 |  -1.7755  
 -1.4438 |  -1.4569 |  -1.7751  
 -1.4328 |  -1.4427 |  -1.7756  
 -1.4591 |  -1.4208 |  -1.7752  
 -1.4213 |  -1.4347 |  -1.7757  
 -1.4235 |  -1.4096 |  -1.7755  
 -1.4305 |  -1.4344 |  -1.7757  
 -1.4168 |  -1.4516 |  -1.7753  
 -1.4305 |  -1.4260 |  -1.7757  
 -1.4396 |  -1.4257 |  -1.7756  
 -1.4259 |  -1.4325 |  -1.7757  
 -1.4259 |  -1.4241 |  -1.7757  
 -1.4304 |  -1.4177 |  -1.7757  
 -1.4270 |  -1.4288 |  -1.7757  
Warning: Maximum number of function evaluations has been exceeded.  
```  
  
```python  
opt2  
```  
  
```txt  
Out[68]: array([-1.42702972, -1.42876755])  
```  
  
```python  
fm(opt2)  
```  
  
```txt  
Out[69]: -1.7757246992239009  
```  
  
对于许多凸优化问题，建议在进行局部最小化之前先进行全局最小化。其主要原因是，局部凸优化算法很容易陷入局部最小值（或进行“盆地跳跃”），从而完全忽略更好的局部极小值和/或全局极小值。以下展示表明，将起始参数化设置为 $x = y = 2$ 得到例如大于零的“最小值”值：  
  
```python  
output = False  
sco.fmin(fo, (2.0, 2.0), maxiter=250)  
```  
  
```txt  
Optimization terminated successfully.  
         Current function value: 0.015826  
         Iterations: 46  
         Function evaluations: 86  
  
Out[70]: array([4.2710728 , 4.27106945])  
```  
  
### 约束优化  
  
到目前为止，本节仅考虑了无约束的优化问题。然而，大量的经济或金融优化问题受到一个或多个约束的限制。这些约束在形式上可以表现为等式或不等式。  
  
作为一个简单的例子，考虑一个（预期效用最大化）投资者的效用最大化问题，该投资者可以投资于两种有风险的证券。今天两种证券的成本为 $q_a = q_b = 10$ 美元。一年后，在状态 $u$ 下它们的收益分别为 $15$ 美元和 $5$ 美元，而在状态 $d$ 下分别为 $5$ 美元和 $12$ 美元。两种状态同样可能发生。用 $r_a$ 和 $r_b$ 分别表示这两种证券的收益向量。  
  
投资者有 $w_0 = 100$ 美元的预算进行投资，并且根据效用函数 $u(w) = \sqrt{w}$ 从未来财富中获得效用，其中 $w$ 为可用的财富（美元金额）。等式11-2是最大化问题的公式，其中 $a$ , $b$ 是投资者购买的证券数量。  
  
等式11-2. 预期效用最大化问题 (1)  
  
$$\begin{aligned} \max_{a,b} \text{E}(u(w_1)) &= p\sqrt{w_{1u}} + (1 - p)\sqrt{w_{1d}} \\ w_1 &= a \cdot r_a + b \cdot r_b \\ w_0 &\geq a \cdot q_a + b \cdot q_b \\ a, b &\geq 0 \end{aligned}$$  
  
代入所有数字假设，就可以得到等式11-3中的问题。请注意，这里转变为求负期望效用的最小值。  
  
等式11-3. 预期效用最大化问题 (2)  
  
$$\begin{aligned} \min_{a,b} -\text{E}(u(w_1)) &= -(0.5 \cdot \sqrt{w_{1u}} + 0.5 \cdot \sqrt{w_{1d}}) \\ w_{1u} &= a \cdot 15 + b \cdot 5 \\ w_{1d} &= a \cdot 5 + b \cdot 12 \\ 100 &\geq a \cdot 10 + b \cdot 10 \\ a, b &\geq 0 \end{aligned}$$  
  
解决这个问题，适合使用 `scipy.optimize.minimize()` 函数。除了要最小化的函数外，该函数还接受等式和不等式形式的条件（作为字典对象的列表）以及参数的边界（作为元组对象的元组）作为输入。[^1]下面的代码将等式11-3中的问题翻译成Python代码：  
  
[^1]: 有关如何使用minimize函数的详细信息和示例，请参阅文档。  
  
```python  
import math  
  
def Eu(p): # 要最小化的函数，以最大化期望效用。  
    s, b = p  
    return -(0.5 * math.sqrt(s * 15 + b * 5) +  
             0.5 * math.sqrt(s * 5 + b * 12))  
  
cons = ({'type': 'ineq',  
         'fun': lambda p: 100 - p[0] * 10 - p[1] * 10}) # 作为dict对象的不等式约束。  
  
bnds = ((0, 1000), (0, 1000)) # 参数的边界值（选得足够宽）。  
  
result = sco.minimize(Eu, [5, 5], method='SLSQP',  
                      bounds=bnds, constraints=cons) # 约束优化。  
```  
  
结果对象包含所有相关信息。关于最小函数值，我们需要记住把正负号换回来：  
  
```python  
result  
```  
  
```txt  
Out[76]:      fun: -9.700883611487832  
             jac: array([-0.48508096, -0.48489535])  
         message: 'Optimization terminated successfully.'  
            nfev: 21  
             nit: 5  
            njev: 5  
          status: 0  
         success: True  
               x: array([8.02547122, 1.97452878])  
```  
  
```python  
result['x'] # 最优参数值（即最优投资组合）。  
```  
  
```txt  
Out[77]: array([8.02547122, 1.97452878])  
```  
  
```python  
-result['fun'] # 作为最佳解值的负最小函数值。  
```  
  
```txt  
Out[78]: 9.700883611487832  
```  
  
```python  
np.dot(result['x'], [10, 10]) # 预算约束具有约束力；所有财富都已投资。  
```  
  
```txt  
Out[79]: 99.99999999999999  
```  
  
## 积分  
  
特别是在涉及估值和期权定价时，积分是一个重要的数学工具。这是由于在一般情况下，衍生品的风险中性价值可以表示为在风险中性或鞅测度下其收益的贴现期望。而期望在离散情况下是一个总和，在连续情况下是一个积分。 `scipy.integrate` 子包提供了用于数值积分的各种函数。这里的示例函数我们在第312页的“逼近”中已经熟悉：  
  
```python  
import scipy.integrate as sci  
  
def f(x):  
    return np.sin(x) + 0.5 * x  
```  
  
积分区间设定为 $[0.5, 9.5]$ ，这就得出了如等式11-4所示的定积分。  
  
等式11-4. 示例函数的积分  
  
$$\int_{0.5}^{9.5} f(x)dx = \int_{0.5}^{9.5} \sin(x) + \frac{x}{2} dx$$  
  
以下代码定义了计算该积分的主要Python对象：  
  
```python  
x = np.linspace(0, 10)  
y = f(x)  
a = 0.5 # 左积分极限。  
b = 9.5 # 右积分极限。  
Ix = np.linspace(a, b) # 积分区间值。  
Iy = f(Ix) # 积分函数值。  
```  
  
图11-15将积分值可视化为函数下方具有灰色阴影的区域：  
  
```python  
from matplotlib.patches import Polygon  
  
fig, ax = plt.subplots(figsize=(10, 6))  
plt.plot(x, y, 'b', linewidth=2)  
plt.ylim(bottom=0)  
Ix = np.linspace(a, b)  
Iy = f(Ix)  
verts = [(a, 0)] + list(zip(Ix, Iy)) + [(b, 0)]  
poly = Polygon(verts, facecolor='0.7', edgecolor='0.5')  
ax.add_patch(poly)  
plt.text(0.75 * (a + b), 1.5, r"$\int_a^b f(x)dx$",  
         horizontalalignment='center', fontsize=20)  
plt.figtext(0.9, 0.075, '$x$')  
plt.figtext(0.075, 0.9, '$f(x)$')  
ax.set_xticks((a, b))  
ax.set_xticklabels(('$a$', '$b$'))  
ax.set_yticks([f(a), f(b)]);  
```  
  
<img width="666" height="439" alt="python_for_finance 11 15" src="https://github.com/user-attachments/assets/0a1f6434-016e-4857-9d68-d8346bec9a65" />
  
图11-15. 作为阴影区域的积分值  
  
### 数值积分  
  
`scipy.integrate` 子包包含了多个函数，用于针对上限和下限积分界限，对给定的数学函数进行数值积分。示例包括用于固定高斯正交的 `sci.fixed_quad()` ，用于自适应正交的 `sci.quad()` ，以及用于龙贝格积分的 `sci.romberg()` ：  
  
```python  
sci.fixed_quad(f, a, b)[0]  
```  
  
```txt  
Out[85]: 24.366995967084602  
```  
  
```python  
sci.quad(f, a, b)[0]  
```  
  
```txt  
Out[86]: 24.374754718086752  
```  
  
```python  
sci.romberg(f, a, b)  
```  
  
```txt  
Out[87]: 24.374754718086713  
```  
  
还有许多积分函数采用具有函数值和输入值的 `list` 或 `ndarray` 对象作为输入。这方面的示例包括使用梯形法则的 `sci.trapz()` ，以及实施辛普森法则的 `sci.simps()` ：  
  
```python  
xi = np.linspace(0.5, 9.5, 25)  
  
sci.trapz(f(xi), xi)  
```  
  
```txt  
Out[89]: 24.352733271544516  
```  
  
```python  
sci.simps(f(xi), xi)  
```  
  
```txt  
Out[90]: 24.37496418455075  
```  
  
### 通过模拟积分  
  
通过蒙特卡罗模拟对期权和衍生品进行估值依赖于一个见解，即可以通过模拟来计算积分。为此，我们在积分限之间抽取 $I$ 个 $x$ 的随机值，并针对每一个随机的 $x$ 值计算积分函数。将所有的函数值相加并取平均值，即可得到在积分区间上的平均函数值。将该值乘以积分区间的长度即可得出对该积分值的估计。  
  
以下代码展示了蒙特卡罗估计的积分值如何（尽管不是单调地）收敛至真实积分值，随着抽样数量不断增加。对于相对较小的随机抽取数量，估计值已经非常接近：  
  
```python  
for i in range(1, 20):  
    np.random.seed(1000)  
    x = np.random.random(i * 10) * (b - a) + a # 每次迭代都会增加随机x值的数量。  
    print(np.mean(f(x)) * (b - a))  
```  
  
```txt  
24.804762279331463  
26.522918898332378  
26.265547519223976  
26.02770339943824  
24.99954181440844  
23.881810141621663  
23.527912274843253  
23.507857658961207  
23.67236746066989  
23.679410416062886  
24.424401707879305  
24.239005346819056  
24.115396924962802  
24.424191987566726  
23.924933080533783  
24.19484212027875  
24.117348378249833  
24.100690929662274  
23.76905109847816  
```  
  
## 符号计算  
  
前面几节主要涉及数值计算。现在本节介绍符号计算，其可有益地应用于金融领域的许多方面。为此，通常使用专门致力于符号计算的SymPy库。  
  
### 基础知识  
  
SymPy引入了新的对象类。一个基础的类是 `Symbol` 类：  
  
```python  
import sympy as sy  
  
x = sy.Symbol('x') # 定义要处理的符号。  
y = sy.Symbol('y')   
  
type(x)  
```  
  
```txt  
Out[94]: sympy.core.symbol.Symbol  
```  
  
```python  
sy.sqrt(x) # 对符号应用函数。  
```  
  
```txt  
Out[95]: sqrt(x)  
```  
  
```python  
3 + sy.sqrt(x) - 4 ** 2 # 在符号上定义的数值表达式。  
```  
  
```txt  
Out[96]: sqrt(x) - 13  
```  
  
```python  
f = x ** 2 + 3 + 0.5 * x ** 2 + 3 / 2 # 符号定义的函数。  
  
sy.simplify(f) # 简化的函数表达式。  
```  
  
```txt  
Out[98]: 1.5*x**2 + 4.5  
```  
  
这已经说明了与常规Python代码的一个重大区别。尽管 $x$ 没有数值，但自 $x$ 是一个 `Symbol` 对象以来， $x$ 的平方根就在SymPy中定义了。在这个意义上， `sy.sqrt(x)` 可以是任意数学表达式的一部分。请注意，SymPy通常会自动简化给定的数学表达式。同样地，可以使用 `Symbol` 对象定义任意函数。这不可与Python函数混淆。  
  
SymPy为数学表达式提供了三种基本的渲染器：  
  
*   基于LaTeX  
*   基于Unicode  
*   基于ASCII  
  
例如，当仅在Jupyter Notebook环境（基于HTML）中工作时，LaTeX渲染通常是一个不错的（即具有视觉吸引力的）选择。下面的代码采用最简单的选项（ASCII），以说明不涉及任何手动排版工作：  
  
```python  
sy.init_printing(pretty_print=False, use_unicode=False)  
  
print(sy.pretty(f))  
```  
  
```txt  
     2  
1.5*x  + 4.5  
```  
  
```python  
print(sy.pretty(sy.sqrt(x) + 0.5))  
```  
  
```txt  
  ___  
\/ x  + 0.5  
```  
  
本节不能深入探讨细节，但SymPy也提供了许多其他有用的数学函数——例如，在涉及到数值评估 $\pi$ 的时候。以下示例显示了高达第400,000位数字时， $\pi$ 的字符串表示的前40个和最后40个字符。它还在其中搜索一个包含六位数的日历年月生日——这是在某些数学和IT界里很流行的任务：  
  
```python  
%time pi_str = str(sy.N(sy.pi, 400000)) # 返回π的前400,000位数字的字符串表示。  
```  
  
```txt  
CPU times: user 400 ms, sys: 10.9 ms, total: 411 ms  
Wall time: 501 ms  
```  
  
```python  
pi_str[:42] # 显示前40位数字……  
```  
  
```txt  
Out[103]: '3.1415926535897932384626433832795028841971'  
```  
  
```python  
pi_str[-40:] # ……以及最后40位数字。  
```  
  
```txt  
Out[104]: '8245672736856312185020980470362464176198'  
```  
  
```python  
%time pi_str.find('061072') # 在字符串中搜索生日日期。  
```  
  
```txt  
CPU times: user 115 µs, sys: 1e+03 ns, total: 116 µs  
Wall time: 120 µs  
  
Out[105]: 80847  
```  
  
### 方程式  
  
SymPy的一个优势是解方程式，例如形式为 $x^2 – 1 = 0$ 。通常，SymPy预设读者正在寻求使给定表达式等于零而获得的方程解答。因此，可能必须重新构造诸如 $x^2 – 1 = 3$ 这样的方程式以获得所需的结果。当然，SymPy可以处理更复杂的表达式，如 $x^3 + 0.5 x^2 – 1 = 0$ 。最后，它还可以处理涉及虚数的问题，例如 $x^2 + y^2 = 0$ ：  
  
```python  
sy.solve(x ** 2 - 1)  
```  
  
```txt  
Out[106]: [-1, 1]  
```  
  
```python  
sy.solve(x ** 2 - 1 - 3)  
```  
  
```txt  
Out[107]: [-2, 2]  
```  
  
```python  
sy.solve(x ** 3 + 0.5 * x ** 2 - 1)  
```  
  
```txt  
Out[108]: [0.858094329496553, -0.679047164748276 - 0.839206763026694*I,  
 -0.679047164748276 + 0.839206763026694*I]  
```  
  
```python  
sy.solve(x ** 2 + y ** 2)  
```  
  
```txt  
Out[109]: [{x: -I*y}, {x: I*y}]  
```  
  
### 积分与微分  
  
SymPy的另一个优势是积分和微分。下面的示例重新审视了用于基于数值和模拟积分的示例函数，并得出了符号上和数值上的精确解。我们需要针对积分界限的符号对象才能开始：  
  
```python  
a, b = sy.symbols('a b') # 积分界限的Symbol对象。  
  
I = sy.Integral(sy.sin(x) + 0.5 * x, (x, a, b)) # 定义并进行了格式化打印(pretty-printed)的Integral对象。  
  
print(sy.pretty(I))   
```  
  
```txt  
  b  
  /  
 |  
 |  (0.5*x + sin(x)) dx  
 |  
 /  
 a  
```  
  
```python  
int_func = sy.integrate(sy.sin(x) + 0.5 * x, x) # 推导并格式化打印了不定积分。  
  
print(sy.pretty(int_func))   
```  
  
```txt  
      2  
0.25*x  - cos(x)  
```  
  
```python  
Fb = int_func.subs(x, 9.5).evalf() # 通过.subs()和.evalf()方法获得不定积分在极限处的值。  
Fa = int_func.subs(x, 0.5).evalf()   
  
Fb - Fa # 积分的精确数值。  
```  
  
```txt  
Out[116]: 24.3747547180867  
```  
  
也可以使用符号积分限对积分进行符号求解：  
  
```python  
int_func_limits = sy.integrate(sy.sin(x) + 0.5 * x, (x, a, b)) # 符号上求解积分。  
  
print(sy.pretty(int_func_limits))   
```  
  
```txt  
        2         2  
- 0.25*a  + 0.25*b  + cos(a) - cos(b)  
```  
  
```python  
int_func_limits.subs({a : 0.5, b : 9.5}).evalf() #在替换期间使用dict对象以数值方式求解积分。  
```  
  
```txt  
Out[119]: 24.3747547180868  
```  
  
```python  
sy.integrate(sy.sin(x) + 0.5 * x, (x, 0.5, 9.5)) # 单步求积分的数值。  
```  
  
```txt  
Out[120]: 24.3747547180867  
```  
  
### 微分  
  
不定积分的导数通常产生原函数。将 `sy.diff()` 函数应用于符号不定积分，可以说明这一点：  
  
```python  
int_func.diff()  
```  
  
```txt  
Out[121]: 0.5*x + sin(x)  
```  
  
如同积分示例一样，现在将使用微分来推导本章前面探讨的凸优化问题的精确解。为此，通过符号化地定义各函数，从而得出偏导数，并确定根：  
  
```python  
f = (sy.sin(x) + 0.05 * x ** 2  
     + sy.sin(y) + 0.05 * y ** 2) # 函数的符号版本。  
  
del_x = sy.diff(f, x) # 推导并打印两个偏导数。  
del_x   
```  
  
```txt  
Out[123]: 0.1*x + cos(x)  
```  
  
```python  
del_y = sy.diff(f, y)   
del_y   
```  
  
```txt  
Out[124]: 0.1*y + cos(y)  
```  
  
对全局极小值的必要但不充分条件是这两个偏导数皆为零。然而，无法保证会有符号解。算法和（多重）存在性问题都会在这里发挥作用。不过，人们可以通过数值求解那两个一阶条件，基于前面的全局和局部最小化计算努力提供“有教养的”猜测：  
  
```python  
xo = sy.nsolve(del_x, -1.5) # 为寻找根提供的“有教养的猜测”和得出的最优值。  
xo   
```  
  
```txt  
Out[125]: -1.42755177876459  
```  
  
```python  
yo = sy.nsolve(del_y, -1.5)   
yo   
```  
  
```txt  
Out[126]: -1.42755177876459  
```  
  
```python  
f.subs({x : xo, y : yo}).evalf() # 全局极小函数的对应值。  
```  
  
```txt  
Out[127]: -1.77572565314742  
```  
  
重申一次，提供未经训练/任意的猜测可能会使算法陷入局部极小值，而不是全局最小值：  
  
```python  
xo = sy.nsolve(del_x, 1.5) # 对根的未受教育猜测。  
xo  
```  
  
```txt  
Out[128]: 1.74632928225285  
```  
  
```python  
yo = sy.nsolve(del_y, 1.5)   
yo  
```  
  
```txt  
Out[129]: 1.74632928225285  
```  
  
```python  
f.subs({x : xo, y : yo}).evalf() # 局部最小值函数值。  
```  
  
```txt  
Out[130]: 2.27423381055640  
```  
  
这在数值上说明了，一阶条件是必需的，但并非充分的。  
  
> **符号计算**  
> 在使用Python进行（金融）数学运算时，SymPy和符号计算被证明是一种有价值的工具。特别是对于交互式金融分析而言，与非符号方法相比，这可以是一种更有效的方法。  
  
## 结论  
  
本章涵盖了金融界重要的一些特定数学主题和工具。例如，函数的逼近在许多金融领域都很重要，比如基于因子的模型、收益率曲线插值以及美式期权的基于回归的蒙特卡罗估值方法。金融领域也经常需要凸优化技术；例如，在针对市场报价或者期权的隐含波动率，来校准带参数的期权定价模型时。  
  
数值积分是核心工具，例如对于期权和衍生品的定价。在推导出（一组）随机过程的风险中性概率测度之后，期权定价归结为求取期权收益在风险中性测度下的期望，并将此数值折现到当前日期。第12章介绍了风险中性测度下几种类型随机过程的模拟。  
  
最后，本章介绍了使用SymPy的符号计算。对于诸如积分、微分或方程式求解之类的许多数学运算，符号计算能被证明是一个有用且有效的工具。  
  
## 进一步的资源  
  
关于本章中使用的Python库的更多信息，请查阅以下网络资源：  
  
*   有关本章中使用的NumPy函数的详细信息，请参见NumPy参考资料。  
*   访问SciPy有关优化和求根的文档以获取 `scipy.optimize` 的详细信息。  
*   使用 `scipy.integrate` 的积分说明在“Integration and ODEs”中。  
*   SymPy网站提供了大量的示例和详细的文档。  
  
有关本章涵盖主题的数学参考，请参见：  
  
*   Brandimarte, Paolo (2006). Numerical Methods in Finance and Economics: A MATLAB-Based Introduction. 第2版, 霍博肯，新泽西州: John Wiley & Sons.  
