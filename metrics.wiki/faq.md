常见问题
===

### metrics在命名的时候， 如何选择，是放在key里面, 还是放在tag里面？

metrics的名字由两部分组成， key和tag。我们在命名的时候， 需要恰当的选择key和tag， 使得后续的查询更加方便。

**如果一个维度能够完整的对数据的全集做一个切分， 并且经常需要针对这个维度进行`group by`的操作， 那么这个维度， 就适合放到Tag里面**。 原因是在Hitsdb里面能够自动帮你做聚合和`group by`的操作。

例如， 我们想统计查看购物车调用的来源的时候， 通常会对来自pc端， 或者来自手机端的请求进行统计。此时， 通常有两种设计方式： 

1. 放在key里面， carts.my_carts.pc.count, carts.my_carts.mobile.count
2. 放在tag里面， carts.my_carts.count {source=pc}, carts.my_carts.count {source=mobile}

如果是采用1这种方式的话， 由于在时序数据库中是按照key的顺序排列的， 所以如果想统计来自pc的数量和来自mobile的数量， 则需要对一个比较大的范围进行扫描， 才能拿到这两个指标的数据。

而如果采用2这种方式的话， 那么由于key相同， 所以数据是放在一起的， 所以查询效率会比第一种方法更高。