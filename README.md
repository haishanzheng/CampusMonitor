# CampusMonitor

## 项目说明

一键部署Zabbix+Grafana+Icinga+SmokePing监控系统

不保证没有错误，有意见或者建议可邮件 haishanzheng@sina.com 。

## Vagrant测试方法

- 安装Vagrant、VirtualBox。
- Clone源代码。
- ansible/group_vars/all.template.yml 复制一份 改名为 all.yml
- ansible/roles/smokeping/templates/Targets.custom.template.j2 复制一份改名为 Targets.custom.j2
- 进入项目目录，运行vagrant up
- 如果有错误，多运行几次vagrant provision

如果要关闭虚拟机，运行vagrant halt。

如果要重来或删除虚拟机们，运行vagrant destroy。

## GUI登录到VirtualBox的虚拟机

打开VirtualBox，打开GUI，使用用户名vagrant，密码vagrant登录。

## SSH登录到VirtualBox的虚拟机

IP为 192.168.3.xx
Private Key为项目目录里面的 .vagrant/machines/#{hostname}/virtualbox/private_key

可以登录主控服务器后，通过ssh -i /vagrantkeys/cm-icinga-private_key 192.168.3.13

## 非Vagrant方法

提供虚拟机

在Ansible根目录建立production.ini，里面按照staging.ini编写远程服务器IP地址。不想安装的服务器可以不写。

ansible-playbook site.yml -i production.ini

## 配置

### Zabbix

http://192.168.3.11/zabbix/setup.php

只需要输入all.yml里面设置的密码，其他默认接口。

登录页面默认用户名密码admin:zabbix

http://192.168.3.11/zabbix

### Grafana

http://192.168.3.11:3000

默认用户名密码为admin:admin

### SmokePing

http://192.168.3.14

### Icinga

设置稍微复杂。

http://192.168.3.13/icingaweb2/setup

ssh到192.168.3.13

sudo icingacli setup token create

得到token，输入到setup web页面。

Database Resource

Database Name: icingaweb2

Username: icingaweb2

密码是all.yml的MYSQL_PASSWORD


set admin username and password to admin:admin

Monitoring IDO Resource

Database Name: icinga2

Username and password is: cat /etc/dbconfig-common/icinga2-ido-mysql.conf


Command Transport

Transport Type: Local Command File


after setup, token file is deleted, setup is disabled.

http://192.168.3.13/icingaweb2

