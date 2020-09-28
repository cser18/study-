## 管道

管道**/**匿名管道**(Pipes)** ：⽤于具有亲缘关系的⽗⼦进程间或者兄弟进程之间的通信。

```Java
import java.io.IOException;
import java.io.PipedInputStream;
import java.io.PipedOutputStream;

public class Pipe {
    public static void main(String[] args) throws IOException {
        // 创建管道
        PipedInputStream pipedInputStream = new PipedInputStream();
        PipedOutputStream pipedOutputStream = new PipedOutputStream();

        pipedInputStream.connect(pipedOutputStream);

        new Thread(new Output(pipedOutputStream)).start();
        new Thread(new Input(pipedInputStream)).start();

    }
}

class Output implements Runnable{
    private PipedOutputStream pipedOutputStream;

    public Output(PipedOutputStream pipedOutputStream) {
        this.pipedOutputStream = pipedOutputStream;
    }

    @Override
    public void run() {
        try {
            pipedOutputStream.write("Hello World".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class Input implements Runnable{
    private PipedInputStream pipedInputStream ;

    public Input(PipedInputStream pipedInputStream) {
        this.pipedInputStream = pipedInputStream;
    }

    @Override
    public void run() {
        byte[] msg = new byte[1024];
        int len = 0;
        try {
            len = pipedInputStream.read(msg);
            System.out.println(new String(msg,0,len));
            pipedInputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }


    }
}

```

