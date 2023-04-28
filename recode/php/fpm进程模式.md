



php-fpm的工作模式和nginx类似，都是一个master，多个worker模型。每个worker都在accept本pool内的监听套接字


**ondemand：按连接创建进程数；**




在php-fpm启动的时候，不会给这个pool启动任何一个worker，是按需启动，当有连接过来才会启动。




1. pm.max_children> 0  ;//进程下最大的进程数max_children


2. pm.process_idle_timeout> 0，默认10s。空闲时间大于idle_time时，停止该进程


1s定时器的作用。
找到空闲worker，如果空闲时间超过pm.process\_idle\_timeout大小，关闭。这个机制可能会关闭所有的worker。










**dynamic:初始化启动number进程数；**·




1s定时器的作用。




检查空闲worker数量，按照一定策略动态调整worker数量，增加或减少。增加时，worker最大数量<=max\_children· <=全局process.max；减少时，只有idle >pm.max\_spare\_servers时才会关闭一个空闲worker。






**优点：动态扩容，不浪费系统资源，master进程设置的1秒定时器对系统的影响忽略不计；**


**缺点：如果所有worker都在工作，新的请求到来只能等待master在1秒定时器内再新建一个worker，这时可能最长等待1s；**
启动时创建start_server个数的进程。运行时最小进程数pm.min\_spare\_servers<=pm.max\_spare\_servers，最大空闲进程是max_spare_servers。
start_server如果没有设置。则start_server = 


pm.min\_spare\_servers + (pm.max\_spare\_servers - pm.min\_spare\_servers) / 2




**static：固定启动进程数；**




**php-fpm启动采用固定大小数量的worker，数量为max_children。


