# 查看
>docker version
>sudo docker info
![](https://img-blog.csdnimg.cn/69a4f23ad55c417bbc93e38b0f39b942.png)
# 镜像命令
   __搜索镜像__
docker search 镜像名
docker search --filter STARS=9000 mysql 
__查看本地镜像__
docker images
__拉取镜像__
docker pull 镜像:tag tag不填就是latest
__删除镜像__
docker rmi -f 镜像名/镜像ID 删除一个镜像
sudo docker rmi -f $(sudo docker images -aq) 删除全部镜像(a显示全部镜像，q只显示id)
__保存镜像__
docker save tomcat > /nginx.tar
__加载镜像__
docker load -i nginx.tar
__创建新的镜像__
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
__DockerFile创建镜像__
docker build -t 镜像名称/次名称：版本号
# 容器命令
__查看正在运行的容器__
docker ps
__查看所有容器__
docker ps -a
__运行容器__
docker run -it --name cq_api -p 9514:9514 -v D:\\folder\\code\\go:/go -d golang  /bin/sh
p:端口映射
v:挂载镜像
__删除容器__
docker rm -f 容器名/容器ID 
docker rm -f $(docker ps -aq)  #删除全部容器
__进入容器__
docker exec -it 容器名/容器ID /bin/bash
docker attach 容器名/容器ID(退出后容器停止)
__容器开启__
docker start 容器名
docker stop 容器名
docker restart 容器名
__容器拷贝文件__
docker cp 容器ID/名称: 容器内路径  容器外路径
docker  cp 容器外路径 容器ID/名称: 容器内路径
__容器日志__
docker logs -f 容器名称
__容器自启动__
docker  update --restart=always 容器Id/容器名
__容器重命名__
docker rename 容器ID/容器名 新容器名
__容器提交成镜像__
docker commit -m="提交信息" -a="作者信息" 容器名/容器ID 提交后的镜像名:Tag
# Docker运维
__查看docker工作目录__
sudo docker info | grep "Docker Root Dir"
__查看docker磁盘占用情况__
du -hs docker工作目录
__查看docker磁盘使用具体__
docker system df
__docker删除停止的容器__
docker rm `docker ps -a | grep Exited | awk '{print $1}'`
__删除名称或标签为none的镜像__
docker rmi -f  `docker images | grep '<none>' | awk '{print $3}'`


# 问题
1. Docker出现exited(127)的解决方法
> 原因：把name的参数放到镜像后面
> 解决：docker run -it -d  -p 80:80 --name kk  nginx:latest
2. Got permission denied while trying to connect to the Docker daemon socket
> 原因：docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问。
> 解决：
> 方法一：使用sudo获取管理员权限，运行docker命令。 sudo docker ps 
> 方法二：测试不成功
> sudo groupadd docker     #添加docker用户组
> sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
> newgrp docker     #更新用户组
3. docker save出错 open ./.docker_temp_xxx: permission denied问题
> 解决：
> sudo docker save 镜像id > 压缩文件名字.tar
