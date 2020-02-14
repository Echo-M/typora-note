-  

- 一个功能

- 浏览器发送Hello轻轻，服务器接收器牛并处理，响应Hello World字符串

-  

-  

- 1、创建Maven工程

- new，选择maven，选择jdk，输入名称即可。

- ##### 1.0 选择自动导入：点击Enable工程

- 就可以自动导入配置了依赖后的相关jar包

-  

- 2、导入spring boot相关依赖

- 首先是继承一个父项目

- 然后是导入web模块的依赖

- ![image-20200119161418299](/Users/baola/Library/Application Support/typora-user-images/image-20200119161418299.png)

-  

- 3、编写一个主程序

- 用处：用于启动Spring boot应用

- 右击java，选择添加java class，可以直接在类的名字前面加上包名，这样就会自动创建一个类，属于这个包。

- 然后对主程序类添加@SpringBootApplication注解，用于表明这是一个主程序类。

- ![image-20200119161500483](/Users/baola/Library/Application Support/typora-user-images/image-20200119161500483.png)

- 在类里面添加main方法，然后写一句话run，用于启动应用程序。

- 

- 4、编写业务逻辑Service或者controller

- 用@Controller注解表明：这个类是一个处理请求的类。

- ```java
  packagecom.atguigu.controller;
  
  @Controller    // 表明这个类是一个控制器类，可以处理请求
  Public class HelloController{
    @ResponseBody
    @RequestMapping("/hello")
    publicStringhello(){
      return"HelloWord!";
    }
  }
  ```

- 

- 5、运行主程序测试

- 方式1：直接在主程序入口类点击运行

- 方式2：使用maven插件将应用打包成一个jar包，这么直接使用java -jar **报名**  

- 打包的时候就可以直接将添加的依赖打包进去

-  

- 6、简化部署 

- 回顾：

- 不需要做任何配置，只需要写一个主程序入口，然后根据具体的业务编辑对应的Controller和Service

- 分析：

- 父依赖：

- 定义父项目，一般用来做依赖版本管理，通常情况下，父依赖还有自己的父项目。

- 各种starters依赖

- Springboot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些Starters，相关应用场景的依赖就会被导入进来。也就是说，starters可以看成依赖的打包。



springboot是用来简化spring框架。

![image-20200119161804370](/Users/baola/Library/Application Support/typora-user-images/image-20200119161804370.png)

优点：

![image-20200119161826837](/Users/baola/Library/Application Support/typora-user-images/image-20200119161826837.png)

