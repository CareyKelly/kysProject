# maven的用处

maven是自动化构建的工具

+ 项目的自动构建，帮助开发人员打包，测试，安装，部署

  手动的去重复编译打包部署，耗费大量时间，利用maven能够自动化处理这些重复的工作

+ 管理项目的依赖，依赖是项目中使用到的其他资源，常见的形式有jar包

  依赖的获取和依赖的版本管理，传统方式是从网站中下载下来然后导到项目中，切换版本时又重新去下载，替换，很麻烦。jar包之间有时候会有依赖，有时会不清楚之间的依赖关系。利用maven来统一管理这些依赖，只需要调整配置文件，配置镜像源，就可以轻松获取不同版本的依赖
  
+ Snapshot （快照）版本代表不稳定、尚处于开发中的版本。每次maven都会自动获取最新的快照

+ Release 版本则代表稳定的版本



# 仓库

仓库是maven存放依赖（jar包）的地方，maven仓库分为三种

+ **本地：**创建在本地的仓库，第一次执行maven命令时才会创建，默认位置为 `user/.m2/repository`
+ **中央：**maven社区管理的仓库，包含了绝大部分开源的依赖
+ **远程：**开发人员或组织自己定义的仓库，存放自己的依赖和代码库

依赖搜索顺序：

```mermaid
graph LR
	i1[本地仓库]--not_found-->i2[中央仓库]--not_found-->i3[远程仓库]
	i3--download-->i1
	i2--download-->i1
```



# 生命周期和插件

### 生命周期

生命周期是对项目的构建和发布过程的描述，主要包含了以下几个**阶段**

+ 项目验证
+ 编译
+ 测试
+ 打包
+ 验证
+ 安装
+ 部署

各阶段按照**顺序执行**，每个阶段都会依赖前一个阶段的完成



maven中存在三种生命周期

+ clean
+ default：validate --> compile --> test --> package --> verify --> install --> deploy
+ site

![生命周期各阶段](D:\learn\kysProject\notes\Maven\images\生命周期各阶段.jpg)



### 插件

插件简单来说就是一些jar包，通过插件来实现对maven项目的一系列构建工作

+ 一个插件可以实现多种功能，这些功能称为**目标**。例如，maven-compiler-plugin支持 compile（编译源代码） 和 testCompile（编译测试代码）

+ 生命周期中的阶段与目标相互绑定，这样当该阶段需要执行的时候，就调用插件来完成绑定的目标

+ 每个**阶段**可以绑定**多个目标**

+ 每个**插件**可以提供**多个目标**

  > 下面以打包类型是jar包为例
  >
  > ![image-20210829182836807](C:\Users\kelly\AppData\Roaming\Typora\typora-user-images\image-20210829182836807.png)



# 构建项目时约定的目录结构

```
project
	/src
		/main
			/java				源码
			/resources			配置文件
			/webapp				web项目的源码
		/test
			/java				测试代码
			/resources			测试相关配置文件
	/pom.xml					maven项目核心配置文件
```



# pom(Project Object Model)文件

pom文件示例（idea搭建的一个简单的web项目）

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>mavenWeb</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>mavenWeb Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>mavenWeb</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```



### 坐标（gav）

用来唯一标识项目或构件的，包含三个标签

+ groupId      组织机构/顶层项目名
+ artifactId    项目/库名称
+ version       版本号
+ package     打包类型（jar，war，rar，ear，pom）

```xml
  <!-- 模型版本，现在一般为4.0 -->
  <modelVersion>4.0.0</modelVersion>

  <!-- 组织机构/项目名称，一般为组织域名倒过来 -->
  <groupId>org.example</groupId>
  <!-- 项目名称 -->
  <artifactId>mavenWeb</artifactId>
  <!-- 版本 -->
  <version>1.0-SNAPSHOT</version>
  <!-- 打包类型 -->
  <packaging>war</packaging>
```



### 配置属性

+ 定义一些配置属性，例如将源码的编码方式定义为UTF-8
+ 定义全局变量`<valueName>value</valueName>`，后面用`${valueName}`访问，例如定义版本号，方便统一管理版本

```xml
<properties>
    <!-- 编码方式 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- 源码编译时JDK版本 -->
    <maven.compiler.source>1.7</maven.compiler.source>
    <!-- 运行时JDK版本 -->
    <maven.compiler.target>1.7</maven.compiler.target>
    <!-- 自定义全局变量 -->
    <junit.version>4.11</junit.version>
  </properties>
```



### 依赖管理

+ dependencies
  + dependency
    + gav		依赖的唯一标识
    + scope    依赖范围

```xml
<dependencies>
    <!-- 一个dependency标签对应一个依赖 -->
    <dependency>
      <!-- 依赖的唯一标识 -->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <!-- 依赖范围 -->
      <scope>test</scope>
    </dependency>
  </dependencies>
```



依赖范围主要有以下三种

1. **compile** : 编译依赖范围，scope元素的缺省值。依赖在编译、测试、运行过程都会被引入

2. **test** : 测试依赖范围。依赖会在测试过程引入（打包的时候不会将这个依赖一起打包）

3. **provided** : 已提供依赖范围。使用此依赖范围的Maven依赖，在编译，测试阶段被引入，但是运行阶段，由于外部容器已经提供该依赖，为了防止依赖冲突，故不需要Maven重复引入该依赖（打包的时候不会将这个依赖一起打包）



依赖调解（间接依赖的版本冲突时）原则

- 第一原则: 路径最短者优先
- 第二原则: 第一声明者优先



### 构建

与构建有关的相关配置都在这

+ build

  + resources    资源路径 
    + resource
      + 指定的路径

  + pluginManagement    插件管理
    + plugins
      + plugin
        + gav
        + 其他配置

```xml
<build>
    <finalName>mavenWeb</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
```



### 继承

### 聚合



# 命令

### mvn clean

清理maven项目

+ 删除 `target` 目录

+ 调用插件 ` maven-clean-plugin` 

  控制台输出如下：

  > 贴控制台输出是为了更好的观察每一个命令调用了那些插件，进行了哪些操作，如不需要可跳过
  
  ```
  D:\code\mavenWeb>mvn clean
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ------------------------< org.example:mavenWeb >------------------------
  [INFO] Building mavenWeb Maven Webapp 1.0-SNAPSHOT
  [INFO] --------------------------------[ war ]---------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:3.1.0:clean (default-clean) @ mavenWeb ---
  [INFO] Deleting D:\code\mavenWeb\target
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  0.338 s
  [INFO] Finished at: 2021-08-22T17:31:52+08:00
  [INFO] ------------------------------------------------------------------------
  ```



### mvn compile

编译maven项目

+ 将 `src/main/java` 目录下的 `.java` 文件编译成 `.class` 文件放在 `target/classes` 目录下

+ 将 `src/main/sources` 目录下的资源文件拷贝到  `target/classes` 目录下（不改变目录结构）

+ 调用插件 `maven-resources-plugin` 拷贝资源文件到目录下，调用 `maven-compiler-plugin`  编译项目

  控制台输出如下

  ```
  D:\code\mavenWeb>mvn compile
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ------------------------< org.example:mavenWeb >------------------------
  [INFO] Building mavenWeb Maven Webapp 1.0-SNAPSHOT
  [INFO] --------------------------------[ war ]---------------------------------
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavenWeb ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\code\mavenWeb\target\classes
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  1.267 s
  [INFO] Finished at: 2021-08-22T17:25:46+08:00
  [INFO] ------------------------------------------------------------------------
  ```

  

### mvn test

测试maven项目

+ 编译所有的源代码

+ 编译所有的测试代码

+ 调用插件 `maven-surefire-plugin`  运行所有的测试

  控制台输出如下：

  ```
  D:\code\mavenWeb>mvn test
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ------------------------< org.example:mavenWeb >------------------------
  [INFO] Building mavenWeb Maven Webapp 1.0-SNAPSHOT
  [INFO] --------------------------------[ war ]---------------------------------
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavenWeb ---
  [INFO] Nothing to compile - all classes are up to date
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\code\mavenWeb\src\test\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ mavenWeb ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\code\mavenWeb\target\test-classes
  [INFO]
  [INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ mavenWeb ---
  [INFO]
  [INFO] -------------------------------------------------------
  [INFO]  T E S T S
  [INFO] -------------------------------------------------------
  [INFO] Running TestHelloMaven
  testName1 kelly
  testName2 Carey
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.057 s - in TestHelloMaven
  [INFO]
  [INFO] Results:
  [INFO]
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
  [INFO]
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  3.459 s
  [INFO] Finished at: 2021-08-22T20:07:01+08:00
  [INFO] ------------------------------------------------------------------------
  ```



### mvn package

打包maven项目

+ 打包类型 jar，war，rar，ear，pom，默认是jar

+ 打包的文件放在 `target` 目录下

+ 先编译，测试，再调用插件 `maven-war-plugin` 进行打包 

  控制台输出如下：

  ```
  D:\code\mavenWeb>mvn package
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ------------------------< org.example:mavenWeb >------------------------
  [INFO] Building mavenWeb Maven Webapp 1.0-SNAPSHOT
  [INFO] --------------------------------[ war ]---------------------------------
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavenWeb ---
  [INFO] Nothing to compile - all classes are up to date
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\code\mavenWeb\src\test\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ mavenWeb ---
  [INFO] Nothing to compile - all classes are up to date
  [INFO]
  [INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ mavenWeb ---
  [INFO]
  [INFO] -------------------------------------------------------
  [INFO]  T E S T S
  [INFO] -------------------------------------------------------
  [INFO] Running TestHelloMaven
  testName1 kelly
  testName2 Carey
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.052 s - in TestHelloMaven
  [INFO]
  [INFO] Results:
  [INFO]
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
  [INFO]
  [INFO]
  [INFO] --- maven-war-plugin:3.2.2:war (default-war) @ mavenWeb ---
  [INFO] Packaging webapp
  [INFO] Assembling webapp [mavenWeb] in [D:\code\mavenWeb\target\mavenWeb]
  [INFO] Processing war project
  [INFO] Copying webapp resources [D:\code\mavenWeb\src\main\webapp]
  [INFO] Webapp assembled in [48 msecs]
  [INFO] Building war: D:\code\mavenWeb\target\mavenWeb.war
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  3.676 s
  [INFO] Finished at: 2021-08-22T22:30:54+08:00
  [INFO] ------------------------------------------------------------------------
  ```

  

### mvn install

发布maven项目

+ 打包发布maven项目到仓库，供其他项目使用

+ 发布目录 `${localRepository}/groupId/artifactId/version/artifactId-version.war` 

  > ps：如果groupId中有 `.`，则创建两级目录。例如 **<groupId>org.example</groupId> **则发布路径：${localRepository}/**org/exmple**/artifactId/version/artifactId-version.war

+ 先编译，测试，打包，再调用插件 `` 进行发布

  控制台输出如下

  ```
  D:\code\mavenWeb>mvn install
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ------------------------< org.example:mavenWeb >------------------------
  [INFO] Building mavenWeb Maven Webapp 1.0-SNAPSHOT
  [INFO] --------------------------------[ war ]---------------------------------
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavenWeb ---
  [INFO] Nothing to compile - all classes are up to date
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ mavenWeb ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\code\mavenWeb\src\test\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ mavenWeb ---
  [INFO] Nothing to compile - all classes are up to date
  [INFO]
  [INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ mavenWeb ---
  [INFO]
  [INFO] -------------------------------------------------------
  [INFO]  T E S T S
  [INFO] -------------------------------------------------------
  [INFO] Running TestHelloMaven
  testName1 kelly
  testName2 Carey
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.063 s - in TestHelloMaven
  [INFO]
  [INFO] Results:
  [INFO]
  [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
  [INFO]
  [INFO]
  [INFO] --- maven-war-plugin:3.2.2:war (default-war) @ mavenWeb ---
  [INFO] Packaging webapp
  [INFO] Assembling webapp [mavenWeb] in [D:\code\mavenWeb\target\mavenWeb]
  [INFO] Processing war project
  [INFO] Copying webapp resources [D:\code\mavenWeb\src\main\webapp]
  [INFO] Webapp assembled in [52 msecs]
  [INFO] Building war: D:\code\mavenWeb\target\mavenWeb.war
  [INFO]
  [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ mavenWeb ---
  [INFO] Installing D:\code\mavenWeb\target\mavenWeb.war to D:\software\repository\org\example\mavenWeb\1.0-SNAPSHOT\mavenWeb-1.0-SNAPSHOT.war
  [INFO] Installing D:\code\mavenWeb\pom.xml to D:\software\repository\org\example\mavenWeb\1.0-SNAPSHOT\mavenWeb-1.0-SNAPSHOT.pom
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  3.808 s
  [INFO] Finished at: 2021-08-22T23:16:46+08:00
  [INFO] ------------------------------------------------------------------------
  ```








