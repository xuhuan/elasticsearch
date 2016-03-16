# 安装

---

参考节点运行状态
```
curl localhost:9200/_nodes/stats/process?pretty
```

参数设置成实际的

```
network.host: 192.168.0.180
```

如果装了license，那么必须所有节点都要统一装，否则无license的无法加入集群
默认开启的是组播，但是局域网测试的时候没成功，其中一个设置下面的参数后成功加入集群
```
discovery.zen.ping.unicast.hosts: ["192.168.0.189"]
```








