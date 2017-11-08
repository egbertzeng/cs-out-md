
一、规划和策略
    规划
        集群一(保障各个节点上的Scala已经安装完成)
        master port 8888
        znode /spark
        bigdata001  master
        bigdata002  master
        bigdata001  worker
        bigdata002  worker
        bigdata003  worker
        bigdata004  worker
        bigdata006  worker

    策略：
        在bigdata6上安装，然后分发到其他机器
二、下载、解压、分发
    下载spark
         wget https://d3kbcqa49mib13.cloudfront.net/spark-2.2.0-bin-hadoop2.7.tgz
    解压spark
        tar -zxvf  spark-2.2.0-bin-hadoop2.7.tgz
    分发spark
        scp -r spark-2.2.0-bin-hadoop2.7 bigdata002:/cloudstar/software/
        scp -r spark-2.2.0-bin-hadoop2.7 bigdata003:/cloudstar/software/
        scp -r spark-2.2.0-bin-hadoop2.7 bigdata004:/cloudstar/software/
        scp -r spark-2.2.0-bin-hadoop2.7 bigdata005:/cloudstar/software/
               
三、配置并分发环境变量
1.配置环境变量
    vim　~/.bashrc
    配置如下内容：
    export SPARK_HOME=/cloudstar/software/spark-2.2.0-bin-hadoop2.7
    export PATH=$PATH:$SPARK_HOME/bin
2.分发环境变量
        scp ~/.bashrc bigdata002:~/.bashrc
        scp ~/.bashrc bigdata003:~/.bashrc
        scp ~/.bashrc bigdata004:~/.bashrc
        scp ~/.bashrc bigdata005:~/.bashrc
        scp ~/.bashrc bigdata006:~/.bashrc
        scp ~/.bashrc bigdata007:~/.bashrc
        scp ~/.bashrc bigdata008:~/.bashrc
        scp ~/.bashrc bigdata009:~/.bashrc
        scp ~/.bashrc bigdata010:~/.bashrc

三、配置spark 
        1.配置spark的env文件
            cp  ${SPARK_HOME}/conf/spark-env.sh.template ${SPARK_HOME}/conf/spark-env.sh
            vim ${SPARK_HOME}/conf/spark-env.sh
            a.无HA的配置方式(没有采用)
              此文件指定spark的运行环境。开头处加入以下内容。
              ###指定jdk安装目
              export JAVA_HOME=/bigdata/software/jdk1.8.0_74
              ###指定scala安装目录
              export SCALA_HOME=/bigdata/software/scala-2.11.8
              ###指定worker节点分配给Excutors的最大内存
              export SPARK_WORKER_MEMORY=24G
              ###指定hadoop集群的配置文件目录
              export HADOOP_CONF_DIR=/bigdata/software/hadoop-2.7.2
              ###指定spark集群的master节点的ip
              export SPARK_MASTER_IP=192.168.1.200

            b.有HA的配置方式（采用）
                此文件指定spark的运行环境。开头处加入以下内容。
                ###指定jdk安装目
                export JAVA_HOME=/cloudstar/software/jdk1.8.0_151
                ###指定scala安装目录
                export SCALA_HOME=/cloudstar/software/scala-2.10.6
                ###指定worker节点分配给Excutors的最大内存
                export SPARK_WORKER_MEMORY=24G
                ###指定hadoop集群的配置文件目录
                export HADOOP_CONF_DIR=/cloudstar/software/hadoop-3.0.0-beta1
                ###指定spark集群的master节点的ip,HA模式下，不要填写任何地址
                SPARK_MASTER_IP=
                ###指定hadoop集群的配置文件目录
                export SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=bigdata001:2181,bigdata002:2181,bigdata003:2181 -Dspark.deploy.zookeeper.dir=/spark"
                     
     
        2.配置spark的slave文件
              cp   ${SPARK_HOME}/conf/slaves.template ${SPARK_HOME}/conf/slaves
              vim  ${SPARK_HOME}/conf/slaves
              配置文件: 此文件用于指定worker节点，一行一个.
                bigdata001
                bigdata002
                bigdata003
                bigdata004
                bigdata005
                
四、分发spark配置文件(在bigdata6上执行)
    scp -r ${SPARK_HOME}/conf/ bigdata002:${SPARK_HOME}/
    scp -r ${SPARK_HOME}/conf/ bigdata003:${SPARK_HOME}/
    scp -r ${SPARK_HOME}/conf/ bigdata004:${SPARK_HOME}/
    scp -r ${SPARK_HOME}/conf/ bigdata005:${SPARK_HOME}/
五、操作spark集群
    1.启动服务器：
        在bigdata001上执行：${SPARK_HOME}/sbin/start-all.sh 
    2.验证服务器：
        bigdata6上jps能看到Master进程，其他节点jps能看到Worker进程.
        浏览器访问:  http://bigdata001:8081
        浏览器访问:  http://bigdata001:7077
    3.关闭服务器：
        在bigdata001上执行：${SPARK_HOME}/sbin/stop-all.sh
    4.连接客户端:(在想要开启spark客户端的机器上执行,bigdata005)
        ${SPARK_HOME}/bin/spark-shell --master spark://bigdata001:7077
    5.验证客户端:
        浏览器访问:  http://bigdata005:4040
    6.测试方法：    在${SPARK_HOME}下有一个文件README.md这个文件中包含大量的spark字样。
        可以使用这个文件来测试安装好的spark能不能用.
        a.在HDFS中创建这个文件
            进入目录： cd ${SPARK_HOME}
            创建文夹： hadoop fs -mkdir /test/data/
            上传文件： hadoop fs -copyFromLocal ${SPARK_HOME}/README.md /test/data
        b.在spark-shell客户端中执行如下命令
            var readmeFile= sc.textFile("hdfs://bigdata002:9000/test/data/README.md")
            readmeFile.collect
            readmeFile.count
        c.测试结果
            以上命令可以在spark-shell客户端形成结果输出。如果能输出结果说明，spark安装成
    7.spark集群的操作命令如下
        a.集群的启动和关闭
            nohup ${SPARK_HOME}/sbin/start-all.sh &
            ${SPARK_HOME}/sbin/stop-all.sh
        b.master启动和关闭
            ${SPARK_HOME}/sbin/start-master.sh
            ${SPARK_HOME}/sbin/stop-master.sh
        c.slave集群启动和关闭
            ${SPARK_HOME}/sbin/start-slaves.sh
            ${SPARK_HOME}/sbin/stop-slaves.sh
        d.slave启动和关闭
            ${SPARK_HOME}/sbin/stop-slave.sh
        e.启动历史记录
            ${SPARK_HOME}/sbin/start-history-server.sh
        f.启动spark-shell
            ${SPARK_HOME}/bin/spark-shell
