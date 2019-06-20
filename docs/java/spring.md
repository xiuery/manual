### Spring
Spring原理

---

#### Spring体系结构
核心容器(Core Container)、数据访问/集成部分(Data Access/Integration)、web、AOP、test；加载流程：应用上下文加载类、


#### AOP
- 实现原理： 动态代理（对接口）和CGlib（对类，创建时候慢，执行效率高）
- AspectJ注解@Aspect
    - @Before
    - @After
    - @AfterReturning
    - @AfterThrowing
    - @Around


#### Ioc
Ioc 是一个通用的设计原则, DI 是具体的设计模式. DI 是 IoC 最典型的实现(但并不是唯一), 所以有时 IoC 和 DI 经常混用


#### 7种传播属性
REQUIRED、SUPPORTS、MANDATORY、REQUIRES_NEW、NOT_SUPPORTED、NEVER、NESTED（这个和REQUIRED区别在于一个是加入到一个事务，一个是在嵌套事务运行）


#### 编程式事务和申明式事务
- 编程式事务：在代码中显式调用beginTransaction()、commit()、rollback()等事务管理相关的方法。灵活
- 申明式事务：在配置文件中做相关的事务规则声明（或通过等价的基于标注的方式）。粒度不够，通过变通的方法可以解决


#### mycat分表分库
通过定义路由规则来（路由规则里面会定义分片字段，以及分片算法），分片算法有多种，你所说的hash是其中一种，还有取模、按范围分片等等



