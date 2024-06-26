# Java 题组 - 01

## SpringBoot 也有定时任务？是什么注解？

在 SpringBoot 中使用定时任务主要有两种不同的方式，一个就是使用 Spring 中的 `@Scheduled` 注解，另一个则是使用第三方框架 `Quartz`。

使用 Spring 中的 `@Scheduled` 的方式主要通过 `@Scheduled` 注解来实现

使用 `Quartz` ，则按照 `Quartz` 的方式，定义 `Job` 和 `Trigger` 即可

## 什么情况线程会进入 WAITING 状态？

一个线程在等待另一个线程执行一个（唤醒）动作时，**该线程进入Waiting状态**

**进入后不能自动唤醒**，必须等待另一个线程调用`notify`方法或者`notifyAll`方法时才能够被唤醒

- 调用`Object`对象的`wait`方法，但没有指定超时值
- 调用`Thread`对象的`join`方法，但没有指定超时值
- 调用`LockSupport`对象的`park`方法

##  简述多进程开发中 join 和 daemon 的区别？

- join: 当子线程调用`join`时，**主线程会被阻塞**，当子线程结束后，主线程才能继续执行
- daemon: **当子进程被设置为守护进程时，主进程结束**，不管子进程是否执行完毕，都会随着主进程的结束而结束

## 异步和同步、阻塞和非阻塞之间的区别？

### 同步

当一个request发送出去以后，会得到一个response，这整个过程就是一个同步调用的过程

### 异步

当一个request发送出去以后，没有得到想要的response，而是通过后面的callbacks状态或者通知的方式获得结果

对于异步请求分两步：

1. 调用方发送request没有返回对应的response（可能是一个空的response）
2. 服务提供方将response处理完成以后通过callback的方式通知调用方

对于1而言是同步操作（调用方请求服务方），对于2而言也是同步操作（服务方回调调用方）

从请求的目的（调用方发送一个request，希望获得对应的response）来看，这两个步骤拆分开来没有任何意义，需要结合起来看，而这整个过程就是一次异步请求

**异步请求有一个最典型的特点：需要callback、状态或者通知的方式来告知调用方结果**

### 阻塞

阻塞调用是指调用方发出request的线程因为某种原因（如：等待系统资源）被服务方挂起, 当服务方得到response后就唤醒挂起线程，并将response返回给调用方

### 非阻塞

非阻塞调用是指调用方发出request的线程在没有等到结果时不会被挂起，并且直到得到response后才返回

**阻塞和非阻塞最大的区别就是看调用方线程是否会被挂起**

## 为什么要分内核态和用户态?

假设没有这种内核态和用户态之分，程序能随意访问硬件资源，比如说分配内存，程序能随意的读写所有的内存空间，如果程序员一不小心将不适当的内容写到了不该写的地方，就很可能导致系统崩溃

用户程序是不可信的，不管程序员是有意的还是无意的，都很容易将系统干到崩溃

正因为如此，Intel就发明了`ring0-ring3`这些访问控制级别来保护硬件资源

`ring`就是我们所说的内核级别，要想使用硬件资源就必须获取相应的权限(设置PSW寄存器，这个操作只能由操作系统设置)。操作系统对内核级别的指令进行封装，统一管理硬件资源，然后向用户程序提供系统服务，用户程序进行系统调用后，操作系统执行一系列的检查验证，确保这次调用是安全的，再进行相应的资源访问操作

**内核态能有效保护硬件资源的安全**

## Java常用集合及特点？

- Vector：底层数据结构是数组，查询快，增删慢，线程安全，效率低，默认长度为10,超过会100%延长，变成20,浪费空间
- ArrayList: 基于数组，支持随机访问，通过索引快速访问元素，可变大小，可以动态增加或删除元素，超过数组需要扩容，扩容成本较高，插入和删除操作可能需要移动其他元素，因此在大量插入和删除时效率可能较低
- LinkedList：使用链表实现，无需扩容，支持快速地插入和删除操作，尤其是在列表开头和中间，不支持随机访问，访问元素的时间复杂度为 $$O(n)$$，占用更多的内存空间，因为每个元素都需要存储指向下一个元素的引用
- HashSet：基于哈希表的实现，通过`hashcode()`和`equals()`保证元素唯一，不保证元素的顺序，元素是无序的，具有常量时间复杂度的查找、插入和删除操作
- HashMap：基于哈希表的实现，允许键值对的存储，通过键来访问值，支持常量时间复杂度的插入、删除和查找操作，不保证元素的顺序，元素是无序的
- TreeSet：基于红黑树的实现（唯一，有序），元素按照自然顺序或者指定的比较器进行排序，支持高效的插入、删除和查找操作，时间复杂度为 $$O(log n)$$
- TreeMap：基于红黑树的实现（唯一，有序），键值对按照键的自然顺序或者指定的比较器进行排序，支持高效的插入、删除和查找操作，时间复杂度为 $$O(log n)$$
- LinkedHashSet：底层数据结构是链表和哈希表（FIFO插入有序，唯一），由链表保证元素有序，由哈希表保证元素唯一
- LinkedHashMap基本特点：基于哈希表和双向链表的实现，保持插入顺序或者访问顺序（最近最少使用），插入、删除和查找操作的时间复杂度为 $$O(1)$$
- PriorityQueue：基于堆的实现，元素按照优先级进行排序，支持高效的插入和删除操作，时间复杂度为 $$O(log n)$$，获取最小（最大）元素的时间复杂度为 $$O(1)$$
- Enumeration：（枚举器，已被弃用）用于遍历集合，但已被Iterator取代

## 介绍 Spring MVC 的工作流程？

用户向服务端发送一次请求，这个请求会先到前端控制器`DispatcherServlet`

`DispatcherServlet`接收到请求后会调用`HandlerMapping`处理器映射器。由此得知，该请求该由哪个`Controller`来处理(并未调用Controller,只是得知)

`DispatcherServlet`调用`HandlerAdapter`处理器适配器，告诉处理器适配器应该要去执行哪个`ControllerHandlerAdapter`

处理器适配器去执行`Controller`并得到`ModelAndView`(数据和视图), 并层层返回给`DispatcherServlet`

`DispatcherServlet` 将 `ModelAndView` 交给 `ViewReslover` 视图解析器解析，然后返回真正的视图

`DispatcherServlet`将模型数据填充到视图中

`DispatcherServlet`将结果响应给用户

## Redis 的特点是什么？

Redis本质上是一个Key-Value类型的内存数据库，很像Memcached

> Memcached是一种开源的高性能分布式内存对象缓存系统，用于减轻数据库负载和提高Web应用程序的响应速度
> 
> 它通过将数据存储在内存中，以便快速访问，而不是直接从数据库中读取或写入，从而降低了系统的响应时间和资源消耗

整个数据库加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存

因为是纯内存操作，Redis的性能非常出色，每秒可以处理超过10万次读写操作，是已知性能最快的Key-Value DB

Redis的出色之处不仅仅是性能，Redis最大的魅力是支持保存多种数据结构，此外单个value的最大限制是1GB,不像Memcached只能保存1MB的数据

因此Redis可以用来实现很多有用的功能。比方说用他的List来做FIFO双向链表，实现一个轻量级的高性能消息队列服务，用他的Set可以做高性能的tag系统等等

另外Redis也可以对存入的Key-Value设置expire时间，因此也可以被当作一个功能加强版的Memcached来用

Redis的主要缺点是**数据库容量受到物理内存的限制**，不能用作海量数据的高性能读写

因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上

## Redis 雪崩和击穿了解吗？

### 击穿

某个KEY失效的时候，正好有大量并发请求访问这个KEY

解决办法：KEY的更新操作**添加全局互斥锁**。完全以缓存为准，使用延迟异步加载的策略(异步线程负责维护缓存的数据，定期或根据条件触发更新)，这样就不会触发更新

### 雪崩

当某一时刻发生大规模的缓存失效的情况，导致大量的请求无法获取数据，从而将流量压力传导到数据库上，导致数据库压力过大甚至宕机

一般来说，由于更新策略、或者数据热点、缓存服务宕机等原因，可能会导致缓存数据同一个时间点大规模不可用，或者都更新。所以，需要我们的更新策略要在时间上合适，数据要均匀分享，缓存服务器要多台高可用

解决办法：更新策略在时间上做到比较平均。如果数据需要同一时间失效，可以给这批数据加上一些随机值，使得这批数据不要在同一个时间过期，降低数据库的压力

使用的热数据尽量分散到不同的机器上。多台机器做主从复制或者多副本，实现高可用

做好主从的部署，当主节点挂掉后，能快速地使用从结点顶上。实现熔断限流机制，对系统进行负载能力控制

对于非核心功能的业务，拒绝其请求，只允许核心功能业务访问数据库获取数据

服务降价：提供默认返回值，或简单的提示信息

## 什么是面向对象，谈谈你的理解？

每个物体包括**动态的行为**和**静态的属性**，这些就构成了一个对象

## 访问数据库除了 JDBC 还有什么？

- `Hibernate` 是一个基于Java的对象关系映射工具，可以直接操作数据库，而不需要手动编写SQL语句
- `MyBatis` 是一个基于Java的持久层框架，也可以直接操作数据库，而不需要手动编写SQL语句
- `Spring Data JPA` 是一个基于Spring Boot的数据访问层框架，提供了简化的数据访问方式，可以通过注解来操作数据库
- `iBatis`与`MyBatis`类似，也是一款基于Java的持久层框架，可以直接操作数据库，而不需要手动编写SQL语句
