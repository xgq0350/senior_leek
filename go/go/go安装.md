# Ubuntu安装Go
1. 下载安装包 wget https://golang.google.cn/dl/go1.17.3.linux-amd64.tar.gz
2. 解压到指定目录： tar -C  /usr/local  xzf go1.16.3.linux-amd64.tar.gz
3. 设置环境变量 export PATH=$PATH:/usr/local/go/bin
4. export GOPATH=/home/ubuntu/software/go
    export GOBIN=$GOPATH/bin
	GOPATH：代表 Go 语言项目的工作目录，用来存放使用 go get 命令获取的项目。
	GOBIN：代表 Go 编译生成的程序的安装目录，如 go install 命令，会把生成的Go程序安装到GOBIN目录下，以供你在终端使用。
5. 生效环境变量： source /etc/profile
6. 配置 GOPROXY 环境变量： export GOPROXY=https://goproxy.io,direct
   
