# 第3章 数据类型和结构  
  
> 糟糕的程序员操心代码。优秀的程序员操心数据结构及其相互关系。  
> ——林纳斯·托瓦兹 (Linus Torvalds)  
  
本章介绍了Python的基本数据类型和数据结构，内容安排如下：  
  
“基本数据类型”（第62页）  
第一节介绍了诸如int、float、bool和str等基本数据类型。  
  
“基本数据结构”（第75页）  
第二节介绍了Python的基础数据结构（例如，list对象），并说明了控制结构、函数式编程方法和匿名函数等。  
  
本章旨在对涉及数据类型和数据结构时的Python特性进行全面介绍。具备其他编程语言（如C或Matlab）背景的读者应该能够轻松掌握Python用法可能带来的差异。此处介绍的主题和惯用法对后续章节非常重要且是基础。  
  
本章涵盖以下数据类型和结构：  
  
<img width="367" height="244" alt="python_for_finance 3 0" src="https://github.com/user-attachments/assets/d3e2591a-67b8-451b-b705-159328074fe3" />

  
## 基本数据类型  
  
Python是一种动态类型语言，这意味着Python解释器在运行时推断对象的类型。相比之下，像C这样的编译语言通常是静态类型的。在这些情况下，必须在编译前为对象指定类型。1  
  
1 Cython包为Python带来了与C语言相媲美的静态类型和编译功能。事实上，Cython不仅是一个包，它是一种将Python和C结合起来的成熟的混合编程语言。  
  
### 整数  
  
最基本的数据类型之一是整数，即int：  
  
```python  
In [1]: a = 10  
        type(a)  
```  
  
```txt  
Out[1]: int  
```  
  
内置函数type为所有具有标准和内置类型的对象以及新创建的类和对象提供类型信息。在后一种情况下，提供的信息取决于程序员存储在类中的描述。有一种说法是“Python中的一切都是对象”。这意味着，例如，即使是刚定义的int对象这样简单的对象也有内置方法。例如，通过调用bit_length()方法，可以获取在内存中表示该int对象所需的位数：  
  
```python  
In [2]: a.bit_length()  
```  
  
```txt  
Out[2]: 4  
```  
  
分配给对象的整数值越高，所需的位数就越多：  
  
```python  
In [3]: a = 100000  
        a.bit_length()  
```  
  
```txt  
Out[3]: 17  
```  
  
一般来说，有太多不同的方法，很难记住所有类和对象的所有方法。像IPython这样的高级Python环境提供了Tab键补全功能，可以显示附加到对象的所有方法。只需键入对象名称，后跟一个点（例如，a.），然后按Tab键。  
  
接着会提供一系列可以在该对象上调用的方法。或者，Python内置函数dir可以提供任何对象的属性和方法的完整列表。  
  
Python的一个特点是整数可以是任意大的。例如，考虑古戈尔（googol）数 $10^{100}$ 。Python处理如此庞大的数字毫无问题：  
  
```python  
In [4]: googol = 10 ** 100  
        googol  
```  
  
```txt  
Out[4]: 10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000  
```  
  
```python  
In [5]: googol.bit_length()  
```  
  
```txt  
Out[5]: 333  
```  
  
**大整数**  
  
Python整数可以是任意大的。解释器只需根据需要使用尽可能多的位/字节来表示数字。  
  
整数上的算术运算也很容易实现：  
  
```python  
In [6]: 1 + 4  
```  
  
```txt  
Out[6]: 5  
```  
  
```python  
In [7]: 1 / 4  
```  
  
```txt  
Out[7]: 0.25  
```  
  
```python  
In [8]: type(1 / 4)  
```  
  
```txt  
Out[8]: float  
```  
  
### 浮点数  
  
最后一个表达式返回数学上正确的结果0.25，2 这引出了下一个基本数据类型，即float。在整数值后加上一个点，如1.或1.0，会让Python将该对象解释为float。涉及float的表达式通常也会返回一个float对象：3  
  
2 这在Python 2.x中是不同的，在Python 2.x中默认是向下取整除法。Python 3.x中的向下取整除法是通过3 // 4完成的，结果为0。  
  
3 在这里以及接下来的讨论中，诸如float、float对象等术语可以互换使用，承认每个float也是一个对象。这同样适用于其他对象类型。  
  
```python  
In [9]: 1.6 / 4  
```  
  
```txt  
Out[9]: 0.4  
```  
  
```python  
In [10]: type (1.6 / 4)  
```  
  
```txt  
Out[10]: float  
```  
  
float要稍微复杂一些，因为有理数或实数的计算机表示通常并不精确，并且取决于所采用的具体技术方法。为了说明这意味着什么，让我们定义另一个float对象b。像这样的float对象在内部表示时总是只能达到一定的精确度。这在将0.1加到b上时就变得很明显了：  
  
```python  
In [11]: b = 0.35  
         type(b)  
```  
  
```txt  
Out[11]: float  
```  
  
```python  
In [12]: b + 0.1  
```  
  
```txt  
Out[12]: 0.44999999999999996  
```  
  
原因是float对象在内部以二进制格式表示；也就是说，十进制数 $0<n<1$ 用以下形式的级数表示： $n=x/2+y/4+z/8+....$ 。对于某些浮点数，二进制表示可能涉及大量元素，甚至可能是一个无穷级数。然而，考虑到用于表示这样数字的固定位数——即表示序列中的项数是固定的——不准确性就不可避免。其他数字由于可以被完美地表示，因此即使只有有限的可用位数，也能被极其精确地存储下来。考虑以下例子：  
  
```python  
In [13]: c = 0.5  
         c.as_integer_ratio()  
```  
  
```txt  
Out[13]: (1, 2)  
```  
  
二分之一，即0.5，被精确存储，因为它具有精确的（有限的）二进制表示 $0.5=1/2$ 。然而，对于b = 0.35，得到的结果与预期的有理数 $0.35=7/20$ 有所不同：  
  
```python  
In [14]: b.as_integer_ratio()  
```  
  
```txt  
Out[14]: (3152519739159347, 9007199254740992)  
```  
  
精度取决于用于表示该数字的位数。一般来说，运行Python的所有平台都使用IEEE 754双精度标准——即64位——进行内部表示。这转化为15位的相对精度。  
  
由于该主题在金融的多个应用领域中具有高度重要性，因此有时有必要确保数字的精确表示，或至少是尽可能最佳的表示。例如，当对大量数字求和时，这个问题可能会很重要。在这种情况下，某种类型和/或数量级的表示误差汇总起来，可能会导致偏离基准值的显著偏差。  
  
decimal模块提供了一个用于浮点数的任意精度对象，并在处理此类数字时提供了解决精度问题的多个选项：  
  
```python  
In [15]: import decimal  
         from decimal import Decimal  
  
In [16]: decimal.getcontext()  
```  
  
```txt  
Out[16]: Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999, Emax=999999, capitals=1, clamp=0, flags=[], traps=[InvalidOperation, DivisionByZero, Overflow])  
```  
  
```python  
In [17]: d = Decimal(1) / Decimal (11)  
         d  
```  
  
```txt  
Out[17]: Decimal('0.09090909090909090909090909091')  
```  
  
可以通过更改Context对象的相应属性值来改变表示的精度：  
  
```python  
In [18]: decimal.getcontext().prec = 4 # 低于默认精度。  
  
In [19]: e = Decimal(1) / Decimal (11)  
         e  
```  
  
```txt  
Out[19]: Decimal('0.09091')  
```  
  
```python  
In [20]: decimal.getcontext().prec = 50 # 高于默认精度。  
  
In [21]: f = Decimal(1) / Decimal (11)  
         f  
```  
  
```txt  
Out[21]: Decimal('0.090909090909090909090909090909090909090909090909091')  
```  
  
如果需要，可以通过这种方式将精度调整以适应手头的具体问题，并且可以使用表现出不同精度等级的浮点数对象进行运算：  
  
```python  
In [22]: g = d + e + f  
         g  
```  
  
```txt  
Out[22]: Decimal('0.27272818181818181818181818181909090909090909090909')  
```  
  
**任意精度浮点数**  
  
decimal模块提供了一个任意精度的浮点数对象。在金融领域，有时可能需要确保高精度并超越64位双精度标准。  
  
### 布尔值  
  
在编程中，评估一个比较或逻辑表达式（如4 > 3、4.5 <= 3.25或(4 > 3) and (3 > 2)）会产生True或False作为输出，这是两个重要的Python关键字。其他的例如def、for和if。keyword模块中提供了完整的Python关键字列表：  
  
```python  
In [23]: import keyword  
  
In [24]: keyword.kwlist  
```  
  
```txt  
Out[24]: ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']  
```  
  
True和False的数据类型为bool，代表布尔值。以下代码展示了Python的比较运算符应用于相同操作数并产生相应的bool对象：  
  
```python  
In [25]: 4 > 3 # 大于。  
```  
  
```txt  
Out[25]: True  
```  
  
```python  
In [26]: type(4 > 3)  
```  
  
```txt  
Out[26]: bool  
```  
  
```python  
In [27]: type(False)  
```  
  
```txt  
Out[27]: bool  
```  
  
```python  
In [28]: 4 >= 3 # 大于或等于。  
```  
  
```txt  
Out[28]: True  
```  
  
```python  
In [29]: 4 < 3 # 小于。  
```  
  
```txt  
Out[29]: False  
```  
  
```python  
In [30]: 4 <= 3 # 小于或等于。  
```  
  
```txt  
Out[30]: False  
```  
  
```python  
In [31]: 4 == 3 # 等于。  
```  
  
```txt  
Out[31]: False  
```  
  
```python  
In [32]: 4 != 3 # 不等于。  
```  
  
```txt  
Out[32]: True  
```  
  
通常，逻辑运算符应用于bool对象上，这反过来又会产生另一个bool对象：  
  
```python  
In [33]: True and True  
```  
  
```txt  
Out[33]: True  
```  
  
```python  
In [34]: True and False  
```  
  
```txt  
Out[34]: False  
```  
  
```python  
In [35]: False and False  
```  
  
```txt  
Out[35]: False  
```  
  
```python  
In [36]: True or True  
```  
  
```txt  
Out[36]: True  
```  
  
```python  
In [37]: True or False  
```  
  
```txt  
Out[37]: True  
```  
  
```python  
In [38]: False or False  
```  
  
```txt  
Out[38]: False  
```  
  
```python  
In [39]: not True  
```  
  
```txt  
Out[39]: False  
```  
  
```python  
In [40]: not False  
```  
  
```txt  
Out[40]: True  
```  
  
当然，这两种类型的运算符经常被组合使用：  
  
```python  
In [41]: (4 > 3) and (2 > 3)  
```  
  
```txt  
Out[41]: False  
```  
  
```python  
In [42]: (4 == 3) or (2 != 3)  
```  
  
```txt  
Out[42]: True  
```  
  
```python  
In [43]: not (4 != 4)  
```  
  
```txt  
Out[43]: True  
```  
  
```python  
In [44]: (not (4 != 4)) and (2 == 3)  
```  
  
```txt  
Out[44]: False  
```  
  
一个主要的应用领域是通过其他Python关键字（如if或while）来控制代码流（本章稍后会有更多示例）：  
  
```python  
In [45]: if 4 > 3: # 如果条件为真，则执行后续代码。  
             print('condition true') # 如果条件为真要执行的代码。  
```  
  
```txt  
condition true  
```  
  
```python  
In [46]: i = 0 # 将参数i初始化为0。  
         while i < 4: # 只要条件为真，就执行并重复后续代码。  
             print('condition true, i = ', i) # 打印一段文本和参数i的值。  
             i += 1 # 将参数值加1；i += 1与i = i + 1相同。  
```  
  
```txt  
condition true, i =  0  
condition true, i =  1  
condition true, i =  2  
condition true, i =  3  
```  
  
在数值上，Python将值0附加到False，将值1附加到True。通过bool()函数将数字转换为bool对象时，0会产生False，而所有其他数字都会产生True：  
  
```python  
In [47]: int(True)  
```  
  
```txt  
Out[47]: 1  
```  
  
```python  
In [48]: int(False)  
```  
  
```txt  
Out[48]: 0  
```  
  
```python  
In [49]: float(True)  
```  
  
```txt  
Out[49]: 1.0  
```  
  
```python  
In [50]: float(False)  
```  
  
```txt  
Out[50]: 0.0  
```  
  
```python  
In [51]: bool(0)  
```  
  
```txt  
Out[51]: False  
```  
  
```python  
In [52]: bool(0.0)  
```  
  
```txt  
Out[52]: False  
```  
  
```python  
In [53]: bool(1)  
```  
  
```txt  
Out[53]: True  
```  
  
```python  
In [54]: bool(10.5)  
```  
  
```txt  
Out[54]: True  
```  
  
```python  
In [55]: bool(-2)  
```  
  
```txt  
Out[55]: True  
```  
  
### 字符串  
  
既然可以表示自然数和浮点数，本小节将转向文本。在Python中表示文本的基本数据类型是str。str对象有许多有用的内置方法。事实上，在处理任何种类和大小的文本及文本文件时，Python通常被认为是一个不错的选择。str对象通常由单引号或双引号定义，或者通过使用str()函数转换另一个对象来定义（即，使用该对象的标准或用户定义的str表示）：  
  
```python  
In [56]: t = 'this is a string object'  
```  
  
关于内置方法，例如，你可以将该对象中第一个单词的首字母大写：  
  
```python  
In [57]: t.capitalize()  
```  
  
```txt  
Out[57]: 'This is a string object'  
```  
  
或者你也可以将其拆分为单字组件，以获得所有单词的list对象（稍后将详细介绍list对象）：  
  
```python  
In [58]: t.split()  
```  
  
```txt  
Out[58]: ['this', 'is', 'a', 'string', 'object']  
```  
  
你还可以搜索单词并获取成功情况下返回的该单词首字母的位置（即索引值）：  
  
```python  
In [59]: t.find('string')  
```  
  
```txt  
Out[59]: 10  
```  
  
如果str对象中没有该单词，则该方法返回-1：  
  
```python  
In [60]: t.find('Python')  
```  
  
```txt  
Out[60]: -1  
```  
  
在字符串中替换字符是一个典型的任务，通过replace()方法可以轻松完成：  
  
```python  
In [61]: t.replace(' ', '|')  
```  
  
```txt  
Out[61]: 'this|is|a|string|object'  
```  
  
剥离字符串——即删除某些前导/尾随字符——通常也是必要的：  
  
```python  
In [62]: 'http://www.python.org'.strip('htp:/')  
```  
  
```txt  
Out[62]: 'www.python.org'  
```  
  
表3-1列出了str对象的一些有用的方法。  
  
<img width="636" height="325" alt="python_for_finance 3 1" src="https://github.com/user-attachments/assets/00167b61-9a9a-4273-89b8-2ccb4aed2343" />
  
**Unicode字符串**  
  
从Python 2.7（用于本书第一版）到Python 3.7（用于本第二版）的一个根本变化是字符串对象的编码和解码以及Unicode的引入。本章不深入探讨在此背景下重要的许多细节；鉴于本书主要处理数值数据和包含英文单词的标准字符串，忽略这部分内容似乎是合理的。  
  
### 衍生探讨：打印与字符串替换  
  
通常通过print()函数来完成对str对象或其他Python对象的字符串表示形式的打印：  
  
```python  
In [63]: print('Python for Finance') # 打印一个str对象。  
```  
  
```txt  
Python for Finance  
```  
  
```python  
In [64]: print(t) # 打印由变量名引用的str对象。  
```  
  
```txt  
this is a string object  
```  
  
```python  
In [65]: i = 0  
         while i < 4:  
             print(i) # 打印int对象的字符串表示形式。  
             i += 1  
```  
  
```txt  
0  
1  
2  
3  
```  
  
```python  
In [66]: i = 0  
         while i < 4:  
             print(i, end='|') # 指定打印时的结束字符；默认是换行符（\n），如前所见。  
             i += 1  
```  
  
```txt  
0|1|2|3|  
```  
  
Python提供了强大的字符串替换操作。一种是老方法，通过%字符，另一种是新方法，通过花括号（{}）和format()。这两种方法在实践中仍被应用。本节无法提供所有选项的详尽说明，但以下代码片段展示了一些重要的选项。首先是做这件事的老方法：  
  
```python  
In [67]: 'this is an integer %d' % 15 # int对象替换。  
```  
  
```txt  
Out[67]: 'this is an integer 15'  
```  
  
```python  
In [68]: 'this is an integer %4d' % 15 # 具有固定数量的字符。  
```  
  
```txt  
Out[68]: 'this is an integer   15'  
```  
  
```python  
In [69]: 'this is an integer %04d' % 15 # 必要时带有前导零。  
```  
  
```txt  
Out[69]: 'this is an integer 0015'  
```  
  
```python  
In [70]: 'this is a float %f' % 15.3456 # float对象替换。  
```  
  
```txt  
Out[70]: 'this is a float 15.345600'  
```  
  
```python  
In [71]: 'this is a float %.2f' % 15.3456 # 具有固定数量的小数。  
```  
  
```txt  
Out[71]: 'this is a float 15.35'  
```  
  
```python  
In [72]: 'this is a float %8f' % 15.3456 # 具有固定数量的字符（并用小数补齐）。  
```  
  
```txt  
Out[72]: 'this is a float 15.345600'  
```  
  
```python  
In [73]: 'this is a float %8.2f' % 15.3456 # 具有固定数量的字符和小数……  
```  
  
```txt  
Out[73]: 'this is a float    15.35'  
```  
  
```python  
In [74]: 'this is a float %08.2f' % 15.3456 # ……并在必要时带有前导零。  
```  
  
```txt  
Out[74]: 'this is a float 00015.35'  
```  
  
```python  
In [75]: 'this is a string %s' % 'Python' # str对象替换。  
```  
  
```txt  
Out[75]: 'this is a string Python'  
```  
  
```python  
In [76]: 'this is a string %10s' % 'Python' # 具有固定数量的字符。  
```  
  
```txt  
Out[76]: 'this is a string     Python'  
```  
  
现在，这是以新方法实现的相同示例。请注意某些地方输出中的细微差别：  
  
```python  
In [77]: 'this is an integer {:d}'.format(15)  
```  
  
```txt  
Out[77]: 'this is an integer 15'  
```  
  
```python  
In [78]: 'this is an integer {:4d}'.format(15)  
```  
  
```txt  
Out[78]: 'this is an integer   15'  
```  
  
```python  
In [79]: 'this is an integer {:04d}'.format(15)  
```  
  
```txt  
Out[79]: 'this is an integer 0015'  
```  
  
```python  
In [80]: 'this is a float {:f}'.format(15.3456)  
```  
  
```txt  
Out[80]: 'this is a float 15.345600'  
```  
  
```python  
In [81]: 'this is a float {:.2f}'.format(15.3456)  
```  
  
```txt  
Out[81]: 'this is a float 15.35'  
```  
  
```python  
In [82]: 'this is a float {:8f}'.format(15.3456)  
```  
  
```txt  
Out[82]: 'this is a float 15.345600'  
```  
  
```python  
In [83]: 'this is a float {:8.2f}'.format(15.3456)  
```  
  
```txt  
Out[83]: 'this is a float    15.35'  
```  
  
```python  
In [84]: 'this is a float {:08.2f}'.format(15.3456)  
```  
  
```txt  
Out[84]: 'this is a float 00015.35'  
```  
  
```python  
In [85]: 'this is a string {:s}'.format('Python')  
```  
  
```txt  
Out[85]: 'this is a string Python'  
```  
  
```python  
In [86]: 'this is a string {:10s}'.format('Python')  
```  
  
```txt  
Out[86]: 'this is a string Python    '  
```  
  
字符串替换在涉及多次打印操作的情况下特别有用，在这些操作中打印的数据会更新，例如在while循环期间：  
  
```python  
In [87]: i = 0  
         while i < 4:  
             print('the number is %d' % i)  
             i += 1  
```  
  
```txt  
the number is 0  
the number is 1  
the number is 2  
the number is 3  
```  
  
```python  
In [88]: i = 0  
         while i < 4:  
             print('the number is {:d}'.format(i))  
             i += 1  
```  
  
```txt  
the number is 0  
the number is 1  
the number is 2  
the number is 3  
```  
  
### 衍生探讨：正则表达式  
  
在处理str对象时，一个强大的工具是正则表达式。Python在re模块中提供了此类功能：  
  
```python  
In [89]: import re  
```  
  
假设一位金融分析师面临一个大型文本文件，如CSV文件，其中包含某些时间序列和相应的日期时间信息。通常，这些信息以Python无法直接解释的格式提供。然而，日期时间信息通常可以用正则表达式来描述。考虑以下str对象，包含三个日期时间元素、三个整数和三个字符串。请注意，三引号允许定义跨越多行的str对象：  
  
```python  
In [90]: series = """  
         '01/18/2014 13:00:00', 100, '1st';  
         '01/18/2014 13:30:00', 110, '2nd';  
         '01/18/2014 14:00:00', 120, '3rd'  
         """  
```  
  
以下正则表达式描述了str对象中提供的日期时间信息的格式：4  
  
4 这里无法深入探讨细节，但互联网上有大量关于正则表达式的总体信息以及特别针对Python的信息。有关该主题的介绍，请参阅Fitzgerald（2012）。  
  
```python  
In [91]: dt = re.compile("'[0-9/:\s]+'") # datetime  
```  
  
有了这个正则表达式，我们就可以继续找到所有的日期时间元素。通常，将正则表达式应用于str对象也会提高典型解析任务的性能：  
  
```python  
In [92]: result = dt.findall(series)  
         result  
```  
  
```txt  
Out[92]: ["'01/18/2014 13:00:00'", "'01/18/2014 13:30:00'", "'01/18/2014 14:00:00'"]  
```  
  
**正则表达式**  
  
当解析str对象时，请考虑使用正则表达式，这可以为此类操作带来便利和性能。  
  
生成的str对象随后可以被解析，以生成Python datetime对象（关于使用Python处理日期和时间数据的概述，请参见附录A）。为了解析包含日期时间信息的str对象，我们需要提供如何解析它们的信息——同样是以str对象的形式：  
  
```python  
In [93]: from datetime import datetime  
         pydt = datetime.strptime(result[0].replace("'", ""),  
         '%m/%d/%Y %H:%M:%S')  
         pydt  
```  
  
```txt  
Out[93]: datetime.datetime(2014, 1, 18, 13, 0)  
```  
  
```python  
In [94]: print(pydt)  
```  
  
```txt  
2014-01-18 13:00:00  
```  
  
```python  
In [95]: print(type(pydt))  
```  
  
```txt  
<class 'datetime.datetime'>  
```  
  
后续章节提供了关于日期时间数据、如何处理此类数据以及datetime对象及其方法的更多信息。这里只是为了引出金融中这个重要的主题。  
  
## 基本数据结构  
  
作为一般规则，数据结构是包含可能大量其他对象的对象。在Python作为内置结构提供的对象中，有以下几种：  
  
tuple  
任意对象的不可变集合；仅有少量可用方法  
  
list  
任意对象的可变集合；有许多可用方法  
  
dict  
键值存储对象  
  
set  
用于其他唯一对象的无序集合对象  
  
### 元组  
  
tuple是一种高级数据结构，但它仍然相当简单且其应用有限。它是通过在括号中提供对象来定义的：  
  
```python  
In [96]: t = (1, 2.5, 'data')  
         type(t)  
```  
  
```txt  
Out[96]: tuple  
```  
  
你甚至可以去掉括号并提供多个对象，只需用逗号分隔即可：  
  
```python  
In [97]: t = 1, 2.5, 'data'  
         type(t)  
```  
  
```txt  
Out[97]: tuple  
```  
  
像Python中几乎所有的数据结构一样，tuple有一个内置索引，借助于它，你可以检索tuple的单个或多个元素。重要的是要记住，Python使用基于零的编号，因此tuple对象的第三个元素位于索引位置2：  
  
```python  
In [98]: t[2]  
```  
  
```txt  
Out[98]: 'data'  
```  
  
```python  
In [99]: type(t[2])  
```  
  
```txt  
Out[99]: str  
```  
  
**基于零的编号**  
  
与某些其他编程语言（如Matlab）相比，Python使用基于零的编号方案。例如，tuple对象的第一个元素的索引值为0。  
  
此对象类型仅提供两个特殊方法：count()和index()。第一个统计特定对象出现的次数，第二个给出该对象第一次出现时的索引值：  
  
```python  
In [100]: t.count('data')  
```  
  
```txt  
Out[100]: 1  
```  
  
```python  
In [101]: t.index(1)  
```  
  
```txt  
Out[101]: 0  
```  
  
tuple对象是不可变对象。这意味着一旦定义，就不能轻易更改。  
  
### 列表  
  
与tuple对象相比，list类型的对象更加灵活且强大。从金融的角度来看，仅使用list对象就可以完成很多工作，例如存储股票报价并追加新数据。list对象通过方括号定义，基本功能和行为与tuple对象相似：  
  
```python  
In [102]: l = [1, 2.5, 'data']  
          l[2]  
```  
  
```txt  
Out[102]: 'data'  
```  
  
也可以使用list()函数来定义或转换list对象。以下代码通过转换前一个示例中的tuple对象来生成一个新的list对象：  
  
```python  
In [103]: l = list(t)  
          l  
```  
  
```txt  
Out[103]: [1, 2.5, 'data']  
```  
  
```python  
In [104]: type(l)  
```  
  
```txt  
Out[104]: list  
```  
  
除了具备tuple对象的特征外，list对象还可通过不同方法进行扩展和缩减。换句话说，str和tuple对象是（带索引的）一旦创建不能更改的不可变序列对象，而list对象是可变的，并可以通过各种操作进行更改。你可以将list对象追加到现有的list对象中，或者执行更多操作：  
  
```python  
In [105]: l.append([4, 3]) # 在末尾追加list对象。  
          l  
```  
  
```txt  
Out[105]: [1, 2.5, 'data', [4, 3]]  
```  
  
```python  
In [106]: l.extend([1.0, 1.5, 2.0]) # 追加list对象的元素。  
          l  
```  
  
```txt  
Out[106]: [1, 2.5, 'data', [4, 3], 1.0, 1.5, 2.0]  
```  
  
```python  
In [107]: l.insert(1, 'insert') # 在索引位置之前插入对象。  
          l  
```  
  
```txt  
Out[107]: [1, 'insert', 2.5, 'data', [4, 3], 1.0, 1.5, 2.0]  
```  
  
```python  
In [108]: l.remove('data') # 删除对象的第一次出现。  
          l  
```  
  
```txt  
Out[108]: [1, 'insert', 2.5, [4, 3], 1.0, 1.5, 2.0]  
```  
  
```python  
In [109]: p = l.pop(3) # 移除并返回位于索引位置的对象。  
          print(l, p)  
```  
  
```txt  
[1, 'insert', 2.5, 1.0, 1.5, 2.0] [4, 3]  
```  
  
切片也很容易实现。这里的切片是指将数据集分解成更小（我们感兴趣的）部分的操作：  
  
```python  
In [110]: l[2:5] # 返回第三到第五个元素。  
```  
  
```txt  
Out[110]: [2.5, 1.0, 1.5]  
```  
  
表3-2总结了list对象的选定操作和方法。  
  
<img width="655" height="379" alt="python_for_finance 3 2" src="https://github.com/user-attachments/assets/af673893-8f07-46a4-998d-3f7fe42223f9" />
  
### 衍生探讨：控制结构  
  
尽管诸如for循环这样的控制结构本身就是一个单独的主题，但在Python中，它们可能最适合通过基于list对象的示例来介绍。这主要是因为通常是在list对象上进行循环，这与通常用在其他语言中的标准有所不同。看看下面的例子。此for循环遍历索引值为2到4的list对象l中的元素，并打印每个元素的平方。请注意第二行中缩进（空格）的重要性：  
  
```python  
In [111]: for element in l[2:5]:  
              print(element ** 2)  
```  
  
```txt  
6.25  
1.0  
2.25  
```  
  
与典型的基于计数器的循环相比，这提供了极高的灵活性。在Python中，基于计数器的循环也是一种选择，但它是使用range对象来实现的：  
  
```python  
In [112]: r = range(0, 8, 1) # 参数为起始值、结束值和步长。  
          r  
```  
  
```txt  
Out[112]: range(0, 8)  
```  
  
```python  
In [113]: type(r)  
```  
  
```txt  
Out[113]: range  
```  
  
作为对比，使用range()实现相同的循环如下：  
  
```python  
In [114]: for i in range(2, 5):  
              print(l[i] ** 2)  
```  
  
```txt  
6.25  
1.0  
2.25  
```  
  
**遍历列表**  
  
在Python中，你可以遍历任意的list对象，无论该对象的内容是什么。这通常可以避免引入计数器。  
  
Python还提供了典型的（条件）控制元素if、elif和else。它们的用法与其他语言相似：  
  
```python  
In [115]: for i in range(1, 10):  
              if i % 2 == 0: # %代表取模运算。  
                  print("%d is even" % i)  
              elif i % 3 == 0:  
                  print("%d is multiple of 3" % i)  
              else:  
                  print("%d is odd" % i)  
```  
  
```txt  
1 is odd  
2 is even  
3 is multiple of 3  
4 is even  
5 is odd  
6 is even  
7 is odd  
8 is even  
9 is multiple of 3  
```  
  
同样，while提供了控制流程的另一种手段：  
  
```python  
In [116]: total = 0  
          while total < 100:  
              total += 1  
          print(total)  
```  
  
```txt  
100  
```  
  
Python的一个特色是所谓的列表推导式（list comprehensions）。这种方法不直接遍历现有的list对象，而是以相当紧凑的方式通过循环生成list对象：  
  
```python  
In [117]: m = [i ** 2 for i in range(5)]  
          m  
```  
  
```txt  
Out[117]: [0, 1, 4, 9, 16]  
```  
  
在某种意义上，这已经提供了第一种生成“类似于”向量化代码的方法，因为循环是隐式的而不是显式的（代码的向量化将在第4章和第5章中更详细地讨论）。  
  
### 衍生探讨：函数式编程  
  
Python也提供了许多支持函数式编程的工具——即将函数应用于一整套输入（在我们的例子中是list对象）。这些工具包括filter()、map()和reduce()。然而，首先需要一个函数定义。从一个非常简单的例子开始，考虑一个返回输入x的平方的函数f()：  
  
```python  
In [118]: def f(x):  
              return x ** 2  
          f(2)  
```  
  
```txt  
Out[118]: 4  
```  
  
当然，函数可以任意复杂，具有多个输入/参数对象，甚至是多个输出（返回对象）。但是，考虑下面的函数：  
  
```python  
In [119]: def even(x):  
              return x % 2 == 0  
          even(3)  
```  
  
```txt  
Out[119]: False  
```  
  
返回的对象是一个布尔值。通过使用map()，可以将此类函数应用于整个list对象：  
  
```python  
In [120]: list(map(even, range(10)))  
```  
  
```txt  
Out[120]: [True, False, True, False, True, False, True, False, True, False]  
```  
  
为此，还可以直接将函数定义作为map()的参数，使用lambda或匿名函数：  
  
```python  
In [121]: list(map(lambda x: x ** 2, range(10)))  
```  
  
```txt  
Out[121]: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]  
```  
  
函数也可以用于过滤list对象。在以下示例中，过滤器返回符合even函数定义的布尔条件的list对象中的元素：  
  
```python  
In [122]: list(filter(even, range(15)))  
```  
  
```txt  
Out[122]: [0, 2, 4, 6, 8, 10, 12, 14]  
```  
  
**列表推导式、函数式编程、匿名函数**  
  
尽可能在Python层面上避免使用循环可以被视为一种良好的实践。列表推导式和函数式编程工具（如filter()、map()和reduce()）提供了一种编写紧凑且通常更具可读性的无（显式）循环代码的手段。在这种情况中，lambda或匿名函数也是强大的工具。  
  
### 字典  
  
dict对象即字典，同样也是可变序列，允许通过键（键可以例如是str对象）来检索数据。它们被称为键值存储。虽然list对象是有序且可排序的，但dict对象通常是无序且不可排序的。5 最好通过一个例子来说明它们与list对象的进一步区别。大括号是用来定义dict对象的：  
  
5 标准dict对象存在变体，包括OrderedDict子类等，它会记住添加条目的顺序。请参见 https://docs.python.org/3/library/collections.html。  
  
```python  
In [123]: d = {  
              'Name' : 'Angela Merkel',  
              'Country' : 'Germany',  
              'Profession' : 'Chancelor',  
              'Age' : 64  
          }  
          type(d)  
```  
  
```txt  
Out[123]: dict  
```  
  
```python  
In [124]: print(d['Name'], d['Age'])  
```  
  
```txt  
Angela Merkel 64  
```  
  
同样，这一类对象有许多内置方法：  
  
```python  
In [125]: d.keys()  
```  
  
```txt  
Out[125]: dict_keys(['Name', 'Country', 'Profession', 'Age'])  
```  
  
```python  
In [126]: d.values()  
```  
  
```txt  
Out[126]: dict_values(['Angela Merkel', 'Germany', 'Chancelor', 64])  
```  
  
```python  
In [127]: d.items()  
```  
  
```txt  
Out[127]: dict_items([('Name', 'Angela Merkel'), ('Country', 'Germany'), ('Profession', 'Chancelor'), ('Age', 64)])  
```  
  
```python  
In [128]: birthday = True  
          if birthday:  
              d['Age'] += 1  
          print(d['Age'])  
```  
  
```txt  
65  
```  
  
有几种方法可以从dict对象中获取迭代器对象。在迭代时，迭代器对象的行为与list对象类似：  
  
```python  
In [129]: for item in d.items():  
              print(item)  
```  
  
```txt  
('Name', 'Angela Merkel')  
('Country', 'Germany')  
('Profession', 'Chancelor')  
('Age', 65)  
```  
  
```python  
In [130]: for value in d.values():  
              print(type(value))  
```  
  
```txt  
<class 'str'>  
<class 'str'>  
<class 'str'>  
<class 'int'>  
```  
  
表3-3提供了dict对象所选操作和方法的摘要。  
  
<img width="422" height="327" alt="python_for_finance 3 3" src="https://github.com/user-attachments/assets/0205ea36-cb12-4a95-a89d-cd69fbef1cb1" />
  
### 集合  
  
本节介绍的最后一种数据结构是set对象。尽管集合论是数学的基石，也是金融理论的基石，但set对象的实际应用并不多。这些对象是其他对象的无序集合，每个元素仅包含一次：  
  
```python  
In [131]: s = set(['u', 'd', 'ud', 'du', 'd', 'du'])  
          s  
```  
  
```txt  
Out[131]: {'d', 'du', 'u', 'ud'}  
```  
  
```python  
In [132]: t = set(['d', 'dd', 'uu', 'u'])  
```  
  
使用set对象，可以像数学集合论中那样对集合执行基本运算。例如，可以生成并集、交集和差集：  
  
```python  
In [133]: s.union(t) # s和t的所有元素。  
```  
  
```txt  
Out[133]: {'d', 'dd', 'du', 'u', 'ud', 'uu'}  
```  
  
```python  
In [134]: s.intersection(t) # s和t中都有的元素。  
```  
  
```txt  
Out[134]: {'d', 'u'}  
```  
  
```python  
In [135]: s.difference(t) # 在s中但不在t中的元素。  
```  
  
```txt  
Out[135]: {'du', 'ud'}  
```  
  
```python  
In [136]: t.difference(s) # 在t中但不在s中的元素。  
```  
  
```txt  
Out[136]: {'dd', 'uu'}  
```  
  
```python  
In [137]: s.symmetric_difference(t) # 在s或t中但不同时在两者中的元素。  
```  
  
```txt  
Out[137]: {'dd', 'du', 'ud', 'uu'}  
```  
  
set对象的一种应用是消除list对象中的重复项：  
  
```python  
In [138]: from random import randint  
          l = [randint(0, 10) for i in range(1000)] # 1000个在0到10之间的随机整数。  
          len(l) # l中的元素个数。  
```  
  
```txt  
Out[138]: 1000  
```  
  
```python  
In [139]: l[:20]  
```  
  
```txt  
Out[139]: [4, 2, 10, 2, 1, 10, 0, 6, 0, 8, 10, 9, 2, 4, 7, 8, 10, 8, 8, 2]  
```  
  
```python  
In [140]: s = set(l)  
          s  
```  
  
```txt  
Out[140]: {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10}  
```  
  
## 结论  
  
基本的Python解释器提供了一套丰富且灵活的数据结构。从金融的角度来看，以下内容可被视为最重要的部分：  
  
**基本数据类型**  
在Python的一般应用，特别是在金融应用中，int、float、bool和str类提供了原子数据类型。  
  
**标准数据结构**  
tuple、list、dict和set类在金融领域有很多应用领域，其中list是适用于许多用例的灵活多面手。  
  
## 进阶资源  
  
关于数据类型和结构，本章主要关注对金融算法和应用可能特别重要的主题。然而，这只能作为在Python中探索数据结构和数据建模的起点。  
  
有很多宝贵的资源可以帮助您从这里进一步深入。Python数据结构的官方文档位于 https://docs.python.org/3/tutorial/datastructures.html。  
  
比较好的书籍参考资料有：  
  
- Goodrich, Michael, 等 (2013). Data Structures and Algorithms in Python. 霍博肯，新泽西州: John Wiley & Sons.  
- Harrison, Matt (2017). Illustrated Guide to Python 3. CreateSpace Treading on Python Series.  
- Ramalho, Luciano (2016). Fluent Python. 塞瓦斯托波尔，加利福尼亚州: O’Reilly.  
  
有关正则表达式的介绍，请参见：  
  
- Fitzgerald, Michael (2012). Introducing Regular Expressions. 塞瓦斯托波尔，加利福尼亚州: O’Reilly.  
