多进程支持
===

如果是纯Jar包依赖的方式，请添加`-Dcom.alibaba.metrics.http.port=xxx`并指定端口

Metrics默认的数据落盘采用的是二进制方式，暂时不支持多进程同时写入，需要切换到日志方式。

1. 按照[这里]()的方式，设置

```
com.alibaba.metrics.reporter.bin.enable=false
```

关闭二进制日志输出

2. 按照[这里]()的方式，设置
```
com.alibaba.metrics.log.path=/path/to/your/dir
```

最后日志会输出在 你要设置的目录`~/logs/metrics/metrics.log`文件里面