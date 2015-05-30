#docker选项格式
##选项的缩写与完整拼写
  命令的某些选项包含缩写和完整拼写两种指定方式，但其作用是一样的，提供完整拼写的目的是为了提高选项的可读性，便于理解。如`-a, --attach`，前者`-a`为缩写方式，后者`--attach`为完整拼写方式。一般情况下，完整拼写方式前缀为两个连接符，缩写方式前缀为一个连接符。  
  >**注意**:如果选项都是缩写方式，多个选项可以合并到一起使用。如`docker run -t -i ubuntu echo hello`与`docker run -ti ubuntu echo hello`是等价的。   

  **选项的值有如果几种：**
##Boolean类型
  一些选项的值格式为`--rm=false`，表示它的值为Boolean类型，并且默认值为false，当指定`--rm`后，则此选项值为true，如果需要指定为false，必须使用`--rm=false`这种方式。

##字符串整数类型
  这是最常见的一种，比如`--name=""`，注意它不表示选项的默认值为""，仅用于说明它的类型。使用时，参数可以是`--name=test`或`--name="test"`，二者是等价的。  

##允许多次指定的类型
  字符串整数类型和Boolean类型的选项只允许被指定一次。而某些参数，如`-p=[]`，允许多次指定，例如，`-p 8080 -p 3306`，表示参数最终的值为8080和3306。
  
#docker daemon命令行
  docker的客户端与服务端是完全相同的文件，区别在于是否以daemon方式启动。当使用client命令时，相应的daemon服务必须先启动。启动daemon服务时，必须指定`-d`选项，还可以通过下面这些参数对daemon服务进行配置。  
  >--api-cors-header=""  
  -b, --bridge=""  
  --bip=""  
  -D, --debug=false  
  --dns=[]  
  --dns-search=[]  
  -e, --exec-driver="native"  
  --fixed-cidr=""   
  --fixed-cidr-v6=""  
  -G, --group="docker"  
  -g, --graph="/var/lib/docker"  
  -H, --host=[]  
  --icc=true  
  --insecure-registry=[]  
  --ip=0.0.0.0  
  --ip-forward=true  
  --ip-masq=true  
  --iptables=true  
  --ipv6=false   
  -l, --log-level="info"  
  --label=[]  
  --log-driver="json-file"
  --mtu=0  
  -p, --pidfile="/var/run/docker.pid"  
  --registry-mirror=[]  
  -s, --storage-driver=""  
  --selinux-enabled=false  
  --storage-opt=[]  
  --tls=false   
  --tlscacert="~/.docker/ca.pem"  
  --tlscert="~/.docker/cert.pem"  
  --tlskey="~/.docker/key.pem"  
  --tlsverify=false
  -v, --version=false  
  --default-ulimit=[]  
  
  `--api-cors-header`表示通过CORS进行跨域调用docker的远程API时，设置的请求头的值。  
  `-b`选项指定守护进程所使用的网桥，所有创建出的容器都通过这个网桥来与外界通信。`--bip`选项指定了这个网桥的IP。其具体细节可以参考[守护进程网络](./docker daemon网络.md)  
  `--dns`和`--dns-search`选项指定守护进程创建容器时的默认DNS服务器和搜索域。  
  `--exec-driver`选项指定运行容器时使用的驱动，目前支持native和lxc两种，其中native是Docker公司采用Go语言开发，直接基于内核和cgroups创建容器的方式，在某些操作系统中可能不受支持。lxc为docker早期版本的默认驱动。  
  `--fixed-cidr`和`--fixed-cidr-v6`指定守护进程创建容器时分配IP的cidr范围，默认为172.17.0.0/16，某些场景中，有可能需要将这个cidr再次进行划分来避免IP冲突。比如，在docker主机A上，指定此cidr为172.17.1.0/24，在docker主机B上，指定此cidr为172.17.2.0/24，为容器的跨主机通信提供支持。  
  `-g`选项指定docker运行时，docker配置、下载的镜像、创建的容器等文件存放的路径。  
  `-H`指定守护进程的docket监听的地址，用于远程调用API，其格式为`-H tcp://0.0.0.0:2375`，可以指定值为`-H tcp://192.168.1.2:2375`来把socket只绑定到某张网卡上。默认情况下，使用2375端口作为无加密访问接口，2376为加密访问接口。  
  `--icc`选项指定时，容器在不使用链接时，容器之间禁止直接使用IP通信。默认情况下，即使没有指定容器连接，容器之间也可以直接通过IP相互访问。  
  `--insecure-registry`指定镜像注册时，允许使用的非加密地址。默认情况下，注册镜像是必须使用TLS(如https)协议来进行传输。在私有registry场景下，可能不需要使用TLS方式通信，则可以通过这个选项来指定私有registry服务是可以使用的，如`--insecure-registry myregistry:5000`指定向myregistry:5000请求下载镜像是允许的。  
  `--ip`选项指定为容器绑定端口时，对应主机的网卡地址，默认情况下，绑定到主机上的所有网卡。  
  `--ip-forward`选项表示启用net.ipv4.ip_forward配置。  
  `--ip-masq`启用IP伪装(IP Masquerade)，使容器隐藏在守护进程后面，不对外暴露。  
  `--iptables`表示通过添加iptables规则来实现容器的网络通信。  
  `--ipv6`表示为容器分配网络时，是否使用ipv6地址。  
  `--label`指定这个守护进程的标签，主要用于集群环境中，多个docker宿主机相互区分的场景下。  
  `--log-driver`指定守护进程创建出的容器的默认log-driver，目前支持json-file和none两种。  
  `--mtu`设置容器的MTU，不指定时，使用默认路由上的MTU，如果默认路由上也没有，则值为1500。  
  `-p`指定守护进程的进程ID文件路径。  
  `--registry-mirror`指定通过守护进程下载镜像时，有限使用的仓库mirror地址，如果所有的mirror中都找不到目标镜像，则从docker官方仓库中下载。  
  `-s`选项指定容器所使用的存储驱动，目前支持aufs、devicemapper、brtfs、overlay四种，这四种存储驱动的细节可以参考[联合文件系统](./目录.md#215-联合文件系统)。`--storage-opt`选项为存储驱动提供额外的配置，提高存储驱动的可定制性，主要用与devicemapper时。    
  `--selinux-enabled`指定容器运行时，是否启用selinux检查，来增强容器的安全性。
  `--tls`、`--tlscacert`、`--tlscert`、`--tlskey`、`--tlsverify`用于设置守护进程与远端通信时的TLS配置，增强通信的安全性。  
  `-v`选项指定输出docker守护进程的版本信息。与客户端的`version`命令输出不同。  
  `--default-ulimit`设置容器的默认ulimit。 
  

#docker client常用命令介绍
##run
  使用指定命令创建新的容器，并运行相应命令。docker最重要的命令之一，其语法为：  
  >docker run [OPTIONS] image [command] [command-params]  

  可用选项为：
  >  -a, --attach=[]  
  --add-host=[]  
  -c, --cpu-shares=0  
  --cap-add=[]  
  --cap-drop=[]  
  --cidfile=""  
  --cpuset-cpus=""  
  -d, --detach=false  
  --device=[]  
  --dns=[]  
  --dns-search=[]  
  -e, --env=[]  
  --entrypoint=""  
  --env-file=[]  
  --expose=[]  
  -h, --hostname=""  
  -i, --interactive=false  
  --ipc=""  
  --link=[]  
  --log-driver=""  
  --lxc-conf=[]  
  -m, --memory=""  
  -l, --label=[]  
  --label-file=[]  
  --mac-address=""  
  --memory-swap=""  
  --name=""  
  --net="bridge"  
  -P, --publish-all=false  
  -p, --publish=[]  
  --pid=""  
  --privileged=false  
  --read-only=false  
  --restart="no"  
  --rm=false  
  --security-opt=[]  
  --sig-proxy=true  
  -t, --tty=false  
  -u, --user=""
  --ulimit=[]      
  -v, --volume=[]   
  --volumes-from=[]  
  -w, --workdir=""  
  
   `-a`选项，指定将容器中的输入输出关联到宿主机的`STDIN`、`STDOUT`、`STDERROR`中，默认情况下，总是将容器的所有输入输出关联到宿主机上。  
   `--add-host`，除了定义的容器link之外，为容器添加额外的自定义主机名、IP映射关系。  
  `-c, --cpu-shares`，为容器指定运行时的CPU加权值。`--cpuset-cpus`选项指定容器运行是使用哪些CPU核心。`-m`选项指定容器运行时的内存限制。`--memory-swap`选项指定容器的内存和交换内存(**swap**)的总和，设置为-1时，则禁止容器使用交换内存。这些参数都是用于对容器使用的计算资源进行限制，具体细节可以参考[容器的计算资源](./目录.md#3-docker的计算资源)。  
  `--cap-add`和`--cap-drop`两个选项分别为容器在镜像本身的基础上，增加或减少Linux能力，主要是与容器的完全性有关，可以参考[docker中的安全增强](./docker中的安全增强.md)。  
   `--cidfile`选项指定将创建出的容器镜像完整ID写入到指定文件中，**注意**：当文件存在时，使用此选项会报错。  
   
   `-d`选项指定容器在后台运行，并将创建出的容器完整ID输出到控制台。  
   `--device`选项指定将宿主机的设备关联到容器上，包括块设备、音频设备、[loop设备]()等设备。这样容器中的应用就可以在不使用`--privileged`设置的情况下，直接访问这些设备。  
   `--dns`选项指定容运行时使用DNS地址。  
   `--dns-search`选项指定容器运行时的的搜索域。  
   `-e`选项以键值对的方式为容器设置环境变量，而`--env-file`选项则允许通过包含多个键值对的文件的方式，同时设置多个环境变量，环境变量之间以**EOL**(回车换行)进行分割。  
  `--entrypoint`选项可以覆盖容器使用的镜像中包含的`ENTRYPOINT`指令，使容器使用其他命令启动。关于`ENTRYPOINT`的作用，参考[Dockerfile指南](./Dockerfile指南.md)。  
  `--expose`选项将容器中的端口暴露给外界，使运行在同一宿主机上的容器可以通过`link`的特殊方式访问容器中的应用，参见[容器链接](./容器链接.md)。  
  `-h`选项为容器指定自定义的主机名，来替换docker自动为容器生成的12位短ID的主机名。  
  `-i`选项指定容器的`STDIN`保持打开状态，使用户可以通过`attach`意外的其他方式，如`nsenter`等工具直接访问容器内部，进行交互。  
  `--ipc`指定容器使用的IPC名字空间，以便多个容器可以使用相同的IPC名字空间进行通信。 `--pid`选项用于指定容器使用的PID名字空间。参见[常用namespace简介](./常用namespace简介.md)。  
  `--link`选项指定运行容器时，创建到其他容器的链接，可以参考[容器链接](./容器链接.md)。**注意**：链接所指向的容器必须是运行中的。  
  `--log-driver`指定容器日志的输出类型。目前版本(1.6)支持的类型有：json-file，syslog，none。默认类型为：json-file，而且只有json-file类型支持docker logs [container]命令。  
  `--lxc-conf`选项为容器指定自定义的lxc配置，它只要在docker的`--exec-driver`使用*lxc*是才有效。  
  `-l`用于为容器指定标签，使`ps`命令执行时有更多的灵活性和可用。设置时，采用键值对的风格进行设置。`--label-file`选项可以为容器指定多个标签，标签之间以**EOL**(回车换行)进行分割。这两个选项的使用方式类似与`-e`与`--env-file`的使用方式，区别在于环境变量在容器运行时，内部可见，而标签则不是。  
  `--mac-address`选项为容器指定mac地址，而不是有docker自动生成。其格式为标准mac地址，例如：92:d0:c6:0a:29:33。  
  `--name`选项用于指定容器的名称，用于替换docker自动生成的随机名称。  
  `--net`选项指定容器的网络模式，支持bridge、host、none、container等四种方式，其中使用container模式时，值为容器的ID或名称，默认为bridge模式。关于这些网络模式的作用，可以参考[容器网络模式](./docker容器网络.md)。  
  `-P`选项在容器使用的镜像通过`EXPOSE`指定了暴露端口的情况下，将这些所有端口都绑定到宿主机上。容器端口与宿主机的端口映射由docker自动生成。  
  `-p`选项指定容器端口与宿主机端口的绑定关系，这样外界可以通过访问宿主机上的相应端口来访问容器中服务。主要有以下几种使用方式：  
  >docker run -p 8080 ubuntu /bin/bash  

  这种情况下，docker自动从宿主机上选择一个可用端口（一般是49153以后的空闲端口）绑定到容器中的8080端口。  
  >docker run -p 8080:8080 ubuntu /bin/bash  

  这种情况下，docker会尝试将宿主机的8080端口绑定到容器中的8080端口上，如果宿主机上的8080端口已经被占用了，则容器创建失败。  
  >docker run -p 192.168.1.1:8080:8080 ubuntu /bin/bash   
  
  这种情况下，docker将宿主机上从指定网卡到宿主机的端口8080请求转换到容器中的8080端口。在宿主机多网卡的中，可以控制哪些流量能够访问容器中的服务，从一定程度上隔离网络。  
  
  `--privileged`选项指定为容器授权。默认情况下，一些比较危险的操作会自动被禁止，例如：
  > docker run -t -i --rm ubuntu bash  
root@bc338942ef20:/# mount -t tmpfs none /mnt  
mount: permission denied  

  `--privileged`选项设置时，容器则可以进行这些操作，例如：
  >docker run --privileged ubuntu bash  
root@50e3f57e16e6:/# mount -t tmpfs none /mnt  
root@50e3f57e16e6:/# df -h  
Filesystem      Size  Used Avail Use% Mounted on  
none            1.9G     0  1.9G   0% /mnt  
  
  `--privileged`选项为容器开放了几乎所有的能力，使容器能够做到主机可以做到的绝大多数功能。在某些特殊场景下，比如说在docker容器中运行docker，它可以使容器能够做到这一点。
  >备注：tmpfs是一种内存文件系统，其默认大小为实际内存的一半。  
  
  `--read-only`选项指定容器自身的文件系统为只读的，但容器使用的数据卷中是可写的。一般与`-v`或`--volumes-from`选项一起使用，来控制容器可以向哪些目录写文件。  
  
  `--restart`选项指定了容器的重启策略。重启策略用于控制docker守护进程在容器退出后的行为。支持的值为no、always、on-failure[:最大重试次数]，默认为no。指定值为always时，无论容器是正常退出或者异常退出，docker守护进程都会自动重启它。指定值为on-failure，当容器异常退出时，docker守护进程会自动尝试指定次数来自动重启容器，当容器正常退出时，则不重启。  
   
  `--rm`选项指定时，退出容器后，docker守护进程会自动删除它。  

  `--security-opt`选项指定容器的安全属性，可以为用户设定个性化的SELinux和AppArmor标签和属性。比如，需要设置一个安全选项，让容器只能监听Apache端口，假设这个策略命名为svirt_apache，那么就可以通过如下命令完成这个需求：  
   >$ docker run --security-opt label:type:svirt_apache -i -t centos \ bash  

  这样做的好处就是，Docker容器不一定要运行在支持SELinux的内核上，也无需用`--privileged`参数运行，避免了很多安全隐患。  
  
  `--sig-proxy`选项指定是否转发所有进行信号给容器中的进程，比如说`SIGKILL`等。  
  `-t`选项为容器分配一个伪tty终端，使外界可以直接通过这个终端与运行中的容器进行交互。  
  `-u`选项指定容器运行时所使用的用户名或UID，参数格式为`<name|uid>[:<group|gid>]`，默认为root用户。**注意**：指定其他用户时，如果镜像中不包含相应用户，则容器创建失败。  
  `-v`选项指定挂载宿主机上的目录到容器中的指定目录上，使容器能够访问主机的文件。参数格式为*hostpath:container_path:[rw|ro]*。其中hostpath为主机上的绝对路径格式的目录，如果此目录不存在，docker守护进程会自动创建对应文件夹。container_path为容器中的对应路径，同样如果容器中不存在指定目录，也会自动创建。rw或ro指定挂在上去的主机目录是否可读，rw表示可读写，ro表示只读。    
  `--volumes-from`指定挂载特定容器上的所有数据卷到新创建出的容器上，使容器之间能够共享文件。参数格式为*容器ID|容器名称:[rw|ro]*，与`-v`选项类似，rw或ro指定挂载的数据卷是否可读写。  
    
  `-w`选项指定容器运行命令的工作目录。默认情况下，运行容器命令的路径为系统的根目录`/`，在需要的时候，可以通过`-w`选项来进行变更。   
  
  `--ulimit`选项在不提高容器特权（如使用`--privileged`）的情况下，为容器指定ulimit配置，如最大网络连接数等。其格式为<type>=<soft limit>[:<hard limit>]。使用示例：  
  >docker run --ulimit nofile=1024:1024 --rm debian ulimit -n  
    1024   
  
  指定容器中的最大连接数为1024。
  >**注意**：如果没有指定hard limit，则使用softlimit作为值，如果不指定，则使用宿主机上守护进程的默认umlimit设置。  
  
  
   
##create
  使用指定命令创建新的容器，但不启动它。其语法与run命令基本一致。
  >docker create [OPTIONS] image [command] [command-params]  

  可用选项与`run`保持一致。
##start
  启动已经关闭的容器。
  >docker start [OPTIONS] CONTAINER [CONTAINER ...]  
  
  可用选项为：
  >-a, --attach=false  
  -i, --interactive=false  
  
  -a选项，是否关联容器的标准输出和标准错误输出（STDIN、STDERR）并转发信号给容器。  
  -i选项，是否关联容器的标准输出（STDIN）到客户端。
  
  `run`命令等价与先`create`，然后再`start`。
   
##stop
  关闭运行中的容器。其语法为：
  >docker stop [OPTIONS] CONTAINER [CONTAINER ...]   
  
   可用选项为：
  >-t, --time=10  
  
  stop容器时，如果容器在-t选项指定的时间内没有成功关闭，则通过kill杀死容器。

##restart
  重启运行中的容器。其语法为：
  >docker restart [OPTIONS] CONTAINER [CONTAINER ...]  
  
  可用选项为：
  >-t, --time=10  
  
  restart命令尝试stop容器，如果容器在-t选项指定的时间内没有成功关闭，通过kill杀死容器，然后再启动容器。
##pause
  通过cgroup freezer挂起容器中的所有进程，这些进行会收到*SIGSTOP*信号。其语法为：
  >docker pause CONTAINER [CONTAINER ...]  
  
##unpause
  通过cgroup freezer取消挂起容器中的所有进程。其语法为：
  >docker pause CONTAINER [CONTAINER ...]  
  
##kill
  发送*SIGKILL*信号给容器中的主进程，来终止容器进程。其语法为：  
  >docker kill [options] CONTAINER [CONTAINER ...]    
    
  可用的选项为  
  >-s, --signal="KILL"  
  
  -s选项指定发送给被杀死的容器进程的信号，默认为*SIGKILL*信号。 

##rm
  删除若干个容器。其语法为：
  >docker rm [options] CONTAINER [CONTAINER ...]    
  
  允许对运行状态的容器进行删除，可用选项为：
  >-f, --force=false  
  -l, --link=false  
  -v, --volumes=false  

  -f选项使用*SIGKILL*信号强行杀死容器进程，删除运行中的容器。  
  -l选项，用于删掉指定的容器连接（***TBD***）。  
  -v选项，用于删除容器使用的卷（***TBD***）。  
  
  
##stats
  输出容器内部的资源使用情况统计，类似与top命令，并不断输出。其语法为：
  >docker stats [OPTIONS] CONTAINER [CONTAINER ...]  
  
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
  >docker attach [OPTIONS] CONTAINER  
  
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

##diff
  比较当前容器相对与容器使用的镜像的文件系统变化。其语法为：
  >docker diff CONTAINER  
  
  列出的修改项为以下三种：
  >+ A：添加文件  
  >+ D：删除文件  
  >+ C：文件内容修改  
  
  
  
##cp  
  复制容器中的指定文件到宿主机上。其语法为：
  >docker cp CONTAINER:CONTAINER_PATH HOSTPATH  
  
  **PS**：此命令对关闭的容器也可以生效。

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
  获取镜像或容器的配置信息。其语法为：
  >docker inspect [OPTIONS] IMAGE/CONTAINER [IMAGE/CONTAINER...]  
  
  可用选项为：
  > -f, --format=""  
  
  -f选项指定显示容器信息的模板，此模板用于过滤获取输出信息的某些特定字段。关于其用法，参考[text/template](http://golang.org/pkg/text/template/)。  

  默认情况下，输出信息显示为JSON格式的字符串。如果通过了-f选项指定了模板，则使用相应模板过滤输出结果。
  例如：
  查询容器的IP地址（*只有运行的容器有IP地址*）:
  >root@ubuntu:~# docker inspect --format='{{.NetworkSettings.IPAddress}}' simulator  
172.17.0.2
 
  更多的示例，可以参考[docker官方命令行文档](https://docs.docker.com/reference/commandline/cli/#inspect)。
  
##rmi
  删除若干个本地的已有镜像。其语法为:
  >docker rmi [OPTIONS] IMAGE [IMAGE...]  

  可用选项为：
  >-f, --force=false  
  --no-prune=false     Do not delete untagged parents    

  删除镜像时，可以通过镜像ID、镜像名称、镜像的标签等多种方式。但**需要注意**，使用镜像ID删除镜像时，如果镜像有多个tag标签，则需要首先把这些标签都删除掉，才能继续删除，否则报出类似下面的错误：
  >Error: Conflict, cannot delete image fd484f19954f because it is tagged in multiple repositories, use -f to force
2013/12/11 05:47:16 Error: failed to remove one or more images  
   
  -f选项指定强行删除镜像，即使镜像包括多个标签。
  --no-prune选项指定为true时，则对没有标签的父镜像也同时删除。


##export
  将容器的文件系统打包成压缩包。其语法为：
  >docker export [OPTIONS] CONTAINER  
  
  可用选项为：
  >-o, --output=""  
  
  默认情况下，export命令将压缩包内容输出到标准输出`STDOUT`，可以通过上面的`-o`选项将其重定向到指定的文件中。  
  实质上相当于将容器的当前文件系统做了快照。

##import
  从指定的压缩包（本地文件或http指定）中导入文件系统，产生一个镜像。本质上是先创建了一个空镜像，然后把压缩包中的内容导入进去。同时可以为产生的镜像打上标签。其语法为：
  >docker import URL|- [REPOSITORY[:TAG]]  
  
  支持的压缩包格式包括.tar, .tar.gz, .tgz, .bzip, .tar.xz, .txz。如果使用URL，则http地址必须指向单个压缩包。也可以使用-来从标准输入`STDIN`中读取压缩包数据。
  可用选项为：
  > -c, --change=[]  
  
  -c选项指定在导入镜像时，需要额外指定的`Dockerfile`指令，支持的指令为：
  >CMD, ENTRYPOINT, ENV, EXPOSE, ONBUILD, USER, VOLUME, WORKDIR  
  
  import和export应当成对使用，类似于对容器操作的save和load，但又有所区别，可以参考
  [export、import和save、load机制的区别](./export、import和save、load机制的区别.md)。

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

