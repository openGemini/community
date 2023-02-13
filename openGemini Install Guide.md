为帮助大家正确使用openGemini，本文内容整理于B站openGemini视频教程第一课《openGemini集群部署和监控》，并在此基础上对openGemini端口、配置文件等常见问题进行详细说明。
![1](https://user-images.githubusercontent.com/49023462/200799881-9cde59bf-f632-4fb8-af8c-0a4b1b10b438.png)

关于openGemini的任何问题，可以在社区openGemini Q&A Issue中和我们交流！我们也会将收集的Q&A在这里面进行发布。
![2](https://user-images.githubusercontent.com/49023462/200800054-59771bb5-2b65-4c0b-89e6-c6d46640d50c.png)

## 端口说明
![3](https://user-images.githubusercontent.com/49023462/200800239-63c53229-b7df-41eb-9504-ec1f4b558173.png)

端口说明如下表所示：
<table>
  <tr>
    <th>组件</th>
    <th>端口</th>
    <th>说明</th>
  </tr>
  <tr>
    <td rowspan="2">ts-sql</td>
    <td>8086</td>
    <td>端口可变更，openGemini对外提供服务的统一入口</td>
  </tr>
  <tr>
    <td>6061</td>
    <td>不可变更，若被其他程序占用，则pprof功能不可用</td>
  </tr>
  <tr>
    <td rowspan="4">ts-meta</td>
    <td>8092</td>
    <td>端口可变更，ts-meta与ts-sql、ts-store之间正常业务交互使用的端口</td>
  </tr>
  <tr>
    <td>8091</td>
    <td>端口可变更，ts-meta的运维接口</td>
  </tr>
  <tr>
    <td>8088</td>
    <td>端口可变更，选举通信使用，三个ts-meta组成一个复制集，复制集之间通过raft协议进行选举</td>
  </tr>
  <tr>
    <td>8010</td>
    <td>端口可变更，ts-store（新）加入集群时使用</td>
  </tr>
  <tr>
    <td rowspan="4">ts-store</td>
    <td>8400</td>
    <td>端口可变更，ts-sql通过该端口将数据写入ts-store</td>
  </tr>
  <tr>
    <td>8401</td>
    <td>端口可变更，ts-sql通过该端口查询ts-store的数据</td>
  </tr>
  <tr>
    <td>8011</td>
    <td>端口可变更，ts-meta监测ts-store心跳使用</td>
  </tr>
  <tr>
    <td>6060</td>
    <td>不可变更，若被其他程序占用，则pprof功能不可用</td>
  </tr>
</table>

## 部署单机
ts-server为openGemini的单机版二进制文件，可简单理解为ts-server由一个ts-sql、一个ts-meta和一个ts-store组成。
部署注意事项：
1.	不修改配置文件，直接通过执行脚本 install.sh拉起，此时ts-server仅在127.0.0.1地址上监听，不能对外提供服务，可用于本地功能测试和学习。
2.	如果需要让其他应用程序或节点可以访问，需要修改配置文件openGemini.singlenode.conf，替换所有127.0.0.1地址，如果端口与本地其他程序由冲突，可以做更改。
3.	原配置文件中数据、日志等默认保存在/tmp/openGemini目录下，建议替换所有/tmp/为其他目录，确保有足够的存储空间。同还需要修改启动脚本scripts/install.sh 的/tmp/目录
4.	启动单机版openGemini
脚本启动：
```
> sh scripts/install.sh
```
手动启动：
```
> ts-server run -config=openGemini.singlenode.conf
```
启动日志的最后一行中出现如下日志，表示成功启动：
```
“successfully opened storage "/xxx/openGemini/data" in 0.034 seconds”
```

## 集群部署
集群部署可以把openGemini的三个组件都部署在一个节点上，也可以把组件分散部署在多个节点上。

### 部署伪集群
把openGemini集群的所有组件都部署在同一个节点上，这种集群部署方式我们称之为伪集群部署，目前社区提供了部署脚本install_cluster.sh。运行命令为： sh scripts/install_cluster.sh，执行该命令，在不修改配置文件的前提下，可直接在本地拉起一个openGemini集群，包括1个ts-sql、3个ts-meta和2个ts-store组件。但该集群仅在本地回环地址127.0.0.1上监听运行，可用于本地功能测试和学习，不能对外提供访问服务。
若要让集群监听本机IP，让外部节点可以访问，配置上相对要复杂一点，虽然可行，但不推荐。同样以部署1个ts-sql、3个ts-meta和2个ts-store组成的集群为例。
1.	分配端口
所有节点均监听本机IP地址，如192.168.0.1，所以所有组件之间不能使用相同的端口，需重新分配。可以做如下分配（参考）：
![4](https://user-images.githubusercontent.com/49023462/200800373-65a3ac6c-f38d-46ed-86d6-8b8f21232d50.png)

2.	配置文件修改
openGemini只有一个集群配置文件openGemini.conf，我们进行集群配置时，如果在一个节点上只部署一个ts-meta、一个ts-sql和一个ts-store，或者其中两个或一个，不存在相同组件部署在同一个节点上。则可以考虑在该节点上所有组件共用一个配置文件openGemini.conf
显然部署伪集群是不能共用一个，因为在同一个节点上有3个ts-meta，2个ts-store。所以我们建议为每个组件对应一个配置文件。具体做法如下：
```
> cp –rf openGemini.conf sql.conf
> cp –rf openGemini.conf meta-1.conf
> cp –rf openGemini.conf meta-2.conf
> cp –rf openGemini.conf meta-3.conf
> cp –rf openGemini.conf store-1.conf
> cp –rf openGemini.conf store-2.conf
```
**修改sql.conf，仅修改如下部分即可，本地IP地址以192.168.0.1为例**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[http]
  bind-address = "192.168.0.1:8086"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
```
**修改meta-1.conf，仅修改如下部分即可**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[meta]
  bind-address = "192.168.0.1:8088"
  http-bind-address = "192.168.0.1:8091"
  rpc-bind-address = "192.168.0.1:8092"
  // 预防/tmp空间不足，建议替换/tmp目录
  dir = "/path/to/openGemini/data/meta/1"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  meta-bind-port = 8010
  members = ["192.168.0.1:8010", "192.168.0.1:8012", "192.168.0.1:8013"]
```
**修改meta-2.conf，仅修改如下部分即可**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[meta]
  bind-address = "192.168.0.1:8089"
  http-bind-address = "192.168.0.1:8093"
  rpc-bind-address = "192.168.0.1:8094"
  // 预防/tmp空间不足，建议替换/tmp目录
  dir = "/path/to/openGemini/data/meta/2"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  meta-bind-port = 8012
  members = ["192.168.0.1:8010", "192.168.0.1:8012", "192.168.0.1:8013"]
```
**修改meta-3.conf，仅修改如下部分即可**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[meta]
  bind-address = "192.168.0.1:8090"
  http-bind-address = "192.168.0.1:8095"
  rpc-bind-address = "192.168.0.1:8096"
  // 为防/tmp空间不足，建议替换/tmp目录
  dir = "/path/to/openGemini/data/meta/3"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  meta-bind-port = 8013
  members = ["192.168.0.1:8010", "192.168.0.1:8012", "192.168.0.1:8013"]
```
**修改store-1.conf，仅修改如下部分即可，为防/tmp空间不足，建议替换/tmp目录**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[data]
  store-ingest-addr = "192.168.0.1:8400"
  store-select-addr = "192.168.0.1:8401"
  store-data-dir = "/path/to/openGemini/data/1"
  store-wal-dir = "/path/to/openGemini/data/1"
  store-meta-dir = "/path/to/openGemini/data/meta/1"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  store-bind-port = 8011
  members = ["192.168.0.1:8010", "192.168.0.1:8012", "192.168.0.1:8013"]
```
**修改store-2.conf，仅修改如下部分即可**
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.1:8094", "192.168.0.1:8096"]
[data]
  store-ingest-addr = "192.168.0.1:8402"
  store-select-addr = "192.168.0.1:8403"
  store-data-dir = "/path/to/openGemini/data/2"
  store-wal-dir = "/path/to/openGemini/data/2"
  store-meta-dir = "/path/to/openGemini/data/meta/2"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  store-bind-port = 8014
  members = ["192.168.0.1:8010", "192.168.0.1:8012", "192.168.0.1:8013"]
```
**如果还需要新增ts-store,可按照该配置，更换一下端口，再拉起即可**

3.	拉起集群
编辑脚本
```
> cp –rf scripts/install_cluster.sh  scripts/cluster.sh
> vim scripts/cluster.sh
```
删除或注释cluster.sh脚本的第27-38行内容
```
# generate config
for((i = 1; i <= 3; i++))
do
    rm -rf config/openGemini-$i.conf
    cp config/openGemini.conf config/openGemini-$i.conf
    sed -i "s/{{meta_addr_1}}/${nodes[1]}/g" config/openGemini-$i.conf
    sed -i "s/{{meta_addr_2}}/${nodes[2]}/g" config/openGemini-$i.conf
    sed -i "s/{{meta_addr_3}}/${nodes[3]}/g" config/openGemini-$i.conf
    sed -i "s/{{addr}}/${nodes[$i]}/g" config/openGemini-$i.conf

    sed -i "s/{{id}}/$i/g" config/openGemini-$i.conf
done
```
然后再修改下启动命令中的配置文件和相关的日志目录
```
nohup build/ts-meta -config config/openGemini -1.conf -pidfile /tmp/openGemini/pid/meta1.pid > /tmp/openGemini/logs/1/meta_extra1.log 2>&1 &
```
改为
```
nohup build/ts-meta -config config/meta-1.conf -pidfile /path/openGemini/pid/meta1.pid > /path/openGemini/logs/1/meta_extra1.log 2>&1 &
```
以此类推
修改完成，执行命令,便可拉起伪集群
```
> sh scripts/cluster.sh
```
### 部署集群

目前还没有开发集群部署的自动化脚本，只能手动部署，欢迎感兴趣的童鞋到社区贡献!
openGemini集群部署如图所示，集群有ts-meta(3x), ts-sql(2x), ts-store(2x)：  

![5](https://user-images.githubusercontent.com/49023462/200800488-5683ecc2-e06b-4b65-a8ca-33b3bceaf6e4.jpg)

这种部署方式，不存在两个相同组件被部署在同一个节点之上，没有端口竞争，则可以让同一个节点上的所有组件共用一个配置文件。
以Node1：192.168.0.1为例，仅需修改配置文件openGemini.conf如下内容：
```
[common]
  meta-join = ["192.168.0.1:8092", "192.168.0.2:8092", "192.168.0.3:8092"]
[meta]
  bind-address = "192.168.0.1:8088"
  http-bind-address = "192.168.0.1:8091"
  rpc-bind-address = "192.168.0.1:8092"
  # 建议修改目录
  dir = "/path/openGemini/data/meta/1"
[http]
  bind-address = "192.168.0.1:8086"
[data]
  store-ingest-addr = "192.168.0.1:8400"
  store-select-addr = "192.168.0.1:8401"
  # 建议修改目录
  store-data-dir = "/path/openGemini/data/1"
  store-wal-dir = "/path/openGemini/data/1"
  store-meta-dir = "/path/openGemini/data/meta/1"
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.1"
  store-bind-port = 8011
  meta-bind-port = 8010
  members = ["192.168.0.1:8010", "192.168.0.2:8010", "192.168.0.3:8010"]
```
Node2，Node3的配置文件，两个地方保持不变：
```
[common]
meta-join = ["192.168.0.1:8092", "192.168.0.2:8092", "192.168.0.3:8092"]
[gossip]
store-bind-port = 8011
meta-bind-port = 8010
members = ["192.168.0.1:8010", "192.168.0.2:8010", "192.168.0.3:8010"]
```
其他地方的IP地址分别替换为Node2的IP 192.168.0.2 和 Node3的IP 192.168.0.3即可，端口可以不用修改。
**需要注意的是，openGemini拉起集群时对组件启动顺序是有要求的，先启动所有三个节点之上的ts-meta组件（命令示例）：**
```
> nohup ts-meta --config openGemini.conf -pidfile meta.pid > meta_extra.log 2>&1 &
```
然后启动ts-store组件（命令示例）：
```
> nohup ts-store --config openGemini.conf -pidfile store.pid > store_extra.log 2>&1 &
```
然后启动ts-store组件（命令示例）：
```
> nohup ts-sql --config openGemini.conf -pidfile sql.pid > sql_extra.log 2>&1 &
```

## 集群扩容
以扩ts-store组件为例，按部署方式可分为三种情况：
1.	新增组件ts-store部署在已有节点上，该节点已存在ts-store组件，这种情况下，ts-store的各个端口需要重新分配。
部署方式如图所示：
![6](https://user-images.githubusercontent.com/49023462/200800553-73d0bb25-de2c-4cf2-b401-8d8ddb00ded2.png)

为新增节点单独准备配置文件，具体配置如下：
```
[common]
  # 保持不变
  meta-join = [meta-join = ["192.168.0.1:8092", "192.168.0.2:8092", "192.168.0.3:8092"]
…
[data]
  store-ingest-addr = "192.168.0.3:8402"
  store-select-addr = "192.168.0.3:8403"
  store-data-dir = "/path/to/openGemini/data/2"
  store-wal-dir = "/path/to/openGemini/data/2"
  store-meta-dir = "/path/to/openGemini/data/meta/2"
…
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.3"
  store-bind-port = 8012
   # 保持不变
  members = ["192.168.0.1:8010", "192.168.0.2:8010", "192.168.0.3:8010"]
```
2.	新增组件ts-store部署在已有节点上，该节点无ts-store组件，这种情况下，不需要重新分配端口，除非端口被其他应用程序占用。
部署方式如图所示：
![7](https://user-images.githubusercontent.com/49023462/200800580-2d1b0f70-fb89-42bd-864f-29da12cd3336.png)

可以该节点其他组件共用同一个配置文件，只需修改ts-store对应的配置项即可(IP和目录)
```
[data]
  store-ingest-addr = "192.168.0.2:8400"
  store-select-addr = "192.168.0.2:8401"
  store-data-dir = "/path/to/openGemini/data/1"
  store-wal-dir = "/path/to/openGemini/data/1"
  store-meta-dir = "/path/to/openGemini/data/meta/1"
…
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.2"
  store-bind-port = 8011
  # 保持不变
  members = ["192.168.0.1:8010", "192.168.0.2:8010", "192.168.0.3:8010"]
```
3.	新增组件ts-store部署在新节点上，该节点无ts-store组件，这种情况下，不需要重新分配端口，除非端口被其他应用程序占用。
部署方式如图所示：
![8](https://user-images.githubusercontent.com/49023462/200800601-896711db-17ee-45c5-8cee-1b9f4d342e63.png)

配置文件的配置与第二种情况一样
```
[common]
  # 保持不变
  meta-join = [meta-join = ["192.168.0.1:8092", "192.168.0.2:8092", "192.168.0.3:8092"]
…
[data]
  store-ingest-addr = "192.168.0.4:8400"
  store-select-addr = "192.168.0.4:8401"
  store-data-dir = "/path/to/openGemini/data/1"
  store-wal-dir = "/path/to/openGemini/data/1"
  store-meta-dir = "/path/to/openGemini/data/meta/1"
…
[logging]
  # 建议修改目录
  path = "/path/openGemini/logs"
[gossip]
  bind-address = "192.168.0.4"
  store-bind-port = 8011
   # 保持不变
  members = ["192.168.0.1:8010", "192.168.0.2:8010", "192.168.0.3:8010"]
```




  
