# 选择 JDK

Java development kits (JDK) 和运行时环境 (JRE) 在 [Scoop Java bucket](https://github.com/scoopinstaller/java) 中.

要添加 bucket, 运行:

```command line
scoop bucket add java
```

## OpenJDK

[OpenJDK](http://openjdk.java.net) 是首选的 JDK (因为它的开源 [许可证](http://openjdk.java.net/legal/gplv2+ce.html)).

Scoop Java bucket 包含 5 种不同 OpenJDK 构建.

### Oracle OpenJDK

Oracle 的 OpenJDK 版本 ([openjdk.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/openjdk.json)) 可用以下命令安装:

```command line
scoop install openjdk
```

### AdoptOpenJDK

[AdoptOpenJDK](https://adoptopenjdk.net) 有带 HotSpot 和 Eclipse OpenJ9 JVMs 的版本.

#### Oracle HotSpot JVM

##### 带 Oracle HotSpot JVM 的 OpenJDK 8 

[adopt8-upstream.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adopt8-upstream.json) 是首选的 Java 8 JDK. Oracle 不再发布 Oracle 8 JDK, 见 [FAQ](https://www.oracle.com/technetwork/java/javase/overview/oracle-jdk-faqs.html). Oracle 8 JRE 可通过 [oraclejre8.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/oraclejre8.json) manifest 安装.

##### 带 Oracle HotSpot JVM 的 AdoptOpenJDK 

安装 [adoptopenjdk-hotspot.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-hotspot.json):

```command line
scoop install adoptopenjdk-hotspot
```

##### 带 Oracle HotSpot JVM(运行时环境) 的 AdoptOpenJDK JRE

安装 [adoptopenjdk-hotspot-jre.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-hotspot-jre.json):

```command line
scoop install adoptopenjdk-hotspot-jre
```

#### Eclipse OpenJ9 JVM

##### 带 Eclipse OpenJ9 JVM 的 AdoptOpenJDK 

安装 [adoptopenjdk-openj9.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-openj9.json):

```
scoop install adoptopenjdk-openj9
```

##### 带 Eclipse OpenJ9 JVM(运行时环境) 的 AdoptOpenJDK JRE

安装 [adoptopenjdk-openj9-jre.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-openj9-jre.json):

```
scoop install adoptopenjdk-openj9-jre
```


### Zulu

- [Zulu](https://www.azul.com/products/zulu-and-zulu-enterprise)


### ojdkbuild

[ojdkbuild](https://github.com/ojdkbuild/ojdkbuild) manifests 需要安装 [lessmsi](https://github.com/ScoopInstaller/Main/blob/master/bucket/lessmsi.json) 且通过 `scoop config MSIEXTRACT_USE_LESSMSI true` 配置.

### Amazon Corretto

- [Amazon Corretto](https://aws.amazon.com/corretto)


## Oracle JDK

[Oracle’s Java](https://www.oracle.com/technetwork/java/index.html) 也在 [oraclejdk](https://github.com/ScoopInstaller/Java/blob/master/bucket/oraclejdk.json) manifest 中可用.


# 切换 Java

今天有两种解决方案可用于切换 java:

1. `scoop reset <java>[@<version>]`
2. 用 [extras](https://github.com/lukesampson/scoop-extras) 中的 [find-java](https://github.com/lukesampson/scoop-extras/blob/master/bucket/find-java.json)

`scoop reset` 非常适合当前会话, 并且还会更新用户路径. 请注意 https://github.com/lukesampson/scoop/issues/3822 - 目前这不能用于所有可用的软件包.

全局安装的 java 优先于用户安装的 java, 因此运行 `sudo scoop install -g oraclejdk-lts` 将安装一个始终默认用于新会话的 java.

## 版本切换示例

```command line
PS C:> scoop install oraclejdk
Installing 'oraclejdk' (12.0.2-10) [64bit]

PS C:> scoop install zulu6
Installing 'zulu6' (6.18.1.5) [64bit]

PS C:> scoop install openjdk10
Installing 'openjdk10' (10.0.1) [64bit]

PS C:> java -version
openjdk version "10.0.1" 2018-04-17
OpenJDK Runtime Environment (build 10.0.1+10)
OpenJDK 64-Bit Server VM (build 10.0.1+10, mixed mode)

PS C:> scoop reset zulu6
Resetting zulu6 (6.18.1.5).
Linking ~\scoop\apps\zulu6\current => ~\scoop\apps\zulu6\6.18.1.5

PS C:> java -version
openjdk version "1.6.0-99"
OpenJDK Runtime Environment (Zulu 6.18.1.5-win64) (build 1.6.0-99-b99)
OpenJDK 64-Bit Server VM (Zulu 6.18.1.5-win64) (build 23.77-b99, mixed mode)

PS C:> scoop reset oraclejdk

PS C:> java -version
java version "12.0.2" 2019-07-16
Java(TM) SE Runtime Environment (build 12.0.2+10)
Java HotSpot(TM) 64-Bit Server VM (build 12.0.2+10, mixed mode, sharing)
```
