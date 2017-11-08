## 云星数据（深圳）有限公司-大数据团队-【大数据部集群署系列003】：zookeeper分布式部署方案
### 1. bigdata001上安装ZOOKEEPER：(在 bigdata001:/cloudstar/software 目录下执行)
```
    ZooKeeper服务器集群概述
        A.ZooKeeper简介
                zk可以用来保证数据在zk集群之间的数据的事务性一致。
                zk传递的数据非常小，一般小于2M
        B.zookeeper的服务器集群搭建的要求
                zk服务器集群规模不小于3个节点，最好是奇数台。
                要求各服务器之间系统时间要保持一致。
        C.zookeeper的服务器集群的规划
                bigdata001  10.99.22.21   server.1
                bigdata002  10.99.22.22   server.2
                bigdata003  10.99.22.23   server.3
```
### 2.下载并解压ZooKeeper
```
   wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6.tar.gz
   tar -zxvf zookeeper-3.3.6.tar.gz
```
### 3.配置环境变量
```
        vim　~/.bashrc
        添加如下内容
        export ZOOKEEPER_HOME=/cloudstar/software/zookeeper-3.3.6
        export PATH=$PATH:$ZOOKEEPER_HOME/bin
        分发环境变量
        scp ~/.bashrc bigdata002:~/.bashrc
        scp ~/.bashrc bigdata003:~/.bashrc
        scp ~/.bashrc bigdata004:~/.bashrc
        scp ~/.bashrc bigdata005:~/.bashrc
        scp ~/.bashrc bigdata006:~/.bashrc
        scp ~/.bashrc bigdata007:~/.bashrc
        scp ~/.bashrc bigdata008:~/.bashrc
        scp ~/.bashrc bigdata009:~/.bashrc
        scp ~/.bashrc bigdata010:~/.bashrc
```
        
###  4..配置zookeeper
```
        mv ${ZOOKEEPER_HOME}/conf/zoo_sample.cfg  ${ZOOKEEPER_HOME}/conf/zoo.cfg
        vim ${ZOOKEEPER_HOME}/conf/zoo.cfg
        (A)指定数据文件的存放位置(同步数据存放的位置)
        修改：dataDir=/tmp/zookeeper   ,此文件夹在系统重启后清空
        变为：dataDir=/cloudstar/software/zookeeper-3.3.6/data
        (B)增加zookeeper节点：(配置如下信息)
        server.1=bigdata001:2888:3888
        server.2=bigdata002:2888:3888
        server.3=bigdata003:2888:3888
        （2888:数据传输端口,3888:leader和flower的选举端口）
        C. 创建zookeeper数据存放文件夹
         mkdir /cloudstar/software/zookeeper-3.3.6/data
         echo 1 > /cloudstar/software/zookeeper-3.3.6/data/myid
```
          
### 5.分发ZOOKEEPER
```
    scp -r /cloudstar/software/zookeeper-3.3.6  bigdata002:/cloudstar/software/
    scp -r /cloudstar/software/zookeeper-3.3.6  bigdata003:/cloudstar/software/
```

### 6.修改各自机器上的ID
```
     echo 2 > /cloudstar/software/zookeeper-3.3.6/data/myid
     echo 3 > /cloudstar/software/zookeeper-3.3.6/data/myid
```

### 7.ZOOKEEPER常用命令
```
        zkServer.sh start   
        zkServer.sh status 
        zkServer.sh stop
        zkServer.sh restart 
```
         
