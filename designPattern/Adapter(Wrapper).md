# Adapter(Wrapper)

转化器接口，转接头

![image-20201216222851875](Adapter(Wrapper).assets/image-20201216222851875.png)

```java
public class Main {
    public static void main(String[] args) throws Exception {
        FileInputStream fis = new FileInputStream("c:/test.text");
        InputStreamReader isr = new InputStreamReader(fis);
        BufferedReader br = new BufferedReader(isr);
        String line = br.readLine();
        while (line != null && !line.equals("")) {
            System.out.println(line);
        }
        br.close();


    }
}
```

![image-20201216223219924](Adapter(Wrapper).assets/image-20201216223219924.png)

## 误区

常见的Adapter类反而不是Adapter

WindowAdapter

KeyAdapter

接口太多，Adapter做了空实现，用到哪个重写哪个，如果直接继承会重写很多方法。什么模式都不是