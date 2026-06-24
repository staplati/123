# 第10章 高性能Python  
  
不要为了迎合你的表现而降低你的期望。提高你的表现水平以满足你的期望。  
——拉尔夫·马斯顿  
  
长期以来存在一种偏见，认为Python本身是一种相对较慢的编程语言，不适合在金融领域实现计算要求高的任务。除了Python是一种解释型语言这一事实外，推理通常遵循以下思路：Python在处理循环时很慢；实现金融算法通常需要循环；因此Python对于金融算法的实现来说太慢了。另一种推理是：其他（编译型）编程语言在执行循环时很快（例如C或C++）；金融算法通常需要循环；因此这些（编译型）编程语言非常适合金融和金融算法的实现。  
  
不可否认，编写出执行速度相当慢——也许对许多应用领域来说太慢了——的正确Python代码是可能的。本章是关于加速在金融背景中经常遇到的典型任务和算法的方法。它表明，通过明智地使用数据结构、选择正确的实现习惯用法和范式，以及使用正确的性能包，Python甚至能够与编译型编程语言竞争。除其他因素外，这是由于其本身得到了编译。  
  
为此，本章介绍了不同的加速代码的方法：  
  
向量化  
利用Python的向量化功能是前面章节中已经广泛使用的一种方法。  
  
动态编译  
使用Numba包允许使用LLVM技术动态编译纯Python代码。  
  
静态编译  
Cython不仅是一个Python包，而且是结合了Python和C的混合语言；例如，它允许使用静态类型声明并静态编译此类调整后的代码。  
  
多进程  
Python的multiprocessing模块允许轻松简单地并行执行代码。  
  
本章讨论以下主题：  
  
“循环”在第276页  
本节讨论Python循环以及如何加速它们。  
  
“算法”在第281页  
本节涉及通常用于性能基准测试的标准数学算法，例如斐波那契数列生成。  
  
“二叉树”在第294页  
二叉树期权定价模型是一种广泛使用的金融模型，它允许对更复杂的金融算法进行有趣的案例研究。  
  
“蒙特卡洛模拟”在第299页  
同样，蒙特卡洛模拟在金融实践中广泛用于定价和风险管理。它的计算要求很高，长期以来一直被认为是C或C++等语言的领域。  
  
“递归pandas算法”在第304页  
本节讨论基于金融时间序列数据的递归算法的加速。特别是，它提出了一种计算指数加权移动平均（EWMA）算法的不同实现。  
  
## 循环  
  
本节解决Python循环问题。任务相当简单：将编写一个函数，提取一定“大量”的随机数并返回这些值的平均值。执行时间是人们感兴趣的，可以通过魔法函数%time和%timeit来估计。  
  
### Python  
  
让我们“慢慢”开始——请原谅这个双关语。在纯Python中，这样一个函数可能看起来像average_py()：  
  
```python  
import random  
  
def average_py(n):  
    s = 0 # ❶ 初始化s的变量值。  
    for i in range(n):  
        s += random.random() # ❷ 将区间(0, 1)内的均匀分布随机值加到s上。  
    return s / n # ❸ 返回平均值（均值）。  
  
n = 10000000 # ❹ 定义循环的迭代次数。  
  
%time average_py(n) # ❺ 对函数进行一次计时。  
```  
  
```txt  
CPU times: user 1.82 s, sys: 10.4 ms, total: 1.83 s  
Wall time: 1.93 s  
Out[4]: 0.5000590124747943  
```  
  
```python  
%timeit average_py(n) # ❻ 对函数进行多次计时以获得更可靠的估计。  
```  
  
```txt  
1.31 s ± 159 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
```python  
%time sum([random.random() for _ in range(n)]) / n # ❼ 使用列表推导式代替该函数。  
```  
  
```txt  
CPU times: user 1.55 s, sys: 188 ms, total: 1.74 s  
Wall time: 1.74 s  
Out[6]: 0.49987031710661173  
```  
  
这为后续的其他方法设定了基准。  
  
### NumPy  
  
NumPy的优势在于其向量化功能。从形式上看，循环在Python层面上消失了；循环发生在更深的一层，基于NumPy提供的优化和编译过的例程。1 函数average_np()利用了这种方法：  
  
```python  
import numpy as np  
  
def average_np(n):  
    s = np.random.random(n) # ❶ “一次性”提取随机数（没有Python循环）。  
    return s.mean() # ❷ 返回平均值（均值）。  
  
%time average_np(n)  
```  
  
```txt  
CPU times: user 180 ms, sys: 43.2 ms, total: 223 ms  
Wall time: 224 ms  
Out[9]: 0.49988861556468317  
```  
  
```python  
%timeit average_np(n)  
```  
  
```txt  
128 ms ± 2.01 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)  
```  
  
```python  
s = np.random.random(n)  
s.nbytes # ❸ 创建的ndarray对象使用的字节数。  
```  
  
```txt  
Out[11]: 80000000  
```  
  
这种加速是相当可观的，几乎达到了10倍或一个数量级。然而，必须付出的代价是显著提高的内存使用量。这是因为NumPy通过预先分配可以在编译层中处理的数据来获得速度。因此，在给定这种方法的情况下，无法处理“流式”数据。根据手头的算法或问题，这种增加的内存使用量甚至可能大得令人望而却步。  
  
**向量化和内存**  
由于通常观察到的简洁语法和速度提升，人们很想尽可能用NumPy编写向量化代码。然而，这些好处往往是以更高的内存占用为代价的。  
  
1 NumPy还可以利用专用的数学库，例如英特尔数学核心函数库（MKL）。  
  
### Numba  
  
Numba是一个允许使用LLVM动态编译纯Python代码的包。在像手头这样的简单情况下的应用出奇地简单，并且动态编译的函数average_nb()可以直接从Python调用：  
  
```python  
import numba  
  
average_nb = numba.jit(average_py) # ❶ 这创建了Numba函数。  
  
%time average_nb(n) # ❷ 编译在运行时发生，导致一些开销。  
```  
  
```txt  
CPU times: user 204 ms, sys: 34.3 ms, total: 239 ms  
Wall time: 278 ms  
Out[14]: 0.4998865391283664  
```  
  
```python  
%time average_nb(n) # ❸ 从第二次执行开始（使用相同的输入数据类型），执行速度更快。  
```  
  
```txt  
CPU times: user 80.9 ms, sys: 457 µs, total: 81.3 ms  
Wall time: 81.7 ms  
Out[15]: 0.5001357454250273  
```  
  
```python  
%timeit average_nb(n)  
```  
  
```txt  
75.5 ms ± 1.95 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)  
```  
  
纯Python与Numba的结合击败了NumPy版本，并保留了原始基于循环的实现的内存效率。同样明显的是，在这样简单的用例中应用Numba几乎没有任何编程开销。  
  
**没有免费的午餐**  
当人们将Python代码的性能与编译版本进行比较时，Numba的应用有时看起来像魔法一样，特别是考虑到它的易用性。然而，有许多用例不适合使用Numba，对于这些用例几乎观察不到性能提升，甚至不可能实现。  
  
### Cython  
  
Cython允许静态编译Python代码。然而，它的应用并不像Numba那样简单，因为代码通常需要更改才能看到显著的速度提升。首先，考虑Cython函数average_cy1()，它为使用的变量引入了静态类型声明：  
  
```python  
%load_ext Cython  
```  
  
```python  
%%cython -a  
import random # ❶ 在Cython上下文中导入random模块。  
  
def average_cy1(int n): # ❷ 为变量n、i和s添加静态类型声明。  
    cdef int i # ❷ 为变量n、i和s添加静态类型声明。  
    cdef float s = 0 # ❷ 为变量n、i和s添加静态类型声明。  
    for i in range(n):  
        s += random.random()  
    return s / n  
```  
  
```txt  
Out[18]: <IPython.core.display.HTML object>  
```  
  
```python  
%time average_cy1(n)  
```  
  
```txt  
CPU times: user 695 ms, sys: 4.31 ms, total: 699 ms  
Wall time: 711 ms  
Out[19]: 0.49997106194496155  
```  
  
```python  
%timeit average_cy1(n)  
```  
  
```txt  
752 ms ± 91.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
观察到了一些加速，但甚至无法接近例如NumPy版本所达到的速度。需要多一点Cython优化才能击败甚至Numba版本：  
  
```python  
%%cython  
from libc.stdlib cimport rand # ❶ 从C导入随机数生成器。  
cdef extern from 'limits.h': # ❷ 导入随机数缩放的常量值。  
    int INT_MAX # ❷ 导入随机数缩放的常量值。  
cdef int i  
cdef float rn  
for i in range(5):  
    rn = rand() / INT_MAX # ❸ 在缩放后添加来自区间(0, 1)的均匀分布随机数。  
    print(rn)  
```  
  
```txt  
0.6792964339256287  
0.934692919254303  
0.3835020661354065  
0.5194163918495178  
0.8309653401374817  
```  
  
```python  
%%cython -a  
from libc.stdlib cimport rand # ❶ 从C导入随机数生成器。  
cdef extern from 'limits.h': # ❷ 导入随机数缩放的常量值。  
    int INT_MAX # ❷ 导入随机数缩放的常量值。  
  
def average_cy2(int n):  
    cdef int i  
    cdef float s = 0  
    for i in range(n):  
        s += rand() / INT_MAX # ❸ 在缩放后添加来自区间(0, 1)的均匀分布随机数。  
    return s / n  
```  
  
```txt  
Out[22]: <IPython.core.display.HTML object>  
```  
  
```python  
%time average_cy2(n)  
```  
  
```txt  
CPU times: user 78.5 ms, sys: 422 µs, total: 79 ms  
Wall time: 79.1 ms  
Out[23]: 0.500017523765564  
```  
  
```python  
%timeit average_cy2(n)  
```  
  
```txt  
65.4 ms ± 706 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)  
```  
  
这个进一步优化的Cython版本average_cy2()现在比Numba版本稍快一些。然而，所付出的努力也稍微大一些。与NumPy版本相比，Cython同样保留了原始基于循环的实现的内存效率。  
  
**Cython = Python + C**  
Cython允许开发人员尽可能多地或尽可能合理地调整代码以提高性能——例如，从纯Python版本开始，然后向代码中添加越来越多的C元素。编译步骤本身也可以参数化，以进一步优化编译版本。  
  
## 算法  
  
本节将上一节中的性能增强技术应用于数学中的一些众所周知的问题和算法。这些算法经常用于性能基准测试。  
  
### 素数  
  
素数不仅在理论数学中发挥着重要作用，而且在许多应用计算机科学学科（如加密）中也发挥着重要作用。素数是大于1的正自然数，它只能被1和它本身整除而没有余数。没有其他因数。虽然由于其稀有性很难找到更大的素数，但很容易证明一个数字不是素数。唯一需要的是除1以外的一个能整除该数字且没有余数的因数。  
  
#### Python  
  
有许多可用的算法实现来测试数字是否为素数。下面是一个Python版本，从算法的角度来看，它还不是最佳的，但已经相当高效了。然而，较大素数p2的执行时间很长：  
  
```python  
def is_prime(I):  
    if I % 2 == 0: return False # ❶ 如果数字是偶数，则立即返回False。  
    for i in range(3, int(I ** 0.5) + 1, 2): # ❷ 循环从3开始，直到I的平方根加1，步长为2。  
        if I % i == 0: return False # ❸ 一旦找到因数，函数就会返回False。  
    return True # ❹ 如果未找到因数，则返回True。  
  
n = int(1e8 + 3) # ❺ 相对较小的非素数和素数。  
n  
```  
  
```txt  
Out[26]: 100000003  
```  
  
```python  
%time is_prime(n)  
```  
  
```txt  
CPU times: user 35 µs, sys: 0 ns, total: 35 µs  
Wall time: 39.1 µs  
Out[27]: False  
```  
  
```python  
p1 = int(1e8 + 7) # ❺ 相对较小的非素数和素数。  
p1  
```  
  
```txt  
Out[28]: 100000007  
```  
  
```python  
%time is_prime(p1)  
```  
  
```txt  
CPU times: user 776 µs, sys: 1 µs, total: 777 µs  
Wall time: 787 µs  
Out[29]: True  
```  
  
```python  
p2 = 100109100129162907 # ❻ 需要更长执行时间的更大素数。  
  
p2.bit_length()  
```  
  
```txt  
Out[31]: 57  
```  
  
```python  
%time is_prime(p2)  
```  
  
```txt  
CPU times: user 22.6 s, sys: 44.7 ms, total: 22.6 s  
Wall time: 22.7 s  
Out[32]: True  
```  
  
#### Numba  
  
函数is_prime()中算法的循环结构非常适合使用Numba进行动态编译。开销再次极小，但加速相当可观：  
  
```python  
is_prime_nb = numba.jit(is_prime)  
  
%time is_prime_nb(n) # ❶ 第一次调用is_prime_nb()包含编译开销。  
```  
  
```txt  
CPU times: user 87.5 ms, sys: 7.91 ms, total: 95.4 ms  
Wall time: 93.7 ms  
Out[34]: False  
```  
  
```python  
%time is_prime_nb(n) # ❷ 从第二次调用开始，加速变得完全可见。  
```  
  
```txt  
CPU times: user 9 µs, sys: 1e+03 ns, total: 10 µs  
Wall time: 13.6 µs  
Out[35]: False  
```  
  
```python  
%time is_prime_nb(p1)  
```  
  
```txt  
CPU times: user 26 µs, sys: 0 ns, total: 26 µs  
Wall time: 31 µs  
Out[36]: True  
```  
  
```python  
%time is_prime_nb(p2) # ❸ 更大素数的加速大约是一个数量级。  
```  
  
```txt  
CPU times: user 1.72 s, sys: 9.7 ms, total: 1.73 s  
Wall time: 1.74 s  
Out[37]: True  
```  
  
#### Cython  
  
Cython的应用也很简单。没有类型声明的普通Cython版本已经显著加速了代码：  
  
```python  
%%cython  
def is_prime_cy1(I):  
    if I % 2 == 0: return False  
    for i in range(3, int(I ** 0.5) + 1, 2):  
        if I % i == 0: return False  
    return True  
```  
  
```python  
%timeit is_prime(p1)  
```  
  
```txt  
394 µs ± 14.7 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)  
```  
  
```python  
%timeit is_prime_cy1(p1)  
```  
  
```txt  
243 µs ± 6.58 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)  
```  
  
然而，真正的改进只有在使用静态类型声明时才会出现。到那时，Cython版本甚至比Numba版本还要快一些：  
  
```python  
%%cython  
def is_prime_cy2(long I): # ❶ 两个变量I和i的静态类型声明。  
    cdef long i # ❶ 两个变量I和i的静态类型声明。  
    if I % 2 == 0: return False  
    for i in range(3, int(I ** 0.5) + 1, 2):  
        if I % i == 0: return False  
    return True  
```  
  
```python  
%timeit is_prime_cy2(p1)  
```  
  
```txt  
87.6 µs ± 27.7 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)  
```  
  
```python  
%time is_prime_nb(p2)  
```  
  
```txt  
CPU times: user 1.68 s, sys: 9.73 ms, total: 1.69 s  
Wall time: 1.7 s  
Out[43]: True  
```  
  
```python  
%time is_prime_cy2(p2)  
```  
  
```txt  
CPU times: user 1.66 s, sys: 9.47 ms, total: 1.67 s  
Wall time: 1.68 s  
Out[44]: True  
```  
  
### 多进程  
  
到目前为止，所有的优化努力都集中在顺序代码执行上。特别是在素数的情况下，可能需要同时检查多个数字。为此，multiprocessing模块可以帮助进一步加速代码执行。它允许生成并行运行的多个Python进程。在手头的简单用例中，该应用非常直观。首先，设置一个具有多个进程的mp.Pool对象。其次，将要执行的函数映射到要检查的素数上：  
  
```python  
import multiprocessing as mp  
  
pool = mp.Pool(processes=4) # ❶ 实例化具有多个进程的mp.Pool对象。  
  
%time pool.map(is_prime, 10 * [p1]) # ❷ 然后将相应的函数映射到包含素数的列表对象。  
```  
  
```txt  
CPU times: user 1.52 ms, sys: 2.09 ms, total: 3.61 ms  
Wall time: 9.73 ms  
Out[47]: [True, True, True, True, True, True, True, True, True, True]  
```  
  
```python  
%time pool.map(is_prime_nb, 10 * [p2]) # ❷ 然后将相应的函数映射到包含素数的列表对象。  
```  
  
```txt  
CPU times: user 13.9 ms, sys: 4.8 ms, total: 18.7 ms  
Wall time: 10.4 s  
Out[48]: [True, True, True, True, True, True, True, True, True, True]  
```  
  
```python  
%time pool.map(is_prime_cy2, 10 * [p2]) # ❷ 然后将相应的函数映射到包含素数的列表对象。  
```  
  
```txt  
CPU times: user 9.8 ms, sys: 3.22 ms, total: 13 ms  
Wall time: 9.51 s  
Out[49]: [True, True, True, True, True, True, True, True, True, True]  
```  
  
观察到的加速是显著的。对于较大的素数p2，Python函数is_prime()需要20多秒。当使用四个进程并行执行时，is_prime_nb()和is_prime_cy2()函数对于10倍的素数p2耗时均不到10秒。  
  
**并行处理**  
每当需要解决同一类型的不同问题时，都应考虑并行处理。当配备有许多核心和足够工作内存的强大硬件可用时，其效果可能是巨大的。multiprocessing是标准库中一个易于使用的模块。  
  
### 斐波那契数列  
  
斐波那契数和数列可以基于一个简单的算法得出。从两个1开始：1, 1。从第三个数开始，下一个斐波那契数是作为前两个数字的总和得出的：1, 1, 2, 3, 5, 8, 13, 21, …。本节分析了两种不同的实现，一种是递归实现，另一种是迭代实现。  
  
#### 递归算法  
  
与常规的Python循环类似，众所周知，常规的递归函数实现在Python中相对较慢。这样的函数可能会调用自身大量次数以得出最终结果。函数fib_rec_py1()展示了这样一种实现。在这种情况下，Numba对加速执行完全没有帮助。然而，仅基于静态类型声明，Cython就显示出了显著的加速：  
  
```python  
def fib_rec_py1(n):  
    if n < 2:  
        return n  
    else:  
        return fib_rec_py1(n - 1) + fib_rec_py1(n - 2)  
  
%time fib_rec_py1(35)  
```  
  
```txt  
CPU times: user 6.55 s, sys: 29 ms, total: 6.58 s  
Wall time: 6.6 s  
Out[51]: 9227465  
```  
  
```python  
fib_rec_nb = numba.jit(fib_rec_py1)  
  
%time fib_rec_nb(35)  
```  
  
```txt  
CPU times: user 3.87 s, sys: 24.2 ms, total: 3.9 s  
Wall time: 3.91 s  
Out[53]: 9227465  
```  
  
```python  
%%cython  
def fib_rec_cy(int n):  
    if n < 2:  
        return n  
    else:  
        return fib_rec_cy(n - 1) + fib_rec_cy(n - 2)  
  
%time fib_rec_cy(35)  
```  
  
```txt  
CPU times: user 751 ms, sys: 4.37 ms, total: 756 ms  
Wall time: 755 ms  
Out[55]: 9227465  
```  
  
递归算法的主要问题是中间结果没有被缓存，而是被重新计算。为了避免这个特定的问题，可以使用一个装饰器来负责中间结果的缓存。这将执行速度提高了多个数量级：  
  
```python  
from functools import lru_cache as cache  
  
@cache(maxsize=None) # ❶ 缓存中间结果……  
def fib_rec_py2(n):  
    if n < 2:  
        return n  
    else:  
        return fib_rec_py2(n - 1) + fib_rec_py2(n - 2)  
  
%time fib_rec_py2(35) # ❷ ……在这种情况下会导致极大的加速。  
```  
  
```txt  
CPU times: user 64 µs, sys: 28 µs, total: 92 µs  
Wall time: 98 µs  
Out[58]: 9227465  
```  
  
```python  
%time fib_rec_py2(80) # ❷ ……在这种情况下会导致极大的加速。  
```  
  
```txt  
CPU times: user 38 µs, sys: 8 µs, total: 46 µs  
Wall time: 51 µs  
Out[59]: 23416728348467685  
```  
  
#### 迭代算法  
  
尽管计算第 $n$ 个斐波那契数的算法可以递归实现，但它并不一定要这样。下面介绍了一种迭代实现，即使在纯Python中，它也比递归实现的缓存变体更快。这也是Numba带来进一步改进的领域。然而，Cython版本最终胜出：  
  
```python  
def fib_it_py(n):  
    x, y = 0, 1  
    for i in range(1, n + 1):  
        x, y = y, x + y  
    return x  
  
%time fib_it_py(80)  
```  
  
```txt  
CPU times: user 19 µs, sys: 1e+03 ns, total: 20 µs  
Wall time: 26 µs  
Out[61]: 23416728348467685  
```  
  
```python  
fib_it_nb = numba.jit(fib_it_py)  
  
%time fib_it_nb(80)  
```  
  
```txt  
CPU times: user 57 ms, sys: 6.9 ms, total: 63.9 ms  
Wall time: 62 ms  
Out[63]: 23416728348467685  
```  
  
```python  
%time fib_it_nb(80)  
```  
  
```txt  
CPU times: user 7 µs, sys: 1 µs, total: 8 µs  
Wall time: 12.2 µs  
Out[64]: 23416728348467685  
```  
  
```python  
%%cython  
def fib_it_cy1(int n):  
    cdef long i  
    cdef long x = 0, y = 1  
    for i in range(1, n + 1):  
        x, y = y, x + y  
    return x  
  
%time fib_it_cy1(80)  
```  
  
```txt  
CPU times: user 4 µs, sys: 1e+03 ns, total: 5 µs  
Wall time: 11 µs  
Out[66]: 23416728348467685  
```  
  
既然一切都这么快，人们可能会想知道为什么我们只计算第80个斐波那契数，而不是（例如）第150个。问题在于可用的数据类型。虽然Python基本上可以处理任意大的数字（见第62页的“基本数据类型”），但对于编译型语言通常并非如此。然而，在Cython中，可以依赖一种特殊的数据类型来允许数字超过具有64位的双精度浮点对象所允许的限制：  
  
```python  
%%time  
fn = fib_rec_py2(150) # ❶ Python版本快速且正确。  
print(fn) # ❶ Python版本快速且正确。  
```  
  
```txt  
9969216677189303386214405760200  
CPU times: user 361 µs, sys: 115 µs, total: 476 µs  
Wall time: 430 µs  
```  
  
```python  
fn.bit_length() # ❷ 生成的整数的位长为103（> 64）。  
```  
  
```txt  
Out[68]: 103  
```  
  
```python  
%%time  
fn = fib_it_nb(150) # ❸ Numba和Cython版本更快但不正确。  
print(fn) # ❸ Numba和Cython版本更快但不正确。  
```  
  
```txt  
6792540214324356296  
CPU times: user 270 µs, sys: 78 µs, total: 348 µs  
Wall time: 297 µs  
```  
  
```python  
fn.bit_length() # ❹ 由于限制为64位int对象，它们遇到了溢出问题。  
```  
  
```txt  
Out[70]: 63  
```  
  
```python  
%%time  
fn = fib_it_cy1(150) # ❸ Numba和Cython版本更快但不正确。  
print(fn) # ❸ Numba和Cython版本更快但不正确。  
```  
  
```txt  
6792540214324356296  
CPU times: user 255 µs, sys: 71 µs, total: 326 µs  
Wall time: 279 µs  
```  
  
```python  
fn.bit_length() # ❹ 由于限制为64位int对象，它们遇到了溢出问题。  
```  
  
```txt  
Out[72]: 63  
```  
  
```python  
%%cython  
cdef extern from *:  
    ctypedef int int128 '__int128_t' # ❺ 导入特殊的128位int对象类型并使用它。  
def fib_it_cy2(int n):  
    cdef int128 i # ❺ 导入特殊的128位int对象类型并使用它。  
    cdef int128 x = 0, y = 1 # ❺ 导入特殊的128位int对象类型并使用它。  
    for i in range(1, n + 1):  
        x, y = y, x + y  
    return x  
  
%%time  
fn = fib_it_cy2(150) # ❻ Cython版本fib_it_cy2()现在更快且正确。  
print(fn) # ❻ Cython版本fib_it_cy2()现在更快且正确。  
```  
  
```txt  
9969216677189303386214405760200  
CPU times: user 280 µs, sys: 115 µs, total: 395 µs  
Wall time: 328 µs  
```  
  
```python  
fn.bit_length() # ❻ Cython版本fib_it_cy2()现在更快且正确。  
```  
  
```txt  
Out[75]: 103  
```  
  
### 圆周率Pi  
  
本节分析的最终算法是一个基于蒙特卡洛模拟的算法，用于导出数字pi（ $\pi$ ）的位数。2 其基本思想依赖于圆的面积 $A$ 由 $A=\pi r^2$ 给出的事实。因此， $\pi=\frac{A}{r^2}$ 。对于半径为 $r=1$ 的单位圆，可以得出 $\pi=A$ 。该算法的思路是模拟坐标值为 $(x,y)$ 的随机点，其中 $x,y\in[-1,1]$ 。边长为2的以原点为中心的正方形面积恰好为4。以原点为中心的单位圆的面积是该正方形面积的一定比例。这个比例可以通过蒙特卡洛模拟来估计：计算正方形内的所有点，然后计算圆内的所有点，并将圆内的点数除以正方形内的点数。以下示例进行了演示（见图10-1）：  
  
```python  
import random  
import numpy as np  
from pylab import mpl, plt  
plt.style.use('seaborn')  
mpl.rcParams['font.family'] = 'serif'  
%matplotlib inline  
  
rn = [(random.random() * 2 - 1, random.random() * 2 - 1)  
      for _ in range(500)]  
  
rn = np.array(rn)  
rn[:5]  
```  
  
```txt  
Out[78]: array([[ 0.45583018, -0.27676067],  
       [-0.70120038,  0.15196888],  
       [ 0.07224045,  0.90147321],  
       [-0.17450337, -0.47660912],  
       [ 0.94896746, -0.31511879]])  
```  
  
```python  
fig = plt.figure(figsize=(7, 7))  
ax = fig.add_subplot(1, 1, 1)  
circ = plt.Circle((0, 0), radius=1, edgecolor='g', lw=2.0,  
                  facecolor='None') # ❶ 绘制单位圆。  
box = plt.Rectangle((-1, -1), 2, 2, edgecolor='b', alpha=0.3) # ❷ 绘制边长为2的正方形。  
ax.add_patch(circ) # ❶ 绘制单位圆。  
ax.add_patch(box) # ❷ 绘制边长为2的正方形。  
plt.plot(rn[:, 0], rn[:, 1], 'r.') # ❸ 绘制均匀分布的随机点。  
plt.ylim(-1.1, 1.1)  
plt.xlim(-1.1, 1.1)  
```  
  
2 这些例子的灵感来自Code Review Stack Exchange上的一个帖子。  
  
<img width="666" height="663" alt="python_for_finance 10 1" src="https://github.com/user-attachments/assets/88148dfc-9f27-4a93-bdcb-ed43f8496fbe" />
  
图10-1. 单位圆和边长为2的正方形以及均匀分布的随机点  
  
此算法的NumPy实现非常简洁，但也极其耗费内存。在给定参数化的情况下，总执行时间约为一秒钟：  
  
```python  
n = int(1e7)  
  
%time rn = np.random.random((n, 2)) * 2 - 1  
```  
  
```txt  
CPU times: user 450 ms, sys: 87.9 ms, total: 538 ms  
Wall time: 573 ms  
```  
  
```python  
rn.nbytes  
```  
  
```txt  
Out[82]: 160000000  
```  
  
```python  
%time distance = np.sqrt((rn ** 2).sum(axis=1)) # ❶ 那些点到原点的距离（欧几里得范数）。  
distance[:8].round(3)  
```  
  
```txt  
CPU times: user 537 ms, sys: 198 ms, total: 736 ms  
Wall time: 651 ms  
Out[83]: array([1.181, 1.061, 0.669, 1.206, 0.799, 0.579, 0.694, 0.941])  
```  
  
```python  
%time frac = (distance <= 1.0).sum() / len(distance) # ❷ 那些在圆内的点相对于所有点的比例。  
```  
  
```txt  
CPU times: user 47.9 ms, sys: 6.77 ms, total: 54.7 ms  
Wall time: 28 ms  
```  
  
```python  
pi_mcs = frac * 4 # ❸ 为了估计圆的面积，这部分占据了等于4的正方形面积，进而估计出π。  
pi_mcs # ❸ 为了估计圆的面积，这部分占据了等于4的正方形面积，进而估计出π。  
```  
  
```txt  
Out[85]: 3.1413396  
```  
  
mcs_pi_py()是一个Python函数，它使用for循环并以节省内存的方式实现蒙特卡洛模拟。请注意，在这种情况下，随机数没有被缩放。执行时间比NumPy版本要长，但在这种情况下Numba版本比NumPy快：  
  
```python  
def mcs_pi_py(n):  
    circle = 0  
    for _ in range(n):  
        x, y = random.random(), random.random()  
        if (x ** 2 + y ** 2) ** 0.5 <= 1:  
            circle += 1  
    return (4 * circle) / n  
  
%time mcs_pi_py(n)  
```  
  
```txt  
CPU times: user 5.47 s, sys: 23 ms, total: 5.49 s  
Wall time: 5.43 s  
Out[87]: 3.1418964  
```  
  
```python  
mcs_pi_nb = numba.jit(mcs_pi_py)  
  
%time mcs_pi_nb(n)  
```  
  
```txt  
CPU times: user 319 ms, sys: 6.36 ms, total: 326 ms  
Wall time: 326 ms  
Out[89]: 3.1422012  
```  
  
```python  
%time mcs_pi_nb(n)  
```  
  
```txt  
CPU times: user 284 ms, sys: 3.92 ms, total: 288 ms  
Wall time: 291 ms  
```  
  
```txt  
Out[90]: 3.142066  
```  
  
仅带有静态类型声明的纯Cython版本的执行速度并没有比Python版本快多少。然而，再次依靠C的随机数生成能力，进一步显著加快了计算速度：  
  
```python  
%%cython -a  
import random  
def mcs_pi_cy1(int n):  
    cdef int i, circle = 0  
    cdef float x, y  
    for i in range(n):  
        x, y = random.random(), random.random()  
        if (x ** 2 + y ** 2) ** 0.5 <= 1:  
            circle += 1  
    return (4 * circle) / n  
```  
  
```txt  
Out[91]: <IPython.core.display.HTML object>  
```  
  
```python  
%time mcs_pi_cy1(n)  
```  
  
```txt  
CPU times: user 1.15 s, sys: 8.24 ms, total: 1.16 s  
Wall time: 1.16 s  
Out[92]: 3.1417132  
```  
  
```python  
%%cython -a  
from libc.stdlib cimport rand  
cdef extern from 'limits.h':  
    int INT_MAX  
def mcs_pi_cy2(int n):  
    cdef int i, circle = 0  
    cdef float x, y  
    for i in range(n):  
        x, y = rand() / INT_MAX, rand() / INT_MAX  
        if (x ** 2 + y ** 2) ** 0.5 <= 1:  
            circle += 1  
    return (4 * circle) / n  
```  
  
```txt  
Out[93]: <IPython.core.display.HTML object>  
```  
  
```python  
%time mcs_pi_cy2(n)  
```  
  
```txt  
CPU times: user 170 ms, sys: 1.45 ms, total: 172 ms  
Wall time: 172 ms  
Out[94]: 3.1419388  
```  
  
**算法类型**  
本节中分析的算法可能与金融算法没有直接关系。然而，优势在于它们简单且易于理解。此外，在金融环境中遇到的典型算法问题可以在这种简化的环境中进行讨论。  
  
## 二叉树  
  
一种流行的期权估值数值方法是Cox、Ross和Rubinstein（1979）首创的二叉树期权定价模型。这种方法依赖于通过（重组）树来表示资产未来可能的演变。在这个模型中，如同在Black-Scholes-Merton（1973）的设置中一样，有一个风险资产（指数或股票）和一个无风险资产（债券）。从今天到期权到期日的相关时间间隔通常被分为长度为 $\Delta t$ 的等距子区间。给定在时间 $s$ 的指数水平为 $S_s$ ，在 $t=s+\Delta t$ 的指数水平由 $S_t=S_s\cdot m$ 给出，其中 $m$ 是从 $\{u,d\}$ 中随机选择的，具有 $0<d<e^{r\Delta t}<u=e^{\sigma\sqrt{\Delta t}}$ 以及 $u=\frac{1}{d}$ 。 $r$ 是恒定的无风险短期利率。  
  
### Python  
  
以下代码展示了一个Python实现，它基于模型的一些固定数值参数创建了一棵重组树：  
  
```python  
import math  
  
S0 = 36. # ❶ 风险资产的初始值。  
T = 1.0 # ❷ 二叉树模拟的时间范围。  
r = 0.06 # ❸ 恒定短期利率。  
sigma = 0.2 # ❹ 恒定波动率因子。  
  
def simulate_tree(M):  
    dt = T / M # ❺ 时间区间的长度。  
    u = math.exp(sigma * math.sqrt(dt)) # ❻ 向上和向下移动的因子。  
    d = 1 / u # ❻ 向上和向下移动的因子。  
    S = np.zeros((M + 1, M + 1))  
    S[0, 0] = S0  
    z = 1  
    for t in range(1, M + 1):  
        for i in range(z):  
            S[i, t] = S[i, t-1] * u  
            S[i+1, t] = S[i, t-1] * d  
        z += 1  
    return S  
```  
  
与典型的树形图中发生的情况相反，向上的移动在ndarray对象中表示为横向移动，这大大减小了ndarray的大小：  
  
```python  
np.set_printoptions(formatter={'float': lambda x: '%6.2f' % x})  
  
simulate_tree(4) # ❶ 具有4个时间区间的树。  
```  
  
```txt  
Out[99]: array([[ 36.00, 39.79, 43.97, 48.59, 53.71],  
       [  0.00, 32.57, 36.00, 39.79, 43.97],  
       [  0.00,  0.00, 29.47, 32.57, 36.00],  
       [  0.00,  0.00,  0.00, 26.67, 29.47],  
       [  0.00,  0.00,  0.00,  0.00, 24.13]])  
```  
  
```python  
%time simulate_tree(500) # ❷ 具有500个时间区间的树。  
```  
  
```txt  
CPU times: user 148 ms, sys: 4.49 ms, total: 152 ms  
Wall time: 154 ms  
Out[100]: array([[ 36.00,  36.32,  36.65, ..., 3095.69, 3123.50, 3151.57],  
       [  0.00,  35.68,  36.00, ..., 3040.81, 3068.13, 3095.69],  
       [  0.00,   0.00,  35.36, ..., 2986.89, 3013.73, 3040.81],  
       ...,  
       [  0.00,   0.00,   0.00, ...,    0.42,    0.42,    0.43],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.41,    0.42],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.00,    0.41]])  
```  
  
### NumPy  
  
运用一些技巧，可以基于完全向量化的代码使用NumPy创建这样的二叉树：  
  
```python  
M = 4  
  
up = np.arange(M + 1)  
up = np.resize(up, (M + 1, M + 1)) # ❶ 具有向上移动总次数的ndarray对象。  
up  
```  
  
```txt  
Out[102]: array([[0, 1, 2, 3, 4],  
       [0, 1, 2, 3, 4],  
       [0, 1, 2, 3, 4],  
       [0, 1, 2, 3, 4],  
       [0, 1, 2, 3, 4]])  
```  
  
```python  
down = up.T * 2 # ❷ 具有向下移动总次数的ndarray对象。  
down  
```  
  
```txt  
Out[103]: array([[0, 0, 0, 0, 0],  
       [2, 2, 2, 2, 2],  
       [4, 4, 4, 4, 4],  
       [6, 6, 6, 6, 6],  
       [8, 8, 8, 8, 8]])  
```  
  
```python  
up - down # ❸ 具有净向上（正）和向下（负）移动的ndarray对象。  
```  
  
```txt  
Out[104]: array([[ 0,  1,  2,  3,  4],  
       [-2, -1,  0,  1,  2],  
       [-4, -3, -2, -1,  0],  
       [-6, -5, -4, -3, -2],  
       [-8, -7, -6, -5, -4]])  
```  
  
```python  
dt = T / M  
  
S0 * np.exp(sigma * math.sqrt(dt) * (up - down)) # ❹ 四个时间区间的树（值的右上三角形）。  
```  
  
```txt  
Out[106]: array([[ 36.00,  39.79,  43.97,  48.59,  53.71],  
       [ 29.47,  32.57,  36.00,  39.79,  43.97],  
       [ 24.13,  26.67,  29.47,  32.57,  36.00],  
       [ 19.76,  21.84,  24.13,  26.67,  29.47],  
       [ 16.18,  17.88,  19.76,  21.84,  24.13]])  
```  
  
在NumPy的情形中，代码稍微更紧凑一些。然而，更重要的是，NumPy向量化在不使用更多内存的情况下实现了一个数量级的加速：  
  
```python  
def simulate_tree_np(M):  
    dt = T / M  
    up = np.arange(M + 1)  
    up = np.resize(up, (M + 1, M + 1))  
    down = up.transpose() * 2  
    S = S0 * np.exp(sigma * math.sqrt(dt) * (up - down))  
    return S  
  
simulate_tree_np(4)  
```  
  
```txt  
Out[108]: array([[ 36.00,  39.79,  43.97,  48.59,  53.71],  
       [ 29.47,  32.57,  36.00,  39.79,  43.97],  
       [ 24.13,  26.67,  29.47,  32.57,  36.00],  
       [ 19.76,  21.84,  24.13,  26.67,  29.47],  
       [ 16.18,  17.88,  19.76,  21.84,  24.13]])  
```  
  
```python  
%time simulate_tree_np(500)  
```  
  
```txt  
CPU times: user 8.72 ms, sys: 7.07 ms, total: 15.8 ms  
Wall time: 12.9 ms  
Out[109]: array([[ 36.00,  36.32,  36.65, ..., 3095.69, 3123.50, 3151.57],  
       [ 35.36,  35.68,  36.00, ..., 3040.81, 3068.13, 3095.69],  
       [ 34.73,  35.05,  35.36, ..., 2986.89, 3013.73, 3040.81],  
       ...,  
       [  0.00,   0.00,   0.00, ...,    0.42,    0.42,    0.43],  
       [  0.00,   0.00,   0.00, ...,    0.41,    0.41,    0.42],  
       [  0.00,   0.00,   0.00, ...,    0.40,    0.41,    0.41]])  
```  
  
### Numba  
  
这种金融算法应该非常适合通过Numba动态编译进行优化。确实，与NumPy版本相比，观察到了另一个数量级的加速。这使得Numba版本比Python（或者更准确地说是混合）版本快了几个数量级：  
  
```python  
simulate_tree_nb = numba.jit(simulate_tree)  
  
simulate_tree_nb(4)  
```  
  
```txt  
Out[111]: array([[ 36.00,  39.79,  43.97,  48.59,  53.71],  
       [  0.00,  32.57,  36.00,  39.79,  43.97],  
       [  0.00,   0.00,  29.47,  32.57,  36.00],  
       [  0.00,   0.00,   0.00,  26.67,  29.47],  
       [  0.00,   0.00,   0.00,   0.00,  24.13]])  
```  
  
```python  
%time simulate_tree_nb(500)  
```  
  
```txt  
CPU times: user 425 µs, sys: 193 µs, total: 618 µs  
Wall time: 625 µs  
Out[112]: array([[ 36.00,  36.32,  36.65, ..., 3095.69, 3123.50, 3151.57],  
       [  0.00,  35.68,  36.00, ..., 3040.81, 3068.13, 3095.69],  
       [  0.00,   0.00,  35.36, ..., 2986.89, 3013.73, 3040.81],  
       ...,  
       [  0.00,   0.00,   0.00, ...,    0.42,    0.42,    0.43],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.41,    0.42],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.00,    0.41]])  
```  
  
```python  
%timeit simulate_tree_nb(500)  
```  
  
```txt  
559 µs ± 46.1 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)  
```  
  
### Cython  
  
和之前一样，Cython需要对代码进行更多的调整才能看到显著的改进。以下版本主要使用静态类型声明和某些导入，它们分别比常规的Python导入和函数提高了性能：  
  
```python  
%%cython -a  
import numpy as np  
cimport cython  
from libc.math cimport exp, sqrt  
cdef float S0 = 36.  
cdef float T = 1.0  
cdef float r = 0.06  
cdef float sigma = 0.2  
def simulate_tree_cy(int M):  
    cdef int z, t, i  
    cdef float dt, u, d  
    cdef float[:, :] S = np.zeros((M + 1, M + 1),  
                                  dtype=np.float32) # ❶ 将ndarray对象声明为C数组对于性能至关重要。  
    dt = T / M  
    u = exp(sigma * sqrt(dt))  
    d = 1 / u  
    S[0, 0] = S0  
    z = 1  
    for t in range(1, M + 1):  
        for i in range(z):  
            S[i, t] = S[i, t-1] * u  
            S[i+1, t] = S[i, t-1] * d  
        z += 1  
    return np.array(S)  
```  
  
```txt  
Out[114]: <IPython.core.display.HTML object>  
```  
  
与Numba版本相比，Cython版本又缩减了30%的执行时间：  
  
```python  
simulate_tree_cy(4)  
```  
  
```txt  
Out[115]: array([[ 36.00,  39.79,  43.97,  48.59,  53.71],  
       [  0.00,  32.57,  36.00,  39.79,  43.97],  
       [  0.00,   0.00,  29.47,  32.57,  36.00],  
       [  0.00,   0.00,   0.00,  26.67,  29.47],  
       [  0.00,   0.00,   0.00,   0.00,  24.13]], dtype=float32)  
```  
  
```python  
%time simulate_tree_cy(500)  
```  
  
```txt  
CPU times: user 2.21 ms, sys: 1.89 ms, total: 4.1 ms  
Wall time: 2.45 ms  
Out[116]: array([[ 36.00,  36.32,  36.65, ..., 3095.77, 3123.59, 3151.65],  
       [  0.00,  35.68,  36.00, ..., 3040.89, 3068.21, 3095.77],  
       [  0.00,   0.00,  35.36, ..., 2986.97, 3013.81, 3040.89],  
       ...,  
       [  0.00,   0.00,   0.00, ...,    0.42,    0.42,    0.43],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.41,    0.42],  
       [  0.00,   0.00,   0.00, ...,    0.00,    0.00,    0.41]],  
      dtype=float32)  
```  
  
```python  
%timeit S = simulate_tree_cy(500)  
```  
  
```txt  
363 µs ± 29.5 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)  
```  
  
## 蒙特卡洛模拟  
  
蒙特卡洛模拟是计算金融中不可或缺的数值工具。远在现代计算机出现之前它就已经被使用了。银行和其他金融机构将其用于定价和风险管理等目的。作为一种数值方法，它也许是金融领域最灵活、最强大的方法。然而，它通常也是对计算要求最高的方法。这就是为什么Python长期以来不被认为是一种实现基于蒙特卡洛模拟的算法（至少对于现实世界的应用场景）的合适编程语言的原因。  
  
本节分析了几何布朗运动的蒙特卡洛模拟，这是一种简单但仍被广泛用于模拟股票价格或指数水平演变的随机过程。除此之外，Black-Scholes-Merton（1973）的期权定价理论也借鉴了这一过程。在他们的设置中，待估值期权的标的物遵循随机微分方程（SDE），如公式10-1所示。 $S_t$ 是标的物在时间 $t$ 的值； $r$ 是恒定的无风险短期利率； $\sigma$ 是恒定的瞬时波动率； $Z_t$ 是一个布朗运动。  
  
公式10-1. Black-Scholes-Merton SDE（几何布朗运动）  
$$dS_t=rS_tdt+\sigma S_tdZ_t$$  
  
该SDE可以在等距时间区间上进行离散化，并根据代表欧拉格式的公式10-2进行模拟。在这种情况下， $z$ 是一个标准正态分布随机数。对于 $M$ 个时间区间，时间区间的长度由 $\Delta t\equiv\frac{T}{M}$ 给出，其中 $T$ 是模拟的时间范围（例如，待估值期权的到期日）。  
  
公式10-2. Black-Scholes-Merton差分方程（欧拉格式）  
$$S_t=S_{t-\Delta t}\exp\left(\left(r-\frac{\sigma^2}{2}\right)\Delta t+\sigma\sqrt{\Delta t}z\right)$$  
  
那么欧洲看涨期权的蒙特卡洛估计量由公式10-3给出，其中 $S_T(i)$ 是在到期日 $T$ 下标的物的第 $i$ 个模拟值，总共模拟的路径数为 $I$ ，且 $i=1,2,\dots,I$ 。  
  
公式10-3. 欧洲看涨期权的蒙特卡洛估计量  
$$C_0=e^{-rT}\frac{1}{I}\sum_I\max(S_T(i)-K,0)$$  
  
### Python  
  
首先，是一个Python（或者更准确地说是混合）版本mcs_simulation_py()，它根据公式10-2实现了蒙特卡洛模拟。说它是混合的，是因为它在ndarray对象上实现了Python循环。正如前面所见，这可能为使用Numba动态编译代码奠定良好的基础。与之前一样，执行时间设定了基准。基于该模拟，对欧洲看跌期权进行估值：  
  
```python  
M = 100 # ❶ 用于离散化的时间区间数。  
I = 50000 # ❷ 要模拟的路径数。  
  
def mcs_simulation_py(p):  
    M, I = p  
    dt = T / M  
    S = np.zeros((M + 1, I))  
    S[0] = S0  
    rn = np.random.standard_normal(S.shape) # ❸ 在一个向量化步骤中提取的随机数。  
    for t in range(1, M + 1): # ❹ 基于欧拉格式实现模拟的嵌套循环。  
        for i in range(I): # ❹ 基于欧拉格式实现模拟的嵌套循环。  
            S[t, i] = S[t-1, i] * math.exp((r - sigma ** 2 / 2) * dt +  
                                           sigma * math.sqrt(dt) * rn[t, i])  
    return S  
  
%time S = mcs_simulation_py((M, I))  
```  
  
```txt  
CPU times: user 5.55 s, sys: 52.9 ms, total: 5.6 s  
Wall time: 5.62 s  
```  
  
```python  
S[-1].mean() # ❺ 基于模拟的期末平均值。  
```  
  
```txt  
Out[121]: 38.22291254503985  
```  
  
```python  
S0 * math.exp(r * T) # ❻ 理论上预期的期末值。  
```  
  
```txt  
Out[122]: 38.22611567563295  
```  
  
```python  
K = 40. # ❼ 欧洲看跌期权的执行价格。  
  
C0 = math.exp(-r * T) * np.maximum(K - S[-1], 0).mean() # ❽ 该期权的蒙特卡洛估计量。  
  
C0  
```  
  
```txt  
Out[125]: 3.860545188088036  
```  
  
图10-2显示了在模拟期结束时（欧洲看跌期权到期时）模拟值的直方图。  
  
<img width="664" height="420" alt="python_for_finance 10 2" src="https://github.com/user-attachments/assets/c41d8d5e-a237-4c5b-ba74-eb3b2d2c88ea" />
  
图10-2. 模拟的期末值的频率分布  
  
### NumPy  
  
NumPy版本mcs_simulation_np()并没有太大的不同。它仍然有一个Python循环，即跨越时间区间的循环。另一个维度由跨越所有路径的向量化代码处理。它比第一个版本快大约20倍：  
  
```python  
def mcs_simulation_np(p):  
    M, I = p  
    dt = T / M  
    S = np.zeros((M + 1, I))  
    S[0] = S0  
    rn = np.random.standard_normal(S.shape)  
    for t in range(1, M + 1): # ❶ 跨越时间区间的循环。  
        S[t] = S[t-1] * np.exp((r - sigma ** 2 / 2) * dt +  
                               sigma * math.sqrt(dt) * rn[t]) # ❷ 使用向量化NumPy代码一次处理所有路径的欧拉格式。  
    return S  
  
%time S = mcs_simulation_np((M, I))  
```  
  
```txt  
CPU times: user 252 ms, sys: 32.9 ms, total: 285 ms  
Wall time: 252 ms  
```  
  
```python  
S[-1].mean()  
```  
  
```txt  
Out[129]: 38.235136032258595  
```  
  
```python  
%timeit S = mcs_simulation_np((M, I))  
```  
  
```txt  
202 ms ± 27.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
### Numba  
  
Numba可以轻松应用于这种类型的算法，并且性能得到显著提升，这应该不足为奇了。Numba版本mcs_simulation_nb()比NumPy版本稍快：  
  
```python  
mcs_simulation_nb = numba.jit(mcs_simulation_py)  
  
%time S = mcs_simulation_nb((M, I)) # ❶ 具有编译时开销的第一次调用。  
```  
  
```txt  
CPU times: user 673 ms, sys: 36.7 ms, total: 709 ms  
Wall time: 764 ms  
```  
  
```python  
%time S = mcs_simulation_nb((M, I)) # ❷ 没有该开销的第二次调用。  
```  
  
```txt  
CPU times: user 239 ms, sys: 20.8 ms, total: 259 ms  
Wall time: 265 ms  
```  
  
```python  
S[-1].mean()  
```  
  
```txt  
Out[134]: 38.22350694016539  
```  
  
```python  
C0 = math.exp(-r * T) * np.maximum(K - S[-1], 0).mean()  
  
C0  
```  
  
```txt  
Out[136]: 3.8303077438193833  
```  
  
```python  
%timeit S = mcs_simulation_nb((M, I))  
```  
  
```txt  
248 ms ± 20.6 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
### Cython  
  
毫不奇怪，使用Cython加速代码需要更多的努力。然而，加速本身并没有更大。与NumPy和Numba版本相比，Cython版本mcs_simulation_cy()似乎还要慢一点。  
  
除其他因素外，将模拟结果转换为ndarray对象也需要一些时间：  
  
```python  
%%cython  
import numpy as np  
cimport numpy as np  
cimport cython  
from libc.math cimport exp, sqrt  
  
cdef float S0 = 36.  
cdef float T = 1.0  
cdef float r = 0.06  
cdef float sigma = 0.2  
  
@cython.boundscheck(False)  
@cython.wraparound(False)  
def mcs_simulation_cy(p):  
    cdef int M, I  
    M, I = p  
    cdef int t, i  
    cdef float dt = T / M  
    cdef double[:, :] S = np.zeros((M + 1, I))  
    cdef double[:, :] rn = np.random.standard_normal((M + 1, I))  
    S[0] = S0  
    for t in range(1, M + 1):  
        for i in range(I):  
            S[t, i] = S[t-1, i] * exp((r - sigma ** 2 / 2) * dt +  
                                      sigma * sqrt(dt) * rn[t, i])  
    return np.array(S)  
  
%time S = mcs_simulation_cy((M, I))  
```  
  
```txt  
CPU times: user 237 ms, sys: 65.2 ms, total: 302 ms  
Wall time: 271 ms  
```  
  
```python  
S[-1].mean()  
```  
  
```txt  
Out[140]: 38.241735841791574  
```  
  
```python  
%timeit S = mcs_simulation_cy((M, I))  
```  
  
```txt  
221 ms ± 9.26 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
### 多进程  
  
蒙特卡洛模拟是一项非常适合并行化的任务。一种方法是将例如10万条路径的模拟并行化为10个进程，每个进程模拟1万条路径。另一种方法是将10万条路径的模拟并行化为多个进程，例如每个进程模拟一种不同的金融工具。下面说明的是前一种情况——即基于固定数量的独立进程并行模拟大量路径。  
  
以下代码再次使用了multiprocessing模块。它将要模拟的路径总数 $I$ 分成大小为 $\frac{I}{p}$ 的较小块，其中 $p>0$ 。之后所有单一任务完成，结果通过np.hstack()被组合到一个单一的ndarray对象中。这种方法可以应用于之前介绍的任何版本。对于这里选择的特定参数化，通过这种并行化方法观察不到加速效果：  
  
```python  
import multiprocessing as mp  
  
pool = mp.Pool(processes=4) # ❶ 用于并行化的Pool对象。  
  
p = 20 # ❷ 模拟被划分成的块数。  
  
%timeit S = np.hstack(pool.map(mcs_simulation_np,  
                               p * [(M, int(I / p))]))  
```  
  
```txt  
288 ms ± 10.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
```python  
%timeit S = np.hstack(pool.map(mcs_simulation_nb,  
                               p * [(M, int(I / p))]))  
```  
  
```txt  
258 ms ± 8.69 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
```python  
%timeit S = np.hstack(pool.map(mcs_simulation_cy,  
                               p * [(M, int(I / p))]))  
```  
  
```txt  
274 ms ± 11.9 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)  
```  
  
**多进程策略**  
在金融领域，有许多算法可以很好地进行并行化。其中一些甚至允许应用不同的策略来并行化代码。蒙特卡洛模拟就是一个很好的例子，可以在单台机器或多台机器上轻松地并行执行多个模拟，并且该算法本身允许将单个模拟分布在多个进程中。  
  
## 递归pandas算法  
  
本节讨论一个有点特殊的主题，然而它在金融分析中却很重要：在存储于pandas DataFrame对象的金融时间序列数据上实现递归函数。虽然pandas允许在DataFrame对象上进行复杂的向量化操作，但某些递归算法很难或不可能向量化，这给金融分析师留下了在DataFrame对象上执行缓慢的Python循环。下面的示例以简单的形式实现了所谓的指数加权移动平均（EWMA）。  
  
对于金融时间序列 $S_t$ ， $t\in\{0,\cdots,T\}$ ，其EWMA由公式10-4给出。  
  
公式10-4. 指数加权移动平均（EWMA）  
$$\text{EWMA}_0=S_0$$  
$$\text{EWMA}_t=\alpha\cdot S_t+(1-\alpha)\cdot\text{EWMA}_{t-1},t\in\{1,\cdots,T\}$$  
  
尽管该算法本质上很简单且易于实现，但可能会导致相当慢的代码。  
  
### Python  
  
首先考虑迭代包含单个金融工具的金融时间序列数据的DataFrame对象的DatetimeIndex的Python版本（见第8章）。图10-3将金融时间序列和EWMA时间序列进行了可视化：  
  
```python  
import pandas as pd  
  
sym = 'SPY'  
  
data = pd.DataFrame(pd.read_csv('../../source/tr_eikon_eod_data.csv',  
                                index_col=0, parse_dates=True)[sym]).dropna()  
  
alpha = 0.25  
  
data['EWMA'] = data[sym] # ❶ 初始化EWMA列。  
  
%%time  
for t in zip(data.index, data.index[1:]):  
    data.loc[t[1], 'EWMA'] = (alpha * data.loc[t[1], sym] +  
                              (1 - alpha) * data.loc[t[0], 'EWMA']) # ❷ 基于Python循环实现算法。  
```  
  
```txt  
CPU times: user 588 ms, sys: 16.4 ms, total: 605 ms  
Wall time: 591 ms  
```  
  
```python  
data.head()  
```  
  
```txt  
Out[154]:              SPY        EWMA  
       Date  
       2010-01-04  113.33  113.330000  
       2010-01-05  113.63  113.405000  
       2010-01-06  113.71  113.481250  
       2010-01-07  114.19  113.658438  
       2010-01-08  114.57  113.886328  
```  
  
```python  
data[data.index > '2017-1-1'].plot(figsize=(10, 6));  
```  
  
<img width="663" height="425" alt="python_for_finance 10 3" src="https://github.com/user-attachments/assets/eace3cbc-22a3-4321-ae31-99d45d16ce0b" />
  
图10-3. 带有EWMA的金融时间序列  
  
现在考虑更通用的Python函数ewma_py()。它可以直接应用于列，或者应用于ndarray对象形式的原始金融时间序列数据：  
  
```python  
def ewma_py(x, alpha):  
    y = np.zeros_like(x)  
    y[0] = x[0]  
    for i in range(1, len(x)):  
        y[i] = alpha * x[i] + (1-alpha) * y[i-1]  
    return y  
  
%time data['EWMA_PY'] = ewma_py(data[sym], alpha) # ❶ 将函数直接应用于Series对象（即列）。  
```  
  
```txt  
CPU times: user 33.1 ms, sys: 1.22 ms, total: 34.3 ms  
Wall time: 33.9 ms  
```  
  
```python  
%time data['EWMA_PY'] = ewma_py(data[sym].values, alpha) # ❷ 将函数应用于包含原始数据的ndarray对象。  
```  
  
```txt  
CPU times: user 1.61 ms, sys: 44 µs, total: 1.65 ms  
Wall time: 1.62 ms  
```  
  
这种方法已经大大加快了代码的执行速度——达到了大约20到100多倍。  
  
### Numba  
  
这个算法本身的结构保证了在应用Numba时会有进一步的加速。事实上，当把函数ewma_nb()应用于数据的ndarray版本时，速度又提高了一个数量级：  
  
```python  
ewma_nb = numba.jit(ewma_py)  
  
%time data['EWMA_NB'] = ewma_nb(data[sym], alpha) # ❶ 将函数直接应用于Series对象（即列）。  
```  
  
```txt  
CPU times: user 269 ms, sys: 11.4 ms, total: 280 ms  
Wall time: 294 ms  
```  
  
```python  
%timeit data['EWMA_NB'] = ewma_nb(data[sym], alpha) # ❶ 将函数直接应用于Series对象（即列）。  
```  
  
```txt  
30.9 ms ± 1.21 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)  
```  
  
```python  
%time data['EWMA_NB'] = ewma_nb(data[sym].values, alpha) # ❷ 将函数应用于包含原始数据的ndarray对象。  
```  
  
```txt  
CPU times: user 94.1 ms, sys: 3.78 ms, total: 97.9 ms  
Wall time: 97.6 ms  
```  
  
```python  
%timeit data['EWMA_NB'] = ewma_nb(data[sym].values, alpha) # ❷ 将函数应用于包含原始数据的ndarray对象。  
```  
  
```txt  
134 µs ± 12.5 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)  
```  
  
### Cython  
  
Cython版本ewma_cy()也取得了相当大的速度提升，但在这种情况下它不如Numba版本快：  
  
```python  
%%cython  
import numpy as np  
cimport cython  
@cython.boundscheck(False)  
@cython.wraparound(False)  
def ewma_cy(double[:] x, float alpha):  
    cdef int i  
    cdef double[:] y = np.empty_like(x)  
    y[0] = x[0]  
    for i in range(1, len(x)):  
        y[i] = alpha * x[i] + (1 - alpha) * y[i - 1]  
    return y  
  
%time data['EWMA_CY'] = ewma_cy(data[sym].values, alpha)  
```  
  
```txt  
CPU times: user 2.98 ms, sys: 1.41 ms, total: 4.4 ms  
Wall time: 5.96 ms  
```  
  
```python  
%timeit data['EWMA_CY'] = ewma_cy(data[sym].values, alpha)  
```  
  
```txt  
1.29 ms ± 194 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)  
```  
  
这最后一个例子再次说明，实现（非标准）算法通常有多种选择。所有的选项可能会产生完全相同的结果，同时也会表现出截然不同的性能特征。在这个例子中，执行时间从0.1毫秒到500毫秒不等——相差5000倍。  
  
**最佳对比次佳**  
总的来说，将算法翻译成Python编程语言很容易。然而，在现有各种性能选项的情况下，以不必要的缓慢方式实现算法也同样容易。对于交互式金融分析来说，次佳解决方案（即能够解决问题，但可能不是最快或最节省内存的方案）可能就非常完美了。对于生产环境中的金融应用，人们应该努力实现最佳解决方案，尽管这可能需要更多的研究和一些正式的基准测试。  
  
## 结论  
  
Python生态系统提供了许多提高代码性能的方法：  
  
习惯用法和范式  
针对特定问题，某些Python范式和习惯用法可能比其他的方法性能更好；例如，在许多情况下，向量化这种范式不仅会使代码更简洁，而且会带来更高的速度（有时以更大的内存占用为代价）。  
  
包  
有大量可用于不同类型问题的包，使用适合该问题的包通常可以带来更高的性能；好的例子是带有ndarray类的NumPy和带有DataFrame类的pandas。  
  
编译  
用于加速金融算法的强大包有Numba和Cython，它们分别用于Python代码的动态和静态编译。  
  
并行化  
一些Python包（例如multiprocessing）允许轻松实现Python代码的并行化；本章中的示例仅在单台机器上使用了并行化，但Python生态系统也提供了用于多机（集群）并行化的技术。  
  
本章介绍的性能方法的一个主要优点是它们通常很容易实现，这意味着所需的额外努力通常很小。换句话说，考虑到今天可用的性能包，性能改进往往是唾手可得的果实。  
  
## 延伸阅读  
  
对于本章介绍的所有性能包，都有有用的网络资源可供参考：  
  
- http://cython.org 是Cython包和编译器项目的主页。  
  
- multiprocessing模块的文档位于 https://docs.python.org/3/library/multiprocessing.html。  
  
- 关于Numba的信息可以在 http://github.com/numba/numba 和 https://numba.pydata.org 找到。  
  
有关书籍形式的参考资料，请参阅以下内容：  
  
- Gorelick, Misha, and Ian Ozsvald (2014). High Performance Python. Sebastopol, CA: O’Reilly.  
  
- Smith, Kurt (2015). Cython. Sebastopol, CA: O’Reilly.  
  
本章引用的原始论文：  
  
- Black, Fischer, and Myron Scholes (1973). “The Pricing of Options and Corporate Liabilities.” Journal of Political Economy, Vol. 81, No. 3, pp. 638–659.  
  
- Cox, John, Stephen Ross, and Mark Rubinstein (1979). “Option Pricing: A Simplified Approach.” Journal of Financial Economics, Vol. 7, No. 3, pp. 229–263.  
  
- Merton, Robert (1973). “Theory of Rational Option Pricing.” Bell Journal of Economics and Management Science, Vol. 4, pp. 141–183.  
