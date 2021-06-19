---
title: 基于 IDEA 针对 Elasticsearch 7.10 源码 Debug
date: 2021-05-17 23:09:45
tags:
  - CICD
  - DevOps
  - Jenkins
categories: [ELK]
---

## 源码下载

ES 仓库地址：https://github.com/elastic/elasticsearch

```
git clone git@github.com:elastic/elasticsearch.git
git checkout 7.10
```

看到其他人的经验，建议 fork 一下仓库，这样，在对源码进行阅读时，可以将注释等笔记，快速提交到自己的仓库中。

## 本机环境

ES 运行和编译所需要的 JDK 版本是需要分开讨论的。
- 运行：只需要 Java 8 及以上版本即可
- 编译：如果是编译源码，对 JDK 的版本要求又高一点。一般在源码根目录下的 `CONTRIBUTING.md` 文件会有对于编译某个版本的 ES 需要的 JDK 的版本要求

在 [ES 7.10 分支上](https://github.com/Michael728/elasticsearch/blob/7.10/CONTRIBUTING.md#contributing-to-the-elasticsearch-codebase)，是有如下的描述：

> JDK 14 is required to build Elasticsearch. You must have a JDK 14 installation with the environment variable JAVA_HOME referencing the path to Java home for your JDK 14 installation. By default, tests use the same runtime as JAVA_HOME. However, since Elasticsearch supports JDK 8, the build supports compiling with JDK 14 and testing on a JDK 8 runtime; to do this, set RUNTIME_JAVA_HOME pointing to the Java home of a JDK 8 installation. Note that this mechanism can be used to test against other JDKs as well, this is not only limited to JDK 8.

简言之，ES 7.10 版本的编译需要 JDK 14 版本，必须有一个环境变量 `JAVA_HOME` 指向 JDK 14 的安装目录。默认情况，测试使用同样的 `JAVA_HOME`。同时，因为 ES 支持 JDK 8，构建支持用 JDK 14 编译，使用 JDK 8 测试。只需要设置一个 `RUNTIME_JAVA_HOME`  指向 JDK 8 的安装目录就行。

[sdkman](https://sdkman.io/install) 管理不同的 JDK 版本，可以同时有多个 JDK 版本存在一台主机随时切换使用。

- jdk 的最低版本，在项目中的 [buildSrc/src/main/resources/minimumCompilerVersion](https://github.com/Michael728/elasticsearch/tree/7.10/buildSrc/src/main/resources) 文件中可以看到 `14`，用命令 `sdk install java 14.0.2.hs-adpt` 安装 JDK 14。此外，JDK 可以在[华为云镜像站](https://mirrors.huaweicloud.com/openjdk/14/)下载，速度比较块快
- gradle 的最低版本，可以在项目的 [buildSrc/src/main/resources/minimumGradleVersion](https://github.com/Michael728/elasticsearch/blob/7.10/buildSrc/src/main/resources/minimumCompilerVersion) 文件下看到 `6.6.1`

## 源码编译，导入 IDEA

> 一些文章比较久，依然让进入 ES 项目的根目录，运行如下命令 `./gradlew idea`，这个命令是为了配置 ES project 可以在 IDEA 中使用。目前，在 ES 7.10 版本的 [CONTRIBUTING.md#importing-the-project-into-intellij-idea](https://github.com/elastic/elasticsearch/blob/7.10/CONTRIBUTING.md#importing-the-project-into-intellij-idea) 中，已经略过了该步骤，可能是目前新版的 IDEA 已经自动支持了，不需要上面的步骤。

- 选择 `File > Open`
- 在对话框中选择根目录下的 `build.gradle` 文件
- 在对话框中选择 `Open as Project`

设置 `Project SDK` 为：JDK 14，这个步骤同时也会将 `Gradle JVM` 也设为 14。

![Project SDK](https://gitee.com/michael_xiang/images/raw/master/uPic/RdBgz6.png)

> 如果本机网络环境不好，使用默认的 maven 源，速度可能比较慢，可以参考 [知乎/ElasticSearch-7.8.0 源码编译调试 (详细)](https://zhuanlan.zhihu.com/p/188725714) 进行 Gradle 源等的设置。

点击侧边栏 Gradle 的 `Reload All Gradle Projects` 按钮，或者，打开根目录下的 `build.gradle` 文件，会有 `Load Gradle Changes` 按钮。这样会进行源码的构建（编译、下载依赖包等）。

![Build](https://gitee.com/michael_xiang/images/raw/master/uPic/sqMozN.png)

看到上图，则表示源码构建成功。

> 关于 [Roload All Gradle Projects](https://www.jetbrains.com/help/idea/gradle.html)，IDEA 是有相关文档介绍的。

## 调试

### 远程调试

当项目导入到 IDEA 之后，会看到有一个默认的 `Remote JVM Debug` 配置 `Debug Elasticsearch`，这个配置是无法修改的：

![Debug](https://gitee.com/michael_xiang/images/raw/master/uPic/pQf2Id.png)

启动的第一步骤就是先启动这个这个，点击 `Debug` 按钮就好。

接着，在项目根目录下运行如下命令，启动一个 ES 实例：
```
./gradlew :run --debug-jvm
# 如果是 Widows 平台，则运行如下命令
./gradlew.bat :run --debug-jvm 
```

ES 实例启动完毕之后，用如下命令验证：
```
curl -u elastic:password localhost:9200
```

上面的 `-u` 是授权验证 `user` 的作用，如果你尝试在浏览器中直接访问 `localhost:9200`，将会弹出对话框让你输入用户名和密码。此外，如果在 Postman 中访问的话，也需要设置 `Authorization`。

> 之所有会需要授权认证，是因为开启了 xpack 的认证，

其实，除了上面的这种 Remote Debug 之外，还可以通过[华为云镜像站点](https://mirrors.huaweicloud.com/elasticsearch/) 下载源码对应的 ES 客户端，然后启动客户端之后，`attach` 到进程中开始调试。下面简要介绍一下步骤：

- 调整 ES 客户端的 config 目录下的 `jvm.options` 文件，加入 JVM 的配置参数：`-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005`
- 启动 ES 客户端：`./elasticsearch`，Windows 客户端是：`./elasticsearch.bat`
- 创建一个 Remote 的远程启动配置：

![Remote](https://gitee.com/michael_xiang/images/raw/master/uPic/VwR3OT.png)

### 源码调试

源码调试的步骤比较繁琐，没有上面的方式方便。

提前在 [华为云镜像站点](https://mirrors.huaweicloud.com/elasticsearch/) 下载好源码对应版本的 ES 客户端安装包，解压好。

![ES Client](https://gitee.com/michael_xiang/images/raw/master/uPic/iBzX4d.png)

在 IDEA 中创建一个 Application 启动配置：

![Application](https://gitee.com/michael_xiang/images/raw/master/uPic/YApR8g.png)

VM options 如下：
```
-Des.path.home=/Users/michael/opt/es/elasticsearch-7.10.2
-Des.path.conf=/Users/michael/opt/es/elasticsearch-7.10.2/config
-Dlog4j2.disable.jmx=true
-Xmx4g
-Xms4g
```

- `Des.path.home` 指刚刚 ES 客户端的解压根目录
- `Des.path.conf` 指刚刚 ES 客户端解解压目录下的配置目录路径

需要打开你 `Project SDK` 设置的 JDK 目录中 `conf/security/java.policy` 的文件，例如我本机就是 `/Users/michael/.sdkman/candidates/java/14.0.2.hs-adpt/conf/security/java.policy` 文件，在 `grant` 括号中，添加如下内容：

```
permission java.lang.RuntimePermission "createClassLoader";
permission java.lang.RuntimePermission "setContextClassLoader";
```

> 上面的设置，是为了避免遇到 `org.elasticsearch.bootstrap.StartupException: java.security.AccessControlException: access denied ("java.lang.RuntimePermission" "createClassLoader")`、和 `java.security.AccessControlException: access denied ("java.lang.RuntimePermission" "createClassLoader")` 的报错

经过上面的设置，就可以通过启动主类的方式成功启动了。

### 断点

断点加在 `org/elasticsearch/rest/action/search/RestSearchAction.java` 137 行，执行搜索就会进入断点。

## FAQ

### JDK 16 isn't compatible with Gradle 6.6.1, Please fix JAVA_HOME environment variable

这是因为 Gradle JVM 的版本与 Gradle 版本不兼容，可以在 IDEA 的 `Build Tools` -> `Gradle` 中进行设置：

![gradle jvm](https://gitee.com/michael_xiang/images/raw/master/uPic/ogkPZx.png)

> 发现关于 JDK 的设置，当你设置了 `Project SDK`  的 JDK 版本，这里的 `Gradle JVM` 将会自动和前者保持保持一致。

参考：
- [Found Invalid Gradle JVM configuration](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360008588420-Found-Invalid-Gradle-JVM-configuration)

### unsupported class file major version 60

解决这个问题，还是采用上面描述的方法，设置正确 `Gradle JVM`  版本。

- [Stackoverflow/How to fix “unsupported class file major version 60” in IntelliJ?](https://stackoverflow.com/questions/67079327/how-to-fix-unsupported-class-file-major-version-60-in-intellij)

### gradle 出现 Connection refused (Connection refused) 问题

与网络有问题，检查 Proxy 是否联通，总之，要保证下载依赖的网络畅通。

## 参考
- [知乎/ElasticSearch-7.8.0 源码编译调试 (详细)](https://zhuanlan.zhihu.com/p/188725714) 推荐
- [CBD Blog/IntelliJ debug elasticsearch 7.10 source code](https://icbd.github.io/IntelliJ-debug-elasticsearch-source-code/)
- [segmentfault/讲得最明白的Elasticsearch源码调试环境搭建教程](https://segmentfault.com/a/1190000023647742) ES 7.1.0 版本的笔记
- [小旋锋/教你编译调试Elasticsearch 6.3.2源码](https://juejin.cn/post/6844903663807234061)
- [lanffy/ElasticSearch源码解读一：源码编译和Debug环境搭建](https://lanffy.github.io/2019/04/08/Elasticsearch-Compile-Source-And-Debug) ES 6.7.0 的版本，该博主围绕 ES 写了不少总结
- [idea源码调试的问题/java.lang.NoClassDefFoundError: org/elasticsearch/plugins/ExtendedPluginsClassLoader](https://elasticsearch.cn/question/8243)
