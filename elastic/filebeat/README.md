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
###################### Filebeat Configuration Example #########################
#=========================== Filebeat inputs =============================

filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/messages
  exclude_lines: ['^DBG']
  exclude_files: ['.gz$']
  fields:
    serverName: 'HostA'
    logType: 'message'

- type: log
  enabled: true
  paths:
    - /var/log/secure*
  exclude_lines: ['^DBG']
  exclude_files: ['.gz$']
  fields:
    serverName: 'HostA'
    logType: 'secure'
  multiline:
    pattern: "^\\s"
    match: after

- type: log
  enabled: true
  paths:
    - /var/log/nginx/*_access.log
  exclude_lines: ['^DBG']
  exclude_files: ['.gz$']
  fields:
    serverName: 'HostA'
    logType: 'nginx'
  multiline:
    pattern: '^{"date'
    negate: true
    match: after
    
- type: log
  enabled: true
  paths:
    - /var/log/db_log/mysql-error_5*
  exclude_lines: ['^DBG']
  exclude_files: ['.gz$']
  fields:
    serverName: 'HostA'
    logType: 'mysql-error'

- type: log
  enabled: true
  paths:
    - /var/log/db_log/mysql-slow*
  exclude_lines: ['^# Time:']
  fields:
    serverName: 'HostA'
    logType: 'mysql-slow'
  multiline:
    pattern: "^# User@Host:"
    negate: true
    match: after

```