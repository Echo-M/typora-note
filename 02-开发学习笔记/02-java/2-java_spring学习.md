[TOC]

# 项目结构

先简单看一下测试项目的结构，用maven构建的，四个包：
entity：存储实体，里面只有一个User类
dao：数据访问，一个接口，两个实现类
service：服务层，一个接口，一个实现类，实现类依赖于IUserDao
test：测试包 



# spring入门

## 一、spring的4种关键策略

### 1、spring通过4种关键策略来简化Java的开发

- 基于POJO的轻量级和最小侵入性编程；
- 通过依赖注入和面向接口实现松耦合；
- 基于切面和管理进行声明式编程；
- 通过切面和模板减少样板式代码。

## 二、什么是依赖注入（DI）

依赖注入的主要目的：是让容器去产生一个对象的实例,然后在整个生命周期中使用他们，同时也让testing工作更加容易。

### 1、3种注入方式

Spring通过DI（依赖注入）实现IOC（控制反转），常用的注入方式主要有三种：构造方法注入，setter注入，基于注解的注入。

#### 1.1 基于autowired注解进行对象注入

需求：创建这个类的一个单例对象。然后可以很方便的在各个模块中用@AutoWired进行对象注入。

**(1) 对自己创建的类进行对象注入**

1. 使用一个对象之前，首先要创建它。创建一个对象在Spring中有固定的模式，在定义类的时候使用@Component注解，@Component默认是单例的。这样spring framework在进行component scan的时候就会创建这个对象。
2. 用的时候很简单，只需要@Autowired就可以了。@Autowired最好使用构造器注入，也就是说，不是直接将@Autowired放在成员变量上面，而是放在构建函数上面，然后通过构造函数的参数注入。为何要这么麻烦？

**(2) 使用成熟的包中的对象**

> 1. 使用配置文件生成bean
>
> ```java
> <bean class="xxx"></bean>
> ```
>
> 2. 使用@Configuration注解生成bean
>
> ```java
> @Configuration
> public class AppConfig {
>     @Bean(name = "timedCache")
>     public TimedCache<String, Muser> listStrBean() {
>         return new TimedCache<String, Muser>(30 * 6000);
>     } 
> }
> ```

##### 1.1.1 autowired和new

遇到的问题：autowired注入为null

参考文章：https://www.cnblogs.com/cat-/p/10014477.html

<font color=dark>如果用「new Object()」的方式创建实例后其class中的Bean的注解会失效</font>

> Spring注入的流程：
>
> 首先他会把项目中target -> classes 目录下的「.class」文件进行解析
>
> 通过Spring.xml中的「context:component-scan」进行注解扫描
>
> **如果这个路径下的「.class」文件的类上面是否存在@Component声明的注解**
>
> **如果被此类注解修饰，Spring会把所有被注解修饰的bean进行实例化操作  供给@Autowired进行注入**
>
> (在spring注解的源码中@Service和@Repository等等都继承了@Component注解)

##### 1.1.2 autowired遇到static

遇到的问题：静态方法怎么使用autowired注解

对应的业务场景：有些静态方法需要依赖被容器管理的类

参考文章：https://www.cnblogs.com/chenfeng1122/p/6270217.html

(1) 错误的使用方法

```java
@Component
public class Test {
    @Autowired
    private static UserService userService;
    
    public static void test() {
        userService.test();
    }
}
```

这样一定会报java.lang.NullPointerException: null异常。

(2) 原因分析：

> **静态变量、类变量**不是对象的属性，而是一个类的属性，所以静态方法是属于类（class）的，普通方法才是属于实体对象（也就是New出来的对象）的，spring注入是在容器中实例化对象，所以不能使用静态方法。

> **静态方法在spring是不推荐使用的**。使用静态变量、类变量扩大了静态方法的使用范围，而依赖注入的主要目的，是让容器去产生一个对象的实例，然后在整个生命周期中使用他们，同时也让testing工作更加容易。**一旦你使用静态方法，就不再需要去产生这个类的实例，这会让testing变得更加困难，同时你也不能为一个给定的类，依靠注入方式去产生多个具有不同的依赖环境的实例**，这种static field是隐含共享的，并且是一种global全局状态，spring同样不推荐这样去做。

(3) 解决方案

在类创建而非实体对象创建的时候，就进行注入。

<font color=dark>方法1、将@Autowire加到构造方法上</font>

```java
@Component
public class Test {
    
    private static UserService userService;
    
    @Autowired
    public Test(UserService userService) {
        Test.userService = userService;
    }
    
    public static void test() {
        userService.test();
    }
}
```

<font color=dark>方法2、用@PostConstruct注解</font>

```java
@Component
public class Test {
    
    private static UserService userService;
    
    @Autowired
    private UserService userService2;
    
    @PostConstruct
    public void beforeInit() {
        userService = userService2;
    }
    
    public static void test() {
        userService.test();
    }
}
```

## 三、spring如何通过注解来管理

所有的spring注解，某个类被注解后可以被spring框架所扫描并注入到spring容器来进行管理

<font color=darkred>用这些注解对应用进行分层之后，就能将请求处理（web-Controller）、业务逻辑处理（Service）、数据库操作处理（do和dao）分离出来，为代码解耦，也方便了以后项目的维护和开发。</font>

在Spring2.5版本中，引入了更多的Spring类注解：<font color=darkred>**`@Component`, `@Service`, `@Controller`**</font>。

- `@Component`是一个通用的Spring容器管理的单例bean组件。能够将Bean注册到Spring容器中，交给Spring进行管理。
- 而`@Repository`, `@Service`, `@Controller`就是针对不同的使用场景所采取的特定功能化的注解组件。

### 1、web层 Controller

#### 1.1 前端请求@Controller

<font color=darkred>`@Controller`注解类进行前端请求的处理、转发、重定向。包括调用Service层的方法</font>

作用于表现层（spring-mvc的注解）

#### 1.2 @RestController

返回数据类型为 Json 字符串，特别适合我们给其他系统提供接口时使用。

#### 1.3 @RequestMapping

(1) 不同前缀访问同一个方法,此时访问hello和hi 都可以访问到say（）这个方法

```java
@RequestMapping(value = {"/hello","/hi"},method = RequestMethod.GET)
public String say(){
	return girlProperties.getName();
}
```

(2) 给类一个RequestMapping, 访问时就是：http://localhost:8099/hello/say

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @Resource
    private  GirlProperties girlProperties;
    @RequestMapping(value = "/say",method = RequestMethod.GET)
    public String say(){
        return girlProperties.getName();
    }
}
```

#### 1.4 @PathVariable：获取url中的数据

```java
@RestController
@RequestMapping("/hello")
public class HelloController {
    @Resource
    private  GirlProperties girlProperties;
  
    @RequestMapping(value = "/say/{id}",method = RequestMethod.GET)
    public String say(@PathVariable("id") Integer id){
        return "id :"+id;
    }
}
```

```java
访问http://localhost:8099/hello/say/100, 结果如下
id :100
```

#### 1.5 @RequestParam :获取请求参数的值

(1) 正常请求

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @Resource
    private  GirlProperties girlProperties;
    @RequestMapping(value = "/say",method = RequestMethod.GET)
    public String say(@RequestParam("id") Integer id){
        return "id :"+id;
    }
}
```

访问 http://localhost:8099/hello/say?id=111 结果如下

```
id :111
```

（2）设置参数非必须的，并且设置上默认值

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @Resource
    private  GirlProperties girlProperties;
    @RequestMapping(value = "/say",method = RequestMethod.GET)
    public String say(@RequestParam(value = "id",required = false,defaultValue = "0") Integer id){
        return "id :"+id;
    }
}
```

访问http://localhost:8099/hello/say 结果如下

```java
id :0
```

#### 1.6 @GetMapping ，当然也有对应的Post等请求的简化写法

这里对应的就是下面这句代码

```java
@GetMapping("/say")
//等同于下面代码
@RequestMapping(value = "/say",method = RequestMethod.GET)
```

### 2、web层-程序入口

#### 1.1 主程序入口类@SpringBootApplication

**@SpringBootApplication**注解用来表示一个主程序类，说明这是一个Spring Boot应用。采用这个标注，说明这个类是应用的主配置类，用这个类的main方法来运行SpringApplication的**`run`**方法。

### 3、do和dao层-数据库处理

#### 3.1 @Repository

<font color=darkred>`@Repository`注解可以标记在任何的类上，用来表明该类是用来执行与数据库相关的操作（即dao对象），并支持自动处理数据库操作产生的异常。</font>

作用于持久层（数据库）

### 4、service层-业务逻辑处理

#### 4.1 @Service

<font color=darkred>`@Service`这个注解只是标注该类处于业务逻辑层。</font>

### 5、其他

#### 5.1 @Autowired

<font color=darkred>`@Autowired`可以用来**自动装配**bean，可以写在字段上、方法上、构造函数上</font>，属于spring注解。

具体的可以看上前面[<font color=blue>1.1 基于autowired注解进行对象注入</font>]

##### ❤@Autowired和@Resource的区别

参考文章：https://www.cnblogs.com/think-in-java/p/5474740.html

1. Autowired byType

@Autowired 注解是按照类型（byType）装配依赖对象，**默认情况下它要求依赖对象必须存在**，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以 结合@Qualifier注解一起使用。

2. resource byName

@Resource 默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不制定name也不制定type属性，这时将**默认通过反射机制使用byName自动注入策略**。

#### 5.2 @Component

<font color=darkred>`@Component`是一个通用的Spring容器管理的单例bean组件。最普通的组件，可以被注入到spring容器进行管理。</font>就是跟`<bean>`一样，可以托管到Spring容器进行管理。

因此，当你的一个类被`@Component`所注解，那么就意味着同样可以用`@Repository`, `@Service`, `@Controller`来替代它，同时这些注解会具备有更多的功能，而且功能各异。

`@Service`, `@Controller` , `@Repository` = {`@Component` + 一些特定的功能}。这个就意味着这些注解在部分功能上是一样的。

##### @Component的使用场景

1. 如果你不知道要在项目的业务层采用`@Service`还是`@Component`注解。那么，`@Service`是一个更好的选择。
2. 如果想使用自定义的组件注解，那么只要在你定义的新注解中加上`@Component`即可。

#### ❤5.3 @Configuration

`@Configuration`的使用 https://www.cnblogs.com/duanxz/p/7493276.html