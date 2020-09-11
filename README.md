[![构建状态](/badges/t-spring-ec2/build.svg)](/p/t-spring-ec2/ci/job)

[TOC]

# 体验示例项目

这个示例代码通过发布一个简单的 Java 网页应用，带你体验代码功能模块如何与持续集成，构建物进行关联的。

文件解释
-----------

样例包括:

* README.md - 本文件
* Jenkinsfile - 用以自动构建和测试的脚本
* Dockerfile - 用以自动构建 Docker 镜像的脚本
* mvnw - Maven 的 wrapper 脚本，可以在没有安装 maven 的情况下自动安装 Maven
* mvnw.cmd - Windows 下用的 mvnw 脚本
* .mvw - Maven wrapper 需要的一些文件
* pom.xml - Maven 项目的基本描述和依赖配置
* src/main - 主 Java 源代码
* src/test - 主 Java 测试代码

快速开始
---------------

如下这些引导，假定你想在自己的电脑上开发本项目。

1. 构建项目.

        $ ./mvnw package

2. 把 war 包复制到 tomcat 目录下.

        $ cp target/ROOT.war <tomcat webapp directory>

4. 重启 Tomcat

5. 打开 http://127.0.0.1:8080/ .
