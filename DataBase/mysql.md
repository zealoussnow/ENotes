## MYSQL基础知识

### 1.mysql数据类型
* char(N):  表示最多达255个字符

* varchar(N): 表示可变长度的字符数据,最多也是255个

* int(N)[unsigned]: 保存从- 2147483647 到2147483648范围之内的任意整数数据。如果使用unsigned选项,则有效数据范围调整为0-4294967295。

* float[(N,d)]: 用于表示数值较小的浮点数据。N代表浮点数小数点左右数据长度的总和，d表示浮点数位于小数点右边的位数。

* date

* text / blob:

* set

* enum

### 2. 常用数据库操作

#### 创建

* 创建数据库

	create database temp
	
	set names gbk  #设置字符集为gbk

* 创建表

	create table temp ... 

#### 查询

* 查询表结构
	
	desc [TABLE_NAME];

	show craete table [TABLE_NAME];

	show full fields from [TABLE_NAME];

#### 连接数据库

* mysql连接数据库

	mysql -uroot -pxxxx或者mysql -h192.xxx.xxx.xxx -uroot -pxxx

* 让mysql允许远程链接,修改库mysql中user表里的host项：

	update user set host='%' where user='xxx';当出现Can't connect to MySQL server on '172.16.175.188' (111)
	这样的错误时，要修改my.cnf，将bind-address=127.0.0.1注释掉，然后重启服务即可。


* 默认MYSQL是没有提供重命名数据库名称的操作的。

### 3. MYSQL的内建函数

如：
	
* 查询当前版本	

		SELECT VERSION();
	
* 查询当前时间	
		
		SELECT NOW();或SELECT CURRENT_TIME();
	
* 查询当前日期	
		
		SELECT DATE(NOW());	或者 SELECT CURRENT_DATE;


### 4. SQL语句分类

* Data Definition Language(DDL) 对数据库的操作
			
	CREATE-在数据库中创建对象
		
	ALTER-修改数据库结构
		
	DROP-删除对象
	
	RENAME-重命名对象
		
* Data Manipulation Language(DML)
			
	SELECT-从数据库中获取数据
	
	INSERT-向一个表格中插入数据
	
	UPDATE-更新一个表格中的已有数据
	
	DELETE-删除表格中的数据

* Data Control Language(DCL)
			
	GRANT-赋予一个用户对数据库或数据表格等指定权限
	
	REVOKE-删除一个用户对数据库或数据表格等的指定权限

* Transaction Control(TCL)
		
	COMMIT - 保存数据操作
	
	SAVEPOINT - 为方便rollback标记一个是事务点
	
	ROLLBACK - 从最后一次COMMIT中恢复到提交前的状态

### 5. 查看mysql数据库状态
			
	show [session|global] status;

默认是session的状态，如果要查看全部的状态，要加上global

比较常用的有：
		
* 查询mysql服务启动时间

		show status like 'uptime'

* 查询执行查询操作的次数

		show status like 'com_select'

* 查询执行插入操作的次数

		show status like 'com_insert'

* 查询执行删除操作的次数

		show status like 'com_delete'

* 查询试图连接MySQL服务器的次数

		show status like 'connections'

* 查询慢查询的次数(默认是10s)

		show status like 'slow_queries'

* 查询和设置慢查询默认的时间

		show variables like 'long_query_time'
		
		set long_query_time = 1

### 小技巧

* 为了存储过程能够正常执行，我们需要把命令执行的结束符修改下，默认的为分号

		delimeter $$ #修改为两个$$
* 亚元表(dual)

		select rand_string from dual;
