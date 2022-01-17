# Choice of JDKs

Java development kits (JDK) and runtime environments (JRE) are available through the [Scoop Java bucket](https://github.com/scoopinstaller/java).

To add the bucket, run:
```
scoop bucket add java
```

## OpenJDK

[OpenJDK](http://openjdk.java.net) is the preferred JDK (because of its Open Source [license](http://openjdk.java.net/legal/gplv2+ce.html)).

The Scoop Java bucket contains five different OpenJDK builds.

### Oracle OpenJDK

Oracle's OpenJDK version ([openjdk.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/openjdk.json)) can be installed with:

```
scoop install openjdk
```

### AdoptOpenJDK

[AdoptOpenJDK](https://adoptopenjdk.net) has versions with HotSpot and Eclipse OpenJ9 JVMs.

#### Oracle HotSpot JVM

##### OpenJDK 8 with Oracle HotSpot JVM

[adopt8-upstream.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adopt8-upstream.json) is the preferred Java 8 JDK. Oracle does not distribute Oracle 8 JDK:s anymore, see FAQ [here](https://www.oracle.com/technetwork/java/javase/overview/oracle-jdk-faqs.html). Oracle 8 JRE is available through the [oraclejre8.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/oraclejre8.json) manifest.

##### AdoptOpenJDK with Oracle HotSpot JVM

[adoptopenjdk-hotspot.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-hotspot.json) can be installed with:

```
scoop install adoptopenjdk-hotspot
```

##### AdoptOpenJDK JRE with Oracle HotSpot JVM (runtime environment)

[adoptopenjdk-hotspot-jre.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-hotspot-jre.json) can be installed with:

```
scoop install adoptopenjdk-hotspot-jre
```

#### Eclipse OpenJ9 JVM

##### AdoptOpenJDK with Eclipse OpenJ9 JVM

[adoptopenjdk-openj9.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-openj9.json) can be installed with:

```
scoop install adoptopenjdk-openj9
```

##### AdoptOpenJDK JRE with Eclipse OpenJ9 JVM (runtime environment)

[adoptopenjdk-openj9-jre.json](https://github.com/ScoopInstaller/Java/blob/master/bucket/adoptopenjdk-openj9-jre.json) can be installed with:

```
scoop install adoptopenjdk-openj9-jre
```


### Zulu

- [Zulu](https://www.azul.com/products/zulu-and-zulu-enterprise)


### ojdkbuild

[ojdkbuild](https://github.com/ojdkbuild/ojdkbuild) manifests requires [lessmsi](https://github.com/ScoopInstaller/Main/blob/master/bucket/lessmsi.json) to be installed and configured by running `scoop config MSIEXTRACT_USE_LESSMSI true`.

### Amazon Corretto

- [Amazon Corretto](https://aws.amazon.com/corretto)


## Oracle JDK

[Oracle’s Java](https://www.oracle.com/technetwork/java/index.html) is also available in the [oraclejdk](https://github.com/ScoopInstaller/Java/blob/master/bucket/oraclejdk.json) manifest.


# Switching Javas

There are two solutions available today for switching java:

1. `scoop reset <java>[@<version>]`
2. Using [find-java](https://github.com/lukesampson/scoop-extras/blob/master/bucket/find-java.json) from [extras](https://github.com/lukesampson/scoop-extras)

`scoop reset` works very well for the current session, and will also update the user's path. Please note https://github.com/lukesampson/scoop/issues/3822 - currently this isn't working for all available packages.

Globally installed javas takes precedence over user-installed javas, so running `sudo scoop install -g oraclejdk-lts` will install a java that is always default for new sessions.


## Example of switching between versions

```
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
