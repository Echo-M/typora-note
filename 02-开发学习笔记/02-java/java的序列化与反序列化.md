[参考文档](https://juejin.im/post/5ce3cdc8e51d45777b1a3cdf)

# java的序列化与反序列化

### 一、含义、意义和适用场景

**背景**：Java平台允许我们在内存中创建可复用的Java对象，但一般情况下，只有当JVM处于运行时，这些对象才可能存在，即，这些对象的生命周期不会比JVM的生命周期更长。但在现实应用中，就可能要求在JVM停止运行之后能够保存(持久化)指定的对象，并在将来重新读取被保存的对象。Java对象序列化就能够帮助我们实现该功能。

**序列化**：将对象写入到IO流中

**反序列化**：从IO流中恢复对象

**意义**：序列化机制允许将实现序列化的Java对象转换位字节序列，这些字节序列可以保存在磁盘上，或通过网络传输，以达到以后恢复成原来的对象。序列化机制使得对象可以脱离程序的运行而独立存在。必须注意地是，对象序列化保存的是对象的”状态”，即它的成员变量。由此可知，**对象序列化不会关注类中的静态变量**。

<font color = dark>**使用场景：所有可在网络上传输的对象都必须是可序列化的，**比如RMI（remote method invoke,即远程方法调用），传入的参数或返回的对象都是可序列化的，否则会出错；**所有需要保存到磁盘的java对象都必须是可序列化的。** </font>

**通常建议：程序创建的每个JavaBean类都实现Serializeable接口。**

### 二、Serializable接口实现序列化

如果需要将某个对象保存到磁盘上或者通过网络传输，那么这个类应该<font color = dark>实现**Serializable**接口</font>或者**Externalizable**接口之一。

#### 2.1 如何实现序列化

- **步骤一：创建一个ObjectOutputStream输出流；**
- **步骤二：调用ObjectOutputStream对象的writeObject输出可序列化对象。**

```java
public class Person implements Serializable {  
	private String name;  
	private int age;  
	
	//我不提供无参构造器  
	public Person(String name, int age) {      
		this.name = name;      
		this.age = age;  
	}  
	
	@Override  public String toString() {      
		return "Person{" +              
						"name='" + name + '\'' +              
						", age=" + age +              
						'}';  
	}
}
	
public class WriteObject {  
	public static void main(String[] args) {      
		try (//创建一个ObjectOutputStream输出流           
			ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("object.txt"))) {          
      //将对象序列化到文件s          
      Person person = new Person("9龙", 23);          
      oos.writeObject(person);      
    } catch (Exception e) {          
      	e.printStackTrace();      
    	}  
  	}
}
```

#### 2.2 如何实现反序列化

- **步骤一：创建一个ObjectInputStream输入流；**
- **步骤二：调用ObjectInputStream对象的readObject()得到序列化的对象。**

```java
public class Person implements Serializable {  
	private String name;
  private int age;
  //我不提供无参构造器
  public Person(String name, int age) {
    System.out.println("反序列化，你调用我了吗？");
    this.name = name;
    this.age = age;
  }  
  
  @Override  
  public String toString() {
    return "Person{" + 
    "name='" + name + '\'' +    
    ", age=" + age +           
    '}';  
  }
}
  
public class ReadObject {
	public static void main(String[] args) {
  	try (//创建一个ObjectInputStream输入流      
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.txt"))) {          
    	Person brady = (Person) ois.readObject();  
      System.out.println(brady);   
    } catch (Exception e) {   
    		e.printStackTrace();    
    }  
  }
}

//输出结果
//Person{name='9龙', age=23}
```

#### 2.3 成员是引用的序列化

如果一个可序列化的类的成员不是基本类型，也不是String类型，那这个引用类型也必须是可序列化的；否则，会导致此类不能序列化。

#### 2.4 序列化的顺序&同一个对象序列化多次

1、同一对象序列化多次，会将这个对象序列化多次吗？答案是否定的。

```java
public class WriteTeacher {
  public static void main(String[] args) throws Exception {
    try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("teacher.txt"))) {
      Person person = new Person("路飞", 20);
      Teacher t1 = new Teacher("雷利", person);
      Teacher t2 = new Teacher("红发香克斯", person);
      //依次将4个对象写入输入流
      oos.writeObject(t1);
      oos.writeObject(t2);
      oos.writeObject(person);
      oos.writeObject(t2);
    }
  }
}
```

依次将t1、t2、person、t2对象序列化到文件teacher.txt文件中。

2、反序列化的顺序与序列化的顺序一致。

```java
public class ReadTeacher {
  public static void main(String[] args) {
    try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("teacher.txt"))) {
      Teacher t1 = (Teacher) ois.readObject();
      Teacher t2 = (Teacher) ois.readObject();
      Person p = (Person) ois.readObject();
      Teacher t3 = (Teacher) ois.readObject();
      System.out.println(t1 == t2);
      System.out.println(t1.getPerson() == p);
      System.out.println(t2.getPerson() == p); 
      System.out.println(t2 == t3); 
      System.out.println(t1.getPerson() == t2.getPerson());
    } catch (Exception e) {
      e.printStackTrace(); 
    }    
  }
}

//输出结果
//false
//true
//true
//true
//true
```

从输出结果可以看出，**Java序列化同一对象，并不会将此对象序列化多次得到多个对象。**

#### 2.5 序列化过程

Java序列化算法

1. 所有保存到磁盘的对象都有一个序列化编码号
2. 当程序试图序列化一个对象时，会先检查此对象是否已经序列化过，只有此对象从未（在此虚拟机）被序列化过，才会将此对象序列化为字节序列输出。
3. 如果此对象已经序列化过，则直接输出编号即可。

下图为上述序列化过程

![image-20191217093637467](/Users/baola/Library/Application Support/typora-user-images/image-20191217093637467.png)

##### java序列化算法潜在的问题

由于java序利化算法不会重复序列化同一个对象，只会记录已序列化对象的编号。**如果序列化一个可变对象（对象内的内容可更改）后，更改了对象内容，再次序列化，并不会再次将此对象转换为字节序列，而只是保存序列化编号。**

##### 可选的自定义序列化

1. 有些时候，我们有这样的需求，某些属性不需要序列化。**使用transient关键字选择不需要序列化的字段。**

   ```java
   public class Person implements Serializable {
     //不需要序列化名字与年龄   
     private transient String name;
     private transient int age;
     private int height;
     private transient boolean singlehood; 
     public Person(String name, int age) {
       this.name = name;     
       this.age = age;  
     }   
     //省略get,set方法
   }
   
   public class TransientTest { 
     public static void main(String[] args) throws Exception { 
       try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt")); 
            ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) { 
           Person person = new Person("9龙", 23);   
           person.setHeight(185);          
         	System.out.println(person);     
         	oos.writeObject(person);       
         	Person p1 = (Person)ios.readObject();
         	System.out.println(p1);  
       }   
     }
   }
   
   //输出结果
   //Person{name='9龙', age=23', singlehood=true', height=185cm}
   //Person{name='null', age=0', singlehood=false', height=185cm}
   ```

   从输出我们看到，**使用transient修饰的属性，java序列化时，会忽略掉此字段，所以反序列化出的对象，被transient修饰的属性是默认值。对于引用类型，值是null；基本类型，值是0；boolean类型，值是false。**

2. 使用transient虽然简单，但将此属性完全隔离在了序列化之外。java提供了**可选的自定义序列化。**可以进行控制序列化的方式，或者对序列化数据进行编码加密等。

   ```java
   private void writeObject(java.io.ObjectOutputStream out) throws IOException；
   private void readObject(java.io.ObjectIutputStream in) throws IOException,ClassNotFoundException;
   private void readObjectNoData() throws ObjectStreamException;
   ```

   通过重写writeObject与readObject方法，可以自己选择哪些属性需要序列化， 哪些属性不需要。如果writeObject使用某种规则序列化，则相应的readObject需要相反的规则反序列化，以便能正确反序列化出对象。这里展示对名字进行反转加密。

   ```java
   public class Person implements Serializable { 
     private String name;  
     private int age;   
     //省略构造方法，get及set方法  
     
     private void writeObject(ObjectOutputStream out) throws IOException {       
       //将名字反转写入二进制流    
       out.writeObject(new StringBuffer(this.name).reverse());   
       out.writeInt(age);   
     }  
     
     private void readObject(ObjectInputStream ins) throws IOException,ClassNotFoundException {      
       //将读出的字符串反转恢复回来      
       this.name = ((StringBuffer)ins.readObject()).reverse().toString();     
       this.age = ins.readInt(); 
     }
   }
   ```

   当序列化流不完整时，readObjectNoData()方法可以用来正确地初始化反序列化的对象。例如，使用不同类接收反序列化对象，或者序列化流被篡改时，系统都会调用readObjectNoData()方法来初始化反序列化的对象。

3. **更彻底的自定义序列化**

   ANY-ACCESS-MODIFIER Object writeReplace() throws ObjectStreamException;
   ANY-ACCESS-MODIFIER Object readResolve() throws ObjectStreamException;

   - **writeReplace：在序列化时，会先调用此方法，再调用writeObject方法。此方法可将任意对象代替目标序列化对象**

     ```java
     public class Person implements Serializable { 
       private String name;  
       private int age;  
       //省略构造方法，get及set方法  
       
       private Object writeReplace() throws ObjectStreamException {    
         ArrayList<Object> list = new ArrayList<>(2);  
         list.add(this.name);    
         list.add(this.age);     
         return list;  
       }   
       
       public static void main(String[] args) throws Exception {   
         try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));       
              ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {    
           Person person = new Person("9龙", 23);     
           oos.writeObject(person);     
           ArrayList list = (ArrayList)ios.readObject();          
           System.out.println(list);   
         } 
       }
     }
     
     //输出结果
     //[9龙, 23]
     ```

   - **readResolve：反序列化时替换反序列化出的对象，反序列化出来的对象被立即丢弃。此方法在readeObject后调用。**

     ```java
     public class Person implements Serializable {   
       private String name;   
       private int age;   
       //省略构造方法，get及set方法  
       
       private Object readResolve() throws ObjectStreamException{    
         return new ("brady", 23);  
       }   
       
       public static void main(String[] args) throws Exception {   
         try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));        
              ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {     
           Person person = new Person("9龙", 23);      
           oos.writeObject(person);        
           HashMap map = (HashMap)ios.readObject();    
           System.out.println(map);    
         }   
       }
     }
     
     //输出结果
     //{brady=23}
     ```

     **readResolve常用来反序列单例类，保证单例类的唯一性。**

     **注意：readResolve与writeReplace的访问修饰符可以是private、protected、public，如果父类重写了这两个方法，子类都需要根据自身需求重写，这显然不是一个好的设计。通常建议对于final修饰的类重写readResolve方法没有问题；否则，重写readResolve使用private修饰。**

### 三、Externalizable：强制自定义序列化

通过实现Externalizable接口，必须实现writeExternal、readExternal方法。

```java
public interface Externalizable extends java.io.Serializable { 
  void writeExternal(ObjectOutput out) throws IOException;   
  void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
}
```

```java
public class ExPerson implements Externalizable { 
  private String name;   
  private int age;   
  
  //注意，必须加上pulic 无参构造器  
  public ExPerson() { 
  }    
  
  public ExPerson(String name, int age) {  
    this.name = name;    
    this.age = age;   
  }    
  
  @Override  
  public void writeExternal(ObjectOutput out) throws IOException {  
    //将name反转后写入二进制流   
    StringBuffer reverse = new StringBuffer(name).reverse();  
    System.out.println(reverse.toString());    
    out.writeObject(reverse);     
    out.writeInt(age);   
  }   
  
  @Override   
  public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {    
    //将读取的字符串反转后赋值给name实例变量   
    this.name = ((StringBuffer) in.readObject()).reverse().toString();     
    System.out.println(name);     
    this.age = in.readInt();  
  }   
  
  public static void main(String[] args) throws IOException, ClassNotFoundException {  
    try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ExPerson.txt"));        
         ObjectInputStream ois = new ObjectInputStream(new FileInputStream("ExPerson.txt"))) 
    {          
      oos.writeObject(new ExPerson("brady", 23));      
      ExPerson ep = (ExPerson) ois.readObject();      
      System.out.println(ep);      
    }   
  }
}

//输出结果
//ydarb
//brady
//ExPerson{name='brady', age=23}
```

注意：Externalizable接口不同于Serializable接口，实现此接口必须实现接口中的两个方法实现自定义序列化，这是强制性的；特别之处是必须提供pulic的无参构造器，因为在反序列化的时候需要反射创建对象。**

### 四、两种序列化方式的对比

![image-20191217095023207](/Users/baola/Library/Application Support/typora-user-images/image-20191217095023207.png)

虽然Externalizable接口带来了一定的性能提升，但变成复杂度也提高了，所以一般通过实现Serializable接口进行序列化。

### 五、序列化的版本号

我们知道，**反序列化必须拥有class文件，但随着项目的升级，class文件也会升级，序列化怎么保证升级前后的兼容性呢？**

java序列化提供了一个private static final long serialVersionUID 的序列化版本号，只有版本号相同，即使更改了序列化属性，对象也可以正确被反序列化回来。

```java
public class Person implements Serializable { 
  //序列化版本号    
  private static final long serialVersionUID = 1111013L; 
  private String name; 
  private int age;    
  //省略构造方法及get,set
}
```

如果反序列化使用的**class的版本号**与序列化时使用的**不一致**，反序列化会**报InvalidClassException异常。**

![image-20191217095335450](/Users/baola/Library/Application Support/typora-user-images/image-20191217095335450.png)

**序列化版本号可自由指定，如果不指定，JVM会根据类信息自己计算一个版本号，这样随着class的升级，就无法正确反序列化；不指定版本号另一个明显隐患是，不利于jvm间的移植，可能class文件没有更改，但不同jvm可能计算的规则不一样，这样也会导致无法反序列化。**

什么情况下需要修改serialVersionUID呢？分三种情况。

- 如果只是修改了方法，反序列化不容影响，则无需修改版本号；
- 如果只是修改了静态变量，瞬态变量（transient修饰的变量），反序列化不受影响，无需修改版本号；
- 如果修改了非瞬态变量，则可能导致反序列化失败。**如果新类中实例变量的类型与序列化时类的类型不一致，则会反序列化失败，这时候需要更改serialVersionUID。**如果只是新增了实例变量，则反序列化回来新增的是默认值；如果减少了实例变量，反序列化时会忽略掉减少的实例变量。

### 六、总结

1. <font color = red>所有需要网络传输的对象都需要实现序列化接口，通过建议所有的javaBean都实现Serializable接口。</font>
2. <font color = red>对象的类名、实例变量（包括基本类型，数组，对其他对象的引用）都会被序列化；方法、类变量、transient实例变量都不会被序列化。</font>
3. <font color = red>如果想让某个变量不被序列化，使用transient修饰。</font>
4. <font color = red>序列化对象的引用类型成员变量，也必须是可序列化的，否则，会报错。</font>
5. <font color = red>反序列化时必须有序列化对象的class文件。</font>
6. <font color = red>当通过文件、网络来读取序列化后的对象时，必须按照实际写入的顺序读取。</font>
7. <font color = red>单例类序列化，需要重写readResolve()方法；否则会破坏单例原则。</font>
8. <font color = red>同一对象序列化多次，只有第一次序列化为二进制流，以后都只是保存序列化编号，不会重复序列化。</font>
9. <font color = red>建议所有可序列化的类加上serialVersionUID 版本号，方便项目升级。</font>