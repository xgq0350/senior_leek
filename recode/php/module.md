## 扩展
### swoole
git clone http://github.com/swoole/swoole-src
git checkout v4.8.2  //(用的是php7.4，swoole选择对应的版本)
phpize7.4
./configure --with-php-config=/usr/bin/php-config7.4  //系统存在多个PHP版本时，需配置with-php-config
sudo make || sudo make install
/etc/php/7.4/mods-available 和 /etc/php/7.4/cli/conf.d 中添加swoole扩展配置
cp .libs/swoole.so /usr/lib/php/20190902  //如果php找不到扩展时

### mysqli
apt install php7.2
apt install php7.2-dev
apt install php-mysql（安装mysql扩展）

### php命令
php -m 查看当前支持的扩展 //配置了但不显示，头部会显示错误
php --ini 查看配置的扩展
 
 ### php扩展
 curl（客户端网络传输函数库，支持http/https/ftp等协议）

Guzzle http客户端

gd2（处理图像的扩展库，支持图片裁剪，生成图片）

ldap （轻量目录访问协议）

pdo_mysql （支持mysql数据库操作）

bz2（用于读写压缩文件）

gettext （用于支持国际化与本地化多语言版本）

imap （互联网电子邮件访问协议数据库）

mbstring （处理和转化多字节编码的字符串）

mysqli （mysql增强版本，用于操作mysql数据库）

sockets （支持socket通信的库）

redis （redis缓存数据库客户端操作库）

memcached （memcached客户端库）

mongodb （mongodb客户端操作库）

phpSpreadsheet （excel扩展库，用纯PHP编写的库，phpexcel由于版本陈旧性能低下 官方放弃维护，）

xlswriter（excel扩展库， 可用于Excel 2007+ XLSX）

maatwebsite（excel扩展库）

openssl （加解密扩展）

其他

sphinx+jieba（支持Mysql全文查询）