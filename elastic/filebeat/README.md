#FileBeat
---
## 目录
.
├── data  //Offset配置文件
├── kibana //kibana模板
│   ├── 5
│   └── 6
├── logs //日志存放路径
├── module //支持日志格式化模块
│   ├── apache2
│   ├── auditd
│   ├── elasticsearch
│   ├── icinga
│   ├── iis
│   ├── kafka
│   ├── kibana
│   ├── logstash
│   ├── mongodb
│   ├── mysql
│   ├── nginx
│   ├── osquery
│   ├── postgresql
│   ├── redis
│   ├── system
│   └── traefik
└── modules.d //模块配置文件

## 配置文件
```yaml

filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/messages
  exclude_lines: ['^DBG']
  exclude_files: ['.gz$']
  fields:
    serverName: 'hostA'
    logType: 'message'

```