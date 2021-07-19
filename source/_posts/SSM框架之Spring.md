---
title: SSM框架之Spring
date: 2020-10-25 00:00:00
tags: SSM框架
categories: 笔记
description: 一个开源的全栈应用程序框架和控制反转容器实现。
---

## Spring简介

①Spring是一个开源框架。

②Spring是一个**IOC**(DI)和**AOP**容器框架。

③Spring为简化企业级开发而生。



### Spring的优良特性

[1]**非侵入式**：基于Spring开发的应用中的对象可以不依赖于Spring的API

[2]**依赖注入**：DI——Dependency Injection，反转控制(IOC)最经典的实现。

[3]**面向切面编程**：Aspect Oriented Programming——AOP

[4]**容器**：Spring是一个容器，因为它包含并且管理应用对象的生命周期

[5]**组件化**：Spring实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。

[6]**一站式**：在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring 自身也提供了表述层的SpringMVC和持久层的Spring JDBC）。



### Spring模块

![BmlzGD.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151027.jpeg)



## IOC容器

### 关于IOC和DI

①IOC(**Inversion of Control**)：**反转控制**。

在应用程序中的组件需要获取资源时，传统的方式是组件主动的从容器中获取所需要的资源，在这样的模式下开发人员往往需要知道在具体容器中特定资源的获取方式，增加了学习成本，同时降低了开发效率。

反转控制的思想完全颠覆了应用程序组件获取资源的传统方式：反转了资源的获取方向——改由容器主动的将资源推送给需要的组件，开发人员不需要知道容器是如何创建资源对象的，只需要提供接收资源的方式即可，极大的降低了学习成本，提高了开发的效率。这种行为也称为查找的被动形式。



②DI(**Dependency Injection**)：**依赖注入**。

IOC的另一种表述方式：即组件以一些预先定义好的方式(例如：setter 方法)接受来自于容器的资源注入。相对于IOC而言，这种表述更直接。



③IOC容器在Spring中的实现

[1]在通过IOC容器读取Bean的实例之前，需要先将IOC容器本身实例化。

[2]Spring提供了IOC容器的两种实现方式

​		(1)BeanFactory：IOC容器的基本实现，是Spring内部的基础设施，是面向Spring本身的，不是提供给开发人员使用的。

​		(2)ApplicationContext：BeanFactory的子接口，提供了更多高级特性。面向Spring的使用者，几乎所有场合都使用ApplicationContext而不是底层的BeanFactory。



④ApplicationContext的主要实现类

[1]ClassPathXmlApplicationContext：对应类路径下的XML格式的配置文件

[2]FileSystemXmlApplicationContext：对应文件系统中的XML格式的配置文件

[3]在初始化时就创建单例的bean，也可以通过配置的方式指定创建的Bean是多实例的。

 

⑤ConfigurableApplicationContext

[1]是ApplicationContext的子接口，包含一些扩展方法

[2]refresh()和close()让ApplicationContext具有启动、关闭和刷新上下文的能力。

 

⑥WebApplicationContext

专门为WEB应用而准备的，它允许从相对于WEB根目录的路径中完成初始化工作



### 通过类型获取bean

从IOC容器中获取bean时，除了通过id值获取，还可以通过bean的类型获取。但如果同一个类型的bean在XML文件中配置了多个，则获取时会抛出异常，所以同一个类型的bean在容器中必须是唯一的。

```java
HelloWorld helloWorld = cxt.getBean(HelloWorld. class);
```



### 给bean的属性赋值

#### 赋值的途径

①通过bean的setxxx()方法赋值

②通过bean的构造器赋值

```java
	 <bean id="book" class="com.atguigu.spring.bean.Book" >
           <constructor-arg value= "10010"/>
           <constructor-arg value= "Book01"/>
           <constructor-arg value= "Author01"/>
           <constructor-arg value= "20.2"/>
	 </bean >
```

通过索引值指定参数位置

```java
     <bean id="book" class="com.atguigu.spring.bean.Book" >
           <constructor-arg value= "10010" index ="0"/>
           <constructor-arg value= "Book01" index ="1"/>
           <constructor-arg value= "Author01" index ="2"/>
           <constructor-arg value= "20.2" index ="3"/>
     </bean >
```

通过类型不同区分重载的构造器

```java
	 <bean id="book" class="com.atguigu.spring.bean.Book" >
      <constructor-arg value= "10010" index ="0" type="java.lang.Integer" />
      <constructor-arg value= "Book01" index ="1" type="java.lang.String" />
      <constructor-arg value= "Author01" index ="2" type="java.lang.String" />
      <constructor-arg value= "20.2" index ="3" type="java.lang.Double" />
	 </bean >
```



③给bean的级联属性赋值

```java
     <bean id="action" class="com.atguigu.spring.ref.Action">
          <property name="service" ref="service"/>
          <!-- 设置级联属性(了解) -->
          <property name="service.dao.dataSource" value="DBCP"/>
     </bean>
```



④p名称空间

为了简化XML文件的配置，越来越多的XML文件采用属性而非子元素配置信息。

Spring从2.5版本开始引入了一个新的p命名空间，可以通过<bean>元素属性的方式配置Bean的属性。

使用p命名空间后，基于XML的配置方式将进一步简化。

```java
<bean 
	id="studentSuper" 
	class="com.atguigu.helloworld.bean.Student"
	p:studentId="2002" p:stuName="Jerry2016" p:age="18" />
```



#### 可以使用的值

①字面量

[1]可以使用字符串表示的值，可以通过value属性或value子节点的方式指定

[2]基本数据类型及其封装类、String等类型都可以采取字面值注入的方式

[3]若字面值中包含特殊字符，可以使用<![CDATA[]]>把字面值包裹起来

②null值

③外部已声明的bean

```java
     <bean id="shop" class="com.atguigu.spring.bean.Shop" >
           <property name= "book" ref ="book"/>
     </bean >
```

④内部bean

当bean实例仅仅给一个特定的属性使用时，可以将其声明为内部bean。内部bean声明直接包含在<property>或<constructor-arg>元素里，不需要设置任何id或name属性。

内部bean不能使用在任何其他地方。

```java
<bean id="shop2" class="com.atguigu.spring.bean.Shop" >
    <property name= "book">
        <bean class= "com.atguigu.spring.bean.Book" >
           <property name= "bookId" value ="1000"/>
           <property name= "bookName" value="innerBook" />
           <property name= "author" value="innerAuthor" />
           <property name= "price" value ="50"/>
        </bean>
    </property>
</bean >
```



#### 集合属性

在Spring中可以通过一组内置的XML标签来配置集合属性，例如：<list>，<set>或<map>。

**①数组和List**

配置java.util.List类型的属性，需要指定<list>标签，在标签里包含一些元素。这些标签可以通过<value>指定简单的常量值，通过<ref>指定对其他Bean的引用。通过<bean>指定内置bean定义。通过<null/>指定空元素。甚至可以内嵌其他集合。

数组的定义和List一样，都使用<list>元素。

配置java.util.Set需要使用<set>标签，定义的方法与List一样。

```java
     <bean id="shop" class="com.atguigu.spring.bean.Shop" >
           <property name= "categoryList">
               <!-- 以字面量为值的List集合 -->
               <list>
                    <value> 历史</value >
                    <value> 军事</value >
               </list>
           </property>
           <property name= "bookList">
               <!-- 以bean的引用为值的List集合 -->
               <list>
                    <ref bean= "book01"/>
                    <ref bean= "book02"/>
               </list>
           </property>
     </bean >
```

**②Map**

Java.util.Map通过<map>标签定义，<map>标签里可以使用多个<entry>作为子标签。每个条目包含一个键和一个值。

必须在<key>标签里定义键。

因为键和值的类型没有限制，所以可以自由地为它们指定<value>、<ref>、<bean>或<null/>元素。

可以将Map的键和值作为<entry>的属性定义：简单常量使用key和value来定义；bean引用通过key-ref和value-ref属性定义。

```java
<bean id="cup" class="com.atguigu.spring.bean.Cup">
	<property name="bookMap">
		<map>
			<entry>
				<key>
					<value>bookKey01</value>
				</key>
				<ref bean="book01"/>
			</entry>
			<entry>
				<key>
					<value>bookKey02</value>
				</key>
				<ref bean="book02"/>
			</entry>
		</map>
	</property>
</bean>
```

**③Properties**

使用<props>定义java.util.Properties，该标签使用多个<prop>作为子标签。每个<prop>标签必须定义key属性

```java
<bean class="com.atguigu.spring.bean.DataSource" id="dataSource">
	<property name="properties">
		<props>
			<prop key="userName">root</prop>
			<prop key="password">root</prop>
			<prop key="url">jdbc:mysql:///test</prop>
			<prop key="driverClass">com.mysql.jdbc.Driver</prop>
		</props>
	</property>
</bean>
<bean class="com.atguigu.spring.bean.DataSource" id="dataSource">
	<property name="properties">
		<props>
			<prop key="userName">root</prop>
			<prop key="password">root</prop>
			<prop key="url">jdbc:mysql:///test</prop>
			<prop key="driverClass">com.mysql.jdbc.Driver</prop>
		</props>
	</property>
</bean>
```

**④集合类型的bean**

如果只能将集合对象配置在某个bean内部，则这个集合的配置将不能重用。我们需要将集合bean的配置拿到外面，供其他bean引用。

配置集合类型的bean需要引入util名称空间

```java
<util:list id="bookList">
	<ref bean="book01"/>
	<ref bean="book02"/>
	<ref bean="book03"/>
	<ref bean="book04"/>
	<ref bean="book05"/>
</util:list>

<util:list id="categoryList">
	<value>编程</value>
	<value>极客</value>
	<value>相声</value>
	<value>评书</value>
</util:list>
```



### 通过工厂创建bean

#### 静态工厂

调用静态工厂方法创建bean是将对象创建的过程封装到静态方法中。当客户端需要对象时，只需要简单地调用静态方法，而不用关心创建对象的细节。

声明通过静态方法创建的bean需要在bean的class属性里指定静态工厂类的全类名，同时在factory-method属性里指定工厂方法的名称。最后使用<constrctor-arg>元素为该方法传递方法参数。

#### 实例工厂

实例工厂方法：将对象的创建过程封装到另外一个对象实例的方法里。当客户端需要请求对象时，只需要简单的调用该实例方法而不需要关心对象的创建细节。

实现方式

**①配置工厂类实例的bean**

**②在factory-method属性里指定该工厂方法的名称**

**③使用 construtor-arg 元素为工厂方法传递方法参数**



#### FactoryBean

Spring中有两种类型的bean，一种是普通bean，另一种是工厂bean，即FactoryBean。

工厂bean跟普通bean不同，其返回的对象不是指定类的一个实例，其返回的是该工厂bean的getObject方法所返回的对象。

工厂bean必须实现***org.springframework.beans.factory.FactoryBean***接口。

```java
<bean id="product" class="com.atguigu.spring.bean.ProductFactory">
	<property name="productName" value="Mp3" />
</bean>
```

### bean的高级配置

#### 配置信息的继承

**①背景**

查看下面两个Employee的配置，其中dept属性是重复的。

```java
<bean id="dept" class="com.atguigu.parent.bean.Department">
	<property name="deptId" value="100"/>
	<property name="deptName" value="IT"/>
</bean>

<bean id="emp01" class="com.atguigu.parent.bean.Employee">
	<property name="empId" value="1001"/>
	<property name="empName" value="Tom"/>
	<property name="age" value="20"/>

	<!-- 重复的属性值 -->	
	<property name="detp" ref="dept"/>
</bean>

<bean id="emp02" class="com.atguigu.parent.bean.Employee">
	<property name="empId" value="1002"/>
	<property name="empName" value="Jerry"/>
	<property name="age" value="25"/>

	<!-- 重复的属性值 -->
	<property name="detp" ref="dept"/>
</bean>
```

**②配置信息的继承**

```java
<!-- 以emp01作为父bean，继承后可以省略公共属性值的配置 -->
<bean id="emp02" parent="emp01">
	<property name="empId" value="1002"/>
	<property name="empName" value="Jerry"/>
	<property name="age" value="25"/>
</bean>
```

Spring允许继承bean的配置，被继承的bean称为父bean。继承这个父bean的bean称为子bean

子bean从父bean中继承配置，包括bean的属性配置

子bean也可以覆盖从父bean继承过来的配置

**③补充说明**

父bean可以作为配置模板，也可以作为bean实例。若只想把父bean作为模板，可以设置<bean>的abstract 属性为true，这样Spring将不会实例化这个bean

如果一个bean的class属性没有指定，则必须是抽象bean

并不是<bean>元素里的所有属性都会被继承。比如：autowire，abstract等。

也可以忽略父bean的class属性，让子bean指定自己的类，而共享相同的属性配置。但此时abstract必须设为true。

#### bean之间的依赖

有的时候创建一个bean的时候需要保证另外一个bean也被创建，这时我们称前面的bean对后面的bean有依赖。例如：要求创建Employee对象的时候必须创建Department。这里需要注意的是依赖关系不等于引用关系，Employee即使依赖Department也可以不引用它。

```java
<bean id="emp03" class="com.atguigu.parent.bean.Employee" depends-on="dept">
	<property name="empId" value="1003"/>
	<property name="empName" value="Kate"/>
	<property name="age" value="21"/>
</bean>
```

#### bean的作用域

在Spring中，可以在<bean>元素的scope属性里设置bean的作用域，以决定这个bean是单实例的还是多实例的。

默认情况下，Spring只为每个在IOC容器里声明的bean创建唯一一个实例，整个IOC容器范围内都能共享该实例：所有后续的getBean()调用和bean引用都将返回这个唯一的bean实例。该作用域被称为singleton，它是所有bean的默认作用域。







当bean的作用域为单例时，Spring会在IOC容器对象创建时就创建bean的对象实例。而当bean的作用域为prototype时，IOC容器在获取bean的实例时创建bean的实例对象。

#### bean的生命周期

①Spring IOC容器可以管理bean的生命周期，Spring允许在bean生命周期内特定的时间点执行指定的任务。

②Spring IOC容器对bean的生命周期进行管理的过程：

​		[1]通过构造器或工厂方法创建bean实例

​		[2]为bean的属性设置值和对其他bean的引用

​		[3]调用bean的初始化方法

​		[4]bean可以使用了

​		[5]当容器关闭时，调用bean的销毁方法

③在配置bean时，通过init-method和destroy-method 属性为bean指定初始化和销毁方法

④bean的后置处理器

​		[1]bean后置处理器允许在调用**初始化方法前后**对bean进行额外的处理

​		[2]bean后置处理器对IOC容器里的所有bean实例逐一处理，而非单一实例。其典型应用是：检查bean属性的正确性或根据特定的标准更改bean的属性。

​		[3] bean后置处理器时需要实现接口：org.springframework.beans.factory.config.BeanPostProcessor。在初始化方法被调用前后，Spring将把每个bean实例分别传递给上述接口的以下两个方法：

​		●postProcessBeforeInitialization(Object, String)

​		●postProcessAfterInitialization(Object, String)

⑤添加bean后置处理器后bean的生命周期

​		[1]通过构造器或工厂方法**创建****bean****实例**

​		[2]为bean的**属性设置值**和对其他bean的引用

​		[3]将bean实例传递给bean后置处理器的**postProcessBeforeInitialization()**方法

​		[4]调用bean的**初始化**方法

​		[5]将bean实例传递给bean后置处理器的**postProcessAfterInitialization()**方法

​		[6]bean可以使用了

​		[7]当容器关闭时调用bean的**销毁方法**

#### 引用外部属性文件

当bean的配置信息逐渐增多时，查找和修改一些bean的配置信息就变得愈加困难。这时可以将一部分信息提取到bean配置文件的外部，以properties格式的属性文件保存起来，同时在bean的配置文件中引用properties属性文件中的内容，从而实现一部分属性值在发生变化时仅修改properties属性文件即可。这种技术多用于连接数据库的基本信息的配置。

①直接配置

```java
<!-- 直接配置 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="user" value="root"/>
	<property name="password" value="root"/>
	<property name="jdbcUrl" value="jdbc:mysql:///test"/>
	<property name="driverClass" value="com.mysql.jdbc.Driver"/>
</bean>
```

②创建properties属性文件

```java
prop.userName=root
prop.password=root
prop.url=jdbc:mysql:///test
prop.driverClass=com.mysql.jdbc.Driver
```

③引入context名称空间

④指定properties属性文件的位置

```java
<!-- 指定properties属性文件的位置 -->
<!-- classpath:xxx 表示属性文件位于类路径下 -->
<context:property-placeholder location="classpath:jdbc.properties"/>
```

⑤从properties属性文件中引入属性值

```java
<!-- 从properties属性文件中引入属性值 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="user" value="${prop.userName}"/>
	<property name="password" value="${prop.password}"/>
	<property name="jdbcUrl" value="${prop.url}"/>
	<property name="driverClass" value="${prop.driverClass}"/>
</bean>
```

#### 自动装配

**①自动装配的概念**

[1]手动装配：以value或ref的方式**明确指定属性值**都是手动装配。

[2]自动装配：根据指定的装配规则，**不需要明确指定**，Spring**自动**将匹配的属性值**注入**bean中。

**②装配模式**

[1]根据**类型**自动装配：将类型匹配的bean作为属性注入到另一个bean中。若IOC容器中有多个与目标bean类型一致的bean，Spring将无法判定哪个bean最合适该属性，所以不能执行自动装配

[2]根据**名称**自动装配：必须将目标bean的名称和属性名设置的完全相同

[3]通过构造器自动装配：当bean中存在多个构造器时，此种自动装配方式将会很复杂。不推荐使用。

**③选用建议**

相对于使用注解的方式实现的自动装配，在XML文档中进行的自动装配略显笨拙，在项目中更多的使用注解的方式实现。

### 通过注解配置bean

相对于XML方式而言，通过注解的方式配置bean更加简洁和优雅，而且和MVC组件化开发的理念十分契合，是开发中常用的使用方式。

#### 使用注解标识组件

①普通组件：@Component

标识一个受Spring IOC容器管理的组件

②持久化层组件：@Respository

标识一个受Spring IOC容器管理的持久化层组件

③业务逻辑层组件：@Service

标识一个受Spring IOC容器管理的业务逻辑层组件

④表述层控制器组件：@Controller

标识一个受Spring IOC容器管理的表述层控制器组件

⑤组件命名规则

​		[1]默认情况：使用组件的简单类名首字母小写后得到的字符串作为bean的id

​		[2]使用组件注解的value属性指定bean的id

 

注意：事实上Spring并没有能力识别一个组件到底是不是它所标记的类型，即使将@Respository注解用在一个表述层控制器组件上面也不会产生任何错误，所以@Respository、@Service、@Controller这几个注解仅仅是为了让开发人员自己明确当前的组件扮演的角色。

#### 扫描组件

组件被上述注解标识后还需要通过Spring进行扫描才能够侦测到。

**①指定被扫描的package**

```java
<context:component-scan base-package="com.atguigu.component"/>
```

**②详细说明**

[1]**base-package**属性指定一个需要扫描的基类包，Spring容器将会扫描这个基类包及其子包中的所有类。

[2]当需要扫描多个包时可以使用逗号分隔。

[3]如果仅希望扫描特定的类而非基包下的所有类，可使用resource-pattern属性过滤特定的类，示例：

```java
<context:component-scan 
	base-package="com.atguigu.component" 
	resource-pattern="autowire/*.class"/>
```

[4]包含与排除

●<context:include-filter>子节点表示要包含的目标类

注意：通常需要与use-default-filters属性配合使用才能够达到“仅包含某些组件”这样的效果。即：通过将use-default-filters属性设置为false，禁用默认过滤器，然后扫描的就只是include-filter中的规则指定的组件了。

●<context:exclude-filter>子节点表示要排除在外的目标类

●component-scan下可以拥有若干个include-filter和exclude-filter子节点

**③JAR包**

必须在原有JAR包组合的基础上再导入一个：spring-aop-4.0.0.RELEASE.jar

#### 组件装配

**①需求**

Controller组件中往往需要用到Service组件的实例，Service组件中往往需要用到Repository组件的实例。Spring可以通过注解的方式帮我们实现属性的装配。

**②实现依据**

在指定要扫描的包时，<context:component-scan> 元素会自动注册一个bean的后置处理器：AutowiredAnnotationBeanPostProcessor的实例。该后置处理器可以自动装配标记了**@Autowired**、@Resource或@Inject注解的属性。

**③@Autowired注解**

[1]根据类型实现自动装配。

[2]构造器、普通字段(即使是非public)、一切具有参数的方法都可以应用@Autowired注解

[3]默认情况下，所有使用@Autowired注解的属性都需要被设置。当Spring找不到匹配的bean装配属性时，会抛出异常。

[4]若某一属性允许不被设置，可以设置@Autowired注解的required属性为 false

[5]默认情况下，当IOC容器里存在多个类型兼容的bean时，Spring会尝试匹配bean的id值是否与变量名相同，如果相同则进行装配。如果bean的id值不相同，通过类型的自动装配将无法工作。此时可以在@Qualifier注解里提供bean的名称。Spring甚至允许在方法的形参上标注@Qualifiter注解以指定注入bean的名称。

[6]@Autowired注解也可以应用在数组类型的属性上，此时Spring将会把所有匹配的bean进行自动装配。

[7]@Autowired注解也可以应用在集合属性上，此时Spring读取该集合的类型信息，然后自动装配所有与之兼容的bean。

[8]@Autowired注解用在java.util.Map上时，若该Map的键值为String，那么 Spring将自动装配与值类型兼容的bean作为值，并以bean的id值作为键。

**④@Resource**

@Resource注解要求提供一个bean名称的属性，若该属性为空，则自动采用标注处的变量或方法名作为bean的名称。

**⑤@Inject**

@Inject和@Autowired注解一样也是按类型注入匹配的bean，但没有reqired属性。

### 泛型依赖注入

可以为子类注入子类对应的泛型类型的成员变量的引用。

#### 实现

[1]组件基类

```java
BaseRepository
public class BaseRepository<T> {
	
	public void save() {
		System.out.println("Saved by BaseRepository");
	}

}
```

```java
BaseService
public class BaseService<T> {
	
	@Autowired
	private BaseRepository<T> repository;
	
	public void add() {
		repository.save();
	}

}
```

[2]组件实体类

```java
UserRepository
@Repository
public class UserRepository extends BaseRepository<User>{
	
	public void save() {
		System.out.println("Saved by UserRepository");
	}

}
```

```java
UserService
@Service
public class UserService extends BaseService<User>{

}
```

[3]模型实体类

```java
User
public class User {
	
}
```

[4]测试

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("di.xml");

UserService us = (UserService) ioc.getBean("userService");

us.add();
```

```java
执行结果
Saved by UserRepository
```

## AOP

### 概述

●AOP(Aspect-Oriented Programming，**面向切面编程**)：是一种新的方法论，是对传统 OOP(Object-Oriented Programming，面向对象编程)的补充。

●AOP编程操作的主要对象是切面(aspect)，而切面**模块化横切关注点**。

●在应用AOP编程时，仍然需要定义公共功能，但可以明确的定义这个功能应用在哪里，以什么方式应用，并且不必修改受影响的类。这样一来横切关注点就被模块化到特殊的类里——这样的类我们通常称之为“切面”。

●AOP的好处：

​		○每个事物逻辑位于一个位置，代码不分散，便于维护和升级

​		○业务模块更简洁，只包含核心业务代码

### 相关术语

***横切关注点***

从每个方法中抽取出来的同一类非核心业务。

***切面(Aspect)***

封装横切关注点信息的类，每个关注点体现为一个通知方法。

***通知(Advice)***

切面必须要完成的各个具体工作

***目标(Target)***

被通知的对象

***代理(Proxy)***

向目标对象应用通知之后创建的代理对象

***连接点(Joinpoint)***

横切关注点在程序代码中的具体体现，对应程序执行的某个特定位置。例如：类某个方法调用前、调用后、方法捕获到异常后等。

在应用程序中可以使用横纵两个坐标来定位一个具体的连接点

***切入点(pointcut)***

定位连接点的方式。每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物。如果把连接点看作数据库中的记录，那么切入点就是查询条件——AOP可以通过切入点定位到特定的连接点。切点通过org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条件

### 相关细节

#### 切入点表达式

通过**表达式的方式**定位**一个或多个**具体的连接点。

**①切入点表达式的语法格式**

  execution([权限修饰符] [返回值类型] [简单类名/全类名] [方法名]([参数列表]))  

**②举例说明**

| 表达式 | execution(***** com.atguigu.spring.ArithmeticCalculator.*****(**..**)) |
| ------ | ------------------------------------------------------------ |
| 含义   | ArithmeticCalculator接口中声明的所有方法。  第一个“*”代表任意修饰符及任意返回值。  第二个“*”代表任意方法。  “..”匹配任意数量、任意类型的参数。  若目标类、接口与该切面类在同一个包中可以省略包名。 |

 

| 表达式 | execution(**public** * ArithmeticCalculator.*(..)) |
| ------ | -------------------------------------------------- |
| 含义   | ArithmeticCalculator接口的所有公有方法             |

 

| 表达式 | execution(public **double** ArithmeticCalculator.*(..)) |
| ------ | ------------------------------------------------------- |
| 含义   | ArithmeticCalculator接口中返回double类型数值的方法      |

 

| 表达式 | execution(public double ArithmeticCalculator.*(**double**,  ..)) |
| ------ | ------------------------------------------------------------ |
| 含义   | 第一个参数为double类型的方法。  “..” 匹配任意数量、任意类型的参数。 |

 

| 表达式 | execution(public double ArithmeticCalculator.*(**double**,  **double**)) |
| ------ | ------------------------------------------------------------ |
| 含义   | 参数类型为double，double类型的方法                           |

 

**③在AspectJ中，切入点表达式可以通过 “&&”、“||”、“!”等操作符结合起来。**

| 表达式 | execution (* *.add(int,..)) **\|\|**  execution(* *.sub(int,..)) |
| ------ | :----------------------------------------------------------- |
| 含义   | 任意类中第一个参数为int类型的add方法或sub方法                |

**实际应用**

![Bw8iWt.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202151626.png)

### 当前连接点

切入点表达式通常都会是从宏观上定位一组方法，和具体某个通知的注解结合起来就能够确定对应的连接点。那么就一个具体的连接点而言，我们可能会关心这个连接点的一些具体信息，例如：当前连接点所在方法的方法名、当前传入的参数值等等。这些信息都封装在JoinPoint接口的实例对象中

### 通知

在具体的连接点上要执行的操作。

一个切面可以包括一个或者多个通知。

通知所使用的注解的值往往是切入点表达式。

#### 前置通知

* 前置通知：在方法执行之前执行的通知

* 使用***@Before***注解

#### 后置通知

* 后置通知：后置通知是在连接点完成之后执行的，即连接点返回结果或者抛出异常的时候

* 使用***@After***注解

#### 返回通知

* 返回通知：无论连接点是正常返回还是抛出异常，后置通知都会执行。如果只想在连接点返回的时候记录日志，应使用返回通知代替后置通知。

* 使用***@AfterReturning***注解

* 在返回通知中访问连接点的返回值
  * 在返回通知中，只要将returning属性添加到@AfterReturning注解中，就可以访问连接点的返回值。该属性的值即为用来传入返回值的参数名称
  * 必须在通知方法的签名中添加一个同名参数。在运行时Spring AOP会通过这个参数传递返回值、
  * 原始的切点表达式需要出现在pointcut属性中                              

#### 异常通知

* 异常通知：只在连接点抛出异常时才执行异常通知

* 将throwing属性添加到@AfterThrowing注解中，也可以访问连接点抛出的异常。Throwable是所有错误和异常类的顶级父类，所以在异常通知方法可以捕获到任何错误和异常。

* 如果只对某种特殊的异常类型感兴趣，可以将参数声明为其他异常的参数类型。然后通知就只在抛出这个类型及其子类的异常时才被执行

#### 环绕通知

* 环绕通知是所有通知类型中功能最为强大的，能够全面地控制连接点，甚至可以控制是否执行连接点。

* 对于环绕通知来说，连接点的参数类型必须是ProceedingJoinPoint。它是 JoinPoint的子接口，允许控制何时执行，是否执行连接点。

* 在环绕通知中需要明确调用ProceedingJoinPoint的proceed()方法来执行被代理的方法。如果忘记这样做就会导致通知被执行了，但目标方法没有被执行。

* 注意：环绕通知的方法需要返回目标方法执行之后的结果，即调用 joinPoint.proceed();的返回值，否则会出现空指针异常。