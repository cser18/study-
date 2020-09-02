## Docker帮助命令

* docker --version
* docker info
* docker  --help

## Docker镜像命令

docker images

* 列出本地主机上的镜像
  * REPOSITORY： 表示镜像的仓库源
  * TAG：镜像的标签
  * IMAGE ID：镜像ID
  * CREATED： 镜像创建时间
  * SIZE：镜像大小
* OPTIONS说明：
  * -a 列出本地所有的镜像（含中间映像层）
  * -q：只显示镜像ID
  * --digests： 显示镜像的摘要信息
  * -- no-trunc： 显示完整的镜像信息

docker search 某个XX镜像的名字

* 网站 https:// hub.docker.com
* 命令
  * docker search [option] 镜像名字
  * -s 列出收藏数不小于指定值的镜像
  * --automated: 只列出automated build 类型的镜像

docker pull 某个XX镜像名字

* 下载镜像
* docker pull 镜像名字[:TAG]

docker rmi 某个xxx镜像名字ID

* 删除镜像
* 删除单个
  * docker rmi -f 镜像ID
* 删除多个
  * docker rmi -f 镜像名1:TAG 镜像名2:TAG
* 删除全部
  * docker rmi -f $(docker images -qa)

## Docker容器命令

* 有镜像才能创建容器，这是根本前提（下载一个Centos镜像演示）

* 新建并启动容器

  * docker  run[OPTIONS] IMAGE [COMMAND][ARG..]

    * OPTIONS

      * --name="容器新名字" 为容器指定一个名称
      * -d 后台运行容器，并返回容器的ID，也即启动守护式容器
      * **-i 以交互模式运行容器，通常与-t同时使用**
      * **-t 为容器重新分配一个伪输入终端，同时与-i同时使用**
      * -P 随机端口映射
      * -p：指定端口映射，有以下四种模式

      ​        ip:hostPort:containerPort

      ​        ip::containerPort

      ​        hostPort:containerPort

      ​		containerPort

* 列出当前suo