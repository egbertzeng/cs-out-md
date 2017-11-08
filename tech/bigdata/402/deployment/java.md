## 【云星数据---大数据部集群署系列002】：Java部署方案

### 1.创建目录
    ssh bigdata002 'mkdir -p /cloudstar/software/'
    ssh bigdata003 'mkdir -p /cloudstar/software/'
    ssh bigdata004 'mkdir -p /cloudstar/software/'
    ssh bigdata005 'mkdir -p /cloudstar/software/'
    ssh bigdata006 'mkdir -p /cloudstar/software/'
    ssh bigdata007 'mkdir -p /cloudstar/software/'
    ssh bigdata008 'mkdir -p /cloudstar/software/'
    ssh bigdata009 'mkdir -p /cloudstar/software/'
    ssh bigdata010 'mkdir -p /cloudstar/software/'

###  2.安装jdk 
```
1.配置环境变量
    vim　~/.bashrc
    配置如下内容：
    export JAVA_HOME=/cloudstar/software/jdk1.8.0_151
    export PATH=$PATH:$JAVA_HOME/bin
2.测试环境变量
    测试命令
    java -veriosn
    命令回显
    java version "1.8.0_151"
    Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
    Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
3.分发环境变量
    scp ~/.bashrc bigdata002:~/.bashrc
    scp ~/.bashrc bigdata003:~/.bashrc
    scp ~/.bashrc bigdata004:~/.bashrc
    scp ~/.bashrc bigdata005:~/.bashrc
    scp ~/.bashrc bigdata006:~/.bashrc
    scp ~/.bashrc bigdata007:~/.bashrc
    scp ~/.bashrc bigdata008:~/.bashrc
    scp ~/.bashrc bigdata009:~/.bashrc
    scp ~/.bashrc bigdata010:~/.bashrc

4.分发java
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata002:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata003:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata004:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata005:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata006:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata007:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata008:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata009:/cloudstar/software/
    scp -r /cloudstar/software/jdk1.8.0_151 bigdata010:/cloudstar/software/
```

    
    
    
    