---
title: Zeppelin Integration Part 1
categories:
- BI
feature_image: "https://picsum.photos/2560/600?image=872"
---

用BI平台，带来更好的数据探索体验。

## BI导言

BI(Business intelligence)，商业智能平台。帮助用户导入、清理、分析各种来源的数据，来提升业务决策的效率与有效性。对于BI平台，整体的需求如下：

1. 多样的数据源支持
2. 响应式的展示（移动和桌面设备）
3. 用户自定义仪表盘，精美的数据可视化展示
4. 权限系统沿用现有业务系统
5. 良好的执行效率，可伸缩的部署

## 入坑

理论上是这样，每个平台在试用的时候的都看起来很美好，支持各种部署、多种数据源、精美的展示。但是当要把BI平台嵌入到业务系统时，就出问题了。一旦你开始从源码编译这些BI平台，尝试读懂项目的结构，就陷入了深渊。

今天尝试的是编译[Zeppelin](https://github.com/apache/zeppelin)，前端我觉得还好，一眼就能看出，还可以独立启动与打包。不过一到后端我就蒙蔽了，首先这个全体打包就写的很诡异，难道你不能给个选项只编译必要的解释器吗，我又不需要全部的解释器啊。我们截取编译时`log`的一部分，放在最后。气死我了

再想了想，其实可以下载`net-install-interpreter`的包，因为它前端是WAR包放在项目里的，只要打包`mvn package`，然后替换前端WAR包就行，在下一节对过程进行描述


> 现在大部分BI平台，都是前后端分离的。
> 这个Zeppelin，乍一看是打包成WAR包，但是实际上是前后端分离的。
> 之前fork的Metabase就好一点，可以前后端完全分开跑。

## 前端 

前端项目似乎有两个，总之编译和启动都比较轻松。

- zeppelin-web
- zeppelin-web-angular

### zeppelin-web

因为有个包是Git上的，可能要配置`.bowerrc`，完全让代理接管请求。

```bash
yarn install
yarn run dev
yarn run build:dist
mvn package
```

### zeppelin-web-angular

```bash
npm install # 默认用LTS版本，不要升级npm
npm start # 然后就能调试前端的项目了，真好
mvn package
```

### 最终目标

两个项目打包完成后，都以`0.10.war`的形式存在。可以将他们拷贝进`zeppelin-0.9.0-bin-netinst`，然后执行

```bash
# export ZEPPELIN_WAR=zeppelin-web-0.10.0-SNAPSHOT.war ZEPPELIN_ANGULAR_WAR=zeppelin-web-angular-0.10.0-SNAPSHOT.war
# 似乎前面那个不替换其实也行，不过为了整体性考虑，需要把前面的登录封死
# 直接替换新UI
export ZEPPELIN_ANGULAR_WAR=zeppelin-web-angular-0.10.0-SNAPSHOT.war
bin/zeppelin-daemon.sh start
```

这里有个小坑，查有关资料可以看出，`zeppelin-web-angular`是新UI，用`Angular`重新构建的。有个[issue](https://github.com/apache/zeppelin/commit/6ddc0c685f9c684c3b79b803cf238266bfd0a669)介绍了二者的合并过程，我看第一眼以为现在的Jetty也是用两个端口来处理两个WAR。好吧，后面一沟通，发现旧UI的菜单里有个`Try the new Zeppelin`。点进去，访问的地址是`localhost:8080/next/`，原来是换了个`context`。从实现进度来看，新UI已经从单独的项目合并进了主分支，迟早会替代旧UI。

最终，我选择在旧UI上关闭掉登录接口，在新UI上加上自定义逻辑。


## 后端 

> 后端项目是哪个？如何只运行后端
> 打包之后，就前后端都运行了
> 前先完成前面的前端编译，最好都能生成对应的dist

### 混沌的mvn

说明：

1. [zeppelin-development](https://zeppelin.apache.org/docs/0.9.0/development/contribution/how_to_contribute_code.html#run-zeppelin-server-in-development-mode)
2. `-pl`指定project名, `--am` 也就是`also make`，会把相依赖的项目也编译。靠，那你早说啊。我前面还喷那么狠，早点写清楚怎么前后端分别测试不就好了...

主要都要在`zeppelin`主目录下，针对`pom.xml`（主要的）的命令。先执行

```bash
mvn clean package -pl 'zeppelin-server' --am -DskipTests
mvn clean install -pl 'zeppelin-server' --am -DskipTests # 稍微改一下，添加到.m2里
```

执行日志如下，两步是类似的。package给你用来测试打包，install就会把一些SNAPSHOT移入本地`.m2/repository`。

```log
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Zeppelin 0.10.0-SNAPSHOT:
[INFO]
[INFO] Zeppelin ........................................... SUCCESS [02:07 min]
[INFO] Zeppelin: Common ................................... SUCCESS [  4.005 s]
[INFO] Zeppelin: Interpreter .............................. SUCCESS [04:15 min]
[INFO] Zeppelin: Interpreter Shaded ....................... SUCCESS [03:06 min]
[INFO] Zeppelin: Interpreter Parent ....................... SUCCESS [  1.890 s]
[INFO] Zeppelin: Markdown interpreter ..................... SUCCESS [04:09 min]
[INFO] Zeppelin: Jupyter Support .......................... SUCCESS [  3.295 s]
[INFO] Zeppelin: Zengine .................................. SUCCESS [04:08 min]
[INFO] Zeppelin: Server ................................... SUCCESS [ 12.739 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  18:10 min
[INFO] Finished at: 2021-08-12T00:51:34+08:00
[INFO] ------------------------------------------------------------------------
scarlet@LAPTOP:~/playground/zeppelin$ mvn clean package -pl 'zeppelin-server' --am -DskipTests
```

执行完之后，就到了最迷惑的地方，先看原文

```bash
cd zeppelin-server
HADOOP_HOME=YOUR_HADOOP_HOME JAVA_HOME=YOUR_JAVA_HOME \
mvn exec:java -Dexec.mainClass="org.apache.zeppelin.server.ZeppelinServer" -Dexec.args=""
# Windows
mvn exec:java -D"exec.mainClass"="org.apache.zeppelin.server.ZeppelinServer"
```

这个步骤我在Linux下都不太好复现，不过通过`VS Code`，找了恰当的使用方法。主要的坑点在于，后台启动的时候，是需要去读取前端进`Jetty`的，所以要在根目录下执行。用前面的步骤`package install`核心组件后。在zeppelin根目录，指向那个server项目，执行相关参数。此时就会自动读取 `web` 和`web-angular`下面的`dist`，然后就相当于启动了项目。

```bash
mvn exec:java -Dexec.mainClass="org.apache.zeppelin.server.ZeppelinServer" -Dexec.args="" -f "/home/scarlet/playground/zeppelin/zeppelin-server/pom.xml"
```


### 默认编译

```bash
# java -version 
# mvn -version 
mvn clean package -DskipTests [Options]
# 你就不能给个选项，改成你发行的另一个版本，用网络来安装解释器吗...
# 我要吐血了，Google也没搜到什么好结果，是大家都放弃了吗..
```

## 解释器

### hive

> hive 被合并进jdbc解释器，如果要安装，需要在interpreter界面配置，并配置有关artifact
> 我用net-install版本地尝试一下，然后手动打包移入离线环境

```bash
./bin/install-interpreter.sh -n jdbc
# Install jdbc(org.apache.zeppelin:zeppelin-jdbc:0.9.0) to /home/scarlet/downloads/zeppelin-0.9.0-bin-netinst/interpreter/jdbc ...
# Interpreter jdbc installed under /home/scarlet/downloads/zeppelin-0.9.0-bin-netinst/interpreter/jdbc.
#1. Restart Zeppelin
#2. Create interpreter setting in 'Interpreter' menu on Zeppelin GUI
#3. Then you can bind the interpreter on your note
```

`/#/interpreter` 界面可以配置`repository`，在`conf/interpreter.json`里, 可以把 `central去掉`

```json
    {
      "id": "central",
      "type": "default",
      "url": "https://repo1.maven.org/maven2/",
      "host": "repo1.maven.org",
      "protocol": "https",
      "releasePolicy": {
        "enabled": true,
        "updatePolicy": "daily",
        "checksumPolicy": "warn"
      },
      "snapshotPolicy": {
        "enabled": true,
        "updatePolicy": "daily",
        "checksumPolicy": "warn"
      },
      "mirroredRepositories": [],
      "repositoryManager": false
    }
```

一个`hive-jdbc`的`pom.xml`可以这么写，或者去`mvnrepository.com`。下面的`Compiled Dependencies`里会写清楚大概的依赖关系

```xml
    <dependency>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-common</artifactId>
      <version>2.6.0</version>
    </dependency>


    <dependency>
      <groupId>org.apache.hive</groupId>
      <artifactId>hive-jdbc</artifactId>
      <version>2.3.9</version>
    </dependency>
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