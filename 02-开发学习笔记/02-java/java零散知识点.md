### 使用new Date()和System.currentTimeMillis()获取当前时间戳的区别

**<font color = gree>建议使用System.currentTimeMillis()而不是new Date().getTime()，原因：性能更好</font>**

在开发过程中，通常很多人都习惯使用new Date()来获取当前时间，使用起来也比较方便，同时还可以获取与当前时间有关的各方面信息，例如获取小时，分钟等等，而且还可以格式化输出，包含的信息是比较丰富的。但是有些时候或许你并不需要获取那么多信息，你只需要关心它返回的毫秒数就行了，例如getTime()。为了获取这个时间戳，很多人也喜欢使用new Date().getTime()去获取，咋一看没什么问题，但其实没这个必要。其实看一下Java的源码就知道了：

```java
public Date()
{
    this(System.currentTimeMillis());
}
```

已经很明显了，new Date()所做的事情其实就是调用了System.currentTimeMillis()。如果仅仅是需要或者毫秒数，那么完全可以使用System.currentTimeMillis()去代替new Date()，效率上会高一点。况且很多人喜欢在同一个方法里面多次使用new Date()，通常性能就是这样一点一点地消耗掉，这里其实可以声明一个引用。



### ArrayList删除元素时报异常：java.util.ConcurrentModificationException

参考文章：https://www.cnblogs.com/loong-hon/p/10256686.html



### Java随机数生成

##### 第一种：new Random()

​		第一种需要借助java.util.Random类来产生一个随机数发生器，也是最常用的一种，构造函数有两个，Random()和Random(long seed)。第一个就是以当前时间为默认种子，第二个是以指定的种子值进行。产生之后，借助不同的语句产生不同类型的数。

　　种子就是产生随机数的第一次使用值,机制是通过一个函数,将这个种子的值转化为随机数空间中的某一个点上,并且产生的随机数均匀的散布在空间中。以后产生的随机数都与前一个随机数有关。以代码为例。

##### 第二种：Math.random()

​		而第二种方法返回的数值是[0.0,1.0）的double型数值，由于double类数的精度很高，可以在一定程度下看做随机数，借助（int）来进行类型转换就可以得到整数随机数了，代码如下。



### list.add所有元素都变成最后一个加入的元素

**<font color = gree>因为list添加的是对象的引用，Test只被new了一次然后不断被赋值</font>**

关于java的栈、堆、方法区：

![image-20200213155348138](/Users/baola/Library/Application%20Support/typora-user-images/image-20200213155348138.png)


输出

[TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9], TestBean [num=9]]

![image-20200213155418979](/Users/baola/Library/Application%20Support/typora-user-images/image-20200213155418979.png)

输出

[TestBean [num=0], TestBean [num=1], TestBean [num=2], TestBean [num=3], TestBean [num=4], TestBean [num=5], TestBean [num=6], TestBean [num=7], TestBean [num=8], TestBean [num=9]]

![image-20200213155443096](/Users/baola/Library/Application%20Support/typora-user-images/image-20200213155443096.png)