# Centos_7.9安装kudu_1.16.0_release

### 说明:

1. 编译平台Centos7.9. 编译参考 [Apache Kudu - Installing Apache Kudu](https://kudu.apache.org/docs/installation.html)  

2. 该安装的集群是每台机器一个master,一个tserver。需要大于等于3个的奇算个集群（例如3台，5台......）。

3. 如用于生产环境需要适当修改参数. 

### 安装说明

1. root用户执行以下命令
   
   ```bash
   yum install cyrus-sasl-plain 
   yum install cyrus-sasl-gssapi
   yum install lsb
   yum install tree
   ```

2. 添加kudu用户
   
   ```bash
   adduser kudu
   passwd kudu
   ```

3. 复制install_kudu_1.16.0_release.tar.gz.0-5到安装的相应目录(例如: /home/kudu),并解压缩
   
   ```bash
   cat install_kudu_1.16.0_release.tar.gz.* | tar -zxv
   ```

4. 安装kudu
   
   ```bash
   cd install_kudu_1.16.0_release
   
   ./install.sh
   脚本第一次要求输入本机ip.
   脚本第二次要求输入其他机器ip
   ```

5. 其他机器重复2-4步骤.

6. 启动kudu master,每台机器执行以下脚本
   
   ```bash
   cd kudu_1.16.0_release
   
   ./start_kudu-master.sh
   ```

7. 启动kudu tserver,每台机器执行以下脚本
   
   ```bash
   ./start_kudu-tserver.sh
   ```

### 脚本说明

- install.sh  安装脚本,回去先删除当前目录的kudu_1.16.0_release目录

- start_kudu-master.sh 启动本机kudu master

- start_kudu-tserver.sh 启动本机kudu tserver

- ps_kudu.sh 查看kudu本机进行

- stop_kudu_master.sh 停止kudu master

- stop_kudu_tserver.sh 停止kudu tserver

- clean_kudu-master.sh 清除kudu master的所有数据

- clean_kudu-tserver.sh 清楚kudu tserver的所有数据

### 查看kudu管理界面

http://xxx.xxx.xxx.xxx:8051

### 常用命令

###### 查看集群状态

kudu cluster ksck xxx.xxx.xxx.141:7051,xxx.xxx.xxx.91:7051,xxx.xxx.xxx.93:7051

```bash
[kudu@xxx-xx-xx-xx bin]$ ./kudu cluster ksck xxx.xxx.xxx.141:7051,xxx.xxx.xxx.91:7051,xxx.xxx.xxx.93:7051
Master Summary
               UUID               |      Address       | Status
----------------------------------+--------------------+---------
 2071d4015dc1485ca335899344f2c4dx | xxx.xxx.xxx.91:7051  | HEALTHY
 7536d32f00c6400aa64b2201c926630x | xxx.xxx.xxx.93:7051  | HEALTHY
 88aea91623f5491b94b5932115a0360x | xxx.xxx.xxx.141:7051 | HEALTHY

Flags of checked categories for Master:
        Flag         |                            Value                            |         Master
---------------------+-------------------------------------------------------------+-------------------------
 builtin_ntp_servers | 0.pool.ntp.org,1.pool.ntp.org,2.pool.ntp.org,3.pool.ntp.org | all 3 server(s) checked
 time_source         | system                                                      | all 3 server(s) checked

Tablet Server Summary
               UUID               |      Address       | Status  | Location | Tablet Leaders | Active Scanners
----------------------------------+--------------------+---------+----------+----------------+-----------------
 c51c8458a62145a7b107da606d4ee02x | xxx.xxx.xxx.91:7050  | HEALTHY | <none>   |       0        |       0
 d5c0b53b602f44708a6c41b76d323eax | xxx.xxx.xxx.93:7050  | HEALTHY | <none>   |       0        |       0
 f428d3dec050461580515d0b96c8d0fx | xxx.xxx.xxx.141:7050 | HEALTHY | <none>   |       0        |       0

Tablet Server Location Summary
 Location |  Count
----------+---------
 <none>   |       3

Flags of checked categories for Tablet Server:
        Flag         |                            Value                            |      Tablet Server
---------------------+-------------------------------------------------------------+-------------------------
 builtin_ntp_servers | 0.pool.ntp.org,1.pool.ntp.org,2.pool.ntp.org,3.pool.ntp.org | all 3 server(s) checked
 time_source         | system                                                      | all 3 server(s) checked

Version Summary
 Version |         Servers
---------+-------------------------
 1.16.0  | all 6 server(s) checked

The cluster doesn't have any matching tablets

The cluster doesn't have any matching system tables
The cluster doesn't have any matching tables
Tablet Replica Count Summary
   Statistic    | Replica Count
----------------+---------------
 Minimum        | 0
 First Quartile | 0
 Median         | 0
 Third Quartile | 0
 Maximum        | 0


OK
```

###### 平衡集群负载

kudu cluster rebalance xxx.xxx.xxx.141:7051,xxx.xxx.xxx.91:7051,xxx.xxx.xxx.93:7051
