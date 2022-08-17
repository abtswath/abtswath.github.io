---
title: openjdk 8 maven 下载项目依赖时遇到的问题
date: 2021-07-27 17:04:55.561
updated: 2021-07-27 20:51:54.717
url: maven-invalid-algorithm-parameter-exception
categories: 
- Java
tags: 
- Java
- Maven
- openjdk
- openjdk 8
---

### 错误信息
```
[ERROR] Plugin org.apache.maven.plugins:maven-surefire-plugin:2.22.2 or one of its dependencies could not be resolved: Failed to read artifact descriptor for org.apache.maven.plugins:maven-surefire-plugin:jar:2.22.2: Could not transfer artifact org.apache.maven.surefire:surefire:pom:2.22.2 from/to aliyunmaven (https://maven.aliyun.com/repository/public): transfer failed for https://maven.aliyun.com/repository/public/org/apache/maven/surefire/surefire/2.22.2/surefire-2.22.2.pom: java.lang.RuntimeException: Unexpected error: java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty -> [Help 1]
```
##### 截图如下
![image.png](https://image.zyfyf.com/halo/3be29ad2ffd08db43750cfce02019fdd.png)
### 问题原因及解决方案
报错提示大致是https证书安全检查问题，以下是搜索引擎加自己尝试得出的一些解决方案：
#### 1. 使用 http 的镜像源，例如：
```xml
<mirror>
	<id>aliyunmaven</id>
	<mirrorOf>*</mirrorOf>
	<name>阿里云公共仓库</name>
	<url>http://maven.aliyun.com/repository/public</url>
</mirror>
```
此方案确实可以解决一部分问题，但还是有些 jar 包会出现同样的错误，具体原因未深究。
#### 2. 生成证书并导入到 jre security 中
首先到处 https 证书，然后使用 keytool 导入证书：
```bash
$ cd <JAVA_HOME>\jre\lib\security
$ keytool -import -alias aliyun-maven -keystore cacerts -file path-to-aliyun-maven.cer
```
证书指纹`changeit`，输入 Y 表示确认。
重新打开 cmd 窗口，再次尝试安装依赖。
如果还是不行报错，参考[https://www.techpaste.com/2017/03/trustanchors-parameter-must-non-empty/](https://www.techpaste.com/2017/03/trustanchors-parameter-must-non-empty/)
#### 3. 替换 jdk 的 cacerts 文件
使用 openjdk 16 中的 cacerts（<JAVA_HOME>\jre\lib\security\cacerts） 文件替换 openjdk 8 中 cacerts 文件，替换前记得备份。

我使用的是方案三，目前没发现任何问题。
