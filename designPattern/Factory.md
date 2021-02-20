# 工厂模式

简单工厂：方便横向扩展 ，猫、狗、猪、牛....

抽象工厂: 方便纵向扩展，产品族 {面包，ak47，车}  {蘑菇，魔法棒，扫帚}....

代码重构的技巧：当从一堆子类中抽象出父类时要慢慢的抽，不要把所有方法都放在父类里，用到哪个抽哪个。

### 创建游戏物体的工厂GameFactory

```java
public abstract class GameFactory {
    public abstract BaseTank createTank(int x, int y, Dir dir, Group group, TankFrame tf);

    public abstract BaseExplode createExplode(int x, int y, TankFrame tf);

    public abstract BaseBullet createBullet(int x, int y, Dir dir, Group group, TankFrame tf);
}
```

### 抽象的子弹、坦克、爆炸

```java
public abstract class BaseBullet {
    public abstract void collideWith(BaseTank tank);

    public abstract void paint(Graphics g);
}
```

```java
public abstract class BaseTank {
    public Group group = Group.BAD;
    public Rectangle rect = new Rectangle();


    public abstract void paint(Graphics g);

    public Group getGroup() {
        return this.group;
    }

    public abstract void die();

    public abstract int getX();

    public abstract int getY();
}
```

```java
public abstract class BaseExplode {
    public abstract void paint(Graphics g);
}
```

## 默认的产品族

### 默认的实现DefaultFactory

```java
public class DefaultFactory extends GameFactory {
    @Override
    public BaseTank createTank(int x, int y, Dir dir, Group group, TankFrame tf) {
        return new Tank(x, y, dir, tf, group);
    }

    @Override
    public BaseExplode createExplode(int x, int y, TankFrame tf) {
        return new Explode(x, y, tf);
    }

    @Override
    public BaseBullet createBullet(int x, int y, Dir dir, Group group, TankFrame tf) {
        return new Bullet(x, y, dir, tf, group);
    }
}
```



### 子弹、坦克、爆炸的具体实现

```java
public class Bullet extends BaseBullet {
	//默认子弹实现
}
```

```java
public class Tank extends BaseTank {
    //默认的坦克实现
}
```

```java
public class Explode extends BaseExplode {
    //默认的爆炸实现
}
```

## 方形产品族

### 方形族的的实现RectFactory

```java
public class RectFactory extends GameFactory {
    @Override
    public BaseTank createTank(int x, int y, Dir dir, Group group, TankFrame tf) {
        return new RectTank(x, y, dir, tf, group);
    }

    @Override
    public BaseExplode createExplode(int x, int y, TankFrame tf) {
        return new RectExplode(x, y, tf);
    }

    @Override
    public BaseBullet createBullet(int x, int y, Dir dir, Group group, TankFrame tf) {
        return new RectBullet(x, y, dir, tf, group);
    }
}
```

### 方形子弹、坦克、爆炸的具体实现

```java
public class RectBullet extends BaseBullet {
	//方形子弹实现
}
```

```java
public class RectTank extends BaseTank {
    //方形的坦克实现
}
```

```java
public class RectExplode extends BaseExplode {
    //方形的爆炸实现
}
```

### 总结

抽象工厂中，如果产品族中只有一个，更换不同的实现工厂，就是简单工厂