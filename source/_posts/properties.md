---
title: properties
abbrlink: 87c331c7
date: 2021-01-20 17:16:29
description: Spring properties yml yaml
categories:
  - JAVA
  - Spring
top: 
tags: Spring
---

## 前言

> Spring properties yml yaml记录

## spring.profiles.active和spring.profiles.include的区别与使用

### spring.profiles.active属性
- 遵循application-${profile}.properties。
1. 开发环境配置文件：application-dev.properties
2. 测试环境配置文件：application-test.properties
3. 生产环境配置文件：application-prod.properties

- 根据部署场景不同，切换不同的配置文件：配置spring.profiles.active，属性值为${profile}。
1. spring.profiles.active=dev：启用application-dev.properties
2. spring.profiles.active=test：启用application-test.properties
3. spring.profiles.active=prod：启用application-prod.properties

- 启动时指定： 在执行有参启动时，可以在命令中进行指定要选用的配置文件，例如：
`java -jar xx.jar --spring.profiles.active=dev`
这个命令的优先级是最高的，即使application.properties中已经配置spring.profiles.active=dev，最终程序还是会用application-test.properties配置文件。

### spring.profiles.include属性

- pring.profiles.include属性：同时启用其他的profile
即在启用application-dev.properties开发环境（主）配置文件时，同时启用application-dev1.properties和application-dev2.propertie。

- 配置方法
1. 若是properties文件：`spring.profiles.include=dev1,dev2`
2. 若是yaml文件中，
    ```yml
    spring.profiles.include:
    -dev1
    -dev2
    ```
    或者：`spring.profiles.include:dev1,dev2`


## 配置多个properties，多环境配置

- application.properties/yml配置
  
  `spring.profiles.active=@profileActive@`
  ```yml
    spring:
    profiles:
        active: @profileActive@
  ```

- pom.xml中profiles配置
  ```yml
<!-- 多环境配置方案 -->
<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <profileActive>dev</profileActive>
        </properties>
        <activation>
            <!-- 默认情况下使用dev开发配置 如 打包时不包含 -p 参数 -->
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <profileActive>prod</profileActive>
        </properties>
    </profile>
</profiles>

<resources>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
    </resource>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
            <include>**/*.json</include>
            <!-- resource可以用${profileActive}配置 -->
            <include>application-${profileActive}.yml</include>
        </includes>
    </resource>
</resources>
  ```
- 多环境使用
打包命令

```shell
mvn package -P dev -DskipTests

mvn package -P prod -DskipTests
```

在dev环境下使用
`@Profile("dev")`


## Spring Boot读取properties配置文件中的数据

### 1、使用@Value注解读取
    application.properties/yml：
    ```yml
    demo.name=Name
    demo.age=18
    ```
    JAVA代码：
    ```java

    @Value("${demo.name}")
    private String name;

    @Value("${demo.age}")
    private String age;

    ```

### 2、使用Environment读取
    application.properties/yml：
    ```yml
    demo.name=Name
    demo.age=18
    ```
    JAVA代码：
    ```java
    
    import org.springframework.core.env.Environment;

    @Autowired
    private Environment environment;

    environment.getProperty("demo.name")
    environment.getProperty("demo.age")

    ```
    如果发现中文乱码可以修改application.properties/yml:
    ```yml
    server.tomcat.uri-encoding=UTF-8
    spring.http.encoding.charset=UTF-8
    spring.http.encoding.enabled=true
    spring.http.encoding.force=true
    spring.messages.encoding=UTF-8
    ```

### 3、使用@ConfigurationProperties注解读取
    application.properties/yml：
    ```yml
    demo.web.title=Name
    demo.web.age=18
    ```
    JAVA代码：
    ```java

    @Data    
    @Component
    @ConfigurationProperties(prefix = "demo")   //节点
    @PropertySource(value = "config.properties")
    public class ConfigBeanProp {
    
        private final Web web = new Web(); //子节点

        @Data
        public static class Web {

            private String title = "xxx";  //可以设置缺省值

            private String age = "20";

        }

    }
    ```
    @Component 表示将该类标识为Bean

    @ConfigurationProperties(prefix = "demo")用于绑定属性，其中prefix表示所绑定的属性的前缀。

    @PropertySource(value = "config.properties")表示配置文件路径。
