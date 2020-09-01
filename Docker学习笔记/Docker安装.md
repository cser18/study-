### Docker的架构图

![D2](https://github.com/cser18/study-/blob/master/img/Docker%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/D2.png)



### Docker的基本组成

#### 镜像

```java
Person p1 = new Person();
Person p2 = new Person();
Person p3 = new Person();
```

Docker 镜像（Image）就是一个可读的模板，镜像可以用来创建Docker容器，一个镜像可以创建很多容器。



| Docker | 面向对象 |
| ------ | -------- |
| 容器   | 对象     |
| 镜像   | 类       |

#### 容器

容器是镜像创建的运行实例。（容器与镜像之间的关系）

可以把容器看做是一个简易版的Linux环境



### 仓库

集中存放镜像文件的场所



### centos6.8安装Docker

1. yum install -y epel-release
2. yum install -y docker-io
3. 安装后的配置文件：ls /etc/sysconfig/docker
4. 启动Docker后台服务：service docker start
5. docker version验证

### docker HelloWorld

* docker run hello-world



![D3](https://github.com/cser18/study-/blob/master/img/Docker%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/D3.png)



* run做了什么？

![D4](https://github.com/cser18/study-/blob/master/img/Docker%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/D4.png)

## 底层原理

### Docker怎么工作的

Docker是一个C/S结构的系统，Docker守护进程运行在主机上，然后通过Socket连接从客户端访问，守护进程从客户端接收命令并管理运行在主机上的容器。**容器，是一个运行时环境，就是我们说的集装箱。**

### Docker与VM区别



