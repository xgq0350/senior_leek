

|   指令 | 含义    |
| --- | --- |
|    FROM [镜像] |  指定新镜像所基于的镜像，第一条指令必须为FROM指令，每创建一个镜像就需要一条FROM指令，例如centos:7。from有两层含义：①开启一个新的镜像②必须写的一行指令   |
| MAINTAINER [名字]    |  说明新镜像的维护人信息（可写可不写）   |
| RUN命令	    |     每一条RUN后面跟一条命令，在所基于的镜像上执行命令，并提交到新的镜像中，RUN必须大写|
|CMD [“要运行的程序”，“参数1”、“参数2”]|	指定启动容器时需要运行的命令或者脚本，Dockerfile只能有一条CMD命令，如果指定多条则只能执行最后一条，“bin/bash”也是一条CMD,并且会覆盖image镜像里面的cmd。|
|EXPOSE [端口号]	|指定新镜像加载到Docker时要开启的端口暴露端口，就是这个容器暴露出去的端口号。|
|ENV [环境变量] [变量值]|	设置一个环境变量的值，会被后面的RUN使用。容器可以根据自己的需求创建时传入环境变量，镜像不可以。|
|ADD [源文件/目录] [目标文件/目录]|	①将源文件复制到目标文件，源文件要与Dockerfile位于相同目录中，②或者是一个URL，③若源文件是压缩包则会将其解压缩|
|COPY [源文件/目录] [目标文件/目录]|	将本地主机上的文件/目录复制到目标地点，源文件/目录要与Dockerfile在相同的目录中，copy只能用于复制，add复制的同时，如果复制的对象是压缩包，ADD还可以解压，copy比add节省资源|
|VOLUME [“目录”]|	在容器中创建一个挂载点，简单来说就是-v，指定镜像的目录挂载到宿主机上。|
|USER [用户名/UID]|	指定运行容器时的用户|
|WORKDIR [路径]|	为后续的RUN、CMD、ENTRYPOINT指定工作目录，相当于是一个临时的"CD"，否则需要使用绝对路径，例如workdir /opt。移动到opt目录，并在这下面的指令都是在opt下执行。|
|ONBUILD [命令]|	指定所生成的镜像作为一个基础镜像时所要运行的命令*（是一种优化）**|
|HEALTHCHECK|	健康检查|
|ENTRYPOINT \[“要运行的程序”，“参数1”，“参数2”]|设定容器启动时第一个运行的命令及其参数 可以通过使用命令docker run --entrypoint 来覆盖镜像中的ENTRYPOINT指令的内容。|


例如：
```
### fd
# Default Dockerfile
#
# @link     https://www.hyperf.io
# @document https://hyperf.wiki
# @contact  group@hyperf.io
# @license  https://github.com/hyperf/hyperf/blob/master/LICENSE


FROM hyperf/hyperf:7.4-alpine-v3.11-swoole
LABEL maintainer="Hyperf Developers <group@hyperf.io>" version="1.0" license="MIT" app.name="Hyperf"


##
# ---------- env settings ----------
##
# --build-arg timezone=Asia/Shanghai
ARG timezone


ENV TIMEZONE=${timezone:-"Asia/Shanghai"} \
    APP_ENV=prod \
    SCAN_CACHEABLE=(true)


# update
RUN set -ex \
    # show php version and extensions
    && php -v \
    && php -m \
    && php --ri swoole \
    #  ---------- some config ----------
    && cd /etc/php7 \
    # - config PHP
    && { \
        echo "upload_max_filesize=128M"; \
        echo "post_max_size=128M"; \
        echo "memory_limit=1G"; \
        echo "date.timezone=${TIMEZONE}"; \
    } | tee conf.d/99_overrides.ini \
    # - config timezone
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    # ---------- clear works ----------
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"


WORKDIR /opt/www


COPY . /opt/www
RUN composer install --no-dev -o && php bin/hyperf.php


EXPOSE 9501


ENTRYPOINT ["php", "/opt/www/bin/hyperf.php", "start"]




