# 索引API



---

创建索引
```
$ curl -XPUT 'http://localhost:9200/twitter/tweet/1' -d '{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}'
```

_shards 碎片信息
```
total 搜索了多少碎片
successful 搜索成功数
failures 搜索失败数
```

默认情况下索引会自动创建，可以通过设置下面参数来禁止自动创建。
```
action.auto_create_index = false
#参数同时支持黑白名单，如下
action.auto_create_index to +aaa*,-bbb*,+ccc*,-* (+ meaning allowed, and - meaning disallowed)
```
同时会自动创建动态类型映射，通过下面参数禁止。
```
index.mapper.dynamic = false
```
