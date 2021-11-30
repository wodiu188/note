# Spring

在spring中所有的Bean都是由xml创建的



下面是配置Bean属性的xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

spring的pom.xml

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.3</version>
</dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
    <version>5.3.3</version>
</dependency>
<dependency>
```





## 第一种注入数据方式



### 通过set

配置文件在resource文件下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userOb" class="com.YH.pojo.User">
        <property name="name" value="YH"/>
        <property name="id" value="1234"/>
    </bean>

    <bean id="ob" class="com.YH.mapper.UserMapperImpl">
        <property name="user" ref="userOb"/>
        <property name="books">
            <array>
                <value>三国</value>
                <value>水浒</value>
                <value>红楼</value>
            </array>
        </property>
        <property name="hoddy">
            <list>
                <value>通个</value>
                <value>无敌</value>
                <value>驾驭</value>
            </list>
        </property>
        <property name="id">
            <value type="int" >123</value>
        </property>
        <property name="map">
            <map>
                <entry key="YH" value="123"/>
                <entry key="song" value="Yes"/>
            </map>
        </property>
        <property name="ob">
            <null/>
        </property>
        <property name="properties">
            <props>
                <prop key="driver">java.sql.DriverManager</prop>
                <prop key="url">http://www.baidu.com</prop>
            </props>
        </property>
        <property name="set">
            <set>
                <value>just do it</value>
            </set>
        </property>
    </bean>

</beans>
```



创建一个JavaBean类并设置Get和Set方法

```java
package com.YH.mapper;

import com.YH.pojo.User;
import lombok.Data;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

@Data
public class UserMapperImpl implements UserMapper{
    private User user;
    private int id;
    private String[] books;
    private List<String> hoddy;
    private Map<String,String> map;
    private Set<String> set;
    private Properties properties;
    private String ob;

}
```



配置数据时要注意不同类型要用不同的标签



### 通过构造函数

```xml

<bean id="userOb1" class="com.YH.pojo.User">
    <constructor-arg index="0" value="123"></constructor-arg>
    <constructor-arg index="1" value="123"></constructor-arg>
</bean>
<bean id="userOb2" class="com.YH.pojo.User">
    <constructor-arg type="java.lang.String" value="111"/>
    <constructor-arg type="int" value="123"/>
</bean>


<bean id="userOb3" class="com.YH.pojo.User">
    <constructor-arg name="id" value="12"/>
    <constructor-arg name="name" value="123"/>
</bean>

<bean id="userOb4" class="com.YH.pojo.User">
    <constructor-arg value="1"/>
    <constructor-arg value="2"/>

</bean>
```

上面是四种通过构造方式传入数据

第一个index指代下标

第二个通过类型传入数据

第三个通过变量名传入

第四个按照递增顺序传入





## 第二种注入方式

通过命名空间传入

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

第一条是属性命名空间

```xml
<bean id="beanOne" class="x.y.ThingOne" p:thingTwo-ref="beanTwo"
        p:thingThree-ref="beanThree" p:email="something@somewhere.com"/>

```

在标签中p:属性="value"即可



第二条是构造函数命名空间

```xml
    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
```

在标签中c:属性="value"即可

## 关于Bean的作用域

![image-20210129160828976](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20210129160828976.png)

这些作用域目前只需要了解前两种后面的是JavaWeb用的

比如

>```xml
><bean id="userOb3" class="com.YH.pojo.User" scope="prototype">
>```

这种会每次getBean都创建一个类

![image-20210129161435036](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20210129161435036.png)



>```xml
><bean id="userOb3" class="com.YH.pojo.User" scope="singleton">
>```

这种会每次getBean都是同一个类

![image-20210129161409100](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20210129161409100.png)



## 自动注解装配

```xml
<bean id="ddd" class="com.YH.pojo.Dog" p:name="zhj"/>
<bean id="ccc" class="com.YH.pojo.Cat" p:name="zzz"/>
<bean id="person1" class="com.YH.pojo.Person" autowire="byName" p:name="YH"/>
```

根据Person的set方法中属性名称进行查找

```java
package com.YH.pojo;


import lombok.Data;

@Data
public class Person {

    Dog ddd;
    Cat ccc;
    String name;
}
```



用@Data注释默认为类的小写



```xml
<bean id="dog" class="com.YH.pojo.Dog" p:name="zhj"/>
<bean id="cat" class="com.YH.pojo.Cat" p:name="zzz"/>
<bean id="person1" class="com.YH.pojo.Person" autowire="byType" p:name="YH"/>
```

根据类型进行查找.如果同一类型有多个则直接报错



使用@Autowired注解进行自动装配,指定字段或者类进行自动从容器中查找注册

@Qualifier来指定匹配字段的名称



@Autowired和@Qualifier的作用都可以用@Resource来实现

```java
@Data
public class Person {
    @Autowired
    @Qualifier(value = "c")
    Dog ddd;


    @Resource(name = "cc")
    Cat ccc;
    String name;
}
```

关于注解需要添加的依赖

```xml
xmlns:con="http://www.springframework.org/schema/context"
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd

```

需要添加这两个依赖添加后结果为

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p" xmlns:context="http://www.springframework.org/schema/tool"
       xmlns:con="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
```

还需要开启注解

```xml
<con:annotation-config/>
```

> 这种方式既可以通过ByName也可以通过ByType
>
> @Nullable被注解字段可以为空

接下来还可以实现自动组件生成

只需要在类中添加@Component即可然后在需要配置属性的字段中添加@Value或者在字段Set上添加@Value

不过需要开启包的扫描

```xml
<con:component-scan base-package="com.YH.pojo"/>
```

这样就可以扫描pojo包下面所有类，当遇到@Component直接类时就进行创建

如果你想指定作用域还可以给类添加

```java
@Scope(value = "")
```

### 导入bean

##### 方法1

> @Configuration
>
> 配置类注解将某一个类添加该注解后该类变成配置类
>
> @Bean
>
> 然后利用这个类可以生成Bean

```xml
<beans>
    <bean id = "pp" class="com.YH.pojo.PP"/>
</beans>
```

```java
@Configuration
public class ATe {
    public PP pp(){
        return new PP();
    }
}

//测试
    @org.junit.Test
    public void TTT(){
        ApplicationContext context = new AnnotationConfigApplicationContext(ATe.class);
        PP pp = context.getBean("pp", PP.class);
        System.out.println(pp);

    }
```

上面两种方式一样只不过读入的时候不一样

##### 方法2

如果在注解上添加@ComponentScan("包名")

```java
@Configuration
@ComponentScan("com.YH.pojo")
public class ATe {
}
```

生成的bean名称就是包的名字(大写不会成为小写)

> 这个时候必须给扫描的包下面添加@Component
>
> 效果和xml的<con:component-scan base-package="com.YH.pojo"/>作用一样







> 第一种方法bean类不需要添加@Component第二种必须添加



## AOP

AOP （Aspect Orient Programming）,直译过来就是 面向切面编程。AOP 是一种编程思想，是面向对象编程（OOP）的一种补充。面向对象编程将程序抽象成各个层次的对象，而面向切面编程是将程序抽象成各个切面。

当层次需要补充代码时可以利用aop不用修改以前的代码即可运作

首先AOP需要导入的依赖

```xml
<!-- ######################引入spiring AOP############################################# -->
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aop
已经被其它依赖引入：可以不配置
-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.7.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjrt
import org.aspectj.lang.annotation.Aspect
用于@Aspect
-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjrt</artifactId>
    <version>1.9.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver
Aspect的依赖包
-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.1</version>
</dependency>
```

### 方式一:

然后创建服务先行类和后行类





后行类

```java
package com.YH.service;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {
    
    
    //method代表使用的方法，args代表参数，target代表运行这个方法的实例，returnValue代表返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"执行了"+method.getName()+"方法并返回了"+returnValue);
    }
}
```





先行类

```java
package com.YH.service;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class BeforeLog implements MethodBeforeAdvice {

//method代表使用的方法，objects代表参数，o代表运行这个方法的实例
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println(o.getClass().getName()+"执行了"+method.getName()+"方法");
    }

}
```

可以看见这两给类都要实现一个aop下面的接口

编写beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

        <bean id="after" class="com.YH.service.AfterLog"/>
        <bean id="before" class="com.YH.service.BeforeLog"/>
        <bean id="service" class="com.YH.service.ServiceImpl"/>
    <aop:config>
        <aop:pointcut id="point" expression="execution(* com.YH.service.ServiceImpl.*(..))"/>
        <aop:advisor advice-ref="after" pointcut-ref="point"/>
        <aop:advisor advice-ref="before" pointcut-ref="point"/>
    </aop:config>

</beans>
```

需要添加关于aop的命名空间还有对应的网站,使用aop:config进行配置

aop:pointcut代表切点

aop:advisor代表切点的修饰(就是在切点之前要调用什么,切点之后调用什么)

### 方式二:

before和after和第一种不一样需要建立一个自定义类不需要继承

比如:

```java
package com.YH.service;

public class diy {
    public void before(){
        System.out.println("运行函数之前");

    }
    public void after(){
        System.out.println("运行之后");
    }
}
```



然后注册和上面不一样需要先<aop:aspect ref="diy">当然这个diy是一个bean

里面的after和before也不一样

```xml
<aop:config>
    <aop:aspect ref="diy">
        <aop:pointcut id="point" expression="execution(* com.YH.service.ServiceImpl.*(..))"/>
        <aop:after method="after" pointcut-ref="point"/>
        <aop:before method="before" pointcut-ref="point"/>
    </aop:aspect>
</aop:config>
```



### 方式三:

首先创建一个类并添加切面注释

```java
package com.YH.service;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect//将这个类作为一个切面类
public class PointCut {

    @Before("execution(* com.YH.service.ServiceImpl.*(..))")
    public void before(){
        System.out.println("前面啊啊啊啊啊啊这是");
    }


    @After("execution(* com.YH.service.ServiceImpl.*(..))")
    public void after(){
        System.out.println("后面");
    }

}
```

并编写before和after注释对应的函数



> 还有一个是@Around自己查吧



创建完这个类还需要注册,千万别忘了!!!

然后开启一下aop注释和autowired一样

```xml
<bean id="PointCut" class="com.YH.service.PointCut"/>
<aop:aspectj-autoproxy/>
```



## 整合mybatis-spring



平时使用mybatis需要一个mybatis-config.xml的文件但是我们可以整合到spring.xml中

```xml
<?xml version="1.0" encoding="UTF8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--    设置一个数据源 就相当于mybatis的environments-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;rewriteBatchedStatements=true"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

    <!--    摒弃了以前使用xml配合工具类创建sqlSessionFactory使用bean来创建-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--mybatis配置文件地址(可加可不加)-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!--申请mapper-->
        <property name="mapperLocations" value="classpath:com/YH/mapper/operaDB.xml"/>
    </bean>

    <!--    创建一个mapper-->
    <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <!--        只能使用接口不能使用实体类-->
        <property name="mapperInterface" value="com.YH.mapper.operaDB"/>
        <!--        这个参数需要传递一个SQL sessionFactory-->
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>

<!--   狂神使用的是SqlSessionTemplate官网使用的是上面那种方法-->
<!--    这种方式创建的是sqlSession,这样SqlSession就不是由spring来管理的-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
</beans>
```

当然为了显示的证明(处于新手阶段)要写出mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>


    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <typeAlias type="com.YH.pojo.User" alias="User"></typeAlias>
    </typeAliases>

</configuration>
```

实现的类

```java
package com.YH.pojo;

import lombok.Data;

@Data
public class User {
    private int id;
    private String name;
    private String password;
}
```



### 方式一

直接继承接口并且按照xml文件中userMapper的Bean来获取出来就可以直接使用只不过这种方式采用的是配置文件

```xml
<!--    创建一个mapper-->
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <!--        只能使用接口不能使用实体类-->
    <property name="mapperInterface" value="com.YH.mapper.operaDB"/>
    <!--        这个参数需要传递一个SQL sessionFactory-->
    <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
</bean>
```
很明显需要一个sqlSessionFactory还需要一个接口



### 方式二

```java
package com.YH.mapper;

import com.YH.pojo.User;

import java.util.List;

public class operaDBImpl implements operaDB{
    private final operaDB operaDB;

    operaDBImpl(operaDB operaDB){
        this.operaDB = operaDB;
    }
    public List<User> selectAllUser() {
        return operaDB.selectAllUser();
    }
}
```

这种方式需要传递进来一个operaDB的mapper

### 方式三

这种方法是直接继承SqlSessionDaoSupport这样spring-mybatis可以自己提供一个sqlSession然后获取一个mapper就能实现了

```java
package com.YH.mapper;

import com.YH.pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import java.util.List;

public class operaDBImpl2 extends SqlSessionDaoSupport implements operaDB {


    public List<User> selectAllUser() {
        SqlSession sqlSession = getSqlSession();
        return sqlSession.getMapper(operaDB.class).selectAllUser();
    }
}
```

```java
package com.YH.mapper;

import com.YH.pojo.User;

import java.util.List;

public interface operaDB {

    public List<User> selectAllUser();

}
```





### 其他注意点

> http://mybatis.org/spring/zh/getting-started.html

官网地址



> mapperLocations属性用来指示mapper的xml文件所在的文件路径
>
> 比如下面这种

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
  <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
</bean>
```

如果你使用了多个数据库，那么需要设置 `databaseIdProvider` 属性：

```xml
<bean id="databaseIdProvider" class="org.apache.ibatis.mapping.VendorDatabaseIdProvider">
  <property name="properties">
    <props>
      <prop key="SQL Server">sqlserver</prop>
      <prop key="DB2">db2</prop>
      <prop key="Oracle">oracle</prop>
      <prop key="MySQL">mysql</prop>
    </props>
  </property>
</bean>
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
  <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
  <property name="databaseIdProvider" ref="databaseIdProvider"/>
</bean>
```



在基础的 MyBatis 用法中，是通过 `SqlSessionFactoryBuilder` 来创建 `SqlSessionFactory` 的。而在 MyBatis-Spring 中，则使用 `SqlSessionFactoryBean` 来创建。



#### SqlSessionTemplate

SqlSessionTemplate它也负责将 MyBatis 的异常翻译成 Spring 中的 `DataAccessExceptions`。



## 事务

主要方式

```xml
<!--    开启并声明事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>

<!--    注册一个事务-->
    <tx:advice id="interceptor" transaction-manager="transactionManager">
        <tx:attributes>
<!--    关于propagation的参数详解        https://blog.csdn.net/sayoko06/article/details/79164858-->
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

<!--    用aop将事务加入类中-->
    <aop:config>
        <aop:pointcut id="txAOP" expression="execution(* com.YH.mapper.UserMapper.*(..))"/>
        <aop:advisor advice-ref="interceptor" pointcut-ref="txAOP"/>
    </aop:config>
```

这种方式是视频中讲的这种麻烦一点但是灵活

注意事务管理的是Bean中的某一个方法，比如我想插入两个数据同时成功插入成功或同时失败，那这两个插入函数要在Bean类下同一个方法中

```Java
public class test {

    @Test
    public void TestOne(){
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        UserMapper sqlSession = context.getBean("sqlSession", UserMapper.class);
        
        sqlSession.InsertUser(new User(9,"YY","kk"));
        sqlSession.DeleteUser(9);
        List<User> users = sqlSession.SelectAll();
        for (User user : users) {
            System.out.println(user);
        }
    }

}
```

就假如test这个类被注册了，那么里面的

> sqlSession.InsertUser(new User(9,"YY","kk"));
>
> sqlSession.DeleteUser(9);

就要在一个方法里面如果test没有注册事务而是直接运行则事务不起作用



关于其他开始管理事务的方式http://mybatis.org/spring/zh/transactions.html#programmatic



## 关于Java反射

```java
System.out.println(System.getProperty("java.class.path"));
```

获取类加载路径



```java
//        application 类加载器
        System.out.println(Me.class.getClassLoader());
//        根加载器（由于是c写的所以加载不到）
        System.out.println(System.class.getClassLoader());
//        扩展类加载器
        System.out.println(Me.class.getClassLoader().getParent());
```