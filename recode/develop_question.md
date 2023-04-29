
## ubuntu
1. kswapd0占用CPU过高问题处理
    
    
kswapd0 是 Linux 系统虚拟内存管理中负责换页的进程。系统物理内存不足时，kswapd0 会频繁的进行换页操作（使用swap分区与内存换页操作交换数据），而换页操作非常消耗 CPU 资源，所以导致该进程持续占用 CPU 资源过高。

> vm.swappiness 内核参数来控制交换空间的大小。
设置vm.swappiness=0 ，告诉内核尽量少用到swap分区，但不代表禁用swap分区；
设置vm.swappiness=100的话，则表示尽量使用swap分区。

cat /proc/sys/vm/swappiness  //查看vm.swappiness
sysctl vm.swappiness=0    //临时修改vm.swappiness
// 永久调整
> vi /etc/sysctl.conf
    #最后一行加上
    vm.swappiness=0
   sysctl -p  //使得修改生效

1.在docker里安装ps命令（apt install procps）
出错提示：Unable to locate package procps

解决：apt-get update