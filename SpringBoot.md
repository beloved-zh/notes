![image-20200413122213918](http://image.beloved.ink/Typora/image-20200413122213918.png)Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用，

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

# 1、HelloWorld

## 1.1、创建项目

![image-20200321094320718](http://image.beloved.ink/Typora/image-20200321094320718.png)

![image-20200321100645749](http://image.beloved.ink/Typora/image-20200321100645749.png)

![image-20200321100918323](http://image.beloved.ink/Typora/image-20200321100918323.png)

![image-20200321101112802](http://image.beloved.ink/Typora/image-20200321101112802.png)

## 1.2、pom.xml分析

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--夫项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.zh</groupId>
    <artifactId>springboot01_helloworld</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot01_helloworld</name>
    <description>zh first springboot project</description>

    <!--java版本-->
    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--web依赖：tomcat，dispatcherServlet,xml-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--单元测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <!--打jar包插件-->
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

- 项目元数据信息：创建时候输入的Project Metadata部分，也就是Maven项目的基本元素，包括：groupId、artifactId、version、name、description等
- parent：继承`spring-boot-starter-parent`的依赖管理，控制版本与打包等内容
- dependencies：项目具体依赖，这里包含了`spring-boot-starter-web`用于实现HTTP接口（该依赖中包含了Spring MVC），官网对它的描述是：使用Spring MVC构建Web（包括RESTful）应用程序的入门者，使用Tomcat作为默认嵌入式容器。；`spring-boot-starter-test`用于编写单元测试的依赖包。更多功能模块的使用我们将在后面逐步展开。
- build：构建配置部分。默认使用了`spring-boot-maven-plugin`，配合`spring-boot-starter-parent`就可以把Spring Boot应用打包成JAR来直接运行。

## 1.3、程序主入口启动程序

```java
package com.zh;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

//程序主入口
@SpringBootApplication
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}
```

==注意：所有的业务代码，必须和这个类在一级目录==

![image-20200321105644000](http://image.beloved.ink/Typora/image-20200321105644000.png)

**SpringBoot项目启动会有一个默认的请求**

![image-20200321105741213](http://image.beloved.ink/Typora/image-20200321105741213.png)

## 1.4、编写Controller测试

```java
package com.zh.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/my")
public class HelloController {

    @RequestMapping("/hello")
    @ResponseBody
    public String hello(){
        return "HelloWorld";
    }

}
```

![image-20200321105844440](http://image.beloved.ink/Typora/image-20200321105844440.png)

## 1.5、端口修改

```properties
# 更改项目端口号
server.port=8088
```

![image-20200321110115109](http://image.beloved.ink/Typora/image-20200321110115109.png)

![image-20200321110125458](http://image.beloved.ink/Typora/image-20200321110125458.png)

## 1.6、将项目打成jar包

![image-20200321110554450](http://image.beloved.ink/Typora/image-20200321110554450.png)

> # 运行

切换到jar包所在目录

==java -jar jar包名==

![image-20200321111236290](http://image.beloved.ink/Typora/image-20200321111236290.png)

## 1.7、修改启动图

https://www.bootschool.net/ascii

直接输入要生成的字母，系统会自动转换，然后复制下面转换好的字符；

到项目下的 resources 目录下新建一个txt文件就可以，banner.txt

![image-20200321111842144](http://image.beloved.ink/Typora/image-20200321111842144.png)

# 2、原理

运行原理及底层代码参考

https://blog.kuangstudy.com/index.php/archives/630/

# 3、SpringBoot配置

## 3.1、配置文件

SpringBoot使用一个全局的配置文件，配置文件的名称是固定的

- application.properties
  - 语法结构：key=value
- application.yaml   ==官方建议==
  - 语法结构：key: 空格 value

**配置文件的作用：**修改SpringBoot自动配置的默认值。因为SpringBoot在底层都已经配置好了

## 3.2、YAML

**标记语言**

以前的配置文件，大多使用XML配置

**yaml配置：**

```yaml
server:
	prot: 8080
```

**xml：**

```xml
<server>
	<prot>8080</prot>
</server>
```

### 3.2.1、基础语法

```yaml
# key: value
# 对空格要求极高
# 普通的key-value
name: zhangheng

# 对象
students:
  name: zhangheng
  age: 18

# 行内写法
student: {name: zhangheng,age: 18}


# 数组
pets:
  - cat
  - dog
  - pig

# 行内写法
pet: [cat,dog,pig]
```

### 3.2.2、使用yaml直接给实体类赋值

- 创建实体类

  ```java
  @Component
  @Data
  @AllArgsConstructor
  @NoArgsConstructor
  @ConfigurationProperties(prefix = "dog")
  public class Dog {
  
      private String dogName;
      private Integer age;
  
  }
  ```

  ```java
  @Component
  @Data
  @AllArgsConstructor
  @NoArgsConstructor
  /*
   * 通过这个配置产生提示，如果不设置，就会提示爆红  prefix绑定配置文件的值
   *
   * @ConfigurationProperties作用：
   * 将配置文件中配置的每一个属性的值，映射到这个组件中
   * 告诉SpringBoot将本类中的所有属性和配置问价中相关的配置进行绑定
   * 参数prefix = "person"：将配置文件中的person与下面的属性一一对应
   *
   * 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能
   */
  @ConfigurationProperties(prefix = "person")
  public class Person {
  
      private String name;
      private Integer age;
      private Boolean happy;
      private Date birth;
      private Map<String,Object> maps;
      private List<Object> lists;
      private Dog dog;
  
  }
  ```

- 导入依赖

  ```xml
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
  </dependency>
  ```

- application.yaml 

  ```yaml
  person:
    name: 张恒${random.uuid}
    age: ${random.int}
    happy: true
    birth: 2000/03/17
    maps: {k1: v1,k2: v2}
    list:
      - code
      - music
      - girl
    dog:
      name: 旺财
      age: 3
  
  dog:
    dog_name: 阿黄
    age: 3
  ```

- 测试

  ```java
  @Autowired
      private Person person;
      @Autowired
      private Dog dog;
  
      @Test
      void contextLoads() {
          System.out.println(person);
          System.out.println(dog);
  }
  ```

  ![image-20200321133906378](http://image.beloved.ink/Typora/image-20200321133906378.png)

==具有命名转换：dogName--->dog_name==

## 3.3、JSR303校验

- 实体类

  - 开启校验==@Validated//数据校验==

  ```java
  @Component
  @Data
  @AllArgsConstructor
  @NoArgsConstructor
  @ConfigurationProperties(prefix = "person")
  @Validated//数据校验
  public class Person {
  
      private String name;
      private Integer age;
      @Email
      private String email;
      private Boolean happy;
      private Date birth;
      private Map<String,Object> maps;
      private List<Object> lists;
      private Dog dog;
  
  }
  ```

- application.yaml 

  ```
  person:
    name: 张恒${random.uuid}
    age: ${random.int}
    email: 123
    happy: true
    birth: 2000/03/17
    maps: {k1: v1,k2: v2}
    list:
      - code
      - music
      - girl
    dog:
      name: 旺财
      age: 3
  ```

- 测试

  - ![image-20200321134520671](http://image.beloved.ink/Typora/image-20200321134520671.png)

**自定义错误提示**

![image-20200321134655398](http://image.beloved.ink/Typora/image-20200321134655398.png)

![image-20200321134722457](http://image.beloved.ink/Typora/image-20200321134722457.png)

**==JSR303常用注释==**

```yaml
@NotNull(message="名字不能为空")
private String userName;

@Max(value=120,message="年龄最大不能查过120")
private int age;

@Email(message="邮箱格式错误")
private String email;

空检查
@Null       
验证对象是否为null
@NotNull    
验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   
检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   
检查约束元素是否为NULL或者是EMPTY.    

Booelan检查
@AssertTrue     验证 Boolean对象是否为 true  
@AssertFalse    验证 Boolean对象是否为 false      

长度检查
@Size(min=, max=)  验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  @Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等除此以外，我们还可以自定义一些数据校验规则
```

![image-20200321134944909](http://image.beloved.ink/Typora/image-20200321134944909.png)

![image-20200321135006962](http://image.beloved.ink/Typora/image-20200321135006962.png)

## 3.4、多环境切换

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

### 3.4.1、多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

**例如：**

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
```

### 3.4.2、yaml的多文档块

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yaml
server:
  port: 8080
#选择要激活那个环境块
spring:
  profiles:
    active: dev

---
server:
  port: 8090
# 模块名
spring:
  profiles: dev

---
server:
  port: 8099
spring:
  profiles: test
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**

## 3.5、配置文件加载位置

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

官方外部配置文件说明参考文档：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-external-config-application-property-files

![image-20200321143121234](http://image.beloved.ink/Typora/image-20200321143121234.png)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```Java
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

```
#配置项目的访问路径server.servlet.context-path=/kuang
```

**拓展，运维小技巧**

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```
java -jar spring-boot-config.jar --spring.config.location=F:/application.proper
```

# 4、SpringBoot Web开发

**所有有关web开发的配置均可在WebMvcAutoConfiguration中查看**

## 4.1、静态资源

### 4.1.1、webjars

**以jar形式使用maven导入相关的静态资源**

如jquery

webjars官网：https://www.webjars.org/

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

![image-20200321151528647](http://image.beloved.ink/Typora/image-20200321151528647.png)

**访问：http://localhost:8080/webjars/jquery/3.4.1/jquery.js**

![image-20200321151557362](http://image.beloved.ink/Typora/image-20200321151557362.png)

### 4.1.2、自定义(推荐)

**在resources文件下创建public，resources，static(默认)**

![image-20200321151659385](http://image.beloved.ink/Typora/image-20200321151659385.png)

**访问：http://localhost:8080/........**

![image-20200321151912725](http://image.beloved.ink/Typora/image-20200321151912725.png)

**==优先级：resources > static > public==**

**一般：**

- public放公共资源。比如js...
- static放静态资源。比如图片...
- resources放一些文件。比如上传文件

## 4.2、定制首页和图标

### 4.2.1、首页

![image-20200321200632884](http://image.beloved.ink/Typora/image-20200321200632884.png)

将首页放在静态资源目录即可找到

==一般将首页index.html放在static==

==在templates目录下的所有页面只能通过controller来跳转==

### 4.2.2、图标

**在SpringBoot的新版本中WebMvcAutoConfiguration中没有图标的设置。需要查看设置降低SpringBoot的版本。虽然新版没有详细说明，但是可以正常设置**

- 创建favicon.ico图标，放在静态资源目录

  ![image-20200321213707639](http://image.beloved.ink/Typora/image-20200321213707639.png)

- 配置application.yaml

  ```yaml
  # 关闭默认图标
  spring.mvc.favivon.enabled: false
  ```

==注意：设置后没有作用。清除浏览器缓存，重启idea==

## 4.3、Thymeleaf模板引擎

### 4.3.1、模板引擎

前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。

jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，思想如下图：

![image-20200321220933348](http://image.beloved.ink/Typora/image-20200321220933348.png)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

### 4.3.2、引入Thymeleaf

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

![image-20200321221418556](http://image.beloved.ink/Typora/image-20200321221418556.png)

![image-20200321221544924](http://image.beloved.ink/Typora/image-20200321221544924.png)

```xml
<!--
Thymeleaf
SpringBoot2.x基于thymeleaf3.x
查看版本如果是2.x就会报错，需要修改版本
-->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
```

![image-20200321222103222](http://image.beloved.ink/Typora/image-20200321222103222.png)

### 4.3.3、Thymeleaf分析

**查看Thymeleaf的自动配置类：ThymeleafProperties**

![image-20200321222422124](http://image.beloved.ink/Typora/image-20200321222422124.png)

### 4.3.4、测试

在templates创建test.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
Hello Thymeleaf!!!
</body>
</html>
```

编写TestController

```java
/**
 * templates下的页面，只能通过Controller来跳转
 * 这个需要模板引擎的支持！ Thymeleaf
 */
@Controller
public class MyController {

    @RequestMapping("/test")
    public String test(){
        return "test";
    }

}
```

测试http://localhost:8080/test

![image-20200321223139500](http://image.beloved.ink/Typora/image-20200321223139500.png)

### 4.3.5、Thymeleaf 语法学习

Thymeleaf 官网：https://www.thymeleaf.org

**修改测试类，添加数据**

```java
@Controller
public class MyController {

    @RequestMapping("/test")
    public String test(Model model){
        
        model.addAttribute("msg","我是提示信息");
        
        return "test";
    }

}
```

**要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。**

```html
 xmlns:th="http://www.thymeleaf.org"
```

**编写前端页面**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div th:text="${msg}"></div>

</body>
</html>
```

**测试**

![image-20200321223632030](http://image.beloved.ink/Typora/image-20200321223632030.png)

### 4.3.6、Thymeleaf 基础语法

==相关语法：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html**==

#### 4.3.6.1、attr

**我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

![image-20200321223854685](http://image.beloved.ink/Typora/image-20200321223854685.png)

#### 4.3.6.2、能写哪些表达式呢？

```yaml
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================

  Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
  Message Expressions: #{...}：获取国际化内容
  Link URL Expressions: @{...}：定义URL；
  Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
      
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
    
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
    
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
    
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
    
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
    
Special tokens:
    No-Operation: _
```

#### 4.3.6.3、**练习测试**

**编写一个Controller，放一些数据**

```java
@RequestMapping("/test2")
public String test2(Model model){

    model.addAttribute("msg","<h1>Hello SpringBoot</h1>");
    //Arrays.asList将数组转换为集合
    model.addAttribute("users", Arrays.asList("张三","李四","王五"));

    return "test";
}
```

**测试页面取出数据**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--转义-->
<div th:text="${msg}"></div>
<hr>
<!--不转义-->
<div th:utext="${msg}"></div>
<hr>

<!--遍历数据-->
<!--th:each每次遍历都会生成当前这个标签：官网#9-->
<h1 th:each="user : ${users}" th:text="${user}"></h1>
<hr>
<!--行内写法：官网#12-->
<h1 th:each="user : ${users}">[[ ${user} ]]</h1>
</body>
</html>
```

**测试**

![image-20200321224915748](http://image.beloved.ink/Typora/image-20200321224915748.png)

### 4.3.7、练习测试

```html
<!--
所有的属性前都可以加上th:
使用Thymeleaf
-->
<!--基本取值-->
<td th:text="${emp.getEId()}"></td>
<!--行内取值-->
<td>[[${emp.getEId()}]]</td>
<!--链接地址-->
<a th:href="@{/user/logout}">注销</a>
<!--三元运算符-->
<td th:text="${emp.getSex()==0?'女':'男'}"></td>
<!--格式化数据-->
<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
<!--属性链接  相加+-->
<a th:href="@{/emp/}+${emp.getEId()}">修改</a>
<!--遍历循环-->
<option th:each="dep:${departments}" th:value="${dep.getDId()}" th:text="${dep.getDName()}"></option>
<!--判断默认选中-->
<input th:checked="${employee.getSex()==1}" >
<!--th:fragment抽取模板-->
<nav th:fragment="sidebar">
<!--
引入模块并传入数据
th:replace="~{页面名::模板名(属性名='value')}"
-->
<div th:replace="~{commons/commons::sidebar(active='main.html')}"></div>
<!--if判断 not非 #strings.isEmpty()使用spring的非空判断-->
th:if="${not #strings.isEmpty(msg)}"
<!--#{}国际化标签-->
<h1 th:text="#{login.tip}"></h1>
 <!--截取部分文本-->   
th:text="${#strings.abbreviate(article.text,100)}"
```

## 4.4、MVC自动配置原理

### 4.4.1、官网阅读

官网地址 ：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

![image-20200322102447266](http://image.beloved.ink/Typora/image-20200322102447266.png)

如果要保留这些Spring Boot MVC定制并进行更多的MVC定制（拦截器，格式化程序，视图控制器和其他功能），则可以添加自己的类型为WebMvcConfigurer的@Configuration类，但不要使用@EnableWebMvc。

如果要提供RequestMappingHandlerMapping，RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义实例，并且仍然保留Spring Boot MVC自定义，则可以声明WebMvcRegistrations类型的bean，并使用它提供那些组件的自定义实例。

如果要完全控制Spring MVC，则可以添加用@EnableWebMvc注释的自己的@Configuration，或者按照@EnableWebMvc的Javadoc中的说明添加自己的@Configuration注释的DelegatingWebMvcConfiguration。

**自定义java配置类。如果只是扩展SpringMvc实现WebMvcConfigurer接口即可，如果想要全面接管SpringMVC添加@EnableWebMvc注解，这样SpringBoot自动配置的SpringMvc将全部失效**

### 4.4.2、视图解析器

**查看源码ContentNegotiatingViewResolver---->ViewResolver**

![image-20200322103916687](http://image.beloved.ink/Typora/image-20200322103916687.png)

![image-20200322103955603](http://image.beloved.ink/Typora/image-20200322103955603.png)

**实现了视图解析器ViewResolver接口的类，我们就可与看成是视图解析器**

**查看自动配置的视图解析器源码**

![image-20200322104354393](http://image.beloved.ink/Typora/image-20200322104354393.png)

![image-20200322104739476](http://image.beloved.ink/Typora/image-20200322104739476.png)

**自定义视图解析器**

```java
// 扩展springMvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    /**
     * 视图解析器
     * 查看源码ContentNegotiatingViewResolver实现了ViewResolver接口
     *
     * 实现了视图解析器ViewResolver接口的类，我们就可与看成是视图解析器
     */

    //添加到ioc容器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }


    /**
     * 静态内部类，自定义视图解析器，实现ViewResolver接口，重写方法
     */
    public static class MyViewResolver implements ViewResolver{

        @Override
        public View resolveViewName(String viewName, Locale locale) throws Exception {
            return null;
        }
    }
}
```

**查看自定义视图解析器**

给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

![image-20200322105816323](http://image.beloved.ink/Typora/image-20200322105816323.png)

运行程序，发起一个请求查看this中的视图解析器，可以看到所有的视图解析器中有自定义的

![image-20200322110011881](http://image.beloved.ink/Typora/image-20200322110011881.png)

### 4.4.3、格式化

**在WebMvcAutoConfiguration中找到FormattingConversionService**

![image-20200322111138662](http://image.beloved.ink/Typora/image-20200322111138662.png)

![image-20200322111236628](http://image.beloved.ink/Typora/image-20200322111236628.png)

**==可以在配置文件中修改格式==**

```yaml
# 自定义的配置日期格式
spring:
  mvc:
    date-format: yyyy-MM-dd
```

### 4.4.4、修改MVC的默认配置

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    /**
     * 需要扩展什么内容，实现对应的方法即可
     * @param registry
     */
    
    //视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/hello ， 就会跳转到test页面；
        registry.addViewController("/hello").setViewName("test");
    }
}
```

**测试内容，可以跳转**

![image-20200322112410049](http://image.beloved.ink/Typora/image-20200322112410049.png)

**要扩展SpringMVC，官方就推荐这样使用，既保SpringBoot留所有的自动配置，也能用扩展的配置！**

### 4.4.5、全面接管SpringMVC

**全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是自己去配置！**

**只需在配置类中要加一个@EnableWebMvc。**

```java
@Configuration
@EnableWebMvc
public class MyMvcConfig implements WebMvcConfigurer {

    //视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/hello ， 就会跳转到test页面；
        registry.addViewController("/hello").setViewName("test");
    }
}
```

**运行**

![image-20200322113131390](http://image.beloved.ink/Typora/image-20200322113131390.png)

![image-20200322113140073](http://image.beloved.ink/Typora/image-20200322113140073.png)

**可以发现，配置类中自定义的可以访问，所有的SpringMVC自动配置都失效了**

**在开发中，不推荐使用全面接管SpringMVC。一般都是配置类扩展SpringMvc**

> **原理分析**

查看@EnableWebMvc注解发现是导入一个类

![image-20200322113521450](http://image.beloved.ink/Typora/image-20200322113521450.png)

这个类继承了一个父类WebMvcConfigurationSupport

![image-20200322113606192](http://image.beloved.ink/Typora/image-20200322113606192.png)

查看Webmvc自动配置类

![image-20200322114032687](http://image.beloved.ink/Typora/image-20200322114032687.png)

## 4.5、国际化i18n

### **4.5.1、配置i18n文件**

![image-20200322232028777](http://image.beloved.ink/Typora/image-20200322232028777.png)

```properties
# login.properties 默认配置
login.btn=登录  
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名

# login_zh_CN.properties  中文配置
login.btn=登录  
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名

# login_en_US.properties  英文配置
login.btn=Sign in
login.password=Password
login.remember=Remember me
login.tip=Please sign in
login.username=Username
```

**==三个properties配置文件key-value。中的key要一样==**

### 4.5.2、配置LocaleResolver组件

**实现LocaleResolver接口，重写方法**

```java
public class MyLocaleResolver implements LocaleResolver {

    //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        //获取请求中的语言参数
        String l = request.getParameter("l");
        Locale locale = Locale.getDefault();//如果没有就使用默认的
        //如果请求的链接携带了国际化参数
        if(!StringUtils.isEmpty(l)){//如果不为空 spring中的方法
            //zh_CN  拆分语言参数
            String[] s = l.split("_");
            //国家，地区
            locale = new Locale(s[0],s[1]);
        }
        return locale;
    }
    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
    }
}
```

### 4.5.3、将组件配置到spring容器`@Bean`

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    //自定义的国际化组件就失效了
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }

}
```

### 4.5.4、使用i18n的值

```html
<!--传递参数 调用i18n的值-->
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
<!--#{} 使用标签调用值-->
<h1 th:text="#{login.tip}"></h1>
<!--行内调用-->
<button>[[#{login.btn}]]</button>
```

## 4.6、拦截器

### 4.6.1、配置拦截器组件

==**拦截器组件需要实现HandlerInterceptor接口重写方法**==

```java
/**
 * 登录拦截器
 */
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        HttpSession session = request.getSession();

        Object username = session.getAttribute("username");
        
        if (username == null){//没有登录
            request.setAttribute("msg","没有权限，请登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }

        return true;
    }

}
```

### 4.6.2、将组件注入到spring容器中

==**addPathPatterns：需要拦截的方法**==

==**excludePathPatterns：需要放行的方法**==

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    //配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor())
            .addPathPatterns("/**")
            .excludePathPatterns("/index.html","/","/user/login","/css/**","/js/**","img/**","favicon.ico");
    }
}
```

## 4.7、页面定制

### 4.7.1、首页设置

==**首页放在templates中不能直接访问，需要从Controller去跳转。配置视图跳转**==

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
}
```

### 4.7.2、404/500

==**在SpringBoot中配置404/500页面只需要在templates中创建error目录中，将404/500页面放入即可**==

![image-20200322235059792](http://image.beloved.ink/Typora/image-20200322235059792.png)

# 5、整合JDBC

## 5.1、SpringData

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

## 5.2、测试数据源

- 添加启动器Spring Web、JDBC API、MySql

  ```xml
  <!--web-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  
  <!--JDBC-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-jdbc</artifactId>
  </dependency>
  
  <!--Mysql-->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <scope>runtime</scope>
  </dependency>
  ```

- 编写yaml配置文件连接数据库

  ```yaml
  spring:
    # 数据源设置
    datasource:
      username: root
      password: 123456
      # 如有时区问题添加配置serverTimezone=UTC
      url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8
      driver-class-name: com.mysql.cj.jdbc.Driver
  ```

- SpringBoot已经进行了自动配置，可以直接使用数据源

  ```java
  @SpringBootTest
  class Springboot05DataApplicationTests {
  
      @Autowired
      DataSource dataSource;
  
      @Test
      void contextLoads() throws SQLException {
  
          //查看默认数据源  class com.zaxxer.hikari.HikariDataSource
          System.out.println("默认数据源===》"+dataSource.getClass());
  
          //获得数据库连接
          Connection conn = dataSource.getConnection();
          System.out.println(conn);
  
          //关闭
          conn.close();
      }
  
  }
  ```

导入的类都在 DataSourceConfiguration 配置类下，Spring Boot 默认使用HikariDataSource 数据源

**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；**

==**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**==

## 5.3、JDBCTemplate

1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；

2、即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。

3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。

4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用

5、JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

**测试**

```java
@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    @GetMapping("/findAll")
    public List<Map<String,Object>> findAll(){
        String sql = "select * from user";

        List<Map<String, Object>> list = jdbcTemplate.queryForList(sql);

        return list;
    }

    @GetMapping("/add")
    public String add(){
        String sql = "insert into mybatis.user(id, name, pwd) values (5,'admin','123456')";

        jdbcTemplate.update(sql);

        return "添加成功";
    }

    @GetMapping("/update/{id}")
    public String add(@PathVariable("id") Integer id){
        String sql = "update mybatis.user set name=?,pwd=? where id=?";

        Object[] objects = new Object[3];
        objects[0] = "张恒";
        objects[1] = "admin";
        objects[2] = id;

        jdbcTemplate.update(sql,objects);

        return "修改成功";
    }


    @GetMapping("/delete/{id}")
    public String delete(@PathVariable("id") Integer id){
        String sql = "delete from mybatis.user where id = ?";

        jdbcTemplate.update(sql,id);

        return "删除成功";
    }

}
```

# 6、整合Druid

## 6.1、Druid简介

Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。

Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。

Github地址：https://github.com/alibaba/druid/

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

![image-20200323104847024](http://image.beloved.ink/Typora/image-20200323104847024.png)

![image-20200323104910115](http://image.beloved.ink/Typora/image-20200323104910115.png)

![image-20200323104933644](http://image.beloved.ink/Typora/image-20200323104933644.png)

## 6.2、配置数据源

- 添加Druid 数据源依赖。

  ```xml
  <!-- Druid数据源 -->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.20</version>
  </dependency>
  ```

- 切换数据源：通过 spring.datasource.type 指定数据源

  ```yaml
  spring:
    # 数据源设置
    datasource:
      username: root
      password: 123456
      # 如有时区问题添加配置serverTimezone=UTC
      url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8
      driver-class-name: com.mysql.cj.jdbc.Driver
      type: com.alibaba.druid.pool.DruidDataSource  # 自定义数据源
  ```

- 测试

  ![image-20200323105406009](http://image.beloved.ink/Typora/image-20200323105406009.png)

- 设置数据源基本参数

  ```yaml
  spring:
    # 数据源设置
    datasource:
      username: root
      password: 123456
      # 如有时区问题添加配置serverTimezone=UTC
      url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8
      driver-class-name: com.mysql.cj.jdbc.Driver
      type: com.alibaba.druid.pool.DruidDataSource  # 自定义数据源
  
      #Spring Boot 默认是不注入这些属性值的，需要自己绑定
      #druid 数据源专有配置
      initialSize: 5            # 初始化建立物理连接个数
      minIdle: 5                # 最小连接池数量
      maxActive: 20             # 最大浏览器数量
      maxWait: 60000            # 获取连接最大等待时间，单位毫秒
      timeBetweenEvictionRunsMillis: 60000
      minEvictableIdleTimeMillis: 300000    # 连接保持空闲而不被驱逐的最长时间
      validationQuery: SELECT 1 FROM DUAL   # 用来检测连接是否有效的sql
      testWhileIdle: true
      testOnBorrow: false
      testOnReturn: false
      poolPreparedStatements: true
  
      #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
      filters: stat,wall,log4j
      maxPoolPreparedStatementPerConnectionSize: 20
      useGlobalDataSourceStat: true
      connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
  ```

- 导入Log4j 的依赖

  ```xml
  <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
  </dependency>
  ```

- 创建DruidDataSource绑定全局配置文件中的参数，添加到容器中，不使用 Spring Boot的自动生成的数据源自己添加 DruidDataSource 组件到容器中，并绑定属性；

  ```java
  @Configuration
  public class DruidConfig {
  
      /*
         将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
         绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
         @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
         前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
       */
      @ConfigurationProperties(prefix = "spring.datasource")
      @Bean
      public DataSource druidDataSource() {
          return new DruidDataSource();
      }
  
  }
  ```

- 测试

  ```java
  @SpringBootTest
  class Springboot05DataApplicationTests {
  
      @Autowired
      DataSource dataSource;
  
      @Test
      void contextLoads() throws SQLException {
  
          //查看默认数据源  class com.zaxxer.hikari.HikariDataSource
          System.out.println("数据源===》"+dataSource.getClass());
  
          //获得数据库连接
          Connection conn = dataSource.getConnection();
          System.out.println(conn);
  
          DruidDataSource druidDataSource = (DruidDataSource) dataSource;
          System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
          System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());
  
          //关闭
          conn.close();
      }
  
  }
  ```

![image-20200323111044959](http://image.beloved.ink/Typora/image-20200323111044959.png)

## 6.3、配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

所以第一步需要设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

```java
//配置 Druid 监控管理后台的Servlet；
//内置 Servlet 容器时没有web.xml文件，所以使用 Spring Boot 的注册 Servlet 方式
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

    // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet
    // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
    Map<String, String> initParams = new HashMap<>();
    //用户名密码的key是固定的
    initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
    initParams.put("loginPassword", "123456"); //后台管理界面的登录密码

    //后台允许谁可以访问
    //initParams.put("allow", "localhost")：表示只有本机可以访问
    //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
    initParams.put("allow", "");
    //deny：Druid 后台拒绝谁访问
    //initParams.put("kuangshen", "192.168.1.20");表示禁止此ip访问

    //设置初始化参数
    bean.setInitParameters(initParams);
    return bean;
}
```

访问：http://localhost:8080/druid/login.html即可进入登录页面

![image-20200323115449292](http://image.beloved.ink/Typora/image-20200323115449292.png)

登录查看基本设置

![image-20200323115518073](http://image.beloved.ink/Typora/image-20200323115518073.png)

## 6.4、**配置 Druid web 监控 filter 过滤器**

```java
//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    //这些请求不进行统计
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

## 6.5、XML配置方式

https://www.cnblogs.com/likearock/p/11789373.html

# 7、整合Mybatis

官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1

- 导入 MyBatis 所需要的依赖

  ```xml
  <!--mybatis-spring-boot-starter-->
  <dependency>
      <groupId>org.mybatis.spring.boot</groupId>
      <artifactId>mybatis-spring-boot-starter</artifactId>
      <version>2.1.0</version>
  </dependency>
  ```

- 配置数据库连接和Mybatis的整合信息

  ```yaml
  spring:
    datasource:
      username: root
      password: 123456
      url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8
      driver-class-name: com.mysql.cj.jdbc.Driver
      type: com.alibaba.druid.pool.DruidDataSource
      initialSize: 5
      minIdle: 5
      maxActive: 20
      maxWait: 60000
      timeBetweenEvictionRunsMillis: 60000
      minEvictableIdleTimeMillis: 300000
      validationQuery: SELECT 1 FROM DUAL
      testWhileIdle: true
      testOnBorrow: false
      testOnReturn: false
      poolPreparedStatements: true
      filters: stat,wall,log4j
      maxPoolPreparedStatementPerConnectionSize: 20
      useGlobalDataSourceStat: true
      connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
  
  
  # 整合mybatis
  mybatis:
    type-aliases-package: com.zh    # 别名
    mapper-locations: classpath:mybatis/mapper/*.xml  # 扫描Mapper.xml
  
  ```

- 创建实体类，导入 Lombok！

  ==**@Mapper表示这个类是一个Mybatis的Mapper**==

  User.java

  ```java
  //@Mapper : 表示本类是一个 MyBatis 的 Mapper
  @Mapper
  @Repository
  public interface UserMapper {
  
      List<User> findAll();
  
      User findById(int id);
  
      int add(User user);
  
      int delete(int id);
  
      int update(User user);
  
  }
  ```

- 对应的Mapper映射文件

  **resources/mybatis/mapper/UserMapper.xml**

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
  <mapper namespace="com.zh.mapper.UserMapper">
  
      <select id="findAll" resultType="User">
          select * from mybatis.user
      </select>
  
      <select id="findById" resultType="User">
          select * from mybatis.user where id = #{id}
      </select>
  
      <insert id="add" parameterType="User">
          insert into mybatis.user (id, name, pwd) values (#{id}, #{name}, #{pwd});
      </insert>
  
      <delete id="delete" >
          delete from mybatis.user where id = #{id}
      </delete>
  
      <update id="update" parameterType="User">
          update mybatis.user set name = #{name},pwd = #{pwd} where id = #{id};
      </update>
  
  </mapper>
  ```

- 编写UserControlle类进行测试

  ```java
  @RestController
  public class UserControlle {
  
      @Autowired
      private UserMapper mapper;
  
      @GetMapping("/findAll")
      public List<User> findAll(){
  
          List<User> list = mapper.findAll();
  
          return list;
      }
  
      @GetMapping("/findById/{id}")
      public User findById(@PathVariable("id") Integer id){
  
          User user = mapper.findById(id);
  
          return user;
      }
  
      @GetMapping("/delete/{id}")
      public String delete(@PathVariable("id") Integer id){
  
          mapper.delete(id);
  
          return "删除成功";
      }
  
      @GetMapping("/add")
      public String add(){
  
          User user = new User(5,"哈哈","888888");
  
          mapper.add(user);
  
          return "添加成功";
      }
  
      @GetMapping("/update")
      public String update(){
  
          User user = new User(5,"10086","00000");
  
          mapper.update(user);
  
          return "修改成功";
      }
  }
  ```

   

# 8、SpringSecurity

## 8.1、简介

SpringSecurity是针对Spring项目的安全框架，也是SpringBoot底层安全模块默认的技术选型，可以实现Web安全控制。对于安全控制，仅需要引入spring-boot-starter-security模块

常用类底层实现类

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

Spring Security的两个主要目标是“认证”和“授权”

参考官网：https://spring.io/projects/spring-security

帮助文档：https://docs.spring.io/spring-security/site/docs/5.3.1.BUILD-SNAPSHOT/reference/html5/

## 8.2、基本配置

- 导入依赖包

  ```xml
  <!-- thymeleaf-Security整合包 -->
  <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-springsecurity4</artifactId>
      <version>3.0.2.RELEASE</version>
  </dependency>
  
  <!--Security-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
  
  <!--thymeleaf-->
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
  </dependency>
  <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-java8time</artifactId>
  </dependency>
  ```

- 配置Controller

  ```java
  @Controller
  public class SecurityController {
  
      @RequestMapping({"/","/index"})
      public String index(){
          return "index";
      }
  
      @RequestMapping({"toLogin"})
      public String toLogin(){
          return "views/login";
      }
  
      @RequestMapping({"level1/{id}"})
      public String level1(@PathVariable("id") Integer id){
          return "views/level1/"+id;
      }
  
      @RequestMapping({"level2/{id}"})
      public String level2(@PathVariable("id") Integer id){
          return "views/level2/"+id;
      }
  
      @RequestMapping({"level3/{id}"})
      public String level3(@PathVariable("id") Integer id){
          return "views/level3/"+id;
      }
  }
  ```

## 8.3、创建SecurityConfig

```java
@EnableWebSecurity  //开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {
}
```

==**@EnableWebSecurity：开启了WebSecurity模式**==

==**继承这个方法WebSecurityConfigurerAdapter：自定义Security策略。**==

==**Security是链式编程**==

```java
@EnableWebSecurity  //开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        //链式编程

        /**
         * 首页所有人可以访问，功能页只有对应权限的人才能访问
         *
         * antMatchers：访问路径
         * permitAll：全部用户
         * hasRole：对应的权限
         */
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");


        /**
         * 没有权限，跳转默认登录页面    /login
         * 跳转自定义登录页面
         * .loginPage("自定义请求");
         * 将此处的请求写在from中的请求，登录调用Security的判断
         * 或者使用.loginProcessingUrl("请求")这里是登录请求
         * loginPage去登陆
         *
         *
         * 自定义from表单属性
         * .usernameParameter("user")
         * .passwordParameter("pwd")
         */
        http.formLogin().loginPage("/toLogin").usernameParameter("user").passwordParameter("pwd");

        /**
         * 开启注销，跳到首页
         * .deleteCookies("remove")          删除cookies
         * .invalidateHttpSession(false)     清除session
         *
         * a标签是get请求
         * 防止网站攻击
         * 解决方法：使用post，或者关闭csrf
         */
        http.csrf().disable();  //关闭csrf功能
        http.logout().logoutSuccessUrl("/");

        /**
         * 开启记住我功能
         * 保存在 cookie 默认保存两周
         * 自定义记住我
         * .rememberMeParameter("name值");
         */
        http.rememberMe().rememberMeParameter("remember");
    }


    /**
     * 认证
     * 密码编码：PasswordEncoder
     * 使用Security的登录，密码不能使用明文，应对密码进行加密
     * Spring Security 5.0+ 新增自带加密方法
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //数据正常从数据库取出
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
                .and()
                .withUser("admin").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and()
                .withUser("beloved").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");

    }
}
```

## 8.4、Security与Thymeleaf整合

**相关jar包**

```xml
<!-- thymeleaf-Security整合包 -->
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity4</artifactId>
    <version>3.0.2.RELEASE</version>
</dependency>
```

==**在页面使用Security需要引入约束**==

==**Security标签sec**==

```html
xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4"
```

**判断是否登录**

```html
<!--以登录-->
<div sec:authorize="isAuthenticated()">
</div>
<!--未登录-->
<div sec:authorize="!isAuthenticated()">
</div>
```

**获取用户名**

```html
<span sec:authentication="name"></span>
```

**判断用户权限**

```html
<!--菜单根据用户权限判断动态实现-->
<div sec:authorize="hasRole('权限值')">
</div>
```

## 8.5、注意事项

==**Spring Security与Spring Boot、Thymeleaf结合对Spring Boot的版本有要求。Spring Boot最高版本是2.0.9不能超过2.1.0，推荐使用2.0.7**==

**使用Security的登录，密码不能使用明文，应对密码进行加密。如果使用明文密码会报错：PasswordEncoder，要进行密码加密**

**注销登录：如果使用<a>标签，是get请求，不支持get请求，防止网站攻击。解决方法：使用post提交或者关闭csrf**

# 9、Shiro

## 9.1、简介

- Apache Shiro是一个java的安全(权限)框架
- Shiro不仅可以在javaSE环境使用，也可以在JavaEE环境使用。它有自己的Session
- Shiro可以完成，认证，授权，加密，会话管理，Web集成，缓存等
- 官网地址：http://shiro.apache.org/

## 9.2、有哪些功能

![image-20200324093548146](http://image.beloved.ink/Typora/image-20200324093548146.png)

- **Authentication**：身份认证，登录，验证用户是不是拥有相应的身份
- **Authorization**：授权，即权限验证，验证某个已认证的用户是否拥有某个权限，即判断用户是否能进行什么操作，如：验证某个用户是否拥有某个角色，或者验证某个用户对某个资源是否具有某个权限
- **Session Management**：会话管理，用户登录后就是第一次会话，在没有退出之前，他的所有信息都在会话中；会话可以是普通的JavaSE环境，也可以是WEB环境
- **Cryptography**：机密，保护数据的安全性，如密码加密存储到数据库中，而不是明文存储
- **Web Support**：web支持，可以非常容易的集成到web环境
- **Caching**：缓存，比如用户登录后，其用户信息，拥有的角色，权限不必每次去查，可以提高效率
- **Concurrency**：Shiro支持多线程应用的并发验证，即，如在一个线程中开启另一个线程，能把权限自动的传播过去
- **Testing**：提供测试支持
- **Run As**：允许一个用户假装为另一个用户的身份访问
- **Remember Me**：记住我，即一次登录后，下次不用登录

## 9.3、Shiro架构(外部)

![image-20200324095235378](http://image.beloved.ink/Typora/image-20200324095235378.png)

**Subject**：应用代码直接交互的对象是Subject，也就是说Shiro的对外API核心就是Subject，Subject代表了当前用户，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫，机器人等，与Subject的所有交互都会委托给Session Management；Subject其实是一个门面，Session Management才是执行者

**Session Management**：安全管理器，即所有与安全有关的操作都会与Session Management交互，并且它管理着所有的Subject，可以看出是Shiro核心，它负责与Shiro的其他组件进行交互，相当于SpringMVC的DispatcherServlet角色

**Realm**：Shiro从Realms获取安全数据（如用户，角色，权限），就是说Session Management要验证用户身份，那么它需要从Realm获取相应的用户进行比较，来确认用户是否合法；也需要从Realm得到用户相应的角色，权限，进行验证用户的操作是否能够进行，可以把Realm看成DataSource

## 9.4、Shiro架构(内部)

![image-20200324100750156](http://image.beloved.ink/Typora/image-20200324100750156.png)

- **Subject**：任何可以与应用交互的'用户'
- **Security Management**：相当于SpringMVC中DispatcherServlet是Shiro的心脏，所有具体的交互都通过Security Management进行控制，它管理着所有的Subject，且负责进行认证，授权，会话，及缓存的管理
- **Authenticator**：负责Subject 认证，是一个扩展点，可以自定义实现，可以使用认证策略（AuthenticationStrategy）即什么情况下算用户认证通过了
- **Authorizer**：授权器，即访问控制器，用来决定主体是否有权限进行相应的操作，即控制着用户能访问应用中的哪些功能
- **Realm**：可以有一个或者多个的realm，可以认为是安全实体数据源，即用于获取安全实体的，可以用JDBC实现，也可以是内存实现等等，由用户提供，所以一般在应用中都需要实现自己的realm
- **Session Management**：管理session生命周期的组件，而Shiro并不仅仅可以用在Web环境，也可以用在普通的JavaStE环境中
- **CacheManager**：缓存控制器，来管理如用户，角色，权限等缓存的，因为这些数据基本上很少改变，放到缓存中后可以提高访问的性能
- **Cryptography**：密码模块，Shiro提高了一些常见的加密组件用于密码加密，解密等

## 9.5、Hello Shiro

官方文档：http://shiro.apache.org/tutorial.html

官方quickstart：https://github.com/apache/shiro/tree/master/samples/quickstart

- 导入依赖

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.apache.shiro</groupId>
          <artifactId>shiro-core</artifactId>
          <version>1.4.0</version>
      </dependency>
  
      <!-- configure logging -->
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>jcl-over-slf4j</artifactId>
          <version>1.7.21</version>
      </dependency>
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-log4j12</artifactId>
          <version>1.7.21</version>
      </dependency>
      <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>1.2.17</version>
      </dependency>
  </dependencies>
  ```

- 导入shiro.ini文件

  ```ini
  [users]
  # user 'root' with password 'secret' and the 'admin' role
  root = secret, admin
  # user 'guest' with the password 'guest' and the 'guest' role
  guest = guest, guest
  # user 'presidentskroob' with password '12345' ("That's the same combination on
  # my luggage!!!" ;)), and role 'president'
  presidentskroob = 12345, president
  # user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
  darkhelmet = ludicrousspeed, darklord, schwartz
  # user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
  lonestarr = vespa, goodguy, schwartz
  
  # -----------------------------------------------------------------------------
  # Roles with assigned permissions
  # 
  # Each line conforms to the format defined in the
  # org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
  # -----------------------------------------------------------------------------
  [roles]
  # 'admin' role has all permissions, indicated by the wildcard '*'
  admin = *
  # The 'schwartz' role can do anything (*) with any lightsaber:
  schwartz = lightsaber:*
  # The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
  # license plate 'eagle5' (instance specific id)
  goodguy = winnebago:drive:eagle5
  ```

- 导入log4j.properties

  ```properties
  log4j.rootLogger=INFO, stdout
  
  log4j.appender.stdout=org.apache.log4j.ConsoleAppender
  log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
  log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n
  
  # General Apache libraries
  log4j.logger.org.apache=WARN
  
  # Spring
  log4j.logger.org.springframework=WARN
  
  # Default Shiro logging
  log4j.logger.org.apache.shiro=INFO
  
  # Disable verbose logging
  log4j.logger.org.apache.shiro.util.ThreadContext=WARN
  log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
  ```

- Quickstart.java

  ```java
  import org.apache.shiro.SecurityUtils;
  import org.apache.shiro.authc.*;
  import org.apache.shiro.config.IniSecurityManagerFactory;
  import org.apache.shiro.mgt.SecurityManager;
  import org.apache.shiro.session.Session;
  import org.apache.shiro.subject.Subject;
  import org.apache.shiro.util.Factory;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  
  
  /**
   * Simple Quickstart application showing how to use Shiro's API.
   *
   * @since 0.9 RC2
   */
  public class Quickstart {
  
      private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
  
  
      public static void main(String[] args) {
  
          // The easiest way to create a Shiro SecurityManager with configured
          // realms, users, roles and permissions is to use the simple INI config.
          // We'll do that by using a factory that can ingest a .ini file and
          // return a SecurityManager instance:
  
          // Use the shiro.ini file at the root of the classpath
          // (file: and url: prefixes load from files and urls respectively):
          Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
          SecurityManager securityManager = factory.getInstance();
  
          // for this simple example quickstart, make the SecurityManager
          // accessible as a JVM singleton.  Most applications wouldn't do this
          // and instead rely on their container configuration or web.xml for
          // webapps.  That is outside the scope of this simple quickstart, so
          // we'll just do the bare minimum so you can continue to get a feel
          // for things.
          SecurityUtils.setSecurityManager(securityManager);
  
          // Now that a simple Shiro environment is set up, let's see what you can do:
  
          // get the currently executing user:
          //获取当前的用户
          Subject currentUser = SecurityUtils.getSubject();
  
          // Do some stuff with a Session (no need for a web or EJB container!!!)
          //通过当前用户拿到session
          Session session = currentUser.getSession();
          session.setAttribute("someKey", "aValue");
          String value = (String) session.getAttribute("someKey");
          if (value.equals("aValue")) {
              log.info("Retrieved the correct value! [" + value + "]");
          }
  
          // let's login the current user so we can check against roles and permissions:
          //判断当前用户是否被认证
          if (!currentUser.isAuthenticated()) {
              //根据当前用户拿到令牌    token 令牌
              UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
              token.setRememberMe(true);  //记住我
              try {
                  //执行了登录操作
                  currentUser.login(token);
              } catch (UnknownAccountException uae) {
                  log.info("There is no user with username of " + token.getPrincipal());
              } catch (IncorrectCredentialsException ice) {
                  log.info("Password for account " + token.getPrincipal() + " was incorrect!");
              } catch (LockedAccountException lae) {
                  log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                          "Please contact your administrator to unlock it.");
              }
              // ... catch more exceptions here (maybe custom ones specific to your application?
              catch (AuthenticationException ae) {
                  //unexpected condition?  error?
              }
          }
  
          //say who they are:
          //print their identifying principal (in this case, a username):
          log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
  
          //test a role:
          //判断当前用户角色
          if (currentUser.hasRole("schwartz")) {
              log.info("May the Schwartz be with you!");
          } else {
              log.info("Hello, mere mortal.");
          }
  
          //判断权限 粗粒度
          //test a typed permission (not instance-level)
          if (currentUser.isPermitted("lightsaber:wield")) {
              log.info("You may use a lightsaber ring.  Use it wisely.");
          } else {
              log.info("Sorry, lightsaber rings are for schwartz masters only.");
          }
  
  
          //细粒度
          //a (very powerful) Instance Level permission:
          if (currentUser.isPermitted("winnebago:drive:eagle5")) {
              log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                      "Here are the keys - have fun!");
          } else {
              log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
          }
  
  
          //注销
          //all done - log out!
          currentUser.logout();
  
          //结束
          System.exit(0);
      }
  }
  ```

## 9.6、整合SpringBoot

### 9.6.1、导入相关依赖包

```xml
<!-- thymeleaf与shiro整合 -->
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>


<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>

<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<!--log4j-->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

<!--druid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.20</version>
</dependency>

<!--mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.0</version>
</dependency>

<!-- shiro-spring整合包 -->
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.4.0</version>
</dependency>

<!--Thymeleaf-->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>

<!--Web-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### 9.6.2、编写UserRealm

==**需要继承AuthorizingRealm父类，实现方法**==

```java
//自定义 UserRealm extends AuthorizingRealm
public class UserRealm extends AuthorizingRealm {

    @Autowired
    private UserService service;

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了授权");

        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();

        //得到当前登录对象
        Subject subject = SecurityUtils.getSubject();
        //拿到user对象
        User user = (User) subject.getPrincipal();

        //设置当前用户的权限
        info.addStringPermission(user.getPerms());

        return info;
    }


    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了认证");

        //获取用户令牌
        UsernamePasswordToken userToken = (UsernamePasswordToken) token;

        //连接数据库
        User user = service.findByName(userToken.getUsername());

        if (user == null){
            //return null; //抛出异常 UnknownAccountException
            throw new LockedAccountException();
        }


        /**
         * 密码认证 shiro做
         * 密码加密shiro有默认的加密方式SimpleCredentialsMatcher
         *
         * CredentialsMatcher实现类可查看加密方式
         *
         * SimpleAuthenticationInfo可以传入四个参数
         * 第一个参数：传入的都是com.java.entity包下的User类的user对象。
         * 第二个参数:  传入的是从数据库中获取到的password，然后再与token中的password进行对比，匹配上了就通过，匹配不上就报异常。
         * 第三个参数，盐–用于加密密码对比。 若不需要，则可以设置为空 “ ”
         * 第四个参数：当前realm的名字。
         */
        return new SimpleAuthenticationInfo(user,user.getPwd(),"");
    }
}
```

### 9.6.3、编写ShiroConfig

```java
@Configuration
public class ShiroConfig {

    //ShiroFilterFactoryBean        3
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();

        //关联DefaultWebSecurityManager
        //设置安全管理器
        bean.setSecurityManager(securityManager);

        /**
         * 添加shiro的内置过滤器
         *
         * anon：无需认证就可以访问
         * authc：必须认证才可以访问
             * user：必须拥有 记住我 功能才能访问
         * perms：拥有对某个资源的权限才能访问
         * role：拥有某个角色权限才能访问
         */
        //拦截请求
        Map<String,String> filterMap = new HashMap<>();
        filterMap.put("/level1/*","anon");
        filterMap.put("/level2/*","authc,perms[vip1]");
        filterMap.put("/level3/*","authc,perms[vip2]");

        bean.setFilterChainDefinitionMap(filterMap);

        //设置登录请求
        bean.setLoginUrl("/toLogin");

        //未授权页面
        bean.setUnauthorizedUrl("/unauthorized");

        return bean;
    }


    //DefaultWebSecurityManager     2
    @Bean(name = "securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();

        //关联UserRealm
        securityManager.setRealm(userRealm);
        securityManager.setRememberMeManager(rememberMeManager());

        return securityManager;
    }

    //创建realm对象，需要自定义       1
    @Bean(name = "userRealm")
    public UserRealm userRealm(){
        return new UserRealm();
    }

    //配置ShiroDialect：用来整合Shiro和thymeleaf
      @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }


    /**
     * cookie对象
     * @return
     */
    public SimpleCookie rememberMeCookie() {
        //这个参数是cookie的名称，对应前端的checkbox的name = rememberMe
        SimpleCookie simpleCookie = new SimpleCookie("rememberMe");
        //setcookie的httponly属性如果设为true的话，会增加对xss防护的安全系数。它有以下特点：

        //setcookie()的第七个参数
        //设为true后，只能通过http访问，javascript无法访问
        //防止xss读取cookie
        simpleCookie.setHttpOnly(true);
        simpleCookie.setPath("/");
        //<!-- 记住我cookie生效时间30天 ,单位秒;-->
        simpleCookie.setMaxAge(2592000);
        return simpleCookie;
    }

    /**
     * cookie管理对象
     * @return
     */
    public CookieRememberMeManager rememberMeManager() {
        CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
        cookieRememberMeManager.setCookie(rememberMeCookie());
        //rememberMe cookie加密的密钥 建议每个项目都不一样 默认AES算法 密钥长度(128 256 512 位)
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));
        return cookieRememberMeManager;
    }
}
```

### 9.6.4、创建ShiroController

```java
@Controller
public class ShiroController {

    @RequestMapping("/unauthorized")
    @ResponseBody
    public String unauthorized(){
        return "没有权限访问此页面";
    }

    @RequestMapping({"/","/index"})
    public String index(){
        return "index";
    }

    @RequestMapping({"toLogin"})
    public String toLogin(){
        return "views/login";
    }

    @RequestMapping({"level1/{id}"})
    public String level1(@PathVariable("id") Integer id){
        return "views/level1/"+id;
    }

    @RequestMapping({"level2/{id}"})
    public String level2(@PathVariable("id") Integer id){
        return "views/level2/"+id;
    }

    @RequestMapping({"level3/{id}"})
    public String level3(@PathVariable("id") Integer id){
        return "views/level3/"+id;
    }

    @RequestMapping("/login")
    public String login(String user,String pwd,boolean rememberMe,Model model){
        System.out.println("记住我"+rememberMe);
        //获取当前用户
        Subject subject = SecurityUtils.getSubject();
        //封装用户登录数据
        UsernamePasswordToken token = new UsernamePasswordToken(user, pwd,rememberMe);
        try {
            //执行登录方法，没有异常ok
            subject.login(token);
            //成功信息存入session
//            Subject subject = SecurityUtils.getSubject();
            Session session = subject.getSession();
            User userInfo = (User) subject.getPrincipal();
            session.setAttribute("user",userInfo);
            return "index";
        }catch (LockedAccountException lae){
            model.addAttribute("msg","用户名或密码错误");
            return "views/login";
        }catch (UnknownAccountException e){ //用户名不存在
            model.addAttribute("msg","用户名错误");
            return "views/login";
        }catch (IncorrectCredentialsException e){ //密码不存在
            model.addAttribute("msg","密码错误");
            return "views/login";
        }
    }

    @RequestMapping("/logout")
    public String logout(){
        Subject subject = SecurityUtils.getSubject();
        subject.logout();
        return "index";
    }
}
```

### 9.6.5、Shiro结合Thymeleaf

==<shiro>标签约束==

```html
xmlns:shiro="http://www.pollix.at/thymeleaf/shiro"
```

==权限判断==

```html
shiro:hasPermission="vip1"
```

==常用<shiro>标签==

```html
<!--验证当前用户是否拥有指定权限。  -->
<a shiro:hasPermission="user:add" href="#" >add用户</a><!-- 拥有权限 -->
 
 
<!--与hasPermission标签逻辑相反，当前用户没有制定权限时，验证通过。-->
<p shiro:lacksPermission="user:del"> 没有权限 </p>
 
 
<!--验证当前用户是否拥有以下所有权限。-->
<p shiro:hasAllPermissions="user:view, user:add"> 权限与判断 </p>
 
 
<!--验证当前用户是否拥有以下任意一个权限。-->
<p shiro:hasAnyPermissions="user:view, user:del"> 权限或判断 </p>
 
 
<!--验证当前用户是否属于该角色。-->
<a shiro:hasRole="admin" href="#">拥有该角色</a>
 
 
<!--与hasRole标签逻辑相反，当用户不属于该角色时验证通过。-->
<p shiro:lacksRole="developer"> 没有该角色 </p>
 
 
<!--验证当前用户是否属于以下所有角色。-->
<p shiro:hasAllRoles="developer, admin"> 角色与判断 </p>
 
 
<!--验证当前用户是否属于以下任意一个角色。-->
<p shiro:hasAnyRoles="admin, vip, developer"> 角色或判断 </p>
 
 
<!--验证当前用户是否为“访客”，即未认证（包含未记住）的用户。-->
<p shiro:guest="">访客 未认证</a></p>
 
 
<!--认证通过或已记住的用户-->
<p shiro:user=""> 认证通过或已记住的用户 </p>
    
 
<!--已认证通过的用户。不包含已记住的用户，这是与user标签的区别所在。-->
<p shiro:authenticated=""> <span shiro:principal=""></span> </p>
    
 
<!--输出当前用户信息，通常为登录帐号信息-->
<p> <shiro:principal/> </p>
 
 
<!--未认证通过用户，与authenticated标签相对应。-->
<!--与guest标签的区别是，该标签包含已记住用户。-->
<p shiro:notAuthenticated=""> 未认证通过用户 </p>
```

### 9.6.6、注意

==记住我需要给pojo序列化==

我们的对象并不只是存在内存中，还需要传输网络，或者保存起来下次再加载出来用，所以需要Java序列化技术。

Java序列化技术正是将对象转变成一串由二进制字节组成的数组，可以通过将二进制数据保存到磁盘或者传输网络，磁盘或者网络接收者可以在对象的属类的模板上来反序列化类的对象，达到对象持久化的目的。

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {

    private Integer id;
    private String name;
    private String pwd;
    private String perms;

}
```

# 10、Swagger

## 10.1、简介

- RestFuI Api 文档自动在线生成工具    API文档与API定义同步更新
- 直接允许，可以在线测试API接口
- 支持多种语言

官网：https://swagger.io/

## 10.2、SpringBoot集成Swagger

### 10.2.1、导入相关依赖

```xml
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

### 10.2.2、swagger_demo

- 编写hello请求

  ```java
  @RestController
  public class HelloController {
  
      @RequestMapping(value = "/hello")
      public String hello(){
          return "Hello Swagger";
      }
  
  }
  ```

- 集成Swagger

  ```java
  @Configuration
  @EnableSwagger2    //开启Swagger2
  public class SwaggerConfig {
  }
  ```

- 测试运行

  http://localhost:8080/swagger-ui.html

  ![image-20200325110203192](http://image.beloved.ink/Typora/image-20200325110203192.png)

## 10.3、配置Swagger

```java
@Configuration
@EnableSwagger2    //开启Swagger2
public class SwaggerConfig {

    //配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
    }

    //配置Swagger信息==apiInfo
    private ApiInfo apiInfo(){
        //作者信息
        Contact contact = new Contact("Beloved","beloved.ink","1425279634@qq.com");
        return new ApiInfo(
                "Beloved的SwaggerAPI文档",
                "描述信息",
                "v1.0", //版本信息
                "beloved.ink",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList()
        );
    }

}
```

![image-20200325111753907](http://image.beloved.ink/Typora/image-20200325111753907.png)

## 10.4、Swagger配置扫描接口

Docket.select()

```java
//配置Swagger的Docket的bean实例
@Bean
public Docket docket(){
    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .select()
            /**
             * RequestHandlerSelectors：配置要扫描接口的方式
             * basePackage：指定要扫描的包
             * any()：扫描全部
             * none()：不扫描
             * withClassAnnotation：扫描类上的注解，参数是一个注解的反射对象
             * withMethodAnnotation：扫描方法上的注解
             *
             */
            .apis(RequestHandlerSelectors.basePackage("com.zh.controller"))
            //paths()：过滤什么路径  包路径
            .paths(PathSelectors.ant("/zh/**"))
            .build()
            ;
}
```

## 10.5、配置Swagger的启动

**==.enable(false)     true：启动    false：关闭==**

![image-20200325114836848](http://image.beloved.ink/Typora/image-20200325114836848.png)

**配置Swagger在生产环境中使用，发布的时候不使用**

- 判断是不是生产环境
- 注入enabl()

**创建环境**

![image-20200325115959724](http://image.beloved.ink/Typora/image-20200325115959724.png)

```yaml
# 选择生产环境
spring:
  profiles:
    active: pro
```

```java
//配置Swagger的Docket的bean实例
@Bean
public Docket docket(Environment environment){

    //设置要显示的Swagger的环境
    Profiles profiles = Profiles.of("dev","test");
    //通过environment.acceptsProfiles判断是否处在自己设定的环境当中
    boolean flag = environment.acceptsProfiles(profiles);

    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .enable(flag)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.zh.controller"))
            .build()
            ;
}
```

## 10.6、配置API文档的分组

```java
.groupName("Beloved")
```

![image-20200325120437600](http://image.beloved.ink/Typora/image-20200325120437600.png)

**配置多个分组：多个Docket实例即可**

```java
@Bean
public Docket docket1(){
    return new Docket(DocumentationType.SWAGGER_2)
            .groupName("A")
            ;
}

@Bean
public Docket docket2(){
    return new Docket(DocumentationType.SWAGGER_2)
            .groupName("B")
            ;
}

@Bean
public Docket docket3(){
    return new Docket(DocumentationType.SWAGGER_2)
            .groupName("C")
            ;
}
```

![image-20200325120659130](http://image.beloved.ink/Typora/image-20200325120659130.png)



## 10.7、实体类配置

**POJO**

```java
@ApiModel("用户实体类")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    @ApiModelProperty("用户id")
    private int uid;
    @ApiModelProperty("用户名")
    private String name;
    @ApiModelProperty("密码")
    private String pwd;

}
```

**controller**

```java
@RestController
public class HelloController {

    @ApiOperation("hello请求")
    @GetMapping(value = "/hello")
    public String hello(){
        return "Hello Swagger";
    }

    @ApiOperation("user请求")
    @GetMapping("/user")
    public User user(User user){
        return user;
    }

    @ApiOperation("求和请求")
    @PostMapping("/add")
    public int add(@ApiParam("参数a") int a,@ApiParam("参数b") int b){
        return a+b;
    }

}
```

## 总结

- 可以通过Swagger给一些比较难理解的属性或者接口，增加注释信息
- 接口文档实时更新
- 可以在线测试

# 11、任务

## 11.1、异步任务

创建service线程停止3秒

```java
@Service
public class AsynsService {

    public void hello(){
        try {
            Thread.sleep(3000);//停止三秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("数据正在处理.....");
    }

}
```

创建controller

```java
@RestController
public class AsynsController {

    @Autowired
    private AsynsService service;


    @RequestMapping("/hello")
    public String hello(){
        service.hello();//停止三秒
        return "ok";
    }

}
```

**测试：请求hello请求，程序等待3秒才会响应数据**

==@Async：表示异步任务==

==@EnableAsync  //开启异步任务==

```java
@Service
public class AsynsService {

    //告诉spring这时一个异步任务
    @Async
    public void hello(){
        try {
            Thread.sleep(3000);//停止三秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("数据正在处理.....");
    }

}
```

```java
@EnableAsync  //开启异步任务
@SpringBootApplication
public class Springboot09TaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot09TaskApplication.class, args);
    }

}
```

**测试：请求hello请求，不会等待，直接响应数据**

## 11.2、邮件任务

**导入jar包**

```xml
<!-- javax,mail-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

**源码：MailSenderAutoConfiguration**

配置参数

```yaml
spring:
  mail:
    username: 1425279634@qq.com
    password: vidkhttrxwhghbdi
    host: smtp.qq.com
    # 开启加密验证
    properties:
      mail.smtp.ssl.enable: true
```

测试

```java
package com.zh;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.mail.javamail.MimeMessageHelper;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;
import java.io.File;

@SpringBootTest
class Springboot09TaskApplicationTests {

    @Autowired
    private JavaMailSenderImpl mailSender;

    @Test
    void contextLoads() {

        //创建一个简单的邮件
        SimpleMailMessage mailMessage = new SimpleMailMessage();

        //标题
        mailMessage.setSubject("测试邮件");
        //内容
        mailMessage.setText("测试邮件内容文本");

        //收件人
        mailMessage.setTo("3519422316@qq.com");
        //发件人
        mailMessage.setFrom("1425279634@qq.com");

        mailSender.send(mailMessage);
    }

    @Test
    void contextLoads02() throws MessagingException {

        //创建一个复杂的邮件
        MimeMessage mimeMessage = mailSender.createMimeMessage();

        //组装  multipart：是否支持多文件
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage,true,"utf-8");

        helper.setSubject("你好我是标题");
        //开启html解析
        helper.setText("<h1 style='color:red'>你好我是文本内容</h1>",true);


        //附件
        helper.addAttachment("eye.png",new File("C:\\Users\\Beloved\\Desktop\\answer_ssm\\img\\eye.png"));
        helper.addAttachment("editor.md.zip",new File("C:\\Users\\Beloved\\Desktop\\editor.md-master.zip"));

        //收件人
        helper.setTo("1425279634@qq.com");
        //发件人
        helper.setFrom("1425279634@qq.com");

        mailSender.send(mimeMessage);
    }
}
```

## 11.3、定时任务

```
TaskScheduler	任务调度者
TaskExecutor	任务执行者

@EnableScheduling //开启定时任务
@Scheduled()     什么时候执行
cron表达式
```

**cron在线工具：http://www.bejson.com/othertools/cron/**

```java
@EnableScheduling //开启定时任务
@SpringBootApplication
public class Springboot09TaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot09TaskApplication.class, args);
    }

}
```

```java
@Service
public class ScheduledService {

    //cron表达式
    //秒 分 时 日 月 周几
    /*
     * 0 18 15 * * ?  每天的15点18分0秒执行一次
     */
    @Scheduled(cron = "0/1 0/1 15 * * ?")
    public void hello(){
        System.out.println("hello被执行了");
    }

}
```

**异步任务，不用管，到时间自动执行**

# 12、分布式Dubbo+Zookeeper+SpringBoot

## 12.1、**什么是分布式系统？**

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

## 12.2、Dubbo文档

官网地址：http://dubbo.apache.org/zh-cn/docs/user/preface/background.html

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。

![image](http://dubbo.apache.org/docs/zh-cn/user/sources/images/dubbo-architecture-roadmap.jpg)

#### 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

#### 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，提升效率的方法之一是将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

#### 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式服务框架(RPC)是关键。

#### 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键。

## 12.3、RPC

RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b

**RPC基本原理**

![image-20200413122607060](http://image.beloved.ink/Typora/image-20200413122607060.png)

**步骤解析：**

![image-20200413122657080](http://image.beloved.ink/Typora/image-20200413122657080.png)

RPC两个核心模块：通讯，序列化。

## 12.4、Dubbo+Zookeeper安装

Apache Dubbo 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

dubbo官网 http://dubbo.apache.org/zh-cn/index.html

1.了解Dubbo的特性

2.查看官方文档

**dubbo基本概念**

![image-20200413122859701](http://image.beloved.ink/Typora/image-20200413122859701.png)

**服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

**调用关系说明**

l 服务容器负责启动，加载，运行服务提供者。

l 服务提供者在启动时，向注册中心注册自己提供的服务。

l 服务消费者在启动时，向注册中心订阅自己所需的服务。

l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

### 12.4.1、Dubbo环境搭建

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

zookeeper：http://zookeeper.apache.org/index.html

### 12.4.2、Window下安装zookeeper

1、下载zookeeper ：地址：http://zookeeper.apache.org/releases.html

![image-20200413123256322](http://image.beloved.ink/Typora/image-20200413123256322.png)

2、运行/bin/zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件；

可能遇到问题：闪退 !

解决方案：编辑zkServer.cmd文件末尾添加pause 。这样运行出错就不会退出，会提示错误信息，方便找到原因。

![image-20200413123838703](http://image.beloved.ink/Typora/image-20200413123838703.png)

![image-20200413123941446](http://image.beloved.ink/Typora/image-20200413123941446.png)

3、修改zoo.cfg配置文件

将conf文件夹下面的zoo_sample.cfg复制一份改名为zoo.cfg即可。

注意几个重要位置：

dataDir=./  临时数据存储的目录（可写相对路径）

clientPort=2181  zookeeper的端口号

修改完成后再次启动zookeeper

4、使用zkCli.cmd测试

ls /：列出zookeeper根下保存的所有节点

create –e /abc 123：创建一个abc节点，值为123

get /abc：获取/abc节点的值

### 14.4.3、window下安装dubbo-admin

dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

**1、下载dubbo-admin**

地址 ：https://github.com/apache/dubbo-admin/tree/master

**2、解压进入目录**

修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址

一般默认即可

```
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest

dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3、在项目目录下**打包dubbo-admin

```
mvn clean package -Dmaven.test.skip=true
```

**第一次打包的过程有点慢，需要耐心等待！直到成功！**

4、执行 dubbo-admin\target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

```
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

**【注意：zookeeper的服务一定要打开！】**

执行完毕，我们去访问一下 http://localhost:7001/ ， 这时候我们需要输入登录账户和密码，我们都是默认的root-root；

登录成功后，查看界面

![image-20200413125757270](http://image.beloved.ink/Typora/image-20200413125757270.png)

## 12.5、Springboot+Dubbo+Zookeeper

### 12.5.1、框架搭建

**1. 启动zookeeper ！**

**2. IDEA创建一个空项目；**

**3.创建一个模块，实现服务提供者：provider-server ， 选择web依赖即可**

**4.项目创建完毕，我们写一个服务，比如卖票的服务；**

编写接口

```java
package com.zh.service;

public interface TicketService {

    public String getTicket();

}
```

编写实现类

```java
package com.zh.service;
public class TicketServiceImpl implements TicketService {

    @Override
    public String getTicket() {
        return "《Java入门到放弃》";
    }
}
```

**5.创建一个模块，实现服务消费者：consumer-server ， 选择web依赖即可**

**6.项目创建完毕，我们写一个服务，比如用户的服务；**

编写service

```java
package com.zh.service;

import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //注册到ioc容器
public class UserService {

    //想要拿到provider-server中的值，要去注册中心获取服务
   
}
```

### 12.5.2、服务提供者

**1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包**

**我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包**

```xml
<!-- dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.3</version>
</dependency>
```

**zookeeper的包我们去maven仓库下载，zkclient；**

```xml
<!-- Zookeeper客户端 -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
```

**【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；**

```xml
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.0.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.0.1</version>
</dependency>
<!--日志会冲突-->
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.0</version>
    <!--排除slf4j-log4j12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**2、在springboot配置文件中配置dubbo相关属性！**

```properties
server.port=8001

# 服务应用名
dubbo.application.name=provider-server
# 注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 哪些服务要被注册
dubbo.scan.base-packages=com.zh.service
```

**3、在service的实现类中配置服务注解，发布服务！注意导包问题**

```java
package com.zh.service;

import org.apache.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

@Service    //使用dubbo的@service注解，将服务发布出去
@Component  //注册到ioc容器中
public class TicketServiceImpl implements TicketService {

    @Override
    public String getTicket() {
        return "《Java入门到放弃》";
    }
}
```

**逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有@component注解的服务，将它发布在指定的注册中心中！**

### 12.5.3、服务消费者

**1、导入依赖，和之前的依赖一样；**

```xml
<!--导入Dubbo+Zookeeper依赖-->
<!-- dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.3</version>
</dependency>
<!--Zookeeper依赖-->
<!-- Zookeeper客户端 -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.0.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.0.1</version>
</dependency>
<!--日志会冲突-->
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.0</version>
    <!--排除slf4j-log4j12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

2、**配置参数**

```properties
server.port=8002

# 消费者名
dubbo.application.name=consumer-server
# 注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；**

![image-20200413130829680](http://image.beloved.ink/Typora/image-20200413130829680.png)

**4. 完善消费者的服务类**

```java
package com.zh.service;

import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //注册到ioc容器
public class UserService {

    //想要拿到provider-server中的值，要去注册中心获取服务
    //远程引用指定的服务，他会按照全类名进行匹配，看谁给注册中心注册了这个全类名
    @Reference //引用  Pom坐标，可以定义相同路径的接口名
    TicketService ticketService;

    public void getTicket(){
        String ticket = ticketService.getTicket();

        System.out.println("在注册中心拿到"+ticket);
    }

}
```

**5. 测试类编写；**

```java
package com.zh;

import com.zh.service.UserService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ConsumerServerApplicationTests {

    @Autowired
    private UserService userService;

    @Test
    void contextLoads() {

        userService.getTicket();
    }

}
```

### 12.5.4、启动测试

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控【可以不用做】**

**3. 开启服务者**

**4. 消费者消费测试，结果：**

![image-20200413131403880](http://image.beloved.ink/Typora/image-20200413131403880.png)

**监控中心 ：**

![image-20200413131430871](http://image.beloved.ink/Typora/image-20200413131430871.png)

### 12.5.5、步骤总结

**前提：zookeeper服务已开启**

1. 提供者提供服务
   - 导入依赖
   - 配置注册中心的地址，以及服务发现名，和要扫描的包
   - 在要被注册的服务上添加@service注解，dubbo的
2. 消费者如何消费
   - 导入依赖
   - 配置注册中心地址，配置自己的服务名
   - 从远程注入服务@Reference













































