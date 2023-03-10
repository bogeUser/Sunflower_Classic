# Python底层

## 1. Python是如何进行内存管理的

**Python进行内存管理可以从三个方面来讲**:

- **引用计数机制**: python内部使用引用计数来保持追踪内存中的对象，Pytho内部会自动记录当前的引用计数。当一个对象被创建(对象创建，对象被作为参数传递，作为容器对象的一个元素)。该对象的引用计数会自动加1，当该对象被销毁时，引用计数减1。
- **垃圾回收机制**:当内存中有不再使用的部分时，垃圾收集器就会自动把他们清理掉。它会去检查那些引用计数为0的对象，然后清除其在内的空间。当两个对象相互引用时，他们本身对其它的引用计数已经为0了，自然也会被清理掉。
- **内存池机制**:内存池机制(Pymalloc)，即预先申请一定数量的，巨细相等的内存块留作备用，当有新的内存需求时，就先从内存池中分配内存给该需求。在python种，很多时候申请的都是小块内存，这些内存在申请之后很快又回被释放，由于这些内存的申请并不是为了创建对象，所以并没有对象一级的内存池机制。这就意味着python在运行期间会大量的加速执行malloc和free操作，频繁的在用户态和和心态之间切换，这将严重影响python的执行效率。为了加速python的执行效率，所以引入了一个内存池机制，用于管理对小块内存的申请和释放。而python的内存管理机制---pymalloc由两套完成，一套针对小目标，就是巨细于256bit时，pymalloc会在内存池中请求内存空间；当大于256bit时，则会直接履行new/malloc的行为来请求内存空间。关于释放内存方面，当一个目标的引用计数为0时，python就会调用它的析构函数。在析构时，也采用了内存池机制，从内存池来的内存会被归还到内存池中，以防止频繁的释放操作。另一套是针对大于256bits时的操作，当某个对象的内存需要大于256bitys的内存时，会使用系统的malloc。另外python对象，list都有其独立的私有内存池，对象间不共享他们的内存池。也就是说如果分配又释放了大量的整数，用于缓存这些整数的内存就不能再被分配给浮点数。
- **python的垃圾回收主要以引用计数为主，标记-清除和分代清除为辅的机制，其中标记-清除和分代回收主要是为了处理循环引用问题**

## 2. python是如何被解释的

python语言是一种解释性语言，他的源码可以直接运行。python解释器会讲过源码转成中间语言，之后再翻译成机器码再执行。

## 3. 什么是python的命名空间

命名空间是名字和对象的映射。也就是可以把一个namespace理解为一个字典，实际上很多当前的python实现namespace就是用字典实现的。并且各个命名空间是独立的，没有任何关系，所以一个命名空间不能有重名，但不同的命名空间可以重名而不受影响。

## 4. 提高python运行效率的方法

- **用生成器，因为可以节约大量的内存**
- **循环代码优化，避免过多重复代码的知心**
- **核心模块用Cpython PyPy等，提高效率**
- **多进程、多线程、协程**
- **多个if elif条件判断，可以把最有可能先发生的条件放到前面，这样可以减少程序判断的次数，提高效率**

## 5. python 语言有哪些缺点

- **运行速度慢，Python的运行速度相比C、C++、Java慢**
- **代码不能加密，因为Python是解释性语言，它的源码是以明文的形式存在**
- **线程不能利用多CPU，由于GIL(global interpreter lock)即全局解释器锁的存在，使得python在任意时刻仅有一个线程在执行任务**

## 6. 但退出python时，是否释放全部内存

不是，循环引用其他对象或引用自全局命名空间的对象的模块，在python退出时并非完全释放。另外，也不会释放C库保留的内存部分。

## 7. Python import的导入机制

## 8. 什么是GIL

python代码的执行是由Python虚拟机(也叫解释器主循环，CPython)来控制，Python在设计之初就考虑到要在解释器的主循环中，同一时刻只有一个线程在执行，即在任意时刻，只有一个主线程在解释器中运行。对于Python虚拟机的访问由全局解释器锁(GIL)来控制，正式这个锁能保证在同一时刻只有一个线程在运行。

在多线程环境中，python虚拟机按以下方式执行:

- 设置GIL
- 切换到一个线程去执行
- 运行:
  - a. 指定数量的字节码令
  - b. 线程主动让出控制
- 把线程设置为睡眠状态
- 解锁GIL
- 再次重复以上步骤

**当调用外部代码(如C/C++扩展函数)的时候，GIL将被锁定，直到这个函数结束为止(由于在这期间没有python的字节码被运行，所以不会做线程切换)**

## 9. \_\_getattr\_\_和\_\_getattribute\_\_的区别

- **\_\_getattr\_\_**: 当对象访问类的属性存在时就不会调用\_\_getattr\_\_方法，如果一个类中没有实现\_\_getattr\_\_，那么当通过实例对象、属性、访问一个不能存在的属性时，会调用父类的\_\_getattr\_\_，而在父类的\_\_getattr\_\_中实现的该方法默认是抛出一个异常。如果在一个类中实现\_\_getattr\_\_，那么当通过实例对象、属性访问一个不存在的属性时，\_\_getattr\_\_会被自动调用，可以在这个方法中编写自己的逻辑代码。
- **\_\_getattribute\_\_**: 无论对象访问类的属性是否存在都会调用\_\_getattribute\_\_方法。当\_\_getattr\_\_ 和 \_\_getattribute\_\_ 同时存在，会优先调用\_\_getattribute\_\_。
- **当创建一个类时，默认集成的是object类，而object类中这两个方法模型是抛出异常，所以如果在创建类时重写了这两个方法，那么就可以通过实际业务需求进行处理而不抛出异常**

## 10. 描述符的作用

当访问一个属性时，可以不直接给一个值，而是接一个描述符(描述符对象)，让访问和修改设置时自动调用\_\_get\_\_方法和\_\_set\_\_方法。再在\_\_get\_\_方法和\_\_set\_\_犯法中进行某种处理，就可以实现更改操作属性的行为目的。

## 11. 数据描述符和非数据描述符的区别

- **数据描述符(资料描述符): ** 同时定义了\_\_get\_\_ 和 \_\_set\_\_方法的描述符称为数据描述符
- **非数据描述符(非资料描述符)**: 只定义了\_\_get\_\_的描述符称为非数据描述符
- **区别**: 当属性名和描述符名相同时，在访问这个同名属性时，如果是资料描述符就先访问描述符，如果是非资料描述符就先访问属性

## 12. 为什么有了GIL还要互斥锁

GIL是为了防止资源竞争，防止的是多线程抢占CPU资源，由于GIL的原因，当一个线程在运行的时候，其他线程只能等待。互斥锁要是为了防止资源竞争，防止的是程序中的资源，比如全局变量。目的是为了防止将来造成的资源混乱。

GIL保证在python解释器中从头到尾只有一个线程在运行，而互斥锁保证从头到尾能够把业务运行完。



