## 使用教程(Quick start)

### 启动(Start)

	在当前目录执行，docker-compose up 或者   docker-compose up -d

	-d：参数作用为让当前进程后台运行，并且下次容器会随着docker的启动而启动

### 容器内包含的服务

	php-fpm  nginx   mysql
	使用以下命令进入容器可以查看:
	docker exec -it lnmp sh
	状态(Status)：
		ps aux|grep nginx
		ps aux|grep mysql
		ps aux|grep php-fpm
	# 或者(Or)
		systemctl status nginx
		systemctl status mysqld
		systemctl status php7

mysql初始密码(Default password)

	cat /var/log/mysqld.log|grep 'A temporary password'

修改mysql密码

	mysql -uroot -p
	输入密码：,umL7n_h+u+d
	SHOW VARIABLES LIKE 'validate_password%';
	set global validate_password_policy=LOW;  
	set global validate_password_length=6;   
	alter user 'root'@'localhost' identified by '123456';

root用户远程登录权限

	CREATE USER 'root'@'%' IDENTIFIED BY '123456';
	GRANT ALL PRIVILEGES ON * . * TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0 ;
	flush privileges;
	quit;
	mysql -uroot -p
	输入密码123456  看是否修改成功



###配置(Config)

	容器中配置文件的路径(Config file path)
	Nginx：/usr/local/nginx/conf/nginx.conf
	MySQL：/etc/my.cnf
	PHP:
		/usr/local/php7/lib/php.ini
		/usr/local/php7/etc/php-fpm.conf
		/usr/local/php7/etc/php-fpm.d/www.conf
	PHP扩展(PHP extension)：
		默认已安装部分扩展在目录：/usr/local/php7/lib/php/extensions/no-debug-non-zts-20170718/
		如果要启用指定扩展，则需要修改php.ini，加上：extension=xxx.so
		xxx为PHP扩展的文件名，然后重启php：systemctl restart php7