# Python Web部署笔记

## Linux系统Centos
```
[root@vultr Python-3.7.8]# uname -a
Linux vultr.guest 5.6.8-1.el7.elrepo.x86_64 #1 SMP Wed Apr 29 11:47:55 EDT 2020 x86_64 x86_64 x86_64 GNU/Linux

[root@vultr Python-3.7.8]# cat /etc/centos-release
CentOS Linux release 7.8.2003 (Core)
```

### SSH密钥连接
SecureCRT 8.5.3 

1. 工具-创建公钥-DSA

2. 把公钥放入Linux目录

    $ `cd ~`  
    $ `mkdir .ssh`  
    $ `chmod 700 .ssh/`  
    $ `ssh-keygen -i -f Identity.pub >authorized_keys`  # 转为OpenSSH密钥格式  
    
    添加多个公钥方法 $ `cat ../id_rsa.pub >> authorized_keys`
    
3. CRT会话属性设置 - SSH2 - 鉴权 - PublicKey 属性 - 使用会话公钥设置 - 选择私钥文件Identity

### SSH密钥生成方式二
使用ssh-keygen生成，会自动在linux目录生成相应文件，把私钥下载到开发机器并在SSH连接工具（如SecureCRT）里配置使用。

$ `ssh-keygen -t dsa`  
$ `cd ~/.ssh`  
$ `mv id_dsa.pub authorized_keys`  
$ `chmod 600 authorized_keys`

### 安全性设置
连接服务器，发现有很多错误连接尝试，挪威、俄罗斯等等国外的IP。几分钟就二十多个了...
```
Last failed login: Sat Aug 22 12:43:27 CST 2020 from ti0011q162-2726.bb.online.no on ssh:notty
There were 33498 failed login attempts since the last successful login.
Last login: Mon Aug 17 22:04:03 2020 from ---------

Last failed login: Sat Aug 22 12:57:11 CST 2020 from 194.87.138.116 on ssh:notty
There were 26 failed login attempts since the last successful login.
Last login: Sat Aug 22 12:51:02 2020 from ---------
```
emm，网络安全还需注意，强烈建议关闭密码连接通道  
$ `vi /etc/ssh/sshd_config`

```
passwordAuthentication no
```
$ `service sshd restart` 或 `systemctl restart sshd.service`重启sshd  
$ `systemctl status sshd`查看状态  

后面再登录看就很干净了
```
Last login: Sat Aug 22 13:05:47 2020 from ------
```

[配置参考1][ssh dsa]  
[配置参考2][ibm konwledge]

[ssh dsa]: https://blog.csdn.net/u013227473/article/details/73127622
[ibm konwledge]: https://www.ibm.com/support/knowledgecenter/SSIGMP_1.0.0/pim/unixandlinux/install_config/t_key_unilinux.htm



## nginx安装

1. [RHEL-CentOS][1] 
2. [installing-a-prebuilt-centos-rhel-package-from-the-official-nginx-repository][2]

[1]: https://nginx.org/en/linux_packages.html#RHEL-CentOS "RHEL-CentOS"
[2]: https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-a-prebuilt-centos-rhel-package-from-the-official-nginx-repository

$ `sudo yum install yum-utils`  
$ `sudo yum update` 

$ `vi /etc/yum.repos.d/nginx.repo`
```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

$ `sudo yum install nginx`

$ `nginx -v`  
$ `curl -I 127.0.0.1`

### nginx控制

[controlling nginx][nginx ctrl]

[nginx ctrl]: https://docs.nginx.com/nginx/admin-guide/basic-functionality/runtime-control/#controlling-nginx

```
nginx -s <signal>

signal: quit reload reopen stop
```

### 使用systemctl启动nginx
配置nginx.service  
$` vi /usr/lib/systemd/system/nginx.service `  
```
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
#User=root
#Group=root
Type=forking
PIDFile=/var/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

#Restart=on-failure
#RestartSec=60s

[Install]
WantedBy=multi-user.target
```
$ `systemctl enable nginx.service`  
$ `systemctl start nginx`  

若因为selinux启动失败解决方法  
$ `setenforce 0`临时关闭selinux  

```
[root@vultr ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

[root@vultr ~]# setenforce 0
[root@vultr ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31
```

永久关闭：  
$ `vi /etc/selinux/config`  

```
SELINUX=disabled
```

[stackoverflow][selinux1]  
[官网解决方式][selinux2]  
[How to Disable SELinux Temporarily or Permanently][selinux3]  
[Disable SELinux on CentOS 7][selinux4]  
[Disable SELinux on CentOS 7 / RHEL 7 / Fedora Linux][selinux5]    
[How to troubleshoot SELinux problems][selinux6]  

[selinux1]: https://stackoverflow.com/a/39979854/13907069
[selinux2]: https://www.nginx.com/blog/using-nginx-plus-with-selinux/
[selinux3]: https://www.tecmint.com/disable-selinux-in-centos-rhel-fedora/
[selinux4]: https://linuxize.com/post/how-to-disable-selinux-on-centos-7/
[selinux5]: https://www.cyberciti.biz/faq/disable-selinux-on-centos-7-rhel-7-fedora-linux/
[selinux6]: http://thedumbterminal.co.uk/posts/2010/07/how_to_troubleshoot_selinux_problems.html



## Python3.7 安装

[python3安装参考1][py1]  
[python3安装参考2][py2]

[py1]: https://tecadmin.net/install-python-3-7-on-centos/
[py2]: https://www.centos.bz/2018/01/%E5%9C%A8centos%E4%B8%8A%E5%AE%89%E8%A3%85python3%E7%9A%84%E4%B8%89%E7%A7%8D%E6%96%B9%E6%B3%95/

$ `sudo yum install yum-utils`  
$ `sudo yum-builddep python`  
$ `wget https://www.python.org/ftp/python/3.7.8/Python-3.7.8.tgz`  
$ `tar xzf Python-3.7.8.tgz`

$ `./configure --enable-optimizations`  
$ `make`

*Fatal Python error: _PySys_BeginInit: can't initialize sys module  
出错： 可能OS版本太老，无法实现优化*

$ `./configure`  
$ `make clean`  
$ `make`  
$ `make altinstall`

$ `python3 --version`
$ `pip3 --version`

### 安装程序所需python库
$ `venv\Scripts\activate`  
$ `pip freeze > requirements.p`

$ `pip3 install -r requirements.p`  
$ `chmod +x app.py`  
$ `./app.py`  # 启动测试 需配置#!/usr/bin/env python3



## Supervisor安装

$ `pip3 install supervisor`  
$ `echo_supervisord_conf > /etc/supervisord.conf`  
$ `mkdir -p /etc/supervisord.d/conf`  
$ `mkdir -p /var/log/supervisor/`  
$ `mkdir -p /var/run/supervisor/`  
$ `echo 'files = supervisord.d/conf/*.conf' >> /etc/supervisord.conf` # [include]前面的注释要去掉  

修改以下默认地址`/var/run/supervisor/supervisor.sock`, `/var/log/supervisor/supervisor.log`, `/var/run/supervisor.pid`

$ `wget -O /usr/lib/systemd/system/supervisord.service https://raw.githubusercontent.com/Supervisor/initscripts/master/centos-systemd-etcs`  
$ `vi /usr/lib/systemd/system/supervisord.service` # 修改`/usr/bin`为`/usr/local/bin`  

```
# supervisord service for systemd (CentOS 7.0+)
# by ET-CS (https://github.com/ET-CS)
[Unit]
Description=Supervisor daemon

[Service]
Type=forking
ExecStart=/usr/local/bin/supervisord -c /etc/supervisord.conf
ExecStop=/usr/local/bin/supervisorctl shutdown
ExecReload=/usr/local/bin/supervisorctl reload
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

$ `systemctl enable supervisord.service`  
$ `systemctl start supervisord.service`  
$ `systemctl status supervisord.service`  

### supervisor配置app运行
$ `vi /etc/supervisord.d/conf/awesome.conf`  
```
[program:awesome]
command=/srv/awesome/www/app.py
directory=/srv/awesome/www
user=wwwdata
startsecs=3
redirect_stderr=true
stdout_logfile_maxbytes=50MB
stdout_logfile_backups=10
stdout_logfile=/srv/awesome/log/app.log
```
$ `useradd wwwdata`  
$ `supervisorctl reload`  
$ `supervisorctl start awesome`  

### supervisor启动报错
某一次发现supervisor启动错误  
$ `systemctl status supervisord`

```
2 16:50:59 vultr.guest systemd[1]: supervisord.service: control process exited, code=exited status=2
Aug 22 16:50:59 vultr.guest systemd[1]: Failed to start Supervisor daemon.
Aug 22 16:50:59 vultr.guest systemd[1]: Unit supervisord.service entered failed state.
Aug 22 16:50:59 vultr.guest systemd[1]: supervisord.service failed
```
使用`systemctl start supervisord`尝试启动后，会出现错误提示，提示使用命令`journalctl -xe`查看具体信息  
$ `journalctl -xe`，可以看到如下信息 

```
Aug 22 16:54:42 vultr.guest supervisord[1659]: Error: Cannot open an HTTP server: socket.error reported errno.ENOENT (2)
Aug 22 16:54:42 vultr.guest supervisord[1659]: For help, use /usr/local/bin/supervisord -h
Aug 22 16:54:42 vultr.guest systemd[1]: supervisord.service: control process exited, code=exited status=2
Aug 22 16:54:42 vultr.guest systemd[1]: Failed to start Supervisor daemon.
```
搜索找到[原因][socket error]，是由于`/var/run/supervisor/`文件夹不存在（不知道怎么没了），解决后成功启动。

[socket error]: https://www.peterbe.com/plog/strange-socket-related-error-with-supervisord "Strange socket related error with supervisord"



## MySQL5.7安装

[mysql install][]

[mysql install]: https://dev.mysql.com/doc/refman/5.7/en/linux-installation-yum-repo.html

$ `wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm`  
$ `yum localinstall mysql80-community-release-el7-3.noarch.rpm`  
$ `yum repolist enabled | grep "mysql.*-community.*"` 检查  

$ `sudo yum-config-manager --disable mysql80-community`  
$ `sudo yum-config-manager --enable mysql57-community`  
$ `sudo yum repolist enabled | grep mysql`  

$ `sudo yum module disable mysql`       # EL8 systems only  
$ `sudo yum install mysql-community-server`  
$ `sudo service mysqld start`  
$ `sudo service mysqld status`   
$ `systemctl enable mysqld`

$ `sudo grep 'temporary password' /var/log/mysqld.log`  #自动生成的root密码  
$ `mysql -uroot -p`  
$ `ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';`  
$ `quit;`  

$ `sudo yum --disablerepo=\* --enablerepo='mysql*-community*' list available`	# 显示系统支持的其他mysql组件包  
$ `sudo yum install package-name`  

### 配置数据库
$ `vi /etc/my.cnf`
```
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
symbolic-links=0
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

# define by bobo.
port=3306
key_buffer_size=16M
max_allowed_packet=8M

log_timestamps=SYSTEM
#default-time-zone='+8:00'
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
skip-character-set-client-handshake


[client]
port=3306
socket=/var/lib/mysql/mysql.sock

[mysqldump]
quick

[mysql]
no-auto-rehash
connect_timeout=2
```
$ `systemctl restart mysqld`

### 创建数据库，恢复数据
$ `source /srv/awesome/www/schema.sql;`  #登录mysql后操作  
$ `mysql -uroot -p awesome < /srv/awesome/backup/awesome_back_20200807.sql`

### 修改config_server.py
部署环境数据库地址用户名等



## SSL

使用Letsencrypt，参考：  
[Let's Encrypt 给网站加 HTTPS 完全指南][ssl1]  
[笔记：Let’s Encrypt 获取 TLS 证书（Webroot + Nginx）][ssl2]   
[How To Secure Nginx with Let's Encrypt on CentOS 7][ssl3]

[ssl1]: https://ksmx.me/letsencrypt-ssl-https/amp/
[ssl2]: https://moxo.io/blog/2018/01/30/obtain-and-renew-tls-certs-using-letsencrypt/
[ssl3]: https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-centos-7

### 安装certbot
$ `yum install epel-release`  
$ `yum install certbot`  

### 常用certbot命令
#### certbot certonly
```
# 查看certonly的详细帮助
certbot -h certonly

# create cert but not install, 可以在末尾加上`--dryrun`仅运行测试
certbot certonly --webroot -w /var/www/letsencrypt -d example.com,www.example.com -m

# create cert from configfile
certbot certonly -c /etc/letsencrypt/example.com.conf

# domain and email
domains = example.com
rsa-key-size = 2048
email = your-email@example.com
text = True

# webroot
authenticator = webroot
webroot-path = /var/www/letsencrypt
```

#### show and delete
```
# show certs
certbot certificates

# delete cert
certbot delete --cert-name example.com
```

#### renew
```
# 查看renew的详细帮助
certbot -h renew

# test: renew all certs, it is only a test, not producing files actually.
certbot renew --dry-run

# renw all certs
certbot renew

# renw all certs in quiet (for crontab)
certbot renew -q

# renew and execute post-scripts
certbot renew -q --post-hook "nginx -s reload"
```

### 生成证书
$ `mkdir /etc/letsencrypt/configs`  
$ `vi /etc/letsencrypt/configs/example.com.conf`  

```
# 写你的域名和邮箱
domains = example.com
rsa-key-size = 2048
email = your-email@example.com
text = True

# 使用webroot方式验证域名，并且使用nginx配置的验证路径
authenticator = webroot
webroot-path = /var/www/letsencrypt
```

$ `vi /etc/nginx/conf.d/awesome.conf`  
```
# /etc/nginx/conf.d/awesome.conf
# nginx conf for awesome

server {
    listen 80 default_server;

    root        /srv/awesome/www;
    access_log  /srv/awesome/log/access.log;
    error_log   /srv/awesome/log/error.log;

    location = / {
        proxy_pass      http://127.0.0.1:9000;
    }

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        root /var/www/letsencrypt;
    }

    location ^~ /static/ {
        root    /srv/awesome/www;
    }

    location / {
        proxy_pass      http://127.0.0.1:9000;
        proxy_set_header Host $Host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
```

$ `mkdir -p /var/www/letsencrypt`  
$ `nginx -s reload`  
$ `certbot -c /etc/letsencrypt/config/example.com.conf certonly`  

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2020-11-10. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
```

### 配置nginx-ssl
[Mozilla SSL Configuration Generator] [ssl-mz]
[ssl-mz]: https://ssl-config.mozilla.org/

$ `mkdir /etc/nginx/ssl`
$ `openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048`

ssl_trusted_certificate 需要下载 Root Certificates，不过根据 Nginx 官方文档 所说，ssl_certificate 如果已经包含了 intermediates 就不再需要提供 ssl_trusted_certificate，这里我直接用了ssl_certificate的文件fullchain.pem
```
$ cd /etc/letsencrypt/live/example.com
$ sudo wget https://letsencrypt.org/certs/isrgrootx1.pem
$ sudo mv isrgrootx1.pem root.pem
$ sudo cat root.pem chain.pem > root_ca_cert_plus_intermediates
```

#### nginx 配置ssl后的完整文件
```
# /etc/nginx/conf.d/awesome.conf
# nginx conf for awesome

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # curl https://ssl-config.mozilla.org/ffdhe2048.txt > /path/to/dhparam
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    # intermediate configuration
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    # OCSP stapling
    ssl_stapling on;
    ssl_stapling_verify on;

    # verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /etc/letsencrypt/live/example.com/fullchain.pem;

    # replace with the IP address of your resolver
    resolver 173.199.96.96 173.199.96.97;


    root        /srv/awesome/www;
    access_log  /srv/awesome/log/access.log;
    error_log   /srv/awesome/log/error.log;

    #location = / {
    #    proxy_pass      http://127.0.0.1:9000;
    #}

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        root /var/www/letsencrypt;
    }

    location ^~ /static/ {
        root    /srv/awesome/www;
    }

    location / {
        proxy_pass      http://127.0.0.1:9000;
        proxy_set_header Host $Host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
$ `nginx -s reload`  

### 更改域名
1. 新域名下DNS配置，www.new.com, new.com指向本服务器IP
  - A记录， @， <ip addr>
  - A记录，www, <ip addr> 或CNAME记录，www, new.com

2. 申请新证书
    - vi /etc/letsencrypt/cofnig/new.conf.conf
```
domains = example.com
rsa-key-size = 2048
email = your-email@example.com
text = True
# webroot with nginx, static file path should be configured in nginx.
authenticator = webroot
webroot-path = /var/www/letsencrypt
```
    - `certbot -c /etc/letsencrypt/config/example.com.conf certonly` 

3. 更改nginx配置文件awesome.conf中的域名地址（包括以域名命名的文件等等）
    
4. 重新启动nginx
  - `nginx -s reload`

### nginx配置ssl优化-awesome.conf

这个配置下，只允许对特定主机地址的访问（在此设置里不允许直接通过ip访问或者其他域名地址访问）。
并且将所有的http, https都重定向到固定的https://www.example.com （主要是example.com会重定向到www.example.com，并且http会变成https）

1. 将ssl证书相关配置放到文件开头，即全局位置
1. 第一段server配置，默认拒绝所有http/https的访问，返回444
2. 第二段server配置，允许对以下地址的http访问并重定向，www.example.com example.com 127.0.0.1
3. 第三段server配置，允许对以下地址的https访问并重定向，example.com
4. 第四段server配置，统一的访问终点https://www.example.com
```
[root@vultr ~]# vi /etc/nginx/conf.d/awesome.conf
# /etc/nginx/conf.d/awesome.conf
# nginx conf for awesome

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # curl https://ssl-config.mozilla.org/ffdhe2048.txt > /path/to/dhparam
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    # intermediate configuration
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    # OCSP stapling
    ssl_stapling on;
    ssl_stapling_verify on;

    # verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /etc/letsencrypt/live/example.com/fullchain.pem;

    # replace with the IP address of your resolver
    resolver 173.199.96.96 173.199.96.97;


#server {
#    listen 80 default_server;
#    listen [::]:80 default_server;
#
#    return 301 https://$host$request_uri;
#}

server {
    listen 80 default_server;
    listen [::]:80 default_server;
    listen 443 ssl default_server;
    listen [::]:443 default_server;
    server_name _;

    return 444;
}

server {
    listen 80;
    listen [::]:80;
    server_name www.example.com example.com 127.0.0.1;

    return 301 https://www.example.com$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name example.com;

    return 301 https://www.example.com$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name www.example.com;

#    if ($host != $server_name) {
#        return 301 https://www.example.com$request_uri;
#    }

    root        /srv/awesome/www;
    access_log  /srv/awesome/log/access.log;
    error_log   /srv/awesome/log/error.log;

    #location = / {
    #    proxy_pass      http://127.0.0.1:9000;
    #}

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        root /var/www/letsencrypt;
    }

    location ^~ /static/ {
        root    /srv/awesome/www;
    }

    location / {
        proxy_pass      http://127.0.0.1:9000;
        proxy_set_header Host $Host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

```

### 自动更新证书
#### 1.crontab (centos, ubuntu)
[crontab guru](https://crontab.guru/#00_00_*_*_*)
    
1. 失败配置，问题在于nginx重启失败：
$ `crontab -e`编辑 
`0 1 * * * /usr/bin/certbot renew --quiet` # 每天1:00运行  
`0 13 13 * * nginx -s reload` # 需要重启nginx，证书才生效
$ `crontab -l`查看
   
查看系统邮件发现cron给系统发了邮件报告错误：
    - /bin/sh: nginx: command not found, 说明nginx路径需手动指定/usr/sbin/nginx
    - Cert not yet due for renewal， 说明certbot renew更新证书有时间要求，查看[官方文档](https://eff-certbot.readthedocs.io/en/stable/using.html#renewing-certificates)发现是离到期30天内

2. 测试配置，应该可以自动更新证书并重新加载到nginx，有待观测
```
[root@vultr ~]# crontab -e
01 00 * * * date >> /root/renew.log;certbot renew --post-hook "/usr/sbin/nginx -s reload" >> /root/renew.log
00 00 1 1-12/6 * echo >renew.log
````
    
#### 扩展内容-cron执行错误会有系统邮件
[您在/var/spool/mail/o2o_mq中有新邮件 - 简书](https://www.jianshu.com/p/0f808060ea87)
```
# yum install mailx
# mail
    
mail常用命令   
# 阅读当前指针指向的邮件
& 直接回车
# 阅读第7封邮件，阅读时，按空格键就是翻页，按回车键就是下移一行
& t 7
# 阅读第7封、第9封邮件
& t 7 9
# 阅读指定范围序号内的所有邮件
& t 11-20
# 第10封邮件
& d 10
# 删除第7封、第9封邮件
& d 7 9
# 删除第10-100封信息
& d 10-100
# 显示当前指针所在的邮件的邮件头
& top
# 显示系统邮件所在的文件，以及邮件总数等信息
& file
# 退出mail命令平台，并不保存之前的操作，比如删除邮件
& x
# 退出mail命令平台,保存之前的操作，比如已用`d`删除的邮件
& q    
``` 

#### 2.systemd，暂未使用

$ `vi /etc/systemd/system/letsencrypt.service`  
```
[Unit]
Description=Let's Encrypt renewal

[Service]
Type=oneshot
ExecStart=certbot renew --quiet --agree-tos
ExecStartPost=nginx -s reload
```

$ `vi /etc/systemd/system/letsencrypt.timer`  
```
[Unit]
Description=Monthly renewal of Let's Encrypt's certificaes

[Timer]
OnCalendar=monthly
Persistent=true

[Install]
WantedBy=timers.target
```

$ `systemctl enable letsencrypt.timer`  
$ `systemctl start letsencrypt.timer`  
$ `systemctl list-timers`  
