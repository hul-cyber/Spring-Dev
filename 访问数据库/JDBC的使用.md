# 使用JDBC
我们在前面介绍JDBC编程时已经讲过，Java程序使用JDBC接口访问关系数据库的时候，需要以下几步：
创建全局`DataSource`实例，表示数据库连接池；
在需要读写数据库的方法内部，按如下步骤访问数据库：
    从全局`DataSource`实例获取`Connection`实例；
    通过`Connection`实例创建`PreparedStatement`实例；
    执行SQL语句，如果是查询，则通过`ResultSet`读取结果集，如果是修改，则获得int结果。
正确编写JDBC代码的关键是使用`try ... finally`释放资源，涉及到事务的代码需要正确提交或回滚事务。
在Spring使用JDBC，首先我们通过IoC容器创建并管理一个`DataSource`实例，然后，Spring提供了一个`JdbcTemplate`，可以方便地让我们操作JDBC，因此，通常情况下，我们会实例化一个JdbcTemplate。顾名思义，这个类主要使用了Template模式。
编写示例代码或者测试代码时，我们强烈推荐使用HSQLDB这个数据库，它是一个用Java编写的关系数据库，可以以内存模式或者文件模式运行，本身只有一个jar包，非常适合演示代码或者测试代码。
我们以实际工程为为例，先创建Maven工程spring-data-jdbc，然后引入以下依赖：
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.0.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.0.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>javax.annotation</groupId>
        <artifactId>javax.annotation-api</artifactId>
        <version>1.3.2</version>
    </dependency>
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>3.4.2</version>
    </dependency>
    <dependency>
        <groupId>org.hsqldb</groupId>
        <artifactId>hsqldb</artifactId>
        <version>2.5.0</version>
    </dependency>
</dependencies>
```
在AppConfig中，我们需要创建以下几个必须的Bean：
```java
@Configuration
@ComponentScan
@PropertySource("jdbc.properties")
public class AppConfig {

    @Value("${jdbc.url}")
    String jdbcUrl;

    @Value("${jdbc.username}")
    String jdbcUsername;

    @Value("${jdbc.password}")
    String jdbcPassword;

    @Bean
    DataSource createDataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl(jdbcUrl);
        config.setUsername(jdbcUsername);
        config.setPassword(jdbcPassword);
        config.addDataSourceProperty("autoCommit", "true");
        config.addDataSourceProperty("connectionTimeout", "5");
        config.addDataSourceProperty("idleTimeout", "60");
        return new HikariDataSource(config);
    }

    @Bean
    JdbcTemplate createJdbcTemplate(@Autowired DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```
在上述配置中：
1. 通过`@PropertySource("jdbc.properties")`读取数据库配置文件；
2. 通过`@Value("${jdbc.url}")`注入配置文件的相关配置；
3. 创建一个DataSource实例，它的实际类型是`HikariDataSource`，创建时需要用到注入的配置；
4. 创建一个JdbcTemplate实例，它需要注入`DataSource`，这是通过方法参数完成注入的。

最后，针对HSQLDB写一个配置文件`jdbc.properties`：
```
# 数据库文件名为testdb:
jdbc.url=jdbc:hsqldb:file:testdb

# Hsqldb默认的用户名是sa，口令是空字符串:
jdbc.username=sa
jdbc.password=
```
可以通过HSQLDB自带的工具来初始化数据库表，这里我们写一个Bean，在Spring容器启动时自动创建一个`users`表：
```java
@Component
public class DatabaseInitializer {
    @Autowired
    JdbcTemplate jdbcTemplate;

    @PostConstruct
    public void init() {
        jdbcTemplate.update("CREATE TABLE IF NOT EXISTS users (" //
                + "id BIGINT IDENTITY NOT NULL PRIMARY KEY, " //
                + "email VARCHAR(100) NOT NULL, " //
                + "password VARCHAR(100) NOT NULL, " //
                + "name VARCHAR(100) NOT NULL, " //
                + "UNIQUE (email))");
    }
}
```
现在，所有准备工作都已完毕。我们只需要在需要访问数据库的Bean中，注入`JdbcTemplate`即可：
```java
@Component
public class UserService {
    @Autowired
    JdbcTemplate jdbcTemplate;
    ...
}
```
## JdbcTemplate用法
Spring提供的`JdbcTemplate`采用Template模式，提供了一系列以回调为特点的工具方法，目的是避免繁琐的`try...catch`语句。
我们以具体的示例来说明JdbcTemplate的用法。
首先我们看`T execute(ConnectionCallback<T> action)`方法，它提供了Jdbc的`Connection`供我们使用：
```java
public User getUserById(long id) {
    // 注意传入的是ConnectionCallback:
    return jdbcTemplate.execute((Connection conn) -> {
        // 可以直接使用conn实例，不要释放它，回调结束后JdbcTemplate自动释放:
        // 在内部手动创建的PreparedStatement、ResultSet必须用try(...)释放:
        try (var ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?")) {
            ps.setObject(1, id);
            try (var rs = ps.executeQuery()) {
                if (rs.next()) {
                    return new User( // new User object:
                            rs.getLong("id"), // id
                            rs.getString("email"), // email
                            rs.getString("password"), // password
                            rs.getString("name")); // name
                }
                throw new RuntimeException("user not found by id.");
            }
        }
    });
}
```