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

```log
[INFO] Zeppelin                                                           [pom]
[INFO] Zeppelin: Common                                                   [jar]
[INFO] Zeppelin: Interpreter                                              [jar]
[INFO] Zeppelin: Interpreter Shaded                                       [jar]
[INFO] Zeppelin: Interpreter Parent                                       [pom]
[INFO] Zeppelin: Markdown interpreter                                     [jar]
[INFO] Zeppelin: Jupyter Support                                          [jar]
[INFO] Zeppelin: Zengine                                                  [jar]
[INFO] Zeppelin: Display system apis                                      [jar]
[INFO] Zeppelin: Jupyter Interpreter                                      [jar]
[INFO] Zeppelin: Jupyter Interpreter Shaded                               [jar]
[INFO] Zeppelin: R                                                        [jar]
[INFO] Zeppelin: Kotlin interpreter                                       [jar]
[INFO] Zeppelin: Groovy interpreter                                       [jar]
[INFO] Zeppelin: Spark Parent                                             [pom]
[INFO] Zeppelin: Spark Shims                                              [jar]
[INFO] Zeppelin: Spark1 Shims                                             [jar]
[INFO] Zeppelin: Spark2 Shims                                             [jar]
[INFO] Zeppelin: Spark3 Shims                                             [jar]
[INFO] Zeppelin: Python interpreter                                       [jar]
[INFO] Zeppelin: Spark Interpreter                                        [jar]
[INFO] Zeppelin: Spark Scala Parent                                       [pom]
[INFO] Zeppelin: Spark Interpreter Scala_2.10                             [jar]
[INFO] Zeppelin: Spark Interpreter Scala_2.11                             [jar]
[INFO] Zeppelin: Spark Interpreter Scala_2.12                             [jar]
[INFO] Zeppelin: Spark dependencies                                       [jar]
[INFO] Zeppelin: Shell interpreter                                        [jar]
[INFO] Zeppelin: Spark-Submit interpreter                                 [jar]
[INFO] Zeppelin: Submarine interpreter                                    [jar]
[INFO] Zeppelin: MongoDB interpreter                                      [jar]
[INFO] Zeppelin: Angular interpreter                                      [jar]
[INFO] Zeppelin: Livy interpreter                                         [jar]
[INFO] Zeppelin: HBase interpreter                                        [jar]
[INFO] Zeppelin: Apache Pig Interpreter                                   [jar]
[INFO] Zeppelin: JDBC interpreter                                         [jar]
[INFO] Zeppelin: File System Interpreters                                 [jar]
[INFO] Zeppelin: Flink Parent                                             [pom]
[INFO] Zeppelin: Flink Shims                                              [jar]
[INFO] Zeppelin: Flink1.10 Shims                                          [jar]
[INFO] Zeppelin: Flink1.11 Shims                                          [jar]
[INFO] Zeppelin: Flink1.12 Shims                                          [jar]
[INFO] Zeppelin: Flink1.13 Shims                                          [jar]
[INFO] Zeppelin: Flink Scala Parent                                       [pom]
[INFO] Zeppelin: Flink Interpreter Scala_2.11                             [jar]
[INFO] Zeppelin: Flink Interpreter Scala_2.12                             [jar]
[INFO] Zeppelin: Flink-Cmd interpreter                                    [jar]
[INFO] Zeppelin: Apache Ignite interpreter                                [jar]
[INFO] Zeppelin: InfluxDB interpreter                                     [jar]
[INFO] Zeppelin: Kylin interpreter                                        [jar]
[INFO] Zeppelin: Lens interpreter                                         [jar]
[INFO] Zeppelin: Apache Cassandra interpreter                             [jar]
[INFO] Zeppelin: Elasticsearch interpreter                                [jar]
[INFO] Zeppelin: BigQuery interpreter                                     [jar]
[INFO] Zeppelin: Alluxio interpreter                                      [jar]
[INFO] Zeppelin: Scio                                                     [jar]
[INFO] Zeppelin: Neo4j interpreter                                        [jar]
[INFO] Zeppelin: Sap                                                      [jar]
[INFO] Zeppelin: Scalding interpreter                                     [jar]
[INFO] Zeppelin: Java interpreter                                         [jar]
[INFO] Zeppelin: Beam interpreter                                         [jar]
[INFO] Zeppelin: Hazelcast Jet interpreter                                [jar]
[INFO] Zeppelin: Apache Geode interpreter                                 [jar]
[INFO] Zeppelin: Kafka SQL interpreter                                    [jar]
[INFO] Zeppelin: Sparql interpreter                                       [jar]
[INFO] Zeppelin: Client                                                   [jar]
[INFO] Zeppelin: Client Examples                                          [jar]
[INFO] Zeppelin: web Application                                          [war]
[INFO] Zeppelin: Server                                                   [jar]
[INFO] Zeppelin: Plugins Parent                                           [pom]
[INFO] Zeppelin: Plugin S3NotebookRepo                                    [jar]
[INFO] Zeppelin: Plugin GitHubNotebookRepo                                [jar]
[INFO] Zeppelin: Plugin AzureNotebookRepo                                 [jar]
[INFO] Zeppelin: Plugin GCSNotebookRepo                                   [jar]
[INFO] Zeppelin: Plugin ZeppelinHubRepo                                   [jar]
[INFO] Zeppelin: Plugin FileSystemNotebookRepo                            [jar]
[INFO] Zeppelin: Plugin MongoNotebookRepo                                 [jar]
[INFO] Zeppelin: Plugin OSSNotebookRepo                                   [jar]
[INFO] Zeppelin: Plugin Kubernetes StandardLauncher                       [jar]
[INFO] Zeppelin: Plugin Flink Launcher                                    [jar]
[INFO] Zeppelin: Plugin Docker Launcher                                   [jar]
[INFO] Zeppelin: Plugin Cluster Launcher                                  [jar]
[INFO] Zeppelin: Plugin Yarn Launcher                                     [jar]
[INFO] Zeppelin: Packaging distribution                                   [pom]
```