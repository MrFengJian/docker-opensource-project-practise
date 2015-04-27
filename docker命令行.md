#docker daemon命令行

#docker client常用命令介绍
##run
  使用指定命令创建新的容器，并运行相应命令。docker最重要的命令之一，其语法为：  
  >docker run [options] image [command] [command-params]  
  
  
##create
  使用指定命令创建新的容器，但不运行命令。其语法与run命令基本一致。
  >docker create [options] image [command] [command-params]  
##start
  启动容器。
  >docker start [container]  
  使用时，允许
##stop
##restart
##kill
  发送SIGKILL信号给容器，来终止容器进程。其语法为：  
  >docker kill [options] container  
  可用的选项为  
  >
##ps
##images
##inspect
##rm
##rmi
##cp
##export
##import
##exec
##diff
##attach
##diff
##login
  登录到指定的registry服务上，按步骤输入用户名、密码等信息即可。
##pull
  从仓库中拉取或者更新指定的镜像。
##push
##version
  显示docker版本信息，主要包括。其语法为：  
  >docker version  
  
##info
  显示docker守护进程的详细信息，主要包括存储后端驱动类型、本地镜像和容器个数、容器驱动类型等信息。其语法为：  
  >docker info  