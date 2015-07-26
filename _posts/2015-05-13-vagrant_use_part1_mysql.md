---
layout: post
title: "vagrant简单使用和mysql安装"
description: ""
category:  tools
tags: [vagrant,mysql]
---
{% include JB/setup %}
###vagrant 基本命令
参考[vagrant_get_start_page][vagrant_get_start_page]

- box list:         vagrant box list
- 通过名称add box:  vagrant box add ubuntu/trusty64
- 添加本地box:      vagrant box add chef/centos-6.5 ~/Downloads/virtualbox.box (通过其他方式下载的）
- 初始化虚拟机:     vagrant init ubuntu/trusty64 （会在当前目录下生成Vagrantfile)
- 启动:             vagrant  up
- 登陆:             vagrant ssh

###vagrant 安装mysql

####1. install mysql
- 修改Vagrantfile，添加 

      config.vm.provision :shell, path: "bootstrap.sh"
- 对centos，bootstrap.sh文件如下(参考 [mysql_install_page][mysql_install_page]) 

      #/usr/bin/env sh
      wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm -O /tmp/mysql-community-release-el6-5.noarch.rpm
      sudo yum -y localinstall /tmp/mysql-community-release-el6-5.noarch.rpm
      yum repolist enabled | grep "mysql.*-community.*"
      sudo yum -y install mysql-community-server
      sudo service mysqld start
      sudo service mysqld status
      #mysql_secure_installation (需要登陆进去后，通过该命令添加root用户，并修改密码)
      #auto start
      sudo /sbin/chkconfig --add mysqld
      sudo /sbin/chkconfig mysqld on
      #sudo /sbin/chkconfig --list mysqld
      #to stop
      #sudo /sbin/chkconfig mysqld off
- 对ubuntu, bootstrap.sh文件如下：

      sudo debconf-set-selections <<< 'mysql-server-5.6 mysql-server/root_password password rootpass'
      sudo debconf-set-selections <<< 'mysql-server-5.6 mysql-server/root_password_again password rootpass'
      sudo apt-get update
      sudo apt-get -y install mysql-server-5.6

####2. 在host上访问guest mysql： 
- 查看guest上mysql运行的端口：

      mysql> SHOW VARIABLES WHERE Variable_name = 'port’;
- 设置端口转发： 将host的4567端口绑定到guest的3306端口(如上）, 修改Vagrantfile 添加:

      config.vm.network "forwarded_port", guest: 3306, host: 4567
- [创建用户][create_mysql_user]：create user 'host'@'%' identified by 'hostpass’;
- 查看用户：select user,host from mysql.user;
- [添加访问权限][mysql_grant]：  grant all privileges on dcdn\_stat.\* to 'host'@'%' with grant option;
- [查看权限][mysql_show_grant]：show grants for host
- 在guest上访问测试：mysql -uhost -phostpass -h10.0.2.15

      sudo /sbin/chkconfig mysqld on

[vagrant_get_start_page]:http://docs.vagrantup.com/v2/getting-started/index.html
[mysql_install_page]:http://dev.mysql.com/doc/mysql-repo-excerpt/5.6/en/linux-installation-yum-repo.html
[create_mysql_user]: https://dev.mysql.com/doc/refman/5.1/en/create-user.html
[drop_mysql_user]:https://dev.mysql.com/doc/refman/5.0/en/drop-user.html
[mysql_grant]:https://dev.mysql.com/doc/refman/5.0/en/grant.html
[mysql_show_grant]: https://dev.mysql.com/doc/refman/5.0/en/show-grants.html
