---
title: Windows下搭建Graylog环境
date: 2020-02-04 11:41:53
---

## Graylog简介

&emsp;&emsp;Graylog是一个开源的日志**聚合**、**分析**、**审计**、**展现**和**预警**的工具。相比起ELK要更加简洁和高效，因为其部署使用简单的缘故，越来越多地被工业界使用，成为日志管理的首选工具。

<!-- more -->

&emsp;&emsp;Graylog的架构非常简单，大致上由三部分组成：

+ MongoDB：用于存储Graylog的配置

+ ElasticSearch：日志文件的持久化与检索功能

+ Graylog：提供对外接口，Web界面

  ![Graylog Architecture](http://docs.graylog.org/en/3.1/_images/architec_small_setup.png)

## Graylog环境搭建--Windows

**Platform:** Windows10

**Environment:** Docker for Windows

&emsp;&emsp;在这篇博客我将分享如何在windows上搭建起graylog环境来收集日志。首先要有windows上的docker环境（如果没有配置，请参考[传送门](https://www.jianshu.com/p/3a339072ca7d)），因为我们使用docker来运行graylog。

### 步骤

1. 新建docker compose文件，命名为`docker-compose.yml`，内容如下：

   ```yaml
   version: '2'
   services:
     graylog-mongo:
       restart: always
       image: mongo:3.6.4
       container_name: graylog-mongo
     graylog-elasticsearch:
       restart: always
       image: elasticsearch:5.6.9
       container_name: graylog-elasticsearch
       environment:
         - http.host=0.0.0.0
         - transport.host=localhost
         - network.host=0.0.0.0
         # Disable X-Pack security: https://www.elastic.co/guide/en/elasticsearch/reference/5.6/security-settings.html#general-security-settings
         - xpack.security.enabled=false
         - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
       ulimits:
         memlock:
           soft: -1
           hard: -1
       mem_limit: 1g
     # Graylog: https://hub.docker.com/r/graylog/graylog/
     graylog:
       restart: always
       image: graylog/graylog:2.4.5-2
       container_name: graylog
       environment:
         # password pepper
         - GRAYLOG_PASSWORD_SECRET=gr8r3hbnvfs73b8wefhweufpokdnc
         - GRAYLOG_ROOT_USERNAME=admin
         # Password: admin
         - GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918
         # elasticsearch host
         - GRAYLOG_ELASTICSEARCH_HOSTS=http://graylog-elasticsearch:9200
         # mongo host
         - GRAYLOG_MONGODB_URI=mongodb://graylog-mongo/graylog
         # time zone
         - GRAYLOG_ROOT_TIMEZONE=Asia/Shanghai
         - GRAYLOG_WEB_ENDPOINT_URI=http://127.0.0.1:9000/api
         - GRAYLOG_WEB_LISTEN_URI=http://0.0.0.0:9000/
         - GRAYLOG_REST_LISTEN_URI=http://0.0.0.0:9000/api
       depends_on:
         - graylog-mongo
         - graylog-elasticsearch
       ports:
         # Graylog web interface and REST API
         - 9000:9000
         # Syslog TCP
         - 514:514
         # Syslog UDP
         - 514:514/udp
         # GELF TCP
         - 12201:12201
         # GELF UDP
         - 12201:12201/udp
   ```

   可以看到上面的文件主要有三个服务：`graylog-mongo`、`graylog-elasticsearch`和`graylog`，对应了graylog架构中的三个部分，每一个部分我们都启动一个docker来运行。这里大部分的配置信息都是参考https://github.com/kwkwc/dockerSH/blob/master/services/graylog/config/graylog.conf，并不需要做过多的改动。注意`graylog`这个服务下有一个`depends_on`的配置项，意思是`graylog`的启动需要依赖于前面两个服务的启动。`GRAYLOG_ROOT_PASSWORD_SHA2`这个是密码，是`admin`这个字符串经过SHA256加密后的结果。

2. 打开命令行，进入到配置文件所在的文件夹，运行：

   ```bash
   docker-compose up -d
   ```

   初次运行需要拉去镜像，会比较慢。运行结果如图：

   ![graylog start](/images/graylog0.png)

   通过`docker ps`命令我们可以看到确实有三个docker容器在运行，说明graylog服务已经成功启动了。

3. 打开浏览器，输入`http://127.0.0.1:9000`，就可以看到graylog的界面，登录用户名和密码都是`admin`。然后就能看到主页面了。

   ![Gralog MainPage](/images/graylog1.png)

4. 接着配置输入点，点击`System/Inputs -> Inputs`。

   ![Graylog Set Inputs](/images/graylog2.png)

5. 点击`Select Input`后选择`GELF UDP`，这是日志的格式，然后点击`Launch new input`。接着设置日志来源的Node和Title，其余的使用默认的就可以了。

   ![Graylog Lauch new input](/images/graylog3.png)

6. 然后我们在主页面就能够看到有一个Local Inputs在运行了。

   ![Graylog Local Inputs Running](/images/graylog4.png)

7. 至此，在windows上我们就使用docker搭建好了Graylog的服务了。

---

## 日志收集

### 收集docker容器内的日志

&emsp;&emsp;搭建好Graylog服务后，我们运行一个demo来测试一下graylog是否能够真正地收集到服务运行的日志。这里我用redis服务作为例子，展示如何使用Graylog收集docker容器的日志。

1. 编写redis容器的启动文件`docker-compose.yml`（与上面的分开地方放）：

   ```yaml
   version: '2'
   services:
     redis:
       restart: always
       image: redis:4.0.10
       container_name: redis
       logging:
         driver: gelf
         options:
           # 将 x.x.x.x 替换成你的 IP
           gelf-address: udp://x.x.x.x:12201
           tag: "redis"
   ```

   **特别注意：IP地址必须使用Node里面的IP地址，点击进入Node里面查看即可。这里不能用localhost或者127.0.0.1，这个docker容器的网络通讯策略有关。**

2. 运行`docker-compose up -d`，启动Redis容器

3. 然后再回到Graylog中，点击Search就能看到系统运行的日志了，当然这里只有启动的日志。

   ![Graylog Collect Log from Redis](/images/graylog5.png)

### 收集非docker容器的日志

&emsp;&emsp;上面看到收集docker容器的日志是非常简单的，在运行docker的时候配置就可以了。如果要收集非docker容器的日志，那么通常需要使用第三方的插件。这里我以一个flask应用为例（Python），展示如何手机非docker容器的日志。

1. 在一个已有的`flask`项目中安装[graypy](https://pypi.org/project/graypy/)插件:

   ```bash
   pip install graypy
   ```

2. 然后在代码中使用，这里我把log的初始化封装成一个函数，返回一个logger，方便全局使用：

   ```python
   import logging
   import graypy
   
   def init_logger():
       # init logger for Graylog
       logger = logging.getLogger()
       logger.setLevel(logging.INFO)
       handler = graypy.GELFUDPHandler('localhost', 12201)
       logger.addHandler(handler)
       return logger
   ```

   其实原理就是在logging的基础上，添加了一个handler，这里使用到了`graypy.GELFUDPHandler`，指定了以UDP的方式向Graylog发送GELF类型的日志，后面的参数是host主机和端口号，与Graylog服务中设置的要保持一致。

3. 简单的使用例子：

   ```python
   logger = init_logger()
   logger.info('this is a log')
   ```

4. 然后运行这个flask服务，打开Graylog就能看到日志的输出了。

   ![Graylog Flask](/images/graylog6.png)

## 总结

&emsp;&emsp;以上就是有关在win10上graylog的搭建以及简单的使用，我们还可以在graylog中进行审计、展现和预警，这些之后有时间我会一一分享。本篇博文长期更新，欢迎订阅，谢谢您的支持！

---

## Reference

1. [Graylog Documentation](http://docs.graylog.org/en/3.1/index.html)
2. [Graylog Website](https://www.graylog.org/)

