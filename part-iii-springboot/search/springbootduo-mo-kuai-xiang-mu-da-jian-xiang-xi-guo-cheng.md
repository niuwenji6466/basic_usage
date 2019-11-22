1. 项目介绍:

本项目包含一个父工程 demo  和 四 个子模块（demo-base, demo-dao, demo-service, demo-web\)， demo-base 为其他三个模块的公共内容， 四个模块都依赖父模块， demo-dao 依赖 demo-base；   demo-service 依赖 demo-dao, 间接依赖 demo-base;   demo-web 依赖 demo-service, 间接依赖demo-base和demo-dao

![](/assets/20181013204132335.png)

-

2.搭建思路：先创建一个 Spring Initializr工程 demo 作为 父工程， 然后在父工程再建四个子 Module \(demo-base, demo-demo-dao, demo-service\), 其实他们就是四个普通的Spring Initializr工程。

3.开始搭建：

首先，创建一个Spring Initializr项目 和 子Module

![](/assets/QQ截图20191122093825.png)

![](/assets/QQ截图20191122093913.png)

![](/assets/QQ截图20191122093932.png)

![](/assets/QQ截图20191122094256.png)

![](/assets/QQ截图20191122094038.png)

![](/assets/QQ截图20191122094053.png)

注意：修改demo 的 pom 文件中的打包方式为 pom

![](/assets/20181014134236477.png)

## 刚才演示创建成一个SpringBoot 工程， 下面正式开始演示搭建多模块

\(1\) 第一步， 删除刚才创建工程里的文件， 只保留\(.idea文件夹 , 和项目 pom 文件, 以及一个 \*.iml 文件 \)

![](/assets/20181013174108323.png)

![](/assets/2018101317431252.png)

（2）第二步， 创建子 Module （demo-base， demo-dao,  demo-service 和 demo-web）  先创建demo-base子工程

![](/assets/QQ截图20191122095018.png)

![](/assets/QQ截图20191122095033.png)

![](/assets/QQ截图20191122095050.png)

![](/assets/QQ截图20191122095228.png)

![](/assets/QQ截图20191122095240.png)创建好了 demo-base 子项目 ， 为子工程 demo-base 生命父工程以及 为 父工程声明子 Module（在 demo 和 demo-base 的 pom 文件中添加如下代码）

在 demo-base 中 声明父工程， 注意：此时demo-base继承的是 SpringBoot提供的父工程， 需要修改&lt;parent&gt;&lt;/parent&gt;中的版本信息， 修改成父项目 demo 的版本信息（直接将父项目 demo 的pom文件 中的版本信息复制粘贴到 mode-base中即可）

![](/assets/20181014083110458.jpg)--&gt;声明父工程

```xml
<parent>
        <groupId>demo</groupId>
        <artifactId>demo</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>
```

--&gt;声明子 Module

```xml
<modules>
         <module>demo-base</module>
</modules>
```

修改后的父工程 demo 的 pom 文件:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 父项目 demo 的版本信息 -->
    <groupId>demo</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>demo</name>
    <description>Demo project for Spring Boot</description>

    <!-- 继承说明：这里继承SpringBoot提供的父工程 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.16.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <!-- 声明子模块 -->
    <modules>
        <module>demo-base</module>
    </modules>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
 </project>
```

修改后的 demo-base 的 pom 文件 :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--  demo-base 的版本信息 -->
    <groupId>demo</groupId>
    <artifactId>demo-base</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>demo-base</name>
    <description>Demo project for Spring Boot</description>

    <!-- 声明父项目 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.16.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


</project>
```

创建demo-dao, demo-service  创建方法一样， 这里只演示demo-dao的创建:

![](/assets/QQ截图20191122101116.png)![](/assets/QQ截图20191122101128.png)![](/assets/QQ截图20191122101139.png)![](/assets/QQ截图20191122101151.png)

![](/assets/QQ截图20191122101204.png)

![](/assets/QQ截图20191122101214.png)

创建demo-web 模块与demo-dao创建除了第4步， 完全相同需要 添加 web依赖， 在创建过程的第4步需要添加 web 依赖

![](/assets/20181014085229751.png)

