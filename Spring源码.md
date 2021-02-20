# Spring源码

![image-20201110141845269](Spring源码.assets/image-20201110141845269.png)

![image-20201110143220124](Spring源码.assets/image-20201110143220124.png)

## 

## Bean的创建过程BeanFactory

1.经历一堆XXXAware，把Bean需要的Spring组件给bean，通过setXX方法

2.BeanPostProcessors 中的postProcessBeforeInitialization

3.IntializingBean 调用init方法

4.init-method 用户自定义的方法

5.BeanPostProcessors中的 postProcessAfterInitialization  初始化完毕后进行 

6.DisposableBean 的destory方法销毁容器

7.Destroy-Method  用户自定义销毁的方法

```java
<p>Bean factory implementations should support the standard bean lifecycle interfaces
* as far as possible. The full set of initialization methods and their standard order is:
* <ol>
* <li>BeanNameAware's {@code setBeanName}
* <li>BeanClassLoaderAware's {@code setBeanClassLoader}
* <li>BeanFactoryAware's {@code setBeanFactory}
* <li>EnvironmentAware's {@code setEnvironment}
* <li>EmbeddedValueResolverAware's {@code setEmbeddedValueResolver}
* <li>ResourceLoaderAware's {@code setResourceLoader}
* (only applicable when running in an application context)
* <li>ApplicationEventPublisherAware's {@code setApplicationEventPublisher}
* (only applicable when running in an application context)
* <li>MessageSourceAware's {@code setMessageSource}
* (only applicable when running in an application context)
* <li>ApplicationContextAware's {@code setApplicationContext}
* (only applicable when running in an application context)
* <li>ServletContextAware's {@code setServletContext}
* (only applicable when running in a web application context)
* <li>{@code postProcessBeforeInitialization} methods of BeanPostProcessors
* <li>InitializingBean's {@code afterPropertiesSet}
* <li>a custom init-method definition
* <li>{@code postProcessAfterInitialization} methods of BeanPostProcessors
* </ol>
*
* <p>On shutdown of a bean factory, the following lifecycle methods apply:
* <ol>
* <li>{@code postProcessBeforeDestruction} methods of DestructionAwareBeanPostProcessors
* <li>DisposableBean's {@code destroy}
* <li>a custom destroy-method definition
* </ol>
```

## 

## FactoryBean

