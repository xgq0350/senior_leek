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
 