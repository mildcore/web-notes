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

    $ cd ~  
    $ mkdir .ssh  
    $ chmod 700 .ssh/  
    $ ssh-keygen -i -f Identity.pub >authorized_keys  
    
    添加多个公钥方法 $ cat ../id_rsa.pub >> authorized_keys
    
3. CRT会话属性设置 - SSH2 - 鉴权 - PublicKey 属性 - 使用会话公钥设置 - 选择私钥文件Identity

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