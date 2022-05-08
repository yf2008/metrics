已接入指标汇总
===

* 注[1] Type目前有两种类型， `Counter`表示持续累加数据， `GAUGE`表示瞬态的数据
* 注[2] Level目前有5种， 按重要程度从高到低排列依次为`CRITICAL` > `MAJOR` > `NORMAL` > `MINOR` > `TRIVIAL`

## System

`system.cpu.*` 指标详细定义参考[这里](https://www.kernel.org/doc/Documentation/filesystems/proc.txt)

`system.mem.*` 相关指标定义参考[这里](https://access.redhat.com/solutions/406773)

|Metric | Type | Level | Tag | Description | 
| ------- | ------ | ------ |------ |------ |
| system.cpu.idle | GAUGE | MAJOR | | 最近5s的空闲cpu使用率 | 
| system.cpu.system | GAUGE | MAJOR | |  最近5s的系统cpu使用率 |  
| system.cpu.user | GAUGE | MAJOR | |  最近5s的用户cpu使用率  |
| system.cpu.iowait | GAUGE | MAJOR | |  最近5s的等待IO完成的cpu使用率  |
| system.cpu.guest | GAUGE | MAJOR | |  最近5s的运行虚拟机的cpu使用率  |
| system.cpu.irq | GAUGE | MAJOR | |  最近5s的处理硬件中断cpu使用率  |
| system.cpu.nice | GAUGE | MAJOR | |  最近5s的低优先级进程cpu使用率  |
| system.cpu.softirq | GAUGE | MAJOR | |  最近5s的处理软中断的cpu使用率  |
| system.cpu.steal | GAUGE | MAJOR | |  最近5s的steal cpu使用率[1]  |
| system.load.15min | GAUGE | MAJOR | |  当前系统最近15分钟load | 
| system.load.5min | GAUGE | MAJOR | |  当前系统最近5分钟load |
| system.load.1min | GAUGE | MAJOR | |  当前系统最近1分钟load |
| system.context_switches | GAUGE | MAJOR | |  最近5s的每秒上下文切换次数 |
| system.interrupts | GAUGE | MAJOR | |  最近5s的每秒中断次数 |
| system.process.blocked | GAUGE | MAJOR | |  系统当前被blocking的进程数 |
| system.process.running | GAUGE | MAJOR | |  系统当前运行的进程数 |
| system.disk.partition.total | GAUGE | TRIVIAL | |  当前系统的磁盘总字节数 |
| system.disk.partition.free | GAUGE | TRIVIAL | |  当前系统的磁盘空闲字节数 |
| system.disk.partition.used_ratio | GAUGE | TRIVIAL | |  当前系统的磁盘使用率 |
| system.mem.buffers | GAUGE | TRIVIAL | |  当前系统的buffer cache的内存数(单位kb) |
| system.mem.cached | GAUGE | TRIVIAL | |  当前系统的pagecache里的内存数(单位kb) |
| system.mem.free | GAUGE | TRIVIAL | |  当前系统的空闲内存(单位kb) |
| system.mem.total | GAUGE | TRIVIAL | |  当前系统的总内存(单位kb) |
| system.mem.used | GAUGE | TRIVIAL | |  当前系统的已经使用的内存(单位kb) |
| system.mem.swap.free | GAUGE | TRIVIAL | |  当前系统的swap空闲内存(单位kb) |
| system.mem.swap.total | GAUGE | TRIVIAL | |  当前系统的swap总内存(单位kb) |
| system.net.in.bytes | GAUGE | MINOR | |  最近30平均每秒网络接收到的字节数 |
| system.net.in.compressed | GAUGE | MINOR | |  最近30平均每秒网络接收到的压缩报文数 |
| system.net.in.dropped | GAUGE | MINOR | |  最近30平均每秒网络接收丢掉的报文数 |
| system.net.in.errs | GAUGE | MINOR | |  最近30平均每秒网络接收的错误数 |
| system.net.in.fifo.errs | GAUGE | MINOR | |  最近30平均每秒网络接收队列的错误数 |
| system.net.in.frame.errs | GAUGE | MINOR | |  最近30平均每秒网络frame错误数 |
| system.net.in.multicast | GAUGE | MINOR | |  最近30平均每秒网络广播的frames数 |
| system.net.in.packets | GAUGE | MINOR | |  最近30平均每秒网络接收到的报文数 |
| system.net.out.bytes | GAUGE | MINOR | |  最近30平均每秒网络发送的字节数 |
| system.net.out.carrier.errs | GAUGE | MINOR | |  最近30平均每秒carrier错误数 |
| system.net.out.collisions | GAUGE | MINOR | |  最近30平均每秒碰撞次数 |
| system.net.out.compressed | GAUGE | MINOR | |  最近30平均每秒网络发送的压缩报文数 |
| system.net.out.errs | GAUGE | MINOR | |  最近30平均每秒网络发送的错误数 |
| system.net.out.fifo.errs | GAUGE | MINOR | |  最近30平均每秒网络发送队列的错误数 |
| system.net.out.packets | GAUGE | MINOR | |  最近30平均每秒网络发送的报文数 |
| system.net.out.dropped | GAUGE | MINOR | |  最近30平均每秒网络发送丢掉的报文数 |
| system.tcp.active_opens | GAUGE | TRIVIAL | |  最近1min平均每秒主动建连的连接数 |
| system.tcp.passive_opens | GAUGE | TRIVIAL | |  最近1min平均每秒被动建连的连接数 |
| system.tcp.attempt_fails | GAUGE | TRIVIAL | |  最近1min平均每秒建连失败次数 |
| system.tcp.current_estab | GAUGE | TRIVIAL | |  最近1min平均每秒处于ESTABLISHED和CLOSE-WAIT状态的TCP连接数 |
| system.tcp.estab_resets | GAUGE | TRIVIAL | |  最近1min平均每秒reset次数 |
| system.tcp.in_errs | GAUGE | TRIVIAL | |  最近1min平均每秒收到的有问题的tcp包数量 |
| system.tcp.out_rsts | GAUGE | TRIVIAL | |  最近1min平均每秒发送的带RST标记的TCP包数量 |
| system.tcp.out_segs | GAUGE | TRIVIAL | |  最近1min平均每秒发送的tcp包数量 |
| system.tcp.retran_ratio | GAUGE | TRIVIAL | |  最近1min平均每秒重传率 |
| system.tcp.retran_segs | GAUGE | TRIVIAL | |  最近1min平均每秒重传的包数量 |

* 注[1] system.cpu.steal这个指标反映了虚拟机的资源争抢情况，如果steal高，说明虚拟机的资源被抢占的厉害

TODO:

* processors
* uptime

## JVM


#### gc

|Metric | Type | Level | Tag | Description | 
| ------- | ------- |------ |------ |------ |
| jvm.gc.xxx.count | Counter | NORMAL | | 从JVM启动开始累计的gc次数 | 
| jvm.gc.xxx.time | Counter | NORMAL | | 从JVM启动开始累计的gc时间(ms) |  
| jvm.class_load.loaded | Gauge | NORMAL | | jvm 加载类的个数  |
| jvm.class_load.unloaded | Gauge| NORMAL | | jvm 卸载类的个数 |
| jvm.file_descriptor.open_ratio | Gauge | NORMAL | | jvm文件句柄打开的百分比 | 
| jvm.buffer_pool.mapped.count | Gauge | NORMAL | | 堆外内存中mapped buffer的个数 |  
| jvm.buffer_pool.mapped.capacity | Gauge | NORMAL | | 堆外内存中mapped buffer的总大小（字节）  |
| jvm.buffer_pool.mapped.used | Gauge| NORMAL | | 堆外内存中mapped buffer已经使用的大小（字节） |
| jvm.buffer_pool.direct.count | Gauge | NORMAL | | 堆外内存中direct buffer的个数 |
| jvm.buffer_pool.direct.used | Gauge| NORMAL | | 堆外内存中direct buffer已使用的大小（字节） |
| jvm.buffer_pool.direct.capacity | Gauge | NORMAL | | 堆外内存中direct buffer的总大小（字节）  |

#### heap


|Metric | Type | Level | Tag | Description | 
| ------- | ------- |------ |------ |------ |
| jvm.mem.total.committed | Gauge | NORMAL | | jvm内存总的commit字节数 | 
| jvm.mem.total.init | Gauge | NORMAL | | jvm内存总的init字节数 |  
| jvm.mem.total.max | Gauge | NORMAL | | jvm内存最大字节数  |
| jvm.mem.total.used | Gauge| NORMAL | | jvm内存使用的字节数  |
| jvm.mem.heap.committed | Gauge | NORMAL | | 堆内存commit字节数 | 
| jvm.mem.heap.init | Gauge | NORMAL | | 堆内存init字节数 |  
| jvm.mem.heap.max | Gauge | NORMAL | | 堆内存max字节数  |
| jvm.mem.heap.used | Gauge| NORMAL | | 堆内存使用字节数  |
| jvm.mem.heap.usage | Gauge | NORMAL | | 堆内存使用率  |
| jvm.mem.non_heap.committed | Gauge | NORMAL | | 非堆内存commit字节数 | 
| jvm.mem.non_heap.init | Gauge | NORMAL | | 非堆内存init字节数 |  
| jvm.mem.non_heap.max | Gauge | NORMAL | | 非堆内存最大字节数  |
| jvm.mem.non_heap.used | Gauge| NORMAL | | 非堆内存使用字节数  |
| jvm.mem.non_heap.usage | Gauge | NORMAL | | 非堆内存使用率  |
| jvm.mem.pools.xxx\_old\_gen.committed | Gauge | NORMAL | | old区commited字节数 | 
| jvm.mem.pools.xxx\_old\_gen.init | Gauge | NORMAL | | old区init字节数 |  
| jvm.mem.pools.xxx\_old\_gen.max | Gauge | NORMAL | | old区max字节数  |
| jvm.mem.pools.xxx\_old\_gen.used | Gauge| NORMAL | | old区使用字节数  |
| jvm.mem.pools.xxx\_old\_gen.usage | Gauge | NORMAL | | old区使用率  |
| jvm.mem.pools.xxx\_old\_gen.used\_after_gc | Gauge | NORMAL | | old区上一次GC后使用的字节数  |
| jvm.mem.pools.xxx\_eden\_space.committed | Gauge | NORMAL | | eden区commit字节数 | 
| jvm.mem.pools.xxx\_eden\_space.init | Gauge | NORMAL | | eden区commit字节数 |  
| jvm.mem.pools.xxx\_eden\_space.max | Gauge | NORMAL | | eden区max字节数  |
| jvm.mem.pools.xxx\_eden\_space.used | Gauge| NORMAL | | eden区使用字节数  |
| jvm.mem.pools.xxx\_eden\_space.usage | Gauge | NORMAL | | eden区使用率  |
| jvm.mem.pools.xxx\_eden\_space.used\_after_gc | Gauge | NORMAL | | eden区上一次GC后使用的字节数  |
| jvm.mem.pools.xxx\_survivor_space.committed | Gauge | NORMAL | | survivor区commit字节数 | 
| jvm.mem.pools.xxx\_survivor_space.init | Gauge | NORMAL | | survivor区init字节数 |  
| jvm.mem.pools.xxx\_survivor_space.max | Gauge | NORMAL | | survivor区max字节数  |
| jvm.mem.pools.xxx\_survivor_space.used | Gauge| NORMAL | | survivor区使用字节数  |
| jvm.mem.pools.xxx\_survivor_space.usage | Gauge | NORMAL | | survivor区使用率  |
| jvm.mem.pools.xxx\_survivor\_space.used\_after_gc | Gauge | NORMAL | | survivor区上一次GC后使用的字节数  |
| jvm.mem.pools.metaspace.init | Gauge | NORMAL | | metaspace init字节数  |
| jvm.mem.pools.metaspace.committed | Gauge| NORMAL | | metaspace commit字节数  |
| jvm.mem.pools.metaspace.max | Gauge | NORMAL | | metaspace max字节数  |
| jvm.mem.pools.metaspace.used | Gauge | NORMAL | | metaspace 使用字节数  |
| jvm.mem.pools.metaspace.usage | Gauge | NORMAL | | metaspace 使用率  |
| jvm.mem.pools.compressed\_class_space.init | Gauge | NORMAL | | compressed\_class_space init字节数  |
| jvm.mem.pools.compressed\_class_space.committed | Gauge| NORMAL | | compressed\_class_space commit字节数  |
| jvm.mem.pools.compressed\_class_space.max | Gauge | NORMAL | | compressed\_class_space max字节数 |
| jvm.mem.pools.compressed\_class_space.used | Gauge | NORMAL | | compressed\_class_space 使用字节数  |
| jvm.mem.pools.compressed\_class_space.usage | Gauge | NORMAL | | compressed\_class_space 使用率  |
| jvm.mem.pools.code_cache.init | Gauge | NORMAL | | code cache 初始字节数  |
| jvm.mem.pools.code_cache.used | Gauge | NORMAL | | code cache 已使用字节数  |
| jvm.mem.pools.code_cache.committed | Gauge | NORMAL | | code cache 已提交字节数  |
| jvm.mem.pools.code_cache.max | Gauge | NORMAL | | code cache 最大配置字节数  |
| jvm.mem.pools.code_cache.usage | Gauge | NORMAL | | code cache 使用率  |



> 非堆内存(Non-heap memory)的数据来自于`java.lang.management.MemoryMXBean#getNonHeapMemoryUsage`，包含了`metaspace`, `compressed class space`, `code cache`三块内容，不包含Mapped Buffer和Direct Buffer。Mapped Buffer和Direct Buffer来自JMX的接口，可通过`"java.nio:type=BufferPool,name=direct"`和`"java.nio:type=BufferPool,name=mapped"`查询出来。

#### thread

|Metric | Type | Level | Tag | Description | 
| ------- | ------- |------ |------ |------ |
| jvm.thread.blocked.count | Gauge | NORMAL | | jvm阻塞线程数 | 
| jvm.thread.count | Gauge | NORMAL | | jvm线程总数 |  
| jvm.thread.deamon.count | Gauge | NORMAL | | jvm deamon线程数  |
| jvm.thread.new.count | Gauge| NORMAL | | jvm 新建线程数  |
| jvm.thread.deadlock.count | Gauge | NORMAL | | jvm内死锁的个数 | 
| jvm.thread.runnable.count | Gauge | NORMAL | | jvm处于runnable的线程个数 |  
| jvm.thread.terminated.count | Gauge | NORMAL | | jvm 终结线程数  |
| jvm.thread.timed_waiting.count | Gauge| NORMAL | | jvm处于timed_waiting状态的线程个数  |
| jvm.thread.waiting.count | Gauge | NORMAL | | jvm处于waiting状态的线程个数  |

## Tomcat

|Metric | Type | Level | Tag | Description | 
| ------- | ------ | ------ |------ |------ |
| middleware.tomcat.http.request_count | Counter[1] | NORMAL[2] | | 从tomcat启动开始的累计请求次数 | 
| middleware.tomcat.http.request.error_count | Counter| NORMAL | | 从tomcat启动开始的累计错误请求次数(返回值400以上的) |  
| middleware.tomcat.http.request.processing_time | Counter| NORMAL | | 从tomcat启动开始的累计请求处理时间  |
| middleware.tomcat.http.request.max_time | Gauge| NORMAL | | 耗时最长的请求的耗时时间  |
| middleware.tomcat.http.request.bytes_sent | Counter| NORMAL | | 从tomcat启动开始的累计发送字节数  |
| middleware.tomcat.http.request.bytes_received | Counter| NORMAL | | 从tomcat启动开始的累计接受字节数  |
| middleware.tomcat.thread.busy_count | Gauge | NORMAL | | tomcat工作线程池中忙碌的线程数 | 
| middleware.tomcat.thread.total_count | Gauge | NORMAL | | tomcat工作线程池中的线程总数 |

## Nginx

> 官方文档：https://nginx.org/en/docs/http/ngx_http_stub_status_module.html

|Metric | Type | Level | Tag | Description | 
| ------- | ------- |------ |------ |------ |
| middleware.nginx.conn.accepted | GAUGE | MAJOR | | 最近5s接受的连接数 | 
| middleware.nginx.conn.active | GAUGE | MAJOR | | 最近5s活跃的连接数(包括waiting的连接数) | 
| middleware.nginx.conn.handled | GAUGE | MAJOR | | 最近5s处理的连接数 | 
| middleware.nginx.conn.reading | GAUGE | MAJOR | | 最近5s正在读取请求header的连接数 | 
| middleware.nginx.conn.waiting | GAUGE | MAJOR | | 最近5s正在等待下一个请求的连接数 | 
| middleware.nginx.conn.writing | GAUGE | MAJOR | | 最近5s正在写响应的连接数 | 
| middleware.nginx.request.avg_rt | GAUGE | MAJOR | | 最近5s的平均rt | 
| middleware.nginx.request.qps | GAUGE | MAJOR || 最近5s的qps | 


## Druid

具体的含义请参考: https://github.com/alibaba/druid/wiki/StatFilter_Items

最大的sql语句数量默认保留250条，可以通过`com.alibaba.metrics.druid.MaxSqlSize`系统属性进行调整。

> 注意：Druid指标默认为关闭，如果sql语句过长，打开后可能会造成大量内存占用，请谨慎评估后使用！

|Metric | Type | Level | Tag | Description | 
| ------- | ------- |------ |------ |------ |
| druid.sql.ExecuteCount | GAUGE | TRIVIAL | sql | 最近1分钟内sql执行次数 | 
| druid.sql.ErrorCount | GAUGE | TRIVIAL | sql | 最近1分钟内的sql执行错误次数 | 
| druid.sql.TotalTime | GAUGE | TRIVIAL | sql | 最近1分钟内的sql执行总时间 | 
| druid.sql.EffectedRowCount | GAUGE | TRIVIAL | sql | 最近1分钟内影响行数 | 
| druid.sql.FetchRowCount | GAUGE | TRIVIAL | sql | 最近1分钟内读取行数 | 
| druid.sql.InTransactionCount | GAUGE | TRIVIAL | sql | 最近1分钟内在事务中执行的次数 | 
| druid.sql.ResultSetHoldTime | GAUGE | TRIVIAL | sql | 最近1分钟内ResultSet持有时间总和 | 
| druid.sql.ExecuteAndResultSetHoldTime | GAUGE | TRIVIAL | sql | 最近1分钟执行+ResultSet持有时间总和 | 
| druid.sql.MaxTimespan | GAUGE | TRIVIAL | sql | 最近1分钟最大执行耗时（毫秒) | 
| druid.sql.ConcurrentMax | GAUGE | TRIVIAL | sql | 最近1分钟内最大并发数 | 
| druid.sql.RunningCount | GAUGE | TRIVIAL | sql | 最近1分钟内正在执行的SQL数量 | 
| druid.sql.EffectedRowCountMax | GAUGE | TRIVIAL | sql | 最近1分钟内最大更新行数 | 
| druid.sql.FetchRowCountMax | GAUGE | TRIVIAL | sql | 最近1分钟内最大返回行数 | 


