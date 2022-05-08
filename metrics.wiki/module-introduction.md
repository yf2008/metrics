Metrics各模块介绍
===

#### metrics-common

提供metrics中的一些公共组件。

#### metrics-core-api

API 提供编程需要的必要接口， 不包括默认的实现， 但是api中自带了一套空实现， 保证即使只依赖API jar包， 不依赖实现jar包， 应用代码也不会出错。 但是空实现， 不会进行任何统计。

#### metrics-core-impl

提供metrics的核心接口的实现。

#### metrics-jvm

从jvm中获取指标， 例如， heap, non-heap, gc, thread等相关的指标。

#### metrics-os

从操作系统中获取指标， 例如load， cpu使用率等指标

#### metrics-reporter

将内存中的metrics数据汇报给各种持久化的系统， 例如日志， Hitsdb等。

具体使用方法参考[这里](tsdb-reporter)

#### metrics-rest

将内存中的metrics数据通过HTTP的形式暴露出去，这里只依赖了标准[JAX-RS](https://jax-rs-spec.java.net/)注解暴露restful服务，并不自带http server。

### metrics创建类图

![metrics-creation-class-diagram]()


### metrics创建时序图

![metrics-creation-sequence-diagram]()
