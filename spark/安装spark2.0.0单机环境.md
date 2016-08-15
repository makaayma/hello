

1、环境准备
（1）配套软件版本要求：Spark runs on Java 6+ and Python 2.6+. For the Scala API, Spark 1.3.1 uses Scala 2.10. You will need to use a compatible Scala version (2.10.x).
（2）安装好linux、jdk、python， 一般linux均会自带安装好jdk与python，但注意jdk默认为openjdk，建议重新安装oracle jdk。
（3）IP：192.168.1.100  hostname：master
（4）scala和spark安装目录：/usr/local/src/
（5）JAVA安装目录：/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home

2、安装scala
（0）选择安装目录
$cd /usr/local/src/
$mkdir scala
$cd scala
（1）下载scala
$wget http://downloads.typesafe.com/scala/2.11.8/scala-2.11.8.tgz
（2）解压文件
$tar -zxvf scala-2.10.5.tgz
（3）配置环境变量
$vim ~/.bashrc
#SCALA VARIABLES START
export SCALA_HOME=/usr/local/src/scala/scala-2.11.6
export PATH=$SCALA_HOME/bin:$PATH
#SCALA VARIABLES END
$ source ~/.bashrc
$ scala -version
Scala code runner version 2.11.6 -- Copyright 2002-2013, LAMP/EPFL
（4）验证scala
$ scala
Welcome to Scala version 2.11.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_101).
Type in expressions to have them evaluated.
Type :help for more information.
scala> 9*9
res0: Int = 81
scala> println("hello ma")
hello ma

3、安装spark
（0）选择安装目录
$cd /usr/local/src/
$mkdir spark
$cd spark
（1）下载spark
wget http://d3kbcqa49mib13.cloudfront.net/spark-2.0.0-bin-hadoop2.7.tgz
（2）解压spark
tar -zxvf spark-2.0.0-bin-hadoop2.7.tgz
（3）配置环境变量
$vim ~/.bashrc
#SPARK VARIABLES START 
export SPARK_HOME=/usr/local/src/spark/spark-2.0.0-bin-hadoop2.7
export PATH=$PATH:$SPARK_HOME/bin 
#SPARK VARIABLES END
$ source ~/.bashrc
（4）配置spark
 	$ cd /usr/local/src/spark/spark-2.0.0-bin-hadoop2.7/conf
$ mv spark-env.sh.template spark-env.sh
$vim spark-env.sh
export SCALA_HOME=/usr/local/src/scala/scala-2.11.6
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home
export SPARK_MASTER_IP=192.168.1.100
export SPARK_WORKER_MEMORY=512m 
export master=spark://192.168.1.100:7070
$mv slaves.template slaves
$vim slaves
master
（5）启动spark
$cd /usr/local/src/spark/spark-2.0.0-bin-hadoop2.7/sbin
$ ./start-master.sh 
说明：本机运行start-all.sh脚本失败，不知道为什么(启动Worker失败)
注意，hadoop也有start-all.sh脚本，因此必须进入具体目录执行脚本
$ jps
71954 Jps
71861 Master
(6)启动Worker（如果上一步启动Worker失败，则运行这一步，否则忽略这一步）
打开浏览器http://localhost:8080 ，复制第二行URL后面内容
（本机的情况：URL：spark://userdeMacBook-Air-3.local:7077）
$cd /usr/local/src/spark/spark-2.0.0-bin-hadoop2.7/bin
$./spark-class org.apache.spark.deploy.worker.Worker spark://userdeMacBook-Air-3.local:7077
$ jps
71954 Jps
71861 Master
71943 Worker

4、验证安装情况
（1）运行自带示例
$ bin/run-example  org.apache.spark.examples.SparkPi
（2）查看集群环境
http://master:8080/
（3）进入spark-shell
$spark-shell
（4）查看jobs等信息
http://master:4040/jobs/