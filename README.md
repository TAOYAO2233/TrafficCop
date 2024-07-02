# 一键脚本
正常一键脚本，刚更新的代码可能会有延迟：
```
curl -fsSL https://raw.githubusercontent.com/ypq123456789/TrafficCop/main/trafficcop.sh -o /root/traffic_monitor.sh && chmod +x /root/traffic_monitor.sh && bash /root/traffic_monitor.sh
```
快速更新版本：
```
curl -H "Accept: application/vnd.github.v3.raw" -fsSL "https://api.github.com/repos/ypq123456789/TrafficCop/contents/trafficcop.sh" | tr -d '\r' > /root/traffic_monitor.sh && chmod +x /root/traffic_monitor.sh && bash /root/traffic_monitor.sh
```
查看日志：
```
tail -f -n 30 /root/traffic_monitor.log
```
查看当前配置：
```
cat traffic_monitor_config.txt
```
杀死所有traffic_monitor进程，万一脚本出bug导致cpu爆满时使用：
```
pkill -f traffic_monitor.sh
```
# 脚本逻辑
首先，这个脚本会判断当前主要使用的网卡名称是什么，选择主要网卡进行流量限制。

其次，这个脚本会要求用户输入限制流量统计的模式，包括四种，第一种是只计算出站流量，第二种是只计算进站流量，第三种是出进站流量都计算，第四种是出站和进站流量只取大。

然后，这个脚本会要求用户输入流量计算周期（默认为月，允许输入季度、年），以及流量周期计算的起始日期。

然后，这个脚本会要求用户输入要限制的流量大小，然后再输入容错范围，后台计算限制流量为要限制的流量大小减去容错范围，单位均为GB。

最后，这个脚本会每隔1分钟检测当前的流量消耗，如果达到了限制值，那么就会使用 tc (Traffic Control) 来限制带宽。

并且，这个脚本会在下一个流量周期到达时，自动解除限制。
# 脚本特色
- ▪️四种模式非常全面，覆盖了几乎市面上所有vps的流量计费模式。
- ▪️允许用户自定义流量计算周期和流量周期计算起始日。
- ▪️允许用户自定义流量容错范围。
- ▪️每一个要求用户输入的参数，脚本每次运行都会读取这些参数，并且会询问用户是否需要更改。
- ▪️在脚本运行的最开始提示用户当前流量统计结果。
- ▪️**使用 tc (Traffic Control) 来限制带宽，而不是完全阻断流量。这样可以确保 SSH 连接始终可用。**
- ▪️允许自定义设置限制带宽（默认为 20 kbit/s）。
