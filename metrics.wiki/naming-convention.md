Metrics命名规范
===

我们把一个metric的名字抽象为MetricName，它由两部分组成，key和tag。

### key的概念

key是一个由`.`分隔的字符串， 它描述了这个metric的基本含义， 例如`system.cpu.user`描述了用户cpu的使用率这一指标， `middleware.dubbo.provider.qps`描述了中间件Dubbo服务端总的qps这一指标。

### tag的概念

metrics的本质是在某个时间点上， 某个key的某个value的一个快照， 它是一个随时间变化的时间序列数据。 而metrics如果只考虑单机上查看这一场景的话， 其实光有key就已经够了， 因为只需要把每个时间点的数据展示出来就好了。 

那为什么会有tag呢？ 因为我们的metrics数据需要支持按应用， 单元，机房等多维度进行聚合， 所以要把这些数据存到专门的时间序列数据库（例如Opentsdb, InfluxDB）中。 为了更好的支持在时间序列数据库中进行动态聚合操作，引入了tag的概念。

tag由两部分组成， tagKey和tagValue, 形式为`{tagKey=tagValue}`。一个tag可以理解为对一组数据全集的一个完整划分， tagKey表示划分方式， tagValue表示划分后的值。 

例如交易下单的场景， 加入需要知道用户是从pc下单， 还是手机下单。 那么按下单来源划分的话， 可以把所有交易下单划分来自pc和来自手机（暂不考虑其他场景）， 所以tagKey就是source， tagValue就是pc或者mobile， 记为`{source=pc}`和`{source=mobile}`.

> 注意， tag和tag之间应当是完全正交的， 也就是没有任何的交叉关系。 

tag的存在使得时间序列数据库中的数据聚合变得非常灵活。我们把订单创建按照来源进行划分之后，当指定了`{source=*}`这一查询条件时， 就可以方便的聚合出，某个时间段内来自于pc端的创建量，和来自手机端的创建量，这一点类似于传统数据库的`group by`操作。 当不指定`source`这一tag的时候， 也能聚合出总的创建量。

### 命名规范

1. key和tag只支持：`[a-z][A-Z][0-9][-_./]`, 不能有空格, 大小写敏感, key原则上不包含大写。
2. 格式为`app_name.category[.sub_category]*`, `category`和`sub_category`里面如果有多个单词，用下划线'_'连接， 不要用'.'连接
3. 需要动态聚合的维度， 放在tag里面， 同时在tagKey也在key中体现。 不需要聚合的维度， 放在key里面。
4. 不要使用太多的tag， 一般而言4-5个已经足够

#### 举例

| key | tag |
|------|--------|
| dubbo.consumer.qps     |        |
| dubbo.consumer.service.qps     |  service=com.taobao.xxxServer      |
| order.create.count     |        |
| order.create.exception.count     | exception=IOException       |
| cart.error.count | error=EXAMPLE_ERROR | 
| inventory.order.count | source=wap_store |


### Metric的等级

构建metric的时候，可以传入一个等级，用于表征指标的重要程度, 默认是`NORMAL`。
目前有5个级别，按照重要性程度从高到低排列依次为：

```
CRITICAL > MAJOR > NORMAL > MINOR > TRIVIAL
```

为了保证数据的实时性， 请不要过多的创建CRITICAL级别的指标。

Metric等级的一个重要用途是控制落盘的频率， 默认情况下频率为：

| 等级 | 落盘频率 |
|---- | --- |
|CRTICAL| 5s|
|MAJOR| 10s|
| NORMAL| 15s|
|MINOR |30s|
|TRIVIAL |1min|