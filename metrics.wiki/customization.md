自定义你的metrics配置
===

Metrics支持以下几种方式进行配置：

* 通过-D参数直接配置。
* 通过`metrics-conf.properties`文件进行配置。里面是`key=value`的形式，所有可选的配置项参见下表，如果没有指定，则会采用默认值。然后在应用的启动参数中加入`-Dcom.alibaba.metrics.config_file=/path/to/metrics-conf.properties`参数。

| 配置名                                      | 默认值     | 说明                   |
| ---------------------------------------- | ------- | -------------------- |
| com.alibaba.metrics.jvm.compilation.enable | true    | 是否收集jvm编译时间指标  |
| com.alibaba.metrics.jvm.compilation.level  | TRIVIAL    | jvm编译指标收集级别            |
| com.alibaba.metrics.druid.enable | true | 是否收集Druid指标 |
| com.alibaba.metrics.druid.level | TRIVIAL | druid指标收集级别 |
| com.alibaba.metrics.druid.MaxSqlSize | 250 | druid指标收集的最大sql条数 |
| com.alibaba.metrics.cleaner.enable | true | 是否开启metrics自动清理功能 |
| com.alibaba.metrics.cleaner.keep_interval | 86400 | 指标保存的最长时间，单位为秒，如超过时间没有更新，会被自动清理掉 |
| com.alibaba.metrics.cleaner.delay | 3600 | 自动清理线程的清理间隔，单位为秒 |
| com.alibaba.metrics.reporter.file.schedule | 5    | metrics.log线程调度间隔，单位秒  |
| com.alibaba.metrics.reporter.bin.schedule  | 5    |二进制落盘线程调度间隔，单位秒            |
| com.alibaba.metrics.log.path | ~ | 日志输出路径，如果配置/aaa/bbb/，就会输出在/aaa/bbb/logs/metrics下面 |
| com.alibaba.metrics.reporter.file.enable | true    | 是否开启metrics.log日志落盘  |
| com.alibaba.metrics.reporter.file.schedule | 5    | metrics.log线程调度间隔，单位秒  |
| com.alibaba.metrics.reporter.bin.enable  | true    | 是否开启二进制落盘            |
| com.alibaba.metrics.reporter.bin.schedule  | 5    |二进制落盘线程调度间隔，单位秒            |
| com.alibaba.metrics.system.cpu.enable    | true    | 是否打开系统cpu的metrics统计  |
| com.alibaba.metrics.system.cpu.level     | MAJOR   | 默认的cpu统计级别           |
| com.alibaba.metrics.system.load.enable   | true    | 是否打开系统load的metrics统计 |
| com.alibaba.metrics.system.load.level    | MAJOR   | 默认的load统计级别          |
| com.alibaba.metrics.system.net.enable    | true    | 是否打开系统网络的metrics统计   |
| com.alibaba.metrics.system.net.level     | TRIVIAL | 默认的网络统计级别            |
| com.alibaba.metrics.system.memory.enable | true    | 是否打开系统内存的metrics统计   |
| com.alibaba.metrics.system.memory.level  | TRIVIAL | 默认的内存统计级别            |
| com.alibaba.metrics.system.tcp.enable    | true    | 是否打开系统tcp的metrics统计  |
| com.alibaba.metrics.system.tcp.level     | TRIVIAL | 默认的tcp统计级别           |
| com.alibaba.metrics.system.disk.enable   | true    | 是否打开系统磁盘的metrics统计   |
| com.alibaba.metrics.system.disk.level    | TRIVIAL | 默认的磁盘统计级别            |
| com.alibaba.metrics.jvm.mem.enable       | true    | 是否打开jvm内存统计          |
| com.alibaba.metrics.jvm.mem.level        | NORMAL  | 默认的jvm内存统计级别         |
| com.alibaba.metrics.jvm.gc.enable        | true    | 是否打开jvm gc统计         |
| com.alibaba.metrics.jvm.gc.level         | NORMAL  | 默认的jvm gc统计级别        |
| com.alibaba.metrics.jvm.class_load.enable | true    | 是否打开jvm类加载统计         |
| com.alibaba.metrics.jvm.class_load.level | NORMAL  | 默认的jvm类加载统计级别        |
| com.alibaba.metrics.jvm.buffer_pool.enable | true    | 是否打开jvm 堆外内存统计       |
| com.alibaba.metrics.jvm.buffer_pool.level | NORMAL  | 默认的jvm堆外内存统计级别       |
| com.alibaba.metrics.jvm.file_desc.enable | true    | 是否打开jvm文件句柄统计        |
| com.alibaba.metrics.jvm.file_desc.level  | NORMAL  | 默认的jvm文件句柄统计级别       |
| com.alibaba.metrics.jvm.thread_state.enable | true    | 是否打开jvm线程状态统计        |
| com.alibaba.metrics.jvm.thread_state.level | TRIVIAL | 默认的jvm线程状态统计级别       |
| com.alibaba.metrics.tomcat.http.enable   | true    | 是否打开tomcat http统计    |
| com.alibaba.metrics.tomcat.http.level    | MAJOR   | 默认的tomcat http统计级别   |
| com.alibaba.metrics.tomcat.thread.enable | true    | 是否打开tomcat 线程池统计     |
| com.alibaba.metrics.tomcat.thread.level  | NORMAL  | 默认的tomcat 线程池统计级别    |
| com.alibaba.metrics.nginx.enable         | true    | 是否打开nginx统计          |
| com.alibaba.metrics.nginx.level          | MAJOR   | 默认的nginx统计级别         |


例如，要禁用metrics.log的输出，可以配置为如下方式

```
➜  cat metrics-conf.properties
com.alibaba.metrics.reporter.file.enable=false
```

### 其他系统参数

以下参数可以通过-D参数进行配置

| 配置名                                      | 默认值     | 说明                   |
| ---------------------------------------- | ------- | -------------------- |
| com.alibaba.metrics.maxMetricCountPerRegistry | 5000    | 每个group保存的指标数，超过将不会被统计  |
| com.alibaba.metrics.maxSubCategoryCount | 20    | FastCompass中统计子类别的个数  |
| com.alibaba.metrics.numberOfBucket | 10    | BucketCounter中保留的最近N个统计间隔数  |
| com.alibaba.metrics.maxCompassErrorCodeCount | 100    | Compass中保留的错误码个数  |
| com.alibaba.metrics.maxCompassAddonCount | 10    | Compass中保留的扩展个数  |
| com.alibaba.metrics.collector.maxCollectNumber | 10000    | Collector一次收集中收集的metrics上限  |

### url统计收敛

有时候当统计url的时候如果url不规范，很容易造成维度发散，从而是的统计内存急剧膨胀。为了避免这些问题，引入了url统计收敛的规则，将发散的维度收敛到同一个指标进行统计。

| 配置名                                      | 默认值     | 说明                   |
| ---------------------------------------- | ------- | -------------------- |
| com.alibaba.metrics.host.consolidation | null    | host收敛规则  |
| com.alibaba.metrics.path.consolidation | null    | url收敛规则  |

规则为\*,\*,$的形式，$代表url按分隔符切分后，这一段作为一个统计维度；*表示这一段收敛掉。

#### host地址收敛例子

输入为
```
"puma.a.com", 
"alingphone.b.com", 
"shop.b.com", 
"nongfushanquan.a.com", 
"kinder.a.com",
"converse.a.com",
"lining.a.com",
"nelly.b.com",
"nelly.ax.b.com"
```
收敛规则为
```
com.alibaba.metrics.host.consolidation=*,$,$
```

输出为
```
*.a.com
*.b.com
*.b.com
*.a.com
*.a.com
*.a.com
*.a.com
*.b.com
*.ax.b.com
```

#### path收敛例子

输入为
```
"http://bds.ecb.a.com/play/u/2579369938/p/2/e/6/t/1/50077092567.mp4",
"http://bds.ecb.a.com/",
"http://bds.ecb.a.com/cross.xml",
"http://acs.m.a.com/gw/query/2.0/",
"http://bds.ecb.a.com/xxxx/yyyy/zzzz",
"http://localhost:8080/waterMark.do",
"http://localhost_8080/home/assets/fonts/ODelI1.woff2home",
"http://localhost:80/registry/machine"
```
收敛规则为
```
com.alibaba.metrics.path.consolidation=$,$,*
```

输出为
```
/play/u/*/*
/cross.xml/*
/gw/query/*/*
/xxxx/yyyy/*/*
/waterMark.do/*
/home/assets/*/*
/registry/machine/*
```


### 验证你的配置

#### HTTP方式

```sh
$curl -i http://localhost:8006/metrics/specific?metric=system.cpu.user
HTTP/1.1 200 OK
Access-control-allow-headers: X-Requested-With, Content-Type, X-Codingpedia
Date: Thu, 01 Dec 2016 05:53:16 GMT
Access-control-allow-methods: GET
Content-type: application/json
Access-control-allow-origin: *
Content-length: 528

{"data":[{"metric":"system.cpu.user","metricType":"GAUGE","tags":{},"timestamp":1480571596480,"value":0}],"success":true,"message":"","timestamp":1480571596480}
```

#### 日志方式

```sh
$tail -f ~/logs/metrics/metrics.log | grep system.cpu.user
{"metric":"system.cpu.user","metricType":"GAUGE","tags":{},"timestamp":1480571701453,"value":0}
```

