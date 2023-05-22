### Java
java基础

---

#### [Installation](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
```
# 下载解压到/usr/local：/usr/local/jdk1.8.0_211

# /etc/profile.d下新建java8.sh添加以下内容
export JAVA_HOME=/usr/local/jdk1.8.0_281
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

# 生效profile
source /etc/profile

# 测试
java -version
```


#### Java内存/垃圾回收
- 线程私有
    - PC寄存器（程序计数器），指向虚拟机字节码指令，唯一没有OOM
    - 虚拟机栈（为每个方法创建一个栈帧，存储局部变量、操作数栈、动态链接、方法出口）
    - 本地方法栈
- 线程共享
    - 方法区/永久代（Java8叫元空间），运行时常量池
    - 堆（新生代<MinorGC>、老年代<MajorGC/FullGC>）
- 直接内存
- 回收机制：如何确定垃圾（引用计数器，可达性分析-GC ROOTS）；复制法、标记清除、标记整理清除、分代回收（年轻代复制法，老年代标记整理清除）
- 垃圾回收器：Serial（新生代-单线程复制算法），ParNew（新生代-Serial+多线程），Parallel Scavenge（新生代-多线程复制算法，自适应调节策略），Serial Old（老年代-标记整理算法），Parallel Old（老年代-多线程标记整理算法），CMS（老年代-多线程标记清楚算法），G1（标记整理算法，精确控制停顿时间，采用区域划分和优先级区域回收机制）


#### ArrayList和LinkedList
ArrayList 是一个可改变大小的数组；LinkedList 是一个双链表，在添加和删除元素时具有比ArrayList更好的性能


#### Java集合
- hashMap/hashTable：HashTable线程安全，代码块中用syn修饰
- ConcurrentHashMap：高并发性能更好，内部实现分段锁
- hashMap实现原理：数组+链表
- hashCode、equals，为什么要重写：equeals比较的是内存地址，重写比较值是否相等；
- Map如何遍历（HashMap, TreeMap, LinkedHashMap, Hashtable）：使用keySet()方法遍历、使用map的values()方法遍历集合的values、使用map的entrySet()方法遍历、通过entrySet()返回的Set的iterator遍历


#### 多线程并发
- 锁：乐观锁（CAS-Conmpare And Swap，带版本号和时间戳实现<ABA问题>）和悲观锁（Synchronized）、自旋锁、同步锁（独占式的悲观锁，同时属于可重入）
- 如何线程安全： 互斥同步锁（悲观锁）、非阻塞同步锁、ThreadLocal/Volaitile（保证可见性不能保证原子性）


#### 线程池
- 参数
    - corePoolSize：核心线程池大小，当新的任务到线程池后，线程池会创建新的线程
    - maximumPoolSize：最大线程池大小，顾名思义，线程池能创建的线程的最大数目
    - workQueue：任务阻塞队列
- 饱和策略
    - AbortPolicy，表示无法处理新任务时抛出异常, 默认策略
    - DiscardOldestPolicy： 该策略将丢弃最老的一个请求，也就是即将被执行的任务，并尝试再次提交当前任务
    - CallerRunsPolicy：用调用者所在线程来运行任务
    - DiscardPolicy：不处理，丢弃掉
    - 自定义
- 作用：
    - 1.重用线程池的线程，避免因为线程的创建和销毁锁带来的性能开销
    - 2.有效控制线程池的最大并发数，避免大量的线程之间因抢占系统资源而阻塞
    - 3.能够对线程进行简单的管理，并提供一下特定的操作如：可以提供定时、定期、单线程、并发数控制等功能


#### 类加载机制
- 加载-验证-准备-解析-初始化
- 启动类加载器，扩展类加载器，应用类加载器。双亲委派加载-委派给父类去加载
- 双亲委派是可以打破的，Tomcat就违反了双亲委派原则


#### Exception
- RuntimeException是Exception子类
- RuntimeException可以不需要进行强制处理，Exception必须强制处理
- 常见RuntimeException类：NumberFormatException、ClassCastException、NullPointerException


#### String
- String是字符串常量，字符串首选类型，内容不允许修改
- StringBuffer线程安全的，内容允许修改
- StringBuilder是非线程安全的，内容允许修改
- StringBuffer容量：无参构造初始化容量为16， 有参构造初始化容量为初始化字符串长度+16；扩容2倍+2
- 使用场景：String - 少量的字符串操作；StringBuilder - 单线程下在字符缓冲区进行大量操作；StringBuffer - 多线程下在字符缓冲区进行大量操作
