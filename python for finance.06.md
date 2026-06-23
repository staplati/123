# 第6章 面向对象编程  
  
> 软件工程的目的是控制复杂性，而不是制造复杂性。  
> ——Pamela Zave  
  
面向对象编程（OOP）是当今最受欢迎的编程范式之一。如果使用得当，与例如过程式编程相比，它提供了许多优势。在许多情况下，OOP似乎特别适合金融建模和实现金融算法。然而，也有许多批评者表达了他们对OOP的某些方面甚至整个范式的怀疑。本章持中立立场，认为OOP是一个重要的工具，它可能不是解决每一个问题的最佳工具，但它应该是金融领域的程序员和量化分析师可用的工具。  
  
随着OOP的出现，也伴随着一些新的术语。为了本书和本章的目的，最重要的术语如下（稍后会有更多）：  
  
类（Class）  
  
某种类型对象的抽象定义。例如，人类。  
  
对象（Object）  
  
类的一个实例。例如，Sandra。  
  
属性（Attribute）  
  
类的特征（类属性）或类的实例的特征（实例属性）。例如，是哺乳动物，是男性或女性，或者眼睛的颜色。  
  
方法（Method）  
  
类或类的实例可以实现的操作。例如，走路。  
  
参数（Parameters）  
  
方法为影响其行为而接受的输入。例如，走三步。  
  
实例化（Instantiation）  
  
基于抽象类创建特定对象的过程。  
  
翻译成Python代码，实现人类示例的简单类可能如下所示：  
  
```python  
class HumanBeing(object): # 类定义语句；self指的是类的当前实例。  
    def __init__(self, first_name, eye_color): # 实例化期间调用的特殊方法。  
        self.first_name = first_name # 使用参数值初始化的名字属性。  
        self.eye_color = eye_color # 使用参数值初始化的眼睛颜色属性。  
        self.position = 0 # 位置属性初始化为0。  
          
    def walk_steps(self, steps): # 步行方法定义，以步数为参数。  
        self.position += steps # 改变给定步数值位置的代码。  
```  
  
基于类的定义，可以实例化并使用一个新的Python对象：  
  
```python  
Sandra = HumanBeing('Sandra', 'blue') # 实例化。  
  
Sandra.first_name # 访问属性值。  
```  
  
```txt  
'Sandra'  
```  
  
```python  
Sandra.position   
```  
  
```txt  
0  
```  
  
```python  
Sandra.walk_steps(5) # 调用方法。  
  
Sandra.position # 访问更新后的位置值。  
```  
  
```txt  
5  
```  
  
有几个基于人类认知的方面可能支持OOP的使用：  
  
自然的思维方式  
  
人类的思维通常围绕现实世界或抽象对象展开，例如汽车或金融工具。OOP适合通过其特征对这些对象进行建模。  
  
降低复杂性  
  
通过不同的方法，OOP有助于降低问题或算法的复杂性，并逐个特征对其进行建模。  
  
更好的用户界面  
  
在许多情况下，OOP允许实现更好的用户界面和更紧凑的代码。例如，在查看NumPy的ndarray类或pandas的DataFrame类时，这一点显而易见。  
  
Pythonic的建模方式  
  
无论OOP的优缺点如何，它只是Python中的主导范式。这就是“Python中一切皆对象”这句格言的由来。OOP还允许程序员构建自定义类，其实例的表现与标准Python类的任何其他实例一样。  
  
还有几个技术方面可能支持OOP：  
  
抽象  
  
使用属性和方法可以构建抽象的、灵活的对象模型，重点关注相关的内容而忽略不需要的内容。在金融领域，这可能意味着有一个通用的类，以抽象的方式对金融工具进行建模。这样的类的实例将是具体的金融产品，例如由投资银行设计和提供的产品。  
  
模块化  
  
OOP简化了将代码分解为多个模块的过程，然后将这些模块链接起来形成完整的代码库。例如，对股票的欧式期权进行建模可以通过一个类或两个类来实现，一个用于标的股票，另一个用于期权本身。  
  
继承  
  
继承是指一个类可以继承另一个类的属性和方法的概念。在金融领域，从通用的金融工具开始，下一层级可能是通用的衍生工具，然后是欧式期权，然后是欧式看涨期权。每个类都可以从更高层级的类继承属性和方法。  
  
聚合  
  
聚合是指一个对象至少部分由多个其他可能独立存在的对象组成的情况。对欧式看涨期权进行建模的类可能会将代表标的股票和用于贴现的相关短期利率的其他对象作为属性。代表股票和短期利率的对象也可以被其他对象独立使用。  
  
组合  
  
组合与聚合类似，但在这里，单个对象不能相互独立存在。考虑一个带有固定腿和浮动腿的定制利率掉期。这两条腿不能独立于掉期本身而存在。  
  
多态  
  
多态可以呈现多种形式。在Python语境中特别重要的是所谓的“鸭子类型（duck typing）”。这指的是可以在许多不同的类及其实例上实现标准操作，而无需确切知道正在处理的是什么对象。对于一类金融工具，这可能意味着可以调用`get_current_price()`方法，而与对象的特定类型（股票、期权、掉期）无关。  
  
封装  
  
这个概念指的是仅通过公共方法使类内部的数据可访问的方法。对股票进行建模的类可能具有属性`current_stock_price`。然后，封装将通过`get_current_price()`方法提供对属性值的访问，并将对用户隐藏数据（即使其私有）。这种方法可能会避免由于简单地使用和可能更改属性值而产生的意外后果。然而，在Python类中如何将数据设为私有是有限制的。  
  
在更高的层面上，许多这些方面可以用软件工程中的两个总体目标来概括：  
  
可重用性  
  
继承和多态等概念提高了代码的可重用性，并提高了程序员的效率和生产力。它们还简化了代码维护。  
  
无冗余性  
  
同时，这些方法允许人们构建几乎无冗余的代码，避免了双重实现的工作量，并减少了调试和测试的工作量以及维护的工作量。它们还可能导致更小的整体代码库。  
  
本章组织如下：  
  
“Python对象概览”（第149页）  
  
本节通过OOP的视角探讨一些Python对象。  
  
“Python类的基础”（第154页）  
  
本节介绍Python中OOP的核心要素，并使用金融工具和投资组合头寸作为主要示例。  
  
“Python数据模型”（第159页）  
  
本节讨论Python数据模型的重要元素以及某些特殊方法扮演的角色。  
  
## Python对象概览  
  
让我们先通过OOP程序员的视角，简要了解一下前几章中遇到的一些标准对象。  
  
### int  
  
从简单的开始，考虑一个整数对象。即使是如此简单的Python对象，也具备主要的OOP特征：  
  
```python  
n = 5 # 新实例n。  
  
type(n) # 对象的类型。  
```  
  
```txt  
int  
```  
  
```python  
n.numerator # 一个属性。  
```  
  
```txt  
5  
```  
  
```python  
n.bit_length() # 一个方法。  
```  
  
```txt  
3  
```  
  
```python  
n + n # 应用+运算符（加法）。  
```  
  
```txt  
10  
```  
  
```python  
2 * n # 应用*运算符（乘法）。  
```  
  
```txt  
10  
```  
  
```python  
n.__sizeof__() # 调用特殊方法__sizeof__()以获取以字节为单位的内存使用量。1  
```  
  
```txt  
28  
```  
  
1 Python中的特殊属性和方法以双前导和双尾随下划线为特征，如 `__XYZ__()`。`n.__sizeof__()` 返回Python对象n的大小（以字节为单位）。  
  
### list  
  
`list`对象有更多的方法，但基本行为方式相同：  
  
```python  
l = [1, 2, 3, 4] # 新实例l。  
  
type(l) # 对象的类型。  
```  
  
```txt  
list  
```  
  
```python  
l[0] # 通过索引选择一个元素。  
```  
  
```txt  
1  
```  
  
```python  
l.append(10) # 一个方法。  
  
l + l # 应用+运算符（连接）。  
```  
  
```txt  
[1, 2, 3, 4, 10, 1, 2, 3, 4, 10]  
```  
  
```python  
2 * l # 应用*运算符（连接）。  
```  
  
```txt  
[1, 2, 3, 4, 10, 1, 2, 3, 4, 10]  
```  
  
```python  
sum(l) # 应用标准的Python函数sum()。  
```  
  
```txt  
20  
```  
  
```python  
l.__sizeof__() # 调用特殊方法__sizeof__()以获取以字节为单位的内存使用量。  
```  
  
```txt  
104  
```  
  
### ndarray  
  
`int`和`list`对象是标准的Python对象。NumPy的`ndarray`对象是来自开源包的“定制”对象：  
  
```python  
import numpy as np # 导入numpy。  
  
a = np.arange(16).reshape((4, 4)) # 一个新实例a。  
  
a   
```  
  
```txt  
array([[ 0,  1,  2,  3],  
       [ 4,  5,  6,  7],  
       [ 8,  9, 10, 11],  
       [12, 13, 14, 15]])  
```  
  
```python  
type(a) # 对象的类型。  
```  
  
```txt  
numpy.ndarray  
```  
  
尽管`ndarray`对象不是标准对象，但它在许多情况下的表现就像标准对象一样——这归功于Python数据模型，正如本章稍后所解释的那样：  
  
```python  
a.nbytes # 一个属性。  
```  
  
```txt  
128  
```  
  
```python  
a.sum() # 一个方法（聚合）。  
```  
  
```txt  
120  
```  
  
```python  
a.cumsum(axis=0) # 一个方法（无聚合）。  
```  
  
```txt  
array([[ 0,  1,  2,  3],  
       [ 4,  6,  8, 10],  
       [12, 15, 18, 21],  
       [24, 28, 32, 36]])  
```  
  
```python  
a + a # 应用+运算符（加法）。  
```  
  
```txt  
array([[ 0,  2,  4,  6],  
       [ 8, 10, 12, 14],  
       [16, 18, 20, 22],  
       [24, 26, 28, 30]])  
```  
  
```python  
2 * a # 应用*运算符（乘法）。  
```  
  
```txt  
array([[ 0,  2,  4,  6],  
       [ 8, 10, 12, 14],  
       [16, 18, 20, 22],  
       [24, 26, 28, 30]])  
```  
  
```python  
sum(a) # 应用标准的Python函数sum()。  
```  
  
```txt  
array([24, 28, 32, 36])  
```  
  
```python  
np.sum(a) # 应用NumPy通用函数np.sum()。  
```  
  
```txt  
120  
```  
  
```python  
a.__sizeof__() # 调用特殊方法__sizeof__()以获取以字节为单位的内存使用量。  
```  
  
```txt  
112  
```  
  
### DataFrame  
  
最后，快速浏览一下pandas的`DataFrame`对象，它的行为与`ndarray`对象类似。首先，基于`ndarray`对象实例化`DataFrame`对象：  
  
```python  
import pandas as pd # 导入pandas。  
  
df = pd.DataFrame(a, columns=list('abcd')) # 一个新实例df。  
  
type(df) # 对象的类型。  
```  
  
```txt  
pandas.core.frame.DataFrame  
```  
  
其次，看一下属性、方法和运算：  
  
```python  
df.columns # 一个属性。  
```  
  
```txt  
Index(['a', 'b', 'c', 'd'], dtype='object')  
```  
  
```python  
df.sum() # 一个方法（聚合）。  
```  
  
```txt  
a    24  
b    28  
c    32  
d    36  
dtype: int64  
```  
  
```python  
df.cumsum() # 一个方法（无聚合）。  
```  
  
```txt  
    a   b   c   d  
0   0   1   2   3  
1   4   6   8  10  
2  12  15  18  21  
3  24  28  32  36  
```  
  
```python  
df + df # 应用+运算符（加法）。  
```  
  
```txt  
    a   b   c   d  
0   0   2   4   6  
1   8  10  12  14  
2  16  18  20  22  
3  24  26  28  30  
```  
  
```python  
2 * df # 应用*运算符（乘法）。  
```  
  
```txt  
    a   b   c   d  
0   0   2   4   6  
1   8  10  12  14  
2  16  18  20  22  
3  24  26  28  30  
```  
  
```python  
np.sum(df) # 应用NumPy通用函数np.sum()。  
```  
  
```txt  
a    24  
b    28  
c    32  
d    36  
dtype: int64  
```  
  
```python  
df.__sizeof__() # 调用特殊方法__sizeof__()以获取以字节为单位的内存使用量。  
```  
  
```txt  
208  
```  
  
## Python类的基础  
  
本节涵盖了在Python中使用OOP的主要概念和具体语法。现在的背景是关于构建自定义类，以对那些无法通过现有的Python对象类型轻松、高效或正确建模的对象类型进行建模。在整个过程中，使用了金融工具的示例。  
  
两行代码就足以创建一个新的Python类：  
  
```python  
class FinancialInstrument(object): # 类定义语句。2  
    pass # 一些代码；这里仅仅使用了pass关键字。  
```  
  
2 建议类使用驼峰命名法。但是，如果没有歧义，也可以使用小写字母或蛇形命名法（如`financial_instrument`）。  
  
```python  
fi = FinancialInstrument() # 名为fi的类的新实例。  
  
type(fi) # 对象的类型。  
```  
  
```txt  
__main__.FinancialInstrument  
```  
  
```python  
fi # 每个Python对象都带有特定的“特殊”属性和方法（来自object）；这里，调用了检索字符串表示的特殊方法。  
```  
  
```txt  
<__main__.FinancialInstrument at 0x116767278>  
```  
  
```python  
fi.__str__()   
```  
  
```txt  
'<__main__.FinancialInstrument object at 0x116767278>'  
```  
  
```python  
fi.price = 100 # 所谓的数据属性——与常规属性相反——可以动态地为每个对象定义。  
  
fi.price   
```  
  
```txt  
100  
```  
  
一个重要的特殊方法是`__init__`，它在每次实例化对象时被调用。它将对象本身（按照惯例为`self`）以及可能的多个其他参数作为参数：  
  
```python  
class FinancialInstrument(object):  
    author = 'Yves Hilpisch' # 类属性的定义（由每个实例继承）。  
      
    def __init__(self, symbol, price): # 初始化期间调用的特殊方法__init__。  
        self.symbol = symbol # 实例属性的定义（每个实例独有）。  
        self.price = price   
```  
  
```python  
FinancialInstrument.author # 访问类属性。  
```  
  
```txt  
'Yves Hilpisch'  
```  
  
```python  
aapl = FinancialInstrument('AAPL', 100) # 名为aapl的类的新实例。  
  
aapl.symbol # 访问实例属性。  
```  
  
```txt  
'AAPL'  
```  
  
```python  
aapl.author   
```  
  
```txt  
'Yves Hilpisch'  
```  
  
```python  
aapl.price = 105 # 更改实例属性的值。  
  
aapl.price   
```  
  
```txt  
105  
```  
  
金融工具的价格定期变化，但金融工具的符号可能不会改变。为了在类定义中引入封装，可以定义两个方法：`get_price()`和`set_price()`。接下来的代码另外继承自前面的类定义（不再继承自`object`）：  
  
```python  
class FinancialInstrument(FinancialInstrument): # 通过从先前版本继承来进行类定义。  
    def get_price(self): # 定义get_price()方法。  
        return self.price   
          
    def set_price(self, price): # 定义set_price()方法...  
        self.price = price # ...并在给定参数值的情况下更新实例属性值。  
```  
  
```python  
fi = FinancialInstrument('AAPL', 100) # 基于名为fi的新类定义的新实例。  
  
fi.get_price() # 调用get_price()方法以读取实例属性值。  
```  
  
```txt  
100  
```  
  
```python  
fi.set_price(105) # 通过set_price()更新实例属性值。  
  
fi.get_price()   
```  
  
```txt  
105  
```  
  
```python  
fi.price # 直接访问实例属性。  
```  
  
```txt  
105  
```  
  
封装通常旨在向使用该类的用户隐藏数据。添加getter和setter方法是实现此目标的一部分。但是，这并不能阻止用户直接访问和操作实例属性。这就是私有实例属性发挥作用的地方。它们由两个前导下划线定义：  
  
```python  
class FinancialInstrument(object):  
    def __init__(self, symbol, price):  
        self.symbol = symbol  
        self.__price = price # price被定义为私有实例属性。  
          
    def get_price(self):  
        return self.__price  
          
    def set_price(self, price):  
        self.__price = price  
```  
  
```python  
fi = FinancialInstrument('AAPL', 100)  
  
fi.get_price() # get_price()方法返回其值。  
```  
  
```txt  
100  
```  
  
```python  
fi.__price # 尝试直接访问该属性会引发错误。  
```  
  
```txt  
-----------------------------------------------------------------  
AttributeError                            Traceback (most recent call last)  
<ipython-input-67-bd62f6cadb79> in <module>  
----> 1 fi.__price   
  
AttributeError: 'FinancialInstrument' object has no attribute '__price'  
```  
  
```python  
fi._FinancialInstrument__price # 如果类名前面加上一个前导下划线，仍然可以直接访问和操作。  
```  
  
```txt  
100  
```  
  
```python  
fi._FinancialInstrument__price = 105   
  
fi.set_price(100) # 将价格设置回其原始值。  
```  
  
**Python中的封装**  
  
尽管在Python中基本上可以通过私有实例属性以及处理它们的相应方法来实现Python类的封装，但无法完全强制向用户隐藏数据。从这个意义上说，它更像是Python中的一种工程原则，而不是Python类的技术特征。  
  
考虑另一个对金融工具的投资组合头寸进行建模的类。通过这两个类，聚合作为一个概念很容易说明。`PortfolioPosition`类的一个实例将`FinancialInstrument`类的一个实例作为属性值。添加一个实例属性，例如`position_size`，就可以计算例如头寸价值：  
  
```python  
class PortfolioPosition(object):  
    def __init__(self, financial_instrument, position_size):  
        self.position = financial_instrument # 基于FinancialInstrument类实例的实例属性。  
        self.__position_size = position_size # PortfolioPosition类的私有实例属性。  
          
    def get_position_size(self):  
        return self.__position_size  
          
    def update_position_size(self, position_size):  
        self.__position_size = position_size  
          
    def get_position_value(self):  
        return self.__position_size * \  
               self.position.get_price() # 根据属性计算头寸价值。  
```  
  
```python  
pp = PortfolioPosition(fi, 10)  
  
pp.get_position_size()  
```  
  
```txt  
10  
```  
  
```python  
pp.get_position_value()   
```  
  
```txt  
1000  
```  
  
```python  
pp.position.get_price() # 可以直接访问附加到实例属性对象上的方法（也可以隐藏）。  
```  
  
```txt  
100  
```  
  
```python  
pp.position.set_price(105) # 更新金融工具的价格。  
  
pp.get_position_value() # 根据更新后的价格计算新的头寸价值。  
```  
  
```txt  
1050  
```  
  
## Python数据模型  
  
上一节中的示例重点介绍了所谓的Python数据或对象模型的某些方面。Python数据模型允许你设计能与Python的基本语言结构一致交互的类。除此之外，它还支持（参见Ramalho (2015)，第4页）以下任务和结构：  
  
* 迭代  
* 集合处理  
* 属性访问  
* 运算符重载  
* 函数和方法调用  
* 对象的创建和销毁  
* 字符串表示（例如，用于打印）  
* 托管上下文（即with块）  
  
由于Python数据模型如此重要，本节将专门介绍一个示例（摘自Ramalho (2015)，稍作调整），该示例探讨了它的几个方面。它实现了一个一维、三元素向量的类（想象欧几里得空间中的向量）。首先是特殊方法`__init__`：  
  
```python  
class Vector(object):  
    def __init__(self, x=0, y=0, z=0): # 三个预初始化的实例属性（想象三维空间）。  
        self.x = x   
        self.y = y   
        self.z = z   
```  
  
```python  
v = Vector(1, 2, 3) # 名为v的类的新实例。  
  
v # 默认的字符串表示。  
```  
  
```txt  
<__main__.Vector at 0x1167789e8>  
```  
  
特殊方法`__repr__`允许定义自定义字符串表示：  
  
```python  
class Vector(Vector):  
    def __repr__(self):  
        return 'Vector(%r, %r, %r)' % (self.x, self.y, self.z)  
```  
  
```python  
v = Vector(1, 2, 3)  
  
v # 新的字符串表示。  
```  
  
```txt  
Vector(1, 2, 3)  
```  
  
```python  
print(v)   
```  
  
```txt  
Vector(1, 2, 3)  
```  
  
`abs()`和`bool()`是两个标准的Python函数，可以通过特殊方法`__abs__`和`__bool__`来定义它们在`Vector`类上的行为：  
  
```python  
class Vector(Vector):  
    def __abs__(self):  
        return (self.x ** 2 + self.y ** 2 +  
                self.z ** 2) ** 0.5 # 在给定三个属性值的情况下返回欧几里得范数。  
                  
    def __bool__(self):  
        return bool(abs(self))  
```  
  
```python  
v = Vector(1, 2, -1) # 具有非零属性值的新Vector对象。  
  
abs(v)  
```  
  
```txt  
2.449489742783178  
```  
  
```python  
bool(v)  
```  
  
```txt  
True  
```  
  
```python  
v = Vector() # 只有零属性值的新Vector对象。  
  
v   
```  
  
```txt  
Vector(0, 0, 0)  
```  
  
```python  
abs(v)  
```  
  
```txt  
0.0  
```  
  
```python  
bool(v)  
```  
  
```txt  
False  
```  
  
如多次所示，`+`和`*`运算符几乎可以应用于任何Python对象。行为是通过特殊方法`__add__`和`__mul__`定义的：  
  
```python  
class Vector(Vector):  
    def __add__(self, other):  
        x = self.x + other.x  
        y = self.y + other.y  
        z = self.z + other.z  
        return Vector(x, y, z) # 在这种情况下，每个特殊方法都返回一个自身同类的对象。  
          
    def __mul__(self, scalar):  
        return Vector(self.x * scalar,  
                      self.y * scalar,  
                      self.z * scalar)   
```  
  
```python  
v = Vector(1, 2, 3)  
  
v + Vector(2, 3, 4)  
```  
  
```txt  
Vector(3, 5, 7)  
```  
  
```python  
v * 2  
```  
  
```txt  
Vector(2, 4, 6)  
```  
  
另一个标准的Python函数是`len()`，它以元素的数量给出对象的长度。当在对象上调用此函数时，它会访问特殊方法`__len__`。另一方面，特殊方法`__getitem__`使得通过方括号表示法进行索引成为可能：  
  
```python  
class Vector(Vector):  
    def __len__(self):  
        return 3 # 所有的Vector类实例的长度都为3。  
          
    def __getitem__(self, i):  
        if i in [0, -3]: return self.x  
        elif i in [1, -2]: return self.y  
        elif i in [2, -1]: return self.z  
        else: raise IndexError('Index out of range.')  
```  
  
```python  
v = Vector(1, 2, 3)  
  
len(v)  
```  
  
```txt  
3  
```  
  
```python  
v[0]  
```  
  
```txt  
1  
```  
  
```python  
v[-2]  
```  
  
```txt  
2  
```  
  
```python  
v[3]  
```  
  
```txt  
-----------------------------------------------------------------  
IndexError                                Traceback (most recent call last)  
<ipython-input-102-f998c57dcc1e> in <module>  
----> 1 v[3]  
  
<ipython-input-97-b0ca25eef7b3> in __getitem__(self, i)  
      7         elif i in [1, -2]: return self.y  
      8         elif i in [2, -1]: return self.z  
----> 9         else: raise IndexError('Index out of range.')  
  
IndexError: Index out of range.  
```  
  
最后，特殊方法`__iter__`定义了在迭代对象的元素期间的行为。定义了此操作的对象称为可迭代对象（iterable）。例如，所有的集合和容器都是可迭代的：  
  
```python  
class Vector(Vector):  
    def __iter__(self):  
        for i in range(len(self)):  
            yield self[i]  
```  
  
```python  
v = Vector(1, 2, 3)  
  
for i in range(3): # 使用索引值进行间接迭代（通过__getitem__）。  
    print(v[i])   
```  
  
```txt  
1  
2  
3  
```  
  
```python  
for coordinate in v: # 直接在类实例上进行迭代（使用__iter__）。  
    print(coordinate)   
```  
  
```txt  
1  
2  
3  
```  
  
**增强Python**  
  
Python数据模型允许定义能与标准Python运算符、函数等无缝交互的Python类。这使得Python成为一种相当灵活的编程语言，可以通过新的类和对象类型轻松地对其进行增强。  
  
作为总结，以下部分在单个代码块中提供了`Vector`类的定义。  
  
## Vector类  
  
```python  
class Vector(object):  
    def __init__(self, x=0, y=0, z=0):  
        self.x = x  
        self.y = y  
        self.z = z  
          
    def __repr__(self):  
        return 'Vector(%r, %r, %r)' % (self.x, self.y, self.z)  
          
    def __abs__(self):  
        return (self.x ** 2 + self.y ** 2 + self.z ** 2) ** 0.5  
          
    def __bool__(self):  
        return bool(abs(self))  
          
    def __add__(self, other):  
        x = self.x + other.x  
        y = self.y + other.y  
        z = self.z + other.z  
        return Vector(x, y, z)  
          
    def __mul__(self, scalar):  
        return Vector(self.x * scalar,  
                      self.y * scalar,  
                      self.z * scalar)  
                        
    def __len__(self):  
        return 3  
          
    def __getitem__(self, i):  
        if i in [0, -3]: return self.x  
        elif i in [1, -2]: return self.y  
        elif i in [2, -1]: return self.z  
        else: raise IndexError('Index out of range.')  
          
    def __iter__(self):  
        for i in range(len(self)):  
            yield self[i]  
```  
  
## 结论  
  
本章从理论上并通过Python示例介绍了面向对象编程的概念和方法。OOP是Python中使用的主要编程范式之一。它不仅允许对相当复杂的应用程序进行建模和实现，而且由于灵活的Python数据模型，还允许创建行为像标准Python对象一样的自定义对象。尽管有许多批评者反对OOP，但可以有把握地说，它为Python程序员和量化分析师提供了强大的工具，在达到一定程度的复杂性时这些工具会很有帮助。第五部分中开发和讨论的衍生品定价包提供了这样一个案例：在该案例中，OOP似乎是处理固有复杂性和抽象要求的唯一明智的编程范式。  
  
## 进一步阅读的资源  
  
以下是关于一般OOP以及特别是Python编程和OOP的宝贵在线资源：  
  
* 关于面向对象编程的讲义（Lecture Notes on Object-Oriented Programming）  
* Python中的面向对象编程（Object-Oriented Programming in Python）  
  
一本关于Python面向对象和Python数据模型的很棒的纸质资源是：  
  
* Ramalho, Luciano (2016). 《流畅的Python》（Fluent Python）。加利福尼亚州塞巴斯托波尔：O’Reilly。  
