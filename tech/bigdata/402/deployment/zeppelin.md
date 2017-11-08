


Zeppelin安装
Zeppelin是一个基于web的笔记本，支持交互式数据分析。你可以用SQL、Scala等做出数据驱动的、交互、协作的文档。(类似于ipython notebook，可以直接在浏览器中写代码、笔记并共享。
支持多种语言，默认是scala(背后是spark shell)，SparkSQL, Markdown 和 Shell。 多用途笔记本可实现你所需要的： 数据采集 、 数据发现 、数据分析 、数据可视化和协作。
一些基本的图表已经包含在Zeppelin中。可视化并不只限于SparkSQL查询，后端的任何语言的输出都可以被识别并可视化。 支持动态表格，Zeppelin 可以在你的笔记本中动态地创建一些输入格式。 
甚至可以添加自己的语言支持、由于目前并不提供binary安装包，需要自己编译。

第一部分：基于别人编译好的
一、安装部署
1.下载解压
wget http://mirrors.tuna.tsinghua.edu.cn/apache/zeppelin/zeppelin-0.7.3/zeppelin-0.7.3-bin-all.tgz
tar -zxvf zeppelin-0.7.3-bin-all.tgz


2.配置环境变量
    vim　~/.bashrc
    配置如下内容：
    export ZEPPELIN_HOME=/cloudstar/software/zeppelin-0.7.3-bin-all
    export PATH=$PATH:$ZEPPELIN_HOME/bin
测试环境变量
    echo $ZEPPELIN_HOME
    返回
    /cloudstar/software/zeppelin-0.7.3-bin-all

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
    
            
3.修改配置文件
    cp $ZEPPELIN_HOME/conf/zeppelin-env.sh.template $ZEPPELIN_HOME/conf/zeppelin-env.sh
    cp $ZEPPELIN_HOME/conf/zeppelin-site.xml.template $ZEPPELIN_HOME/conf/zeppelin-site.xml
3.修改zeppelin-env.sh
    cp $ZEPPELIN_HOME/conf/
    vim zeppelin-env.sh
    
export MASTER=spark://bigdata001:7077,bigdata002:7077
export HADOOP_CONF_DIR=/cloudstar/software/hadoop-3.0.0-beta1/etc/hadoop

4.修改zeppelin-site.xml
    vim $ZEPPELIN_HOME/conf/zeppelin-site.xml
    
    <property>
      <name>zeppelin.server.port</name>
      <value>8089</value>
      <description>Server port.</description>
    </property>


6.启动和关闭
    $ZEPPELIN_HOME/bin/zeppelin-daemon.sh stop
    nohup   $ZEPPELIN_HOME/bin/zeppelin-daemon.sh start & 
    nohup   $ZEPPELIN_HOME/bin/zeppelin-daemon.sh restart &
7.验证启动
    浏览器输入，可以看到zeppelin的界面
    http://bigdata001:8089/




二、测试案例
1.在页面上输入
    val rdd1 = sc.textFile("hdfs://bigdata002:9000/test/data")
    rdd1.count()
三、电影测试案例
MovieLens 100k数据包含有100，000条用户与电影的相关数据。 
首先下载并解压数据：
http://files.grouplens.org/datasets/movielens/ml-100k.zip
[root@chinac82 software]# yum install -y unzip zip
[root@chinac82 software]# chmod ugo+rwx ml-100k.zip
[root@chinac82 software]# unzip ml-100k.zip
[root@chinac82 software]# cd ml-100k
#用户文件(ID,年龄,性别,职业,邮编)
[root@chinac82 ml-100k]#  head -2 u.user
1|24|M|technician|85711
2|53|F|other|94043
#电影文件(ID,标题,发行日期,等)
[root@chinac82 ml-100k]#  head -2 u.item
1|Toy Story (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?Toy%20Story%20(1995)|0|0|0|1|1|1|0|0|0|0|0|0|0|0|0|0|0|0|0
2|GoldenEye (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?GoldenEye%20(1995)|0|1|1|0|0|0|0|0|0|0|0|0|0|0|0|0|1|0|0
#用户对电影的评分文件(用户ID，电影ID，分数(1~5),时间戳)，以\t分隔
[root@chinac82 ml-100k]#  head -2 u.data
196    242    3    881250949
186    302    3    891717742
上传数据到hdfs
[root@chinac82 ml-100k]# hadoop fs -put u.user u.item u.data  /data/sparktd/
剩下的操作见zeppelin页面



