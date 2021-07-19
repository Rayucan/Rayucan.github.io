---
title: SSM框架之SpringMVC
date: 2020-11-01 00:00:00
tags: SSM框架
categories: 笔记
description: 轻量级的、基于MVC的Web层应用框架。
---

## SpringMVC简介

①  一种轻量级的、基于MVC的Web层应用框架。偏前端而不是基于业务逻辑层。

②  Spring框架的一个后续产品。



### SpringMVC特性

①  Spring 为展现层提供的基于 MVC 设计理念的优秀的 Web 框架，是目前最主流的 MVC 框架之一。

②  Spring3.0 后全面超越 Struts2，成为最优秀的 MVC 框架。

③  Spring MVC 通过一套 MVC 注解，让 POJO 成为处理请求的控制器，而无须实现任何接口。

④  支持 REST 风格的 URL请求。

⑤  采用了松散耦合可插拔组件结构，比其他 MVC 框架更具扩展性和灵活性。

### SpringMVC能做什么

* 天生与Spring框架集成，如：(IOC,AOP)

* 支持Restful风格

* 进行更简洁的Web层开发

* 支持灵活的URL到页面控制器的映射

* 非常容易与其他视图技术集成，如:Velocity、FreeMarker等等。

* 因为模型数据不存放在特定的API里，而是放在一个Model里(Map数据结构实现，因此很容易被其他框架使用)

* 非常灵活的数据验证、格式化和数据绑定机制、能使用任何对象进行数据绑定，不必实现特定框架的API

* 更加简单、强大的异常处理

* 对静态资源的支持

* 支持灵活的本地化、主题等解析

### 常用组件

* **DispatcherServlet**：前端控制器

* **Controller**：处理器/页面控制器，做的是MVC中的C的事情，但控制逻辑转移到前端控制器了，用于对请求进行处理

* **HandlerMapping** ：请求映射到处理器，找谁来处理，如果映射成功返回一个HandlerExecutiongChain对象（包含一个Handler处理器(页面控制器)对象、多个**HandlerInterceptor**拦截器对象）

* **ViewResolver** : 视图解析器，找谁来处理返回的页面。把逻辑视图解析为具体的View,进行这种策略模式，很容易更换其他视图技术；

* 如InternalResourceViewResolver将逻辑视图名映射为JSP视图

* **LocalResolver**：本地化、国际化

* **MultipartResolver**：文件上传解析器

* **HandlerExceptionResolver**：异常处理器



## @RequestMapping映射

### 请求注解

* SpringMVC使用@RequestMapping注解为控制器指定可以处理哪些 URL 请求

* 在控制器的**类定义及方法定义处**都可标注 @RequestMapping
  * **标记在类上**：提供初步的请求映射信息。相对于 WEB 应用的根目录
  * **标记在方法上**：提供进一步的细分映射信息。相对于标记在类上的 URL。

* 若类上未标注 @RequestMapping，则方法处标记的 URL 相对于 WEB 应用的根目录

作用：DispatcherServlet 截获请求后，就通过控制器上 @RequestMapping 提供的映射信息确定请求所对应的处理方法。 

### 源码

```java
package org.springframework.web.bind.annotation;
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
String[] value() default {};
RequestMethod[] method() default {};
String[] params() default {};
String[] headers() default {};
String[] consumes() default {};
String[] produces() default {};
}
```

### 请求方式

①  标准的 HTTP 请求报头

![BwJToQ.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151144.png)

②  映射请求参数、请求方法或请求头

@RequestMapping 除了可以使用请求 URL 映射请求外，还可以使用请求方法、请求参数及请求头映射请求

@RequestMapping 的 value【重点】、method【重点】、params【了解】 及 heads【了解】 

分别表示请求 URL、请求方法、请求参数及请求头的映射条件，他们之间是与的关系，联合使用多个条件可让请求映射更加精确化。

③  params 和 headers支持简单的表达式：

* param1: 表示请求必须包含名为 param1 的请求参数

* !param1: 表示请求不能包含名为 param1 的请求参数

* param1 != value1: 表示请求包含名为 param1 的请求参数，但其值不能为 value1

* {"param1=value1", "param2"}: 请求必须包含名为 param1 和param2 的两个请求参数，且 param1 参数的值必须为 value1

### 请求参数&请求头

①  RequestMapping_请求参数&请求头【了解】

```java
//了解: 可以使用 params 和 headers 来更加精确的映射请求. params 和 headers 支持简单的表达式.
@RequestMapping(value="/testParamsAndHeaders",
params= {"username","age!=10"}, headers = { "Accept-Language=en-US,zh;q=0.8" })
public String testParamsAndHeaders(){
System.out.println("testParamsAndHeaders...");
return "success";
}
```

②  请求URL

```java
<!--设置请求参数和请求头信息 -->        
<a href="springmvc/testParamsAndHeaders">testParamsAndHeaders</a>
```

### 支持Ant 路径风格

* **Ant** **风格资源地址支持** **3** **种匹配符**：【了解】
  * ?：匹配文件名中的一个字符
  * *：匹配文件名中的任意字符
  * ** ： 匹配多层路径

* @RequestMapping 还**支持** **Ant** **风格的** **URL**：

  ```
  /user/*/createUser
  匹配 /user/aaa/createUser、/user/bbb/createUser 等 URL
  /user/**/createUser
  匹配 /user/createUser、/user/aaa/bbb/createUser 等 URL
  /user/createUser??
  匹配 /user/createUseraa、/user/createUserbb 等 URL
  ```

### 请求占位符PathVariable注解

* @PathVariable 映射 URL 绑定的占位符

* 带占位符的 URL 是 Spring3.0新增的功能，该功能在 SpringMVC 向 REST 目标挺进发展过程中具有里程碑的意义

* 通过 @PathVariable 可以将 URL中占位符参数绑定到控制器处理方法的入参中：
  * URL 中的 {**xxx**} 占位符可以通过 @PathVariable("**xxx**") 绑定到操作方法的入参中



## RESTful

### REST风格是什么？

①  REST：即 Representational State Transfer。**（资源）表现层状态转化。是目前最流行的一种互联网软件架构**。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用

* **资源（Resources**）：网络上的一个实体，或者说是网络上的一个具体信息。

它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。

可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的 URI 。

获取这个资源，访问它的URI就可以，因此 URI 即为每一个资源的独一无二的识别符。

* **表现层（Representation）**：把资源具体呈现出来的形式，叫做它的表现层（Representation）。比如，文本可以用 txt 格式表现，也可以用 HTML 格式、XML 格式、JSON 格式表现，甚至可以采用二进制格式。

* **状态转化（State Transfer）**：每发出一个请求，就代表了客户端和服务器的一次交互过程。HTTP协议，是一个无状态协议，即所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生“状态转化”（State Transfer）。而这种转化是建立在表现层之上的，所以就是 “表现层状态转化”。

* 具体说，就是 HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：

  ***GET 用来获取资源，POST 用来新建资源，PUT用来更新资源，DELETE用来删除资源。***

②  URL风格示例

/order/1 HTTP **GET** ：得到 id = 1 的 order  

/order/1 HTTP **DELETE**：删除 id = 1的 order  

/order/1 HTTP **PUT**：更新id = 1的 order  

/order   HTTP **POST**：新增 order 

③  HiddenHttpMethodFilter

浏览器 form 表单只支持 GET 与 POST 请求，而DELETE、PUT 等 method 并不支持，

Spring3.0 添加了一个过滤器，可以将这些请求转换为标准的 http 方法，使得支持 GET、POST、PUT 与 DELETE 请求。



## 请求数据传入

### 请求处理方法签名

* Spring MVC 通过分析处理方法的签名，HTTP请求信息绑定到处理方法的相应人参中。

* Spring MVC 对控制器处理方法签名的限制是很宽松的，几乎可以按喜欢的任何方式对方法进行签名。 

* 必要时可以对方法及方法入参标注相应的注解（ @PathVariable 、@RequestParam、@RequestHeader 等）、

* Spring MVC 框架会将 HTTP 请求的信息绑定到相应的方法入参中，并根据方法的返回值类型做出相应的后续处理。

### @RequestParam注解

* 在处理方法入参处使用 @RequestParam 可以把请求参数传递给请求方法

* value：参数名

* required：是否必须。默认为 true, 表示请求参数中必须包含对应的参数，若不存在，将抛出异常

* defaultValue: 默认值，当没有传递参数时使用该值

### @RequestHeader 注解

* 使用 @RequestHeader 绑定请求报头的属性值

* 请求头包含了若干个属性，服务器可据此获知客户端的信息，**通过@RequestHeader即可将请求头中的属性值绑定到处理方法的入参中**

  ![BwNmPH.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151119.png)



### @CookieValue 注解

* 使用 @CookieValue 绑定请求中的 Cookie 值

* **@CookieValue** 可让处理方法入参绑定某个 Cookie 值

  ![BwNtiQ.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151134.png)

### 使用POJO作为参数

* 使用 POJO 对象绑定请求参数值

* Spring MVC **会按请求参数名和 POJO属性名进行自动匹配，自动为该对象填充属性值，支持级联属性**。如：dept.deptId、dept.address.tel 等



## 响应数据传出

### SpringMVC 输出模型数据概述

提供了以下几种途径输出模型数据：

* **ModelAndView**: 处理方法返回值类型为 ModelAndView 时, 方法体即可通过该对象添加模型数据

* **Map** **及 Model**: 入参为 org.springframework.ui.Model、

  org.springframework.ui.ModelMap 或 java.uti.Map 时，处理方法返回时，Map 中的数据会自动添加到模型中。

* **@SessionAttributes**: 将模型中的某个属性暂存到 HttpSession 中，以便多个请求之间可以共享这个属性

* **@ModelAttribute**: 方法入参标注该注解后, 入参的对象就会放到数据模型中

### 处理模型数据之 ModelAndView

①  控制器处理方法的返回值如果为 ModelAndView, 则其既包含视图信息，也包含模型数据信息。

②  添加模型数据:

```java
MoelAndView addObject(String attributeName, Object attributeValue)

ModelAndView addAllObject(Map<String, ?> modelMap)
```

③  设置视图:

```java
void setView(View view)

void setViewName(String viewName)
```



### 处理模型数据之 Map

* Spring MVC 在内部使用了一个 org.springframework.ui.Model 接口存储模型数据

  具体使用步骤

  ​	1)    **Spring MVC在调用方法前会创建一个隐含的模型对象作为模型数据的存储容器**。

  ​	2)    **如果方法的入参为 Map或 Model类型**，Spring MVC 会将隐含模型的引用传递给这些入参。

  ​	3)    在方法体内，开发者可以通过这个入参对象访问到模型中的所有数据，也可以向模型中添加**新的属性数据**
  
  

### 处理模型数据之 SessionAttributes 注解

* **若希望在多个请求之间共用某个模型属性数据**，则可以在控制器类上标注一个 **@SessionAttributes**, Spring MVC 将在模型中对应的属性暂存到 HttpSession 中。

* @SessionAttributes 除了可以通过**属性名**指定需要放到会话中的属性外，还可以通过模型属性的**对象类型**指定哪些模型属性需要放到会话中

  例如：

  ​	①  @SessionAttributes(types=User.class) 会将隐含模型中所有类型为 User.class 的属性添加到会话中。

  ​	②  @SessionAttributes(value={“user1”, “user2”})

  ​	③  @SessionAttributes(types={User.class, Dept.class})

  ​	④  @SessionAttributes(value={“user1”, “user2”}, types={Dept.class}) 



### @SessionAttributes 源码

```java
package org.springframework.web.bind.annotation;
 
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
 
@Target({ElementType.TYPE}) //说明这个注解只能应用在类型上面
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface SessionAttributes { 
String[] value() default {};   //推荐使用 
Class<?>[] types() default {};  //范围太广 
}
```



## @ModelAttribute注解

### 使用场景

![BwI4Xt.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151332.png)
![BwIInP.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151418.png)

### 源码

```java
@Target({ElementType.PARAMETER, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ModelAttribute {
/**
 * The name of the model attribute to bind to.
 * <p>The default model attribute name is inferred from the declared
 * attribute type (i.e. the method parameter type or method return type),
 * based on the non-qualified class name:
 * e.g. "orderAddress" for class "mypackage.OrderAddress",
 * or "orderAddressList" for "List&lt;mypackage.OrderAddress&gt;".
 */
String value() default ""; 
}
```

### 运行原理

①  执行@ModelAttribute注解所修饰的方法，将从数据库中获取的对象存放到Map集合中，key为user

②  SpringMVC从Map集合中获取 user对象，将表单数据封装到与参数名称对应的user对象属性上

③  SpringMVC将user对象作为参数，传递给目标方法。

④  注意：@ModelAttribute 注解修饰的方法中，放入到Map集合中的key值，应该和目标方法参数类型的类名称首字母小写一致。

### 源码分析

①  调用 @ModelAttribute 注解修饰的方法. 实际上把 @ModelAttribute 方法中 Map 中的数据放在了 implicitModel 中.

②  解析请求处理器的目标参数, 实际上该目标参数来自于 WebDataBinder 对象的 target 属性

**1). 创建 WebDataBinder 对象:**

​		【1】确定 objectName 属性: 若传入的 attrName 属性值为 "", 则 objectName 为类名第一个字母小写. 

​		注意: attrName. 若目标方法的 POJO 属性使用了 @ModelAttribute 来修饰, 则 attrName 值即为 @ModelAttribute 的 value 属性值  

​		【2】确定 target 属性:

​		在 implicitModel 中查找 attrName 对应的属性值. 

* 若存在, ok

* 若不存在，则验证当前 Handler 是否使用了 @SessionAttributes 进行修饰, 若使用了, 则尝试从 Session 中获取 attrName 所对应的属性值. 

* 若 session 中没有对应的属性值, 则抛出了异常. 

* 若 Handler 没有使用 @SessionAttributes 进行修饰, 或 @SessionAttributes 中没有使用 value 值指定的 key和 attrName 相匹配, 则通过反射创建了 POJO 对象 

**2). SpringMVC 把表单的请求参数赋给了 WebDataBinder 的 target 对应的属性.** 

**3). SpringMVC 会把 WebDataBinder 的 attrName 和 target 给到 implicitModel. 进而传到 request 域对象中.** 

**4). 把 WebDataBinder 的 target 作为参数传递给目标方法的入参.** 



### SpringMVC 确定目标方法 POJO 类型入参的过程

①  确定一个 key:

* 若目标方法的 POJO 类型的参数木有使用 @ModelAttribute 作为修饰, 则 key 为 POJO 类名第一个字母的小写

* 若使用了@ModelAttribute 来修饰, 则 key 为 @ModelAttribute 注解的 value 属性值. 

②  在 implicitModel 中查找 key 对应的对象, 若存在, 则作为入参传入

* 若在 @ModelAttribute 标记的方法中在 Map 中保存过, 且 key 和 ① 确定的 key 一致, 则会获取到. 

③  若 implicitModel 中不存在 key 对应的对象, 则检查当前的 Handler 是否使用 @SessionAttributes 注解修饰, 

④  若使用了该注解, 且 @SessionAttributes 注解的 value 属性值中包含了 key, 则会从 HttpSession 中来获取 key 所对应的 value 值, 若存在则直接传入到目标方法的入参中. 若不存在则将抛出异常. 

⑤  若 Handler 没有标识 @SessionAttributes 注解或 @SessionAttributes 注解的 value 值中不包含 key, 则会通过反射来创建 POJO 类型的参数, 传入为目标方法的参数

SpringMVC 会把 key 和 POJO 类型的对象保存到 implicitModel 中, 进而会保存到request 中. 



### @ModelAttribute注解修饰POJO类型的入参

```java
@RequestMapping("/testModelAttribute")
//public String testModelAttribute(User user){
 
public String testModelAttribute(@ModelAttribute("abc") User user){
 
System.out.println("修改 user="+user);                
return "success";
}
 
/**
 * @ModelAttribute 注解也可以来修饰目标方法 POJO 类型的入参, 其 value 属性值有如下的作用:
1). SpringMVC 会使用 value 属性值在 implicitModel 中查找对应的对象, 若存在则会直接传入到目标方法的入参中.
2). SpringMVC 会以 value 为 key, POJO 类型的对象为 value, 存入到 request 中. 
 */
@ModelAttribute
public void getUser(@RequestParam(value="id",required=false) Integer id,Map<String,Object> map){
if(id!=null){        
//模拟从数据库中获取到的user对象
User user = new User(1,"Tom","123456","tom@atguigu.com",12);
System.out.println("从数据库中查询的对象：user="+user );
//map.put("user", user); //BindingAwareModelMap
 
//map.put("abc", user); //BindingAwareModelMap
}
}
```

```java
user user: ${requestScope.user }
<br><br>  
abc user: ${requestScope.abc }
<br><br>      
```



### @sessionAttributes注解引发的异常

①   由@SessionAttributes引发的异常                               

②   如果在处理类定义处标注了@SessionAttributes(“xxx”)，则尝试从会话中获取该属性，并将其赋给该入参，然后再用请求消息填充该入参对象。如果在会话中找不到对应的属性，则抛出 HttpSessionRequiredException 异常



## 视图解析

### 视图和视图解析器

* 请求处理方法执行完成后，最终返回一个 ModelAndView 对象。对于那些返回 String，View 或 ModeMap 等类型的处理方法，**Spring MVC** **也会在内部将它们装配成一个 ModelAndView** **对象**，它包含了逻辑名和模型对象的视图

* Spring MVC 借助**视图解析器**（**ViewResolver**）得到最终的视图对象（View），最终的视图可以是 JSP ，也可能是 Excel、JFreeChart等各种表现形式的视图

* 对于最终究竟采取何种视图对象对模型数据进行渲染，处理器并不关心，处理器工作重点聚焦在生产模型数据的工作上，从而实现 MVC 的充分解耦

#### 视图

* **视图**的作用是渲染模型数据，将模型里的数据以某种形式呈现给客户

* 为了实现视图模型和具体实现技术的解耦，Spring 在 org.springframework.web.servlet 包中定义了一个高度抽象的 **View** 接口

* **视图对象由视图解析器负责实例化**。由于视图是**无状态**的，所以他们不会有**线程安全**的问题

#### 视图解析器

* SpringMVC 为逻辑视图名的解析提供了不同的策略，可以在 Spring WEB 上下文中**配置一种或多种解析策略**，**并指定他们之间的先后顺序**。每一种映射策略对应一个具体的视图解析器实现类

* 视图解析器的作用比较单一：将逻辑视图解析为一个具体的视图对象

* 所有的视图解析器都必须实现 ViewResolver 接口



### mvc:view-controller标签

* 若希望直接响应通过 SpringMVC 渲染的页面，可以使用 **mvc:view-controller** 标签实现

```java
<!-- 直接配置响应的页面：无需经过控制器来执行结果 -->
<mvc:view-controller path="/success" view-name="success"/>
```

* 请求的路径：

```java
http://localhost:8080/SpringMVC_02_View/success
```

* 配置<mvc:view-controller>会导致其他请求路径失效，解决方法：

```java
<!-- 在实际开发过程中都需要配置mvc:annotation-driven标签，后面讲，这里先配置上 -->
<mvc:annotation-driven/>
```



### 自定义视图

* 自定义视图（需要加入SpringMVC，那么一定需要实现框架的接口）

* 若希望使用 Excel 展示数据列表，仅需要扩展 SpringMVC 提供的 **AbstractExcelView** 或 AbstractJExcelView 即可。

* 实现 buildExcelDocument() 方法，在方法中使用模型数据对象构建 Excel 文档就可以了。

* **AbstractExcelView** **基于** **POI API**，而 AbstractJExcelView 是基于 JExcelAPI 的。

* **视图对象需要配置** **IOC** **容器中的一个** **Bean****，使用** **BeanNameViewResolver** **作为视图解析器即可**

* 若希望直接在浏览器中直接下载 Excel 文档，则可以设置响应头 **Content-Disposition** 的值为 **attachment;filename=xxx.xls**



### 重定向

①  关于重定向

* 一般情况下，控制器方法返回字符串类型的值会被当成逻辑视图名处理

* 如果返回的字符串中带 **forward:** **或** **redirect:** 前缀时，SpringMVC 会对他们进行特殊处理：将 forward: 和 redirect: 当成指示符，其后的字符串作为 URL 来处理

* redirect:success.jsp：会完成一个到 success.jsp 的重定向的操作

* forward:success.jsp：会完成一个到 success.jsp 的转发操作

②  定义页面链接

 ```java
<a href="springmvc/testRedirect">testRedirect</a>
 ```

③  定义控制器方法

```java
@RequestMapping("/testRedirect")
public String testRedirect(){
System.out.println("testRedirect");
return "redirect:/index.jsp";
//return "forward:/index.jsp";
}
```



## Restful CURD



## 数据绑定流程

### 提出问题

日期字符串格式的表单参数，提交后转换为Date类型

```java
 <!--    
 解决问题：
 1.数据类型转换
 2.数据格式
 3.数据校验
 -->    
BirthDay :<form:input path="birthDay"/> 
    
Employee类中增加日期类型属性：
//关于类型转换 
private Date birthDay ;
```

### 数据绑定流程原理

①  Spring MVC 主框架将 ServletRequest 对象及目标方法的入参实例传递给 WebDataBinderFactory 实例，以创建 **DataBinder** 实例对象

②  DataBinder 调用装配在 Spring MVC 上下文中的 **ConversionService** 组件进行**数据类型转换、数据格式化**工作。将 Servlet 中的请求信息填充到入参对象中

③  调用 **Validator** 组件对已经绑定了请求消息的入参对象进行数据合法性校验，并最终生成数据绑定结果 **BindingData** 对象

④  Spring MVC 抽取 **BindingResult** 中的入参对象和校验错误对象，将它们赋给处理方法的响应入参

### 自定义类型转换器

#### 类型转换器概述

* **ConversionService** 是 Spring 类型转换体系的核心接口。
* 可以利用 **ConversionServiceFactoryBean** 在 Spring 的 IOC 容器中定义一个ConversionService. Spring 将自动识别出 IOC 容器中的 ConversionService，并在Bean属性配置及Spring MVC处理方法入参绑定等场合使用它进行数据的转换
* 可通过ConversionServiceFactoryBean 的converters属性注册自定义的类型转换器



#### Spring支持的转换器类型

Spring 定义了 3 种类型的转换器接口，实现任意一个转换器接口都可以作为自定义转换器注册到 ConversionServiceFactoryBean 中：

* **Converter<S,T>**：将 S 类型对象转为 T 类型对象

* **ConverterFactory**：将相同系列多个 “同质” Converter 封装在一起。如果希望将一种类型的对象转换为另一种类型及其子类的对象（例如将 String 转换为 Number 及 Number 子类（Integer、Long、Double 等）对象）可使用该转换器工厂类

* **GenericConverter**：会根据源类对象及目标类对象所在的宿主类中的上下文信息进行类型转换



### &lt;mvc:annotation-driven /&gt;配置

①  直接配置响应的页面：无需经过控制器来执行结果 ；但会导致其他请求路径失效，需要配置mvc:annotation-driven标签

  <mvc:view-controller  path="/success" view-name="success"/>  

②  RESTful-CRUD操作，删除时，通过jQuery执行delete请求时，找不到静态资源，需要配置mvc:annotation-driven标签  

  <mvc:default-servlet-handler/> 将在 SpringMVC 上下文中定义一个  DefaultServletHttpRequestHandler，它会对进入 DispatcherServlet 的请求进行筛查，如果发现是没有经过映射的请求，就将该请求交由 WEB 应用服务器默认的 Servlet 处理，如果不是静态资源的请求，才由 DispatcherServlet 继续处理。  

③  配置类型转换器服务时，需要指定转换器服务引用

<mvc:annotation-driven conversion-service=“conversionService”/> 会将自定义的

ConversionService 注册到 Spring MVC 的上下文中



### <mvc:annotation-driven /> 作用

* <mvc:annotation-driven /> 会自动注册：

**RequestMappingHandlerMapping** 、**RequestMappingHandlerAdapter** 与

**ExceptionHandlerExceptionResolver** 三个bean。

* 还将提供以下支持：
  * 支持使用 **ConversionService** 实例对表单参数进行类型转换
  * 支持使用 **@NumberFormat****、****@DateTimeFormat** 注解完成数据类型的格式化
  * 支持使用 **@Valid** 注解对 JavaBean 实例进行 JSR 303 验证
  * 支持使用 **@RequestBody** 和 **@ResponseBody** 注解



### InitBinder注解

@InitBinder

* 由 **@InitBinder** 标识的方法，**可以对** **WebDataBinder** **对象进行初始化**。WebDataBinder 是 DataBinder 的子类，用于完成由表单字段到 JavaBean 属性的绑定

* **@InitBinder**方法不能有返回值，它必须声明为void。

* **@InitBinder**方法的参数通常是WebDataBinder

​                               

实验代码

```java
/**
由 @InitBinder 标识的方法，可以对 WebDataBinder 对象进行初始化。
WebDataBinder 是 DataBinder 的子类，用于完成由表单字段到 JavaBean 属性的绑定
@InitBinder方法不能有返回值，它必须声明为void。
@InitBinder方法的参数通常是 WebDataBinder
 */
@InitBinder
public void initBinder(WebDataBinder dataBinder){
dataBinder.setDisallowedFields("lastName");
}
```



### 数据的格式化

#### 数据格式化概述

* 对属性对象的输入/输出进行格式化，从其本质上讲依然属于 “类型转换” 的范畴。

* Spring 在格式化模块中定义了一个实现 ConversionService 接口的**FormattingConversionService** 实现类，该实现类扩展了 GenericConversionService，因此它**既具有类型转换的功能，又具有格式化的功能**

* FormattingConversionService 拥有一个 **FormattingConversionServiceFactroyBean** 工厂类，后者用于在 Spring 上下文中构造前者，FormattingConversionServiceFactroyBean 内部已经注册了 :

  * NumberFormatAnnotationFormatterFactroy：支持对数字类型的属性使用 **@NumberFormat** 注解
  *  JodaDateTimeFormatAnnotationFormatterFactroy：支持对日期类型的属性使用 **@DateTimeFormat** 注解

* 装配了 FormattingConversionServiceFactroyBean 后，就可以在 Spring MVC 入参绑定及模型数据输出时使用注解驱动了。

* <mvc:annotation-driven /> 默认创建的 **ConversionService** 实例即为

   DefaultFormattingConversionService

#### 日期格式化概述

* **@DateTimeFormat** **注解可对** **java.util.Date****、****java.util.Calendar****、****java.long.Long** **时间类型进行标注**：
  * **pattern** **属性**：类型为字符串。指定解析/格式化字段数据的模式，如：”**yyyy-MM-dd hh:mm:ss**”
  * iso 属性：类型为 DateTimeFormat.ISO。指定解析/格式化字段数据的ISO模式，包括四种：ISO.NONE（不使用） -- 默认、ISO.DATE(yyyy-MM-dd) 、ISO.TIME(hh:mm:ss.SSSZ)、 ISO.DATE_TIME(yyyy-MM-dd hh:mm:ss.SSSZ)
  * style 属性：字符串类型。通过样式指定日期时间的格式，由两位字符组成，第一位表示日期的格式，第二位表示时间的格式：S：短日期/时间格式、M：中日期/时间格式、L：长日期/时间格式、F：完整日期/时间格式、-：忽略日期或时间格式

#### 数值格式化概述

* **@NumberFormat** **可对类似数字类型的属性进行标注**，它拥有两个互斥的属性：

  * style：类型为 NumberFormat.Style。用于指定样式类型，包括三种：**Style.NUMBER**（正常数字类型）、 **Style.CURRENCY**（货币类型）、 **Style.PERCENT**（百分数类型）
  * **pattern**：类型为 String，自定义样式，如pattern="**#,###**"；

  ```java
  <!-- 声明类型转换器服务 -->
  <!-- <bean id="conversionService"
   class="org.springframework.context.support.ConversionServiceFactoryBean"> -->
  <bean id="conversionService"
   class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
  <property name="converters">
  <set>
  <!-- 引用类型转换器 -->
  <ref bean="stringToEmployeeConverter"/>
  </set>                        
  </property>
  </bean>
  ```



### 错误消息的显示及国际化

#### 在页面上显示错误

* Spring MVC 除了会将表单/命令对象的校验结果保存到对应的 BindingResult 或 Errors 对象中外，**还会将所有校验结果保存到** **“隐含模型”**

* 即使处理方法的签名中没有对应于表单/命令对象的结果入参，校验结果也会保存在 “隐含对象” 中。

* 隐含模型中的所有数据最终将通过 HttpServletRequest 的属性列表暴露给 JSP 视图对象，因此在 JSP 中可以获取错误信息

* 在 JSP 页面上可通过 <form:errors path=“userName”>显示错误消息



### 提示消息的国际化

* 每个属性在数据绑定和数据校验发生错误时，都会生成一个对应的 FieldError 对象。

* **当一个属性校验失败后，校验框架会为该属性生成** **4** **个消息代码**，这些代码以**校验注解类名为前缀**，结合 **modleAttribute****、属性名及属性类型名**生成多个对应的消息代码：例如 User 类中的 password 属性标注了一个 @Pattern 注解，当该属性值不满足 @Pattern 所定义的规则时, 就会产生以下 4 个错误代码：
  * Pattern.user.password
  * Pattern.password
  * Pattern.java.lang.String
  * Pattern
* 当使用 Spring MVC 标签显示错误消息时， Spring MVC 会查看 WEB 上下文是否装配了对应的国际化消息，如果没有，则显示默认的错误消息，否则使用国际化消息。
* 若数据类型转换或数据格式转换时发生错误，或该有的参数不存在，或调用处理方法时发生错误，都会在隐含模型中创建错误消息。其错误代码前缀说明如下：
  * **required**：必要的参数不存在。如 @RequiredParam(“param1”) 标注了一个入参，但是该参数不存在
  * **typeMismatch**：在数据绑定时，发生数据类型不匹配的问题
  * **methodInvocation**：Spring MVC 在调用处理方法时发生了错误



### 提示消息的国际化实验

①  定义国际化资源文件

```java
NotEmpty.employee.lastName=\u7528\u6237\u540D\u4E0D\u80FD\u4E3A\u7A7A
Email.employee.email=\u7535\u5B50\u90AE\u4EF6\u5730\u5740\u4E0D\u5408\u6CD5
Past.employee.birthDay=\u65E5\u671F\u5FC5\u987B\u662F\u4E00\u4E2A\u8FC7\u53BB\u7684\u65F6\u95F4
typeMismatch.employee.birthDay=\u4E0D\u662F\u4E00\u4E2A\u65E5\u671F\u6709\u6548\u683C\u5F0F
```

②  声明国际化资源配置

```java
<!-- 声明国际化资源文件 -->
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
<property name="basename" value="i18n"></property>
</bean>
```



### 返回JSON

①  加入 jar 包：

jackson-annotations-2.1.5.jar

jackson-core-2.1.5.jar

jackson-databind-2.1.5.jar

②  编写目标方法，使其返回 JSON 对应的对象或集合

```java
@ResponseBody  //SpringMVC对JSON的支持
@RequestMapping("/testJSON")
public Collection<Employee> testJSON(){
return employeeDao.getAll();
}
```

③  增加页面代码：index.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
 "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="scripts/jquery-1.9.1.min.js"></script>
<script type="text/javascript">
$(function(){ 
$("#testJSON").click(function(){
 
var url = this.href ;
var args = {};
$.post(url,args,function(data){
for(var i=0; i<data.length; i++){
var id = data[i].id;
var lastName = data[i].lastName ;
alert(id+" - " + lastName);
}
});
 
return false ;
});                
});
</script>
 
</head>
<body>
 
<a href="empList">To Employee List</a>
<br><br>
 
<a id="testJSON" href="testJSON">testJSON</a>
 
</body>
</html>
```



### HttpMessageConverter原理

#### HttpMessageConverter<T>

①  **HttpMessageConverter<T>** 是 Spring3.0 新添加的一个接口，负责将请求信息转换为一个对象（类型 T），将对象（类型为 T）输出为响应信息

**②**  **HttpMessageConverter<T>** 接口定义的方法：

* Boolean canRead(Class<?> clazz,MediaType mediaType): 指定转换器可以读取的对象类型，即转换器是否可将请求信息转换为 clazz 类型的对象，同时指定支持 MIME 类型(text/html,applaiction/json等)

* Boolean canWrite(Class<?> clazz,MediaType mediaType):指定转换器是否可将 clazz 类型的对象写到响应流中，响应流支持的媒体类型在MediaType 中定义。

* List<MediaType> getSupportMediaTypes()：该转换器支持的媒体类型。

* T read(Class<? extends T> clazz,**HttpInputMessage** inputMessage)：将请求信息流转换为 T 类型的对象。

* void write(T t,MediaType contnetType,**HttpOutputMessgae** outputMessage):将T类型的对象写到响应流中，同时指定相应的媒体类型为 contentType。



### 使用HttpMessageConverter

* **使用** **HttpMessageConverter<T>** **将请求信息转化并绑定到处理方法的入参中或将响应结果转为对应类型的响应信息，****Spring** **提供了两种途径**：
  * 使用 **@RequestBody / @ResponseBody** 对处理方法进行标注
  * 使用 **HttpEntity<T> / ResponseEntity<T>** 作为处理方法的入参或返回值
* 当控制器处理方法使用到 @RequestBody/@ResponseBody 或HttpEntity<T>/ResponseEntity<T> 时, Spring 首先根据请求头或响应头的 Accept 属性选择匹配的HttpMessageConverter, 进而根据参数类型或泛型类型的过滤得到匹配的 HttpMessageConverter 若找不到可用的 HttpMessageConverter 将报错
* **@RequestBody** **和** **@ResponseBody** **不需要成对出现**



## 国际化

### 页面中获取国际化资源信息

①  在页面上能够根据浏 览器语言设置的情况对文本, 时间, 数值进行本地化处理

②  可以在 bean 中获取国际化资源文件 Locale 对应的消息

③  可以通过超链接切换 Locale, 而不再依赖于浏览器的语言设置情况 

解决方法:

①  使用 JSTL 的 fmt 标签 

②  在 bean 中注入 ResourceBundleMessageSource 的实例, 使用其对应的 getMessage 方法即可

③  配置 LocalResolver 和 LocaleChangeInterceptor

### 控制器中获取国际化资源信息

①  屏蔽视图控制器，执行控制器中方法处理请求

```java
<!-- 测试国际化
<mvc:view-controller path="/testI18N" view-name="i18n"/>
 -->
<mvc:view-controller path="/testI18N2" view-name="i18n2"/>
```

②  控制器方法

```java
package com.atguigu.springmvc.crud.handlers;
 
import java.util.Locale;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.support.ResourceBundleMessageSource;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
 
@Controller
public class I18NHandler {
@Autowired
private ResourceBundleMessageSource messageSource; //从IOC容器中获取bean对象，进行依赖注入
 
@RequestMapping("/testI18N")
public String testI18N(Locale locale){
System.out.println(locale);
String user = messageSource.getMessage("i18n.user", null, locale);
System.out.println("i18n.user="+user);
return "i18n";
} 
}
```

### 通过超链接切换Locale

①  国际化概述

* 默认情况下，SpringMVC 根据 **Accept-Language** 参数判断客户端的本地化类型。

* 当接受到请求时，SpringMVC 会在上下文中查找一个本地化解析器（**LocalResolver**），找到后使用它获取请求所对应的本地化类型信息。

* SpringMVC 还允许装配一个**动态更改本地化类型的拦截器**，这样通过指定一个请求参数就可以控制单个请求的本地化类型。

* SessionLocaleResolver & LocaleChangeInterceptor 工作原理 

![BTrCNj.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151514.png)



②  本地化解析器和本地化拦截器

* **AcceptHeaderLocaleResolver**：**根据** **HTTP** **请求头的** **Accept-Language** **参数确定本地化类型**，如果没有显式定义本地化解析器， SpringMVC 使用该解析器。

* **CookieLocaleResolver**：根据指定的 Cookie 值确定本地化类型

* **SessionLocaleResolver**：根据 Session 中特定的属性确定本地化类型
* **LocaleChangeInterceptor**：从请求参数中获取本次请求对应的本地化类型。



## 文件上传

* Spring MVC 为文件上传提供了直接的支持，这种支持是通过即插即用的 **MultipartResolver** 实现的。 

* Spring 用 **Jakarta Commons FileUpload** 技术实现了一个 MultipartResolver 实现类：**CommonsMultipartResovler**  

* Spring MVC 上下文中默认没有装配 MultipartResovler，因此默认情况下不能处理文件的上传工作，如果想使用 Spring 的文件上传功能，需现在上下文中配置 MultipartResolver



## 拦截器

### 自定义拦截器

* Spring MVC也可以使用拦截器对请求进行拦截处理，用户可以自定义拦截器来实现特定的功能，**自定义的拦截器必须实现HandlerInterceptor接口**
  *  **preHandle**()：这个方法在业务处理器处理请求之前被调用，在该方法中对用户请求 request 进行处理。如果程序员决定该拦截器对请求进行拦截处理后还要调用其他的拦截器，或者是业务处理器去进行处理，则返回true；如果程序员决定不需要再调用其他的组件去处理请求，则返回false。
  * **postHandle**()：这个方法在业务处理器处理完请求后，但是DispatcherServlet 向客户端返回响应前被调用，在该方法中对用户请求request进行处理。
  *  **afterCompletion**()：这个方法在 DispatcherServlet 完全处理完请求后被调用，可以在该方法中进行一些资源清理的操作。



## 异常处理

Spring MVC 通过HandlerExceptionResolver 处理程序的异常，包括Handler映射、数据绑定以及目标方法执行时发生的异常



### HandlerExceptionResolver 



### 公共的处理异常的类@ControllerAdvice



### ExceptionHandlerExceptionResolver

* 主要处理 Handler 中用 **@ExceptionHandler** 注解定义的方法。

* @ExceptionHandler 注解定义的方法**优先级问题**：例如发生的是NullPointerException，但是声明的异常有 RuntimeException 和 Exception，此候会根据异常的最近继承关系找到继承深度最浅的那个 @ExceptionHandler 注解方法，即标记了 RuntimeException 的方法

* ExceptionHandlerMethodResolver 内部若找不到@ExceptionHandler 注解的话，会找 **@ControllerAdvice** 中的**@ExceptionHandler** 注解方法



### 异常处理_ResponseStatusExceptionResolver

* 在异常及异常父类中找到 **@ResponseStatus** 注解，然后使用这个注解的属性进行处理。

* 定义一个 **@ResponseStatus** 注解修饰的异常类

*  若在处理器方法中抛出了上述异常：若 **ExceptionHandlerExceptionResolver** 不解析上述异常。由于触发的异常**UnauthorizedException** 带有 **@ResponseStatus** 注解。因此会被 **ResponseStatusExceptionResolver** 解析到。最后响应**HttpStatus.UNAUTHORIZED** 代码给客户端。**HttpStatus.UNAUTHORIZED** 代表响应码401，无权限。 关于其他的响应码请参考 **HttpStatus** 枚举类型源码。



### 异常处理_DefaultHandlerExceptionResolver

* 对一些特殊的异常进行处理，比如：
  * NoSuchRequestHandlingMethodException、
  * **HttpRequestMethodNotSupportedException**、
  * HttpMediaTypeNotSupportedException、
  * HttpMediaTypeNotAcceptableException等



### 异常处理_SimpleMappingExceptionResolver

* 如果希望对所有异常进行统一处理，可以使用 SimpleMappingExceptionResolver，它将异常类名映射为视图名，即发生异常时使用对应的视图报告异常



## SpringMVC运行流程

### 流程图

![BTshwD.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151501.png)



### Spring工作流程描述

1)    用户向服务器发送请求，请求被SpringMVC 前端控制器 DispatcherServlet捕获；

2)    DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI）:

判断请求URI对应的映射

​		①   不存在：

​			a)    再判断是否配置了mvc:default-servlet-handler：

​			b)    如果没配置，则控制台报映射查找不到，客户端展示404错误

​			c)    如果有配置，则执行目标资源（一般为静态资源，如：JSP,HTML）

​		②   存在：

​			a)    执行下面流程

3)    根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain对象的形式返回；

4)    DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。

5)    如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(...)方法【正向】

6)    提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)方法，处理请求。在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：

​		①   HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息

​		②   数据转换：对请求消息进行数据转换。如String转换成Integer、Double等

​		③   数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等

​		④   数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

7)    Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象；

8)    此时将开始执行拦截器的postHandle(...)方法【逆向】

9)    根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行HandlerExceptionResolver进行异常处理）选择一个适合的ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet，根据Model和View，来渲染视图

10)  在返回给客户端时需要执行拦截器的AfterCompletion方法【逆向】

11)  将渲染结果返回给客户端



## Spring整合SpringMVC

### Spring、SpringMVC整合问题

①  需要进行 Spring 整合 SpringMVC 吗 ? 

②  还是否需要再加入 Spring 的 IOC 容器 ? 

③  是否需要在web.xml 文件中配置启动 Spring IOC 容器的 ContextLoaderListener ?

* **需要**:通常情况下, 类似于数据源, 事务, 整合其他框架都是放在 Spring 的配置文件中(而不是放在 SpringMVC 的配置文件中).

实际上放入 Spring 配置文件对应的 IOC 容器中的还有 Service 和 Dao. 

* **不需要**:都放在SpringMVC 的配置文件中.也可以分多个Spring的配置文件,然后使用 import 节点导入其他的配置文件 



### Spring整合SpringMVC：解决方案配置监听器

①  监听器配置

```java
<!-- 配置启动 Spring IOC 容器的 Listener -->
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:beans.xml</param-value>
</context-param>
<listener>
<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

②  创建Spring的bean的配置文件：beans.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
 
<!-- 设置扫描组件的包 -->
<context:component-scan base-package="com.atguigu.springmvc"/>
 
<!-- 配置数据源, 整合其他框架, 事务等. -->
 
</beans>
```

③  springmvc配置文件：springmvc.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
 
<!-- 设置扫描组件的包 -->
<context:component-scan base-package="com.atguigu.springmvc"/> 
<!-- 配置视图解析器 -->
<bean id="internalResourceViewResolver"
   class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/views/"/>
<property name="suffix" value=".jsp"/>
</bean>
 
<mvc:default-servlet-handler/> 
<mvc:annotation-driven/> 
</beans>
```

在HelloWorldHandler、UserService类中增加构造方法，启动服务器，查看构造器执行情况。

问题: 若 Spring 的 IOC 容器和 SpringMVC 的 IOC 容器扫描的包有重合的部分, 就会导致有的 bean 会被创建 2 次.

**解决**:

使 Spring 的 IOC 容器扫描的包和 SpringMVC 的 IOC 容器扫描的包没有重合的部分. 

使用 exclude-filter 和 include-filter 子节点来规定只能扫描的注解

采用②的方式解决：

| **springmvc.xml**                                            |
| ------------------------------------------------------------ |
| <context:component-scan  base-package="com.atguigu.springmvc"  use-default-filters="false">  <**context:include-filter**  type="annotation"         expression="org.springframework.stereotype.Controller"/>  <**context:include-filter**  type="annotation"         expression="org.springframework.web.bind.annotation.ControllerAdvice"/>  </context:component-scan> |

 

| **beans.xml**                                                |
| ------------------------------------------------------------ |
| <context:component-scan  base-package="com.atguigu.springmvc">  <**context:exclude-filter**  type="annotation"        expression="org.springframework.stereotype.Controller"/>  <**context:exclude-filter**  type="annotation"        expression="org.springframework.web.bind.annotation.ControllerAdvice"/>  </context:component-scan>  <!--  配置数据源, 整合其他框架, 事务等. --> |



### SpringIOC 容器和 SpringMVC IOC 容器的关系

SpringMVC 的 IOC 容器中的 bean 可以来引用 Spring IOC 容器中的 bean. 

返回来呢 ? 反之则不行. Spring IOC 容器中的 bean 却不能来引用 SpringMVC IOC 容器中的 bean 

* 在 Spring MVC 配置文件中引用业务层的 Bean

* 多个 Spring IOC 容器之间可以设置为父子关系，以实现良好的解耦。

* Spring MVC WEB 层容器可作为 “业务层” Spring 容器的子容器：

  即 WEB 层容器可以引用业务层容器的 Bean，而业务层容器却访问不到 WEB 层容器的 Bean



### Spring对比Structs2

①. Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter

②. Spring MVC 会稍微比 Struts2 快些. Spring MVC 是基于方法设计, 而 Sturts2 是基于类, 每次发一次请求都会实例一个 Action.

③. Spring MVC 使用更加简洁, 开发效率Spring MVC确实比 struts2 高: 支持 JSR303, 处理 ajax 的请求更方便

④. Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些. 