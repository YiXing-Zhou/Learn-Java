## 自动装配

除了使用 XML 和 Annotation 的方式装配 Bean 以外，还有一种常用的装配方式——自动装配。自动装配就是指 [Spring](http://c.biancheng.net/spring/) 容器可以自动装配（autowire）相互协作的 Bean 之间的关联关系，将一个 Bean 注入其他 Bean 的 Property 中。

要使用自动装配，就需要配置 <bean> 元素的 autowire 属性

## autowire 的 5 个属性值

​																					autowire 的属性和作用

| 名称        | 说明                                                         |
| ----------- | :----------------------------------------------------------- |
| byName      | 根据 Property 的 name 自动装配，**如果一个 Bean 的 name 和另一个 Bean 中的 Property 的 name 相同，则自动装配这个 Bean 到 Property 中。** |
| byType      | 根据 Property 的数据类型（Type）自动装配，**如果一个 Bean 的数据类型兼容另一个 Bean 中 Property 的数据类型，则自动装配。** |
| constructor | **根据构造方法的参数的数据类型，进行 byType 模式的自动装配。** |
| autodetect  | **如果发现默认的构造方法，则用 constructor 模式，否则用 byType 模式。** |
| no          | 默认情况下，不使用自动装配，Bean 依赖必须通过 ref 元素定义。 |



## 自动装配

​	Spring 的自动装配是根据 xml 文件来做的，并不是采用注解的方式，这样做的好处是可以直接进行自动装配，相互协作的 Bean 之间的关联关系，将一个 Bean 注入其他 Bean 的 Property 中，自动装配需要用到Set注入，所以，对应的对象一定要有Set方法进行注入。



```xml
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
  <bean id="personDao" class="com.mengma.annotation.PersonDaoImpl" />
  <!--通过配置autowire来根据名字进行自动装配-->
  <bean id="personService" class="com.mengma.annotation.PersonServiceImpl"
    autowire="byName" />
  <!--通过配置autowire来根据名字进行自动装配-->
  <bean id="personAction" class="com.mengma.annotation.PersonAction"
    autowire="byName" />
</beans>
```

