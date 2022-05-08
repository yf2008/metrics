HTTP查询接口
===

## 查询当前内存中数据

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


## 指定时间范围查询

metrics同时支持指定时间范围，查询存储在磁盘上的数据。

### 请求地址

```
http://127.0.0.1:8006/metrics/search
```

### 请求格式

HTTP POST请求，json格式的请求放在POST BODY里面

主要字段及含义：

| 字段名 | 含义 | 类型 | 
| --- | --- | --- |
| precision  | 查询间隔，将返回统计间隔小于等于这个值的数据，如果查询秒级的统计数据，分钟级的数据就不会被返回，单位秒。默认无限制 | int | 
| limit | 最多返回多少条metrics记录。默认无限制 | int | 
| source | CURRENT：查询保留的实时数据，ARCHIVE：查询归档压缩后的数据。默认查询实时 | 枚举类型MetricSource，（CURRENT,ARCHIVE） | 

#### 示例
```
{
    "startTime": 1495455700000,
	"endTime": 1495468200000,
	"precision": 1,
	"limit": 1000,
    "queries": [
        {
            "key": "metric.name.count",
            "tags": {
                "tag1": "value1"
            }
        },
        {
            "key": "metric.name.count.delta",
            "tags": {
                "tag1": "value1"
            }
        },
        {
            "key": "metric.name.count.error",
            "tags": {
                "tag1": "value1"
            }
        },
        {
            "key": "metric.name.count.m1",
            "tags": {
                "tag1": "value1"
            }
        }
    ]
}
```
### 响应格式

主要字段及含义：

| 字段名 | 含义 | 类型 | 
| --- | --- | --- |
| precision  | 这个metrics的统计精度 | int | 
| msg(新增字段) | 当此次请求出现异常的时候，这里会有一些辅助说明 | String | 
| valueStatus | 标识着这个metrics的值的情况。EXIST代表这个点有取值；NAN代表这个点没有取值；INVALID_PRECISION代表这个字段在这个时间点上存储的时间精度比查询的时间精度粗 | 枚举类型ValueStatus(EXIST,NAN,INVALID_PRECISION) | 
| dataStatus | COLLECTED：表示收集完成，UNFINISHED：表示未完成，此时也会返回已有的数据 | 枚举类型DataStatus（COLLECTED, UNFINISHED） |  
| recordStatus | LIMITED：表示返回的结果并不是全量的结果；ENTIRE：返回全量结果 | 枚举类型RecordStatus（LIMITED，ENTIRE） | 
| metricType(新增字段) | 数据类型 | 枚举类型MetricType(GUAGE，COUNT，DELTA) |
| meterName(新增字段) | 度量器名称，采集端根据这个名字预设场景方便用户选择 | String，目前有Gauge, Counter, Histogram, Meter, Timer, Compass, FastCompass几种度量器 | 

#### 示例
```
{
    "dataStatus": "COLLECTED",
    "endTime": 1505184127233,
    "recordStatus": "ENTIRE",
    "result": [
        {
            "meterName": "Gauge",
            "metricName": "system.cpu.user",
            "metricType": "GAUGE",
            "precision": 5,
            "tags": {},
            "timestamp": 1505184120000,
            "value": 0,
            "valueStatus": "EXIST"
        },
        {
            "meterName": "Gauge",
            "metricName": "system.cpu.user",
            "metricType": "GAUGE",
            "precision": 5,
            "tags": {},
            "timestamp": 1505184125000,
            "value": 0,
            "valueStatus": "EXIST"
        }
    ],
    "startTime": 1505184117233,
    "msg": "success"
}
```

curl 命令行访问实例：

```
curl -v -X POST -H "Content-Type: application/json" --data '
{
    "startTime": 1547106884487,
    "endTime": 1547106904487,
    "precision": 15,
    "limit": 1000,
    "queries": [
        {
            "key": "gateway.client.qps.count",
            "tags": {}
        }
    ]
}' http://localhost:8006/metrics/search
```