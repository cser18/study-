## 三种创建方式

1. 继承Thread类(重点)
2. 实现Runnable接口(重点)
3. 实现Callable接口(了解)



## 1. 继承Thread

* 自定义线程类
* 重写run()方法，编写线程执行体
* 创建线程对象，调用start()方法启动线程
* *不建议使用：避免OOP单继承局限性*

```java
// 创建线程的方式一 继承Thread类 重写run方法
public class Thread1 extends Thread{
    @Override
    public void run(){
         // run方法线程体
        for (int i = 0; i < 20; i++) {
            System.out.println("我在看代码");
        }
    }

    public static void main(String[] args){
//        创建一个线程对象
        Thread1 thread1 = new Thread1();
        thread1.start();
        // main线程 主线程
        for(int i = 0; i < 100; i++){
            System.out.println("我在学多线程");
        }
    }

}
```

### 例子：简单的下载器

```java
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

public class Thread2 extends Thread {
    private String url;
    private String name;

    public Thread2(String url, String name) {
        this.url = url;
        this.name = name;
    }

    public void run(){
        webDownloader downloader = new webDownloader();
        downloader.downloader(url, name);
        System.out.println("下载了文件名："+name);
    }

    public static void main(String[] args) {
        String url = "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1352698016,3156902503&fm=26&gp=0.jpg";
        String file_path ="data/2.jpg";
        Thread2 thread2 = new Thread2(url,file_path);
        thread2.start();

    }
}

class webDownloader{
//    下载方法
    public void downloader(String url, String name){
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


```

## 实现Runnable

* 定义MyRunnable类实现Runnable接口
* 实现run()方法，编写线程执行体
* 创建线程对象，调用start()方法启动线程
* 推荐使用：避免单继承局限性、灵活多变

```java
//创建线程方式2 : 实现runnable接口，重写run方法 执行线程需要
//丢入到runnable接口实现类 调用start方法
public class Thread3 implements Runnable {
    @Override
    public void run() {
        // run方法线程体
        for (int i = 0; i < 20; i++) {
            System.out.println("我在看代码");
        }
    }

    public static void main(String[] args){
//        创建一个线程对象
        Thread3 thread3 = new Thread3();
        // 创建线程对象 通过线程对象来开启我们的线程 代理
//        Thread thread = new Thread(thread3);
//        thread.start();
        new Thread(thread3).start();
        // main线程 主线程
        for(int i = 0; i < 100; i++){
            System.out.println("我在学多线程");
        }
    }

}

```

## 实现Callable接口 (可以定义返回值、和抛出异常)

* 实现Callable接口 需要返回值类型
* 重写call方法，需要抛出异常
* 创建对象目标
* 创建执行服务ExecutorService ser= Executors.newFixedThreadPool(1);
* 提交执行 Future<Boolean> result = ser.submit(t1)
* 获取结果 boolean r = result.get()
* 关闭服务：ser.shutdownNow()