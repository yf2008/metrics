## 启动
dubbo metrics具体的使用方式可以参考`metrics-demo`模块中的`Bootstrap.java`,调用`init`方法直接启动，注册默认的metrics指标(包括JVM信息，系统信息等), 调用`destroy`方法销毁资源
## http接口  
可以通过HTTP接口查询当前的metrics，http server默认监听在8006端口。可通过编程接口，以及-D参数`com.alibaba.metrics.http.binding.host`进行配置。

### 返回当前内存中的metrics信息
`/metrics/list`
例如: http://localhost:8006/metrics/list: 
```json
{
  data: {
    jvm: [
      {
        interval: 15,
        metric: "jvm.buffer_pool.direct.capacity",
        metricLevel: "NORMAL",
        metricType: "GAUGE",
        tags: {}
      },
      {
        interval: 15,
        metric: "jvm.buffer_pool.direct.count",
        metricLevel: "NORMAL",
        metricType: "GAUGE",
        tags: {}
      }
      ...
    ],
    system: [
      {
        interval: 5,
        metric: "system.context_switches",
        metricLevel: "MAJOR",
        metricType: "GAUGE",
        tags: {}
      },
      {
        interval: 5,
        metric: "system.cpu.guest",
        metricLevel: "MAJOR",
        metricType: "GAUGE",
        tags: {}
      }
      ...
    ],
    nginx: [
      {
        interval: 5,
        metric: "middleware.nginx.conn.accepted",
        metricLevel: "MAJOR",
        metricType: "GAUGE",
        tags: {}
      },
      {
        interval: 5,
        metric: "middleware.nginx.conn.active",
        metricLevel: "MAJOR",
        metricType: "GAUGE",
        tags: {}
      }
      ...
    ],
    tomcat: [
      {
        interval: 15,
        metric: "middleware.tomcat.thread.busy_count",
        metricLevel: "NORMAL",
        metricType: "GAUGE",
        tags: {}
      },
      {
        interval: 15,
        metric: "middleware.tomcat.thread.max_pool_size",
        metricLevel: "NORMAL",
        metricType: "GAUGE",
        tags: {}
      }
    ]
  },
  success: true,
  message: "",
  timestamp: 1548057052707
}
```
### 返回某个group下的所有metrics指标
`/metrics/$group` 

例如: http://localhost:8006/metrics/jvm: 
```json
{
  data: [
    {
      interval: 15,
      metric: "jvm.buffer_pool.direct.capacity",
      metricLevel: "NORMAL",
      metricType: "GAUGE",
      tags: { },
      timestamp: 1548057958220,
      value: 32768
    },
    {
      interval: 15,
      metric: "jvm.buffer_pool.direct.count",
      metricLevel: "NORMAL",
      metricType: "GAUGE",
      tags: { },
      timestamp: 1548057958220,
      value: 4
    }
    ...
  ],
  success: true,
  message: "",
  timestamp: 1548057958228
}
```
### 返回某一个group下指定的metrics
`/metrics/$group/$metricName`

例如: http://localhost:8006/metrics/jvm/jvm.buffer_pool.direct.count
```json
{
  data: [
    {
      interval: 15,
      metric: "jvm.buffer_pool.direct.count",
      metricLevel: "NORMAL",
      metricType: "GAUGE",
      tags: { },
      timestamp: 1548058350794,
      value: 5
    }
  ],
  success: true,
  message: "",
  timestamp: 1548058350795
}
```

### 访问某一个group下指定的metrics， 按照tag进行过滤

`http://$IP:8006/metrics/$group/$metricName?tagKey=$tagKey&tagValue=$tagValue`

如果多个tag可以指定多个tagKey和tagValue


### 访问某个具体的级别的指标

`http://$IP:8006/metrics/$group/level/$level`


### 访问某个级别以上的指标

`http://$IP:8006/metrics/$group/level/$level？above=true`


### 一次获取多个group的指标

`http://$IP:8006/metrics/$group[，$group]*`

### 一次获取多个group的指标, 不同group指定不同级别

`http://$IP:8006/metrics/$group[，$group]*/level/$level[,$level]*`

### 指定获取一个或多个metrics指标（metrics 1.0.5版本以上支持）

#### GET方式

`http://$IP:8006/metrics/specific?metric=$metricName[&metric=$metricName]*`


#### POST方式

将`metric=$metric&metric=$metric`放在POST BODY里面。

### 过滤掉统计值为0的统计项

GET方式：`http://$IP:8006/metrics/specific?metric=$metricName&zeroIgnore=true`

POST方式：将zeroIgnore=true放在POST BODY里面。

zeroIgnore默认为false，当需要过滤掉统计值为0的统计项，需要置成true