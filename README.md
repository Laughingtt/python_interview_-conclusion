# python_interview_-conclusion


## python基础知识点

1.  简述多线程，多进程，线程池，进程池，协程，并说明使用场景

```
进程: 一个运行起来的程序或者软件叫做进程，进程是操作系统分配资源的 基本单位
线程：指进程内的一个执行单元,也是进程内的可调度实体。
协程：是一种程序组件，是由子例程（过程、函数、例程、方法、子程序）的概念泛化而来的，子例程只有一个入口点且只返回一次，而协程允许多个入口点，可以在指定位置挂起和恢复执行。
```


2. 使用多进程时，进程之间通信有几种方式


```
主要Queue和Pipe这两种方式:
Queue用于多个进程间实现通信
Pipe是两个进程的通信
还可以使用共享内存的方式
```

3. python 中赋值，浅拷贝，深拷贝区别


```
赋值: 建立对象的引用值
浅拷贝: 只拷贝父对象，不会拷贝对象的内部的子对象
深拷贝: 拷贝对象及其子对象
```

4. 简述python 垃圾回收机制的原理


```
python采用的是引用计数机制为主，标记-清除和分代收集两种机制为辅的策略
引用计数：
当值被多次引用时候，不会在内存中重复创建数据，而是引用计数器+1 。 当对象被销毁时候同时会让引用计数器-1,如果引用计数器为0，则将对象从refchain链表中摘除，同时在内存中进行销毁（实则放入缓存）。

标记清除：创建特殊链表专门用于保存 列表、元组、字典、集合、自定义类等对象，之后再去检查这个链表中的对象是否存在循环引用，如果存在则让双方的引用计数器均 - 1 。

分代回收：对标记清除中的链表进行优化，将那些可能存在循引用的对象拆分到3个链表，链表称为：0/1/2三代，每代都可以存储对象和阈值，当达到阈值时，就会对相应的链表中的每个对象做一次扫描，除循环引用各自减1并且销毁引用计数器为0的对象。
```
5. 迭代器和可迭代对象,生成器区别


```
可迭代对象: 在Python中，但凡内部含有iter方法，且返回一个迭代器（生成器）的可被定为是可迭代对象。可直观的查看里面的数据,但比较占用内存

迭代器:是一个可以迭代取值的工具。专业点就是看类型中有没有iter和next方法，是一个非常节省内存，可以记录取值位置，可以直接通过循环+next方法取值，但是不直观，操作方法比较单一的数据集。当你数据量过大，大到足以撑爆你的内存或者你以节省内存为首选因素时，将数据集设置为迭代器是一个不错的选择。

生成器: 使用了 yield 的函数被称为生成器（generator）,跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

```

6. 将字典 dic = {"bob":22,"hele":21,"cansel":21} 先按照value进行排序，再按照key进行排序

```
sorted(dic.items(),key=lambda x:(x[1],x[0]))
[('cansel', 21), ('hele', 21), ('bob', 22)]
```

7. 一行代码实现1-100之和(可以有多种解法)

```
循环: 
    c = 0
    for i in range(100):
        c+=i
sum求和:
    sum(range(100))
reduce累加:
    import functools
    functools.reduce(lambda x,y:x+y,range(100))
```

8. Python-遍历列表时删除元素的正确做法

```
遍历在新在列表操作，删除时在原来的列表操作:
a = [1,2,3,4,5,6,7,8]
print(id(a))
print(id(a[:]))
for i in a[:]:
    if i>5:
        pass
    else:
        a.remove(i)
    print(a)
print('-----------')

使用filter 筛选 :
a=[1,2,3,4,5,6,7,8]
b = filter(lambda x: x>5,a)
print(list(b))

列表解析:
a=[1,2,3,4,5,6,7,8]
b = [i for i in a if i>5]

```

9. python代码实现删除一个list里面的重复元素

```
def distFunc1(a):
    """使用集合去重"""
    a = list(set(a))
    print(a)

def distFunc2(a):
    """将一个列表的数据取出放到另一个列表中，中间作判断"""
    list = []
    for i in a:
        if i not in list:
            list.append(i)
    #如果需要排序的话用sort
    list.sort()
    print(list)

def distFunc3(a):
    """使用字典"""
    b = {}
    b = b.fromkeys(a)
    c = list(b.keys())
    print(c)
```

10. python中类方法、类实例方法、静态方法有何区别？


```
类方法: 是类对象的方法，在定义时需要在上方使用 @classmethod 进行装饰,形参为cls，表示类对象，类对象和实例对象都可调用

类实例方法: 是类实例化对象的方法,只有实例对象可以调用，形参为self,指代对象本身;

静态方法: 是一个任意函数，在其上方使用 @staticmethod 进行装饰，可以用对象直接调用，静态方法实际上跟该类没有太大关系
```

11. python Map,reduce,filter 用法

```
map ：
求一个序列或者多个序列进行函数映射之后的值
x = [1,2,3,4,5]
y = [2,3,4,5,6]
list(map(lambda x,y:(x*y)+2,x,y))
# 输出：
[4, 8, 14, 22, 32]

reduce:
对一个序列进行压缩运算，得到一个值
from functools import reduce
y = [2,3,4,5,6]
reduce(lambda x,y: x + y,y) # 直接返回一个值


filter: 
filter的功能是过滤掉序列中不符合函数条件的元素，当序列中要删减的元素可以用某些函数描述时，就应该想起filter函数。
x = [1,2,3,4,5]
list(filter(lambda x:x%2==0,x)) 
输出: [2, 4]
# 找出偶数。python3.*之后filter函数返回的不再是列表而是迭代器，所以需要用list转换。


```

12. python中钻石继承的执行顺序是什么？

```
继承关系 ：A -> B,C -> D

类的实例话顺序如下：A->C->B->D
类方法继承执行顺序: D->B->C->A （深度优先）
查看类解析顺序(method resolution orde)：D.mro()

```

13. 装饰器的作用和功能

```
装饰器的作用是用来装饰函数或者类，在其基本的方法下增加新的方法，不改变原方法。

引入日志

函数执行时间统计

执行函数前预备处理

执行函数后的清理功能

权限校验等场景

缓存

import time

def deco(f):
    def wrapper():
        start_time = time.time()
        f()
        end_time = time.time()
        execution_time = (end_time - start_time)*1000
        print("time is %d ms" %execution_time )
    return wrapper

@deco
def f():
    print("hello")
    time.sleep(1)
    print("world")

if __name__ == '__main__':
    f()

```

14. 对设计模式的理解，简述你了解的设计模式？

```
工厂模式:工厂模式用于生成不同类型的有着相似功能的类，factory的作用在于生成类

建造着模式:建造模式：建造者模式更为高层一点，将所有细节都交由子类实现

单例模式: 实例化的类只能存在一个
适配器模式: 将一个类的接口转换成客户希望的另一个接口
```

15. 描述数组、链表、队列、堆栈的区别？

```
数组与链表是数据存储方式的概念，数组在连续的空间中存储数据，而链表可以在非连续的空间中存储数据；

队列和堆栈是描述数据存取方式的概念，队列是先进先出，而堆栈是后进先出；队列和堆栈可以用数组来实现，也可以用链表实现。
```

16. 递归fib函数,递归函数条件是？动态规划的思想是什么？

```
递归函数: 结束的条件以及递归的关系，需要注意的是递归深度不宜太深
动态规划：动态规划的实质是分治思想和解决冗余。因此，动态规划是一种将问题实例分解为更小的/相似的子问题，并存储子问题的解，使得每个子问题只求解一次，最终获得原问题的答案，以解决最优化问题的算法策略。
```

17. 了解的排序算法的一些思想?

```
冒泡排序的基本思想是，对相邻的元素进行两两比较，顺序相反则进行交换，这样，每一趟会将最小或最大的元素“浮”到顶端，最终达到完全有序。

插入排序基本思想是每一步将一个待排序的记录，插入到前面已经排好序的有序序列中去，直到插完所有元素为止。

快速排序通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

```


18. 一只青蛙要跳上n层高的台阶，一次能跳一级，也可以跳两级，请问这只青蛙有多少种跳上这个n层台阶的方法？

```
递归: 递推公式f(n)=f(n-1) + f(n-2)
动态规划：记录每次跳跃的值，不用每次都去计算。
```

19. Linux的相关指令，grep,awk,sed

```
grep 文本过滤命令
sed 行编辑器或者替换字符
awk 报告生成器
```

20. 分布式计算和集群计算区别

```
分布式计算：分布式是指将多台服务器集中在一起，每台服务器都实现总体中的不同业务，做不同的事情。并且每台服务器都缺一不可，如果某台服务器故障，则网站部分功能缺失，或导致整体无法运行。存在的主要作用是大幅度的提高效率，缓解服务器的访问和存储压力。

集群：集群是是指将多台服务器集中在一起，每台服务器都实现相同的业务，做相同的事情。但是每台服务器并不是缺一不可，存在的作用主要是缓解并发压力和单点故障转移问题。可以利用一些廉价的符合工业标准的硬件构造高性能的系统。实现：高扩展、高性能、低成本、高可用！
```

21. git reset、git revert 和 git checkout 有什么区别 reverty撤销该版本的修改

```
git reset : 退回到某次提交的节点
git revert: 撤销本次版本的修改
git checkout : 切换分支
```

22. 从另外一条分支中提取出某一个commit记录?

```
使用cherry-pick
```

23. git merge和git rebase区别

```
git merge : 将分支的修改合并到主线，以提交顺序合并，如果多个人同一时间开发，会有错综发杂的提交线

git rebase: 将分支修改的内容变基到主线，将最新的记录全部执行主线的开头位置，并删除之前分支的记录，使得主线始终保持在一条线
```


24. 索引的工作原理及其种类

```
数据库索引，是数据库管理系统中一个排序的数据结构，以协助快速查询，更新数据库表中数据。索引的实现通常使用B树以其变种B+树。

在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。

为表设置索引要付出代价的：一是增加了数据库的存储空间，二是在插入和修改数据时要花费较多的时间（因为索引也要随之变动）
```

25. drop,delete与truncate的区别

```
drop直接删掉表，truncate删除表中数据，再插入时自增长id又从1开始，delete删除表中数据，可以加where字句。

1.delete 语句执行删除的过程是每次从表中删除一行，并且同时将该行的删除操作作为事务记录在日志中保存以便进行回滚操作。truncate table则一次性地从表中删除所有的数据并不把单独的删除操作记录记入日志保存，删除行是不能恢复的。并且在删除的过程中不会激活与表有关的删除触发器，执行速度快。

2.表和索引所占空间。当表被truncate后，这个表和索引所占用的空间会恢复到初始大小，而delete操作不会减少表或索引所占用的空间。drop语句将表所占用的空间全释放掉。

3.一般而言，drop>truncate>delete

4.应用范围。truncate只能对table，delete可以是table和view

5.truncate和delete只删除数据，而drop则删除整个表（结构和数据)

6.truncate与不带where的delete:只删除数据，而不删除表的结构（定义）drop语句将删除表的结构被依赖的约束(constrain),触发器（trigger)索引(index);依赖于该表的存储过程/函数将被保留，但其状态会变为:invalid.
```

26. Redis有哪些优缺点?

```
优点

读写性能优异， Redis能读的速度是110000次/s，写的速度是81000次/s。

支持数据持久化，支持AOF和RDB两种持久化方式。

支持事务，Redis的所有操作都是原子性的，同时Redis还支持对几个操作合并后的原子性执行。

数据结构丰富，除了支持string类型的value外还支持hash、set、zset、list等数据结构。

支持主从复制，主机会自动将数据同步到从机，可以进行读写分离。

缺点

数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上。

Redis 不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。

主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性。

Redis 较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，运维人员在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费
```
