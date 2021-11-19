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

## JWT使用

https://github.com/auth0/java-jwt

https://www.jianshu.com/p/0fed399c2561

JWT 规定了7个官方字段，提供使用。

- iss (issuer)：发布者
- sub (subject)：主题
- iat (Issued At)：生成签名的时间
- exp (expiration time)：签名过期时间
- aud (audience)：观众，相当于接受者
- nbf (Not Before)：生效时间
- jti (JWT ID)：编号

引入依赖

```xml
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.4.0</version>
</dependency>
```

```java
public class TestJWT {
    public static void main(String[] args) {
        String token = TestJWT.createToken();
        System.out.println(token);
        TestJWT.verifierToken(token);

    }
    public static String createToken(){
        // 使用对称算法HS256，传入密钥
        Algorithm secret = Algorithm.HMAC256("brysj");
        // 生成token
        String token = JWT.create()
                .withIssuer("nyq") // 发布者姓名
                .withIssuedAt(new Date()) // 生成签名时间
                .withExpiresAt(DateUtils.addHours(new Date(),2))// 生成签名有效期 当前时间加2小时
                .withClaim("age", "18") //插入数据
                .sign(secret); //签名
        return token;
    }
    public static void verifierToken(String token){
        Algorithm algorithm = Algorithm.HMAC256("brysj");
       /** 验证
        JWTVerifier verifier = JWT.require(algorithm)
                .withIssuer("nyq") //匹配指定的token发布者 auth0
                .build();
        DecodedJWT jwt = verifier.verify(token); //解码JWT ，verifier 可复用
        */

        // 解码
        DecodedJWT jwt = JWT.decode(token);

        //直接输出是 类名 + 哈希码 (默认执行了 `toString`方法)
        System.out.println(jwt);
        //获取算法类型
        System.out.println(jwt.getAlgorithm());// => HS256
        //获取token类型
        System.out.println(jwt.getType());// => JWT
        // 获取withIssuer 设置的值
        System.out.println(jwt.getIssuer()); // => nyq
        // 获取开始生效时间/创建时时间
        System.out.println(jwt.getIssuedAt()); // =>Tue Apr 13 16:55:23 CST 2021
        // 获取过期时间，
        System.out.println(jwt.getExpiresAt()); //=>Tue Apr 13 18:55:23 CST 2021
    }
}
```