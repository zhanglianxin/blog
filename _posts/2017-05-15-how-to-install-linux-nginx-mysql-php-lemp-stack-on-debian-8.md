---
layout: post
title: 在 Debian 8 上安装 Nginx
---

> 原文： [《How To Install Linux, Nginx, MySQL, PHP (LEMP Stack) on Debian 8》](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-debian-8)

## 安装 Nginx web 服务器

略。

## 安装 MySQL 来管理站点数据

略。

## 安装 PHP 进行处理

由于 Nginx 不像其他 web 服务器那样包含本地 PHP 处理，所以我们需要安装 `fpm`，代表”fastCGI 进程管理器“。我们会告诉 Nginx 将 PHP 请求传递给这个软件进行处理。我们还将安装一个额外的帮助程序包，允许 PHP 与我们的 MySQL 数据库后端进行通信。该安装将拉入必要的 PHP 核心文件来完成这项工作。

安装 `php5-fpm` 和 `php5-mysql` 模块：

```shell
$ sudo apt-get install php5-fpm php5-mysql
```

我们现在已经安装了我们的 PHP 组件，但是我们需要进行一些轻微的配置更改，使我们的安装更加安全。

使用 root 权限打开主 `php-fpm` 配置文件：

```shell
$ sudo nano /etc/php5/fpm/php.ini
```

修改如下：

```shell
; cgi.fix_pathinfo provides *real* PATH_INFO/PATH_TRANSLATED support for CGI.  PHP's
; previous behaviour was to set PATH_TRANSLATED to SCRIPT_FILENAME, and to not grok
; what PATH_INFO is.  For more information on PATH_INFO, see the cgi specs.  Setting
; this to 1 will cause PHP CGI to fix its paths to conform to the spec.  A setting
; of zero causes PHP to behave as before.  Default is 1.  You should fix your scripts
; to use SCRIPT_FILENAME rather than PATH_TRANSLATED.
; http://php.net/cgi.fix-pathinfo
cgi.fix_pathinfo=0 # 取消注释，并将其值修改为 0
```

重启 PHP 处理器，使更改配置生效：

```shell
$ sudo systemctl restart php5-fpm
```

## 配置 Nginx 使用 PHP 处理器

现在，我们安装了所有必需的组件。 我们仍然需要的唯一配置更改是告诉 Nginx 将我们的 PHP 处理器用于动态内容。

我们在服务器块级别（服务器块类似于 Apache 虚拟主机）执行此操作。 通过键入以下内容打开默认的 Nginx 服务器块配置文件：

```shell
$ sudo nano /etc/nginx/conf.d/default.conf
```

我们需要对我们网站的这个文件进行一些修改。

* 首先，我们需要将 `index.php` 添加为我们的索引指令的第一个值，以便在请求目录时提供名为 `index.php` 的文件（如果可用）。
* 我们可以修改 `server_name` 指令来指向我们服务器的域名或公共 IP 地址。
* 对于实际的 PHP 处理，我们只需要取消注释处理 PHP 请求的一段文件。这就是 `location ~\.php$` 位置块，包含的 `fastcgi-php.conf` 片段和 `php-fpm` 关联的套接字。
* 我们还将取消处理 `.htaccess` 文件的位置块的注释。Nginx 不处理这些文件。如果这些文件中的任何一个文件碰巧发现在文档根目录中，则不应将其提供给访问者。

您需要做的更改如下：

```shell
# /etc/nginx/conf.d/default.conf
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root   /var/www/html;
    index  index.php index.html index.htm index.nginx-debian.html;

    server_name localhost;

    location / {
        try_files  $uri $uri/ =404;
    }

    location ~ \.php$ {
        fastcgi_pass   unix:/var/run/php5-fpm.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

保存以上更改之后，通过键入以下内容来测试您的配置文件的语法错误：

```shell
$ sudo nginx -t
```

如果有任何错误报告，请返回并重新检查您的文件，然后再继续。

准备好后，重新加载 Nginx 进行必要的更改：

```shell
$ sudo systemctl reload nginx
```

## 创建 PHP 文件测试配置

略。

## 还有一步

需要修改 `/etc/nginx/nginx.conf` 为：

```shell
user www-data;
```

---

Reference: 

0.  [How To Install Linux, Nginx, MySQL, PHP (LEMP) Stack on Debian 7](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-debian-7)
1.  [Installing Nginx with PHP (as PHP-FPM) and MariaDB (LEMP) on Debian 8](https://www.howtoforge.com/tutorial/installing-nginx-with-php-fpm-and-mariadb-lemp-on-debian-jessie/)
2.  [Installing Nginx, MySQL, PHP (LEMP) Stack On A Debian 8.3 Cloud Server Or VPS](https://www.atlantic.net/community/howto/install-lemp-on-debian-8/)
3.  [Install NGINX and PHP-FPM running on UNIX file sockets](https://support.rackspace.com/how-to/install-nginx-and-php-fpm-running-on-unix-file-sockets/)
4.  [nginx php-fpm 安装配置](http://www.nginx.cn/231.html)

