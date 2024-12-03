### 自我介绍

各位面试官你们好，我叫郑海飞，我本科就读于天津工业大学，网络空间安全专业，本科期间获得过三次校长奖学金，一次国家励志奖学金，硕士就读于河北工业大学人工智能专业，在此期间曾在火星先驱公司实习了五个月。

在研二的时候在天津市公路局实习了一整年，期间我的主要工作任务有两个，一个是进行数据同步工作，使用 SeaTunnel 代替 DataX 平台来进行的数据同步。另一个任务就是做可视化大屏的后台开发。

我在研究生期间的主要工作都是在做后端开发，有一些相关的实战经验，所以求职方向也是后端开发，希望能获得这次工作机会！谢谢。

> Hello, interviewers. My name is Zheng Haifei. I completed my undergraduate studies at Tiangong University, majoring in Cyberspace Security, and I pursued a master's degree in Artificial Intelligence at Hebei University of Technology. During the time, I interned at 火星先驱 Company for five months. 
>
> During my second year of graduate school, I interned at the Tianjin Highway Bureau for a full year. My main tasks were twofold: First, I was responsible for data synchronization. we primarily used the DataX and SeaTunnel platforms. The second task involved back-end development, where our team mainly developed visual dashboards for each business functions.
>
> Throughout my master's studies, I focused on back-end development, gaining experience in real projects. so, I am seeking opportunities in back-end development, and I hope to secure this position. Thank you!

### 项目中的技术应用

#### 1. 利用 Redis Geo 实现了水位告警地道附近道路的快速查找，显著提高了查询效率

当时我们的业务需求是根据地道的坐标来查找地道附近的两条车道的交通状况，用以判断是否因为地道水位上涨导致交通堵塞。因为这个功能和经纬度的坐标有关系，一开始我们是直接每次请求那个地道的时候我们就计算所有路段和它的距离，但是这样计算速度有点慢，然后我们优化的时候，选择使用redis的GEO数据结构来实现，GEO底层使用的就是 Zset 的结构存储的，它把经纬度经过它的 GEOhash 算法计算为一个得分，作为 zset 的排序依据，然后由于这个算法的特性呢，地理位置距离近的元素排序总是接近的。所以它获取某个经纬度的临近数据就很快，当时我们是先把所有路段的经纬度和路段id存储了redis中，每当请求某个地道的时候，我们就直接用REDIS的GEO Search 去找离他最近的两个路段，然后用路段 id 再去找交通量。



#### 2. 利用 Oracle 物化视图实现读写分离，通过定期刷新策略，确保物化视图数据的实时性与业务需求的平衡；

**P：**这个是在我们接手的地道项目中遇到的问题，当时的一个大屏的展示功能接口，基本是在五分钟左右才会有响应，当时是要求我们优化这个接口的响应速度。这是一个统计当天每个辖区普通公路观测站流量的接口。这个表存储的是每分钟的每个观测站的流量。

**A：**

+ **定位**：我们定位到了这个接口中的两个sql执行很慢基本在两三分钟。这两个sql都是查询的一张表，一个是设备在线的数据，一个是设备不在线的数据。我在 navicat 中执行对应的sql，发现很慢。
+ **解决：**
  + 首先想到的是优化sql语句，可以它的sql语句并不复杂，没有联表，只是有个日期的where，和聚合函数。但是这个日期字段他是个String，它使用了toDate函数将所有日期转为Date类型判断的，我没有想到什么好的方法可以处理这个String的日期字段，然后我就先尝试优化这个聚合函数，直接在代码逻辑中去处理聚合函数，我是把这个查询作为了子查询，把聚合函数提出到外层，进行尝试。第一次执行我发现就几百毫秒，很意外，我还以为优化很有效呢，但是我多尝试了几次就发现，这个sql的执行时间是不稳定的，它大多数时间还是要三分钟才能执行完毕。然后我就觉得可能主要问题是锁，可能是每分钟的定时任务插入数据的时候都上了行锁，然后sql查询的也是最新的一段时间的数据，可能导致了慢sql。
  + 然后我使用了物化视图（使用中，我发现只有对应模式的用户可以创建物化视图，即使是管理员也没有权限，即使管理员给自己赋权也不能）使插入和检索分离开，并且将String类型的日期在物化视图中改为Date类型的，且只在物化视图中保留必要的字段，大大提高了检索速度，然后也是使用定时任务来更新物化视图。

**R：**优化之后整体接口的速度基本在五百毫秒左右，响应速度大幅提升，但是相应的实时性上要比之前差一点，因为是定时更新的物化视图嘛。

> This issue arose in a tunnel project we took over. The interface for a large-screen display function took about five minutes to respond. We were tasked with optimizing the response speed of this interface.
>
> - Location problem
>
>   : We identified that SQL queries in this interface were extremely slow, taking around two to three minutes each. 
>
>   - I first considered optimizing the SQL queries. However, I noticed that the execution time was inconsistent. Most of the time, it still took around three minutes to complete. I suspected that the main issue was related to locking, possibly caused by a scheduled task inserting data every minute, which locked the table. 
>   - To address this, I used materialized views to separate the insert and select operations. Additionally, I used a scheduled task to update the materialized view.
>
> After optimization, the interface's response time was reduced to around 500 milliseconds. However, the trade-off was a slight reduction in real-time accuracy since the materialized view is updated on a schedule.



#### 3 . 利用线程池优化多表数据计算，使接口平均响应时间缩短 79%（从 12 秒降至 2.5 秒）

这也是一个接口优化的任务，这本来是一个计算型接口，需要通过四张表的数据源（MTC进，ETC进，MTC出，ETC出）来计算当日高速流量数据。

原本的处理逻辑是串行获取四张表的数据然后计算，接口响应速度太慢了，我们优化的处理手段就是使用了一个 Executors.newFixedThreadPool 直接初始化了四个线程，因为我们这个业务就四个线程，并行的去获取四张表的数据，然后通过 Future 的 get 方法获取到所有线程的结果后再计算。

#### 4. 使用不同 topic 隔离业务数据，通过线程池提高 Consumer 消费能力，缓解消息积压问题；

在业务高峰期，节假日的时候，单线程处理业务数据会出现消息堆积的问题，使用线程池提升我们的consumer的消费能力。

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
    listener:
      simple:
        concurrency: 5   # 最小消费者线程数
        max-concurrency: 10  # 最大消费者线程数
        prefetch: 1  # 一次只拉取一条消息，处理完后再拉取下一条

```

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.concurrent.Executor;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

@Configuration
public class ThreadPoolConfig {

    @Bean("consumerThreadPool")
    public Executor consumerThreadPool() {
        return Executors.newFixedThreadPool(10); // 使用固定大小的线程池
    }
}

```

```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

import java.util.concurrent.Executor;

@Service
public class RabbitMQConsumer {

    // 注入自定义线程池
    @Autowired
    private Executor consumerThreadPool;

    // 监听指定的队列，并发处理消息
    @RabbitListener(queues = "testQueue")
    public void receiveMessage(String message) {
        // 将任务提交给线程池处理
        consumerThreadPool.execute(() -> processMessage(message));
    }

    // 消息处理逻辑
    private void processMessage(String message) {
        try {
            System.out.println("Processing message: " + message);
            // 模拟消息处理的耗时操作
            Thread.sleep(1000);  // 假设每条消息处理时间为1秒
            System.out.println("Message processed: " + message);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.err.println("Message processing interrupted: " + message);
        }
    }
}

```



#### 5. 实现基于策略模式的流量计算框架，支持对出津、入津、跨域等多种场景的动态切换，降低代码耦合；

当时是因为我们的流量计算任务每次在节假日的时候都要切换算法，并且手动切换我觉得这太离谱了，但是由于每年的节假日都是当年才公布的，所以也无法提前得知，于是我们就优化这个接口，当时就是让前端负责每年吧，节假日数据记录下来，根据日期传参我们来判断是否为节假日，自动使用不同的算法，优化的时候发现这四个业务的计算输入输出基本都一样，连查数据的sql都一样，代码很冗余，于是就整体优化了一下。直接把八种策略，放进我们的静态的 contextholder的map中，根据前端传来的业务直接过去对应的算法来计算。



#### 6. 实现基于 Redis ZSet 的 IP 请求频率限制过滤器，进行高效的访问频率统计和排名，限制不同部门 IP 的访问次数；

我们使用 Redis 的 ZSet（有序集合）来存储 IP 请求的时间戳，并根据时间窗口来计算每个 IP 的访问次数。通过 ZSet 的自动排序特性，可以实时进行排名统计。

**主要步骤**

1. **请求记录**：将每个请求的时间戳添加到 Redis ZSet 中，使用 IP 作为成员值，时间戳作为分数。
2. **清理过期请求**：惰性清理（每次请求进来清理）掉超出统计时间窗口的请求记录，以便统计最新的请求频率。
3. **访问频率计算**：在每次请求到达时，统计 ZSet 中属于当前时间窗口的请求个数，如果超过限制则拒绝访问。
4. **按部门设置限制**：可以根据 IP 的所属部门在 Redis 中设置不同的限流值。

3. **Redis 数据结构设计**

- 使用 Redis ZSet，键的格式为 `rate_limit:{ip}`，每个 IP 的请求时间戳存储为 ZSet 的成员，时间戳作为分数。

```java
import redis.clients.jedis.Jedis;

public class RateLimiter {
    private static final int TIME_WINDOW = 60; // 时间窗口为60秒

    private Jedis redis;

    public RateLimiter(Jedis jedis) {
        this.redis = jedis;
    }

    public boolean isAllowed(String ip, String department, long currentTime) {
        String key = "rate_limit:" + ip;
        long windowStart = currentTime - TIME_WINDOW;

        // 删除过期请求记录
        redis.zremrangeByScore(key, 0, windowStart);

        // 获取当前时间窗口内的请求数
        long requestCount = redis.zcount(key, windowStart, currentTime);

        // 获取该部门的访问限制值
        String limitKey = "rate_limit:dept_limit:" + department;
        int limit = Integer.parseInt(redis.hget(limitKey, "limit_value"));

        if (requestCount >= limit) {
            return false; // 请求频率超出限制
        }

        // 添加当前请求的时间戳到 ZSet
        redis.zadd(key, currentTime, String.valueOf(currentTime));

        return true;
    }

    // 获取 IP 访问排名
    public void getTopIps(int topN) {
        // 查询前 N 个访问最多的 IP
        redis.zrevrangeByScore("rate_limit:{ip}", "+inf", "-inf", 0, topN).forEach(ip -> {
            System.out.println("IP: " + ip);
        });
    }
}

```



#### 7. 实际项目中的锁的使用

在活动审批的功能中呢，我们有审批通过和审批撤销的功能，我们的上一个审批通过的节点可以撤销审批，所以我们在审批通过和撤销时，都需要对其上锁，保证并发安全。



```java
// 使用 Map 存储每个活动的 ReentrantLock
private final Map<String, ReentrantLock> activityLocks = new ConcurrentHashMap<>();

// 获取活动的锁
private ReentrantLock getLock(String activityId) {
    return activityLocks.computeIfAbsent(activityId, k -> new ReentrantLock());
}

// 审批通过方法
public void approve(String activityId) {
    ReentrantLock lock = getLock(activityId);
    lock.lock(); // 阻塞直到获取到锁
    try {
        // 执行审批通过的逻辑
        System.out.println("活动 " + activityId + " 审批通过");
    } finally {
        lock.unlock(); // 确保锁释放
    }
}

// 审批回撤方法
public void rollbackApproval(String activityId) {
    ReentrantLock lock = getLock(activityId);
    lock.lock(); // 阻塞直到获取到锁
    try {
        // 执行审批回撤的逻辑
        System.out.println("活动 " + activityId + " 审批回撤");
    } finally {
        lock.unlock(); // 确保锁释放
    }
}

```











#### 1. 自己项目中的 AOP

在我们所开发的所有项目中，都是使用aop来记录系统的操作日志的，还有统计接口访问情况。主要是为了我们系统出现错误的时候快速定位，以及记录不同部门对我们服务的访问情况。

主要思路是这样的，我们是自定义了日志注解，然后使用aop中的环绕通知，对使用我们这个注解的所有方法做增强，我们通过环绕通知记录了接口响应时间、访问接口的ip、请求时间等，获取到这些参数以后，保存到数据库。



#### 2. Redis GEO

当时我们的业务需求是根据地道的坐标来查找地道附近的两条车道的交通状况，用以判断是否因为地道水位上涨导致交通堵塞。因为这个功能和经纬度的坐标有关系，一开始我们是直接每次请求那个地道的时候我们就计算所有路段和它的距离，但是这样计算速度有点慢，然后我们优化的时候，选择使用redis的GEO数据结构来实现，GEO底层使用的就是 Zset 的结构存储的，它把经纬度经过它的 GEOhash 算法计算为一个得分，作为 zset 的排序依据，然后由于这个算法的特性呢，地理位置距离近的元素排序总是接近的。所以它获取某个经纬度的临近数据就很快，当时我们是先把所有路段的经纬度和路段id存储了redis中，每当请求某个地道的时候，我们就直接用REDIS的GEO Search 去找离他最近的两个路段，然后用路段 id 再去找交通量。





#### 3 我的项目中的线程池使用

超限治理系统中：因为一超四罚（驾驶员、车辆、车辆所属企业、订单委托企业）政策的存在，所以治超系统大屏需要一个联动效果，左边展示了所有的超限车辆名单（车牌，时间，地点），点击某辆车的信息，右侧要展示 一超四罚的信息（人、车辆、车辆所属企业），所有信息都要使用大件运输许可的接口数据排除。

人和车辆的超限信息在我们单位的治超部门的数据库中，企业信息是在执法大队的数据库中，驾驶人黑名单是在我们自己的数据库中（因为这是我们自己定义的标准，不是官方的标准，当一个司机上个月超限超过三次加入黑名单），还有一个大件运输许可的接口数据。

原本的串行获取数据，再计算逻辑接口时间基本在两秒多，使用线程池和Future类来并行获取每个线程数据，接口速度优化到几百毫秒。具体实现就是定义了一个ExecutorService的Bean，使用线程池的submit方法来提交任务，然后用Future接收返回值，用Future的get方法获取结果。后面再进行逻辑处理。

##### 3.1 具体要获取那些信息呢

具体来说就是这个驾驶员 本月的的超限次数，平均超限百分比，联系方式，是否为黑名单成员；
车辆的超限次数，平均超限的百分比，车辆允许最大载重，实际承载最大重量；
车辆所属企业的，超限次数，拥有车辆数，联系人方式（企业重点关注的是超限车次占拥有车辆的百分比）；

##### 3.2 涉及哪些数据库，为什么不在一个数据库中

本接口涉及了，三个数据库，一个接口信息，我们单位治超部门的数据库，我们信息中心的数据库，执法大队的数据库，大件运输许可的接口。因为公路局的治超部门的主要任务是防止超重车辆压坏道路、桥梁这种道路基础设施，治超的执法权并不在公路局。所以我们的治超部门只有基本的超限信息，而执法大队则包含着比较全面的信息，包括物流企业的信息。使用了我们自己的数据库是因为这个黑名单的功能是我们自己设计的，不是官方标准，就用了自己的数据库存储。大件运输许可这个我也不清楚，我只知道他们那边这个数据只有接口提供。

##### 3.3 线程池的参数如何设计的

在接口中，用了线程池进行优化
主要的参数：

- 核心线程数选择的128，最大线程数选择的128（因为项目部署的服务器的64核的，然后处理的任务是获取不同数据源数据，本质是网络IO。所以选择了经验值2 * 8。）
- 阻塞队列选择的是链表阻塞队列，大小1024，这个是经验值。
- 拒绝策略选择是任务调用线程去执行。



**代码实现**

```java
		// 创建线程池
		ExecutorService executor = Executors.newFixedThreadPool(3);
		// 创建线程1
		Callable<List<Map<String, Object>>> alarmDetailsTask = () -> {
            System.out.println("Callable running: " + Thread.currentThread().getId());
            String sql = String.format("", date);
            return oracleJdbc.queryForList(sql);
        };
		// 创建线程2
        Callable<List<Map<String, Object>>> anomalousChangesTask = () -> {
            System.out.println("Callable running: " + Thread.currentThread().getId());
            String sql = String.format("",date);
            return oracleJdbc.queryForList(sql);
        };
		// 创建线程3
        Callable<Integer> maxHeightTask = () -> {
            System.out.println("Callable running: " + Thread.currentThread().getId());
            String sql = String.format("",date);
            return oracleJdbc.queryForObject(sql, Integer.class);
        };
		
		// 提交线程
		Future<List<Map<String, Object>>> alarmDetailsFuture = executor.submit(alarmDetailsTask);
        Future<List<Map<String, Object>>> anomalousChangesFuture = executor.submit(anomalousChangesTask);
        Future<Integer> maxHeightFuture = executor.submit(maxHeightTask);
		//获取线程结果
		List<Map<String, Object>> alarmDetails = alarmDetailsFuture.get();
		List<Map<String, Object>> anomalousChanges = anomalousChangesFuture.get();
        Integer maxHeight = maxHeightFuture.get();
```





#### 4 你们的数据量那么大，有没有分库分表

我们的业务时存在水平分表的，比如我们的ETC或者MTC数据，大约数据量一天在 20 到 40万之间，我们对这个数据的处理就是水平分表，因为它一个月的数据在一千万左右，我们当时是按照它的年月来分表的，一张表一个月的数据，表名中会带有年月，我们在代码中，或者数据同步中，都是用年月拼接表名，访问对应的数据。

还有一个数据就是 门架数据，这个数据量大概一天在四百万左右，这个表我们的处理方式是只保留一天的数据，每天凌晨之后首先用 oracle的 定时任务 完成对于这个表的清洗，先确保数据的正确性，因为我们的数据是接口数据，那边接口是每次只提供了五分钟的数据，并且可能会出现多提供了几十秒，或者少提供了几十秒的情况 ，可能出现重复数据。所以我们凌晨之后先oracle定时任务清洗数据，然后用springboot的定时任务，进行数据计算，包括计算每天的进津车流量，出津车流量，域内流量，每小时流量等，以提供给我们的业务功能使用。然后利用 dataX-Web 的定时任务将前一天的数据全部写到我们的hadoop的hdfs中，最后我们再删除表中前一天的所有数据。



> 服务器相关信息：
>
> + oracle数据库的硬盘空间：挂载的空间一共19T
> + hadoop集群空间： 一共十二台服务器，每个挂载空间都在5T
>
> oracle 存储信息：
>
> + 我们为每个业务都建立了独立的表空间，开启了分区机制（按天分区），并且开启了自动扩容机制
> + ETC和MTC的每月数据量：1-2GB
> + 门架数据每月数据量：15-20GB









#### 6 





### 三、面试实战

#### 3.1 柠檬微趣

##### 1. 很多个请求，每个请求用一个线程，如何同时写日志文件

首先呢，我推荐使用成熟的日志框架，比如说 Log4j 2，SLF4J，这些框架都提供了多线程下的日志处理机制。并且作为成熟的框架很多问题都考虑到了。

其次如果我们要自己实现多线程日志安全，我觉得 如果 所有线程都要写同一个日志文件，那么这注定是不能多线程并行写入的，我现在能想到的有两种解决方案：

1. 使用 synchronized 锁机制实现，我们自定义的日志类可以在方法上加上锁，这样可以确保不会有线程安全问题，但是性能很差，本身IO任务就是性能瓶颈，再加锁性能更差了。
2. 第二种方案就是使用消息队列。我们可以把一个线程的日志都封装成一个消息，写入消息队列，由一个线程专门去负责日志的磁盘写操作。

##### 2. 进程间的通信方式

1. 管道：通信的方式是**单向**的，shell 命令中的「`|`」竖线就是匿名管道
2. 消息队列：
3. 共享内存
4. 信号量
5. 信号 ：比如说，Ctrl+C 这样的信号，或者说 kill 命令这种就是信息
6. Socket

##### 3. 线程间的通信

线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。

1. 锁机制

2. wait/notify 等待

3. Volatile 内存共享

4. 信号量机制(Semaphore)

5. 信号机制(Signal)


##### 4. b+树为什么对磁盘友好

首先呢数据库的性能瓶颈就是在磁盘IO上，磁盘友好其实就是磁盘IO的次数少。B+树的结构设计上主要有两点，让他对磁盘友好：

1. B+树的非叶子节点不存储数据，只存储索引，而且一个非叶子节点可以存储多个key，这种设计使得 B+ 树的结构更加的扁平化，因为磁盘读取的单位是页，所以这种结构能让系统读取一页数据的时候，可能包含更多的非叶子节点，这样就能减少磁盘IO。
2. B+树的叶子节点之间，存在指针，按叶子节点本身关键字的大小顺序链接。这就使得我们在范围查询的时候，可以减少很多的磁盘IO次数。

##### 5. 对 https 的了解

因为 HTTTP 协议的信息是明文传输的，存在安全问题，HTTPS 的产生就是为了解决安全问题，HTTPS 在 TCP 和 HTTP 中间加入了 SSL/TSL 安全协议，使报文可以加密传输。HTTPS 在 TCP 三次握手之后，还需要进行 SSL/TLS 的握手过程，来确定加密密钥，这个过程使用非对称加密来传递对称加密的密钥，握手结束后后面的通信都是用对称加密进行，因为非对称加密效率低，耗费资源。

##### 6. https安全协议的握手过程

1. 首先客户端请求非对称加密的公钥，并且发送一些 SSL 的版本信息这类数据。
2. 服务器端返回 数字证书 还有 公钥。
3. 客户端 验证数字证书，生成 随机密钥，用于对称加密，通过非对称加密的公钥加密后传输给服务器端。
4. 服务器端 用私钥解密 对称加密的密钥，并且告知客户端后面的通信都使用对称加密。

![image-20240811104154190](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111041408.png)



![image-20240828164208001](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408281642166.png)



##### 7. TCP 如何保证可靠传输

TCP协议主要通过以下七点来保证传输可靠性：连接管理，校验和，序列号，确认应答，超时重传，流量控制，拥塞控制。

1. **连接管理**：即三次握手和四次挥手。连接管理机制能够建立起可靠的连接，这是保证传输可靠性的前提。

2. **校验和**：发送方对发送数据的二进制求和取反，然后将值填充到TCP的校验和字段中，接收方收到数据之后，以相同的方式计算校验和并进行对比。如果结果不符合预期，则将数据包丢弃。

3. **序列号**：TCP将每个字节的数据都进行了编号，这就是序列号。序列号的具体作用如下：能够保证可靠性，既能防止数据丢失，又能避免数据重复。能够保证有序性，按照序列号顺序进行数据包还原。能够提高效率，基于序列号可实现多次发送，一次确认。

4. **确认应答**：接收方接收数据之后，会回传ACK报文，报文中带有此次确认的序列号，用于告知发送方此次接收数据的情况。在指定时间后，若发送端仍未收到确认应答，就会启动超时重传。

5. **超时重传**：具体来说，超时重传主要有两种场景：数据包丢失：在指定时间后，若发送端仍未收到确认应答，就会启动超时重传，向接收端重新发送数据包。确认包丢失：当接收端收到重复数据(通过序列号进行识别)时将其丢弃，并重新回传ACK报文。

6. **流量控制**：接收端处理数据的速度是有限的，如果发送方发送数据的速度过快，就会导致接收端的缓冲区溢出，进而导致丢包……为了避免上述情况的发生，TCP支持根据接收端的处理能力，来决定发送端的发送速度。这就是流量控制。流量控制是通过在TCP报文段首部维护一个滑动窗口来实现的。

7. **拥塞控制**：拥塞控制就是当网络拥堵严重时，发送端减少数据发送。拥塞控制是通过发送端维护一个拥塞窗口来实现的。可以得出，发送端的发送速度，受限于滑动窗口和拥塞窗口中的最小值。
   拥塞控制方法分为：慢开始，拥塞避免快重传和快恢复。



##### 8. 四次挥手

**第一次挥手**：客户端发送一个 FIN（SEQ=x） 标志的数据包->服务端，用来关闭客户端到服务端的数据传送。然后客户端进入 **FIN-WAIT-1** 状态。

**第二次挥手**：服务端收到这个 FIN（SEQ=X） 标志的数据包，它发送一个 ACK （ACK=x+1）标志的数据包->客户端 。然后服务端进入 **CLOSE-WAIT** 状态，客户端进入 **FIN-WAIT-2** 状态。

**第三次挥手**：服务端发送一个 FIN (SEQ=y)标志的数据包->客户端，请求关闭连接，然后服务端进入 **LAST-ACK** 状态。

**第四次挥手**：客户端发送 ACK (ACK=y+1)标志的数据包->服务端，然后客户端进入**TIME-WAIT**状态，服务端在收到 ACK (ACK=y+1)标志的数据包后进入 CLOSE 状态。此时如果客户端等待 **2MSL** 后依然没有收到回复，就证明服务端已正常关闭，随后客户端也可以关闭连接了。

###### 8.1 为什么是四次挥手

因为结束通信时，需要双方都不再发送信息。相比于三次握手，四次挥手主要就是服务端的ACK和FIN分为两次发送，因为第一次是表示服务端知道客户端没有发送信息的需求了，但是服务端自己还可能要继续发送消息，消息发送完了，然后第二次是表示自己也没有发送信息的需求了。所以四次挥手要比三次握手多一次。

###### 8.2 TCP 四次挥手过程中，为什么需要等待 2MSL, 才进入 CLOSED 关闭状态？

为了确定 服务端 收到了客户端的最后一个 ACK，如果服务端没有收到这个 ACK，则会重发FIN，客户端等待 2MSL 时间就是为了确保这种情况下，客户端能收到 重发的FIN。

为什么是2MSL：第四次挥手到达服务器端最长时间MSL，服务器重发第三次挥手到达客户端最长需要MSL，所以客户端收到服务器端重发的第三次挥手的时间最大2MSL。

> MSL(Maximum Segment Lifetime) : 一个片段在网络中最大的存活时间，2MSL 就是一个发送和一个回复所需的最大时间。如果直到 2MSL，Client 都没有再次收到 FIN，那么 Client 推断 ACK 已经被成功接收，则结束 TCP 连接。



##### 9. 三次握手

TCP提供面向连接的服务，在传送数据前必须建立连接，TCP连接是通过三次握手建 立的。

**一次握手**:客户端发送带有 SYN（SEQ=x） 标志的数据包 -> 服务端，然后客户端进入 **SYN_SEND** 状态，等待服务端的确认；

**二次握手**:服务端发送带有 SYN+ACK(SEQ=y,ACK=x+1) 标志的数据包 –> 客户端,然后服务端进入 **SYN_RECV** 状态；

**三次握手**:客户端发送带有 ACK(ACK=y+1) 标志的数据包 –> 服务端，然后客户端和服务端都进入**ESTABLISHED** 状态，完成 TCP 三次握手。

**为什么不是两次**

服务端需要第三次握手确定客户端是否能够接收到它的消息。



##### 10. GC

**频繁 minor gc 怎么办？**

优化Minor GC频繁问题：通常情况下，由于新生代空间较小，Eden区很快被填满， 就会导致频繁Minor GC，因此可以通过增大新生代空间 -Xmn 来降低Minor GC的频 率。

频繁Full GC怎么办？

**Full GC的排查思路大概如下：** 

1. 清楚从程序角度，有哪些原因导致FGC？ 

+ 大对象 ：系统频繁的加载大对象（比如SQL查询未做分页），导致大对象频繁进入了老年代。 从而老年代空间不足，频繁 FULLGC。
+ 内存泄漏 ：频繁创建了大量对象，但是无法被回收，先引发FGC，最后导致OOM. 
+ 程序频繁生成一些长生命周期的对象 ，当这些对象的存活年龄超过分代年龄时便会进入老年代，最后引发FGC.  
+ 代码中显式调用了system.gc 方法。 
+ JVM参数设置问题：包括总内存大小、新生代和老年代的大小、Eden区和S区的 大小、元空间大小、垃圾回收算法等等。 

2. 清楚排查问题时能使用哪些工具 

+ 公司的监控系统：大部分公司都会有，可全方位监控JVM的各项指标。 
+ JDK的自带工具，包括jmap、jstat等常用命令：

```bash
# 查看堆内存各区域的使用率以及GC情况
jstat -gcutil -h20 pid 1000
# 查看堆内存中的存活对象，并按空间排序
jmap -histo pid | head -n20
# dump堆内存文件
jmap -dump:format=b,file=heap pid
```

3. 排查指南 

+ 查看监控，以了解出现问题的时间点以及当前FGC的频率（可对比正常情况看频 率是否正常） 
+ 了解JVM的参数设置，包括：堆空间各个区域的大小设置，新生代和老年代分别 采用了哪些垃圾收集器，然后分析JVM参数设置是否合理。 
+ 再对步骤1中列出的可能原因做排除法，其中内存泄漏、代码显式调用gc方法比较容易排查。 
+ 针对大对象或者长生命周期对象导致的FGC，可通过 jmap -histo 命令并结合 dump堆内存文件作进一步分析，需要先定位到可疑对象。 
+ 通过可疑对象定位到具体代码再次分析，这时候要结合GC原理和JVM参数设 置，弄清楚可疑对象是否满足了进入到老年代的条件才能下结论。

**有没有处理过内存泄漏问题？是如何定位的？**

内存泄漏是内在病源，外在病症表现可能有： 

+ 应用程序长时间连续运行时性能严重下降 
+ CPU 使用率飙升，甚至到 100% 
+ 频繁 Full GC，各种报警，例如接口超时报警等 
+ 应用程序抛出 OutOfMemoryError 错误 

严重内存泄漏往往伴随频繁的 Full GC，所以分析排查内存泄漏问题首先还得从查 看 Full GC 入手。主要有以下操作步骤： 

1. 使用 top  查看进程使用 CPU 和 MEM 的情况 
2. 使用 top -Hp [pid] 查看进程下的所有线程占 CPU 和 MEM 的情况 
3. 将线程 ID 转换为 16 进制： printf "%x\n" [pid] ，输出的值就是线程栈信息中的 nid。 例如： printf "%x\n" 29471 ，换行输出 731f。 
4. 抓取线程栈： jstack 29452 > 29452.txt ，可以多抓几次做个对比。 在线程栈信息中找到对应线程号的 16 进制值。
5. 使用 jstat -gcutil [pid] 5000 10 每隔 5 秒输出 GC 信息，输出 10 次， 查看 YGC 和 Full GC 次数。通常会出现 YGC 不增加或增加缓慢，而 Full GC 增加很快。
6. 如果发现 Full GC 次数太多，就很大概率存在内存泄漏了 
7. 使用 jmap -histo:live [pid] 输出每个类的对象数量，内存大小(字节单位) 及全限定类名。 
8. 生成 dump 文件，借助工具分析哪 个对象非常多，基本就能定位到问题在那了 使用 jmap 生成 dump 文件：

```bash
# jmap -dump:live,format=b,file=29471.dump 29471
Dumping heap to /root/dump ...
Heap dump file created
```

9. 通常使用图形化工具分析，如 JDK 自带的 jvisualvm。 或使用第三方式具分析的，如 JProfiler 也是个图形化工具，GCViewer 工具。 Eclipse 或以使用 MAT 工具查看。或使用在线分析平台 GCEasy。 注意：如果 dump 文件较大的话，分析会占比较大的内存。 
10. 在 dump 文析结果中查找存在大量的对象，再查对其的引用。 基本上就可以定位到代码层的逻辑了。



##### 链表排序

```java
// 主函数，接收链表头节点并返回排序后的链表头节点
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;  // 空链表或只有一个节点，直接返回
        }

        // 找到链表的中间节点
        ListNode mid = getMid(head);
        ListNode left = head;
        ListNode right = mid.next;
        mid.next = null;  // 将链表分成两部分

        // 递归排序左右两部分
        left = sortList(left);
        right = sortList(right);

        // 合并排序后的链表
        return merge(left, right);
    }

    // 找到链表的中间节点（快慢指针法）
    private ListNode getMid(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;  // slow 最终指向中间节点
    }

    // 合并两个有序链表
    private ListNode merge(ListNode left, ListNode right) {
        ListNode dummy = new ListNode(0);  // 临时的哨兵节点
        ListNode tail = dummy;

        while (left != null && right != null) {
            if (left.val <= right.val) {
                tail.next = left;
                left = left.next;
            } else {
                tail.next = right;
                right = right.next;
            }
            tail = tail.next;
        }

        // 将剩余的节点接到尾部
        if (left != null) {
            tail.next = left;
        } else {
            tail.next = right;
        }

        return dummy.next;  // 返回合并后的链表头节点
    }
```

##### 手写正则

```java
private boolean matchHelper(String text, String pattern, int i, int j) {
        // 如果模式串和输入串都到达结尾，匹配成功
        if (j == pattern.length()) {
            return i == text.length();
        }

        // 判断当前字符是否匹配
        boolean firstMatch = (i < text.length() &&
                              (pattern.charAt(j) == text.charAt(i) ||
                               pattern.charAt(j) == '.'));

        // 处理 '*' 和 '?'
        if (j + 1 < pattern.length() && pattern.charAt(j + 1) == '*') {
            // '*' 情况：要么跳过'*'和前一个字符，要么匹配第一个字符后继续处理
            return matchHelper(text, pattern, i, j + 2) ||
                   (firstMatch && matchHelper(text, pattern, i + 1, j));
        } else if (j + 1 < pattern.length() && pattern.charAt(j + 1) == '?') {
            // '?' 情况：要么跳过'?'和前一个字符，要么匹配第一个字符后继续处理
            return matchHelper(text, pattern, i, j + 2) ||
                   (firstMatch && matchHelper(text, pattern, i + 1, j + 2));
        } else {
            // 普通匹配或 '.' 匹配
            return firstMatch && matchHelper(text, pattern, i + 1, j + 1);
        }
    }
```



#### 3.2 美团

##### 1. 手写多线程题目: T1线程输出都是A，T2线程输出的都是B，T3线程输出的都是C要求三个线程启动后输出顺序: ABCABCABC

```java
import java.util.concurrent.Semaphore;

public class ABCThreadsSemaphoreLoop {

    // 初始化信号量，只有A的信号量初始为1，表示T1可以首先执行，其他为0，表示T2和T3需要等待
    private static final Semaphore semaphoreA = new Semaphore(1);
    private static final Semaphore semaphoreB = new Semaphore(0);
    private static final Semaphore semaphoreC = new Semaphore(0);

    public static void main(String[] args) {
        // 线程T1输出'A'
        Thread T1 = new Thread(() -> {
            for (int i = 0; i < 3; i++) {  // 循环三次
                try {
                    semaphoreA.acquire();  // 获取信号量，T1开始执行
                    System.out.print("A");
                    semaphoreB.release();  // 释放T2的信号量
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // 线程T2输出'B'
        Thread T2 = new Thread(() -> {
            for (int i = 0; i < 3; i++) {  // 循环三次
                try {
                    semaphoreB.acquire();  // 获取信号量，T2开始执行
                    System.out.print("B");
                    semaphoreC.release();  // 释放T3的信号量
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // 线程T3输出'C'
        Thread T3 = new Thread(() -> {
            for (int i = 0; i < 3; i++) {  // 循环三次
                try {
                    semaphoreC.acquire();  // 获取信号量，T3开始执行
                    System.out.print("C");
                    semaphoreA.release();  // 释放T1的信号量，让下一轮A执行
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // 启动线程
        T1.start();
        T2.start();
        T3.start();
    }
}

```

##### 2.手撕阻塞队列

使用 synchronized 实现

```java
public class BlockingQueueSync<T> {
    private final Queue<T> queue;
    private final int capacity;
    private final Object lock = new Object();  // 锁对象

    public BlockingQueueSync(int capacity) {
        this.queue = new LinkedList<>();
        this.capacity = capacity;
    }

    public void put(T element) throws InterruptedException {
        synchronized (lock) {
            while (queue.size() == capacity) {
                lock.wait();  // 在锁对象上调用 wait
            }
            queue.add(element);
            lock.notifyAll();  // 在锁对象上调用 notifyAll
        }
    }

    public T take() throws InterruptedException {
        synchronized (lock) {
            while (queue.isEmpty()) {
                lock.wait();  // 在锁对象上调用 wait
            }
            T element = queue.poll();
            lock.notifyAll();  // 在锁对象上调用 notifyAll
            return element;
        }
    }
}

```

使用 ReentrantLock 实现

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BlockingQueueReentrantLock<T> {
    private final Queue<T> queue;
    private final int capacity;
    private final Lock lock = new ReentrantLock();  // 使用ReentrantLock
    private final Condition notFull = lock.newCondition();  // 条件变量：队列非满
    private final Condition notEmpty = lock.newCondition();  // 条件变量：队列非空

    public BlockingQueueReentrantLock(int capacity) {
        this.queue = new LinkedList<>();
        this.capacity = capacity;
    }

    // put 方法
    public void put(T element) throws InterruptedException {
        lock.lock();
        try {
            // 当队列已满，等待直到有空间
            while (queue.size() == capacity) {
                notFull.await();  // 阻塞等待，直到有空间
            }

            // 添加元素到队列
            queue.add(element);
            // 通知等待的消费者线程队列不再为空
            notEmpty.signalAll();
        } finally {
            lock.unlock();  // 确保锁的释放
        }
    }

    // take 方法
    public T take() throws InterruptedException {
        lock.lock();
        try {
            // 当队列为空，等待直到有元素
            while (queue.isEmpty()) {
                notEmpty.await();  // 阻塞等待，直到有元素可取
            }

            // 移除并返回队列头部的元素
            T element = queue.poll();
            // 通知等待的生产者线程队列不再满
            notFull.signalAll();
            return element;
        } finally {
            lock.unlock();  // 确保锁的释放
        }
    }
```







#### 3.3 去哪儿

##### 1. 写一个能打满cpu的代码

```java
public static void main(String[] args) {
        while (true) {
            new Thread(() -> {
                while (true) {
                    // 无限循环，导致CPU满载
                }
            }).start();
        }
    }
```



##### 2. Jstack的输出信息

`jstack` 是一个用于生成 Java 线程的堆栈跟踪的工具，通常用于诊断 Java 应用程序的性能问题和死锁。在运行 `jstack` 后，你会得到一段关于当前 Java 进程中所有**线程的状态信息**。以下是 `jstack` 输出信息的主要组成部分：

1. **线程名称**：每个线程的名称，比如 `Thread-0`、`main` 等。

2. **线程ID**：每个线程的唯一标识符，例如 `0x00007f2a6c001800`。

3. **线程状态**：线程当前的状态，如 `RUNNABLE`、`BLOCKED`、`WAITING`、`TIMED_WAITING`、`TERMINATED` 等。

4. **堆栈帧**：显示线程在运行时的调用堆栈，包括每个方法的调用顺序和参数。例如：
   ```
   at com.example.MyClass.myMethod(MyClass.java:10)
   at com.example.MyClass.main(MyClass.java:5)
   ```

5. **锁信息**：如果线程在等待某个锁或被阻塞，输出中会包含锁的信息，如：
   
   - 所持有的锁（例如，锁的对象及其哈希码）
   - 等待的锁
   
6. **线程优先级**：线程的优先级，如 `5 (NORMAL)`。

7. **本地变量和寄存器**（可选）：某些情况下，`jstack` 可能会显示当前执行的本地变量的值。

以下是一个 `jstack` 输出的示例：

```
"main" #1 prio=5 os_prio=0 tid=0x00007f2a6c001800 nid=0x2f03 waiting for monitor entry [0x00007f2a6c4c6000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at com.example.MyClass.myMethod(MyClass.java:10)
        - waiting to lock <0x00000000c0f3f3f0> (a java.lang.Object)
        at com.example.MyClass.main(MyClass.java:5)

"Thread-0" #2 prio=5 os_prio=0 tid=0x00007f2a6c002000 nid=0x2f04 runnable [0x00007f2a6c5c7000]
   java.lang.Thread.State: RUNNABLE
        at com.example.MyClass.run(MyClass.java:15)
```

使用 `jstack` 的时候，确保你有适当的权限来访问目标 Java 进程。



#### 3.4 深信服

##### 1. leetcode155：最小栈

##### 2. [LCR 094. 分割回文串 II](https://leetcode.cn/problems/omKAoA/)

```java
public int minCut(String s) {
        int n = s.length();
        int[] dp = new int[s.length()];
        boolean[][] check = new boolean[n][n];
        for(int i = n - 1; i >= 0 ; i--){
            for(int j = i ; j < n ; j++){
                if(s.charAt(i) == s.charAt(j) && (j - i <= 1 || check[i + 1][j - 1])){
                    check[i][j] = true;
                }
            }
        }
        dp[0] = 0;
        for(int i = 0; i < n ; i++){
            if(check[0][i]){
                dp[i] = 0;
                continue;
            }
            dp[i] = i;
            for(int j = 1 ; j <= i ; j++){
                if(check[j][i]){
                    dp[i] = Math.min(dp[i],dp[j-1] + 1);
                }
            }

        }
        return dp[n-1];
    }
```

##### 3. 死锁会导致负载增加还是降低？分情况分析一下，言之有理即可。

死锁通常会导致系统负载降低，但具体影响取决于系统对死锁的检测和处理机制、死锁线程数量以及其他并发任务的负载情况。以下是分情况的分析：

1. **没有死锁检测的情况**

如果系统中没有启用死锁检测机制，死锁线程会永远等待资源释放，无法继续执行。此时：

- **系统负载通常会降低**：死锁的线程在等待资源时会处于阻塞状态，不再消耗 CPU 资源。因此，整体的 CPU 使用率会降低。
- **资源利用率下降**：死锁导致的资源无法被其他线程使用，会让一些其他线程等待或者受阻，但不会显著增加 CPU 的负载。
- **内存等资源可能会被占用**：尽管 CPU 负载降低，但内存等资源依然被占用，导致资源得不到有效释放，进而影响其他任务的执行效率。

2. **有死锁检测机制的情况**

如果系统具备死锁检测机制，且频繁检测死锁或尝试恢复死锁：

- **负载可能会增加**：死锁检测机制会消耗 CPU 资源。如果系统频繁地检查死锁，特别是在死锁检测算法复杂的情况下（如银行家算法），可能会对系统负载造成较大影响。
- **短期内可能导致 CPU 负载上升**：如果系统采取重试策略，在死锁线程反复重试获取资源时，会导致一定的 CPU 消耗，并增加系统负载。

##### 4. 60亿条32位整数数据，求某个重复出现的数字？

**方法 1：位图法（Bitmap）**

位图法适用于内存受限且数据范围已知的情况。在32位整数范围内，有 2322^{32}232 个可能值（约42亿个），刚好在一个32位整数数组中使用位图的方法就可以存储重复信息。

**步骤**：

1. 创建一个大小为 232/82^{32} / 8232/8 字节（约512MB）的位数组，称为 `bitmap`。每一位代表一个整数是否出现。
2. 遍历这60亿个整数，将相应位设置为1。如果一个数字已经对应为1，则说明该数字是重复的。
3. 记录找到的重复数字并终止遍历。

**方法二：布隆过滤器**

##### 5.Leetcode146 LRU缓存

##### 6.手写生产者消费者

阻塞队列

```java
public class MyBlockingQueue {

    private int maxSize;
    private List<Long> queue;

    public MyBlockingQueue(int maxSize) {
        this.maxSize = maxSize;
        this.queue = new ArrayList<>();
    }

    public synchronized void put() {
        while (queue.size() == maxSize) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Long value = System.currentTimeMillis();
        queue.add(value);
        System.out.println("put:" + value);
        notify();
    }

    public synchronized void take() {
        while (queue.isEmpty()) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Long value = queue.get(0);
        System.out.println("take:" + value);
        queue.remove(0);
        notify();

    }
}
```

生产者

```java
public class Producer implements Runnable {

    private MyBlockingQueue queue;

    public Producer(MyBlockingQueue queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            queue.put();
        }
    }
}
```

消费者

```java
public class Consumer implements Runnable {

    private MyBlockingQueue queue;

    public Consumer(MyBlockingQueue queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            queue.take();
        }
    }
}
```

使用

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyBlockingQueue queue = new MyBlockingQueue(100);
        Producer producer = new Producer(queue);
        Consumer consumer = new Consumer(queue);
        new Thread(producer).start();
        Thread.sleep(10);
        new Thread(consumer).start();
    }
}
```

**基于自旋锁的生产者消费者**

```java
public class ProducerConsumerWithSpinLock {
    private static int BUFFER_SIZE;
    private final ArrayList<Integer> buffer;
    private final AtomicInteger count = new AtomicInteger(0);

    public static void main(String[] args) {
        ProducerConsumerWithSpinLock pc = new ProducerConsumerWithSpinLock(5);
        Thread producerThread = new Thread(pc::produce);
        Thread consumerThread = new Thread(pc::consume);
        producerThread.start();
        consumerThread.start();
    }

    public ProducerConsumerWithSpinLock(int size) {
        this.BUFFER_SIZE = size;
        buffer = new ArrayList<>(size);
    }

    // 生产者方法
    public void produce() {
        int item = 0;
        while (true) {
            while (count.get() == BUFFER_SIZE) {
                // 缓冲区已满，自旋等待
            }
            buffer.add(item++);
            count.incrementAndGet();
            System.out.println("Produced: " + (item - 1));
        }
    }

    // 消费者方法
    public void consume() {
        while (true) {
            while (count.get() == 0) {
                // 缓冲区为空，自旋等待
            }
            int consumedItem = buffer.removeFirst();
            count.decrementAndGet();
            System.out.println("Consumed: " + consumedItem);
        }
    }
}

```



#### 3.5 用友二面

##### 1. 你了解开发环境、测试环境、生产环境是如何切换的吗？

通过 yaml 的配置属性，spring.profiles.active 来配置对应环境。

为每个环境创建不同的配置文件，通过 spring.profiles.active 来决定激活哪个环境

- `application-dev.yml`：开发环境配置
- `application-test.yml`：测试环境配置
- `application-prod.yml`：生产环境配置

##### 2. 谈谈你对spring、springboot对开发的优势，在架构方面提供了哪些便利性？

Spring和Spring Boot是Java生态系统中非常流行的框架，它们为开发提供了许多优势，并且在架构方面提供了极大的便利性。以下是它们的主要优势和在架构方面的贡献：

1. **Spring的优势**

- **松耦合与依赖注入**：Spring通过依赖注入（Dependency Injection）和控制反转（IoC）来实现松耦合，使得组件之间的依赖关系管理更加灵活。开发者不需要手动管理对象的创建和依赖关系，Spring会自动完成这些任务。
- **AOP（面向切面编程）**：Spring提供了AOP支持，可以轻松实现横切关注点，例如日志记录、事务管理、安全性等。这减少了代码的重复，并使得核心业务逻辑更加清晰。
- **丰富的生态系统**：Spring拥有庞大的生态系统，包括Spring MVC、Spring Data、Spring Security、Spring Batch等。开发者可以快速集成各种功能模块，减少开发时间。
- **事务管理**：Spring为事务管理提供了方便的抽象，使得开发者可以通过注解或XML配置实现事务控制。这在处理数据库操作时非常有用，确保了数据的完整性和一致性。

2. **Spring Boot的优势**

- **快速启动和配置**：Spring Boot极大地简化了Spring应用的配置。通过自动配置（Auto Configuration），开发者可以在无需手动配置的情况下启动一个Spring应用，减少了配置文件的复杂性。
- **嵌入式服务器**：Spring Boot集成了嵌入式Tomcat、Jetty等服务器，开发者可以将应用打包成一个独立的可执行JAR文件，无需依赖外部的应用服务器，简化了部署流程。
- **依赖管理**：Spring Boot利用“起步依赖（Starter Dependencies）”来简化依赖管理。开发者只需添加相应的起步依赖，Spring Boot会自动导入所需的库，大幅减少了配置的复杂性。
- **生产级特性**：Spring Boot内置了许多生产级特性，例如应用监控（Actuator）、健康检查、度量监控等，便于在生产环境中管理和维护应用。

##### 3. 实习的最大收获

跳出技术（后端）思维，需要更高的视野来看我们的项目。通过几个需求快速的了解，整个业务的上下游关系，搞明白我们业务部门的大致业务点。从业务赋能，当需求过来考虑技术解决方案，来确定这个需求的成本，

##### 4. 你是怎么理解线程不安全呢？为什么会发生线程不安全呢？

线程不安全是指在多线程环境下，多个线程对共享资源进行访问时，由于未能正确同步，导致数据不一致或程序出现意外行为的现象。线程不安全的问题会在高并发场景下频繁出现，影响程序的正确性和稳定性。

1. **线程不安全的原因**

线程不安全通常是由于多个线程同时访问和修改共享资源，而这些操作未加以正确同步控制导致的。其根本原因包括：

- **原子性缺失**：一个操作是不可分割的，即在操作执行过程中不能被其他线程中断。例如，`i++` 这样的操作不是原子操作，它包括读取变量、加1、再写入变量三个步骤，若不加同步控制，多个线程对同一变量同时执行该操作时，可能会导致数据错误。
- **可见性问题**：在多线程环境下，各个线程可能会缓存变量的值，这导致一个线程对变量的修改，另一个线程可能不可见。例如，某线程修改了一个共享变量的值，其他线程可能不会立刻感知到这个变化，从而读取到旧值。
- **有序性问题**：编译器、CPU 或内存可能会对代码进行重排序，以优化执行效率。然而，这可能导致程序执行的顺序与代码顺序不同，从而引发不可预测的行为。在多线程环境下，重排序可能导致其他线程观察到不同步的操作顺序。

##### 5. 你碰到过数据库中过的死锁吗？请你结合数据库的锁机制描述一个死锁的实际实例；

实例：**银行转账中的死锁**

考虑一个银行转账场景，其中两个账户 A 和 B 在同一个数据库表 `accounts` 中，事务 T1 和事务 T2 分别操作账户 A 和 B，产生了死锁。

1. **事务 T1** 先读取并锁定账户 A，计划转账一部分金额到账户 B。

   ```
   sql复制代码BEGIN TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A';
   -- 行级排他锁 X 锁在账户 A 上
   ```

2. **事务 T2** 几乎在同时开始，读取并锁定账户 B，计划转账一部分金额到账户 A。

   ```
   sql复制代码BEGIN TRANSACTION;
   UPDATE accounts SET balance = balance - 200 WHERE account_id = 'B';
   -- 行级排他锁 X 锁在账户 B 上
   ```

3. **事务 T1** 尝试锁定账户 B，以完成转账操作：

   ```
   sql复制代码UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B';
   -- T1 试图申请账户 B 的 X 锁，但账户 B 当前被 T2 锁定，因此 T1 进入等待状态
   ```

4. **事务 T2** 尝试锁定账户 A，以完成转账操作：

   ```
   sql复制代码UPDATE accounts SET balance = balance + 200 WHERE account_id = 'A';
   -- T2 试图申请账户 A 的 X 锁，但账户 A 当前被 T1 锁定，因此 T2 也进入等待状态
   ```

此时，**死锁**发生了，因为：

- T1 持有账户 A 的锁，等待 T2 释放账户 B 的锁。
- T2 持有账户 B 的锁，等待 T1 释放账户 A 的锁。
- 形成循环等待，两者都无法释放锁，所有相关的事务都陷入等待，无法继续执行。

**如何解决和避免死锁**

1. **事务中固定访问资源的顺序**：通过设计所有事务按相同的顺序访问资源，避免形成循环等待。例如，始终先锁定账户 ID 较小的，再锁定账户 ID 较大的。
2. **使用短事务，及时提交或回滚**：在代码逻辑上减少持锁时间，不让事务长时间占有锁资源。事务中应避免长时间的等待或阻塞操作。
3. **设置超时机制**：许多数据库允许为事务设置锁等待时间（如 `SET LOCK_TIMEOUT`），如果超过时间限制，会自动回滚操作并释放锁资源。
4. **数据库的死锁检测与回滚**：大部分数据库管理系统（如 MySQL、Oracle 等）都有死锁检测机制。一旦检测到死锁，会主动选择一个事务进行回滚，以释放锁资源，解除死锁。

##### 6. Linux命令，怎么杀死进程，kill -9什么意思

**使用 `kill` 命令**：

- `kill <PID>`：发送默认的 `SIGTERM`（信号 15），请求进程正常终止。进程可以选择忽略这个信号。
- `kill -9 <PID>` 或 `kill -SIGKILL <PID>`：发送 `SIGKILL` 信号，强制终止进程。此信号不能被进程捕获或忽略。



#### 3.6 途虎养车一面

##### 1. fib通项公式

![image-20241017132533551](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202410171325738.png)



#### 3.7 菜鸟一面

##### 1. 多态原理

虚拟机运行角度解释多态实现原理 **方法的动态绑定**、方法表

1. 将一个方法调用同一个方法主体关联起来被称作绑定，JAVA中分为前期绑定和后期绑定（动态绑定）
2. 在程序执行之前进行绑定（由编译器和连接程序实现）叫做前期绑定
   1. 因为在编译阶段被调用方法的直接地址就已经存储在方法所属类的常量池中了，程序执行时直接调用 (invokestatic指令) ，如，final，static，private，构造方法，成员变量（包括静态及非静态）
3. 后期绑定含义就是在程序运行时根据对象的类型信息进行绑定 实例调用 (invokevirtual)
   1. 想实现后期绑定，需要在运行时能判断对象的类型，从而找到对应的方法，即必须在对象中安置某种“类型信息”，JAVA中除了static方法、final方法（private方法属于）之外，其他的方法都是后期绑定
   2. 后期绑定会涉及到JVM管理下【每个类里都有】的一个重要的数据结构——**方法表**，方法表以数组的形式记录当前类及其所有父类的可见方法字节码在内存中的直接地址

##### 2. 索引类型

MySQL目前主要有以下几种索引类型：
1.普通索引
2.唯一索引
3.主键索引
4.组合索引
5.全文索引



#### 3.8 虾皮 一面

##### 1. 快排

```java
public static int[] quickSort(int[] nums, int left, int right){
        if(left < right){
            int pivot = partition(nums, left, right);
            quickSort(nums, left, pivot - 1);
            quickSort(nums, pivot + 1, right);
        }
        return nums;
    }

    private static int partition(int[] nums, int left, int right) {
        int k = nums[left];
        while(left < right){
            while(left < right && nums[right] >= k) right--;
            nums[left] = nums[right];
            while(left < right && nums[left] <= k) left++;
            nums[right] = nums[left];
        }
        nums[left] = k;
        return left;

    } 
```

##### 2. 进程间的通信方式

1. 管道：通信的方式是**单向**的，shell 命令中的「`|`」竖线就是匿名管道
2. 消息队列：
3. 共享内存
4. 信号量
5. 信号 ：比如说，Ctrl+C 这样的信号，或者说 kill 命令这种就是信息
6. Socket

##### 3. 死锁

**死锁怎么产生的**：

死锁是在多线程中，当多个线程相互等待对方释放资源，时产生的一种“僵局”

**死锁的排查**：

Jstack 线程堆栈分析工具，最后可以直接输出死锁。

**避免死锁：**

1.资源编号，按顺序获取

2.资源预分配

##### 4. cpu飙高

当Java后端系统中CPU飙高时，需要快速定位并缓解问题，以下是1分钟内的应急处理步骤：

1. **定位高CPU线程**：
   - 使用命令行工具（`top`）找到消耗CPU最高的Java进程，记下其PID（进程ID）。
   - 使用`top -H -p <PID>`查看该Java进程中哪个线程的CPU使用率最高，记录下高CPU线程的TID（线程ID）。
2. **将TID转换为十六进制**：
3. **打印线程堆栈信息**：
   - 使用`jstack <PID> | grep TID`，打印出堆栈信息，然后找到最上边的堆栈信息，查看对应代码，分析问题
4. **快速缓解**：
   - 如果发现是死循环或特定业务逻辑导致CPU飙高，可能需要临时禁用相关功能或接口，或者做接口的限流、熔断降级。

通过以上步骤，可以迅速定位并采取应急措施，为进一步优化或修复争取时间。

##### 5. mysql语句的执行逻辑

- 连接器：建立连接，管理连接、校验用户身份；

- 查询缓存：查询语句如果命中查询缓存则直接返回，否则继续往下执行。MySQL 8.0 已删除该模块；
- 解析 SQL，通过解析器对 SQL 查询语句进行词法分析、语法分析，然后构建语法树，方便后续模块读取表名、字段、语句类型；
- 执行 SQL：执行 SQL 共有三个阶段：
  - 预处理阶段：检查表或字段是否存在；将 `select *` 中的 `*` 符号扩展为表上的所有列。
  - 优化阶段：基于查询成本的考虑， 选择查询成本最小的执行计划；
  - 执行阶段：根据执行计划执行 SQL 查询语句，从存储引擎读取记录，返回给客户端；

##### 6. 索引失效场景

+ or语句：若是or条件中有一个没有索引，那么就不走索引。
+ 如果字段类型是字符串，where时一定用引号括起来，否则会因为隐式类型转换，索引失效 
+ 模糊查询，如果%号在前面也会导致索引失效。 
+ 联合索引，在使用的时候没有遵循最左匹配法则，导致失效；
+ 在索引列上使用mysql的内置函数或者运算，索引失效。；
+ ==还有一种比较复杂的情况：就是使用联合索引的时候，where语句有 and 的两个并列条件，第一个条件使用了范围查询，第二个条件会索引失效，但是在MySQL5.6 之后，有了索引下推，解决了这个问题，索引下推，把where里的第二个条件放到存储引擎去判断，减少回表次数。==

通常情况下，想要判断出这条sql是否有索引失效的情况，可以使用explain执行计划来分析

##### 7. mysql explain

当我们使用mysql的执行计划explain来去查看这条sql的执行情况时，我们一般重点关注几个字段：

+ **key 和 key_len 字段**：key 列表示 MySQL 实际使用到的索引。可以通过key和key_len检查是否命中了索引，如果是联合索引可以通过 key_len 判断命中了几个索引字段，也可以判断索引是否有失效的情况，
+ **type 字段**：type显示的是访问类型，主要有这几种类型：system > const > eq_ref > ref > range > index > all。阿里巴巴的java开发手册推荐，至少要达到 range 级别，要求是 ref 级别，如果可以是 const 最好。 说明： 
  + const：表示使用唯一索引扫描，并且只匹配一条数据。
  + eq_ref：唯一性索引扫描，并且也只匹配一条数据，与const不同的是eq_ref用于联表的查询。
  + ref：非唯一索引扫描，返回匹配某个单独值的所有行。
  + range：只检索给定范围的行，返回匹配指定区间的所有行。
  + index：全索引扫描，一般是因为没有where条件。
  + all：全表扫描。

+ **extra字段**：这列包含了 MySQL 解析查询的额外信息。
  + Using index：表示使用了覆盖索引，且不需要访问表的数据行
  + Using index condition：表示使用了索引条件下推
  + Using where：表明where语句中的条件无法完全使用索引过滤，MYSQL服务器层将在存储引擎层返回行以后再应用WHERE过滤条件。表示我们的sql语句依然有优化空间避免在服务器层应用where

##### 8. redis 内存淘汰策略

1. no-eviction：禁止驱逐数据，也就是说当内存不足以容纳新写⼊数据时，新写入操作会报错。  
2. volatile-ttl：从已设置过期时间的数据集中挑选最快要过期的数据淘汰 
3. volatile-random：从已设置过期时间的数据集中任意选择数据淘汰 
4. allkeys-random：从数据集中任意选择数据淘汰 
5. volatile-lru：从已设置过期时间的数据集中挑 选最近最少使用的数据淘汰 
6. allkeys-lru：在所有键中，移除最近最少使用的 key（这个是最常⽤的） 
7. volatile-lfu：从已设置过期时间的数据集中挑选最不经常使用的数据淘汰 
8. allkeys-lfu：在所有键中，移除最不经常使用的 key



#### 3.9 京东 一面

##### 1. Java运行线程的几种方式

**实现线程的方式**

1. 实现thread类
2. 继承 callable 接口
3. 继承 runnable 接口

**运行线程的方式**

1. 通过 `.start()` 运行一个线程
2. 通过 `.run()` 运行一个线程

3. 通过线程池 `.submit()` 运行一个线程

##### 实现一个接口的方式

1. 直接用 类 实现接口

2. 用匿名内部类实现接口 

```java
   Callable<String> t2 = new Callable<String>() {
               @Override
               public String call() throws Exception {
                   return "";
               }
           };
```

3. 用 lambda 表达式 实现

```java
Callable<String> t1 = () -> {
            return "";
        };
```



##### 2. 类加载过程，实际中类加载会遇到哪些问题

**类加载过程**

![image-20241203214020653](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202412032140870.png)

+ **加载**：通过类的全限定名（包名 + 类名），获取到该类的.class文件的二进制字节流，将二进制字节流所代表的静态存储结构，转化为方法区运行时的数据结构，在内存中生成一个代表该类的Java.lang.Class对象，作为方法区这个类的各种数据的访问入口

+ **连接**：验证、准备、解析 3 个阶段统称为连接。

  + **验证**：确保class文件中的字节流包含的信息，符合当前虚拟机的要求，保证这个被加载的class类的正确性，不会危害到虚拟机的安全。验证阶段大致会完成以下四个阶段的检验动作：文件格式校验、元数据验证、字节码验证、符号引用验证。
  + **准备**：为类中的静态字段分配内存，并设置默认的初始值，比如int类型初始值是0。被final修饰的static字段不会设置，因为final在编译的时候就分配了。
  + **解析**：解析阶段是虚拟机将常量池的「符号引用」直接替换为「直接引用」的过程。符号引用是以一组符号来描述所引用的目标，符号可以是任何形式的字面量，只要使用的时候可以无歧义地定位到目标即可。直接引用可以是直接指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄，直接引用是和虚拟机实现的内存布局相关的。如果有了直接引用， 那引用的目标必定已经存在在内存中了。

  > 符号引用：
  >
  > + 每个字段（包括成员变量和静态变量）在类文件中都以符号引用的方式存储。
  > + 类中定义的每个方法（包括构造方法、普通方法、静态方法等）在类文件中也以符号引用的形式存储。
  > + 对于每个类的符号引用，虚拟机需要通过查找类的符号表（如常量池）来定位它的实际内存位置。

+ **初始化**：初始化是整个类加载过程的最后一个阶段，初始化阶段简单来说就是执行类的构造器方法（() ），要注意的是这里的构造器方法()并不是开发者写的，而是编译器自动生成的。

+ **使用**：使用类或者创建对象。

+ **卸载**：如果有下面的情况，类就会被卸载：1. 该类所有的实例都已经被回收，也就是Java堆中不存在该类的任何实例。2. 加载该类的ClassLoader已经被回收。 3. 类对应的Java.lang.Class对象没有任何地方被引用，无法在任何地方通过反射访问该类的方法。



### 职业规划

1. 提升技术能力：首先对自己专业上的技术能力要有深入的掌握，我现在对一些技术点，因为没有使用场景很多只是一个了解的状态，后面要结合工作上的使用场景有更深入的了解，并且实时关注一些Java新特性的发展，累计自己的项目经验。拓展自己的技术栈，比如像mongodb，es这样经常听到的技术，但是也是没有使用场景只是跟着视频学习过，希望对这些技术栈有更深入的了解。
2. 拓展技术领域：也可以逐步的拓展自己所掌握的技术领域，比如向大数据这个领域多了解一些技术，比如 hadoop，spark，Hbase，横向拓展自己的技术栈。
3. 培养软实力：无论是沟通能力还是团队协作能力，这些在团队合作中，都是非常重要的，并且学习了解一些业务场景下的解决方案。并且提升自己的学习能力，借鉴一些前辈的学习方式，提升自己的学习效率。也可以和团队的leader学习项目的管理经验。
4. 实践与进阶：参与开源项目，了解最最先进的技术，拓展自己的视野，关注行业动态和技术趋势，了解新技术和新应用场景，为自己的职业发展做好准备。并且可以开始主键的学习了解如何带领团队、激发团队成员的潜力以及做出明智的决策。
5. 建立人脉关系：加入一些技术社区、参加技术大会和研讨会，与同行交流心得，扩展人脉圈。在社交媒体或博客平台上分享自己的技术和见解，提升自己的知名度。与行业专家互动交流，了解他们的成功经验和发展建议，为自己的职业发展提供指导。





### 反问问题

#### 技术面

1. ​	`我想了解一下咱们公司对于应届生的培养与管理时怎么样的
2. 咱们公司的晋升机制是怎么样的？多久考核一次？有没有量化指标？
3. 如果进入贵公司，我会进入那个部门，做什么样的业务，就是岗位职责是什么，技术栈是什么
4. 我们的对接人或者是客户是谁？是开发中间件还是面向用户
5. 请问您对于我今天的表现有什么建议吗？



#### HR面

1. 您能介绍一下我们部门在整个业务环节中扮演的角色吗？
2. 新员工入职的培训流程/培养机制？
3. 团队总共有多少人？
4. 公司/部门的氛围如何？工作节奏怎么样？
5. 公司的薪资结构是什么样的，公积金的缴纳比例。
6. 能不能提前进入公司实习，实习期多长能转正

