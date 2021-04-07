---
title: JetBrains PyCharm/IDEA 常用插键总结
date: 2020-05-05 22:30:02
tags:
  IDE
  IDEA
  Pycharm
categories: ToolsDev
keywords: [pycharm, idea, plugin]
---

> 工欲善其事必先利其器

## 前言

IDEA 有用一套非常强大的[插件市场](https://plugins.jetbrains.com/)，提供了丰富的插键帮助开发者提升开发效率。之前总结过一篇[博文](https://michael728.github.io/2019/05/11/tools-dev-pycharm-idea/)，渐渐觉得有必要写一篇文章单独罗列一下 IDEA 常用的插键！

## 主题

[IDEA主题-](http://riaway.com/) `Settings->Editor->Color Scheme->Import Scheme`，导入 jar 包即可。这种方式安装主题略微麻烦，其实在应用市场里提供了很多主题可以直接安装的。
[Dracula](https://draculatheme.com/) 在 IDEA 中，可以像安装插键一样搜索该主题进行安装。吸血鬼主题插件，各 IDE 都有适配的主题，非常推荐！
[Material Theme](https://plugins.jetbrains.com/plugin/8006-material-theme-ui) 主题插键

## 安装说明

`Install JetBrains plugin`：弹出IntelliJ IDEA 公司自行开发的插件仓库列表，供下载安装。
`Browse repositories`：弹出插键仓库中所有插键列表供下载安装。
`Install plugin from disk`：浏览本地的插键文件进行安装。

## 插键列表

Lombok 当你使用 lombok 包进行开发时，要同时安装该插键才行！笔者近期就因为升级 IDEA 重装之后忘记安装该插键，导致代码报红问题，半天才发现因为该插键忘记重新安装了。

---

[TestMe](https://github.com/wrdv/testme-idea) Junit 等测试工具的生成插件,支持 `Junit4/5,TestNG,Spock` 等测试框架代码的生成

### 数据库相关

[Free MyBatis plugin](https://plugins.jetbrains.com/plugin/8321-free-mybatis-plugin) 快速从代码跳转到 mapper 及从 mapper 返回代码

---

[Mybatis Log](https://blog.csdn.net/qq_22194659/article/details/89011988) 
 在控制台打印mybatis的脚本日志，需要配置才能在控制台输出，[参考](https://blog.csdn.net/VICHOU_FA/article/details/79285749)

----

Mongo

### [Codota](https://www.codota.com/) 

可以搜索知名开源项目中该类或者方法的使用案例.写代码的时候，某个类不熟悉用法，右键选择”Get relevant examples“

比如我们想了解：SerializationUtils 工具类的用法，也可以直接在网址上搜索即可，网址：https://www.codota.com/code，选择对应的包名

---

### CamelCase

CamelCase （https://plugins.jetbrains.com/plugin/7160-camelcase）：是一个日常开发中常用的一款插键。可以辅助你将变量名方便的进行转换。

我们先简单看一下变量常见的命名方式有哪些：
- `kebab-case`：烤肉串式命名，单词之间用中划线 `-` 隔开，例如 `user-name`，还确实挺形象;

![aleetI](https://gitee.com/michael_xiang/images/raw/master/uPic/aleetI.png)

- 蛇形命名法，蛇在地上爬，因此，蛇形命名法单词之前是下划线 `_` 隔开的。

![RS7szz](https://gitee.com/michael_xiang/images/raw/master/uPic/RS7szz.png)

蛇形命名具体又分为如下两种：
  - `snake_case`：小蛇形命名法，例如 `user_name`；
  - `SNAKE_CASE`：大蛇形命名法，例如 `USER_NAME`；

- 驼峰命名法，单词首字母大写，看上去像驼峰

![k2u36P](https://gitee.com/michael_xiang/images/raw/master/uPic/k2u36P.png)

驼峰命名具体分为如下两种：
  - `camelCase`：驼峰命名法，也称小驼峰命名法，首字母小写，例如 `userName`；
  - `PascalCase`：帕斯卡命名法，也称大驼峰命名法，首字母大写，例如 `UserName`

除以上命名方式之外，匈牙利命名法也比较常见，它的基本命名原则是：`属性+类型+对象描述`，例如 `getListUsers`。

铺垫到此结束，上今天的主角！看看 CamelCase 插键主页上是如何介绍它的：

> Switch easily between kebab-case, SNAKE_CASE, PascalCase, camelCase, snake_case or space case. See Edit menu or use ⇧ + ⌥ + U / Shift + Alt + U. Allows to disable some conversions or change their order in the preferences.

该插键使得我们能够方便的在各种命名方式的变量名之间转换。
- Windows 上的快捷键是：`Shift+Alt+U`
- Mac 上的快捷键是：`Shift+Option+U`

下面上个动图演示一把：

![演示](https://gitee.com/michael_xiang/images/raw/master/uPic/演示.gif)

不要小看这个插键噢，在变量名挺长的情况下，如果手写经常会发生敲错变量名的情况，转换命名方式就不那么方便了，这时候这款插键就能很好的提高你的开发效率了！

---

String Manipulation：字符串转换工具，安装好插件后，选中需要处理的内容后，按快捷键 `Alt+m`，即可弹出工具功能列表

---

anyrule：该插键在 VSCode 也有提供，可以用来搜索适合场景的正则表达式
- 新建一个文件（因为 anyrule 一定要在文本环境下才能生成正则表达式），然后按下快捷键 `Alt+a` 功能键，这时候就会弹出 anyrule 正则表达式搜索框（anyrule 生成的是 javascript 的正则表达式，如果需要用到 Java 中。需要将最前面和最后面的"/"去掉）
- 理解这个表达式的运行原理，点击右下角的"图解正则"。（就是将刚才any-rule生成的正则表达式填入 https://regexper.com/

---

### GsonFormatPlus

给你一段 Json 格式数据，除了手写之外，利用该插键可以快速生成一个和该Json数据对应的Java对象：

比如一段Json格式数据如下：

```shell
{
    "status": 0,
    "msg": "操作成功",
    "data": 1
}
```

GsonFormat则是一个JSON格式数据 → 对象的快速代码自动生成插件

---

### saveAction
格式化、serialVersionUID生成等，很有用的插键

参考：
- [嗨：VSCode和IDEA都请安装上这个神奇的插件](https://mp.weixin.qq.com/s/h7RBvJ20NfQxwTYXaa0eiA)

----

## 编码规范

![门](https://gitee.com/michael_xiang/images/raw/master/uPic/BKcj52.png)

## 概述

今天介绍的插件主要是围绕编码规范的。有追求的程序员，往往都有代码洁癖，要尽量减少代码的「坏味道」。

代码静态检查是有很多种类，例如圈复杂度、重复率等。业界提供了很多静态检查的插件来识别这些不合规的代码，帮助提高项目的质量。比较知名的一个产品是 SonarQube，它提供了一个「门禁」平台，集成了很多静态检查检查。下次有机会介绍一下该平台的搭建。

本文主要介绍 IDEA 中对于 Java 语言静态检查的好插件：
- [Alibaba Java Coding Guidelines](https://plugins.jetbrains.com/plugin/10046-alibaba-java-coding-guidelines) 阿里基于他们 Java 规范提供的插件
- [CheckStyle-IDEA](https://plugins.jetbrains.com/plugin/1065-checkstyle-idea) 检查代码的格式是否符合规范
- [FindBugs-IDEA](https://plugins.jetbrains.com/plugin/3847-findbugs-idea) 检查代码是否有常见的一些 Bug

## Alibaba Java Coding Guidelines

阿里巴巴 Java 编码指南插件支持。

首先说说阿里的 p3c 项目，它的 Github 主页地址是：https://github.com/alibaba/p3c 阿里之前开源过一份 Java 开发手册。手册从编程规约、异常日志、单元测试、MySQL 数据库、工程结构、设计规约等角度，介绍了阿里的 Java 开发规范，这个对于 Java 新手帮助挺大的，能够学到不少东西。有一些坑可能老司机也会翻车。

![泰山版-图片来PDF 文档，侵删](https://gitee.com/michael_xiang/images/raw/master/uPic/nY2JnQ.png)

开发人员 Coding 时，可能就忘记规范了，写出来的代码还是会有『坏味道』。这时候`Alibaba Java Coding Guidelines` 插件就派上用场了。它会根据上面的 Java 开发规范对你的代码进行检查，不符合规范的代码会有提示，并给出修改建议。阿里作为国内 Java 大厂，基于成千上万的工程师总结出来的踩坑经验，我相信给出的规范建议还是比较可靠的。

上个栗子：

```java
  String str = "hello";
  for (int i = 0; i < 100; i++) {
      str = str + "world!";
  }
```

先不要往下看，试着分析一下这段代码哪里可以优化？

![错误示例](https://gitee.com/michael_xiang/images/raw/master/uPic/58tCby.png)

其实，插键扫描的结果不仅有这个问题，还有『魔法数字』的问题。插件的用法，见下面的截图。

![示例](https://gitee.com/michael_xiang/images/raw/master/uPic/f7H5DJ.png)

除此以外，建议在 IDEA 进行代码提交时，勾选上它提供的检查项按钮，这样如果有不合规的代码进行提交，就会提醒你修改：

![commit-checks-job](https://gitee.com/michael_xiang/images/raw/master/uPic/commit-checks-job.gif)

## CheckStyle-IDEA

![插件](https://gitee.com/michael_xiang/images/raw/master/uPic/ts6AC7.png)

项目主页：https://github.com/jshiell/checkstyle-idea

安装好之后，进入设置，勾选上你要选用的默认检查规范：

![kl0L2s](https://gitee.com/michael_xiang/images/raw/master/uPic/kl0L2s.png)

大厂往往都有自己的语言规范，可以导入选用：

![导入规则](https://gitee.com/michael_xiang/images/raw/master/uPic/mdIYTo.png)

右键菜单，选择 `Check Current File` 即可检查当前文件是否符合编码规范：

![使用](https://gitee.com/michael_xiang/images/raw/master/uPic/aqDFoC.png)

检查结果：

![效果](https://gitee.com/michael_xiang/images/raw/master/uPic/yMhJMp.png)

如果不符合规范的写法有点多，整改起来就会很痛苦了，这时候该怎么办？有一个功能叫格式化代码（`Reformat Code`），快捷键是：
- Mac：`Command+Option+L`
- Win：`Ctrl+Shift+L`

如果想按照你指定的规则进行格式化，可以按照如下方式进行自定义：

![格式化规则导入](https://gitee.com/michael_xiang/images/raw/master/uPic/yTSEUo.png)

## FindBugs-IDEA

FindBugs 是一款老牌 Java 静态检查插件了。它的功能和阿里 p3c 那个插件很像，只不过它历史悠久、国际化一点。它同样的可以扫描代码，发现一些可能会引入 Bug 的代码段，给出参考建议。

启动 FindBugs 的方式，右键菜单中，Findbugs 提供了好几个选项：

![启动](https://gitee.com/michael_xiang/images/raw/master/uPic/n9x5iN.png)

- `Analyze Current File`：检查当前文件
- `Analyze Class uner Cursor`：检查光标处的类
- `Analyze Package Files`：检查包文件
- `Analyze Modul Files`：检查 Module 文件
- `Analyze Project Files`：检查项目文件
- `Analyze Scope Files`：检查指定范围内的文件
- `Analyze All Modified Files`：检查所有修改过的文件
- `Analyze changelist files`：检查变更列表中的文件

检查结果：

![结果](https://gitee.com/michael_xiang/images/raw/master/uPic/jgOys4.png)

检查结果分为如下类别：
- `Bad practice`：不好的做法，代码违反了公认的最佳实践标准；
- `Malicious code vulnerbility`：恶意的代码漏洞；
- `Correctness`：可能不正确，比如错误的强制类型转换；
- `Performance`：潜在的性能问题；
- `Security`：安全性；
- `Dodgy code`：糟糕的代码，FindBugs团队认为该类型下的问题代码导 Bug 的可能性很高；
- `Experimental`：实验；
- `Multithreaded correctness`：关注于同步和多线程问题；
- `Internationalization`：国际化

扫描出来的结果怎么看懂，官网有专门的一页介绍 FindBugs Bug Description http://findbugs.sourceforge.net/bugDescriptions.html

![看懂结果](https://gitee.com/michael_xiang/images/raw/master/uPic/p26DR1.png)

除了上面右键菜单启动检查之外，还可以在相应文件夹右键菜单中启动：

![选中检查](https://gitee.com/michael_xiang/images/raw/master/uPic/J58j0l.png)

针对检查的严格程度，其实是可以调节的，建议修改为 `low`，这样会尽可能的扫描出有潜在 Bug 的代码片段：

![严格程度](https://gitee.com/michael_xiang/images/raw/master/uPic/aZ4dnN.png)

## 总结

以上是目前工作中经常用到的静态代码检查插键，虽然不是用了它们就真的能写出好代码，但是这些工具的确能够让你能够在前人的肩膀上少踩一些坑。比如阿里的那个检查插键，你可以按照它的提示，对照着他们的 Java 开发手册查看，分析一下，为何他们会有如此的规约。高楼大厦不都是一砖一瓦砌成的嘛？小知识点的基础打扎实了，才能走得更高！

> 由于国内网络问题，我提前将最新版的《阿里巴巴 Java 开发手册（泰山版）.pdf》上传至云盘了，有需要的小伙伴公众号后台回复 `泰山版` 即可获得下载链接。

## 一言

上周的写的一篇文章分享到微信群里，群友有人指出来标题有错别字，真是有点尴尬！其实，我写文章也比较随意，也是最近才开始坚持每周至少输出一篇分享的。但想想既然写了，就要尽量保证质量。引以为戒，以后要认真点才行！

目前分享的内容主要是开发环境、效率工具等。自己一直对这方面的文章比较感兴趣，因此之前就有相关积累。今后希望能通过阅读带来一些读书总结的分享，也希望关注的同学后台多留言，给出你的建议，感谢~

> 生命不息，折腾不止！关注 「Coder魔法院」，祝你 Niubilitiy ！

## 参考

- 代码缺陷扫描神器——FindBugs：https://juejin.im/entry/591ad01ba22b9d005833903e

----

## 调试

[Java Stream Debugger](https://plugins.jetbrains.com/plugin/9696-java-stream-debugger) 可以查看stream中间操作的数据状态

---

[jclasslib](https://my.oschina.net/leon1314/blog/1817046) IDEA 如何使用jclasslib 插件查看 Java 字节码

---

VisualVM Launcher 对于经常实用 visualvm 进行性能调优和 debug 的场景，这个快速启动 visualvm 并打开正在调试的应用的按钮。

### 使用技巧

[Key Promoter X](https://plugins.jetbrains.com/plugin/9792-key-promoter-x) 快捷键提示

---

IDE Feature Trainer 安装之后，可以在 `View > Tool Windows > Learn` 里打开练习页面，了解 IDEA 的快捷键。


### 界面优化

Rainbow Brackets 彩虹括号，这个插件方便查看括号的匹配
- ctrl+鼠标右键点击 所在区域高亮
- alt+鼠标右键点击 非所选区域外不高亮

---

CodeGlance 可以在编辑区的右边增加一个类似 VSCode 的缩略图的效果，拖动比较方便。

### 辅助功能

[Statistic](https://plugins.jetbrains.com/plugin/4509-statistic) 代码统计

---

Maven Helper：安装之后，直接打开pom文件，即可查看依赖数，还能自动分析是否存在 jar 包冲突，查看[idea 中解决maven 包冲突的问题](https://blog.csdn.net/sunpeng_sp/article/details/77393348)

----

[GitToolBox](https://github.com/zielu/GitToolBox) Git 功能增强，比如可以在状态栏显示当前光标所在行的修改者

---

Restful Tookit：编写 RESTful 接口必不可少的插件，编写完接口当然还需要调试，可以搭配HTTP Client 一起使用

---

[Dash](https://zealdocs.org/): `ctrl+shift+h`

### 语言支持

BashSupport

### 花里胡哨

activate-power-mode：写代码的时候，就会附加一些狂拽炫酷屌炸天的效果

---

AceJump：允许您快速将光标导航到编辑器中可见的任何位置，只需点击`ctrl +;`，然后输入一个你想要跳转到的字符，之后键入匹配的字符就跳转到你想要挑战的地方了

## 参考

[那些提高效率的Idea插件](https://blog.imcompany.cn/post/na-xie-ti-gao-xiao-lu-de-ideacha-jian/)


