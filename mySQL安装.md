# MySQL安装
#### 拷贝my_default.ini--赋值为my.ini--该文件为initial的初始文件
 - 在my.ini文件中增加：
```
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:\wamp-all\mysql-5.7.13
# 设置mysql数据库的数据的存放目录
datadir=C:\wamp-all\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=InnoDB
```
#### 存储引擎InnoDB支持事物，MyISAM不支持事务
#### MySQL安装常用命令：
- 起停服务：
    - net start mysql
    - net stop mysql
- 卸载mySQL：
    - mysqld -remove
- 安装mySQL：
    - cd 解压包目录下的bin文件夹
    - mysqld install
    - mysqld --initialize-insecure 
    - net start mysql
- 连接数据库：
    - mysql -u root --skip-password
- 修改root的密码：
    - ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';