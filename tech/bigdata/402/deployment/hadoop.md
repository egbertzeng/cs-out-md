
一、规划和策略
    bigdata001    master
    bigdata002    master
    bigdata003    slave
    bigdata004    slave
    bigdata005    slave
   在bigdata001上配置，而后同步到其他机器上。
二、下载hadoop
    wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.0.0-beta1/hadoop-3.0.0-beta1.tar.gz
三、解压hadoop
    tar -zxvf  hadoop-3.0.0-beta1.tar.gz  
四、分发hadoop
    scp -r /cloudstar/software/hadoop-3.0.0-beta1    bigdata002:/cloudstar/software/
    scp -r /cloudstar/software/hadoop-3.0.0-beta1    bigdata003:/cloudstar/software/
    scp -r /cloudstar/software/hadoop-3.0.0-beta1    bigdata004:/cloudstar/software/
    scp -r /cloudstar/software/hadoop-3.0.0-beta1    bigdata005:/cloudstar/software/
五、配置hadoop
    1.配置hadoop的运行环境
            vim ${HADOOP_HOME}/etc/hadoop/hadoop-env.sh
            在此文件中只需要配置jdk的路径即可：
            找到: export java_home=${java_home}这一行，修改为：
            export java_home=/bigdata/software/jdk1.8.0_74  #jdk实际安装目录是/bigdata/software/jdk1.8.0_74

    2.配置hadoop的核心配置文件
            vim ${HADOOP_HOME}/etc/hadoop/core-site.xml
            将其中的<configuration></configuration>修改为如下内容即可.
            <configuration>
                <property>
                    <name>fs.defaultfs</name>
                    <value>hdfs://cluster1</value>
                </property>
                <property>
                    <name>hadoop.tmp.dir</name>
                     <value>/cloudstar/software/hadoop-3.0.0-beta1/tmp</value>
                </property>
                <property>
                    <name>ha.zookeeper.quorum</name>
                    <value>bigdata001:2181,bigdata002:2181,bigdata003:2181</value>
                </property>
            </configuration>

         3.配置hdfs的配置文件
            vim ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
            将其中的<configuration></configuration>修改为如下内容即可.
             <configuration>
                <property>
                    <name>dfs.webhdfs.enabled</name>
                    <value>true</value>
                </property>
                <property>
                        <name>dfs.replication</name>
                        <value>3</value>
                </property>
                <property>
                        <name>dfs.nameservices</name>
                        <value>cluster1</value>
                </property>
                <property>
                        <name>dfs.ha.namenodes.cluster1</name>
                        <value>bigdata001,bigdata002</value>
                </property>
                <property>
                        <name>dfs.namenode.rpc-address.cluster1.bigdata001</name>
                        <value>bigdata001:9000</value>
                </property>
                <property>
                        <name>dfs.namenode.http-address.cluster1.bigdata001</name>
                        <value>bigdata001:50070</value>
                </property>
                <property>
                        <name>dfs.namenode.rpc-address.cluster1.bigdata002</name>
                        <value>bigdata002:9000</value>
                </property>
                <property>
                        <name>dfs.namenode.http-address.cluster1.bigdata002</name>
                        <value>bigdata002:50070</value>
                </property>
                <property>
                        <name>dfs.namenode.shared.edits.dir</name>
                        <value>qjournal://bigdata001:8485;bigdata002:8485/cluster1</value>
                </property>
                <property>
                        <name>dfs.journalnode.edits.dir</name>
                        <value>/cloudstar/software/hadoop-3.0.0-beta1/tmp/journal</value>
                </property>
                <property>
                        <name>dfs.ha.automatic-failover.enabled.cluster1</name>
                        <value>true</value>
                </property>
                <property>
                        <name>dfs.client.failover.proxy.provider.cluster1</name>
                        <value>org.apache.hadoop.hdfs.server.namenode.ha.configuredfailoverproxyprovider</value>
                </property>
                <property>
                        <name>dfs.ha.fencing.methods</name>
                        <value>sshfence</value>
                </property>
                <property>
                        <name>dfs.ha.fencing.ssh.private-key-files</name>
                        <value>/root/.ssh/id_rsa</value>
                </property>
                <property> 
                        <name>dfs.namenode.datanode.registration.ip-hostname-check</name>
                        <value>false</value>
                </property>
             </configuration>

        4.配置机器中从节点
            vim ${HADOOP_HOME}/etc/hadoop/workers
            此文件中配置的是hadoop集群中从节点，一行指定一个从节点.
                bigdata001
                bigdata002
                bigdata003
                bigdata004
                bigdata005


        5.配置hadoop2的计算框架
                    vim ${HADOOP_HOME}/etc/hadoop/mapred-site.xml
                  将其中的<configuration></configuration>修改为如下内容即可.
                  <configuration>
                        <property>
                            <name>mapreduce.framework.name</name>
                            <value>yarn</value>
                        </property>
                        <property>
                          <name>mapreduce.jobhistory.address</name>
                          <value>bigdata001:10020</value>
                          <description>mapreduce jobhistory server ipc host:port</description>
                        </property>
                        <property>
                          <name>mapreduce.jobhistory.webapp.address</name>
                          <value>bigdata001:19888</value>
                          <description>mapreduce jobhistory server web ui host:port</description>
                        </property>
                        <property>
                            <name>mapreduce.jobhistory.done-dir</name>
                            <value>/history/done</value>
                        </property>
                        <property>
                            <name>mapreduce.jobhistory.intermediate-done-dir</name>
                            <value>/history/done_intermediate</value>
                        </property>
                  </configuration>
         6.配置hadoop2的yarn计算框架
            vim ${HADOOP_HOME}/etc/hadoop/yarn-site.xml
            将其中的<configuration></configuration>修改为如下内容即可.
            a.无yarn-ha的配置方法
                  <configuration>
                    <property>
                        <name>yarn.resourcemanager.hostname</name>
                        <value>bigdata001</value>
                    </property>
                    <property>
                        <name>yarn.nodemanager.aux-services</name>
                        <value>mapreduce_shuffle</value>
                    </property>
                  </configuration>

             b.有yarn-ha的配置方法
            <configuration>
                <!-- site specific yarn configuration properties -->
                <property>
                        <name>yarn.resourcemanager.ha.enabled</name>
                        <value>true</value>
                </property>
                <property>
                        <name>yarn.resourcemanager.cluster-id</name>
                        <value>cluster1</value>
                </property>
                <property>
                        <name>yarn.resourcemanager.ha.rm-ids</name>
                        <value>bigdata001,bigdata002</value>
                </property>
                <property>
                        <name>yarn.resourcemanager.hostname.bigdata001</name>
                        <value>bigdata001</value>
                </property>
                <property>
                        <name>yarn.resourcemanager.hostname.bigdata002</name>
                        <value>bigdata002</value>
                </property>
                <property>
                        <name>yarn.resourcemanager.zk-address</name>
                        <value>bigdata001:2181,bigdata002:2181,bigdata003:2181</value>
                </property>
                <property>
                        <name>yarn.nodemanager.aux-services</name>
                        <value>mapreduce_shuffle</value>
                </property>
                <property>
                        <name>yarn.log-aggregation-enable</name>
                        <value>true</value>
                </property>
            </configuration>

六、分发hadoop的配置文件
    scp  -r ${HADOOP_HOME}/etc/hadoop/  bigdata002:${HADOOP_HOME}/etc/
    scp  -r ${HADOOP_HOME}/etc/hadoop/  bigdata003:${HADOOP_HOME}/etc/
    scp  -r ${HADOOP_HOME}/etc/hadoop/  bigdata004:${HADOOP_HOME}/etc/
    scp  -r ${HADOOP_HOME}/etc/hadoop/  bigdata005:${HADOOP_HOME}/etc/
七、初始hadoop集群
        1.启动journalnode(在规划的journalnode主机上执行)
                因为我们的journalnode规划在bigdata001,bigdat002上，
                因此我们可以只在这三台机器上执行如下命令：
                ${HADOOP_HOME}/sbin/hadoop-daemons.sh start journalnode

        2.启动zookeeper (如果zookeeper在前面安装的时候已经启动了，就不必再启动了)
           
        3.格式化zkfc(目的就是要在zookeeper机器中创建[hadoop-ha]节点,用于保证hadoop集群的ha)
                因为zkfc要担当起集群中切换的使命，只需在活动主节点上执行这个命令，备用主节点不要执行。
                因为本案例中bigdata001为活动主节点，bigdata002为备用主节点。只需在bigdata001上执行这个命令。
                在bigdata001上执行：  ${HADOOP_HOME}/bin/hdfs  zkfc  -formatzk
                在bigdata2上执行: ${zookeeper_home}/bin/zkcli.sh
                        输入：ls /  可以看到 hadoop-ha节点
                        输入：ls /hadoop-ha  可以看到cluster1节点
        4.格式化活动主节点(在bigdata001上执行 )
                格式：  ${HADOOP_HOME}/bin/hdfs  namenode  -format
                启动：  ${HADOOP_HOME}/sbin/hadoop-daemon.sh  start  namenode
                验证：jps能看到namenode
        5.格式化备用主节点(在bigdata7上执行 )
                格式：${HADOOP_HOME}/bin/hdfs  namenode  -bootstrapstandby
                启动：${HADOOP_HOME}/sbin/hadoop-daemon.sh  start  namenode
                验证：jps能看到namenode
        6.启动zkfc(在bigdata001、bigdata002上执行)
                在bigdata001上执行: ${HADOOP_HOME}/sbin/hadoop-daemon.sh start zkfc
                在bigdata002上执行: ${HADOOP_HOME}/sbin/hadoop-daemon.sh start zkfc

                ${HADOOP_HOME}/sbin/hadoop-daemons.sh stop zkfc

                验证： dfszkfailovercontroller
        7.启动datanode(在主节点上执行)
                 在bigdata001上执行: ${HADOOP_HOME}/sbin/hadoop-daemons.sh start datanode
                验证：在datanode节点上jps能看到 datanode
                                ${HADOOP_HOME}/sbin/hadoop-daemons.sh start datanode

        8.日常维护
            ${HADOOP_HOME}/sbin/hadoop-daemons.sh start namenode
            ${HADOOP_HOME}/sbin/hadoop-daemons.sh start datanode
            ${HADOOP_HOME}/sbin/yarn-daemons.sh start resourcemanager
            ${HADOOP_HOME}/sbin/yarn-daemons.sh start nodemanager
 
六、验证hadoop集群
        1.验证hdfs
                a.在不同的主机上执行jps命令可以看到相应的进程
                b.在windows上用浏览器查看：http://bigdata001:50070 #能看到namenode的管理界面
                c.我们的bigdata6、bigdata7一个为ha的active状态,另一个则是standby状态。
        2.验证yarn
                a.在不同的主机上执行jps命令可以看到相应的进程
                b.在windows上用浏览器查看：http://bigdata001:8088  #能看到yarn的管理界面
                c.可以成功执行单词计数示例程序：
                  ${HADOOP_HOME}/bin/hadoop  jar  ${HADOOP_HOME}/share/hadoop/mapreduce/ hadoop-mapreduce-examples-3.0.0-beta1.jar  作业名称 /输入目录 /输出目录
                  ${HADOOP_HOME}/bin/hadoop  jar  ${HADOOP_HOME}/share/hadoop/mapreduce/ hadoop-mapreduce-examples-3.0.0-beta1.jar  wordcount  /data/mr/in/ /data/mr/out/test001





