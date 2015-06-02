#概述
  默认情况下，docker daemon守护进程在启动时，会自动初始化网络环境，为docker容器通信提供支持。而docker提供了多个选项来控制创建出的网络的行为。
  主要为以下一个选项：
#相关daemon选项
  >-b, --bridge=""  
  --bip=""  
  --ip-forward=true  
  --ip-masq=true  
  --iptables=true  
  --icc=true  
  
  以上选项中：
  + `--iptables`选项确保Docker对于宿主机上的iptables规则拥有添加权限，使容器链接通信成为可能；  
  + `--ip-forward`选项确保net.ipv4.ip_forward可以使用，使得多网络接口设备模式下，数据报可以在网络设备之间转发；  
  + `--bip`选项在docker daemon进程启动过程中，为网络环境中的网桥配置CIDR网络地址；   + `-b, --bridge`选项为Docker网络环境指定具体的通信网桥，若选项的值为”none”，则说明不需要为Docker Container创建网桥服务，关闭Docker Container的网络能力；  
  + `--icc`选项确保Docker容器之间在没有容器链接的情况下也可以完成通信。  
