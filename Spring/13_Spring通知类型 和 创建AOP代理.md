## Spring 通知类型

​	通知（Advice）其实就是对目标切入点进行增强的内容，Spring AOP 为通知（Advice）提供了 org.aopalliance.aop.Advice 接口。

### Spring 通知的5种类型

| 名称                                                        | 说明                                                         |
| ----------------------------------------------------------- | :----------------------------------------------------------- |
| org.springframework.aop.MethodBeforeAdvice（前置通知）      | 在方法之前自动执行的通知称为前置通知，**可以应用于权限管理等功能。** |
| org.springframework.aop.AfterReturningAdvice（后置通知）    | 在方法之后自动执行的通知称为后置通知，**可以应用于关闭流、上传文件、删除临时文件等功能。** |
| org.aopalliance.intercept.MethodInterceptor（环绕通知）     | 在方法前后自动执行的通知称为环绕通知，**可以应用于日志、事务管理等功能。** |
| org.springframework.aop.ThrowsAdvice（异常通知）            | 在方法抛出异常时自动执行的通知称为异常通知，可以应用于处理异常记录日志等功能。 |
| org.springframework.aop.IntroductionInterceptor（引介通知） | 在目标类中添加一些新的方法和属性，可以应用于修改旧版本程序（增强类）。 |

### 声明式 Spring AOP

​	Spring 创建一个 AOP 代理的基本方法是使用 org.springframework.aop.framework.ProxyFactoryBean，这个类对应的切入点和通知提供了完整的控制能力，并可以生成指定的内容。

| 属性名称         | 描 述                                                        |
| ---------------- | ------------------------------------------------------------ |
| target           | 代理的目标对象                                               |
| proxyInterfaces  | 代理要实现的接口，如果有多个接口，则可以使用以下格式赋值： <list>   <value ></value>   ... </list> |
| proxyTargetClass | 是否对类代理而不是接口，设置为 true 时，使用 CGLIB 代理      |
| interceptorNames | 需要植入目标的 Advice                                        |
| singleton        | 返回的代理是否为单例，默认为 true（返回单实例）              |
| optimize         | 当设置为 true 时，强制使用 CGLIB                             |