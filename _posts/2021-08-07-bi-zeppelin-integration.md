---
title: Zeppelin Integration Part 1
categories:
- BI
feature_image: "https://picsum.photos/2560/600?image=872"
---

用BI平台，带来更好的数据探索体验。

理论上是这样，每个平台在试用的时候的都看起来很美好，支持各种部署、多种数据源、精美的展示。但是当要把BI平台嵌入到业务系统时，就出问题了。一旦你开始从源码编译这些BI平台，尝试读懂项目的结构。
就陷入了深渊。

今天尝试的是编译[Zeppelin](https://github.com/apache/zeppelin)，前端我觉得还好，一眼就能看出，还可以独立启动与打包。不过一到后端我就蒙蔽了，首先这个全体打包就写的很诡异，难道你不能给个选项只编译必要的解释器吗，我又不需要全部的解释器啊。我们截取编译时`log`的一部分，放在最后。气死我了

再想了想，其实可以下载`net-install-interpreter`的包，因为它前端是WAR包放在项目里的，只要打包`npm build`，然后替换前端WAR包就行。


> 现在大部分BI平台，都是前后端分离的。
> 这个Zeppelin，乍一看是打包成WAR包，但是实际上是前后端分离的。
> 之前fork的Metabase就好一点，可以前后端完全分开跑。

### 前端 zeppelin-web-angular

> 很容易看出来，前端项目是这个

```bash
npm install # 默认用LTS版本，不要升级npm
npm start # 然后就能调试前端的项目了，真好
```

### 后端 

> 后端项目是哪个？如何只运行后端
> 打包之后，就前后端都运行了


### 默认方式

```bash
# java -version 
# mvn -version 
mvn clean package -DskipTests [Options]
# 你就不能给个选项，改成你发行的另一个版本，用网络来安装解释器吗...
# 我要吐血了，Google也没搜到什么好结果，是大家都放弃了吗..
```

## 附言

### 说好的log

> 吐了，耗时这么久，一点用没有

```log
[INFO] Reactor Summary for Zeppelin 0.10.0-SNAPSHOT:
[INFO] 
[INFO] Zeppelin ........................................... SUCCESS [ 11.131 s]
[INFO] Zeppelin: Common ................................... SUCCESS [02:04 min]
[INFO] Zeppelin: Interpreter .............................. SUCCESS [02:12 min]
[INFO] Zeppelin: Interpreter Shaded ....................... SUCCESS [02:45 min]
[INFO] Zeppelin: Interpreter Parent ....................... SUCCESS [02:01 min]
[INFO] Zeppelin: Markdown interpreter ..................... SUCCESS [  7.085 s]
[INFO] Zeppelin: Jupyter Support .......................... SUCCESS [02:01 min]
[INFO] Zeppelin: Zengine .................................. SUCCESS [02:06 min]
[INFO] Zeppelin: Display system apis ...................... SUCCESS [ 10.930 s]
[INFO] Zeppelin: Jupyter Interpreter ...................... SUCCESS [  6.097 s]
[INFO] Zeppelin: Jupyter Interpreter Shaded ............... SUCCESS [02:03 min]
[INFO] Zeppelin: R ........................................ SUCCESS [ 15.752 s]
[INFO] Zeppelin: Kotlin interpreter ....................... SUCCESS [01:11 min]
[INFO] Zeppelin: Groovy interpreter ....................... SUCCESS [02:01 min]
[INFO] Zeppelin: Spark Parent ............................. SUCCESS [  2.043 s]
[INFO] Zeppelin: Spark Shims .............................. SUCCESS [  2.013 s]
[INFO] Zeppelin: Spark1 Shims ............................. SUCCESS [02:02 min]
[INFO] Zeppelin: Spark2 Shims ............................. SUCCESS [02:03 min]
[INFO] Zeppelin: Spark3 Shims ............................. SUCCESS [  4.717 s]
[INFO] Zeppelin: Python interpreter ....................... SUCCESS [  4.122 s]
[INFO] Zeppelin: Spark Interpreter ........................ SUCCESS [ 24.586 s]
[INFO] Zeppelin: Spark Scala Parent ....................... SUCCESS [  2.472 s]
[INFO] Zeppelin: Spark Interpreter Scala_2.10 ............. SUCCESS [  9.145 s]
[INFO] Zeppelin: Spark Interpreter Scala_2.11 ............. SUCCESS [02:08 min]
[INFO] Zeppelin: Spark Interpreter Scala_2.12 ............. SUCCESS [  8.306 s]
[INFO] Zeppelin: Spark dependencies ....................... SUCCESS [02:25 min]
[INFO] Zeppelin: Shell interpreter ........................ SUCCESS [02:03 min]
[INFO] Zeppelin: Spark-Submit interpreter ................. SUCCESS [02:02 min]
[INFO] Zeppelin: Submarine interpreter .................... SUCCESS [ 40.875 s]
[INFO] Zeppelin: MongoDB interpreter ...................... SUCCESS [ 33.415 s]
[INFO] Zeppelin: Angular interpreter ...................... SUCCESS [  2.232 s]
[INFO] Zeppelin: Livy interpreter ......................... SUCCESS [  4.039 s]
[INFO] Zeppelin: HBase interpreter ........................ SUCCESS [02:09 min]
[INFO] Zeppelin: Apache Pig Interpreter ................... SUCCESS [ 18.745 s]
[INFO] Zeppelin: JDBC interpreter ......................... SUCCESS [ 36.842 s]
[INFO] Zeppelin: File System Interpreters ................. SUCCESS [  3.562 s]
[INFO] Zeppelin: Flink Parent ............................. SUCCESS [  2.694 s]
[INFO] Zeppelin: Flink Shims .............................. SUCCESS [  3.223 s]
[INFO] Zeppelin: Flink1.10 Shims .......................... SUCCESS [  3.918 s]
[INFO] Zeppelin: Flink1.11 Shims .......................... SUCCESS [  4.557 s]
[INFO] Zeppelin: Flink1.12 Shims .......................... SUCCESS [  4.755 s]
[INFO] Zeppelin: Flink1.13 Shims .......................... SUCCESS [  4.970 s]
[INFO] Zeppelin: Flink Scala Parent ....................... SUCCESS [  4.261 s]
[INFO] Zeppelin: Flink Interpreter Scala_2.11 ............. SUCCESS [ 33.348 s]
[INFO] Zeppelin: Flink Interpreter Scala_2.12 ............. SUCCESS [05:47 min]
[INFO] Zeppelin: Flink-Cmd interpreter .................... SUCCESS [02:02 min]
[INFO] Zeppelin: Apache Ignite interpreter ................ SUCCESS [ 15.615 s]
[INFO] Zeppelin: InfluxDB interpreter ..................... SUCCESS [02:05 min]
[INFO] Zeppelin: Kylin interpreter ........................ SUCCESS [02:01 min]
[INFO] Zeppelin: Lens interpreter ......................... SUCCESS [ 12.648 s]
[INFO] Zeppelin: Apache Cassandra interpreter ............. SUCCESS [01:40 min]
[INFO] Zeppelin: Elasticsearch interpreter ................ SUCCESS [  9.981 s]
[INFO] Zeppelin: BigQuery interpreter ..................... SUCCESS [  6.010 s]
[INFO] Zeppelin: Alluxio interpreter ...................... SUCCESS [  8.230 s]
[INFO] Zeppelin: Scio ..................................... SUCCESS [01:14 min]
[INFO] Zeppelin: Neo4j interpreter ........................ SUCCESS [  5.129 s]
[INFO] Zeppelin: Sap ...................................... SUCCESS [  4.238 s]
[INFO] Zeppelin: Scalding interpreter ..................... FAILURE [ 13.825 s]
[INFO] Zeppelin: Java interpreter ......................... SKIPPED
[INFO] Zeppelin: Beam interpreter ......................... SKIPPED
[INFO] Zeppelin: Hazelcast Jet interpreter ................ SKIPPED
[INFO] Zeppelin: Apache Geode interpreter ................. SKIPPED
[INFO] Zeppelin: Kafka SQL interpreter .................... SKIPPED
[INFO] Zeppelin: Sparql interpreter ....................... SKIPPED
[INFO] Zeppelin: Client ................................... SKIPPED
[INFO] Zeppelin: Client Examples .......................... SKIPPED
[INFO] Zeppelin: web Application .......................... SKIPPED
[INFO] Zeppelin: Server ................................... SKIPPED
[INFO] Zeppelin: Plugins Parent ........................... SKIPPED
[INFO] Zeppelin: Plugin S3NotebookRepo .................... SKIPPED
[INFO] Zeppelin: Plugin GitHubNotebookRepo ................ SKIPPED
[INFO] Zeppelin: Plugin AzureNotebookRepo ................. SKIPPED
[INFO] Zeppelin: Plugin GCSNotebookRepo ................... SKIPPED
[INFO] Zeppelin: Plugin ZeppelinHubRepo ................... SKIPPED
[INFO] Zeppelin: Plugin FileSystemNotebookRepo ............ SKIPPED
[INFO] Zeppelin: Plugin MongoNotebookRepo ................. SKIPPED
[INFO] Zeppelin: Plugin OSSNotebookRepo ................... SKIPPED
[INFO] Zeppelin: Plugin Kubernetes StandardLauncher ....... SKIPPED
[INFO] Zeppelin: Plugin Flink Launcher .................... SKIPPED
[INFO] Zeppelin: Plugin Docker Launcher ................... SKIPPED
[INFO] Zeppelin: Plugin Cluster Launcher .................. SKIPPED
[INFO] Zeppelin: Plugin Yarn Launcher ..................... SKIPPED
[INFO] Zeppelin: Packaging distribution ................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  54:42 min
[INFO] Finished at: 2021-08-08T19:16:09+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project zeppelin-scalding_2.10: Could not resolve dependencies for project org.apache.zeppelin:zeppelin-scalding_2.10:jar:0.10.0-SNAPSHOT: The following artifacts could not be resolved: cascading:cascading-core:jar:2.6.1, cascading:cascading-hadoop:jar:2.6.1, cascading:cascading-local:jar:2.6.1, com.hadoop.gplcompression:hadoop-lzo:jar:0.4.19, cascading.avro:avro-scheme:jar:2.1.2: Could not find artifact cascading:cascading-core:jar:2.6.1 in aliyunmaven (https://maven.aliyun.com/repository/public) -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <args> -rf :zeppelin-scalding_2.10
```