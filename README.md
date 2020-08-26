### 准备 

* [Docker CE](https://github.com/yeasy/docker_practice/tree/master/install)
* Linux （仅在 Centos 7.7 中测试，其他系统及版本有待测试）

### 说明

#### 安装内容：

* nginx 1.9.2
* mysql 5.7
* php 7.4
* compose 2.0

#### 目录结构
```
docker-lnmp
├── docker-compose.yml 
├── nginx
│  ├── conf # nginx相关配置
│  ├── log  # nginx日志，访问日志、错误日志
│  └── ssl  # ssl证书
├── html # web文件目录
├── mysql
│  ├── conf # mysql相关配置
│  ├── log  # mysql日志
│  ├── init # 初始文件
│  └── data # mysql数据
├── php
│  ├── conf  # php相关配置
│  └── Dockerfile  # php镜像文件
```

### 安装

> 开始在本地的虚拟机中安装，由于镜像问题，一直超时安装不成功，换成阿里云镜像中，虽然快了一些，但还是一直超时，最后直接在阿里云服务器上安装成功，建议使用云服务器安装，省心^_^。

```shell
$ git clone https://github.com/vvk/docker-lnmp.git
$ cd docker-lnmp
$ docker-compose up -d
Creating docker-lnmp_mysql_1 ... done
Creating docker-lnmp_php_1   ... done
Creating composer             ... done
Creating docker-lnmp_nginx_1 ... done
```

测试是否启动成功：

```shell
$ docker ps
a7c797cb350d        nginx:1.9.1        "/docker-entrypoint.…"   40 seconds ago      Up 38 seconds       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   docker-lnmp_nginx_1
fcabcbdd2fbd        php:7.4-fpm    "docker-php-entrypoi…"   42 seconds ago      Up 40 seconds       0.0.0.0:9000->9000/tcp                     docker-lnmp_php_1
66857b56b61a        mysql:5.7           "docker-entrypoint.s…"   42 seconds ago      Up 40 seconds       0.0.0.0:3306->3306/tcp, 33060/tcp          docker-lnmp_mysql_1
```

此时，表示docker启动成功。

> 注：mysql默认用户名 `root`的密码为 `root`，同时添加用户名 `user`，密码为 `user123456`。

`docker-compose` 常用命令可参考 [https://yeasy.gitbook.io/docker_practice/compose/commands](https://yeasy.gitbook.io/docker_practice/compose/commands)

### 测试

* 测试是否可以正常连接 `nginx` 容器：

```shell
$ curl 127.0.0.1
this is index.html.
```

* 测试 `php` 文件是否可以正常执行：在浏览器中输入 `ip/p.php`，如果可正常显示phpinfo页面，则表面 `php` 文件可正常执行。
* 测试 `php` 是否可以正常连接 `mysql`：在浏览器中输入 `ip/db.php`，如果显示连接成功，则 `php` 可以正常连接 `mysql`。

### Laravel 开发

可以使用如下命令创建 `Laravel` 项目：

```shell
docker run -it -v $PWD:/app composer:2.0 composer create-project --prefer-dist laravel/laravel:~6 blog -vvv
```

上面命令执行完以后，会在目录 `$PWD/blog` 创建 `Laravel` 项目，可以通过 `ip/blog/public` 在浏览器中正常访问。也可以修改 `$PWD/nginx/conf/default.conf` 配置文件或在 `$PWD/nginx/conf` 目录中添加 `nginx` 配置文件，让域名指向 `laravel` 根目录，以便可以直接通过域名访问 `laravel` 项目。

### 参考 资料

* [从入门到实践](https://yeasy.gitbook.io/docker_practice/)
* [LNMP Docker](https://github.com/khs1994-docker/lnmp/blob/master/README.cn.md)
* [PHP Docker Official Images](https://hub.docker.com/_/php?tab=description)
