# 	1、介绍

MyBatis-Plus是一个 MyBatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

官方地址：https://mp.baomidou.com/

## 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

# 2、快速入门

1. 创建数据库`mybatis_plus`

2. 创建`user`表，并插入数据

   ```sql
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
   	id BIGINT(20) NOT NULL COMMENT '主键ID',
   	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
   	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
   	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
   	PRIMARY KEY (id)
   );
   
   DELETE FROM user;
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

3. 创建springboot项目`mybatisplus01_helloworld`

4. 导入依赖

   ```xml
   <!-- mysql驱动 -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
   </dependency>
   <!-- lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
   </dependency>
   <!-- mybatis-plus-boot-starter -->
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.0.5</version>
   </dependency>
   ```

5. 连接数据库，与mybatis一样

   ```properties
   spring.datasource.username=root
   spring.datasource.password=123456
   spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   ```

6. pojo

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private Long id;
       private String name;
       private Integer age;
       private String email;
   }
   ```

7. mapper接口

   ```java
   // 使用mybatis——puls 在对应的Mapper上继承基本的接口BaseMapper
   @Repository   //代表持久层
   public interface UserMapper extends BaseMapper<User> {
   
       /*
        * 继承BaseMapper，所以的CRUD就已经可以使用
        */
   
   }
   ```

8. 在主启动上添加注解，扫描mapper包

   ```java
   //扫描mapper
   @MapperScan("com.zh.mapper")
   @SpringBootApplication
   public class Mybatisplus01HelloworldApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(Mybatisplus01HelloworldApplication.class, args);
       }
   
   }
   ```

9. 测试

   ```java
   @SpringBootTest
   class Mybatisplus01HelloworldApplicationTests {
   
       @Autowired
       private UserMapper mapper;
   
       @Test
       void contextLoads() {
           // 参数是一个Wrapper，条件构造器，使用null
           // 查询全部
           List<User> list = mapper.selectList(null);
           list.forEach(System.out::println);
       }
   
   }
   ```

![image-20200501222522282](http://image.beloved.ink/Typora/image-20200501222522282.png)

# 3、配置日志

![image-20200501223149230](http://image.beloved.ink/Typora/image-20200501223149230.png)

```properties
# 配置日志  使用控制台输出
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![image-20200501223402285](http://image.beloved.ink/Typora/image-20200501223402285.png)

# 4、CRUD扩展

## 4.1、插入

```java
@Test
public void insert(){
    User user = new User();
    user.setName("Beloved");
    user.setAge(20);
    user.setEmail("1425279634@qq.com");

    int res = mapper.insert(user);  //自动添加id
    System.out.println(res);        //受影响的行数
    System.out.println(user);       
}
```

![image-20200501224134929](http://image.beloved.ink/Typora/image-20200501224134929.png)

> 数据库插入的id默认值为：全局唯一id

## 4.2、主键生成策略

在pojo中的id字段修改主键生成策略

![image-20200501230448932](http://image.beloved.ink/Typora/image-20200501230448932.png)

![image-20200501230609625](http://image.beloved.ink/Typora/image-20200501230609625.png)

### 4.2.1、默认   ID_WORKER

分布式唯一id生成方案：https://www.cnblogs.com/haoxinyue/p/5208136.html

**雪花算法：**

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。具体实现的代码可以参看https://github.com/twitter/snowflake。

### 4.2.2、主键自增

配置主键自增：

1. 在pojo实体类id字段配置`@TableId(type = IdType.AUTO)`

2. 设置数据库字段为自增

   ![image-20200501231050412](http://image.beloved.ink/Typora/image-20200501231050412.png)

3. 测试

   ![image-20200501231212785](http://image.beloved.ink/Typora/image-20200501231212785.png)

![image-20200501231238145](http://image.beloved.ink/Typora/image-20200501231238145.png)

## 4.3、更新

```java
@Test
public void update(){
    User user = new User();
    user.setId(1L);
    user.setName("Beloved");
    user.setEmail("1425279634@qq.com");

    //虽然名字是updateById  但是参数是一个对象
    int res = mapper.updateById(user);
    System.out.println(res);
}
```

![image-20200501232208739](http://image.beloved.ink/Typora/image-20200501232208739.png)

## 4.4、自动填充

创建时间，修改时间！这些操作一般都是自动化完成

阿里巴巴开发手册：所以的数据库表：gmt_create、gmt_modified创建时间、修改时间，几乎所有的表都要配置，且需要自动化

**方式一：数据库级别**

1. 在表中增加字段，并更新pojo

   ![image-20200501233806734](http://image.beloved.ink/Typora/image-20200501233806734.png)

2. 修改sql语句实现数据库级别，不展示

**方式二：代码级别**

1. 在字段上增加需要的注解

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
   
       //对应数据库的主键
       @TableId(type = IdType.AUTO)
       private Long id;
       private String name;
       private Integer age;
       private String email;
   
       //插入时更新
       @TableField(fill = FieldFill.INSERT)
       private Date createTime;
       //插入修改时更新
       @TableField(fill = FieldFill.INSERT_UPDATE)
       private Date updateTime;
   
   }
   ```

2. 编写处理器来处理注解

   ```java
   @Slf4j
   @Component  //处理器一定要加入ioc容器
   public class MyMetaObjectHandler implements MetaObjectHandler {
       //插入的填充策略
       @Override
       public void insertFill(MetaObject metaObject) {
           log.info("开始执行插入的填充策略...");
           //                          字段名             填充的值
           //setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject)
           this.setFieldValByName("createTime",new Date(),metaObject);
           this.setFieldValByName("updateTime",new Date(),metaObject);
       }
       //更新的填充策略
       @Override
       public void updateFill(MetaObject metaObject) {
           log.info("开始执行更新的填充策略...");
           this.setFieldValByName("updateTime",new Date(),metaObject);
       }
   }
   ```

3. 测试插入

   ![image-20200502000249171](http://image.beloved.ink/Typora/image-20200502000249171.png)

4. 测试修改

   ![image-20200502000513165](http://image.beloved.ink/Typora/image-20200502000513165.png)

![image-20200502000541497](http://image.beloved.ink/Typora/image-20200502000541497.png)

##  4.5、乐观锁

> 乐观锁：顾名思义十分乐观，它总是认为不会出现问题，无论干什么不去上锁！如果出现问题，再次更新值测试
>
> 悲观锁：顾名思义十分悲观，它总是认为会出现问题，无论干什么都会上锁！再去操作。

意图：

当要更新一条记录的时候，希望这条记录没有被别人更新

乐观锁实现方式：

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

```sql
-- 乐观锁：1、查询，获得版本号  version = 1
-- A
update user set name = "beloved",version = version +1
where id = 2 and version = 1

-- B 线程抢先完成，这个时候 version=2，会导致A线程失败
update user set name = "beloved",version = version +1
where id = 2 and version = 1
```

**测试**

1. 给数据库增加version字段

   ![image-20200502130115599](http://image.beloved.ink/Typora/image-20200502130115599.png)

2. pojo更新字段

   ```java
   @Version  //乐观锁version注解
   private Integer version;
   ```

3. 注册组件

   ```java
   //扫描mapper
   @MapperScan("com.zh.mapper")
   @EnableTransactionManagement
   @Configuration  //配置类
   public class MyBatisPlusConfig {
   
       //注册乐观锁插件
       @Bean
       public OptimisticLockerInterceptor optimisticLockerInterceptor() {
           return new OptimisticLockerInterceptor();
       }
   
   }
   ```

4. 测试

   ```java
   //测试乐观锁成功
   @Test
   public void version01(){
   
       //查询用户信息
       User user = mapper.selectById(1L);
       //修改用户信息
       user.setName("张三");
       //执行更新操作
       mapper.updateById(user);
   
   }
   
   //测试乐观锁失败
   @Test
   public void version02(){
       //线程一
       User user = mapper.selectById(1L);
       user.setName("张三111");
   
       //线程二
       //模拟另一个线程执行了插队操作
       User user2 = mapper.selectById(1L);
       user2.setName("张三222");
       mapper.updateById(user2);
   
       //如果没有乐观锁就会覆盖线程插队的值
       mapper.updateById(user);//操作失败
   }
   ```

   ![image-20200502131753651](http://image.beloved.ink/Typora/image-20200502131753651.png)

## 4.6、查询

```java
//根据id查询单个记录
@Test
public void selectById(){
    User user = mapper.selectById(1L);
    System.out.println(user);
}

//批量查询
@Test
public void selectBatchIds(){
    List<User> list = mapper.selectBatchIds(Arrays.asList(1, 2, 3));
    list.forEach(System.out::println);
}

//条件查询 map
@Test
public void selectByMap(){
    HashMap<String,Object> map = new HashMap<>();
    map.put("name","Beloved");
    map.put("age",20);
    //动态sql
    List<User> list = mapper.selectByMap(map);
    list.forEach(System.out::println);
}
```

## 4.7、分页查询

MybatisPlus自带分页插件

1. 配置分页插件拦截器组件

   ```java
   //分页插件
   @Bean
   public PaginationInterceptor paginationInterceptor() {
       return new PaginationInterceptor();
   }
   ```

2. 直接使用page对象即可

   ```java
   @Test
   //测试分页查询
   public void page(){
   
       /*
        * current: 当前页
        * size: 每页大小
        */
       Page<User> page = new Page<>(2,3);
       mapper.selectPage(page,null);
   
       page.getRecords().forEach(System.out::println);
   
       //总页数
       System.out.println(page.getTotal());
   }
   ```

![image-20200502133848356](http://image.beloved.ink/Typora/image-20200502133848356.png)

## 4.8、删除

```java
//根据id删除
@Test
public void deleteById(){
    mapper.deleteById(1256231939020390405L);
}

//根据id批量删除
@Test
public void deleteBatchIds(){
    mapper.deleteBatchIds(Arrays.asList(1,2,3));
}

//根据条件
@Test
public void deleteByMap(){
    HashMap<String,Object> map = new HashMap<>();
    map.put("name","Billie");
    mapper.deleteByMap(map);
}
```

## 4.9、逻辑删除

> 物理删除：从数据库中直接删除  deleted = 0
>
> 逻辑删除：在数据库中没有删除，而是通过变量让 数据失效  deleted = 1

1. 在数据库中增加deleted字段

   ![image-20200502135028931](http://image.beloved.ink/Typora/image-20200502135028931.png)

2. 在pojo更新属性

   ```java
   @TableLogic  //逻辑删除
   private int deleted;
   ```

3. 注册组件并配置

   **注意：3.1.1以上，不在需要配置bean。只需配置即可使用**

   **使用mp自带方法删除和查找都会附带逻辑删除功能 (自己写的xml不会)**

   ```java
   //逻辑删除
   @Bean
   public ISqlInjector sqlInjector(){
       return new LogicSqlInjector();
   }
   ```

   ```properties
   # 逻辑删除
   # 逻辑已删除值(默认为 1)
   mybatis-plus.global-config.db-config.logic-delete-value=1
   # 逻辑未删除值(默认为 0)
   mybatis-plus.global-config.db-config.logic-not-delete-value=0
   ```

4. 测试删除

   ![image-20200502140404873](http://image.beloved.ink/Typora/image-20200502140404873.png)

   ![image-20200502140440348](http://image.beloved.ink/Typora/image-20200502140440348.png)

5. 测试查询

   ![image-20200502140614320](http://image.beloved.ink/Typora/image-20200502140614320.png)

# 5、性能分析插件

作用：性能分析拦截器，用于输出每条SQL语句及执行时间

MybatisPlus提供性能分析差劲，超过设置的时间，代码会报错，停止运行

**该插件 `3.2.0` 以上版本移除推荐使用第三方扩展 **

```java
@Bean
@Profile({"dev","test"})// 设置 dev test 环境开启
public PerformanceInterceptor performanceInterceptor() {
    PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
    //sql最大执行时间，超过则报错
    performanceInterceptor.setMaxTime(1);
    //是否格式化sql
    performanceInterceptor.setFormat(true);
    return performanceInterceptor;
}
```

**要在springboot中配置环境为dev或test**

```properties
# 设置开发环境
spring.profiles.active=dev
```

**测试使用**

![image-20200502142332679](http://image.beloved.ink/Typora/image-20200502142332679.png)

# 6、条件构造器

写一些复杂的sql可以使用Wrapper

**测试一：条件查询**

```java
@Test
public void test01(){
    //查询name和age不为空，且age>=23

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    wrapper
            .isNotNull("name")
            .isNotNull("age")
            .ge("age",23);

    List<User> list = mapper.selectList(wrapper);//和map类似

    list.forEach(System.out::println);
}
```

![image-20200502145821616](http://image.beloved.ink/Typora/image-20200502145821616.png)

**测试二：查询一条数据**

```java
@Test
public void test02(){
    //查询name为Beloved

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    wrapper.eq("name","Beloved");

    //查询一条数据，多条用list或者map
    User user = mapper.selectOne(wrapper);

    System.out.println(user);
}
```

![image-20200502145937294](http://image.beloved.ink/Typora/image-20200502145937294.png)

**测试三：区间与统计**

```java
@Test
public void test03(){
    //查询age到20~30之间

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    wrapper.between("age",20,30);

    List<User> list = mapper.selectList(wrapper);

    list.forEach(System.out::println);

    //统计数
    System.out.println(mapper.selectCount(wrapper));
}
```

![image-20200502150026382](http://image.beloved.ink/Typora/image-20200502150026382.png)

![image-20200502150035172](http://image.beloved.ink/Typora/image-20200502150035172.png)

**测试四：模糊查询**

```java
@Test
public void test04(){
    //模糊查询

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    // name中不包含a，且邮箱是t开头
    // likeRight    t%
    // likeLeft     %t
    wrapper.notLike("name","a")
            .likeRight("email","t");

    List<Map<String, Object>> map = mapper.selectMaps(wrapper);

    map.forEach(System.out::println);
}
```

![image-20200502150119989](http://image.beloved.ink/Typora/image-20200502150119989.png)

**测试五：子查询**

```java
@Test
public void test05(){
    //子查询

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    // id在子查询中的数据
    // inValue:可以是eq...
    wrapper.inSql("id","select id from user where id < 3");

    List<User> objects = mapper.selectList(wrapper);

    objects.forEach(System.out::println);
}
```

![image-20200502150232672](http://image.beloved.ink/Typora/image-20200502150232672.png)

**测试六：排序**

```java
@Test
public void test06(){
    //排序

    QueryWrapper<User> wrapper = new QueryWrapper<>();

    //通过id排序
    wrapper.orderByDesc("id");

    List<User> list = mapper.selectList(wrapper);

    list.forEach(System.out::println);
}
```

![image-20200502150310932](http://image.beloved.ink/Typora/image-20200502150310932.png)

# 7、代码自动生成器

创建`mybatisplus02_code`空项目测试

- 导入所需依赖

  ```xml
  <!-- mysql驱动 -->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
  </dependency>
  <!-- lombok -->
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
  </dependency>
  <!-- mybatis-plus-boot-starter -->
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.0.5</version>
  </dependency>
  <!-- mybatis-plus代码生成器模板引擎 -->
  <dependency>
      <groupId>org.apache.velocity</groupId>
      <artifactId>velocity-engine-core</artifactId>
      <version>2.0</version>
  </dependency>
  <!-- swagger-ui -->
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.9.2</version>
  </dependency>
  
  <!-- springfox-swagger2 -->
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>2.9.2</version>
  </dependency>
  ```

- 导入相关配置

  ```properties
  # 设置开发环境
  spring.profiles.active=dev
  
  spring.datasource.username=root
  spring.datasource.password=123456
  spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  
  
  # 配置日志  使用控制台输出
  mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
  
  # 逻辑删除
  # 逻辑已删除值(默认为 1)
  mybatis-plus.global-config.db-config.logic-delete-value=1
  # 逻辑未删除值(默认为 0)
  mybatis-plus.global-config.db-config.logic-not-delete-value=0
  ```

- 导入相关组件的配置

  - `MyBatisPlusConfig`

    ```java
    package com.zh.config;
    
    import com.baomidou.mybatisplus.core.injector.ISqlInjector;
    import com.baomidou.mybatisplus.extension.injector.LogicSqlInjector;
    import com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor;
    import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
    import com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor;
    import org.mybatis.spring.annotation.MapperScan;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.Profile;
    import org.springframework.transaction.annotation.EnableTransactionManagement;
    
    //扫描mapper
    @MapperScan("com.zh.mapper")
    @EnableTransactionManagement
    @Configuration  //配置类
    public class MyBatisPlusConfig {
    
        //注册乐观锁插件
        @Bean
        public OptimisticLockerInterceptor optimisticLockerInterceptor() {
            return new OptimisticLockerInterceptor();
        }
    
        //分页插件
        @Bean
        public PaginationInterceptor paginationInterceptor() {
            return new PaginationInterceptor();
        }
    
    
        //逻辑删除
        @Bean
        public ISqlInjector sqlInjector(){
            return new LogicSqlInjector();
        }
    
        //性能分析插件
        @Bean
        @Profile({"dev","test"})// 设置 dev test 环境开启
        public PerformanceInterceptor performanceInterceptor() {
            PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
            //sql最大执行时间，超过则报错
            performanceInterceptor.setMaxTime(1000);
            //是否格式化sql
            performanceInterceptor.setFormat(true);
            return performanceInterceptor;
        }
    }
    ```

  - `MyMetaObjectHandler`

    ```java
    package com.zh.handler;
    
    import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
    import lombok.extern.slf4j.Slf4j;
    import org.apache.ibatis.reflection.MetaObject;
    import org.springframework.stereotype.Component;
    
    import java.util.Date;
    
    @Slf4j
    @Component  //处理器一定要加入ioc容器
    public class MyMetaObjectHandler implements MetaObjectHandler {
        //插入的填充策略
        @Override
        public void insertFill(MetaObject metaObject) {
            log.info("开始执行插入的填充策略...");
            //                          字段名             填充的值
            //setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject)
            this.setFieldValByName("createTime",new Date(),metaObject);
            this.setFieldValByName("updateTime",new Date(),metaObject);
        }
        //更新的填充策略
        @Override
        public void updateFill(MetaObject metaObject) {
            log.info("开始执行更新的填充策略...");
            this.setFieldValByName("updateTime",new Date(),metaObject);
        }
    }
    ```

- 编写代码生成器

  ```java
  package com.zh;
  
  import com.baomidou.mybatisplus.annotation.DbType;
  import com.baomidou.mybatisplus.annotation.FieldFill;
  import com.baomidou.mybatisplus.annotation.IdType;
  import com.baomidou.mybatisplus.annotation.TableField;
  import com.baomidou.mybatisplus.generator.AutoGenerator;
  import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
  import com.baomidou.mybatisplus.generator.config.GlobalConfig;
  import com.baomidou.mybatisplus.generator.config.PackageConfig;
  import com.baomidou.mybatisplus.generator.config.StrategyConfig;
  import com.baomidou.mybatisplus.generator.config.po.TableFill;
  import com.baomidou.mybatisplus.generator.config.rules.DateType;
  import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
  
  import java.util.ArrayList;
  
  import static com.baomidou.mybatisplus.annotation.FieldFill.INSERT;
  import static com.baomidou.mybatisplus.annotation.FieldFill.INSERT_UPDATE;
  
  /**
   * 代码自动生成
   */
  public class ZhCode {
  
      public static void main(String[] args) {
          //需要构建一个代码自动生成器对象
          AutoGenerator mpg = new AutoGenerator();
  
          //配置策略
  
          //1、全局配置
          GlobalConfig gc = new GlobalConfig();
          //获取当前系统目录
          String projectPath = System.getProperty("user.dir");
          //输出位置
          gc.setOutputDir(projectPath+"/src/main/java");
          //作者
          gc.setAuthor("Beloved");
          //是否打开资源管理器
          gc.setOpen(false);
          //是否覆盖原来生成的
          gc.setFileOverride(false);
          //自动生成service前面又I前缀  ，通过正则表达式去除
          gc.setServiceName("%sService");
          //设置主键生成策略
          gc.setIdType(IdType.AUTO);
          //设置时间格式
          gc.setDateType(DateType.ONLY_DATE);
          //是否设置Swagger
          gc.setSwagger2(true);
          mpg.setGlobalConfig(gc);
  
          //2、设置数据源
          DataSourceConfig config = new DataSourceConfig();
          config.setUrl("jdbc:mysql://localhost:3306/mybatis_plus");
          config.setDriverName("com.mysql.cj.jdbc.Driver");
          config.setUsername("root");
          config.setPassword("123456");
          config.setDbType(DbType.MYSQL);
          mpg.setDataSource(config);
  
          //3、包的配置
          PackageConfig pc = new PackageConfig();
          //模块名
          pc.setModuleName("");
          //生成在那个包下
          pc.setParent("com.zh");
          //实体类包名
          pc.setEntity("pojo");
          //dao层包名
          pc.setMapper("mapper");
          //service包名
          pc.setService("service");
          pc.setServiceImpl("service.impl");
          //controller包名
          pc.setController("controller");
          mpg.setPackageInfo(pc);
  
          //4、策略配置
          StrategyConfig strategy = new StrategyConfig();
          //需要生成的表，可以传入多个参数
          strategy.setInclude("user");
          //表表和列的命名规则    下划线转驼峰命名
          strategy.setNaming(NamingStrategy.underline_to_camel);
          strategy.setColumnNaming(NamingStrategy.underline_to_camel);
          //自动lombok
          strategy.setEntityLombokModel(true);
          //逻辑删除
          strategy.setLogicDeleteFieldName("deleted");
          //自动填充策略
          TableFill createTime = new TableFill("create_time", INSERT);
          TableFill updateTime = new TableFill("update_time", INSERT_UPDATE);
          ArrayList<TableFill> tableFills = new ArrayList<>();
          tableFills.add(createTime);
          tableFills.add(updateTime);
          strategy.setTableFillList(tableFills);
          //乐观锁
          strategy.setVersionFieldName("version");
          //开启controller驼峰命名格式
          strategy.setRestControllerStyle(true);
          //设置链接请求为下划线  localhost:8080//hello_v1_v2
          strategy.setControllerMappingHyphenStyle(true);
          mpg.setStrategy(strategy);
  
          //执行
          mpg.execute();
      }
  
  }
  ```

- 运行测试

  ![image-20200502162640854](http://image.beloved.ink/Typora/image-20200502162640854.png)

