## 概述

基于API的通过写相关的监控埋点代码的方式使用Metrics对业务逻辑有一定的代码侵入，对代码的可读性会造成一些影响，所以我们提供了基于注解的使用方式。注解不仅会减少对业务代码的污染，提升可维护性，也可以提高开发效率。

## 特征及相关限制
- 类方法级别的注解
- 仅可以在java类的public方法上增加注解，不能在其他可见性的方法标注
- 支持所有的Metrics度量类型
- 支持同一个方法上标注多个不同类型的注解，但不支持标注多个相同类型的注解
- 仅支持静态tag配置，暂不支持动态tag
- 除EnableGauge外均依赖aop机制，EnableGauge注解对应的度量器需要单独注册
- 因依赖于aop机制，被注解方法在对象内部调用没有效果
- 注解仅限于在spring管控的bean下的方法上添加，非spring管控的类上添加的注解是无效的

## maven依赖

Metrics的注解实现方案，采用了annotation和springaop分离的模块化思路。

- 若是应用中直接依赖了spring，可以通过直接依赖metrics-annotation-springaop模块，maven坐标如下：
```
     <dependency>
       <groupId>com.alibaba.middleware</groupId>
       <artifactId>metrics-annotation-springaop</artifactId>
       <version>2.0.0</version>
     </dependency>
```

- 若是没有依赖spring或者spring版本有冲突，可以仅依赖metrics-annotation模块
```
    <dependency>
      <groupId>com.alibaba.middleware</groupId>
      <artifactId>metrics-annotation</artifactId>
      <version>2.0.0</version>
    </dependency>
```

然后自己去配置aop的拦截及Gauge类型度量器的注册，实现方案可以参考metrics-annotation-springaop模块中的MetricsAnnotationInterceptor类的实现逻辑。

## 注解使用举例

所有的注解使用方式相同，都需要在bean所在类的public方法上标记，以EnableGauge举例：

```
    @EnableGauge(group = "test", key = "test.item.relation-message.size", level = MetricLevel.TRIVIAL)
    public Long getScItemNeedRefreshSize() {
        try {
            Long ssize = some.ssize(needRefresh);
            return ssize;
        } catch (Throwable t) {
            logger.error("Error", t);
        }
        return -1L;
    }
```

## bean配置方式

### 注解方式

- 若是使用SpringBoot的应用，需要在启动类的SpringBootApplication注解的scanBasePackages属性中增加包路径com.alibaba.metrics.annotation，以便扫描到该路径下的配置类MetricsAnnotationInterceptor，完成aop拦截的配置及Gauge类型度量的注册。

```
@SpringBootApplication(scanBasePackages = {...,"com.alibaba.metrics.annotation"})
```
- 若是无法通过自动扫描的机制实现，可以参考MetricsAnnotationInterceptor中的实现代码自己配置aop实现拦截及Gauge类型的注册。

### xml方式

只需要添加`component-scan`标签即可。

```xml
...
  <context:component-scan base-package="com.alibaba.metrics.annotation"/>
...
```

## 注意问题

- 所有的注解都需要在bean所在类的public方法上标记，并且部分注解对方法的返回值有特定要求
- 注解所能支持的功能是Metrics相关API接口的子集，注解方式不能满足时，还需要使用api接口
- 不同方法的EnableGauge注解不能使用相同的key和tag属性，但是其他注解可以
- EnableGauge注解的方法必须是无参方法，且返回值支持int、long、float、double及对应的包装类类型
- 使用时请仔细阅读注解的注释