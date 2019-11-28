# 

## 1、创建一个新springboot项目，引入相应的jar，pom文件如下 {#h2_1}

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.quick</groupId>
    <artifactId>quick-moredatasource</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>quick-moredatasource</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.3.RELEASE</version>
        <relativePath/>
    </parent>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.8</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.31</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.0</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.33</version>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.9</version>
            <optional>true</optional>
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

## 2、编写数据源的配置参数，我们以两个数据源为例 {#h2_2}

```
spring:
  datasource:
    master:
      url: jdbc:mysql://127.0.0.1:3306/mytest0?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&failOverReadOnly=false&allowMultiQueries=true&useSSL=false
      username: root
      password: root
    slave:
      url: jdbc:mysql://127.0.0.1:3306/mytest1?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&failOverReadOnly=false&allowMultiQueries=true&useSSL=false
      username: root
      password: root
```

## 3、编写数据源配置对象，MasterDataSourceConfig ，SlaveDataSourceConfig {#h2_3}

```
@Configuration
@EnableConfigurationProperties
public class MasterDataSourceConfig {
    @Bean(name = "masterDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.master")
    @Primary
    public DataSource masterDataSource() {
        DruidDataSource masterDataSource = new DruidDataSource();
        masterDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        masterDataSource.setTestWhileIdle(true);
        masterDataSource.setTestOnBorrow(false);
        masterDataSource.setTestOnReturn(false);
        masterDataSource.setMaxWait(15000);
        masterDataSource.setTimeBetweenEvictionRunsMillis(60000);
        masterDataSource.setPoolPreparedStatements(false);
        masterDataSource.setValidationQuery("select 1 ");
        masterDataSource.setInitialSize(20);
        masterDataSource.setMaxActive(80);
        return masterDataSource;
    }

    @Bean(name = "masterSqlSessionFactory")
    @Primary
    public SqlSessionFactory masterSqlSessionFactory(@Qualifier("masterDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/master/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "masterTransactionManager")
    @Primary
    public DataSourceTransactionManager masterTransactionManager(@Qualifier("masterDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean(name = "masterSqlSessionTemplate")
    @Primary
    public SqlSessionTemplate masterSqlSessionTemplate(@Qualifier("masterSqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }
}
```

```
@Configuration
@EnableConfigurationProperties
public class SlaveDataSourceConfig {
    @Bean(name = "slaveDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.slave")
    public DataSource slaveDataSource() {
        DruidDataSource queryDataSource = new DruidDataSource();
        queryDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        queryDataSource.setTestWhileIdle(true);
        queryDataSource.setTestOnReturn(false);
        queryDataSource.setTestOnBorrow(false);
        queryDataSource.setMaxWait(15000);
        queryDataSource.setTimeBetweenEvictionRunsMillis(60000);
        queryDataSource.setPoolPreparedStatements(false);
        queryDataSource.setValidationQuery("select 1 ");
        queryDataSource.setMaxActive(80);
        queryDataSource.setInitialSize(20);
        return queryDataSource;
    }

    @Bean(name = "slaveSqlSessionFactory")
    public SqlSessionFactory slaveSqlSessionFactory(@Qualifier("slaveDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/master/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "slaveTransactionManager")
    public DataSourceTransactionManager slaveTransactionManager(@Qualifier("slaveDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean(name = "slaveSqlSessionTemplate")
    public SqlSessionTemplate slaveSqlSessionTemplate(@Qualifier("slaveSqlSessionFactory") SqlSessionFactory sqlSessionFactory) {
        return new SqlSessionTemplate(sqlSessionFactory);
    }
}
```

## 4、创建domain {#h2_4}

```
@Data
public class Book {
    private Integer id;
    private String bookName;
    private String author;
    private String isbn;
}
```

## 5、编写mapper.xml在resources文件夹下创建文件夹/mapper/master/ {#h2_5}

#### 创建名为`BookMasterMapper.xml`的mapper文件，内容如下： {#h4_7}

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="Book">
    <sql id="Query_Column">
        id as id,
        book_name as bookName,
        author as author,
        isbn as isbn
    </sql>
    <insert id="save" parameterType="com.quick.domain.Book">
        insert into book (book_name, author,isbn)
        values (#{bookName,jdbcType=VARCHAR},
         #{author,jdbcType=VARCHAR},
        #{isbn,jdbcType=VARCHAR})
    </insert>
</mapper>
```

## 6、编写对应的mapper类 {#h2_8}

```
@Repository
public class BookMasterMapper {
    @Resource(name = "masterSqlSessionTemplate")
    private SqlSessionTemplate sqlClient;

    public long save(Book book) {
        return sqlClient.insert("Book.save",book);
    }
}
```

```
@Repository
public class BookSlaveMapper {
    @Resource(name = "slaveSqlSessionTemplate")
    private SqlSessionTemplate sqlClient;

    public long save(Book book) {
        return sqlClient.insert("Book.save",book);
    }
}
```

## 7、ok编写测试service {#h2_9}

```
@Service
public class BookService {
    @Resource
    private BookMasterMapper bookMasterMapper;
    @Resource
    private BookSlaveMapper bookSlaveMapper;
    public void save(Book book){
        bookMasterMapper.save(book);
        bookSlaveMapper.save(book);
    }
}
```

#### 注入不同数据源的mapper对象。 {#h4_10}

## 8、启动类编写 {#h2_11}

```
@SpringBootApplication(exclude = {MybatisAutoConfiguration.class})
public class AppStart {

	public static void main(String[] args) {
		SpringApplication.run(AppStart.class, args);
	}
}
```

#### 此处需要去掉mybatis的自动配置 {#h4_12}

## 9、编写单元测试类 {#h2_13}

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class QuickMoredatasourceApplicationTests {
	@Resource
	private BookService bookService;
	@Test
	public void contextLoads() {
		Book book = new Book();
		book.setAuthor("wang");
		book.setBookName("boot");
		book.setIsbn("1122");
		bookService.save(book);
	}
}
```

## 10、执行完成看一下数据库数据 {#h2_14}



