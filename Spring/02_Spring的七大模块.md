## Spring 架构图

![Spring框架图](https://bkimg.cdn.bcebos.com/pic/2e2eb9389b504fc245d07093e5dde71191ef6d9d?x-bce-process=image/resize,m_lfit,w_440,limit_1/format,f_auto)

## Spring 的七大模块

### 核心容器

### 应用上下文(context)模块

### Spring AOP 模块

### JDBC 和 DAO 模块

### 对象/关系映射集成模块

### Spring Web 模块

### Spring MVC 框架

- DispatcherServlet

  前端控制器，主要职责是接收所有请求（根据配置文件来决定），并将请求转发给对应的控制器，接收控制器的处理结果，确定最终由哪个视图完成响应。

- HandlerMapping

  处理请求路径与控制器的映射关系

- Controller

  实际处理请求的组件，例如接收请求参数，决定最终是转发或重定向的方式来响应。

- ModelAndView

  控制器的处理结果，其中的Model表示转发的数据（如果是重定向，则Model没有意义），而View表示最终负责的响应的视图组件的名称

- ViewResover

  根据视图组件名称，确定是哪个视图组件。

  ![img](https://bkimg.cdn.bcebos.com/pic/faf2b2119313b07e595da04202d7912396dd8c9c?x-bce-process=image/resize,m_lfit,w_1000,limit_1/format,f_auto)