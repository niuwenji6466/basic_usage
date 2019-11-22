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

\(4\) 第三步 保留demo-web的启动类 和 配置文件， 其他项目的启动类都删除， 整个项目只需要一个启动类和一个配置文件

demo-base:/demo/demo-base/src/main/java/demobase/demo/DemoBaseApplication.java   删除

```
                  /demo/demo-base/src/main/java/resource/\*   删除
```

demo-dao:/demo/demo-dao/src/main/java/demodao/demo/DemoDaoApplication.java   删除

```
                 /demo/demo-dao/src/main/java/resource/\*   删除
```

demo-service:/demo/demo-service/src/main/java/demoservice/demo/DemoServiceApplication.java   删除

```
                      /demo/demo-service/src/main/java/resource/\*   删除
```

![](/assets/20181014094228550.png)

![](/assets/20181014095129942.png)

\(5\)第四步 在 demo-dao 中添加 demo-base 的依赖信息， 在 demo-service 中添加 demo-dao 的依赖信息， 在 demo-web 中添加 demo-service 的依赖信息,  依赖信息添加到 各 pom 文件下的  &lt;dependencies&gt; &lt;/dependencies&gt;节点下

添加依赖信息后的 demo-dao 的依赖信息

```xml
<dependencies>

        <!-- 添加 demo-base 的依赖 -->
        <dependency>
            <groupId>demo</groupId>
            <artifactId>demo-base</artifactId>
            <version>0.0.1-SNAPSHOT</version>
            <scope>compile</scope>
        </dependency>

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
```

添加依赖信息后的 demo-service 的依赖信息

```xml
<dependencies>

        <!-- 添加 demo-dao 的依赖 -->
        <dependency>
            <groupId>demo</groupId>
            <artifactId>demo-dao</artifactId>
            <version>0.0.1-SNAPSHOT</version>
            <scope>compile</scope>
        </dependency>

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
```

添加依赖信息后的 demo-web 的依赖信息

```xml
<dependencies>

        <!-- 添加 demo-service 的依赖 -->
        <dependency>
            <groupId>demo</groupId>
            <artifactId>demo-service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
</dependencies>
```

\(6\)第五步 编写测试代码

编写好代码后三个模块的目录结构（demo-base还未用上， 但实际项目中很有用）：

![](/assets/20181014140918773.jpg)

下面贴上代码：

给demo-dao 添加

Demo.java

```java
package demo.demodao;

import javax.persistence.*;

@Entity(name = "demo")  //设置实体名， 在数据库中是表名
public class Demo {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)  //设置自动增长
    @Column(name = "id")
    private Integer id;


    @Column(name = "name") //设置数据库字段名    
    private String name;

    @Column(name = "id")
    private Integer id;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }
}
```

DemoRepository.java 

```java
package demo.demodao;
 
import org.springframework.data.jpa.repository.JpaRepository;
 
public interface DemoRepository extends JpaRepository<Demo, Integer> {
 
}
```

DemoService.java

```java

package demo.demoservice;
 
import demo.demodao.Demo;
 
import java.util.List;
 
 
public interface DemoService {
 
    Demo addOne(Demo demo);
 
}
```

DemoServiceImpl.java

```java

package demo.demoservice.impl;
 
import demo.demodao.Demo;
import demo.demodao.DemoRepository;
import demo.demoservice.DemoService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
 
@Service
public class DemoServiceImpl implements DemoService {
 
    @Autowired
    private DemoRepository demoRepository;
 
    @Override
    public Demo addOne(Demo demo) {
        return this.demoRepository.save(demo);
    }
}
```

DemoController.java  

```java

package demo.demoweb;
 
import demo.demodao.Demo;
import demo.demoservice.DemoService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;
 
@Controller
public class DemoController {
 
    @Autowired
    private DemoService demoService;
 
    @ResponseBody  // 返回 Json 数据
    @GetMapping("add")
    private Demo add(){
        Demo demo = new Demo();
        demo.setName("姓名");
        demo.setId(1);
        return demoService.addOne(demo); // 成功返回 保存后的 demo
    }
 

}
```

第六步： 创建数据库， 修改配置文件

 创建输数据库 test

![](/assets/20181014141543318.png)

 修改配置文件： 将 demo-web Resource目录下 application.properties 文件名改为 applicatin.yml并添加以下内容：

```
spring:
  datasource:
    # jdbc:mysql://localhost:3306/test 数据库地址
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8&useSSL=false
    username: root # 数据库用户名
    password: xxxxxx  # 数据库密码
    driver-class-name: com.mysql.jdbc.Driver  # 数据库驱动
 
  jpa:
      hibernate:
        ddl-auto: create-drop  #  create-drop  如果实体对应的表已存在，先删除再创建，否则直接创建
        #  ！！！注意： 第一次运行时可设置为 create-drop, 这样就不需要手动创建数据库表, 但是后面运行务必设置为none
```

![](/assets/20181014153348119.png)

\(7\) 第七步： 大功告成， 运行项目

![](/assets/20181014144051432.png)

直接运行会报错

![](/assets/20181014144300454.png)

### **已解决：将启动类 DemoApplication 移动到 demo 包下**



![](/assets/20181014145602218.png)

移动：



![](/assets/QQ截图20191122134712.png)

![](/assets/QQ截图20191122134723.png)

![](/assets/QQ截图20191122134734.png)

### **运行成功！**

![](/assets/QQ截图20191122134936.png)

