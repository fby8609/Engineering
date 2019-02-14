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