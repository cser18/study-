## 函数式接口

* 任何接口，如果只包含唯一一个抽象方法，那么它就是一个函数式接口。

```java
public interface Runnable{
    public abstract void run();
}
```

* 对函数式接口，我们可以lamda表达式创建对象



## 为啥会出现函数式编程

不断的简化代码，下面用代码来介绍

```java
package dome;

public class lambda {
    //    3. 静态内部类
    static class Lambda2 implements ILIKE {

        @Override
        public void ilike() {
            System.out.println("i like lambda2");
        }
    }

    public static void main(String[] args) {
        // 匿名内部类
        ILIKE lambda3 = new ILIKE() {
            @Override
            public void ilike() {
                System.out.println("i like lambda3");
            }
        };
        Lambda1 lambda1 = new Lambda1();
        lambda1.ilike();

        Lambda2 lambda2 = new Lambda2();
        lambda2.ilike();

        lambda3.ilike();

        lambda3 = ()->{
            System.out.println("i like lambda4");
        };
        lambda3.ilike();

    }
}


// 1. 定义一个接口只有一个抽象方法
interface ILIKE {
    void ilike();
}

// 2. 定义一个外部类
class Lambda1 implements ILIKE {

    @Override
    public void ilike() {
        System.out.println("i like lambda1");
    }
}
```

结果：

```
i like lambda1
i like lambda2
i like lambda3
i like lambda4
```

