# DCDB监控指标说明

DCDB提供两种方式可以监控数据库性能及运行状况。一种是通过组件的端口查看组件运行的各项数据，另一种方式可以使用Prometheus+grafana构建直观的图标来监控数据。DCDB是一款基于主流思想的分布式数据库，其中包括db、meta、store三大组件。

## 端口查看

但我们成功的部署DCDB后，通过设置的ip+port就可以轻松的查看DCDB中组件的各项运行数据及环境变化的情况。以下图的部署的情况为例：

| IP           | 组件  | 端口（默认） |
| ------------ | ----- | ------------ |
| 00.000.00.00 | db    | 8888         |
| 00.000.00.00 | meta  | 8010         |
| 00.000.00.00 | store | 8110         |

Example：当我们向查看db组件的指标时，只需要在web上输入http://00.000.00.00:8888/，就会显示db的各项指标了，如下图：

![企业微信截图_0fcbdf31-d6cf-4984-9e7a-8038f33d6a19](/Users/liuyong/Library/Containers/com.tencent.WeWorkMac/Data/Library/Application Support/WXWork/Temp/ScreenCapture/企业微信截图_0fcbdf31-d6cf-4984-9e7a-8038f33d6a19.png)

由于很多指标是详细监控DCDB内部细节，这里就不一一说明指标的含义了，有兴趣可以阅读源代码来分析。

## Prometheus+grafana

使用这种方式是基于直观和横向监控的考虑出发，我们设置监控DCDB的重要的指标来监控DCDB的健康的情况和处于不同场景下DCDB的性能表现。

### db的重要指标

![企业微信截图_068b9086-3508-43b3-8fe1-494d29970429](/Users/liuyong/Library/Containers/com.tencent.WeWorkMac/Data/Library/Application Support/WXWork/Temp/ScreenCapture/企业微信截图_068b9086-3508-43b3-8fe1-494d29970429.png)

total_select_time_cost_qps(db)：db的select语句的qps总和。

 total_dml_time_const_qps(db)：db总dml语句的qps总和。

select_time_cost_qps(db)：各个db节点中select语句的qps。

dml_time_const_qps(db)：各个节db点中dml语句的qps。

![image-20200305180312052](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305180312052.png)

select_time_cost_latency(db)：各个db节点中db的select延迟。

dml_time_cost_latency(db)：各个db节点中db的dml延迟。

![image-20200305180550209](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305180550209.png)

process_io_write_bytes_second：各个db进程每秒写io的速度。

process_io_read_bytes_second：各个db进程每秒读io的速度。

![image-20200305180825404](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305180825404.png)

process_cpu_usage：各个db节点cpu使用情况。

### store的重要指标

![image-20200305181112333](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305181112333.png)

total_select_time_cost_qps(store)：store的select语句的qps总和。

total_dml_time_cost_qps(store)：store总dml语句的qps总和。

select_time_cost_qps(store)：各个store节点中select语句的qps。

dml_time_const_qps(store)：各个节store点中dml语句的qps。

![image-20200305181402264](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305181402264.png)

select_time_cost_latency(store)：各个store节点中store的select延迟。

dml_time_cost_latency(store)：各个store节点中store的dml延迟。

process_io_write_bytes_second：各个store进程每秒写io的速度。

process_io_read_bytes_second：各个store进程每秒读io的速度。

![image-20200305181544630](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305181544630.png)

process_cpu_usage：各个store节点cpu使用情况。

![image-20200305181618697](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305181618697.png)

每个节点磁盘利用率的情况。
