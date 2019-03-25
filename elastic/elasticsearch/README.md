# Elasticsearch 
---

**创建索引**

```
curl -XPUT -H"Content-Type: application/json" 'http://127.0.0.1:9200/twitter' -d '{"settings": {"index": {"number_of_shards": 8,"number_of_replicas": 1}}}'
```

**删除索引**

```
curl -XDELETE http://127.0.0.1:9200/{index_name}
```

**开启慢日志**

```
PUT {INDEX_PAATERN}/_settings
{
    "index.indexing.slowlog.level": "INFO",
    "index.indexing.slowlog.threshold.index.warn": "10s",
    "index.indexing.slowlog.threshold.index.info": "5s",
    "index.indexing.slowlog.threshold.index.debug": "2s",
    "index.indexing.slowlog.threshold.index.trace": "500ms",
    "index.indexing.slowlog.source": "1000",
    "index.search.slowlog.level": "INFO",
    "index.search.slowlog.threshold.query.warn": "10s",
    "index.search.slowlog.threshold.query.info": "5s",
    "index.search.slowlog.threshold.query.debug": "2s",
    "index.search.slowlog.threshold.query.trace": "500ms",
    "index.search.slowlog.threshold.fetch.warn": "1s",
    "index.search.slowlog.threshold.fetch.info": "800ms",
    "index.search.slowlog.threshold.fetch.debug": "500ms",
    "index.search.slowlog.threshold.fetch.trace": "200ms"
}
```

**head插件**

```
通过Chorme浏览器安装Head插件：ElasticSearch Head
```

**数据迁移**

```
①更新elasticsearch集群配置文件并逐台重启
# vim elasticsearch.yml
# path.repo: /mnt/xxx/my_backup

②创建snapshot Repositories
curl -XPUT -H"Content-Type: application/json" 'http://127.0.0.1:9200/_snapshot/my_backup' -d '{"type": "fs", "settings": {"compress": true, "location": "/mnt/xxx/my_backup"}}'

验证：
curl http://127.0.0.1:9200/_snapshot/my_backup

③备份指定索引
curl -XPUT -H'Content-Type: application/json' 'http://127.0.0.1:9200/_snapshot/my_backup/orders_20180827_snapshot?wait_for_completion=true' -d '{"indices": "orders_20180827"}'

查看备份状态：
curl 'http://127.0.0.1:9200/_snapshot/my_backup/orders_20180827_snapshot'
curl 'http://127.0.0.1:9200/_snapshot/my_backup/_all'

④目标集群创建snapshot Respositories
curl -XPUT -H"Content-Type: application/json" 'http://127.0.0.1:9200/_snapshot/my_backup' -d '{"type": "fs", "settings": {"compress": true, "location": "/mnt/xxx/my_backup"}}'

验证：
curl http://127.0.0.1:9200/_snapshot/my_backup

⑤恢复索引：

curl -XPOST -H'Content-Type: application/json' 'http://127.0.0.1:9200/_snapshot/my_backup/orders_20180827_snapshot/_restore'
```

创建索引
```
curl -XPUT -H"Content-Type: application/json" -d '{
    "mappings":{
        "order":{
            "_all":{
                "enabled":false,
                "analyzer":"ik_max_word"
            },
            "properties":{
                "assignId":{
                    "type":"long"
                },
                "carrierId":{
                    "type":"long"
                },
                "carrierMobile":{
                    "type":"keyword"
                },
                "carrierName":{
                    "type":"text"
                },
                "carrierNo":{
                    "type":"keyword"
                },
                "channel":{
                    "type":"keyword"
                },
                "city":{
                    "type":"keyword"
                },
                "cityId":{
                    "type":"long"
                },
                "complain":{
                    "type":"boolean"
                },
                "courierRank":{
                    "type":"boolean"
                },
                "creator":{
                    "type":"keyword"
                },
                "ctime":{
                    "type":"date",
                    "format":"dateOptionalTime"
                },
                "debits":{
                    "properties":{
                        "infoId":{
                            "type":"long"
                        },
                        "toMobile":{
                            "type":"keyword"
                        },
                        "toName":{
                            "type":"text",
                            "analyzer":"ik_smart"
                        }
                    }
                },
                "delFlag":{
                    "type":"boolean"
                },
                "fromMobile":{
                    "type":"keyword"
                },
                "fromName":{
                    "type":"text",
                    "analyzer":"ik_smart"
                },
                "ftime":{
                    "type":"date",
                    "format":"dateOptionalTime"
                },
                "grabType":{
                    "type":"integer"
                },
                "gttime":{
                    "type":"date",
                    "format":"dateOptionalTime"
                },
                "orderId":{
                    "type":"long"
                },
                "orderNumber":{
                    "type":"keyword"
                },
                "orderTravelWay":{
                    "type":"integer"
                },
                "patime":{
                    "type":"date",
                    "format":"dateOptionalTime"
                },
                "placeTime":{
                    "type":"date",
                    "format":"dateOptionalTime"
                },
                "status":{
                    "type":"short"
                },
                "tags":{
                    "type":"text",
                    "analyzer":"ik_smart"
                },
                "type":{
                    "type":"integer"
                },
                "userId":{
                    "type":"long"
                },
                "userRank":{
                    "type":"boolean"
                }
            }
        }
    },
    "settings":{
        "index":{
            "refresh_interval":"100ms",
            "number_of_shards":"12",
            "number_of_replicas":"1"
        }
    }
}' 'http://127.0.0.1:9200/orders_20180827'
```