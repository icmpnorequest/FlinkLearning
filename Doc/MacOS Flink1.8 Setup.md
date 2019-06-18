## MacOS 在 Intellij IDEA上 配置 Flink-1.8.0 环境

[TOC]



### 1 简介

**本文目的**：详细介绍 MacOS 上的 Intellij IDEA 中配置 Flink-1.8.0 的过程。



### 2 配置先决条件

#### 2.1 系统

MacOS Mojave

#### 2.2 Java 环境

（1）未安装 jdk 的情况

- 下载 jdk 1.8，[官网链接](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。

- 配置环境变量等操作，请参考[这篇博客](https://blog.csdn.net/deliciousion/article/details/78046007)，这里不作详述。

（2）已安装 jdk ，但该版本高于 jdk 1.8 的情况，必须再安装 jdk 1.8

- 下载与环境配置同（1）
- 因为安装了两个版本的 jdk，所以需要指定使用哪个 java 版本。有手动切换和自动切换两种方法，可参考[这篇博文](https://segmentfault.com/a/1190000013131276)，这里不作赘述。

```
$ java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
```

#### 2.3 Intellij IDEA

若未下载，可前往[官网链接](https://www.jetbrains.com/idea/download/#section=mac)下载，community 版本即可。

#### 2.4 Maven

- 若未下载，可前往[官网链接](https://maven.apache.org/download.cgi)下载。我下载的是 apache-maven-3.6.1-bin.tar.gz.

- 安装配置可参考[这篇博客](https://my.oschina.net/90888/blog/1789496)，这里不再详述。

```
$ mvn -version
Apache Maven 3.6.1 (d66c9c0b3152b2e69ee9bac180bb8fcc8e6af555; 2019-04-05T03:00:29+08:00)
Maven home: /Users/yantong/Downloads/apache-maven-3.6.1
Java version: 1.8.0_211, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_211.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.14.5", arch: "x86_64", family: "mac"
```

#### 2.5 Flink

于[官网链接](https://flink.apache.org/downloads.html)上下载 Apache flink-1.8.0 Scala 2.11

解压

```
$ cd ~/Downloads        # Go to download directory
$ tar xzf flink-*.tgz   # Unpack the downloaded archive
```



### 3 创建 Maven 项目

我们将使用 Maven archetypes 来构建项目。

#### 3.1 进入到你想创建项目的工作目录

例如，我想创建在 IdeaProjects 下，进入该目录

```
$ cd IdeaProjects
```

#### 3.2 使用 Maven 创建项目

1. 打开 Terminal，键入

```
$ mvn archetype:generate
```

2. 回车，会出现如下信息

```
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] >>> maven-archetype-plugin:3.1.0:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO] 
[INFO] <<< maven-archetype-plugin:3.1.0:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO] 
[INFO] 
[INFO] --- maven-archetype-plugin:3.1.0:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
```

3. 回车，当出现`Define value for property 'groupId'`时，键入如下，并回车

```
Define value for property 'groupId': org.apache.flink
```

4. 当出现如下等`Define value for property 'artifactId':`和 `Define value for property 'version' 1.0-SNAPSHOT:`信息时，键入如下，并回车

```
Define value for property 'groupId': org.apache.flink
Define value for property 'artifactId': flink-quickstart-java
Define value for property 'version' 1.0-SNAPSHOT: : 1.8.0
Define value for property 'package' org.apache.flink: : com.panda
Confirm properties configuration:
groupId: org.apache.flink
artifactId: flink-quickstart-java
version: 1.8.0
package: com.panda
```

5. 出现 BUILD SUCCESS 信息时，表示创建成功。

6. 使用tree 查看建立好的 项目

```
$ tree flink-quickstart-java
flink-quickstart-java
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── panda
    │               └── App.java
    └── test
        └── java
            └── com
                └── panda
                    └── AppTest.java
```

#### 3.3 使用 IntelliJ IDEA 打开创建好的项目

##### Step 1: 打开 Intellij IDEA，选择 Import Project

![1](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/1.png)

##### Step 2: 选择 Import project from external model，选择Maven，点击Next

![2](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/2.png)

##### Step 3: 点击Next

![3](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/3.png)

##### Step 4: 点击Next

![4](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/4.png)

##### Step 5: 点击Next

![5](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/5.png)

##### Step 6: 点击 Finish

![6](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/6.png)

##### Step 7: 导入成功，即可看到如图

![7](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/7.png)

##### Step 8: 打开pom.xml，添加依赖

因为我们需导入是 Flink-1.8.0，参考其[官方链接](https://ci.apache.org/projects/flink/flink-docs-release-1.8/dev/projectsetup/dependencies.html)，导入 Java 编程环境必须的依赖。官网提供了 Java 和 Scala的，按自己需要添加。下面是 Java 环境的必需依赖。

```
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-java</artifactId>
  <version>1.8.0</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-streaming-java_2.11</artifactId>
  <version>1.8.0</version>
  <scope>provided</scope>
</dependency>
```

将上面这部分代码复制到 pom.xml 的<dependencies>里面，如图所示，复制完后，点击右下角的"Enable Auto-import"

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/8.png)



### 4 WordCount 测试代码

#### Step 1: 复制官网上的 WordCount 测试代码

本文的目的是搭建环境，并进行测试，所以使用来自[官方文档](https://ci.apache.org/projects/flink/flink-docs-release-1.8/dev/batch/#dataset-transformations)的 WordCount 代码进行测试。在目录下新建一个 Java Class文件，并将以下代码复制进去。

```java
package com.panda;

import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.java.DataSet;
import org.apache.flink.api.java.ExecutionEnvironment;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.util.Collector;

public class WordCount {

    public static void main(String[] args) throws Exception {
        final ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment();

        DataSet<String> text = env.fromElements(
                "Who's there?",
                "I think I hear them. Stand, ho! Who's there?");

        DataSet<Tuple2<String, Integer>> wordCounts = text
                .flatMap(new LineSplitter())
                .groupBy(0)
                .sum(1);

        wordCounts.print();
    }

    public static class LineSplitter implements FlatMapFunction<String, Tuple2<String, Integer>> {
        public void flatMap(String line, Collector<Tuple2<String, Integer>> out) {
            for (String word : line.split(" ")) {
                out.collect(new Tuple2<String, Integer>(word, 1));
            }
        }
    }
}
```

#### Step 2: 运行程序，解决 Caused by: java.lang.ClassNotFoundException: org.apache.flink.api.java.DataSet 问题

右键运行 WordCount.java 文件，明明代码没有问题，pom.xml 文件也按照官方文档的 Basic Dependencies 进行配置，为何还是报以下错误：

```java
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/flink/api/java/DataSet
	at java.lang.Class.getDeclaredMethods0(Native Method)
	at java.lang.Class.privateGetDeclaredMethods(Class.java:2701)
	at java.lang.Class.privateGetMethodRecursive(Class.java:3048)
	at java.lang.Class.getMethod0(Class.java:3018)
	at java.lang.Class.getMethod(Class.java:1784)
	at sun.launcher.LauncherHelper.validateMainClass(LauncherHelper.java:544)
	at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:526)
Caused by: java.lang.ClassNotFoundException: org.apache.flink.api.java.DataSet
	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	... 7 more

Process finished with exit code 1
```

由 Caused by: java.lang.ClassNotFoundException: org.apache.flink.api.java.DataSet 得知，缺少 Flink-1.8.0 中的 lib 包。

#### Step 3: 导入 flink-1.8.0 中的 lib 和 opt 包

1. 打开 Project Stucture；
2. 选择左列中的 Modules；
3. 选择右列中的 Dependencies；
4. 选择右列中左下角的"+"号；
5. 选择"1 JARs or directories"；
6. 选择下载的 flink-1.8.0 中的 lib 和 opt 包；

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/9.png)

添加好如图：

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/10.png)

7. 点击 apply 和 ok。

#### Step 4: 再次运行 WordCount.java 程序，成功

运行结果：

```java
log4j:WARN No appenders could be found for logger (org.apache.flink.api.java.ExecutionEnvironment).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
(hear,1)
(ho!,1)
(them.,1)
(Stand,,1)
(I,2)
(Who's,2)
(there?,2)
(think,1)

Process finished with exit code 0
```



### 5 将 Java 程序打包成 jar 包

#### Step 1: 打开 Project Structure -> artifacts

选择 JAR -> From modules with dependencies

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/11.png)

打开如图：

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/17.png)

#### Step 2: 选择 WordCount 作为MainClass

点击 MainClass，选择 WordCount，点OK

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/12.png)

#### Step 3: 勾选"Include in project build"，点OK

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/14.png)

#### Step 4: Build artifacts

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/15.png)

#### Step 5: Build 成功之后，会出现一个 out 文件夹，里面就有打包好的 jar 包

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/16.png)



### 6 启动 Flink，运行 jar 包

#### Step 1: 进入到 flink-1.8.0 的路径

我的 flink-1.8.0 就在 Downloads 里面，读者可根据自己的下载位置进入路径

```
cd ~/Downloads/flink-1.8.0
```

#### Step 2: 启动 flink，打开浏览器

在 Terminal 中输入以下命令，启动 Flink

```
./bin/start-cluster.sh
```

打开浏览器，在地址栏输入 localhost:8081，回车，即可显示 flink 的可视化界面

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/18.png)

#### Step 3: 提交 jar 包到 flink 上

首先，为了方便，把5中生成的 flink-quickstart-java.jar 复制到 flink-1.8.0 的examples中

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/19.png)

在 Terminal 中键入

```
$ ./bin/flink run -c com.panda.WordCount examples/flink-quickstart-java.jar
```

其中，-c 表示选择的 class，因为我是将整个项目打包成 jar 包的，需要选择MainClass

在 Terminal 中显示结果如下：

```
Starting execution of program
(I,2)
(Stand,,1)
(Who's,2)
(hear,1)
(ho!,1)
(them.,1)
(there?,2)
(think,1)
Program execution finished
Job with JobID 79202112687cdbaacca520c1db6da68c has finished.
Job Runtime: 2230 ms
Accumulator Results: 
- ca35e52bc9f5a07b095f6da5746cb9bf (java.util.ArrayList) [8 elements]
```

在 Step 4中打开的网页，可以看到可视化项目处理过程

![](/Users/yantong/IdeaProjects/flink-quickstart-java/MacOS Flink Setup Image/20.png)



### 7 总结

到这里，在 MacOS 上搭建 Flink-1.8.0 的环境基本就结束了。本文细致地介绍了搭建、代码测试的全过程，之后我将会继续学习 Flink 的程序编写，批处理、流处理等，欢迎关注点赞，谢谢。