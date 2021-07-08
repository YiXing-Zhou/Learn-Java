## JDK 动态代理

JDK 动态代理是通过 JDK 中的 java.lang.reflect.Proxy 类实现的

### 1. 创建接口 CustomerDao

```java
package com.mengma.dao;
public interface CustomerDao {
    public void add(); // 添加
    public void update(); // 修改
    public void delete(); // 删除
    public void find(); // 查询
}
```

### 2. 实现这个接口 CustomerDaoImpl

```java
package com.mengma.dao;
public class CustomerDaoImpl implements CustomerDao {
    @Override
    public void add() {
        System.out.println("添加客户...");
    }
    @Override
    public void update() {
        System.out.println("修改客户...");
    }
    @Override
    public void delete() {
        System.out.println("删除客户...");
    }
    @Override
    public void find() {
        System.out.println("修改客户...");
    }
}
```

### 3. 创建一个切面类 Myspect

```java
package com.mengma.jdk;

public class Myspect {

  public void myBefore() {
    System.out.println("方法执行之前");
  }

  public void mtAfter() {
    System.out.println("方法执行之后");
  }
}

```

### 4. 创建代理类 MyBeanFactory

```java
package com.mengma.jdk;

import com.mengma.dao.CustomerDao;
import com.mengma.dao.CustomerDaoImpl;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MyBeanFactory {

  public static CustomerDao getBean() {
    // 准备目标类
    final CustomerDao customerDao = new CustomerDaoImpl();
    // 创建切面实例
    final Myspect myspect = new Myspect();
    // 使用代理类，进行增强
    return (CustomerDao) Proxy.newProxyInstance(
        MyBeanFactory.class.getClassLoader(),
        new Class[]{CustomerDao.class},
        new InvocationHandler() {
          public Object invoke(Object proxy, Method method,Object[] args) throws Throwable {
            myspect.myBefore(); // 前增强
            Object obj = method.invoke(customerDao, args);
            myspect.mtAfter(); // 后增强
            return obj;
          }
        });
  }
}

```

​	使用代理类对创建的实例 customerDao 中的方法进行增强的代码，其中 Proxy 的 newProxyInstance() 方法的第一个参数是当前类的类加载器，第二参数是所创建实例的实现类的接口，第三个参数就是需要增强的方法

### 5. 创建测试类

```java
package com.mengma.jdk;
import org.junit.Test;
import com.mengma.dao.CustomerDao;
public class JDKProxyTest {
    @Test
    public void test() {
        // 从工厂获得指定的内容（相当于spring获得，但此内容时代理对象）
        CustomerDao customerDao = MyBeanFactory.getBean();
        // 执行方法
        customerDao.add();
        customerDao.update();
        customerDao.delete();
        customerDao.find();
    }
}
```

