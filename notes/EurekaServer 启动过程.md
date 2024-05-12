# EurekaServer 启动过程

启动类 `com.netflix.eureka.EurekaBootStrap`  相关启动方法 `contextInitialized()`

```java

/**
 * 当Servlet上下文初始化时调用此方法。主要完成以下功能：
 * 1. 初始化Eureka环境；
 * 2. 初始化Eureka服务器上下文；
 * 3. 将Eureka服务器上下文保存到Servlet上下文中。
 *
 * @param event ServletContext事件，用于获取ServletContext对象。
 */
public void contextInitialized(ServletContextEvent event) {
    try {
        // 初始化Eureka环境设置
        initEurekaEnvironment();
        // 初始化Eureka服务器上下文
        initEurekaServerContext();

        ServletContext sc = event.getServletContext();
        // 将Eureka服务器上下文实例保存到Servlet上下文中，以便其他地方使用
        sc.setAttribute(EurekaServerContext.class.getName(), serverContext);
    } catch (Throwable e) {
        // 记录无法启动Eureka服务器的错误，并抛出运行时异常
        logger.error("Cannot bootstrap eureka server :", e);
        throw new RuntimeException("Cannot bootstrap eureka server :", e);
    }
}

```