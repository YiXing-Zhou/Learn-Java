## Spring 基于 Annotation 装配 Bean

在 [Spring](http://c.biancheng.net/spring/) 中，尽管使用 XML 配置文件可以实现 Bean 的装配工作，但如果应用中 Bean 的数量较多，会导致 XML 配置文件过于臃肿，从而给维护和升级带来一定的困难。

[Java](http://c.biancheng.net/java/) 从 JDK 5.0 以后，提供了 Annotation（注解）功能，Spring 也提供了对 Annotation 技术的全面支持。Spring3 中定义了一系列的 Annotation（注解），常用的注解如下。



## 注解

- @Component

  可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。

- @Respository

  用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

- @Service

  通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

- @Controller

  通常作用在控制层（如 [Struts2](http://c.biancheng.net/struts2/) 的 Action），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

- @Autowired

  用于对 Bean 的属性变量、属性的 Set 方法及构造函数进行标注，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配。

- @Resource

  其作用与 Autowired 一样。其区别在于 @Autowired 默认按照 Bean 类型装配，而 @Resource 默认按照 Bean 实例名称进行装配。

  

  @Resource 中有两个重要属性：name 和 type。

  

  Spring 将 name 属性解析为 Bean 实例名称，type 属性解析为 Bean 实例类型。如果指定 name 属性，则按实例名称进行装配；如果指定 type 属性，则按 Bean 类型进行装配。

  

  如果都不指定，则先按 Bean 实例名称装配，如果不能匹配，则再按照 Bean 类型进行装配；如果都无法匹配，则抛出 NoSuchBeanDefinitionException 异常。

- @Qualifier

  与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定。



## 使用注解注册Bean当Spring的简单demo

​	该 Demo 才用了 Dao + service + Controller 三层进行编写，并且使用了Spring注解的方式来进行Bean 的注册，主要实现了一个用户插入功能

### 1. PersonDao 接口

```java
package com.mengma.annotation;

public interface PersonDao {
  public void add();
}

```

### 2. PersonDaoimpl

```java
package com.mengma.annotation;

import org.springframework.stereotype.Repository;

// @Respository 用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean
@Repository("personDao")
public class PersonDaoImpl implements PersonDao {

  @Override
  public void add() {
    System.out.println("Dao层的add方法执行成功");
  }
}

```

### 3. PersonService 接口

```java
package com.mengma.annotation;


public interface PersonService {
  public void add();
}

```

### 4. PersonServiceImpl 实现类

```java
package com.mengma.annotation;

import javax.annotation.Resource;
import org.springframework.stereotype.Service;

// @Service  通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean
@Service("personService")
public class PersonServiceImpl implements PersonService {
	// @Resource 是用来在Bean对象中调用其他对象的，类似于xml配置， <property name="personService"ref="personService"/>
  // 可以看到这个就注入了一个PersonDao对象进来，这样才能保证正常的使用
  @Resource(name = "personDao")
  private PersonDao personDao;

  public PersonDao getPersonDao() {
    return personDao;
  }

  @Override
  public void add() {
    // 调用dao层的add方法
    personDao.add();
    System.out.println("Service层的add方法执行成功");
  }
}

```

### 5.PersonAction类（这个类可以看做Controller）

```java
package com.mengma.annotation;

import javax.annotation.Resource;
import org.springframework.stereotype.Controller;

//@Controller 通常作用在控制层（如 [Struts2](http://c.biancheng.net/struts2/) 的 Action），用于将控制层的类标识为 Spring 中的 Bean
// 将这个类注册到 Bean当中，且这个类中的方法调用了Service层的Add方法来进行Add的操作。
@Controller("personAction")
public class PersonAction {
  @Resource(name = "personService")
  private PersonService personService;

  public PersonService getPersonService() {
    return personService;
  }

  public void add(){
    personService.add();
    System.out.println("Action层的add方法执行了。");
  }
}

```



### 6. 测试类

```java
// 写一个测试类用来调用controller层的类进行操作
import com.mengma.annotation.PersonAction;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Mytest {

  public static void main(String[] args) {
    String xmlPath = "applicationContext.xml";
    // 在 Xml 文件当中获取到application的上下文对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
    // 通过上下文对象获取当响应的Bean，也就是Controller层的类对象，并且调用他的add方法来进行操作，
    PersonAction personAction = (PersonAction) applicationContext.getBean("personAction");
    personAction.add();
  }


}

```



### 7. xml 当中配置Spring扫描类通过注解的绑定

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:p="http://www.springframework.org/schema/p"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
            http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx-2.5.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
  <!--使用context命名空间，通知spring扫描指定目录，进行注解的解析-->
  <context:component-scan base-package="com.mengma.annotation"/>
</beans>
```

