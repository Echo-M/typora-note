# （一）工程规范

[笑总总结文档](https://yuque.antfin-inc.com/paimaitest/atp_dev/iu04g8)

具体如下：

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2019/png/12552/1563184263528-3fded4fe-6add-412b-adf4-5e96c8a36b4a.png?x-oss-process=image/resize,w_1492)

## 工程结构

工程结构如下图所示：

主要分为四个部分：

1、**ptc-web**：web层只有一个Controller了，先功能再业务，Controller下分各业务的package；

2、**ptc-service：**业务相关的先业务再功能，ui，性能，监控，日志 各业务还是各自独立的package，每个业务package下自己组织domain，dao，module，manager，service等，业务package内的分层还是需要按照规范有意识隔离，为了以后维护扩展，但是不是自己业务特有的放到业务package外。整个工程通用的class抽出来放到ptc-common；

3、**ptc-common：**放置工程通用的类，工具，中间件等；

4、**ptc-client：**对外开放的接口，比如hsf服务，遵循最小范围原则，不是需要开放的别放这里暴露出去；

![image-20190826143028473](/Users/baola/Library/Application Support/typora-user-images/image-20190826143028473.png)

## ptc-web

- 存放各种Controller包，对访问控制进行转发
- 对各类参数基本校验
- 不复用的业务简单处理
- web层模块比较薄，只有一个Controller了，先功能再业务，Controller下分各业务的package。

![image-20190827132040528](/Users/baola/Library/Application Support/typora-user-images/image-20190827132040528.png)

- acl（访问控制列表）

  ​		访问控制列表（ACL）是一种基于[包过滤](https://baike.baidu.com/item/包过滤/2724082)的[访问控制](https://baike.baidu.com/item/访问控制/8545517)技术，它可以根据设定的条件对接口上的数据包进行过滤，允许其通过或丢弃。

  ​		访问控制列表被广泛地应用于路由器和三层交换机。借助于访问控制列表，可以有效地控制用户对网络的访问，从而最大程度地保障网络安全。

- Application

  ​		配置类（@SpringBootApplication），可以通过该类进行启动

- buc

  - BucController：获取用户ID

- Controller

  - measurement：精准测试（日志链路）

  - performance：性能

    > - @RequestMapping   和  @GetMapping @PostMapping 区别
    > - @GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)的缩写。 
    > - @PostMapping是一个组合注解，是@RequestMapping(method = RequestMethod.POST)的缩写。

- filter：过滤器

  主要是在客户端请求(HttpServletRequest)进行预处理，以及对服务器响应(HttpServletResponse)进行后处理，在调用Servlet的service方法之前调用，之后可以选择拦截这个请求或继续传递下去。

  参考：[拦截器和过滤器](https://juejin.im/post/5ba3bdae6fb9a05cfc54d16d)

  ```java
  @Component
  public class MyFilter implements Filter {
      private Logger logger = LoggerFactory.getLogger(MyFilter.class);
      @Override
      public void init(FilterConfig filterConfig) throws ServletException {
          logger.info("filter init");
      }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
          logger.info("doFilter");
          //对request,response进行预处理
          //TODO 进行业务逻辑
          filterChain.doFilter(servletRequest, servletResponse);
      }
  
      @Override
      public void destroy() {
          logger.info("filter destroy");
      }
  }
  ```

  - AddResponseHeaderFilter：添加返回包头

- security：安全校验和过滤

  - http安全域名校验，参考 http://gitlab.alibaba-inc.com/middleware-container/pandora-boot/wikis/spring-boot-security-http
  - JSONP，详见 http://gitlab.alibaba-inc.com/middleware-container/pandora-boot/wikis/spring-boot-security-jsonp
  - 使用XSS过滤，详见 http://gitlab.alibaba-inc.com/middleware-container/pandora-boot/wikis/spring-boot-security-xss

- velocity：是一个基于Java的模板引擎   [velocity入门](https://blog.csdn.net/u014282557/article/details/76193014)

  - DateTool：返回当前时间

## ptc-service

（在设计实现时需要考虑下manager层和service层）

> manager：业务处理层
>
> service：相对具体的业务逻辑服务

**主程序**

- acl：访问控制列表

  ​	acl参考文档 http://gitlab.alibaba-inc.com/middleware-container/pandora-boot/wikis/spring-boot-acl

- common：通用

  - atc（拍卖测试中心，网址atc.alibaba.net）

    - impl：存放接口实现类

      > java impl 是一个资源包，用来存放java文件的。
      >
      > 在Java开发中，通常将后台分成几层，常见的是三层mvc：model、view、controller，模型视图控制层三层，而impl通常处于controller层的service下，用来存放接口的实现类，**impl的全称为implement**，表示实现的意思。

      ​	 - DefaultTimeoutEventServiceWrapper超时中心基础服务接口实现类

  - httpClient

    - httpXmlClient：封装了http的post和get请求

      > Package org.apache.http介绍文档 https://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/org/apache/http/package-summary.html

    - SSLClient：用于进行https请求的httpClient

  - user

    - EnhancedUserService：根据工号查询员工钉钉的其他信息

  - util

    - ApiSessionUtil：获取用户相关信息（包括nick、id等等）

      > session：存储特定用户会话所需的属性和配置信息

    - DateUtil

    - HttpClientUtil

    - JsonpResponseUtils

    - UrlUtil：根据Url和参数名称获取参数

- config：存放配置类

  - 

- measurement（精准测试）

- monitor（业务监控）

- performance（性能测试）

- uiautomation（UI自动化测试）

  - common
  - mapper
  - module
    - 存放了一些DO类（DO的意思是数据对象类），
  - service
    - UI自动化业务提供的具体服务类，形式：接口实现类，用于用例管理、用例结果管理

**resource配置文件**

![image-20190902130646579](/Users/baola/Library/Application Support/typora-user-images/image-20190902130646579.png)



