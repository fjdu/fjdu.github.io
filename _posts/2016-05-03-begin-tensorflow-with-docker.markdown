---
layout: post
title:  "初试牛刀：Mac 系统里基于 Docker 使用 Tensorflow"
date:   2016-05-03 Tue 04:18:07
categories: machine learning
---

0. 什么是 Docker？
    0. 粗略的说，是一种轻量级的虚拟机。
    1. 运行在原生系统上，不需要在原生系统里真正启动另外一个操作系统。
1. 安装 Docker
    1. 下载 Docker Toolbook 安装；会顺带装上 VirtualBox。
    2. 创建一个名为 tensorflow 的虚拟机：<br>
        `docker-machine create --driver virtualbox tensorflow`
    3. 启用 tensorflow 环境：<br>
        `docker-machine env tensorflow`<br>
        `eval $(docker-machine env tensorflow)`
2. 下载 tensorflow 镜像文件：<br>
    `docker run -it gcr.io/tensorflow/tensorflow:latest-devel`<br>
    1. 注意：有几种可下载的源：<br>
    `gcr.io/tensorflow/tensorflow` 是仅支持 CPU 的镜像文件。<br>
    `gcr.io/tensorflow/tensorflow:latest-devel` 是仅支持 CPU 的、包含源代码的镜像文件。<br>
    1. 运行前者会直接打开 Jupyter Notebook，并且提供了几个供入门的示例文件；运行后者只是进入虚拟机。<br>
    1. 还有别的镜像文件，是支持 GPU 的。<br>
    1. 镜像文件占用空间很大，两到三 GB。
3. 在虚拟机外部运行下面的命令获得 tensorflow 这个虚拟机的 IP：<br>
    `docker-machine ip tensorflow`<br>
    如果上一步直接进入了虚拟机，需要先退出才能只用 docker 相关的命令。
4. 运行下面的命令启动虚拟机：<br>
    `docker run -p 8888:8888 -it gcr.io/tensorflow/tensorflow:latest-devel`
5. 运行 `ipython notebook`，然后使用之前获得的 IP 地址以及 上面指定的 8888 端口在网页打开 ipython notebook (Jupyter notebook)。
    - 示例文件位于`/tensorflow/tensorflow/tools/docker/notebooks` 目录。
    - 可以通过建立软链接让 Jupyter notebook 可以访问这个目录。
6. 合上 MacBook 再打开，可能会发现“read: operation timed out”这样的提示，并且会发现 ipython notebook 似乎与虚拟机断开了联系。这时候，通过 `docker ps` 显示出正在运行的 container，然后通过 `docker attach CONTAINER` 就可以重新连接上了。
7. 在虚拟机内可以使用<br>
    `apt-get update`<br>
    `apt-get install vim`<br>
    来安装完整版的 vim，然后按照喜好进行配置。
8. 如果希望保留对虚拟机做过的改动，记得<br>
    `docker commit CONTAINER [REPOSITORY[:TAG]]`
9. 使用 `docker history IMAGE` 查看镜像文件的演变历史。
