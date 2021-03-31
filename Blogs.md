# Blogs



### @Repository 与 @Mapper区别

Mybatis+生成后需要在Mapper层加上注解

1. @Mapper 加上后service层注入会爆红，不影响使用，影响美观
2. @Repository 代替1，但是需要在启动类上加上注解@MapperScan("com.baizhuoqiukui.blogs.*.mapper")

**@Mapper 是 Mybatis 的注解，和 Spring 没有关系，@Repository 是 Spring 的注解，用于声明一个 Bean**

参考：https://blog.csdn.net/Xu_JL1997/article/details/90934359



## lombok

**@Slf4j**

如果不想每次都写private final Logger logger = LoggerFactory.getLogger(当前类名.class); 可以用注解@Slf4j;

```yml
# 配置日志打印级别
logging:
  level:
    root: warn
    com:
      baizhuoqiukui: debug
```

```java
log.debug("login {}", authUser);//authUser会在{}中打印
```

**@Accessors(chain=true)**

链式访问，该注解设置chain=true，生成setter方法返回this（***也就是返回的是对象\***），代替了默认的返回void。

**@Accessors(fluent = true)**

与chain=true类似，区别在于getter和setter不带set和get前缀。

**@Accessors(prefix = "f")**

set方法忽略指定的前缀。不推荐大神们这样去命名。