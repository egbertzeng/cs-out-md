【云星数据---大数据部集群署系列004】：scala部署方案
### 1.下载并解压scala
```
wget https://downloads.lightbend.com/scala/2.10.6/scala-2.10.6.tgz
tar -zxvf  scala-2.10.6.tgz
```


### 2.配置环境变量
```
    vim ~/.bashrc
    配置如下内容：
    export SCALA_HOME=/cloudstar/software/scala-2.10.6
    export PATH=$PATH:$SCALA_HOME/bin
```

### 3.测试环境变量
```
    命令：
    scala 
    返回
    Welcome to Scala version 2.10.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_151).
    Type in expressions to have them evaluated.
    Type :help for more information.
```
### 4.分发环境变量
```
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
### 5.分发scala
```
    scp -r /cloudstar/software/scala-2.10.6 bigdata002:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata003:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata004:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata005:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata006:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata007:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata008:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata009:/cloudstar/software/
    scp -r /cloudstar/software/scala-2.10.6 bigdata010:/cloudstar/software/
```