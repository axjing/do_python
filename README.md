# do_python
[[Python编码实践]]
# python面试指南

**目录：**

[TOC]

## 编译型语言和解释型语言的区别
|类型	|原理	|优点|	缺点|
| ---| ---|---|---|
|编译型语言|	通过专门的编译器，将所有源代码一次性转换成特定平台（Windows、Linux 等）执行的机器码（以可执行文件的形式存在）。|	编译一次后，脱离了编译器也可以运行，并且运行效率高。|	可移植性差，不够灵活。|
|解释型语言	|由专门的解释器，根据需要将部分源代码临时转换成特定平台的机器码。	|跨平台性好，通过不同的解释器，将相同的源代码解释成不同平台下的机器码。|	一边执行一边转换，效率很低。|

python属于**面向对象、解释型、弱类型、动态型**语言；

## 生成器和迭代器的区别   
1. 迭代器：Python中一个实现_iter_方法和_next_方法的类对象，就是迭代器。
1. 生成器：本质上是动态生成迭代的值，使用完直接丢弃，可以有效节省内存空间，但这些值只能被迭代一次。
3. 迭代器就是用于迭代操作的的对象，遵从迭代协议（内部实现了__iter__()和__next__()方法，可以像列表（可迭代对象，只有__iter__()方法）一样迭代获取其中的值，与列表不同的是，构建迭代器的时候，不像列表一样一次性把数据加到内存，而是以一种延迟计算的方式返回元素，即调用next方法时候返回此值。
4. 生成器本质上也是一个迭代器，自己实现了可迭代协议，与生成器不同的是生成器的实现方式不同，可以通过生成器表达式和生成器函数两种方式实现，代码更简洁。生成器和迭代器都是惰性可迭代对象，只能遍历一次，数据取完抛出Stopiteration异常

### 什么是 Python 生成器？
generator，有两种产生生成器对象的方式：
1. 列表生成式加括号：

```g1 = (x for x in range(10))```

2. 在函数定义中包含```yield```关键字：

```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

g2 = fib(8)

```

对于generator对象g1和g2，可以通过```next(g1)```不断获得下一个元素的值，如果没有更多的元素，就会报错```StopIteration```

也可以通过for循环获得元素的值。

生成器的好处是不用占用很多内存，只需要在用的时候计算元素的值就行了。

通过列表生成式，可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含百万元素的列表，不仅是占用很大的内存空间，如：我们只需要访问前面的几个元素，后面大部分元素所占的空间都是浪费的。因此，没有必要创建完整的列表（节省大量内存空间）。在Python中，我们可以采用生成器：边循环，边计算的机制—>generator

### 什么是 Python 迭代器？
Python中可以用于for循环的，叫做可迭代`Iterable`，包括list/set/tuple/str/dict等数据结构以及生成器；可以用以下语句判断一个对象是否是可迭代的：

```py
from collections import Iterable
isinstance(x, Iterable)
```

迭代器```Iterator```，是指可以被```next()```函数调用并不断返回下一个值，直到```StopIteration```；生成器都是Iterator，而列表等数据结构不是；可以通过以下语句将list变为Iterator：

```iter([1,2,3,4,5])```

生成器都是Iterator，但迭代器不一定是生成器。

## python闭包
当某个函数被当成对象返回时，夹带了外部变量，就形成了一个闭包。**一个函数定义中引用了函数外定义的变量，并且该函数可以在其定义环境外被执行。这样的一个函数我们称之为闭包。**

- 闭包中的引用的自由变量只和具体的闭包有关联，闭包的每个实例引用的自由变量互不干扰。
- 一个闭包实例对其自由变量的修改会被传递到下一次该闭包实例的调用。
### 闭包的作用和使用场景

闭包夹带了外部变量（私货），如果它不夹带私货，它和普通的函数就没有任何区别。同一个的函数夹带了不同的私货，就实现了不同的功能。闭包和面向接口编程的概念很像，闭包相当于轻量级的接口封装。
## list、dict、tuple、set区别
### 定义
- dict: python 内置的字典，在其他的语言中称其为map(j键值对key-value)存储数据，具有极快的查找速度，这种通过key计算位置的方法也称为哈希算法（Hash)
- list:是一个有序的集合，可以随时添加和删除其中的数据
- tuple:也是一种有序的集合，但与list不同的是tuple一点初始化，其中的元素就不能做出修改
- set与dict类似但set中只有key,并不存储value,有序key是不可重复的，所以set中的元素不会是重复的
### 区别
- dict 与 set: 他们的原理是一样的，不一样的是set只是存放key,而不存放value.
- dict 与 list:
  - dict是无序的，内部存放的数据和key存储的顺序是无关的；list是又续存储，所以可以使用切片来获取数据
  - dict 查找个插入数据极快，不会对着数据的增加而变慢；list随着元素的增多，器查找数据插入数据都会变慢。
  - dist存储数据需要浪费大量的内存，内存浪费多；list占用空间小，浪费内存小
- list与tuple： tuple初始化数据后不能对其中的元素进行修改，但tuple所指向的元素有一个list,是可以对这个list做出修改的。
  - list 长度可变，tuple不可变；
  - list 中元素的值可以改变，tuple 不能改变；
  - list 支持```append```; ```insert```; ```remove```; ```pop```等方法，tuple 都不支持

## Python 中的 list、tuple、dict和set 是怎么实现的？
- list 本质是顺序表，只不过每次表的扩容都是指数级，所以动态增删数据时，表并不会频繁改变物理结构，同时受益于顺序表遍历的高效性（通过角标配合表头物理地址，计算目标元素的位置），使得python的list综合性能比较优秀；
- tuple 本质上就是顺序表，不可修改不可扩容，只读；
- dict：本质上是顺序表，不过每个元素存储位置的角标，不是又插入顺序决定的，而是由key经过hash算法和其他机制，动态生成的，即key通过hash散列，生成value应该存储的位置，然后再去存储这个value；所以dict的查询时间复杂度是o（1）；因此，dict的key只能为可hash的对象，即不可变类型；
- set 实现列表去重：
本质上是通过__hash__和__eq__来实现对每个元素的hash散列，判断hash值是否一致；一致的话，判断对象是否具有一模一样的方法和属性，如果都一致，则去重；因此，set元素也必须是可hash的；
>set本质也是dict，只是键值都一样；实现去重其实就是这么个过程：首先对key进行hash，在dict中这一步是为了获取value的索引，这里也一样；如果索引相同，说明要么数据重复了，要么key发生了hash碰撞，这时候就去比较两个key对应的value是否相同，如果也相同，确认是数据重复，则去重（保留最新的那个）；如果数据不同，说明只是在当前hash算法中，两个key刚好发生了hash碰撞（概率相当低），此时不会发生去重；
## 装饰器
装饰器本质上是一个Python函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。它经常用于有切面需求的场景，比如：*插入日志、性能测试、事务处理、缓存、权限校验* 等场景。装饰器是解决这类问题的绝佳设计，有了装饰器就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。

总结，装饰器的作用就是为已经存在的函数或对象添加额外的功能。
### 代码示例
```python
def debug(func):
    def wrapper(*args, **kwargs):  # 指定宇宙无敌参数
        print("[DEBUG]: enter {}()".format(func.__name__))
        print ('Go triping...',)
        return func(*args, **kwargs)
    return wrapper  # 返回

@debug
def air(something):
    print("Airway {}!".format(something))

@debug
def bus(something,people):
    print(people)
    print("Go {}!".format(something))

air("123")

bus("shopping","Xiaoming")
```

```python
[DEBUG]: enter air()
Go triping...
Airway 123!
[DEBUG]: enter bus()
Go triping...
Xiaoming
Go shopping!
```

## Python 中使用多线程可以达到多核CPU一起使用吗？

Python中有一个被称为Global Interpreter Lock（GIL）的东西，它会确保任何时候你的多个线程中，只有一个被执行。线程的执行速度非常之快，会让你误以为线程是并行执行的，但是实际上都是轮流执行。经过GIL这一道关卡处理，会增加执行的开销。

可以通过多进程实现多核任务。
- 根本区别：进程是操作系统资源分配的基本单位，而线程是任务调度和执行的基本单位
- 在开销方面：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。
- 所处环境：在操作系统中能同时运行多个进程（程序）；而在同一个进程（程序）中有多个线程同时执行（通过CPU调度，在每个时间片中只有一个线程执行）
- 内存分配方面：系统在运行的时候会为每个进程分配不同的内存空间；而对线程而言，除了CPU外，系统不会为线程分配内存（线程所使用的资源来自其所属进程的资源），线程组之间只能共享资源。
- 包含关系：没有线程的进程可以看做是单线程的，如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

## GIL(Global Interpreter Lock)
全局解释器锁

全局解释器锁(Global Interpreter Lock)是计算机程序设计语言解释器用于同步线程的一种机制，它使得任何时刻仅有一个线程在执行。即便在多核处理器上，使用 GIL 的解释器也只允许同一时间执行一个线程，常见的使用 GIL 的解释器有CPython与Ruby MRI。可以看到GIL并不是Python独有的特性，是解释型语言处理多线程问题的一种机制而非语言特性。
即Python为了保证线程安全而采取的独立线程运行的限制,说白了就是一个核只能在同一时间运行一个线程.**对于io密集型任务，python的多线程起到作用，但对于cpu密集型任务，python的多线程几乎占不到任何优势，还有可能因为争夺资源而变慢。**

见[Python 最难的问题](http://www.oschina.net/translate/pythons-hardest-problem)

### GIL的设计初衷?
<details>
<summary>下拉</summary>
单核时代高效利用CPU, 针对解释器级别的数据安全(不是thread-safe 线程安全)。
首先需要明确的是GIL并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。当Python虚拟机的线程想要调用C的原生线程需要知道线程的上下文，因为没有办法控制C的原生线程的执行，所以只能把上下文关系传给原生线程，同理获取结果也是线
程在python虚拟机这边等待。那么要执行一次计算操作，就必须让执行程序的线程组串行执行。
</details>

### 为什么要加在解释器,而不是在其他层?
<details>
<summary>下拉</summary>
GIL锁加在解释器一层，也就是说Python调用的Cython解释器上加了GIL锁，因为你python调用的所有线程都是原生线程。原生线程是通过C语言提供原生接口，相当于C语言的一个函数。你一调它，你就控制不了了它了，就必须等它给你返回结果。只要已通过python虚拟机
，再往下就不受python控制了，就是C语言自己控制了。加在Python虚拟机以下加不上去，只能加在Python解释器这一层。
</details>

### GIL的实现是线程不安全?为什么?
<details>
<summary>下拉</summary>
是不安全的，具体情况要分类讨论。

单核情况下:

![单核情况下——线程不安全](https://images2017.cnblogs.com/blog/1088183/201709/1088183-20170926140930839-80064182.png)

> 解释:
> 1. 到第5步的时候，可能这个时候python正好切换了一次GIL(据说python2.7中，每100条指令会切换一次GIL),执行的时间到了，被要求释放GIL,这个时候thead 1的count=0并没有得到执行，而是挂起状态，count=0这个上下文关系被存到寄存器中.
> 2. 然后到第6步，这个时候thead 2开始执行，然后就变成了count = 1,返回给count，这个时候count=1.
> 3. 然后再回到thead 1，这个时候由于上下文关系，thead 1拿到的寄存器中的count = 0，经过计算，得到count = 1，经过第13步的操作就覆盖了原来的count = 1的值，所以这个时候count依然是count = 1，所以这个数据并没有保护起来。

python2.x和3.x都是在执行IO操作的时候，强制释放GIL，使其他线程有机会执行程序。

Python2.x Python使用计数器ticks计算字节码，当执行100个字节码的时候强制释放GIL，其他线程获取GIL继续执行。ticks可以看作是Python自己的计数器，专门作用于GIL，释放后归零，技术可以调整。

Python3.x Python使用计时器，执行时间达到阈值后，当前线程释放GIL。总体来说比Python3.x对CPU密集型任务更好，但是依然没有解决问题。

多核情况下:

多个CPU情况下，单个CPU释放GIL锁，其他CPU上的线程也会竞争，但是CPU-A可能又马上拿到了GIL，这样其他CPU上的线程只能继续等待，直到重新回到待调度状态。造成多线程在多核CPU情况下，效率反而会下降，出现了大量的资源浪费。
</details>

## Python 中的垃圾回收机制？
[Python垃圾回收机制--完美讲解!](https://www.jianshu.com/p/1e375fb40506)

## 什么是 lambda 表达式？
称匿名函数，常用来表示内部仅包含 1 行表达式的函数。如果一个函数的函数体仅有 1 行表达式，则该函数就可以用 lambda 表达式来代替。

简单来说，lambda表达式通常是当你需要使用一个函数，但是又不想费脑袋去命名一个函数的时候使用，也就是通常所说的匿名函数。
lambda表达式一般的形式是：关键词lambda后面紧接一个或多个参数，紧接一个冒号“：”，紧接一个表达式

```python
def func1(a1,a2):
    return a1 + a2

func2 = lambda a1,a2:a1+a2

f1 = func1(2980,200)
f2 = func2(2980,200)

print(f1)
print(f2)
```

```python
# 输出结果是一样的，两种方式一样
3180
3180
```

## 什么是深拷贝和浅拷贝？
赋值（=），就是创建了对象的一个新的引用，修改其中任意一个变量都会影响到另一个。

浅拷贝 copy.copy：创建一个新的对象，但它包含的是对原始对象中包含项的引用（如果用引用的方式修改其中一个对象，另外一个也会修改改变）

深拷贝：创建一个新的对象，并且递归的复制它所包含的对象（修改其中一个，另外一个不会改变）{copy模块的deep.deepcopy()函数}

## 双等于和 is 有什么区别？
```==```比较的是两个变量的 value，只要值相等就会返回True

```is```比较的是两个变量的 id，即```id(a) == id(b)```，只有两个变量指向同一个对象的时候，才会返回True

但是需要注意的是，比如以下代码：

```
a = 2
b = 2
print(a is b)
```

按照上面的解释，应该会输出False，但是事实上会输出True，这是因为Python中对小数据有缓存机制，-5~256之间的数据都会被缓存。

------

## 其它

### 类型转换
- list(x)
- str(x)
- set(x)
- int(x)
- tuple(x)

### try...except

### list
- ```lst[a:b]```：左闭右开
- ```lst.append(value)```：在末尾添加元素，复杂度O(1)
- ```lst.pop()```：弹出列表末尾元素，复杂度O(1)
- ```lst.pop(index)```：弹出任意位置元素，将后面的元素前移，复杂度O(n)
- ```lst.insert(index, value)```：插入元素，后面的元素后移，复杂度O(n)
- ```lst.remove(value)```：移除等于value的第一个元素，后面的元素前移，复杂度O(n)
- ```lst.count(value)```：计数值为value的元素个数
- ```lst.sort(reverse = False)```：排序，默认升序


------
##  Python的函数参数传递

看两个例子:

```python
a = 1
def fun(a):
    a = 2
fun(a)
print a  # 1
```

```python
a = []
def fun(a):
    a.append(1)
fun(a)
print a  # [1]
```

所有的变量都可以理解是内存中一个对象的“引用”，或者，也可以看似c中void*的感觉。

通过`id`来看引用`a`的内存地址可以比较理解：

```python
a = 1
def fun(a):
    print "func_in",id(a)   # func_in 41322472
    a = 2
    print "re-point",id(a), id(2)   # re-point 41322448 41322448
print "func_out",id(a), id(1)  # func_out 41322472 41322472
fun(a)
print a  # 1
```

注：具体的值在不同电脑上运行时可能不同。

可以看到，在执行完`a = 2`之后，`a`引用中保存的值，即内存地址发生变化，由原来`1`对象的所在的地址变成了`2`这个实体对象的内存地址。

而第2个例子`a`引用保存的内存值就不会发生变化：

```python
a = []
def fun(a):
    print "func_in",id(a)  # func_in 53629256
    a.append(1)
print "func_out",id(a)     # func_out 53629256
fun(a)
print a  # [1]
```

这里记住的是类型是属于对象的，而不是变量。而对象有两种,“可更改”（mutable）与“不可更改”（immutable）对象。在python中，strings, tuples, 和numbers是不可更改的对象，而 list, dict, set 等则是可以修改的对象。(这就是这个问题的重点)

当一个引用传递给函数的时候,函数自动复制一份引用,这个函数里的引用和外边的引用没有半毛关系了.所以第一个例子里函数把引用指向了一个不可变对象,当函数返回的时候,外面的引用没半毛感觉.而第二个例子就不一样了,函数内的引用指向的是可变对象,对它的操作就和定位了指针地址一样,在内存里进行修改.


## @staticmethod和@classmethod

Python其实有3个方法,即静态方法(staticmethod),类方法(classmethod)和实例方法,如下:

```python
def foo(x):
    print "executing foo(%s)"%(x)

class A(object):
    def foo(self,x):
        print "executing foo(%s,%s)"%(self,x)

    @classmethod
    def class_foo(cls,x):
        print "executing class_foo(%s,%s)"%(cls,x)

    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)"%x

a=A()

```

这里先理解下函数参数里面的self和cls.这个self和cls是对类或者实例的绑定,对于一般的函数来说我们可以这么调用`foo(x)`,这个函数就是最常用的,它的工作跟任何东西(类,实例)无关.对于实例方法,我们知道在类里每次定义方法的时候都需要绑定这个实例,就是`foo(self, x)`,为什么要这么做呢?因为实例方法的调用离不开实例,我们需要把实例自己传给函数,调用的时候是这样的`a.foo(x)`(其实是`foo(a, x)`).类方法一样,只不过它传递的是类而不是实例,`A.class_foo(x)`.注意这里的self和cls可以替换别的参数,但是python的约定是这俩,还是不要改的好.

对于静态方法其实和普通的方法一样,不需要对谁进行绑定,唯一的区别是调用的时候需要使用`a.static_foo(x)`或者`A.static_foo(x)`来调用.

| \\      | 实例方法     | 类方法            | 静态方法            |
| :------ | :------- | :------------- | :-------------- |
| a = A() | a.foo(x) | a.class_foo(x) | a.static_foo(x) |
| A       | 不可用      | A.class_foo(x) | A.static_foo(x) |



## 类变量和实例变量

**类变量：**

> ​	是可在类的所有实例之间共享的值（也就是说，它们不是单独分配给每个实例的）。例如下例中，num_of_instance 就是类变量，用于跟踪存在着多少个Test 的实例。

**实例变量：**

> 实例化之后，每个实例单独拥有的变量。

```python
class Test(object):  
    num_of_instance = 0  
    def __init__(self, name):  
        self.name = name  
        Test.num_of_instance += 1  
  
if __name__ == '__main__':  
    print Test.num_of_instance   # 0
    t1 = Test('jack')  
    print Test.num_of_instance   # 1
    t2 = Test('lucy')  
    print t1.name , t1.num_of_instance  # jack 2
    print t2.name , t2.num_of_instance  # lucy 2
```

> 补充的例子

```python
class Person:
    name="aaa"

p1=Person()
p2=Person()
p1.name="bbb"
print p1.name  # bbb
print p2.name  # aaa
print Person.name  # aaa
```

这里`p1.name="bbb"`是实例调用了类变量,这其实和上面第一个问题一样,就是函数传参的问题,`p1.name`一开始是指向的类变量`name="aaa"`,但是在实例的作用域里把类变量的引用改变了,就变成了一个实例变量,self.name不再引用Person的类变量name了.

可以看看下面的例子:

```python
class Person:
    name=[]

p1=Person()
p2=Person()
p1.name.append(1)
print p1.name  # [1]
print p2.name  # [1]
print Person.name  # [1]
```



## Python自省

这个也是python彪悍的特性.

自省就是面向对象的语言所写的程序在运行时,所能知道对象的类型.简单一句就是运行时能够获得对象的类型.比如type(),dir(),getattr(),hasattr(),isinstance().

```python
a = [1,2,3]
b = {'a':1,'b':2,'c':3}
c = True
print type(a),type(b),type(c) # <type 'list'> <type 'dict'> <type 'bool'>
print isinstance(a,list)  # True
```



## 字典推导式

可能你见过列表推导时,却没有见过字典推导式,在2.7中才加入的:

```python
d = {key: value for (key, value) in iterable}
```

## Python中单下划线和双下划线

```python
>>> class MyClass():
...     def __init__(self):
...             self.__superprivate = "Hello"
...             self._semiprivate = ", world!"
...
>>> mc = MyClass()
>>> print mc.__superprivate
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: myClass instance has no attribute '__superprivate'
>>> print mc._semiprivate
, world!
>>> print mc.__dict__
{'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
```

`__foo__`:一种约定,Python内部的名字,用来区别其他用户自定义的命名,以防冲突，就是例如`__init__()`,`__del__()`,`__call__()`这些特殊方法

`_foo`:一种约定,用来指定变量私有.程序员用来指定私有变量的一种方式.不能用from module import * 导入，其他方面和公有一样访问；

`__foo`:这个有真正的意义:解析器用`_classname__foo`来代替这个名字,以区别和其他类相同的命名,它无法直接像公有成员一样随便访问,通过对象名._类名__xxx这样的方式可以访问.



## 字符串格式化:%和.format

.format在许多方面看起来更便利.对于`%`最烦人的是它无法同时传递一个变量和元组.你可能会想下面的代码不会有什么问题:

```
"hi there %s" % name
```

但是,如果name恰好是(1,2,3),它将会抛出一个TypeError异常.为了保证它总是正确的,你必须这样做:

```
"hi there %s" % (name,)   # 提供一个单元素的数组而不是一个参数
```

但是有点丑..format就没有这些问题.你给的第二个问题也是这样,.format好看多了.

你为什么不用它?


## 10 `*args` and `**kwargs`

用`*args`和`**kwargs`只是为了方便并没有强制使用它们.

当你不确定你的函数里将要传递多少参数时你可以用`*args`.例如,它可以传递任意数量的参数:

```python
>>> def print_everything(*args):
        for count, thing in enumerate(args):
...         print '{0}. {1}'.format(count, thing)
...
>>> print_everything('apple', 'banana', 'cabbage')
0. apple
1. banana
2. cabbage
```

相似的,`**kwargs`允许你使用没有事先定义的参数名:

```python
>>> def table_things(**kwargs):
...     for name, value in kwargs.items():
...         print '{0} = {1}'.format(name, value)
...
>>> table_things(apple = 'fruit', cabbage = 'vegetable')
cabbage = vegetable
apple = fruit
```

你也可以混着用.命名参数首先获得参数值然后所有的其他参数都传递给`*args`和`**kwargs`.命名参数在列表的最前端.例如:

```
def table_things(titlestring, **kwargs)
```

`*args`和`**kwargs`可以同时在函数的定义中,但是`*args`必须在`**kwargs`前面.

当调用函数时你也可以用`*`和`**`语法.例如:

```python
>>> def print_three_things(a, b, c):
...     print 'a = {0}, b = {1}, c = {2}'.format(a,b,c)
...
>>> mylist = ['aardvark', 'baboon', 'cat']
>>> print_three_things(*mylist)

a = aardvark, b = baboon, c = cat
```

就像你看到的一样,它可以传递列表(或者元组)的每一项并把它们解包.注意必须与它们在函数里的参数相吻合.当然,你也可以在函数定义或者函数调用时用*.



## 面向切面编程AOP和装饰器

这个AOP一听起来有点懵,同学面阿里的时候就被问懵了...

装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。概括的讲，**装饰器的作用就是为已经存在的对象添加额外的功能。**

#@  Python中重载


函数重载主要是为了解决两个问题。

1. 可变参数类型。
2. 可变参数个数。

另外，一个基本的设计原则是，仅仅当两个函数除了参数类型和参数个数不同以外，其功能是完全相同的，此时才使用函数重载，如果两个函数的功能其实不同，那么不应当使用重载，而应当使用一个名字不同的函数。

那么对于情况 1 ，函数功能相同，但是参数类型不同，python 如何处理？答案是根本不需要处理，因为 python 可以接受任何类型的参数，如果函数的功能相同，那么不同的参数类型在 python 中很可能是相同的代码，没有必要做成两个不同函数。

那么对于情况 2 ，函数功能相同，但参数个数不同，python 如何处理？大家知道，答案就是缺省参数。对那些缺少的参数设定为缺省参数即可解决问题。因为你假设函数功能相同，那么那些缺少的参数终归是需要用的。

鉴于情况 1 跟 情况 2 都有了解决方案，python 自然就不需要函数重载了。



## Python中的作用域

Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定的。

当 Python 遇到一个变量的话他会按照这样的顺序进行搜索：

本地作用域（Local）→当前作用域被嵌入的本地作用域（Enclosing locals）→全局/模块作用域（Global）→内置作用域（Built-in）



## 协程



简单点说协程是进程和线程的升级版,进程和线程都面临着内核态和用户态的切换问题而耗费许多切换时间,而协程就是用户自己控制切换的时机,不再需要陷入系统的内核态.

Python里最常见的yield就是协程的思想!可以查看第九个问题.


## 闭包

闭包(closure)是函数式编程的重要的语法结构。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。

当一个内嵌函数引用其外部作作用域的变量,我们就会得到一个闭包. 总结一下,创建一个闭包必须满足以下几点:

1. 必须有一个内嵌函数
2. 内嵌函数必须引用外部函数中的变量
3. 外部函数的返回值必须是内嵌函数

感觉闭包还是有难度的,几句话是说不明白的,还是查查相关资料.

重点是函数运行后并不会被撤销,就像16题的instance字典一样,当函数运行完后,instance并不被销毁,而是继续留在内存空间里.这个功能类似类里的类变量,只不过迁移到了函数上.

闭包就像个空心球一样,你知道外面和里面,但你不知道中间是什么样.

## Python垃圾回收机制

Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。

### 1 引用计数

PyObject是每个对象必有的内容，其中`ob_refcnt`就是做为引用计数。当一个对象有新的引用时，它的`ob_refcnt`就会增加，当引用它的对象被删除，它的`ob_refcnt`就会减少.引用计数为0时，该对象生命就结束了。

优点:

1. 简单
2. 实时性

缺点:

1. 维护引用计数消耗资源
2. 循环引用

### 2 标记-清除机制

基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。

### 3 分代技术

分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。

Python默认定义了三代对象集合，索引数越大，对象存活时间越长。

举例：
当某些内存块M经过了3次垃圾收集的清洗之后还存活时，我们就将内存块M划到一个集合A中去，而新分配的内存都划分到集合B中去。当垃圾收集开始工作时，大多数情况都只对集合B进行垃圾回收，而对集合A进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合B中的某些内存块由于存活时间长而会被转移到集合A中，当然，集合A中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。
