## 一、安装MySQL

在Ubuntu中，默认情况下，只有最新版本的MySQL包含在APT软件包存储库中，要安装它，只需更新服务器上的包索引并安装默认包`apt-get`。

```
sudo apt-get update
```

1、安装MySQL服务：

```
sudo apt install mysql-server-5.7
```

2、查看MySQL版本

```
mysql -V
```

## 二、配置MySQL

先启动MySQL

```
sudo service mysql start
```

然后配置

```
sudo mysql_secure_installation
```

需要输入Y or N，以及设密码

1. VALIDATE PASSWORD PLUGIN can be used to test passwords and improve security. It checks the strength of password and allows the users to set only those passwords which are secure enough. Would you like to setup VALIDATE PASSWORD plugin?
Press y|Y for Yes, any other key for No: **N**
2. Please set the password for root here.
New password: （设置密码，可以123456）
Re-enter new password: （再次输入）
3. By default, a MySQL installation has an anonymous user, allowing anyone to log into MySQL without having to have a user account created for them. This is intended only for testing, and to make the installation go a bit smoother. You should remove them before moving into a production environment.
Remove anonymous users? (Press y|Y for Yes, any other key for No) : **N**
4. Normally, root should only be allowed to connect from 'localhost'. This ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : **N**
5. By default, MySQL comes with a database named 'test' that anyone can access. This is also intended only for testing, and should be removed before moving into a production environment.
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : **N**
6. Reloading the privilege tables will ensure that all changes made so far will take effect immediately.
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : **Y**

Success. All done! 

## 三、修改root账户认证方式

连接到MySQL：

```
sudo mysql -uroot -p
```

1、查看用户：

```
mysql> select user, plugin from mysql.user;
```

2、重置Root密码，修改认证方式：

```
mysql> update mysql.user set authentication_string=PASSWORD('123456'), plugin='mysql_native_password' where user='root';
mysql> flush privileges;
mysql> exit
```

## 四、配置远程访问MySQL

1、修改配置文件，注释掉bind-address = 127.0.0.1

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

2、保存退出，然后进入MySQL服务

```
mysql -uroot -p
```

3、执行授权命令：

```
mysql> grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
mysql> flush privileges;
mysql> exit
```

4、重启

```
sudo service mysql restart
```

## 五、如果要删除MySQL，一定要删干净

1、删除MySQL:

```
sudo apt autoremove --purge mysql-server-*
sudo apt remove mysql-server
sudo apt autoremove mysql-server
sudo apt remove mysql-common
```

2、清理残留数据

```
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```
