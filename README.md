# OpenROAD-flow in Docker faster way

## 一、简介及环境

通过修改[OpenROAD-flow项目](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts)逻辑满足特殊情况下Docker的快速搭建

(解决国内网络环境对Docker构建中涉及git、wget等命令的过程产生的影响）

搭建需求如下：

系统

* Ubuntu20.04LTS（Aliyun—ECS with 2h4G）
* or Ubuntu20.04LTS（VMware虚拟机 2h2G）
* or Google cloud shell

工具

* [Docker](https://www.jianshu.com/p/da1c7dc4217a)

Docker配置（用户名、密码于[Docker官网](https://www.docker.com/)注册）

```shell

docker login -u 用户名 -p 密码
sudo service docker start

```

## 二、搭建步骤

### 解压

下载[OpenROAD-flow-docker.tar.gz](https://cloud.189.cn/t/JVj6rmquiqIb)文件并在其目录下使用以下命令解压

```shell

tar -pzxf OpenROAD-flow-docker.tar.gz 

```

### 编译

在OpenROAD-flow目录，执行编译

```shell
cd OpenROAD-flow
./build_openroad.sh

```

## 三、使用

### 运行容器

```shell

docker run -it -u $(id -u ${USER}):$(id -g ${USER}) --name openroad openroad/flow bash

```

### 设置环境变量

```shell

source ./setup_env.sh

```

## 四、检验

```shell

cd flow && make

```

## 五、查看示例

此步骤需要安装[klayout](https://www.klayout.de/build.html)

```shell

klayout results/nangate45/gcd/6_final.gds

```

Enjoy!

## 注：该项目的部分产生过程如下：

### clone[原项目](https://github.com/The-OpenROAD-Project/OpenROAD-flow.git)

```shell

git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow.git

```

### yosys预处理

预先下载yosys中的abc项目

```shell

cd OpenROAD-flow/tools/yosys/
git clone https://github.com/YosysHQ/abc

```

### Makefile预处理

修改OpenROAD-flow/tools/yosys/目录下的Makefile，令ABCREV = default，使用:wq保存退出

```shell

vim Makefile

```

### packages引入及Dockerfile文本替换

将[packages.tar.xz](https://cloud.189.cn/t/QjQB3qy2qIZv)放在OpenROAD-flow/tools/OpenROAD目录下

并将该目录下Dockerfile内文本以[Dockerfile-transform](https://cloud.189.cn/t/6JZnyeYbArMv)内文本替代（切勿直接替换文件）

本项目仅供个人学习使用，本人仅对原项目进行编译逻辑上的修改，所有LICENSE沿用原项目并在项目内的文件中保留所有声明，请勿用于违法途径。
