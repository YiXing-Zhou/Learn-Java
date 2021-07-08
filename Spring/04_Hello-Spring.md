## 创建Maven项目

​	使用idea创建一个maven项目，不用选择任何的包，直接创建即可,创建完成的项目结构如下。

![image-20210624135610833](/Users/zhouyixing/Library/Application Support/typora-user-images/image-20210624135610833.png)

## pom 当中引入 spring 对应的包

1. 需要导入的是 Springframework-webmvc的包，这个包会帮忙引入他所需要的依赖，也就是Spring相关的核心包等等。

   ```java
   // 写在 pom 当中进行配置，记得需要标签  <dependencies> 
   <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.0.RELEASE</version>
     </dependency>
   ```

2. 在maven当中创建目录com.kuang.pojo

   ```java
   // pojo 目录中创建一个Hello类,这个类一定要有Set方法，这个方法是Spring用来依赖注入的。
   package com.kuang.pojo;
   
   public class Hello {
     private String str;
   
     public String getStr() {
       return str;
     }
   
     public void setStr(String str) {
       this.str = str;
     }
   
     @Override
     public String toString() {
       return "Hello{" +
           "str='" + str + '\'' +
           '}';
     }
   }
   
   ```

3. resources 目录中创建 xml 文件

   ```java
   // Spring 推荐的 xml 命名方式为，applicationContext.xml 这里直接命名为 beans.xml
   // 在 xml 文件当中
   // property 可以给类中的属性注入值。
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
   	// 这样可以让 Hello 成功被 Spring 托管，当这个类成功被Spring托管以后，就可以看到这个类里边就可以看到响应的图标。
     <bean id="hello" class="com.kuang.pojo.Hello">
       // value 只能传入基本数据类型，ref 可以传入已经被Spring托管了的类的名字，也就是说已经在xml文件当中创建了的对象。
       <property name="str" value="Spring"/>
     </bean>
   
   
   </beans>
   ```

4. 创建测试，就直接可以在test目录里边创建一个

   ``` java
   import com.kuang.pojo.Hello;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
   
     public static void main(String[] args) {
       // 获取Spring的上下文对象
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
       // 通过上下文对象，获取Bean，获取到被Bean托管的 hello,这个 hello 就是 xml 里边注册的 name ,通过这个 name 就可以拿到这个对象，所以，这个对象就是一个 Hello 类型的类，可以进行强制转换，在Hello类调用的时候，就可以直接用 Set 进行依赖注入，这样就可以愉快的使用了。
       Hello hello = (Hello) context.getBean("hello");
       // 通过依赖注入的方式来实现 IOC 控制反转的思想。
       hello.setStr("周一星");
       System.out.println(hello.toString());
     }
   }
   
   // 运行一下这个测试方法，就可以的到将 Str 注入的实例当中，并且添加给这个类的属性。这是通过 Hello 对象的 tostring 实现的。
   Hello{str='周一星'}
   
   ```

   

