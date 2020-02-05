---
title: CentOS下安装Docker
date: 2020-02-05 14:33:12
---

# Docker安装--CentOS

**Platform**： CentOS7.5

&emsp;&emsp;[Docker](https://www.docker.com/)是一个开源的应用容器引擎，开发者可以将应用打包后放到容器中，然后发布到任何流行的linux机器上，这样就免去了在一台宿主机上配置环境的工作，也实现了不同服务之间的隔离。今天在这篇博客我就来和大家分享一下如何在CentOS系统中安装Docker。

<!-- more -->

## 步骤

1. 查看内核版本，docker官方要求3.8以上，建议是3.10.

   ```bash
   uname -a
   ```

2. 更新yum包

   ```bash
   yum update
   ```

3. 安装`yum-config-manager`及其依赖

   ```bash
   yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

4. 卸载当前存在的旧版本docker（Optional）

   ```bash
   yum remove docker  docker-common docker-selinux docker-engine
   ```

5. 设置yum源，第一个是中央仓库，第二个是阿里云镜像仓库（速度会快一点），二者选其一即可。

   ```bash
   yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   
   yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```

6. 查看仓库中docker的所有版本

   ```bash
   yum list docker-ce --showduplicates | sort -r
   ```

7. 安装docker，我们可以指定版本安装，也可以使用默认安装。

   ```bash
   yum -y install docker-ce # Default
   
   yum install docker-ce-18.03.1.ce # Specific Version
   ```

8. 安装完成后需要启动服务：

   ```bash
   systemctl start docker # Start Docker Service
   systemctl enable docker # Auto Start
   ```

9. 验证安装

   ```bash
   docker version
   ```

   ![Docker Version](/images/docker0.png)

至此，在centOS上安装Docker的整个过程已经完成！