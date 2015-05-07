#docker daemon命令行

#docker client常用命令介绍
##run
  使用指定命令创建新的容器，并运行相应命令。docker最重要的命令之一，其语法为：  
  >docker run [OPTIONS] image [command] [command-params]  
  
  
##create
  使用指定命令创建新的容器，但不运行命令。其语法与run命令基本一致。
  >docker create [OPTIONS] image [command] [command-params]  
##start
  启动已经关闭的容器。
  >docker start [OPTIONS] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]  
  
  可用选项为：
  >-a, --attach=false  
  -i, --interactive=false  
  
  -a选项，是否关联容器的标准输出和标准错误输出（STDIN、STDERR）并转发信号给容器。  
  -i选项，是否关联容器的标准输出（STDIN）到客户端。
   
##stop
  关闭运行中的容器。其语法为：
  >docker stop [OPTIONS] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]   
  
   可用选项为：
  >-t, --time=10  
  
  stop容器时，如果容器在-t选项指定的时间内没有成功关闭，则通过kill杀死容器。

##restart
  重启运行中的容器。其语法为：
  >docker restart [OPTIONS] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]  
  
  可用选项为：
  >-t, --time=10  
  
  restart命令尝试stop容器，如果容器在-t选项指定的时间内没有成功关闭，通过kill杀死容器，然后再启动容器。
##pause
  通过cgroup freezer挂起容器中的所有进程，这些进行会收到*SIGSTOP*信号。其语法为：
  >docker pause CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]  
  
##unpause
  通过cgroup freezer取消挂起容器中的所有进程。其语法为：
  >docker pause CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]  
  
##kill
  发送*SIGKILL*信号给容器中的主进程，来终止容器进程。其语法为：  
  >docker kill [options] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]    
    
  可用的选项为  
  >-s, --signal="KILL"  
  
  -s选项指定发送给被杀死的容器进程的信号，默认为*SIGKILL*信号。 

##rm
  删除若干个容器。其语法为：
  >docker rm [options] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]    
  
  允许对运行状态的容器进行删除，可用选项为：
  >-f, --force=false  
  -l, --link=false  
  -v, --volumes=false  

  -f选项使用*SIGKILL*信号强行杀死容器进程，删除运行中的容器。  
  -l选项，用于删掉指定的容器连接（***目前没有实现功能***）。  
  -v选项，用于删除容器使用的卷（***目前没有实现功能***）。  
  
  
##stats
  输出容器内部的资源使用情况统计，类似与top命令，并不断输出。其语法为：
  >docker stats [OPTIONS] CONTAINER_ID/CONTAINER_NAME [CONTAINER_ID/CONTAINER_NAME ...]  
  
  **只对运行状态的容器有效**。
##ps
  列出容器。其语法为：
  >docker ps [OPTIONS]  
  
  可用选项为：
  >-a, --all=false  
  --before=""       
  -f, --filter=[]  
  -l, --latest=false  
  -n=-1               
  --no-trunc=false  
  -q, --quiet=false  
  -s, --size=false  
  --since=""  
  
  -a选项，指定是否列出所有容器，默认只列出运行中的容器。  
  --before、--since选项，指定列出某个ID对应容器创建前和创建后的所有容器。  
  --no-trunc选项，指定是对容器的ID进行截断处理。容器ID为64位字符串，默认截断为12位。  
  -q选项，指定只显示容器ID，而不显示其他容器信息。  
  -l选项，显示**最近创建**的容器，包括非运行状态的容器。  
  -n选项，显示**最近创建的n个**容器，-1表示不使用这个选项。

##exec
  在执行中的容器中运行命令，其语法为：
  >docker exec [OPTIONS] CONTAINER COMMAND [ARG...]  
  
##attach
  关联进入容器的会话，与容器中进程进行交互。其语法为：
  >docker attach [OPTIONS] CONTAINER_ID/CONTAINER_NAME  
  
  使用此命令时，要求容器是运行中的，并打开了交互会话。可用选项为：
  >--no-stdin=false  
  --sig-proxy=true  
  
  --no-stdin选项指定attach会话是是否关联标准输入。  
  --sig-proxy表示是否将所有进程信号转发给容器的主进程(**即进程号为1的进程**)。  
 
  如果要退出attach会话，有两种方式：  

  + ctrl p后按ctrl q，这种方式只退出会话，容器依然保持运行状态。   
  
  + ctrl c，这种方式将通过发送*SIGKILL*信号给容器的方式杀死容器。  
  
  >**PS**:如果容器使用它-t选项（启用tty模拟终端）方式运行，则使用attach命令时，禁止将attach命令的输出进行重定向，以避免安全问题。  
  

##rename
  修改容器名称。其语法为：
  >docker rename OLD_NAME NEW_NAME  
  
##images
  查询docker主机本身已经下载的镜像信息。其语法为：
  >docker images [OPTIONS] [REPOSITORY]  
  
  可用选项为：
  >-a, --all=false
  -f, --filter=[]      Provide filter values (i.e., 'dangling=true')  
  --help=false         Print usage  
  --no-trunc=false
  -q, --quiet=false    Only show numeric IDs  

  -a选项，由于docker的镜像分层按照树状进行组织，-a选项指定时，显示中间层的镜像，否则只显示叶子节点（即最终输出的镜像）。  
  --no-trunc选项，对镜像的ID不进行截断处理。原始镜像ID长度为64位，为显示方便，默认截断为12位。  
  -q选项，指定时，只显示镜像的ID，而不显示镜像名称、仓库、标签等详细信息。  
  -f选项，使用过滤器来对本地镜像进行过滤。目前可用的过滤器只有两个：
  >+ dangling：值为true或false，表示镜像是否是dangling的（**TBD，官方没有说明**）    
  >+ label：使用镜像标签进行过滤，方式为label=<key>或label=<key>=<value>。  
  

##inspect
  
##rmi
##cp
##export
##import
##diff

##login
  登录到指定的registry服务上，按步骤输入用户名、密码等信息即可。如果没有指定，则访问docker官方registry服务器https并://index.docker.io/v1/。
##logout
  从指定的registry服务上登出，不指定时，则访问docker官方registry服务器https://index.docker.io/v1/。
##pull
  从指定仓库中拉取或者更新指定的镜像。其语法为：
  >docker pull [OPTIONS] NAME[:TAG]  
  
  可用选项为：
  >-a, --all-tags=false   
  
  -a选项指定是或否下载指定名称仓库中的所有tag的镜像。
  默认从docker hub官方仓库(registry.hub.docker.com)中下载，如果需要从其他仓库中下载，则在NAME前面增加仓库位置。例如：
  >docker pull 192.168.1.100:5000/centos:centos6  
  
##push
  上传镜像到指定仓库中。其语法为：
  >docker push NAME[:TAG]
  
  默认上传到docker hub官方仓库(registry.hub.docker.com)，如果需要上传到其他仓库，则在NAME前带上仓库位置。例如：
  >docker pull 192.168.1.100:5000/centos:centos6  
  
##version
  显示docker版本信息，主要包括docker client、docker daemon、go语言、git版本等信息。其语法为：  
  >docker version  
  
  输出类似如下内容
>root@ubuntu:~# docker version  
Client version: 1.6.0  
Client API version: 1.18  
Go version (client): go1.4.2  
Git commit (client): 4749651  
OS/Arch (client): linux/amd64  
Server version: 1.6.0  
Server API version: 1.18    
Go version (server): go1.4.2  
Git commit (server): 4749651  
OS/Arch (server): linux/amd64  

  
##info
  显示docker守护进程的详细信息，主要包括存储后端驱动类型、本地镜像和容器个数、容器驱动类型等信息。其语法为：  
  >docker info  
  
  输出类似如下内容：
  >root@ubuntu:~# docker info  
  Containers: 3  
  Images: 57  
  Storage Driver: aufs  
  Root Dir: /var/lib/docker/aufs  
  Backing Filesystem: extfs  
  Dirs: 63  
  Dirperm1 Supported: false  
  Execution Driver: native-0.2  
  Kernel Version: 3.13.0-46-generic  
  Operating System: Ubuntu 14.04.2 LTS  
  CPUs: 2  
  Total Memory: 3.861 GiB  
  Name: ubuntu  
  ID: ZIVL:QRFH:4PSA:AKCW:TKLD:PF7D:N4QD:YQOF:YEFX:2RFH:56UU:C7MW  

