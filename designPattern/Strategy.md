# Strategy

策略模式是定义一系列完成相同功能的算法，只是具体实现不同，消除客户端的条件判断语句。

### 开火策略的抽象

```java
public interface FireStrategy {
    void fire(Tank t);
}
```

### 四向开火策略

```java
public class FourFireStrategy implements FireStrategy {
    @Override
    public void fire(Tank t) {
		//实现四个方向开火
    }
}
```

### 默认开火策略

```java
public class DefaultFireStrategy implements FireStrategy {
    @Override
    public void fire(Tank t) {
		//默认开火策略
    }
}
```

## 策略模式和工厂模式的区别

- 用途不一样
  工厂是创建型模式,它的作用就是**创建对象**；
  策略是行为型模式,它的作用是让**一个对象在许多行为中选择一种行为**;

- 关注点不一样
  一个关注**对象**创建
  一个关注**行为**的封装
- 解决不同的问题
  工厂模式是创建型的设计模式，它接受指令，创建出符合要求的实例；它主要解决的是资源的统一分发，将对象的创建完全独立出来，**让对象的创建和具体的使用客户无关**。主要应用在多数据库选择，类库文件加载等。
  策略模式是为了解决的是策略的切换与扩展，更简洁的说是**定义策略族**，分别封装起来，让他们之间可以相互替换，**策略模式让策略的变化独立于使用策略的客户**。