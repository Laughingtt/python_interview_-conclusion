# python_interview_-conclusion

## python基础知识点

1. 简述多线程，多进程，线程池，进程池，协程，并说明使用场景

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

27.map函数和reduce函数区别

```
①从参数方面来讲：map()包含两个参数，第一个参数是一个函数，第二个是序列（列表 或元组）。其中，函数（即 map的第一个参数位置的函数）可以接收一个或多个参数。 educe()第一个参数是函数，第二个是序列（列表或元组）。但是，其函数必须接收两个参数。
②从对传进去的数值作用来讲：map()是将传入的函数依次作用到序列的每个元素，每个元素都是独自被函数“作用”一次 。reduce()是将传人的函数作用在序列的第一个元素得到结果后，把这个结果继续与下一个元素作用（累积计算）。
```

28. mro 是什么? 类中的一些魔法方法有哪些

```
mro是方法调用执行的dag顺序图

面向对象中带双下划线的特殊方法。
Copy
# 答案
#__setattr__: 添加/修改属性会触发它的执行
#__delattr__: 删除属性的时候会触发
#__getattr__: 只有在使用点调用属性且属性不存在的时候才会触发
# __getattribute__: 不管是否存在,我都会执行
"单下划线" 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量。

"双下划线" 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。
```

29. isinstance 和 type 的作用?

```
type和isinstance都可以判断变量是否属于某个内建类型
type只接收一个参数，不但可以判断变量是否属于某个类型，而且可以得到参数变量未知的所属的类型；而isinstance只能判断是否属于某个已知类型，不能直接得到变量未知的所属的类型
```

30. 简述TCP三次握手、四次挥手的流程

```
1首先客户端向服务端发送一个带有SYN 标志，以及随机生成的序号100(0字节)的报文
2服务端收到报文后返回一个报文(SYN200(0字节)，ACk1001(字节+1))给客户端
3客户端再次发送带有ACk标志201(字节+)序号的报文给服务端
至此三次握手过程结束，客户端开始向服务端发送数据。
1客户端向服务端发起请求：我想给你通信，你准备好了么？
2服务端收到请求后回应客户端：I'ok，你准备好了么
3客户端礼貌的再次回一下客户端：准备就绪，咱们开始通信吧！
整个过程跟打电话的过程一模一样:1喂，你在吗2在，我说的你听得到不3恩，听得到(接下来请
开始你的表演)
补充：SYN：请求询问，ACk：回复，回应。
```

https://zhuanlan.zhihu.com/p/38240894

![image](https://user-images.githubusercontent.com/56598312/201477854-acbdc95a-d52b-4094-ae39-0c6948ebdba0.png)

31. 生产者消费者模型应用场景?

```
生产者与消费者模式是通过一个容器来解决生产者与消费者的强耦合关系，生产者与消费者之间不直接进行通讯，
而是利用阻塞队列来进行通讯，生产者生成数据后直接丢给阻塞队列，消费者需要数据则从阻塞队列获取，
实际应用中，生产者与消费者模式则主要解决生产者与消费者的生产与消费的速率不一致的问题，达到平衡生产者与消费者的处理能力，而阻塞队列则相当于缓冲区。

应用场景：用户提交订单，订单进入引擎的阻塞队列中，由专门的线程从阻塞队列中获取数据并处理。

优势：
1；解耦
假设生产者和消费者分别是两个类。如果让生产者直接调用消费者的某个方法，那么生产者对于消费者就会产生依赖（也就是耦合）。
将来如果消费者的代码发生变化，可能会影响到生产者。而如果两者都依赖于某个缓冲区，两者之间不直接依赖，耦合也就相应降低了。
2：支持并发
生产者直接调用消费者的某个方法，还有另一个弊端。由于函数调用是同步的（或者叫阻塞的），在消费者的方法没有返回之前，生产者只能一直等着
而使用这个模型，生产者把制造出来的数据只需要放在缓冲区即可，不需要等待消费者来取。
3：支持忙闲不均
缓冲区还有另一个好处。如果制造数据的速度时快时慢，缓冲区的好处就体现出来了。
当数据制造快的时候，消费者来不及处理，未处理的数据可以暂时存在缓冲区中。等生产者的制造速度慢下来，消费者再慢慢处理掉。
```

32. 什么是rpc及应用场景?

```
RPC主要用于公司内部的服务调用，性能消耗低，传输效率高，服务治理方便。HTTP主要用于对外的异构环境，浏览器接口调用，APP接口调用，第三方接口调用等

```

33. 简述with方法打开处理文件帮我我们做了什么？

```
打开文件在进行读写的时候可能会出现一些异常状况，如果按照常规的f.open写法，我们需要try,except,finally，做异常判断，并且文件最终不管遇到什么情况，都要执行finally f.close()关闭文件，with方法帮我们实现了finally中f.close
		（当然还有其他自定义功能，有兴趣可以研究with方法源码）
```

34. 数据库怎么优化查询效率？

```
1、储存引擎选择：如果数据表需要事务处理，应该考虑使用InnoDB，因为它完全符合ACID特性。 如果不需要事务处理，使用默认存储引擎MyISAM是比较明智的
2、分表分库，主从。
3、对查询进行优化，要尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引
4、应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描
5、应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描
6、应尽量避免在 where 子句中使用 or 来连接条件，如果一个字段有索引，一个字段没有索引，将导致引擎放弃使用索引而进行全表扫描
7、Update 语句，如果只更改1、2个字段，不要Update全部字段，否则频繁调用会引起明显的性能消耗，同时带来大量日志
8、对于多张大数据量（这里几百条就算大了）的表JOIN，要先分页再JOIN，否则逻辑读会很高，性能很差
```

35. 索引有什么作用,有那些分类,有什么好处和坏处?

```
作用：
索引提供指向存储在表的指定列中的数据值的指针，然后根据您指定的排序顺序对这些指针排序。数据库使用索引以找到特定值，然后顺指针找到包含该值的行。这样可以使对应于表的SQL语句执行得更快，可快速访问数据库表中的特定信息。

分类：
1、唯一索引

唯一索引是不允许其中任何两行具有相同索引值的索引。当现有数据中存在重复的键值时，大多数数据库不允许将新创建的唯一索引与表一起保存。

2、主键索引

数据库表经常有一列或多列组合，其值唯一标识表中的每一行。该列称为表的主键。在数据库关系图中为表定义主键将自动创建主键索引，主键索引是唯一索引的特定类型。该索引要求主键中的每个值都唯一。当在查询中使用主键索引时，它还允许对数据的快速访问。

3、聚集索引

在聚集索引中，表中行的物理顺序与键值的逻辑（索引）顺序相同。一个表只能包含一个聚集索引。如果某索引不是聚集索引，则表中行的物理顺序与键值的逻辑顺序不匹配。与非聚集索引相比，聚集索引通常提供更快的数据访问速度。

4、索引列

可以基于数据库表中的单列或多列创建索引。多列索引可以区分其中一列可能有相同值的行。如果经常同时搜索两列或多列或按两列或多列排序时，索引也很有帮助。例如，如果经常在同一查询中为姓和名两列设置判据，那么在这两列上创建多列索引将很有意义。

优点：

1、大大加快数据的检索速度。
2、创建唯一性索引，保证数据库表中每一行数据的唯一性。
3、加速表和表之间的连接。
4、在使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间。

缺点：

1、索引需要占物理空间。
2、当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度。
```

35. 简述 Flask 上下文管理流程?

```
# a、简单来说，falsk上下文管理可以分为三个阶段：
　　1、'请求进来时'：将请求相关的数据放入上下问管理中
　　2、'在视图函数中'：要去上下文管理中取值
　　3、'请求响应'：要将上下文管理中的数据清除
# b、详细点来说：
　　1、'请求刚进来'：
        将request，session封装在RequestContext类中
        app，g封装在AppContext类中
        并通过LocalStack将requestcontext和appcontext放入Local类中
　　2、'视图函数中'：
        通过localproxy--->偏函数--->localstack--->local取值# 前提:
    不熟的话:记不太清了,应该是……分两个阶段吧   
# 创建:
    当请求刚进来的时候,会将request和session封装成一个RequestContext()对象,
    接下来把这个对象通过LocalStack()放入内部的一个Local()对象中;
　　 因为刚开始 Local 的ctx中session是空的;
　　 所以,接着执行open_session,将cookie 里面的值拿过来,重新赋值到ctx中
    (Local实现对数据隔离,类似threading.local) 
# 销毁:
    最后返回时执行 save_session() 将ctx 中的session读出来进行序列化,写到cookie
    然后给用户,接着把 ctx pop掉
　　3、'请求响应时'：
        先执行save.session()再各自执行pop(),将local中的数据清除
```

36. Flask 框架默认 session 处理机制?

```
# 前提:
    不熟的话:记不太清了,应该是……分两个阶段吧   
# 创建:
    当请求刚进来的时候,会将request和session封装成一个RequestContext()对象,
    接下来把这个对象通过LocalStack()放入内部的一个Local()对象中;
　　 因为刚开始 Local 的ctx中session是空的;
　　 所以,接着执行open_session,将cookie 里面的值拿过来,重新赋值到ctx中
    (Local实现对数据隔离,类似threading.local) 
# 销毁:
    最后返回时执行 save_session() 将ctx 中的session读出来进行序列化,写到cookie
    然后给用户,接着把 ctx pop掉
```

37. spark shuffle原理

```
https://zhuanlan.zhihu.com/p/344843993#:~:text=%E5%A6%82%E6%9E%9C%E5%9C%A8%E9%87%8D%E5%88%86%E5%8C%BA%E7%9A%84%E8%BF%87%E7%A8%8B%E4%B8%AD%EF%BC%8C%E5%A6%82%E6%9E%9C%E6%95%B0%E6%8D%AE%E5%8F%91%E7%94%9F%E4%BA%86%E8%B7%A8%E8%8A%82%E7%82%B9%E7%A7%BB%E5%8A%A8%EF%BC%8C%E5%B0%B1%E8%A2%AB%E7%A7%B0%E4%B8%BA%20Shuffle%20%EF%BC%8C%E5%9C%A8%20Spark%20%E4%B8%ADShuffle%20%E8%B4%9F%E8%B4%A3%E5%B0%86%20Map%20%E7%AB%AF%E7%9A%84%E5%A4%84%E7%90%86%E7%9A%84%E4%B8%AD%E9%97%B4%E7%BB%93%E6%9E%9C%E4%BC%A0%E8%BE%93%E5%88%B0,%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F%E6%9C%89%E4%B8%A4%E7%A7%8D%EF%BC%9AHash%20based%20Shuffle%20%E4%B8%8E%20Sort%20based%20Shuffle%E3%80%82%20
```

38. 数据库索引

```
1. 为什么需要索引
        如果不存在索引的话，在数据库中查找数据需要逐行遍历，对行内的数据的关键字进行对比，再进行增删改查操作，如果存在索引，能很快定位到你需要的数据在那一行，不需要对行内的数据进行对比。索引可以加快数据查找的速度，但一般索引会存在单独一张表，定位到索引之后再去数据表中查找
        
2. 索引的原理B Tree
        索引查找一般需要快速定位到需要的信息位置，所以一般索引查找采用B tree数据结构，B+树中的B代表平衡（balance），而不是二叉（binary），因为B+树是从最早的平衡二叉树演化而来的。插入或者删除元素都会导致节点发生裂变反应，有时候会非常麻烦，但正因为如此才让B树能够始终保持多路平衡，这也是B树自身的一个优势：自平衡。
	平衡(在一般平衡树数据结构中)背后的想法是任何两个子树之间的深度差异为零或一(取决于树)。换句话说，用于查找叶节点的比较次数总是相似的。

    B tree特点 
    https://blog.csdn.net/weixin_52622200/article/details/118529638
    * B树可以定义一个m值作为预定范围，即m路(阶)B树。
    * 每个节点最多有m个孩子。
    * 每个节点至少有ceil(m/2)个孩子，除了根节点和叶子节点外。
    * 对于根节点，子树个数范围为[2,m]，节点内值的个数范围为[1,m-1]。
    * 对于非根节点，节点内的值个数范围为[ceil(m/2)-1,m-1]。
    * 根节点(非叶子节点)至少有两个孩子。
    * 一个有k个孩子的非叶子节点包含k-1个值。
    * 所有叶子节点在同一层。
    * 节点内的值按照从小到大排列。
    * 父节点的若干值作为分离值分成多个子树，左子树小于对应分离值，对应分离值小于右子树。

3.    B+Tree相对于B-Tree有几点不同：
        非叶子节点只存储键值信息。
        所有叶子节点之间都有一个链指针。
        数据记录都存放在叶子节点中。

```

39. IO多路复用

```
1. 什么是IO多路复用
        单线程或单进程同时监测若干个文件描述符是否可以执行IO操作的能力。

        select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理就是select /epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。

    I/O多路复用指：通过一种机制，可以监视多个描述符，监听的描述符发生了改变，比如可读了或者可写了，一旦它发生了改变，那我就可以得到一个回调信息或者我主动的去知道系统发生变化了！普通文件操作所有系统都是完成不了的，普通文件是属于I/O操作！但是对于python来说文件变更python是监控不了的，所以我们能用的只有是“终端的输入输出，Socket的输入输出”
```

40. 数据库事务
    https://zhuanlan.zhihu.com/p/43493165

```
数据库的事务（Transaction）是一种机制、一个操作序列，包含了一组数据库操作命令。事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么都执行，要么都不执行，因此事务是一个不可分割的工作逻辑单元。

在数据库系统上执行并发操作时，事务是作为最小的控制单元来使用的，特别适用于多用户同时操作的数据库系统。例如，航空公司的订票系统、银行、保险公司以及证券交易系统等。


原子性（Atomicity）: 事务要么全部完成，要么全部取消。 如果事务崩溃，状态回到事务之前（事务回滚）。
隔离性（Isolation）: 如果2个事务 T1 和 T2 同时运行，事务 T1 和 T2 最终的结果是相同的，不管 T1和T2谁先结束。
持久性（Durability）: 一旦事务提交，不管发生什么（崩溃或者出错），数据要保存在数据库中。
一致性（Consistency）: 只有合法的数据（依照关系约束和函数约束）才能写入数据库

原子性，确保不管交易过程中发生了什么意外状况（服务器崩溃、网络中断等），不能出现A账户少了一个亿，但B账户没到帐，或者A账户没变，但B账户却凭空收到一个亿（数据不一致）。A和B账户的金额变动要么同时成功，要么同时失败(保持原状)。
隔离性，如果A在转账1亿给B（T1），同时C又在转账3亿给A（T2），不管T1和T2谁先执行完毕，最终结果必须是A账户增加2亿，而不是3亿，B增加1亿，C减少3亿。
持久性，确保如果 T1 刚刚提交，数据库就发生崩溃，T1执行的结果依然会保持在数据库中。
一致性，确保钱不会在系统内凭空产生或消失， 依赖原子性和隔离性。
```

41. 线程阻塞

```
什么是线程阻塞？
            在某一时刻某一个线程在运行一段代码的时候，这时候另一个线程也需要运行，但是在运行过程中的那个线程执行完成之前，另一个线程是无法获取到CPU执行权的（调用sleep方法是进入到睡眠暂停状态，但是CPU执行权并没有交出去，而调用wait方法则是将CPU执行权交给另一个线程），这个时候就会造成线程阻塞。

            1. join
                让一个线程等待另一个线程完成才继续执行。如A线程执行体中调用B线程的join()方法，则A线程被阻塞，直到B线程执行完为止，A才能得以继续执行。
            2. 线程让步：yield()
            当某个线程调用yiled()方法从运行状态转换到就绪状态后，CPU从就绪状态线程队列中只会选择与该线程优先级相同或优先级更高的线程去执行。
            3. sleep
            让当前的正在执行的线程暂停指定的时间，并进入阻塞状态。在其睡眠的时间段内，该线程由于不是处于就绪状态，因此不会得到执行的机会。即使此时系统中没有任何其他可执行的线程，出于sleep()中的线程也不会执行。因此sleep()方法常用来暂停线程执行。

为什么会出现线程阻塞？

1.睡眠状态：当一个线程执行代码的时候调用了sleep方法后，线程处于睡眠状态，需要设置一个睡眠时间，此时有其他线程需要执行时就会造成线程阻塞，而且sleep方法被调用之后，线程不会释放锁对象，也就是说锁还在该线程手里，CPU执行权还在自己手里，等睡眠时间一过，该线程就会进入就绪状态，典型的“占着茅坑不拉屎”；

2.等待状态：当一个线程正在运行时，调用了wait方法，此时该线程需要交出CPU执行权，也就是将锁释放出去，交给另一个线程，该线程进入等待状态，但与睡眠状态不一样的是，进入等待状态的线程不需要设置睡眠时间，但是需要执行notify方法或者notifyall方法来对其唤醒，自己是不会主动醒来的，等被唤醒之后，该线程也会进入就绪状态，但是进入仅需状态的该线程手里是没有执行权的，也就是没有锁，而睡眠状态的线程一旦苏醒，进入就绪状态时是自己还拿着锁的。等待状态的线程苏醒后，就是典型的“物是人非，大权旁落“；

3.礼让状态：当一个线程正在运行时，调用了yield方法之后，该线程会将执行权礼让给同等级的线程或者比它高一级的线程优先执行，此时该线程有可能只执行了一部分而此时把执行权礼让给了其他线程，这个时候也会进入阻塞状态，但是该线程会随时可能又被分配到执行权，这就很”中国化的线程“了，比较讲究谦让；

4.自闭状态：当一个线程正在运行时，调用了一个join方法，此时该线程会进入阻塞状态，另一个线程会运行，直到运行结束后，原线程才会进入就绪状态。这个比较像是”走后门“，本来该先把你的事情解决完了再解决后边的人的事情，但是这时候有走后门的人，那就会停止给你解决，而优先把走后门的人事情解决了；
```

42. 守护线程

```
如果当前python线程是守护线程，那么意味着这个线程是“不重要”的，“不重要”意味着如果他的主进程结束了但该守护线程没有运行完，守护进程就会被强制结束。如果线程是非守护线程，那么父进程只有等到守护线程运行完毕后才能结束。

守护线程的作用：
    守护线程作用是为其他线程提供便利服务，守护线程最典型的应用就是 GC (垃圾收集器)。
守护线程的特点：
    只要当前 主线程中尚存任何一个非守护线程没有结束，守护线程就全部工作；
	只有当最后一个非守护线程结束是，守护线程随着主线程一同结束工作。
```

43. python GIL锁

```
简而言之，Python全局解释器锁或GIL是一种互斥锁（或锁），仅允许一个线程持有Python解释器的控制权。

这意味着在任何时间点只能有一个线程处于执行状态。对于执行单线程程序的开发人员而言，GIL的影响并不明显，但它可能是CPU绑定和多线程代码的性能瓶颈。

GIL锁产生原因:
	python的内存管理，Python使用引用计数进行内存管理。这意味着用Python创建的对象具有引用计数变量，该变量跟踪指向该对象的引用数。当此计数达到零时，
	将释放对象占用的内存。解决解释器中多个线程的竞争资源问题。
	
	
GIL锁影响:
	Python的多线程在多核CPU上，只对于IO密集型计算产生正面效果；而当有至少有一个CPU密集型线程存在，那么多线程效率会由于GIL而大幅下降。

```

44. 线程池实现

核心线程数、最大线程数、任务队列。核心线程数指的是线程池的基本大小；最大线程数指的是，同一时刻线程池中线程的数量最大不能超过该值；任务队列是当任务较多时，线程池中线程的数量已经达到了核心线程数，这时候就是用任务队列来存储我们提交的任务。
与其他池化技术不同的是，线程池是基于生产者-消费者模式来实现的，任务的提交方是生产者，线程池是消费者。当我们需要执行某个任务时，只需要把任务扔到线程池中即可。线程池中执行任务的流程如下图如下。

![image](https://user-images.githubusercontent.com/56598312/201480576-538745a1-f69e-4f0b-a67f-f6be04f51d10.png)

先判断线程池中线程的数量是否超过核心线程数，如果没有超过核心线程数，就创建新的线程去执行任务；如果超过了核心线程数，就进入到下面流程。
判断任务队列是否已经满了，如果没有满，就将任务添加到任务队列中；如果已经满了，就进入到下面的流程。
再判断如果创建一个线程后，线程数是否会超过最大线程数，如果不会超过最大线程数，就创建一个新的线程来执行任务；如果会，则进入到下面的流程。
执行拒绝策略。

45. 信号量（Semaphore）

信号量也是一把锁，用来控制线程并发数的。

信号量的主要作用：

**主要用在数据库应用中，比如连接数据库的连接，限制同时连接的数量，如数据库连接池。**

BoundedSemaphore或Semaphore管理一个内置的计数 器，每当调用acquire()时-1，调用release()时+1。

计数器不能小于0，当计数器为 0时，acquire()将阻塞线程至同步锁定状态，直到其他线程调用release()。(类似于停车位的概念)

```python
import threading, time


class myThread(threading.Thread):
    def run(self):
        if semaphore.acquire():  # 每次最大5个线程运行，具体哪些线程运行，系统自动调配。
            print(self.name)
            time.sleep(3)
            semaphore.release()


if __name__ == "__main__":
    semaphore = threading.Semaphore(5)  # 每次最大5个线程运行
    thrs = []
    for i in range(100):  # 创建了100个线程。
        thrs.append(myThread())
    for t in thrs:
        t.start()  # 启动了100个线程

```

46. k8s (Kubernetes)
    Kubernetes 这个单词来自于希腊语，含义是舵手或领航员。K8S是它的缩写，用“8”字替代了“ubernete”这8个字符。
    容器的编排、管理和调度一套管理系统，对Docker及容器进行更高级更灵活的管理

利用kubernetes，你可以快速高效地响应客户如下请求：

* 应用程序的动态、精准部署
* 应用程序的动态扩展
* 无缝推出新功能
* 按需优化使用硬件资源

47. CICD

假设你在本地修改了一些代码以增加一些特性，当你把这些改动 push 到远程仓库的特性分支后，项目里设定的 CICD pipeline
就被触发了，一般流程如下：

运行自动脚本（串行或并行）：

* 构建并测试应用
* 在合并前 review 改动
  上述流程如果没有问题就可以把改动合并入主分支，CD 会自动把它部署到产品中，如果发现了任何问题，还能一键回滚。
  工作流程图:
  ![image](https://pic3.zhimg.com/80/v2-e319f6f348bfabeeb16995ada4241076_1440w.webp)
