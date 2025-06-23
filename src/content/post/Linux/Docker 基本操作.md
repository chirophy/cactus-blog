---
时间: 2017/04/17
---
`Docker` 主要由 `image(镜像)`、`container(容器)`、`repository(仓库)` 三大块组成

## 安装

### 1. 官方脚本安装(特别推荐！)

安装命令如下：

```bash
curl -fsSL <https://get.docker.com> | bash -s docker --mirror Aliyun
```

或者使用国内 `daocloud` 一键安装命令：

```bash
curl -sSL <https://get.daocloud.io/docker> | sh
```

### 2. 使用 `repository` 仓库文件安装

```bash
# 设置仓库
sudo yum install -y yum-utils
sudo yum-config-manager \\
	    --add-repo \\
	    <https://download.docker.com/linux/centos/docker-ce.repo>
	    
# 安装Docker Engine
sudo yum install docker-ce docker-ce-cli containerd.io

# 列出所有版本的Docker Engine
yum list docker-ce --showduplicates | sort -r

# 安装指定版本的
sudo yum install docker-ce-18.09.1 docker-ce-cli-18.09.1 containerd.io

# 启动docker
sudo systemctl start docker

# 验证docker是否正确安装，通过运行hello-world镜像
sudo docker run hello-world
```

### 3. 使用 `rpm` 包方式安装

从官网 [**下载**](https://download.docker.com/linux/centos/) 对应版本的 `rpm` 包进行安装

可通过 `wget` 命令或者 `curl` 命令下载该安装包到指定的文件夹下

```bash
# 安装当前目录下的docker-ce-20.10.9-3.el7.x86_64.rpm
sudo yum install ./docker-ce-20.10.9-3.el7.x86_64.rpm

# 启动docker
sudo systemctl start docker

# 验证docker是否正确安装，通过运行hello-world镜像
sudo docker run hello-worl
```

## 卸载

### 1. 卸载旧版本

```bash
sudo yum remove docker \\
                  docker-client \\
                  docker-client-latest \\
                  docker-common \\
                  docker-latest \\
                  docker-latest-logrotate \\
                  docker-logrotate \\ 
                  docker-engine
```

### 2. 强制卸载

```bash
# 卸载
sudo yum remove docker-ce docker-ce-cli containerd.io

# 删除指定的目录
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

## 常用命令

### 启动docker

```bash
systemctl start docker
```

### 设置开机启动

```bash
systemctl enable docker
```

### 查看版本

```bash
docker version
```

### 获取镜像

```bash
docker pull [镜像名称]

# eg：
docker pull ubuntu
```

### 创建并启动容器

```bash
docker run -it [镜像名称] bash
```

> -it ：这是两个参数(-i表示交互式操作， -t 表示终端)
> 
> bash ：表示进入操作终端，基于交互式进行相关操作

### 查看所有的容器

```bash
docker ps -a
```

### 启动容器

```bash
docker start [容器ID]
```

### 停止容器

```bash
docker stop [容器ID]
```

### 进入容器

```bash
docker exec -it [容器ID] /bin/bash
```

**注：**推荐使用 `docker exec`

使用此命令 如果从该容器退出 容器不会**停止**

### 导出容器

```bash
docker export [容器ID] > [导出名称]
```

### 导入容器

```bash
docker import [容器快照名]
or
# 通过指定 URL 或者某个目录来导入
docker import <http://example.com/exampleimage.tgz> example/imagerepo
```

### 删除容器

```bash
docker rm -f [容器ID]
```

## 镜像加速

- DaoCloud:[https://hub.daocloud.io/](https://hub.daocloud.io/)
- 科大镜像：[https://docker.mirrors.ustc.edu.cn/](https://docker.mirrors.ustc.edu.cn/)
- 网易：[https://hub-mirror.c.163.com/](https://hub-mirror.c.163.com/)
- 阿里云：https://<你的id>.mirror.aliyuncs.com
- 七牛云：[https://reg-mirror.qiniu.com](https://reg-mirror.qiniu.com)

1. 对于使用 `systemd` 的系统，请在 `/etc/docker/daemon.json` 中写入如下内容（如果文件不存在请新建该文件）：

```bash
{"registry-mirrors":["<https://reg-mirror.qiniu.com/>"]}
```

1. 重新启动服务

```bash
systemctl restart docker
```

1. 检查加速镜像是否生效

```bash
docker info
```

## 更新

国内主流docker镜像已于2024年6月6日集体下架，建议自建仓库。