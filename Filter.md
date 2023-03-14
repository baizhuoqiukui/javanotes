# Filter

过滤器是一个对象，可以认为是Servlet的一种“加强版”，对request和response进行过滤

Filter是javax.servlet包下的一个接口

由于 **Filter 是单例多线程的**，所以为了保证其线程安全性，一般情况下是不为 Filter 类定义可修改的成员变量的。因为每个线程均可修改这个成员变量，会出现线程安全问题。

```java
public interface Filter {

    /**
     * 服务器启动的时候，Web服务器创建Filter的实例对象，并调用其init方法，完成对象的初始化功能。
     * 由 Web 容器调用以向过滤器指示它正在投入使用。 servlet 容器在实例化过滤器后只调用一次 init 方法。在要求过滤器执行任何过滤工作之前，init 方法必须成功完成。如果 init 方法出现以下任一情况，则 Web 容器无法将过滤器投入使用：
     * 1. 抛出ServletException异常
     * 2. 没有在web容器定义的时间内返回
     * 每个过滤器都可以访问一个 FilterConfig 对象，从中可以获取其初始化参数，它可以使用对 ServletContext 的引用，例如，加载过滤任务所需的资源
     */
    public void init(FilterConfig filterConfig) throws ServletException;

    /**
     * 每次客户端发起请求，容器都会调用Filter的doFilter方法。传入此方法的 FilterChain 允许 Filter 将请求和响应传递给链中的下一个实体。此方法可以多次执行
     */
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    /**
     * 仅当过滤器的 doFilter 方法中的所有线程都退出或经过超时时间后才调用此方法。
     * 此方法使过滤器有机会清理被占用的任何资源（例如，内存、文件句柄、线程），请空资源与内存保持同步，服务关闭时，web服务器销毁Filter的实例对象
     */
    public void destroy();

}
```

## FilterConfig

```java
/**
 * servlet 容器在初始化期间将信息传递给过滤器的过滤器配置对象
 */
public interface FilterConfig {

    /**
     * 得到filter名称
     */
    public String getFilterName();

    /**
     * 返回Servlet上下文对象的引用
     */
    public ServletContext getServletContext();

    /**
     * 返回在部署描述中指定名称的初始化参数的值，如果不存在返回null
     */
    public String getInitParameter(String name);

    /**
     * 返回过滤器的所有初始化参数的名称的枚举集合
     */
    public Enumeration<String> getInitParameterNames();

}
```