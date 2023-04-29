​
composer require maxakawizard/xls-writer出错提示
curl error 7 while downloading https://api.github.com/: Failed to connect to api.github.com 443

解决办法

Composer 各大厂商镜像地址 | Laravel China 社区

composer clearcache

composer update

composer 参数 --prefer-dist和--prefer-source区别
-prefer-dist 会从github 上下载.zip压缩包，并缓存到本地。下次再安装就会从本地加载，但没有保留 .git文件夹,没有版本信息。

–prefer-source 会从github 上clone 源代码，但保留了.git文件夹，从而可以实现版本控制。适合用于修改源代码。

composer配置全局仓库
composer config repo.packagist composer https://mirrors.aliyun.com/composer/

composer包编写        
创建git项目XXX
在XXX项目下执行composer init
composer.json添加自动加载"autoload": { "psr-4": { "Composer\\": "src/" } }
验证composer.json是否正确composer validate composer.json
提交代码到git并且打上tag
将项目上传至https://packagist.org 
composer require *** 即可拉取最新代码
composer的仓库repositories配置参考
        The composer.json schema - Composer

composer 报错OpenSSL SSL_read: Connection was reset
        git config --global http.sslVerify "false"  

        或配置增加

        [http] sslverify = false [https] sslverify = false

​