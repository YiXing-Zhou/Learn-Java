## CGLB

CGLIB（Code Generation Library）**是一个高性能开源的代码生成包**，它被许多 AOP 框架所使用，其底层是通过使用一个小而快的字节码处理框架 ASM（[Java](http://c.biancheng.net/java/) 字节码操控框架）转换字节码并生成新的类。

### 1. 创建目标类 GoodsDao

```java
package com.mengma.dao;
public class GoodsDao {
    public void add() {
        System.out.println("添加商品...");
    }
    public void update() {
        System.out.println("修改商品...");
    }
    public void delete() {
        System.out.println("删除商品...");
    }
    public void find() {
        System.out.println("修改商品...");
    }
}
```

### 2. 创建代理类

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



### 3. 创建实例工厂类

```java
package com.mengma.cglib;
import java.lang.reflect.Method;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;
import com.mengma.dao.GoodsDao;
import com.mengma.jdk.MyAspect;
public class MyBeanFactory {
    public static GoodsDao getBean() {
        // 准备目标类
        final GoodsDao goodsDao = new GoodsDao();
        // 创建切面类实例
        final MyAspect myAspect = new MyAspect();
        // 生成代理类，CGLIB在运行时，生成指定对象的子类，增强
        Enhancer enhancer = new Enhancer();
        // 确定需要增强的类
        enhancer.setSuperclass(goodsDao.getClass());
        // 添加回调函数
        enhancer.setCallback(new MethodInterceptor() {
            // intercept 相当于 jdk invoke，前三个参数与 jdk invoke—致
            @Override
            public Object intercept(Object proxy, Method method, Object[] args,
                    MethodProxy methodProxy) throws Throwable {
                myAspect.myBefore(); // 前增强
                Object obj = method.invoke(goodsDao, args); // 目标方法执行
                myAspect.myAfter(); // 后增强
                return obj;
            }
        });
        // 创建代理类
        GoodsDao goodsDaoProxy = (GoodsDao) enhancer.create();
        return goodsDaoProxy;
    }
}
```

### 4.  创建测试类

```java
import com.mengma.cglib.MyBeanFactory;
import com.mengma.dao.GoodsDao;
import org.junit.Test;

public class myTest {
@Test
  public void test(){
  // 从工厂获得指定的内容（相当于spring获得，但此内容时代理对象）
  GoodsDao goodsDao = MyBeanFactory.getBean();
  // 执行方法
  goodsDao.add();
  goodsDao.update();
  goodsDao.delete();
  goodsDao.find();
}
}

```

