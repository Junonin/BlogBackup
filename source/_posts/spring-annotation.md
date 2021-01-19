---
title: Spring常用注解
abbrlink: 85ef0b3a
date: 2021-01-14 11:30:49
description: Spring常用注解
categories:
  - JAVA
  - Spring
top:
tags: Spring
---

## 前言

> Spring常用注解

## Annotation

### @Controller

标识一个该类是Spring MVC controller处理器

### @RestController

Spring4之后加入的注解，原来在`@Controller`中返回json需要`@ResponseBody`来配合，如果直接用`@RestController`替代`@Controller`就不需要再配置`@ResponseBody`，默认返回json格式。

### @ResponseBody

将`controller`的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。

### @Service

标注业务层组件，用注解的方式把这个类注入到spring配置中

### @Autowired

用来装配bean，都可以写在字段上，或者方法上。
默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，例如：@Autowired(required=false)

### @RequestMapping

类定义处: 提供初步的请求映射信息，相对于 WEB 应用的根目录。
方法处: 提供进一步的细分映射信息，相对于类定义处的 URL。

### @RequestParam

将请求参数区数据映射到功能处理方法的参数上，例如`@RequestParam(value = "xxx")`

### @ModelAttribute

`@ModelAttribute`注解，实际上是一种接受参数并且自动放入Model对象中

1. 标记在方法上，会在每一个`@RequestMapping`标注的方法前执行，如果有返回值，则自动将该返回值加入到`ModelMap`中。
2. 标记在方法的参数上，会将客户端传递过来的参数按名称注入到指定对象中，并且会将这个对象自动加入`ModelMap`中，便于View层使用。

### @PostConstruct

标记是在项目启动的时候执行这个方法。用来修饰一个`非静态的void()方法`。
spring容器启动时就执行，多用于一些全局配置、数据字典之类的加载。
被`@PostConstruct`修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。`@PostConstruct`在构造函数之后执行,`init()`方法之前执行。`PreDestroy()`方法在`destroy()`方法执行执行之后执。

### @PreDestroy


### @Repository

用于标注数据访问组件，即DAO组件

### @Component

泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注

### @Scope

...

### @SessionAttributes

...

### @Required

适用于bean属性setter方法，并表示受影响的`bean`属性必须在XML配置文件在配置时进行填充。否则，容器会抛出一个`BeanInitializationException异常`。

### @Qualifier

当你创建多个具有相同类型的 `bean` 时，并且想要用一个属性只为它们其中的一个进行装配，在这种情况下，你可以使用`@Qualifier`注释和`@Autowired`注释通过指定哪一个真正的 bean 将会被装配来消除混乱。

### @Resource

...

