# Jmeter HTML 报告
---
## 配置文件
```
apache-jmeter-4.0/bin/jmeter.properties

jmeter.save.saveservice.bytes = true
jmeter.save.saveservice.label = true
jmeter.save.saveservice.latency = true
jmeter.save.saveservice.response_code = true
jmeter.save.saveservice.response_message = true
jmeter.save.saveservice.successful = true
jmeter.save.saveservice.thread_counts = true
jmeter.save.saveservice.thread_name = true
jmeter.save.saveservice.time = true
```

## WEB页面编jmx文件
```
省略。。。
```

## 执行压测命令
```shell
执行压测命令
# jmeter -n -t F:\PerformanceTest\TestCase\script\getToken.jmx -l testLogFile -e -o ./output
生成图标
# jmeter -g D:\apache-jmeter-3.0\bin\testLogFile -o ./output
```
![](/assets/WX20180927-110901.png)
