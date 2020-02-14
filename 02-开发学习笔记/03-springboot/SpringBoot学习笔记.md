[TOC]

# 一些资源

官方网站：[https://spring.io](https://spring.io/)

aop和ioc：https://spring.io/projects/spring-framework

参考书：Spring实战

# Spring核心

## 一、Spring之旅

### 1.1 Spring策略——简化Java开发

通过4种关键策略来简化Java的开发：

- 基于POJO的轻量级和最小侵入性编程；
- 通过依赖注入和面向接口实现松耦合；
- 基于切面和管理进行声明式编程；
- 通过切面和模板减少样板式代码。

#### 1.1.1 POJO的最小侵入性编程

避免框架自身的API而弄乱我们的应用代码，意味着某个类在Spring应用和非Spring应用中都可以发挥同样的作用。

#### 1.1.2 依赖注入DI和面向接口

举例依赖注入的一种方式——构造器注入（Constructor injection）

目标：让互相协作的软件组件保持松散耦合。

具体思想：通过传参的方式传入quest，而不是在BraveKnight类中定义一个Quest类的变量。

![image-20190926203152569](/Users/baola/Library/Application Support/typora-user-images/image-20190926203152569.png){:height="50%" width="50%"}

#### 1.1.3 应用切面（AOP）

**目标**：

- 把遍布应用各处的功能分离出来形成可重用的组件。

AOP和OOP的不同：

- AOP面向切面编程：希望能够将通用需求功能从不相关的类当中分离出来，能够使得很多类共享一个行为，一旦发生变化，不必修改很多类，而只需要修改这个行为即可。
- OOP面向对象编程：将需求功能划分为不同的并且相对独立，封装良好的类，并让它们有着属于自己的行为，依靠继承和[多态](https://baike.baidu.com/item/多态)等来定义彼此的关系。

**举个AOP的例子**：

- 不是AOP的：

![](/Users/baola/Library/Application Support/typora-user-images/image-20190926203829364.png){:height="20%" width="50%"}

- AOP的，把Minstrel声明为一个切面：在xml文件中，将Minstrel声明为一个bean，并定义切点。

![image-20190926204006531](/Users/baola/Library/Application Support/typora-user-images/image-20190926204006531.png)

#### 1.1.4 通过模板封装来消除样板式代码

以JDBC（Java数据库连接）为例，样板式代码如下：

![image-20190927105527264](/Users/baola/Library/Application Support/typora-user-images/image-20190927105527264.png)

模板代码：将样板式代码封装到模板中，不需要迎合JDBC API的需求，仅仅关注于获取员工数据的核心逻辑。

![image-20190927110046993](/Users/baola/Library/Application Support/typora-user-images/image-20190927110046993.png)

### 1.2 容器——容纳你的bean

**容器**：负责创建对象，装配他们，配置他们，并管理他们的整个生命周期（从new到finalize()）

两种不同类型的容器：

1. bean工厂：由org.springframework.beans.factory.BeanFactory接口定义，最简单的容器，提供基本的DI支持。
2. 应用上下文：基于BeanFactory构建，提供应用框架级别的服务。



# 慕课网Spring入门篇

## 一、Spring入门

### 1、spring是什么

- 是一个开源框架，为了解决企业应用开发的复杂性而创建的，但现在已经不止应用于企业应用

- **是一个轻量级的控制反转(IOC)和面向切面(AOP)的容器框架**
  - 轻量：从大小与开销两方面而言Spring都是轻量的。
    - 完整的Spring框架可以在一个大小只有1MB多的JAR文件里发布。
    - 并且Spring所需的处理开销也是微不足道的。
    - 此外，Spring是非侵入式的：典型地，Spring应用中的对象不依赖于Spring的特定类。
  - 通过控制反转（IoC）的技术达到松耦合的目的
    - 什么是控制反转：把控制权交出去
  - 提供了面向切面编程（AOP）的丰富支持，允许通过分离应用的业务逻辑和系统级服务进行内聚性的开发
  - 包含并管理应用对象的配置和生命周期，这个意义上来说spring是一种容器，可以管理自己创建的对象
  - 支持将其他简单的组件进行配置、组合成复杂的应用，这个意义上来说spring是一种框架

PS：<font color=dark>所有的对象在spring应用中都叫Bean</font>

### 2、Spring核心组件

![image-20191017204835562](/Users/baola/Library/Application Support/typora-user-images/image-20191017204835562.png)

### 3、spring作用

1. 容器
2. 提供多种技术支持
   - JMS
   - MQ支持
   - UnitTest：单元测试
   - 。。。
3. AOP（事务管理、日志等）
4. 提供很多方便应用的辅助类（JDBC Template等）
5. 对主流应用框架（Hibernate/ibatis等）提供了良好的支持

### 4、spring的适用范围

![image-20191018100358658](/Users/baola/Library/Application Support/typora-user-images/image-20191018100358658.png)

### 5、什么是框架

简单来说，就是制订一套规范或者规则，程序员在该规范下工作。换一种说法，使用别人搭好的舞台来进行表演。

#### 5.1 特点

- 半成品
- 封装了特定的处理流程和控制逻辑
- 成熟的、不断升级改进的团建

#### 5.2 和类库的区别

1、

框架：封装逻辑、高内聚

类库：松散的工具组合

**类库可以组合成框架**

2、

框架：专注于某一领域

类库：更通用



## 二、IOC

### 1、接口和面向接口编程

**接口：**

关于接口的相关知识参考Java学习笔记中的接口。

**面向接口编程：**

结构设计中，每层只向外（上层）提供一组功能接口，各层之间仅依赖接口而非实现类

这里的“接口”是指用于隐藏具体实现和实现多态性的组件，但是接口并不提供具体的实现，具体实现由接口的实现类提供。

### 2、什么是IOC

#### 2.1 IOC：控制反转

是指应用程序本身不负责依赖对象的创建和维护，而是由外部容器负责创建和维护

 ![image-20191018103301796](/Users/baola/Library/Application Support/typora-user-images/image-20191018103301796.png)

**使用过程：**

1、找IOC容器

2、容器返回对象

3、使用对象

*PS：*<font color=dark>在IOC容器中，所有的对象都称为Bean</font>

#### 2.2 DI：依赖注入

**是IOC的一种实现方式。**

**所谓依赖注入，就是由IOC容器在运行期间，动态的将某种依赖关系注入到对象中。**

目标：让互相协作的软件组件保持松散耦合。

##### 构造注入（Constructor injection）

具体思想：通过传参的方式传入quest，而不是在BraveKnight类中定义一个Quest类的变量。

![image-20190926203152569](/Users/baola/Library/Application Support/typora-user-images/image-20190926203152569.png){:height="50%" width="50%"}

![image-20191018113140388](/Users/baola/Library/Application Support/typora-user-images/image-20191018113140388.png)

##### 设值注入

自动的调用set方法使用ref中变量的值对name中的变量进行赋值

![image-20191018113437810](/Users/baola/Library/Application Support/typora-user-images/image-20191018113437810.png)

#### 2.3 扩展理解

> 哪些方面的控制被反转了呢？
>
> 获得依赖对象的**过程**被反转了。过程由自身管理变为了由IOC容器主动注入，也就是依赖注入（DI）。

## 三、单元测试@Test

#### 1 加入依赖包

使用spring的测试框架需要加入以下依赖包：
JUnit 4 （官方下载：http://www.junit.org/）
Spring Test （Spring框架中的test包）
Spring 相关其他依赖包（不再赘述了，就是context等包）

如果使用maven，在基于spring的项目中添加如下依赖：

```xml
<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.9</version>
            <scope>test</scope>
</dependency> 
<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version> 3.2.4.RELEASE  </version>
            <scope>provided</scope>
</dependency> 
```

#### 2 创建测试类

##### 2.1、创建基类

用来完成对Spring配置文件的加载和销毁

```java
package Solin.Test;
 
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
 
@RunWith(SpringJUnit4ClassRunner.class) //使用junit4进行测试
@ContextConfiguration(locations={"classpath:applicationContext.xml"}) //加载配置文件 
//------------如果加入以下代码，所有继承该类的测试类都会遵循该配置，也可以不加，在测试类的方法上
//控制事务，参见下一个实例  
//这个非常关键，如果不加入这个注解配置，事务控制就会完全失效！  
//@Transactional  
//这里的事务关联到配置文件中的事务控制器（transactionManager = "transactionManager"），同时
//指定自动回滚（defaultRollback = true）。这样做操作的数据才不会污染数据库！  
//@TransactionConfiguration(transactionManager = "transactionManager", defaultRollback = true)  
//------------  
public class BaseJunit4Test{
	
}
```

**所有的单元测试类都继承自基类，通过他的getBean方法获取想要得到的对象。**

下面说明一下几个用到的注解：

1. @RunWith：具体执行单元测试的类需要加该注解

   `@RunWith(SpringJUnit4ClassRunner.class) `

   > 是junit提供给其他框架测试环境接口扩展，为了便于使用spring的依赖注入，spring提供了org.springframework.test.context.junit4.SpringJUnit4ClassRunner作为Junit测试环境

2. @ContextConfiguration：导入配置文件

   `@ContextConfiguration({"classpath:applicationContext.xml","classpath:spring/buyer/applicationContext-service.xml"})`

   上面是分模块导入的，如果引入多个模块就导入多个`applicationContext-service.xml`文件，如果是整个文件都导入，则直接写`@ContextConfiguration(locations = "classpath:applicationContext.xml")`

3. @TransactionConfiguration：配置

   `@TransactionConfiguration(transactionManager = "transactionManager", defaultRollback = true)`

   将这里的事务关联到配置文件中的事务控制器`transactionManager`，同时制定自动回滚，这样做操作的数据不会污染数据库。

##### 2.2、创建测试子类

```java
package Solin.Test;
 
import java.util.List;
 
import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.annotation.Rollback;
import org.springframework.transaction.annotation.Transactional;
 
import Solin.Entity.ImageInfo;
import Solin.Service.ImageInfoService;
 
public class ImageInfoTest extends BaseJunit4Test{
	@Autowired //自动注入
	private ImageInfoService imageInfoService;
	
	@Test
	@Transactional    //标明此方法需使用事务  
  @Rollback(false)  //标明使用完此方法后事务不回滚,true时为回滚 
	public void test(){
		System.out.println("测试Spring整合Junit4进行单元测试");
		
		ImageInfo imageInfo = new ImageInfo();
		imageInfo.setParentID(999);
		imageInfo.setImgAddr("地球");
		imageInfoService.saveImageInfo(imageInfo);
		
		List<ImageInfo> list = imageInfoService.getImageInfoList(95);
		for(ImageInfo img : list){
			System.out.println("parentID:"+img.getParentID()+"------imgAddr:"+img.getImgAddr());
		}
	}
}
```

注意单元测试方法要加注解：@Test

#### 3 执行

右键方法名，选择则“Run As”→“JUnit Test”即可

> 也可以执行整个类的所有方法

## 三、Bean

### 1、Bean容器的初始化

具体见“spring核心组件”——“核心组件介绍”

> spring的初始化方式总结起来就是对xml的引入，因为xml文件自身就是包含了bean，本地引入，服务器相对路径引入，以及直接使用xml。

> spring就是利用反射加xml方式进行。

<font color=dark>在IOC容器中，所有的对象都称为Bean</font>

### 2、Bean的配置项

![image-20191018114520650](/Users/baola/Library/Application Support/typora-user-images/image-20191018114520650.png)

理论上来说，class是必须设置的，其他可以不用配置。

可以通过id和class来获取bean，如果通过id获取，就必须配置id。

### 3、Bean的作用域

![image-20191018114855434](/Users/baola/Library/Application Support/typora-user-images/image-20191018114855434.png)

![image-20191018115047762](/Users/baola/Library/Application Support/typora-user-images/image-20191018115047762.png)

- singleton：单例，指一个Bean容器中只存在一份

  ​	注意：这里是说一个Bean容器，所以不同的Bean容器的实例不是一个，@Test的每个方法在执行的时候都会新创建一个容器，因为他初始化的时候会调用一个before方法，启动IOC容器，销毁的时候会调用after方法，关闭IOC容器。

### 4、Bean的生命周期

**生命周期：定义、初始化、使用和销毁**

#### 4.1 定义

Bean的定义由spring帮我们完成了

#### 4.2 初始化

两种方法：

##### 接口

实现org.springframework.beans.factory.InitializingBean接口，覆盖afterPropertiesSet方法

![image-20191018142525366](/Users/baola/Library/Application Support/typora-user-images/image-20191018142525366.png)

##### xml配置

配置init-method属性

![image-20191018142448972](/Users/baola/Library/Application Support/typora-user-images/image-20191018142448972.png)

#### 4.3 销毁

##### 接口

实现org.springframework.beans.factory.DisposableBean接口，覆盖destroy方法

![image-20191018142758638](/Users/baola/Library/Application Support/typora-user-images/image-20191018142758638.png)

##### xml配置

配置destroy-method

![image-20191018142737708](/Users/baola/Library/Application Support/typora-user-images/image-20191018142737708.png)

#### 4.4 全局的初始化和销毁方法

![image-20191018150812254](/Users/baola/Library/Application Support/typora-user-images/image-20191018150812254.png)

#### 4.5 整个过程示例

##### 接口方式示例

0. ……
1. 加载xml，获取bean配置
2. 调用init方法
3. 关掉容器
4. 调用销毁的方法

![image-20191018151231321](/Users/baola/Library/Application Support/typora-user-images/image-20191018151231321.png)

##### 三种方式同时使用

调用顺序：

1. 接口方式
2. 单个bean配置的方式
3. 前两个有一个配置了的话，默认的全局初始化和销毁方式不生效

### 5、Aware

- Spring中提供了一些以Aware结尾的接口，实现了Aware接口的Bean在被初始化后，可以获取相应资源

- 通过Aware接口，可以对Spring相应资源进行操作（慎重）,前提配置<bean>标签，并使用ioc容器去加载
  - ApplicationContextAware：Bean类实现该接口，通过覆盖该接口提供的setApplicationContext方法，可以直接获取spring上下文，而不用我们自己手动创建。
  - BeanNameAware：Bean类实现该接口，通过覆盖该接口提供的setBeanName方法，可以得到bean的name(id)
  - 同时实现两个接口，通过setBeanName可以得到bean的name，通过applicationContext.getBean(this.name)得到此bean的实例。

- 为对Spring进行简单的扩展提供了方便的入口。



> Spring Aware本来就是Spring设计用来框架内部使用的，用Aware是为了让Bean和Spring框架耦合，当然不使用也可以实现，我个人觉得应该是两个原因，一是老师为了教学效果，因为前面用到了Aware，二大概是为了耦合吧，有轮子不用自己造，不也挺好的。

<font color=red>这一章先忽略，以后再学</font>

### 6、Bean的自动装配

> 当Spring装配Bean属性时，有时候非常明确，就是需要将某个Bean的引用装配给指定属性。比如，如果我们的应用上下文中只有一个`org.mybatis.spring.SqlSessionFactoryBean`类型的Bean，那么任意一个依赖`SqlSessionFactoryBean`的其他Bean就是需要这个Bean。毕竟这里只有一个`SqlSessionFactoryBean`的Bean。

四种自动装配策略：

```java
public interface AutowireCapableBeanFactory{

	//无需自动装配
	int AUTOWIRE_NO = 0;

	//按名称自动装配bean属性
	int AUTOWIRE_BY_NAME = 1;

	//按类型自动装配bean属性
	int AUTOWIRE_BY_TYPE = 2;

	//按构造器自动装配
	int AUTOWIRE_CONSTRUCTOR = 3;

	//过时方法，Spring3.0之后不再支持
	@Deprecated
	int AUTOWIRE_AUTODETECT = 4;
}
```

#### 四种方式

##### 1.byName

<font color=dark>根据属性名自动装配：把与Bean的属性具有相同名字的其他Bean自动装配到Bean的对应属性中。</font>

此选项将检查容器，并根据名字查找与属性完全一致的bean，并将其与属性自动装配

**举例：**

首先，在User的Bean中有个属性`Role myRole`，再创建一个Role的Bean，它的名字如果叫myRole，那么在User中就可以使用byName来自动装配。

```java
public class User{
	private Role myRole;
}
public class Role {
	private String id;	
	private String name;
}
```

上面是Bean的定义，再看配置文件。只要属性名称和Bean的名称可以对应，那么在user的Bean中就可以使用byName来自动装配。

```xml
<bean id="myRole" class="com.viewscenes.netsupervisor.entity.Role">
	<property name="id" value="1001"></property>
	<property name="name" value="管理员"></property>
</bean>

<bean id="user" class="com.viewscenes.netsupervisor.entity.User" autowire="byName"></bean>
```

那么，如果属性名称对应不上呢？

##### 2.byType

<font color=dark>使用类型来自动装配：</font>把与Bean的属性具有相同类型的其他Bean自动装配到Bean的对应属性中。

还是上面的例子，如果使用byType，Role Bean的ID都可以省去。

```
<bean class="com.viewscenes.netsupervisor.entity.Role">
	<property name="id" value="1001"></property>
	<property name="name" value="管理员"></property>
</bean>

<bean id="user" class="com.viewscenes.netsupervisor.entity.User" autowire="byType"></bean>
```

**3种情况：**

1. 容器中存在一个与指定属性类型相同的bean，那么将与该属性自动装配
2. 如果存在多个与指定属性类型相同的bean，则抛出异常，并指出不能使用bytype方式
3. 如果没有找到，则什么事都不发生

##### 3.constructor

它是说，把与Bean的构造器入参具有相同类型的其他Bean自动装配到Bean构造器的对应入参中。值的注意的是，**具有相同类型的其他Bean**这句话说明它在查找入参的时候，还是通过Bean的类型来确定。

构造器中入参的类型为Role

```
public class User{
	private Role role;

	public User(Role role) {
		this.role = role;
	}
}

<bean id="user" class="com.viewscenes.netsupervisor.entity.User" autowire="constructor"></bean>
复制代码
```

##### 4.autodetect

它首先会尝试使用constructor进行自动装配，如果失败再尝试使用byType。不过，它在Spring3.0之后已经被标记为`@Deprecated`。

##### 5.No 默认自动装配

默认情况下，default-autowire属性被设置为none，标示所有的Bean都不使用自动装配（<font color=dark>不做任何操作</font>），除非Bean上配置了@autowire属性。

如果你需要为所有的Bean配置相同的autowire属性，有个办法可以简化这一操作。 在根元素Beans上增加属性`default-autowire="byType"`。

```
<beans default-autowire="byType">
```



Spring自动装配的优点不言而喻。但是事实上，在Spring XML配置文件里的自动装配并不推荐使用，其中笔者认为最大的缺点在于不确定性。或者除非你对整个Spring应用中的所有Bean的情况了如指掌，不然随着Bean的增多和关系复杂度的上升，情况可能会很糟糕。

## 四、AOP



# Spring学习

## 一、Spring核心组件

### 1、什么是java组件？

java组件实际上就是类。

<font color=darkred>组件是抽象的概念而已，通俗的说是一些符合某种规范的类组合在一起就构成了组件。他可以提供某些特定的功能。</font>
拿[J2EE](https://www.baidu.com/s?wd=J2EE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)来说，有什么servlet，jsp, javabean，ejb都是组件。但<font color=darkred>实际他们都是类，只不过**有他们特殊的规定**</font>。

**举个例子，拿javabean来说：**

> javabean也就是个类，但你的类想成为javabean你必须，给你的类里的变量 （如xx x）,添两个函数，getXxx()和setXxx()并且类里要有无参的[构造函数](https://www.baidu.com/s?wd=构造函数&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。有了这些就是JAVABEAN了。
> 你要问为什么要有这些规定呢，目前只能说组件之间要想相互使用必须得有一种规范来约束。等你接触多了就更理解了。

### 2、核心组件介绍

#### 2.1 Bean 组件

Bean 组件在 Spring 的 **<font color=dark>`org.springframework.beans`</font>** 包下。

这个包下的所有类主要解决了三件事：<font color=darkred>Bean 的定义、Bean 的创建以及对 Bean 的解析。</font>

对 Spring 的使用者来说唯一需要关心的就是 **Bean 的创建**，其他两个由 Spring 在内部帮你完成了，对你来说是透明的。

##### Bean的创建

Spring Bean 的创建是典型的工厂模式，他的顶级接口是 **<font color=dark>`BeanFactory`</font>**，下图是这个工厂的继承层次关系：

BeanFactory提供了配置结构和基本功能，可以加载和初始化Bean

![image-20191017172035586](/Users/baola/Library/Application Support/typora-user-images/image-20191017172035586.png)





#### 2.2 Context 组件

**<font color=dark>`org.springframework.context`</font>**

**<font color=dark>`ApplicationContext`</font>**保存了Bean对象，并在Spring中被广泛使用

初始化有三种方式：

- 本地文件
- Classpath
- web应用中依赖Servlet或者listener

#### 2.3 Core 组件



# Pandora Boot

## 一、Pandora Boot应用快速开发

**百技课程-Pandora Boot应用快速开发**：https://www.atatech.org/articles/101935?spm=ata.13269325.0.0.713149faY6cP3V

**问题回答：**

1. pandora这个中间件产品的功能是

   A: rpc框架 B: 消息队列 **C: 类隔离容器**

2. 课程中pandora boot应用的入口在哪里

   A: main函数 **B:  web.xml**

3. 集团里pandora boot应用的端口是什么

   **A:** **7001** B: 7002 C: 8080

4. 集团里pandora boot应用的endpoint端口是什么

   A: 7001 **B: 7002** C: 8080

5. 遇到pandora boot相关的问题可以怎样解决（多选）

   **A: 到WIKI查找 B: 到ATA上搜索 C: 加钉钉答疑群提问**

### 1、快速入门

#### 1.1 自动创建应用

pandora boot 提供了配套工具，用户只需要勾选需要使用的中间件，就可以一键创建包含示例代码的，并能在 Aone 上发布上线的应用。

快速创建应用：http://mw.alibaba-inc.com/bootstrap.html



# Spring Boot

## 一、Spring Boot入门

### 1、Spring Boot简介

![image-20190826141817202](/Users/baola/Library/Application Support/typora-user-images/image-20190826141817202.png)

### 2、微服务

### 3、环境准备

### 4、Spring Boot HelloWorld

实现一个功能：浏览器发送Hello请求，服务器接收器牛并处理，响应HelloWorld字符串



#### 1. 创建Maven工程

new，选择maven，选择jdk，输入名称即可。

1.0 选择自动导入：点击Enable工程

就可以自动导入配置了依赖后的相关jar包

#### 2. 导入spring boot相关依赖

首先是继承一个父项目

然后是导入web模块的依赖

#### 3. 编写一个主程序

用处：用于启动Spring boot应用

1. 右击java，选择添加javaclass，可以直接在类的名字前面加上包名，这样就会自动创建一个类，属于这个包。
2. 然后对主程序类添加@SpringBootApplication注解，用于表明这是一个主程序类。
3. 在类里面添加main方法，然后写一句话run，用于启动应用程序。

#### 4. 编写业务逻辑Service或者controller

用@Controller注解表明：这个类是一个处理请求的类。

```java
packagecom.atguigu.controller;
@Controller    // 表明这个类是一个控制器类，可以处理请求
Public classHelloController{
  
    @ResponseBody
    @RequestMapping("/hello")
        publicStringhello(){
        return"HelloWord!";
    }
}
```

#### 5. 运行主程序测试

- 方式1：直接在主程序入口类点击运行（在ide里面运行）

- 方式2：使用maven插件将应用打包成一个jar包，打包的时候可以直接将添加的依赖打包进去。得到jar包之后，就可以直接使用下面的命令来运行程序：

  ```cmd
  java -jar 报名**
  ```

### 5、Hello Word探究

#### 0. 工程的基础结构

 \- src/main/java 程序开发以及主程序(*Application.java)入口

 \- src/main/resources 配置文件

 \- src/test/java 测试程序

#### 1. 简化部署 

- **回顾：**

  ​	不需要做任何配置，只需要写一个主程序入口，然后根据具体的业务编辑对应的Controller和Service

- **分析：**

  ​	父依赖：定义父项目，一般用来做依赖版本管理，通常情况下，父依赖还有自己的父项目。

- **各种starters依赖：**

  ​	Springboot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些Starters，相关应用场景的依赖就会被导入进来。也就是说，starters可以看成依赖的打包。 

#### 2. 主程序类，入口类

```java
package com.alibaba.paimaitest;

import com.alibaba.paimaitest.config.BeanConfig;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.taobao.pandora.boot.PandoraBootstrap;
import org.springframework.boot.web.servlet.ServletComponentScan;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.ImportResource;
import org.springframework.context.annotation.PropertySource;

/**
 * Pandora Boot应用的入口类
 * @author chengxu
 */
@SpringBootApplication(scanBasePackages = {"com.alibaba.paimaitest"})
@ServletComponentScan
@Import({BeanConfig.class})
@ImportResource({"classpath:config/auction-atc-task.xml"})
@PropertySource(value = {"classpath:application.properties"})
public class Application {

    public static void main(String[] args) {
        PandoraBootstrap.run(args);
        SpringApplication.run(Application.class, args);
        PandoraBootstrap.markStartupAndWait();
    }
}
```

##### @SpringBootApplication

**@SpringBootApplication**注解用来表示一个主程序类，说明这是一个Spring Boot应用

采用这个标注，说明这个类是应用的主配置类，应该运行这个类的main方法来启动应用。

**@SpringBootApplication**其实是一个组合注解：

```
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class,
        attribute = "exclude"
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class,
        attribute = "excludeName"
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};
}
```

##### @SpringBootConfiguration：配置类

标注在某个类上，表示这是一个Spring Boot的配置类。

配置类——(可替代)——配置文件；

​      配置类也是容器中的一个组件@Component。

##### @EnableAutoConfiguration：开启自动配置功能

添加了这个标注，Spring Boot就可以帮我们自动配置

AutoConfigurationPackage：

将主配置类（@SpringBootApplication标注的类）的所在包及其下面所有子包里面的所有组件扫描到Spring容器中。

Import：给容器中导入组件。

将所有需要导入的组件以全类名的方式导入到组件中。

#### 3. 自己的业务逻辑

##### 2.1 Controller

Controller文件夹下面放控制器，用来处理应用请求

用到的注解有requestMapping、restController、ResponseBody、Controller等等。

##### 2.2 resource文件夹

存放用到的资源

###### 1️⃣static

![image-20190824134148203](/Users/baola/Library/Application Support/typora-user-images/image-20190824134148203.png)

存放静态资源：包括js css images等

###### 2️⃣templates

存放所有的模板页面

可以使用模板引擎

PS：我们现在先不考虑模板的问题，原因（前后端分离）如下：

> 现在很多开发，都采用了**前后端完全分离**的模式，即后端只提供数据接口，前端通过AJAX请求获取数据，完全不需要用的模板引擎。这种方式的优点在于前后端完全分离，并且随着近几年前端工程化工具和MVC框架的完善，使得这种模式的维护成本相对来说也更加低一点。

###### 3️⃣application.properties

![image-20190824134326524](/Users/baola/Library/Application Support/typora-user-images/image-20190824134326524.png)

Springboot应用的配置文件，可以修改一些默认设置

比如可以配置服务器端口号等



### 6、创建Spring Boot工程的方式

#### 1. 手动创建：Maven工程

#### 2. 使用Spring Initializer快速创建

#### 3. 打包的方式

可以是jar包，也可以是war包。





## 二、Spring Boot的配置文件

### 1 配置文件的名字和分类

**不同配置文件的对比：注意：配置文件的名字是固定的**，这样Springboot就会把他们当做全局配置文件

- application.properties

- application.yml（.yml或者.yaml）文件：

  - 是YAML（YAML Ain't Markup Language）语言的文件

  - **以数据为中心**，比json、xml等更适合做配置文件

  - http://www.yaml.org/   参考语法规范

  - 我们就把他当做一种标记语言来看待：YAML ain't (a/isn't) ：是一种标记语言，也不是一种标记语言

  - 配置例子：比较简洁，以数据为中心

    ```java
    server:
    	port: 8080
    ```

- Pom.xml文件：以前用到的配置文件

  - 配置例子

    ```java
    <server>
    	<port>8080</port>
    </server>
    ```

### 2 YAML的基本语法

#### 2.1 基本语法

```yaml
server:
	port: 8080
```

- 属性和值之间要有冒号+空格，来表示一对键值对（注意冒号和空格都是一定要有的，尤其是空格）
- 以空格的缩进来控制层级关系：只要是左对齐的一列数据，都是同一个层级的
- 属性和值是大小写敏感的

#### 2.2 值的写法

##### 基本类型：普通的值（数字、字符串、布尔）

​		k: v  字面量直接写

​				字符串不需要加上单引号或者双引号

##### 对象、Map（属性和值）

对象还是k: v的形式

```
friends:
		lastName: zhangsan
		age: 20
```

在上面的例子中，friends是一个对象，是key，lastName: zhangsan和age: 20是value，lastName是friends中的一个属性

##### 数组（List、Set）

### 3 properties文件的配置方法

#### 3.1 基本语法

![image-20190824215318532](/Users/baola/Library/Application Support/typora-user-images/image-20190824215318532.png)

直接是k=v的形式

### 4 配置文件注入

- **目的：将配置文件中的每一个属性值映射到这个组件中**
- **怎么实现？**

#### 4.1 yml——@ConfigurationProperties

- 默认需要从**全局配置文件**中获取值
- 使用一个注解@ConfigurationProperties：告诉Springboot将本类中的所有属性和配置文件中的相关配置进行绑定
- 如何使用？

```
@ConfigurationProperties(prefix = "person")
```

其中prefix是前缀，表示这个类需要映射的是以person为开头的配置

只有这个类是容器中一个组件，才可以通过配置文件将配置的值和类中的属性一一对应

- **配置文件处理器**

可以导入一个配置文件处理器，以后编写配置就有提示了。

![image-20190824214256006](/Users/baola/Library/Application Support/typora-user-images/image-20190824214256006.png)

- JSR303数据校验

下图是无效的，因为@value不支持

如果是@ConfigurationProperties的话，就支持

![image-20190824221656179](/Users/baola/Library/Application Support/typora-user-images/image-20190824221656179.png)

#### 4.2 properties——@value

注解：

@value(”$(person.last-name)“)

@value("#(11*2)")

需要每一个属性值都加上@value注解，比@ConfigurationProperties的批量注解形式要麻烦一些

person.last-name是在配置文件中已经定义了的

#### 4.3 @value和@ConfigurationProperties获取值的比较

|                | @ConfigurationProperties                    | @value                      |
| :------------- | ------------------------------------------- | --------------------------- |
| 功能           | 批量注入配置文件中的属性                    | 一个个的置顶                |
| 松散绑定       | 松散语法：last-name和lastName是一样的意思   | 不支持                      |
| SpEL表达式     | 不支持                                      | 支持：比如@value("#(11*2)") |
| JSR303数据校验 | 支持：具体情况见4.1的配置文件注入值数据校验 | 不支持                      |
| 复杂类型封装   | 支持                                        | 不支持                      |

如果只是在某个业务逻辑中需要获取一下配置文件中的某个值，就用@value；

如果专门编写了javaBean，来和配置文件进行映射，就用@ConfigurationProperties，一次性注入。

#### 4.4 @PropertySource

- 把所有需要配置的东西写在全局配置文件中并不太好，所以单独创建一个person.properties的配置文件，这样的话
- Properties注解没办法读取到该配置文件的内容，所以……

![image-20190824223021612](/Users/baola/Library/Application Support/typora-user-images/image-20190824223021612.png)

- 所以我们用一个注解@PropertySource，如下图所示。从而告诉Spring Boot我们要加载类路径下的person.properties文件

![image-20190824223300770](/Users/baola/Library/Application Support/typora-user-images/image-20190824223300770.png)

#### 4.5 @ImportResource

- **定义：**

参数：类路径的resources目录下的xml文件，的相对路径

```java
@SpringBootApplication(scanBasePackages = {"com.alibaba.paimaitest"})
@ServletComponentScan
@Import({BeanConfig.class})
@ImportResource({"classpath:config/auction-atc-task.xml"})
@PropertySource(value = {"classpath:application.properties"})
public class Application {

    public static void main(String[] args) {
        PandoraBootstrap.run(args);
        SpringApplication.run(Application.class, args);
        PandoraBootstrap.markStartupAndWait();
    }
}
```

- **作用：**

导入Spring的配置文件，让其生效。可以在ioc容器中导入配置文件中定义的组件

​		（组件？ 可以是一个javabean）

​		（关于什么是ioc？ https://www.cnblogs.com/chenssy/p/9576769.html）

如下图，是一个配置文件示例：

![image-20190824224156588](/Users/baola/Library/Application Support/typora-user-images/image-20190824224156588.png)

- **上面描述的方式是Spring的方式，在配置文件中采用bean标签定义组件**

- **SpringBoot推荐采用全注解的方式来定义组件**

  ​		也就是定义配置类，通过**@Configuration注解定义配置类**

  ​		然后通过**@Bean注解给这个类添加组件**。组件的id默认就是方法名，将方法的返回值添加到容器中

  ​		具体情况如下图所示：

![image-20190824225001600](/Users/baola/Library/Application Support/typora-user-images/image-20190824225001600.png)

### 4 配置文件怎么支持中文？

配置文件中讲中文先转换为ascii码，从而支持中文的显示

![image-20190824141739587](/Users/baola/Library/Application Support/typora-user-images/image-20190824141739587.png)

这样，在配置文件中显示的是中文，但在转换的时候会按照ascii码来转换

### 5 配置文件的路径

配置文件放在**<font color=red>src/main/resources</font>**或者**<font color=red>类路径/congfig</font>**下面

### 6 配置文件的作用

- 一般来说SpringBoot在底层会默认配置好，所以我们不需要任何配置就可以启动应用程序
- 那如果要修改的话，可以通过配置文件修改SpringBoot自动配置的默认值

### 7 配置文件的加载顺序

spring boot启动的时候会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

- file：./config/
- File：./
- Classpath：/config/
- Classpath：/

以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，而且高优先级配置的内容会覆盖低优先级的配置内容。

我们也可以通过配置spring.config.location来改变默认配置。

## 三、Maven的pom.xml

参考资料：

https://www.cnblogs.com/zjdxr-up/p/8417733.html  

https://blog.csdn.net/zmh458/article/details/79603873

### 1 pom文件的位置

一般一个应用中有多个pom文件，最外层的是主pom文件。

### 2 主pom文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

    <parent>
        <groupId>com.taobao</groupId>
        <artifactId>parent</artifactId>
        <version>2.0.0</version>
    </parent>

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.alibaba.baola</groupId>
    <artifactId>baola-test</artifactId>
    <packaging>pom</packaging>
    <version>1.0.0-SNAPSHOT</version>
    <name>baola-test</name>

    <properties>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.source>1.8</maven.compiler.source>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <mockito-all.version>1.10.19</mockito-all.version>
        <mybatis-starter.version>2.0.1</mybatis-starter.version>
        <maven-antrun.version>1.8</maven-antrun.version>
        <spring-boot.version>2.0.9.RELEASE</spring-boot.version>
        <pandora-boot.version>2019-09-stable</pandora-boot.version>
        <pandora-boot-maven-plugin.version>2.1.11.8</pandora-boot-maven-plugin.version>
        <velocity.starter.version>1.0.4.RELEASE</velocity.starter.version>
        <alibaba-security-dependency.version>2.0.0-SNAPSHOT</alibaba-security-dependency.version>
    </properties>

    <modules>
        <module>baola-test-service</module>
        <module>baola-test-start</module>
    </modules>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.taobao.pandora</groupId>
                <artifactId>pandora-boot-starter-bom</artifactId>
                <version>${pandora-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis-starter.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba.baola</groupId>
                <artifactId>baola-test-service</artifactId>
                <version>${project.version}</version>
            </dependency>
            <!-- security -->
            <dependency>
                <groupId>com.alibaba.security</groupId>
                <artifactId>security-spring-dependencies</artifactId>
                <version>${alibaba-security-dependency.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.boot</groupId>
                <artifactId>velocity-spring-boot-starter</artifactId>
                <version>${velocity.starter.version}</version>
            </dependency>
            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>servlet-api</artifactId>
              <version>99.0-does-not-exist</version>
            </dependency>

            <dependency>
              <groupId>servlet-api</groupId>
              <artifactId>servlet-api</artifactId>
              <version>999-no-exist-SNAPSHOT</version>
            </dependency>

            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>javax.servlet-api</artifactId>
              <version>999-not-exist-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>org.mockito</groupId>
                <artifactId>mockito-all</artifactId>
                <version>${mockito-all.version}</version>
                <scope>test</scope>
            </dependency>
            
            <dependency>
                <artifactId>spring-jcl</artifactId>
                <groupId>org.springframework</groupId>
                <version>999-not-exist-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>jcl-over-slf4j</artifactId>
                <version>1.7.26</version>
            </dependency>
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
                <version>999-not-exist-v3</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <artifactId>maven-antrun-plugin</artifactId>
                    <version>${maven-antrun.version}</version>
                </plugin>
                <plugin>
                    <groupId>com.taobao.pandora</groupId>
                    <artifactId>pandora-boot-maven-plugin</artifactId>
                    <version>${pandora-boot-maven-plugin.version}</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

最外层是<project> </project>

内部依次是：

#### 1. 父依赖

定义父项目，一般用来做依赖版本管理，通常情况下，父依赖还有自己的父项目。

#### 2. Maven项目坐标元素

- maven的世界中拥有数量非常巨大的构件，也就是平时用的一些jar,war等文件。 

- maven定义了这样一组规则： 

  ​		世界上任何一个构件都可以使用Maven坐标唯一标志，maven坐标的元素包括groupId, artifactId, version，package，classifier。 

  ​		只要在pom.xml文件中配置好dependancy的groupId，artifact，verison，classifier, maven就会从仓库中寻找相应的构件供我们使用。

- 那么，"maven是从哪里下载构件的呢？" 

  ​		答案很简单，maven内置了一个中央仓库的地址（http://repol.maven.org/maven2）,该中央仓库包含了世界上大部分流行的开源项目构件，maven会在需要的时候去那里下载。 

**具体坐标介绍**

- GroupID 

  ​		：是项目组织唯一的标识符，实际对应JAVA的包的结构，是main目录里java的目录结构。

  ​		举例：定义了项目属于哪个组，举个例子，如果你的公司是mycom，有一个项目为myapp，那么groupId就应该是com.mycom.myapp. 

- ArtifactID 

  ​		：是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称。 

  ​		举例：定义了当前maven项目在组中唯一的ID,比如，myapp-util,myapp-domain,myapp-web等。

- version 

  ​		：指定了myapp项目的当前版本，SNAPSHOT意为快照，说明该项目还处于开发中，是不稳定的版本。 

- name 

  ​		：声明了一个对于用户更为友好的项目名称，不是必须的，推荐为每个pom声明name，以方便信息交流

- packaging【可选的，默认为jar】： 

  ​		：当不定义packaging时，maven会使用默认值jar。 

- classifier

  ​		：该元素用来帮助定义构件输出的一些附属构件。 

项目构件的文件名是坐标相对应的，一般的规则为：artifact-version.packing

#### 3. properties版本

主要用来定义版本号

#### 4. modules

##### maven module和maven project

> Maven Module也是一个maven 工程，但是却是一个子工程，必须有父工程存在并依赖，Maven Module不能抛弃父工程单独存在。

> Maven Project可以理解为一个单独、独立的工程，在打包为jar或者war时，可以单独运行。如果在pom文件中添加了对父工程的依赖，此时作为父工程的子工程。

> 另外一点区别是，如果是Maven Module，那么在父工程的POM文件中肯定有module节点

#### 5. 依赖

结构如下所示：

PS：**properties中的版本号可以用在这里**

```xml
 		<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

##### 排除依赖

当出现冲突的时候，就会需要排除依赖的情况：

1. 包名字冲突
2. 包版本号冲突

*举例：*不想使用project-B中版本的project-C依赖包，而将其环卫1.1.0的project-C包。

```xml
<dependencies>
    <dependency>
        <groupId>com.juv</groupId>
        <artifactId>project-B</artifactId>
        <version>1.0.0</version>
        <exclusions>
            <exclusion>//可以有多个
                <groupId>com.juv</groupId>
                <artifactId>project-C</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>com.juv</groupId>
        <artifactId>project-B</artifactId>
        <version>1.1.0</version>
    </dependency>
</dependencies>
```

#### 6. 插件

结构如下所示：

PS：**properties中的版本号也可以用在这里**

```xml
    <build>
        <pluginManagement>
        		<!-- 这个plugin插件，可以将应用打包成一个可执行的jar包 -->
            <plugins>
                <plugin>
                    <artifactId>maven-antrun-plugin</artifactId>
                    <version>${maven-antrun.version}</version>
                </plugin>
                <plugin>
                    <groupId>com.taobao.pandora</groupId>
                    <artifactId>pandora-boot-maven-plugin</artifactId>
                    <version>${pandora-boot-maven-plugin.version}</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
```

### 3 module的pom文件

和主pom文件相比，缺少了build、modules和properties

需要注意的点：

1. 模块的pom文件的dependency结构直接下面这样，没有dependencyManagement

```xml
  <dependencies>
    <dependency>
    </dependency>
  </dependencies>
```

2. dependency可以没有版本号，直接继承父pom文件的依赖的版本号
3. 一般只有本模块使用的依赖，加到模块的pom文件中即可

#### 多模块maven项目中的版本号

对于上面第2点补充说明：

我们可以在父项目的pom文件中的`dependencyManagement`中进行声明依赖，子模块直接使用，不需要指定版本号。

![image-20191015155801270](/Users/baola/Library/Application Support/typora-user-images/image-20191015155801270.png)

子项目引用时，直接进行如下引用即可：

![image-20191015155813100](/Users/baola/Library/Application Support/typora-user-images/image-20191015155813100.png)

## 四、各种注解的作用

### 1、java注解

#### 0.什么是注解

在Java中，**注解和注释，我们可以理解为同一个概念。**说起注释，我们需要先知道什么是元metadata，这个单词我们国内一般翻译为元数据，所谓元数据，就是数据的数据。也就是说，元数据存在的意义，就是为了描述数据。有了元数据的概念以后，我们就可以把**注释称作Java代码的元数据**。

#### 1.@override

这个注释的作用，是用来标识当前的方法，是否覆盖了它的父类中的方法。

**为什么要用Override注释呢？**

​		Override注释与Java语法有异曲同工之妙，它更多的起到的作用是规范Java代码的编写，方便程序员使用，减少产生BUG的机会。关于这一点，我们可以通过下面2种方式来证明。

​		1. 源代码经过编译后，用反编译工具JD打开后，所有Override注释的部分都没有了；

​		2. 从JVM的角度来看，在Class文件的结构中，没有注释内容的定义。

**官方定义：**

```java
/**
 * Indicates that a method declaration is intended to override a
 * method declaration in a superclass.  If a method is annotated with
 * this annotation type but does not override a superclass method,
 * compilers are required to generate an error message.
 *
 * @author  Joshua Bloch
 * @since 1.5
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

#### 2.@Deprecated

Deprecated注释是一个**标记注释。**

**标记注释：**

​		所谓标记注释，就是已经过期或者即将过期的方法，不推荐我们使用。这个方法不推荐使用的原因，可能是存在安全问题，或者是有了更好的方法来替代他。

​		源代码中加入这个标记后，并不影响程序的编译，但加上这个标记后，编译器会显示一些警告信息。

**例如：**

​		当我们使用myeclipse写代码时，使用提示功能：
​		`String str = "yangcq";`
​		当我们输入`str.`以后，myeclipse会提示String类的所有可用方法，这时，我们找到`getBytes`这个方法，发现有些不同，方法前面多了一个斜线。这个就是过期的方法，说明`getBytes`方法使用了`@Deprecated`注释。

![image-20191017174911226](/Users/baola/Library/Application Support/typora-user-images/image-20191017174911226.png)

### 2、Spring注解

- 

#### 1.@SpringBootApplication

**@SpringBootApplication**注解用来表示一个主程序类，说明这是一个Spring Boot应用。采用这个标注，说明这个类是应用的主配置类，用这个类的main方法来运行SpringApplication的**`run`**方法。

#### 2.@Component

<font color=darkred>`@Component`是一个通用的Spring容器管理的单例bean组件。最普通的组件，可以被注入到spring容器进行管理。</font>就是跟`<bean>`一样，可以托管到Spring容器进行管理。

因此，当你的一个类被`@Component`所注解，那么就意味着同样可以用`@Repository`, `@Service`, `@Controller`来替代它，同时这些注解会具备有更多的功能，而且功能各异。

`@Service`, `@Controller` , `@Repository` = {`@Component` + 一些特定的功能}。这个就意味着这些注解在部分功能上是一样的。

##### 2.1使用场景

1. 如果你不知道要在项目的业务层采用`@Service`还是`@Component`注解。那么，`@Service`是一个更好的选择。

2. 如果想使用自定义的组件注解，那么只要在你定义的新注解中加上`@Component`即可。

#### 3.@Repository

<font color=darkred>`@Repository`注解可以标记在任何的类上，用来表明该类是用来执行与数据库相关的操作（即dao对象），并支持自动处理数据库操作产生的异常。</font>

作用于持久层（数据库）

#### 4.@Service

<font color=darkred>`@Service`这个注解只是标注该类处于业务逻辑层。</font>

作用于业务逻辑层

#### 5.@Controller

<font color=darkred>`@Controller`注解类进行前端请求的处理、转发、重定向。包括调用Service层的方法</font>

作用于表现层（spring-mvc的注解）

#### 6.@Autowired

<font color=darkred>`@Autowired`可以用来**自动装配**bean，可以写在字段上、方法上、构造函数上</font>，属于spring注解

##### 6.1 什么是自动装配？

##### 6.2 属性中的@Autowired

#### @Autowired和@Resource

https://www.cnblogs.com/think-in-java/p/5474740.html

#### 7.@Configuration

`@Configuration`的使用 https://www.cnblogs.com/duanxz/p/7493276.html



### 8. controller

#### @RestController

返回数据类型为 Json 字符串，特别适合我们给其他系统提供接口时使用。

#### @RequestMapping

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

#### @PathVariable：获取url中的数据

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

#### @RequestParam :获取请求参数的值

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

#### @GetMapping ，当然也有对应的Post等请求的简化写法

这里对应的就是下面这句代码

```java
@GetMapping("/say")
//等同于下面代码
@RequestMapping(value = "/say",method = RequestMethod.GET)
```

# Spring-Web开发

## 一、简介和回顾

**使用SpringBoot：**

1）创建SpringBoot应用，选中我们需要的模块，比如web、db等等。

2）选中了这些模块之后，SpringBoot就会在配置文件中自动导入对应的依赖。全部在autoConfiguration里面。

3）我们只需要在配置文件中指定少量的配置就可以运行起来。

4）自己编写业务代码。



**自动配置原理？**

- 这个场景SpringBoot帮我们配置了什么？
- 能不能修改？
- 能修改哪些配置？
- 能不能拓展？
- ……

```
xxxxAutoConfiguration：帮我们给容器中自动配置组件
xxxxProperties：配置类来封装配置文件中的内容
```



## 二、Spring Boot对静态资源的映射规则

### 1 引入jquery-webjars

ResourcesProperties



这是jQuery-webjars提供的各种静态资源，不是自己的。

![image-20190826003539106](/Users/baola/Library/Application Support/typora-user-images/image-20190826003539106.png)

### 2 使用自己的静态资源的映射规则

静态资源文件夹：

![image-20190826004232263](/Users/baola/Library/Application Support/typora-user-images/image-20190826004232263.png)

### 3 模板引擎

#### 什么是模板引擎？

模板引擎可以让（网站）程序实现[界面](https://www.baidu.com/s?wd=界面&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)与数据分离，这就大大提升了开发效率，良好的设计也使得代码重用变得更加容易。

我们司空见惯的模板安装卸载等概念，基本上都和模板引擎有着千丝万缕的联系。模板引擎可以实现的功能：

- 代码分离（[业务逻辑](https://www.baidu.com/s?wd=业务逻辑&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)代码和用户[界面](https://www.baidu.com/s?wd=界面&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)代码）
- 数据分离（动态数据与静态数据）
- 代码单元共享（代码重用）
- 多语言、动态页面与[静态页面](https://www.baidu.com/s?wd=静态页面&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)自动均衡（SDE）等等与用户[界面](https://www.baidu.com/s?wd=界面&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)可能没有关系的功能。

**结构如图所示**：

![image-20190826005423506](/Users/baola/Library/Application Support/typora-user-images/image-20190826005423506.png)

- Template：里面的值比如user是动态的

  ​		那么数据从哪里来呢？

- Data：组装了一些数据

- TemplateEngine：将data和模板交给模板引擎TemplateEngine，模板引擎负责将数据写到对应的模板中，然后输出。

#### 不同模板引擎

SpringBoot模板引擎整合详解：https://blog.csdn.net/qq_30764991/article/details/79894603

SpringBoot框架下面有很多好用的模板引擎，比如JSP和Thymeleaf等等

推荐使用thymeleaf：语法更简单



### 4 SpringBoot对SpringMVC做了哪些自动配置

SpringBoot官方参考文档：https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/

可以看到29.1讲的是关于SpringMVC的内容

**具体如下：**

Spring Boot 自动配置好了SpringMVC.

以下是SpringBoot对SpringMVC的默认配置:

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
  - 自动配置了ViewResovler：视图解析器，根据方法的返回值得到视图对象，视图对象决定如何渲染（转发？重定向？）
  - ContentNegotiatingViewResolver：组合所有的视图解析器
- Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-static-content))).
- Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.
- Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-message-converters)).
- Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-message-codes)).
- Static `index.html` support.
- Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-favicon)).
- Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-web-binding-initializer)).

If you want to keep Spring Boot MVC features and you want to add additional [MVC configuration](https://docs.spring.io/spring/docs/5.1.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, you can declare a `WebMvcRegistrationsAdapter` instance to provide such components.

If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`.



### 5 如何修改SpringBoot的自动配置

模式：

1）![image-20190826082109403](/Users/baola/Library/Application Support/typora-user-images/image-20190826082109403.png)

2）