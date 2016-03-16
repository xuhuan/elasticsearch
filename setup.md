# 安装

---

### 安装步骤：

1. 前提条件是安装 jdk 并且设置了 JAVA_HOME 系统变量
2. 根据系统下载压缩包或者安装文件
3. 解压到指定文件夹或者安装
4. 根据需要配置config文件夹下的elasticsearch.yml文件
5. 运行 elasticsearch 或者 elasticsearch.bat 即可



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









