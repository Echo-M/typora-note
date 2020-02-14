# JavaScript学习

## JS小知识

### 将JSON对象转为字符串

JSON.stringify() 方法用于将一个json值转为字符串；

JSON.parse() 方法用于将json字符串转化成对象；

当我们用JSON.stringify()方法将json值转为字符串时，你会发现所有字符串都连在一块，根本看不懂。那么就有下面的解决方法了：

    JSON.stringify(json,null,"\t");  //缩进一个tab
    
    JSON.stringify(json,null,5);     //缩进5个空格

有时候你会发现，如果打印这些字符串，他们还是连在一块的，这是因为html忽略了你的空格或者tab，那么就用<pre></pre>标签吧，它可以定义预格式化的文本。被包围在pre元素中的文本通常会保留空格和换行符。

### JsonP和jsoncallback使用

#### 1、跨域时为什么要用jsonp

一般jquery跨域用到的两个方法为：`$.ajax` 和 `$.getJSON`

JSON数据是一种能很方便通过JavaScript解析的结构化数据。如果获取的数据文件存放在远程服务器上（域名不同，也就是跨域获取数据），则需要使用jsonp类型。使用这种类型的话，会创建一个查询字符串参数 callback=? ，这个参数会加在请求的URL后面。服务器端应当在JSON数据前加上回调函数名，以便完成一个有效的JSONP请求。如果要指定回调函数的参数名来取代默认的callback，可以通过设置`$.ajax()`的jsonp参数。

其实jquery跨域的原理是通过外链 <script>  来实现的,然后在通过回调函数加上回调函数的参数来实现真正的跨域。 

Jquery 在每次跨域发送请求时都会有callback这个参数，其实这个参数的值就是回调函数名称，所以，服务器端在发送json数据时，应该把这个参数放到前面，这个参数的值往往是随机生成的，如：jsonp1294734708682，同时也可以通过 $.ajax 方法设置 jsonpcallback 方法的名称。明白了原理后，服务器端应该这样发送数据：

```java
string message = "jsonp1294734708682({\"userid\":0,\"username\":\"null\"})";
```

#### 2、怎么返回jsonp格式的数据

所以服务端在返回数据的时候，应该返回jsonp格式的数据，怎么封装呢？

```java
// 返回数据调用getJsonP函数进行封装，请求参数加上callback
public String getMethodList(@RequestParam(value = "callback", required = false) String callback) 
{
    return JsonpResponseUtils.getJsonP(JSONObject.toJSONString(ImageAlgoMethodEnum.getMethodList()), callback);
}

// getJsonP函数的实现
public class JsonpResponseUtils {

    /**
     * 转化为JsonP的格式
     * @param jsonStr
     * @param callback
     * @return
     */
    public static String getJsonP(String jsonStr, String callback) {
        return  callback + "(" +  jsonStr + ")";
    }
}
```

#### 3、callback是什么

CALLBACK，即回调函数，是一个通过[函数指针](https://baike.baidu.com/item/函数指针/2674905)调用的函数。如果你把函数的[指针](https://baike.baidu.com/item/指针/2878304)（地址）作为[参数传递](https://baike.baidu.com/item/参数传递/9019335)给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

**实现的机制**

[1]定义一个回调函数；

[2]提供函数实现的一方在初始化的时候，将回调函数的函数[指针](https://baike.baidu.com/item/指针)注册给调用者；

[3]当特定的事件或条件发生的时候，调用者使用[函数指针](https://baike.baidu.com/item/函数指针)调用回调函数对事件进行处理。