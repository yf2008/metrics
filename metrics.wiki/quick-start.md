
## spring-boot应用

TODO, 即将开源

## Jar包依赖

### Maven依赖

```xml
<dependency>
  <groupId>com.alibaba.middleware</groupId>
  <artifactId>metrics-core-api</artifactId>
  <version>2.0.6</version>
</dependency>
<dependency>
  <groupId>com.alibaba.middleware</groupId>
  <artifactId>metrics-core-impl</artifactId>
  <version>2.0.6</version>
</dependency>
```

### Metrics命名规范

参见 [命名规范](naming-convention)

### Metric类型介绍

Metrics有一下几种基本类型， 请根据使用场景进行选择。

* Counter（计数器）
* Meter（吞吐率度量器）
* Histogram（直方分布度量器）
* Gauge(瞬态值度量器)
* Timer（吞吐率和响应时间分布度量器）
* Compass(吞吐率， 响应时间分布， 成功率和错误码度量器)
* FastCompass(高效统计吞吐率，平均rt及自定义维度的度量器)
* ClusterHistogram(集群分布度量器)

### Annotation支持

详情请访问[这里](annotation)

### Counter(计数器)

Counter主要用于统计某类指标的数量， 它将保存从系统启动开始持续的累加值。
支持对Counter进行+1, -1, +n, -n的操作。

> 例如： 可以用Counter来统计队列中剩余任务的数量

```java
Counter myCounter = MetricManager.getCounter("test", MetricName.build("myapp.mybiz.hello.queue"));

public void enter() {
    // your logic
    myCounter.inc();
}

public void exit() {
    // your logic
    myCounter.dec();
}
```

`Counter`将产生以下指标(这里省略前缀)：

| name | description|
|---|---|
|.count|累计的总次数|


### Meter（吞吐率度量器）

Meter可以用于统计某类事件发生的吞吐率， 它支持统计累计的qps， 同时也追踪1min, 5min, 15min的移动平均qps。
当你只关心qps， 却并不关心rt的时候， 可以使用`Meter`

```java
Meter methodMeter = MetricManager.getMeter("test", MetricName.build("myapp.mybiz.hello"));

public void hello() {
    // your logic
    methodMeter.mark();
}
```

Meter将产生以下指标， 这里省略前缀：

| name | description|
|---|---|
|.count|累计的总次数|
|.m1| 最近1分钟qps的移动平均|
|.m5| 最近5分钟qps的移动平均|
|.m15| 最近15分钟的qps的移动平均|
|.mean_rate| 累计总次数除以累计总时间的到的平均qps|

### Histogram（直方分布度量器）

Histogram（直方图）用于统一某类指标的分布情况， 如最大, 最小, 平均值, 标准差， 以及各种分位数， 例如90%， 95%的数据分布在某个范围内。

Histogram性能开销大，目前观察的结果是，在线上实测中发现，当要被埋点的方法QPS达到2000以上，会产生younggc时间明显延长的现象，这种情况下请谨慎使用！

> 例如， 统计用户购物车里面商品的数量， 我们并不关心每一个用户购物车里的商品具体是什么， 但是我们想了解用户购物车里面商品数量的一个分步情况， 此时， Histogram就能派上用场

```java
Histogram numOfItemInCart = MetricManager.getHistogram("test", MetricName.build("cart.my_carts.items"));

public void viewMyCart() {
    int numberOfItem = getNumberOfItemInCart();
    numOfItemInCart.update(numberOfItem);
}
```

Histogram将产生以下指标， 这里省略前缀(例子里面是cart.my_carts.items)：

| name | description|
|---|---|
| .count| 累计样本总数， 即调用update方法的次数 |
| .max| 样本的最大值 | 
| .min| 样本的最小值 | 
| .mean| 样本的平均值 |
| .stddev| 样本的标准差 |
| .median| 样本的中位数 |
| .p75| 样本的75%分位数， 75%的样本的取值范围 |
| .p95| 样本的95%分位数， 95%的样本的取值范围 |
| .p99| 样本的99%分位数， 99%的样本的取值范围 |

### Gauge（瞬态度量器）

Gauge用于测量系统内任意数据的瞬态值， 它的实现是最灵活的， 可以自由定义任意的实现， 然后只需要向系统注册一个实现即可。

> 例如， 统计某中间件中Listener的数量, 可以通过Guage的方式来实现

```java
Gauge<Integer> listenerSizeGauge = new Gauge<Integer>() {
	@Override
	public Integer getValue() {
		return defaultEnv.getAllListeners().size();
	}
};

MetricManager.register("test", 
		MetricName.build("abc.defaultEnv.listenerSize"), listenerSizeGauge);
```

不同其他几个Metric， Gauge的实现需要向MetricManager注册。

Gauge将产生以MetricName的key为名字的指标， 它的值通过调用`getValue`方法获取到。

### Timer（吞吐率和响应时间分布度量器）

Timer可以方便的统计某个业务接口的QPS，RT等数据, 相当于Meter+Histogram。

> 例如， 统计某方法创建的吞吐率和rt分布， 可以使用Timer

```java
// 获取一个Timer的实例，同样的名字只会实例化一次
Timer timer = MetricManager.getTimer("test", MetricName.build("order.create").level(MetricLevel.CRITICAL));

void doGetSth() {
    // 获取timer的上下文，并开始计时
    Timer.Context context = timer.time();

    try {
        // 执行业务逻辑
        doCreateOrder();
    } catch (Exception e1) {
        // 处理异常
    } finally {
        // 停止上下文
        // timer会自动记录当前的运行时间，调用次数，从而算出qps，rt的分布情况等信息
        context.stop();
    }
}
```

Timer将产生以下指标， 这里省略前缀：

| name | description|
|---|---|
| .count| 累计调用总次数 |
|.m1| 最近1分钟qps的移动平均|
|.m5| 最近5分钟qps的移动平均|
|.m15| 最近15分钟的qps的移动平均|
|.mean_rate| 累计总次数除以累计总时间的到的平均qps|
| .max| rt的最大值 | 
| .min| rt的最小值 | 
| .mean| rt的平均值 |
| .stddev| rt的标准差 |
| .median| rt的中位数 |
| .p75| rt的75%分位数， 75%的rt的取值范围 |
| .p95| rt的95%分位数， 95%的rt的取值范围 |
| .p99| rt的99%分位数， 99%的rt的取值范围 |


### Compass（吞吐率， 响应时间， 成功率， 错误码度量器）

Compass性能开销大，目前观察的结果是，在qps2000以上会产生younggc时间明显延长的现象，请谨慎使用！内部默认使用Histogram统计rt分布。

Compass能够方便的统计某个业务接口的吞吐率， 响应时间， 成功率， 和错误码， 它用起来比Timer更加方便。 
它的使用方法为：

> 例如： 统计查看购物车接口的吞吐率， 响应时间， 成功率， 和错误码， 可以用Compass

```java
private void doGetCompass() {

        Compass compass = MetricManager.getCompass("test", MetricName.build("carts.my_cart"));
        // 获取Compass的上下文，并开始计时
        Compass.Context context = compass.time();

        try {
            // 执行业务逻辑
            doCreateOrder();
            // 标记该次调用是成功的
            context.success();
        } catch (BizException1 e1) {
            // 标记BizException1出现了1次
            context.error("BizException1");
        } catch (BizException2 e2) {
            // 标记BizException2出现了1次
            context.error("BizException2");
        } finally {
            // 停止上下文，会自动记录当前的运行时间，和调用次数，成功次数，成功率，每个错误码的次数
            // 从而算出qps，rt的分布情况等信息，如最大rt，最小rt，平均rt，
            // 90%, 95%, 99%的rt落在哪个范围内
            context.stop();
        }
    }
```

Compass将产生以下指标， 这里省略前缀：

| name | TAG| description|
|---|---|---|
| .count| |累计调用总次数 |
| .success_count| |累计调用成功总次数 |
| .success_rate| |累计调用成功率 |
| .error.count| error=$ERROR_CODE| 某个错误码出现的累计次数| 
|.m1| |最近1分钟qps的移动平均|
|.m5| |最近5分钟qps的移动平均|
|.m15| |最近15分钟的qps的移动平均|
|.mean_rate| |累计总次数除以累计总时间的到的平均qps|
| .max| |rt的最大值 | 
| .min| |rt的最小值 | 
| .mean| |rt的平均值 |
| .stddev| |rt的标准差 |
| .median| |rt的中位数 |
| .p75| |rt的75%分位数， 75%的rt的取值范围 |
| .p95| |rt的95%分位数， 95%的rt的取值范围 |
| .p99| |rt的99%分位数， 99%的rt的取值范围 |

### FastCompass（精简的、高性能的吞吐率， 响应时间， 成功率， 命中率度量器）

FastCompass能够方便的统计某个业务接口的吞吐率， 响应时间， 成功率， 错误率，命中率， 针对高QPS的场景优化过，在数万QPS压力下不会出现性能问题。

它的使用方法为：

```java

private void doFastCompass() {            
        FastCompass cacheRead = MetricManager.getFastCompass("test", MetricName.build("cache.read"));
        if (OK) {
            //缓存命中
            cacheRead.record(rt, "hit");
        } else if (SUCCESS) {
            //访问成功
            cacheRead.record(rt, "success");
         } else {
            //访问失败
            cacheRead.record(rt, "error");
         }
}

```

FastCompass将产生以下指标， 这里省略前缀：

| name | TAG| description|
|---|---|---|
| .bucket_count| |统计周期内调用总次数 |
| .bucket_sum| |统计周期内调用总延迟 |
| .success_bucket_count| |统计周期内成功总数 |
| .error_bucket_count| | 统计周期内错误总数| 
| .hit_bucket_count| |统计周期内命中总数|
| .qps| |每秒访问量|
| .rt| |平均延迟|
| .success_rate| |成功率|
| .hit_rate| | 命中率 | 

### ClusterHistogram（集群分位数）

为了支持集群分位数统计，metrics支持把数据通过分桶方式记录下来，在集群范围内进行分位数统计。

> 分位数统计里面最准确的话，拿rt来说，就是每个请求的rt都收集上来，拿到整个集群的所有rt后进行统计，t-digest用的是这种方式，准确性比较搞，但比较耗费（客户端和服务端）内存和cpu。目前比较折中的一种办法就是，在客户端按照不同的区间进行分桶，比如rt 0-100, 100-200, .. 这样，然后客户端只统计每个区间里的数量，然后进行汇总计算，好处是比较节约内存和，但是会损失一些精度，区间分的越细，就越趋近于第一种情况。当每个桶只存放一个值之后，就退化成第一种情况了。所以如果只关心p99的话，那么可以把p99附近的区间设置的细一些，其他区间可以稍微宽一些，这样可以兼顾准确性和cpu消耗。

需要考虑传入的是一个long数组，表示如何对这个数据进行一个划分, 比如传入`{1, 10, 100}`，表示把需要统计的指标划分为4个桶，分别是[0,1), [1,10),[10,100), [100,无穷)，当调用update方法的时候，会自动更新到相应的桶里面。

使用方法：

```java
    @Test
    public void testClusterHistogramImpl() {
        ClusterHistogram ch = MetricManager.getClusterHistogram("test", MetricName.build("cluster"),
                new long[]{1, 10, 100});
        ch.update(40);
        ch.update(60);
        // ...
    }
```

集群分位数的计算方法为：

```
第一步：将n个变量值从小到大排列，X(j)表示此数列中第j个数。
第二步：计算指数，设(n+1)P%=j+g，j为整数部分，g为小数部分。
第三步：1)当g=0时：P百分位数=X(j);
2)当g≠0时：P百分位数=g*X(j+1)+(1-g)*X(j)=X(j)+g*[X(j+1)-X(j)]。
```

### TAG的使用

例如， 我们要为创建订单添加一个来源的tag， tagName是`source`， tagValue是`pc`, `mobile`等， 可以用如下的方法：

```java
// 获取一个Timer的实例，同样的名字只会实例化一次
Counter counter = MetricManager.getCounter("test", MetricName.build("order.create").tagged("source", "pc"));
```

### 查看你的metrics

接入完毕之后， 就可以启动应用， 然后通过HTTP端口查询你的metrics了。
具体请参见 [通过HTTP访问metrics](http-api)