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



## Python3.7 安装
$ `sudo yum install yum-utils`  
$ `sudo yum-builddep python`  
$ `wget https://www.python.org/ftp/python/3.7.8/Python-3.7.8.tgz`  
$ `tar xzf Python-3.7.8.tgz`

$ `./configure --enable-optimizations`  
$ `make`

Fatal Python error: _PySys_BeginInit: can't initialize sys module  
出错： 可能OS版本太老，无法实现优化

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
$ `echo 'files = supervisord.d/conf/*.conf' >> /etc/supervisor.conf` # [include]前面的注释要去掉  

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
$`vi /etc/my.cnf`
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
$ `source /srv/awesome/www/schema.sql;`
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

### 安装cerbot
$ `yum install epel-release`  
$ `yum install certbot`  

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

nginx 配置ssl后的完整文件
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

### 自动更新证书
#### 1.crontab (centos, ubuntu)
$ `crontab -e`  
`0 1 * * * /usr/bin/certbot renew --quiet` # 每天1:00运行
$ `crontab -l`  

#### 2.systemd

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