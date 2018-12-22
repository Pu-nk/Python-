## Python学习手册

#### 前言

这份笔记上所记的，不能是照搬照抄的东西，因为那样没有意义。我希望能够把经过自己反复思考的东西保留下来，供日后时常温习。这份笔记也不会是一份工工整整顺序井然的笔记，因为好的想法总出现在灵光一闪之间，但为了日后阅读方便，我会给每个知识点加一个小标题索引。最后既然喜欢python这门语言，希望你能认真学好。

#### 第十二章 if测试和语法规则

##### 语句的分隔符

要说在python中如何跨行输入，主要的方法有两种。其一是使用语法括号对，其二是使用“\”。

~~~python
#method 1:
list = [1, 2, 3
        4, 5, 6]
#method2:
list = [value, name, so\
        me, to, kind]
~~~

当然，也可以使用三重引号横跨数行输入

~~~python
"""
your father is ligang
my father is who ?
"""
~~~

还有一些比较特殊的就是在每一句后面可以加封号，但并不常用。

##### 真值测试

> 1. 任何非零的数字和非空的对象都为真
> 2. 数字零、空对象以及特殊对象None都为假

常见的布尔表达式运算符：

1. X and Y
2. X or Y
3. not X

> True 和 False 的本质就是整数0 和 1，bool类在python中只是int类的一个子类

~~~python
>> 2 or 3, 3 or 2
(2, 3)
>> [] or 3
3
>> [] or {}
{}

>> 2 and 3, 3 and 2
(3, 2)
>> [] and {}
[]
>> 3 and []
[]
#可以观察到，or运算符只要检测到一个测试值为真的元素就会返回该元素，而and运算符则会检测全部的元素并返回最后一个值
~~~



##### if/else 三元表达式

~~~python
#我们之前学习的if/else语句一般是如下形式：

if X:
    A = Y
else:
    A = Z
#在python2.5中引入了一种简便的格式, 称为“短路运算”：

A = Y if X else Z

#for instance:

>>sex = 'female' if value == 1 else 'male'
>>sex
'male'
~~~



#### 第十三章 While 和 for循环

##### pass 的作用

pass在程序中表达的意义和特殊对象None一样，代表什么也没有。但在实际工作中，pass自有其存在的意义。例如：pass可以忽略try语句抛出的异常，可以定义没有属性的空类，以及填充没有内容的函数。

~~~python
def fun1():
    pass

#或者也可以用...来填充函数主体

def fun1():
    ...
    
~~~

这种表示方法同样可以用于对象的赋值

~~~python
>>> x = ...
>>> x
Ellipsis
~~~

##### 左闭右开

这种现象在python中很常见，例如在数组对象的切片、以及内置函数range( )中

~~~python
example = [1, 2, 4, 8]
example[0:3] = [1, 2, 4]

range(2,5)
(2,3,4)
~~~

##### Range()函数

Range 函数可以让for循环进行更加特殊的遍历可迭代对象

~~~python
for i in range(0,len(s),2):print(s[i],end = '')
~~~

用分片的方式同样可以达到这一效果

~~~  python
for item in s[::2]:print(item,end = '')
~~~

##### 并行遍历：Zip和Map

Zip函数可以并行遍历两个数组

~~~  python
L1 = [1, 2, 3]
L2 = [4, 5, 6]
for (x,y) in zip(L1,L2):print(x,y,'--',x+y)
[output]
1 4 -- 5
2 5 -- 7
3 6 -- 9
~~~

> 注：$^1$ `zip`函数会以最短的序列来截断所得的元组

~~~python
list(map(ord,'spam'))
output:
[115, 112, 97, 109]
~~~

**用`zip`构造字典** 

~~~python
D2 = {}
keys = ['name','age','dream']
values = ['john',21,'doctor']
for (key,value) in zip(keys, values):D2[key] = value
~~~

##### 产生偏移和元素：`enumerate`

**`enumerate(iterable)`**生成一个索引和对象的元组（tuple)

~~~ python
for (offset, item) in enumerate(s):
    print(item,'-->',offset)
    
s --> 0
p --> 1
a --> 2
m --> 3
~~~



#### 第十四章 迭代和解析

##### 迭代协议

~~~ python
f = open('D:/data.txt','r')
f.__next__()
'1231312423454\n'
f.__next__()
'2232434343545\n'
f.__next__()
'2312312323423432\n'
~~~

有`__ next__ `方法的对象会前进到下一个结果，而在一系列 结果 的末尾时，则会引发 `StopIteration`。 在 Python 中，任何这类对象都认为是可迭代的。 任何这类对象也能以 for 循环或其他迭代工具遍历， 因为所有迭代工具内部工作起来都是在每次迭代中调用`__ next__`， 并且捕捉`StopIteration` 异常来确定何时离开。



所以现在读取文本的最好方法并不是文件的`readlines` 方法，可以直接通过for循环读取文件，从内存使用的角度来看效果更好。

~~~python
for i in f:print(i)
3423423542
243241312312
423432523454323

~~~

当然也可以用`while`语句来模拟for循环读取文件，但是运行的会慢一些。因为迭代器是以C语言的速度在运行，而`while`版本是通过python虚拟机来运行Python字节码的。



##### 手动迭代`next()`

内置函数`next()` 是一个手动迭代函数，它会自动调用一个可迭代对象的`__next__`方法

~~~python
f = open('D:/data.txt','r')
next(f)
'1231312423454\n'
~~~

迭代对象还有一点要注意：当for循环开始时，会通过它传给`iter`内置对象，以便从可迭代对象获得一个迭代器，返回的对象含有需要的`__next__`方法。

~~~ python
L  = [1,4,6,8,9]
B = iter(L)
type(L)
<class 'list'>
type(B)
<class 'list_iterator'>
B.__next__()
1
~~~

不过这一步对于文件对象不是必要的，因为文件本身就是自己的迭代器，列表以及很多其他的内置对象，不是自身的迭代器，因为 它们支持多次打开迭代器 。对 这样的 对象，我们必须调用 `iter` 来启动迭代：

~~~python
f = open('D:/data.txt','r')
iter(f) is f
True

~~~



##### 利用手动迭代模拟for循环

~~~python
L = [1, 4, 9]
I = iter(L)
while True:
    try:
        X = next(I)
    except StopIteration:
        break
    print(X**2, end = ' ')
    
1 16 81 
~~~

 

##### 其他内置类型迭代器

除了列表和文件，字典也有自己的迭代器，每次调用`next( )`函数返回字典的一个键。这意味着我们不需要调用keys方法去遍历字典键，只需要用for循环迭代字典对象本身。

~~~python
I = iter(D)
next(I)
~~~



##### `enumerate` 的工作原理

`enumerate` 函数生成的对象本身就是一个迭代器,带有`__next__`方法，可以通过`list`函数一次性看到迭代器的全部结果。

~~~python
E = enumerate('spam')
E
<enumerate object at 0x000001DA4F081F78>
I =iter(E)
I
<enumerate object at 0x000001DA4F081F78>
type(E)
<class 'enumerate'>
type(I)
<class 'enumerate'>
E.__next__()
(0, 's')
E is I
True
E.__next__()
(1, 'p')
list(E)
[(2, 'a'), (3, 'm')]

~~~

##### 列表解析式

列表解析式实现了用一条单独的语句替代繁琐的for循环，且运行速度比手动的for循环更快。

~~~ python
#列表解析式
L = [X+10 for X in S]
#for循环
L = []
for X in S:
    L.append(X+10)

~~~

##### 在文件上使用列表解析

~~~python
lines = [line.rstrip() for line in lines]

~~~

**扩展的列表解析语法**

~~~python
#添加一条if过滤子句
lines = [line.rstrip() for line in open('script1.py')
        if line[0] == 'p']
#构建x+y连接的列表
[x+y for x in 'abc'for y in 'lmn']
~~~



##### 其他迭代环境

`map`函数返回的对象是一个迭代器，其相比于列表解析的局限之处在于`map`必须是函数作用于可迭代对象。

~~~python
a = map(ord,'spam')
a
<map object at 0x0000012782076898>
a.__next__()
115
a.__next__()
~~~

除此之外，`zip`、`filter`、`enumerate`等函数都会返回一个迭代器

~~~python
#zip
x, y= [1,2], [3, 4]
s = zip(x, y)
s
<zip object at 0x0000012781E4E188>
s.__next__()
(1, 3)

#filter
def is_odd(n):
    return n % 2 == 1
newlist = filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
newlist
<filter object at 0x0000012782076F98>
newlist.__next__()
1

#enumerate
d = enumerate(x)
d.__next__()
(0, 1)
~~~

`sorted( )`也是应用了迭代协议的一个内置函数，但是与上述这些函数不同的是该函数返回一个真正的列表而不是一个可迭代对象。

~~~python
list(open('D:/data.txt'))
['1231312423454\n', '2232434343545\n', '2312312323423432\n', '3423423542\n', '243241312312\n', '423432523454323\n', '\n', '12312\n', '3\n', '24\n', '2342345235\n', '454\n', '35\n', '45\n', '43\n', '534\n', '4\n', '32423\n', '4\n', '234\n', '2345\n', '435345\n', '34\n']
tuple(open('D:/data.txt'))
('1231312423454\n', '2232434343545\n', '2312312323423432\n', '3423423542\n', '243241312312\n', '423432523454323\n', '\n', '12312\n', '3\n', '24\n', '2342345235\n', '454\n', '35\n', '45\n', '43\n', '534\n', '4\n', '32423\n', '4\n', '234\n', '2345\n', '435345\n', '34\n')
'&&'.join(open('D:/data.txt'))
'1231312423454\n&&2232434343545\n&&2312312323423432\n&&3423423542\n&&243241312312\n&&423432523454323\n&&\n&&12312\n&&3\n&&24\n&&2342345235\n&&454\n&&35\n&&45\n&&43\n&&534\n&&4\n&&32423\n&&4\n&&234\n&&2345\n&&435345\n&&34\n'

~~~

##### 迭代环境

函数调用中有一种特殊的`*arg`的形式，它会把一个集合的值解包为单个的参数。

~~~python
def f(a, b, c, d):print(a, b, c, d, sep ='&')
f(*[1, 2, 3, 4])
~~~

##### Range 迭代器

~~~python
R = range(10)
W =iter(R)
W.__next__()
0
~~~

python 3.0中的`range`函数支持索引、`len( )`、迭代

##### 单个迭代器VS多个迭代器

`range( )`函数与其他内置函数比如`map`等的区别在于``range( )``

函数支持多个迭代器，每个迭代器都能记住自己的位置。而其他函数则不能。

~~~python
#range()
R = range(3)
I1 = iter(R)
next(I1)
0
I2 = iter(R)
next(I2)
0
#zip()
Z = zip((1,2,3),(10,11,12))
I1 = iter(Z)
I2 = iter(Z)
next(I1)
(1, 10)
next(I1)
(2, 11)
next(I2)
(3, 12)

~~~



##### 其他迭代器主题

- 使用`yield`语句，用户定义的函数可以转换成可迭代的生成器函数
- 当编写在圆括号里时，列表解析式可以转化为可迭代的生成器函数
- 用户定义的类通过`__iter__`或`__getitem__`运算符重载变的可以迭代



#### 第十五章 文档（略）



#### 第十六章 函数基础

**函数**是一个通用结果程序部件，其主要作用是保证最大化的代码重用和最小化的代码冗余。

##### 编写函数

**`def `**  创建了一个对象并将其赋值给某一个变量名，当python运行到`def`语句时它会生成一个新的函数对象并将其赋值给这个函数名，所以函数名是一个函数的引用。

**`	return`**语句将一个结果对象发送给调用者，直到函数完成工作程序的控制权才回到调用者手里。函数通过`return`将计算结果传递给调用者。

**`yield`**向调用者发回一个结果对象，但是记住它的离开位置。像生成 器这样的函数也可以通过 yield 语句来返回值，并挂起它们的状态以便稍后能够恢复状态。

**`global`**声明了一个模块级的函数并被赋值。

**`nonlocal`** 声明了将要赋值的一个封闭的函数变量。



##### def语句是实时运行的

Python的`def`语句是一个可执行的语句，当它运行时，它创建一个新的函数对象并将其赋值给一个变量名。因为`def`是一个语句，所以它可以出现在任一语句可以出现的地方。

~~~python
if test:
  def func():
    ...
else:
  def func():
    ...
 func() 
~~~

同时定义的函数还可以赋值给别的变量名，其实没有什么特殊的，这只是建立了变量名与函数对象之间的引用。

~~~python
def func():
  ...
othername = func
othername()
~~~

函数的作用取决于传递给它的值，称为多态，是函数的核心概念之一。

##### Python中的多态

在Python中每个操作都是多态的操作。如果对象传给函数的对象有预期的方法和表达式操作符，那么它们对于函数的逻辑来说就是有着即插即用的兼容性。



#### 第十七章 作用域

深入介绍Python作用域以及参数传递

##### Python作用域基础

Python 将一个变量名被赋值的地点关联为一个特定的命名空间，也就是说，在代码中一个变量的赋值位置决定了这个变量将存在于哪个命名空间，这就是它可见的范围。除打包代码之外，函数还为程序增加了一个额外的命名空间层：**在默认的情况下，一个函数的所有变量名都是与函数的命名空间相关联的。** 



##### 作用域法则

- 内嵌的模块是全局作用域（一个创建于模块文件顶层的命名空间），对于外部的全局变量就成为一个模块对象的属性。
- 全局作用域仅限于单个文件而言。
- 每次对函数的调用都会创建一个新的本地作用域。也就是说将会存在由那个函数船舰的变量的命名空间。可以认为每一个`def`语句都定义了一个新的本地作用域，但是因为Python允许函数在循环中调用自身（递归），所以从技术上来讲，本地作用域对应的实际上是函数调用。
- 所有的变量名都可以归纳为全局变量，本地变量和内置变量（Python的预定义``__builtin__`提供的对象）

> 注：在交互模块中执行的代码实际都输入到`__main__`模块中执行，所以也遵守这些法则

![1545144122488](C:\Users\pzh\AppData\Roaming\Typora\typora-user-images\1545144122488.png)

##### 变量名解析：LEGB原则

Python在解析变量名的时候，会先搜索本地作用域，然后搜索上一层结构中`def`的本地作用域，然后是全局作用域，最后是内置作用域。并且在第一处能找到这个变量名的作用域停下来。

> 注：如果写一条`print = 3`,则会覆盖掉`print`函数

~~~python
print = 1
print(1)
Traceback (most recent call last):
  File "<input>", line 1, in <module>
TypeError: 'int' object is not callable
~~~



##### 内置作用域

内置作用域是一个名为`__builtin__` 的内置模块，但是必须导入之后才能使用内置作用域。

~~~python
import builtins
dir(builtins)
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'WindowsError', 'ZeroDivisionError', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'all', 'any', 'ascii', 'bin', 'bool', 'breakpoint', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'execfile', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'runfile', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']

~~~

这些变量名中前一半是内置的异常，后一半是内置的函数。

##### global 语句

global是一个命名空间的声明，它告诉Python函数生成一个或多个全局变量名。

~~~python
var = 99
def glob1():
  global var
  var += 1
~~~

##### 其他调用全局变量的方式

当调用模块时，模块中的全局变量相当于模块对象的一个属性被导入。

~~~python
var = 99
def glob2():
  import files
  files.var += 1
  
def glob3():
  import sys
  sys.modules['files']
  
  
~~~

##### 作用域和嵌套函数

在**LEGB**法则中，我们通常很少提及这个**E** 。现在我们将进一步深入学习一些**LEGB**查找法则中的**E**。该层内容包含了任意嵌套函数内部的本地作用域，也称作静态嵌套作用域。

~~~python
##根据LEGB法则，python首先搜索本地作用域（X = 88）
def fun1():
    x = 88
    def fun2():
        print(x)
    fun2()
    
fun1()
88
##然后搜索代码中嵌套了的函数作用域，此时（x = 99)
def fun1():
    x = 88
    def fun2():
        x = 99
        print(x)
    fun2()
    
fun1()
99
##最后搜索全局变量
##总的来说，全局作用域里定义的变量本地作用域无法直接引用，但是本地作用域中的变量其嵌套作用域中的变量可以引用。但不同的层中如果各自定义了变量，则以各自层中的变量值为准。


~~~

##### 工厂函数

即一个能够记住嵌套作用域的变量值的函数，尽管那个作用域已经不存在了。

~~~python
def f1():
  def f2():
    return X
  return f1
~~~

这里定义了一个外部的函数，这个函数简单的生成并返回一个嵌套的函数，却并不调用这个内嵌的函数。如果我们调用外部的函数，我们得到的是一个内嵌函数的引用。这个内嵌函数时通过运行内嵌的def而创建的。



##### 嵌套作用域和lambda函数

简单的说，`lambda`就是一个简短的表达式，与`def`很相似。但是能够用于列表、元组、字典等对象中。

~~~python

~~~

##### 作用域与带有循环变量的默认参数相比较

在已给出的法则中有个值得注意的特例：如果`lambda`或者`def`在函数中定义，嵌套在一个循环之中，并且嵌套的函数引用了上层作用域的变量，该变量被循环所改变，所有咋i这个循环中产生的函数会有相同的值（在最后一次循环中完成时被引用变量的值）

~~~python
def makeActions():
  acts = []
  for i in range(5):
    acts.append(lambda x:i ** x)
  return acts

acts = makeActions()
acts[0](2)
16
acts[1](2)
16
acts[2](2)
16
acts[3](2)
16
#将i值作为参数传递给匿名函数

def makeActions():
  acts = []
  for i in range(5):
    acts.append(lambda x，i=i:i ** x)
  return acts

acts = makeActions()
acts[0](2)
0
acts[1](2)
1
acts[2](2)
4
acts[3](2)
9
acts[4](2)
16

~~~

##### nonlocal 语句

`nonlocal`语句是`global`的近亲，与`global`不同的是，`nonlocal`应用于一个嵌套函数。

~~~python
def f1():
    x = 99
    def f2():
        nonlocal x 
        x = 88
        print(x)
    f2()
    print(x)
    
f1()
88
88
~~~

> 注：当执行到`nonlocal`语句时，`nonlocal`声明的变量必须存在

`nonlocal`可以应用于嵌套函数修改本地作用域中的变量。

~~~python
#本地作用域变量不可修改
def tester(start):
  state =start
  def nested(label):
    print(label,state)
   return nested
F = tester(0)
F('spam')
spam 0
F('egg')
egg 0
#本地作用域变量可修改
def tester(start):
  state =start
  def nested(label):
    nonlocal state
    print(label,state)
    state += 1
  return nested
F = tester(0)
F('spam')
spam 0
F('egg')
egg 1
~~~

利用类和函数属性也可以实现类似于`nonlocal`的功能

~~~python
#使用类
class tester:
  def __init__(self,start):
    self.start = start
  def __call__(self,label):
    print(label, self.state)
    self.state += 1
    
   
#使用函数属性
def tester(start):
  def nested(label):
  print(label,nested.state)
  nested.state += 1
  nested.state = start
  return nested
##函数名nested是包围nested的tester作用域中的一个本地变量； 同样，它可以在nested内自由地引用。这段代码还依赖于这样一个事实：本地修改一个对象并不是给一个名称赋值；当它自增nested.state，它是在修改对象nested引用的一部分，而不是指定 nested本身。（nested说白了也就是tester里的一个对象）
~~~

**（尚有疑问）** 



#### 第十八章 参数

