# API 规范



---


Multiple Indices  
支持同时搜索多个索引，可以用 , 作为间隔符指定多个索引，也可以用 _all 搜索所有索引

同时也支持通配符 * ，以及 + 和 - 
如 +test*,-test3

所有的多个索引都支持下面的参数
ignore_unavailable 
是否忽略无效的索引，值true/false
allow_no_indices
是否允许不存在的索引，值true/false
expand_wildcards
扩大通配符范围，open的话只针对open的索引，closed针对closed的索引，none都不允许，值open/closed/open,closed/none/all（等价open,closed）


Date math
时间匹配
基本格式如下
```
<static_name{date_math_expr{date_format|time_zone}}>
```
```
<logstash-{now/d}> logstash-2024.03.22
<logstash-{now/M}> logstash-2024.03.01 
<logstash-{now/M{YYYY.MM}}> logstash-2024.03 
<logstash-{now/M-1M{YYYY.MM}}> logstash-2024.02 
<logstash-{now/d{YYYY.MM.dd|+12:00}}> logstash-2024.03.23
```

{ 和 } 符号需要转义符\来实现
```
<elastic\\{ON\\}-{now/M}>  elastic{ON}-2024.03.01
```


通用参数
下面的适用于所有的rest接口

```
pretty=true输出美化后的内容，只用于开发阶段
format=yaml输出yaml格式的内容
human=true输出适合人类阅读的内容，默认false
```

时间匹配
可以用now；
以||结尾的时间字符串；
```
+1h 加1小时；
-1d 加1天；
/d 取最近的天
```
```
y (year), M (month), w (week), d (day), h (hour), m (minute),  s (second)
```
输出过滤
filter_path 参数里多个用 , 分隔，支持 * 和 ** 通配符
支持 _source  过滤
例如：
```
curl -XGET 'localhost:9200/_search?pretty&filter_path=hits.hits._source&_source=title'
{
  "hits" : {
    "hits" : [ {
      "_source":{"title":"Book #2"}
    }, {
      "_source":{"title":"Book #1"}
    }, {
      "_source":{"title":"Book #3"}
    } ]
  }
}
```

扁平参数
flat_settings 
设置为true返回类似下面
```
{
  "persistent" : { },
  "transient" : {
    "discovery.zen.minimum_master_nodes" : "1"
  }
}
```
false的话返回类似下面
```
{
  "persistent" : { },
  "transient" : {
    "discovery" : {
      "zen" : {
        "minimum_master_nodes" : "1"
      }
    }
  }
}
```
rest参数都使用下划线

布尔值 false, 0, no 和 off 作为 false，其他值都是true

时间单位
```
y Year 
M Month 
w Week 
d Day 
h Hour 
m Minute 
s Second 
ms Milli-second
```

大小单位
```
b Bytes 
kb Kilobytes 
mb Megabytes 
gb Gigabytes 
tb Terabytes 
pb Petabytes
```
距离单位
```
Mile mi or miles 
Yard yd or yards 
Feet ft or feet 
Inch in or inch 
Kilometer km or kilometers 
Meter m or meters 
Centimeter cm or centimeters 
Millimeter mm or millimeters 
Nautical mile NM, nmi or nauticalmiles
```
模糊匹配
fuzziness 

case 参数

在不支持post的情况下可以用 source 来替代

URL访问安全
通过设置 config.yml 文件里下面参数来实现
```
rest.action.multi.allow_explicit_index: false
```
参数默认是 true，设置为false后会拒绝指定的请求

