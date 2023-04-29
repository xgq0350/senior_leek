
# Mysql安装

sudo apt install mysql-server

sudo apt install mysql-client

# mysql开启、停止、状态

service mysql start（/etc/init.d/mysql start）

service mysql stop（/etc/init.d/mysql stop）

service mysql status（/etc/init.d/mysql status）

# 安装不成功问题：

1. mysql -u root -p登录出错。
      >sudo mysql 可以登录mysql。
2. 修改密码出错。
>use mysql;
>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';    
**ERROR 1819 (HY000): Your password does not satisfy the current policy requirements**

>查看当前密码策略。
>SHOW VARIABLES LIKE 'validate_password%';
* validate-password=ON/OFF/FORCE/FORCE_PLUS_PERMANENT: 决定是否使用该插件(及强制/永久强制使用)。
validate_password_dictionary_file：插件用于验证密码强度的字典文件路径。
validate_password_length：密码最小长度。
validate_password_mixed_case_count：密码至少要包含的小写字母个数和大写字母个数。
validate_password_number_count：密码至少要包含的数字个数。
validate_password_policy：密码强度检查等级，0/LOW、1/MEDIUM、2/STRONG。
validate_password_special_char_count：密码至少要包含的特殊字符数。
其中，关于validate_password_policy-密码强度检查等级：
0/LOW：只检查长度。
1/MEDIUM：检查长度、数字、大小写、特殊字符。
2/STRONG：检查长度、数字、大小写、特殊字符字典文件。
[参考解决办法](https://blog.csdn.net/shi_hong_fei_hei/article/details/127339956)
3.  Host is not allowed to connect to this MySQL server解决方法
>my.cnf配置文件修改bind-address=0.0.0.0或注释。
> use mysql;
> update user set host = '%' where user = 'root';
> FLUSH PRIVILEGES;
# Mysql主从搭建
