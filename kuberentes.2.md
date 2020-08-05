# 二进制部署kubernetes

## 目录

- kubernetes的五个组件
  - master节点的三个组件
    -  <a href="kube-apiserver">kube-apiserver</a>
    - [kube-controller-manager](https://www.cnblogs.com/yanyanqaq/p/12607713.html#kube-controller-manager)
    - [kube-scheduler](https://www.cnblogs.com/yanyanqaq/p/12607713.html#kube-scheduler)
  - node节点的两个组件
    - [kubelet](#kubelet)
    - [kube-proxy](https://www.cnblogs.com/yanyanqaq/p/12607713.html#kube-proxy)
- [1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#1集群架构)
- 2.基础环境准备
  - 2.1.系统设置
    - [2.1.1.设置主机名](https://www.cnblogs.com/yanyanqaq/p/12607713.html#211设置主机名)
    - [2.1.2.关闭防火墙和selinux](https://www.cnblogs.com/yanyanqaq/p/12607713.html#212关闭防火墙和selinux)
    - [2.1.3.设置网卡](https://www.cnblogs.com/yanyanqaq/p/12607713.html#213设置网卡)
    - [2.1.4.设置yum源](https://www.cnblogs.com/yanyanqaq/p/12607713.html#214设置yum源)
    - [2.1.5.安装常用工具](https://www.cnblogs.com/yanyanqaq/p/12607713.html#215安装常用工具)
  - 2.2.安装bind服务
    - [2.2.1.安装bind 9](https://www.cnblogs.com/yanyanqaq/p/12607713.html#221安装bind-9)
    - [2.2.2.配置bind 9](https://www.cnblogs.com/yanyanqaq/p/12607713.html#222配置bind-9)
    - [2.2.3.检查配置并启动bind 9](https://www.cnblogs.com/yanyanqaq/p/12607713.html#223检查配置并启动bind-9)
    - [2.2.4.检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#224检查)
    - [2.2.5.配置DNS客户端](https://www.cnblogs.com/yanyanqaq/p/12607713.html#225配置dns客户端)
    - [2.2.6.检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#226检查)
  - 2.3.准备签发证书环境
    - [2.3.1.安装cfssl](https://www.cnblogs.com/yanyanqaq/p/12607713.html#231安装cfssl)
    - [2.3.2.创建生成ca证书csr的json配置文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#232创建生成ca证书csr的json配置文件)
    - [2.3.3.生成ca证书文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#233生成ca证书文件)
  - 2.4.部署docker
    - [2.4.1.安装](https://www.cnblogs.com/yanyanqaq/p/12607713.html#241安装)
    - [2.4.2.配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#242配置)
    - [2.4.3.启动](https://www.cnblogs.com/yanyanqaq/p/12607713.html#243启动)
  - 2.5.部署docker镜像私有仓库harbor
    - [2.5.1.下载软件并解压](https://www.cnblogs.com/yanyanqaq/p/12607713.html#251下载软件并解压)
    - [2.5.2.配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#252配置)
    - [2.5.3.安装docker-compose](https://www.cnblogs.com/yanyanqaq/p/12607713.html#253安装docker-compose)
    - [2.5.4.安装harbor](https://www.cnblogs.com/yanyanqaq/p/12607713.html#254安装harbor)
    - [2.5.5.检查harbor启动情况](https://www.cnblogs.com/yanyanqaq/p/12607713.html#255检查harbor启动情况)
    - [2.5.6.配置harbor的dns内网解析](https://www.cnblogs.com/yanyanqaq/p/12607713.html#256配置harbor的dns内网解析)
    - [2.5.7.安装NGINX并配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#257安装nginx并配置)
    - [2.5.8.浏览器打开harbor.od.com并测试](https://www.cnblogs.com/yanyanqaq/p/12607713.html#258浏览器打开harborodcom并测试)
    - [2.5.9.检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#259检查)
- 3.部署master节点
  - 3.1.部署etcd集群
    - [3.1.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#311集群架构)
    - [3.1.2.创建基于根证书的config配置文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#312创建基于根证书的config配置文件)
    - [3.1.3.创建生成自签发证书的csr的json配置文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#313创建生成自签发证书的csr的json配置文件)
    - [3.1.4.生成etcd证书文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#314生成etcd证书文件)
    - [3.1.5.检查生成的证书文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#315检查生成的证书文件)
    - [3.1.6.创建etcd用户](https://www.cnblogs.com/yanyanqaq/p/12607713.html#316创建etcd用户)
    - [3.1.7.下载软件，解压，做软连接](https://www.cnblogs.com/yanyanqaq/p/12607713.html#317下载软件，解压，做软连接)
    - [3.1.8.创建目录，拷贝证书文件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#318创建目录，拷贝证书文件)
    - [3.1.9.创建etcd服务启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#319创建etcd服务启动脚本)
    - [3.1.10.授权目录权限](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3110授权目录权限)
    - [3.1.11.安装supervisor软件](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3111安装supervisor软件)
    - [3.1.12.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3112创建supervisor配置)
    - [3.1.13.启动etcd服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3113启动etcd服务并检查)
    - [3.1.14.部署启动所有集群](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3114部署启动所有集群)
    - [3.1.15.检查集群状态](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3115检查集群状态)
  - 3.2.部署kube-apiserver集群
    - [3.2.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#321集群架构)
    - [3.2.2.下载软件，解压，做软连接](https://www.cnblogs.com/yanyanqaq/p/12607713.html#322下载软件，解压，做软连接)
    - [3.2.3.签发client证书](https://www.cnblogs.com/yanyanqaq/p/12607713.html#323签发client证书)
    - [3.2.4.签发kube-apiserver证书](https://www.cnblogs.com/yanyanqaq/p/12607713.html#324签发kube-apiserver证书)
    - [3.2.5.拷贝证书文件至各节点，并创建配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#325拷贝证书文件至各节点，并创建配置)
    - [3.2.6.创建apiserver启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#326创建apiserver启动脚本)
    - [3.2.7.授权和创建目录](https://www.cnblogs.com/yanyanqaq/p/12607713.html#327授权和创建目录)
    - [3.2.8.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#328创建supervisor配置)
    - [3.2.9.启动服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#329启动服务并检查)
    - [3.2.10.部署启动所有集群](https://www.cnblogs.com/yanyanqaq/p/12607713.html#3210部署启动所有集群)
  - 3.3.部署四层反向代理
    - [3.3.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#331集群架构)
    - [3.3.2.安装NGINX和keepalived](https://www.cnblogs.com/yanyanqaq/p/12607713.html#332安装nginx和keepalived)
    - [3.3.3.启动代理并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#333启动代理并检查)
  - 3.4.部署controller-manager
    - [3.4.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#341集群架构)
    - [3.4.2.创建启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#342创建启动脚本)
    - [3.4.3.授权文件权限，创建目录](https://www.cnblogs.com/yanyanqaq/p/12607713.html#343授权文件权限，创建目录)
    - [3.4.4.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#344创建supervisor配置)
    - [3.4.5.启动服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#345启动服务并检查)
    - [3.4.6.部署启动所有集群](https://www.cnblogs.com/yanyanqaq/p/12607713.html#346部署启动所有集群)
  - 3.5.部署kube-scheduler
    - [3.5.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#351集群架构)
    - [3.5.2.创建启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#352创建启动脚本)
    - [3.5.3.授权文件权限，创建目录](https://www.cnblogs.com/yanyanqaq/p/12607713.html#353授权文件权限，创建目录)
    - [3.5.4.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#354创建supervisor配置)
    - [3.5.5.启动服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#355启动服务并检查)
    - [3.5.6.部署启动所有集群](https://www.cnblogs.com/yanyanqaq/p/12607713.html#356部署启动所有集群)
  - 3.6.检查master节点
    - [3.6.1.建立kubectl软链接](https://www.cnblogs.com/yanyanqaq/p/12607713.html#361建立kubectl软链接)
    - [3.6.2.检查master节点](https://www.cnblogs.com/yanyanqaq/p/12607713.html#362检查master节点)
- 4.部署node节点
  - 4.1.部署kubelet
    - [4.1.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#411集群架构)
    - [4.1.2.签发kubelet证书](https://www.cnblogs.com/yanyanqaq/p/12607713.html#412签发kubelet证书)
    - [4.1.3.拷贝证书文件至各节点，并创建配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#413拷贝证书文件至各节点，并创建配置)
    - [4.1.4.准备pause基础镜像](https://www.cnblogs.com/yanyanqaq/p/12607713.html#414准备pause基础镜像)
    - [4.1.5.创建kubelet启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#415创建kubelet启动脚本)
    - [4.1.6.授权，创建目录](https://www.cnblogs.com/yanyanqaq/p/12607713.html#416授权，创建目录)
    - [4.1.7.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#417创建supervisor配置)
    - [4.1.8.启动服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#418启动服务并检查)
    - [4.1.9.部署所有节点](https://www.cnblogs.com/yanyanqaq/p/12607713.html#419部署所有节点)
    - [4.1.10.检查所有节点并给节点打上标签](https://www.cnblogs.com/yanyanqaq/p/12607713.html#4110检查所有节点并给节点打上标签)
  - 4.2.部署kube-proxy
    - [4.2.1.集群架构](https://www.cnblogs.com/yanyanqaq/p/12607713.html#421集群架构)
    - [4.2.2.签发kube-proxy证书](https://www.cnblogs.com/yanyanqaq/p/12607713.html#422签发kube-proxy证书)
    - [4.2.3.拷贝证书文件至各节点，并创建配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#423拷贝证书文件至各节点，并创建配置)
    - [4.2.4.创建kube-proxy启动脚本](https://www.cnblogs.com/yanyanqaq/p/12607713.html#424创建kube-proxy启动脚本)
    - [4.2.5.授权，创建目录](https://www.cnblogs.com/yanyanqaq/p/12607713.html#425授权，创建目录)
    - [4.2.6.创建supervisor配置](https://www.cnblogs.com/yanyanqaq/p/12607713.html#426创建supervisor配置)
    - [4.2.7.启动服务并检查](https://www.cnblogs.com/yanyanqaq/p/12607713.html#427启动服务并检查)
    - [4.2.8.部署所有节点](https://www.cnblogs.com/yanyanqaq/p/12607713.html#428部署所有节点)
- 5.验证kubernetes集群
  - [5.1.在任意一个节点上创建一个资源配置清单](https://www.cnblogs.com/yanyanqaq/p/12607713.html#51在任意一个节点上创建一个资源配置清单)
  - 5.2.应用资源配置，并检查
    - [5.2.1.hdss7-21.host.com上](https://www.cnblogs.com/yanyanqaq/p/12607713.html#521hdss7-21hostcom上)
    - [5.2.2.hdss7-22.host.com上](https://www.cnblogs.com/yanyanqaq/p/12607713.html#522hdss7-22hostcom上)
    - [5.2.3.查看kubernetes是否搭建好](https://www.cnblogs.com/yanyanqaq/p/12607713.html#523查看kubernetes是否搭建好)



#### **kubernetes的五个组件**

##### **master节点的三个组件**

###### **kube-apiserver**

```
整个集群的唯一入口，并提供认证、授权、访问控制、API注册和发现等机制。
```

###### **kube-controller-manager**

```
控制器管理器
负责维护集群的状态，比如故障检测、自动扩展、滚动更新等。保证资源到达期望值。
```

###### **kube-scheduler**

```
调度器
经过策略调度POD到合适的节点上面运行。分别有预选策略和优选策略。
```

##### **node节点的两个组件**

###### **kubelet**

```
在集群节点上运行的代理，kubelet会通过各种机制来确保容器处于运行状态且健康。kubelet不会管理不是由kubernetes创建的容器。kubelet接收POD的期望状态（副本数、镜像、网络等），并调用容器运行环境来实现预期状态。
kubelet会定时汇报节点的状态给apiserver，作为scheduler调度的基础。kubelet会对镜像和容器进行清理，避免不必要的文件资源占用。
```

###### **kube-proxy**

```
kube-proxy是集群中节点上运行的网络代理，是实现service资源功能组件之一。kube-proxy建立了POD网络和集群网络之间的关系。不同node上的service流量转发规则会通过kube-proxy来调用apiserver访问etcd进行规则更新。
service流量调度方式有三种方式：userspace（废弃，性能很差）、iptables（性能差，复杂，即将废弃）、ipvs（性能好，转发方式清晰）。
```

#### **1.集群架构**

|     **主机名**     | **IP地址** |
| :----------------: | :--------: |
| hdss7-11.host.com  | 10.4.7.11  |
| hdss7-12.host.com  | 10.4.7.12  |
| hdss7-21.host.com  | 10.4.7.21  |
| hdss7-22.host.com  | 10.4.7.22  |
| hdss7-200.host.com | 10.4.7.200 |

#### **2.基础环境准备**

##### **2.1.系统设置**

###### **2.1.1.设置主机名**

```
hostnamectl set-hostname hdss7-xx.host.com
```

###### **2.1.2.关闭防火墙和selinux**

```
systemctl stop firewalld
systemctl disable firewalld
setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
```

###### **2.1.3.设置网卡**

```
cat /etc/sysconfig/network-scripts/ifcfg-eth0 
TYPE=Ethernet
BOOTPROTO=none
NAME=eth0
DEVICE=eth0
ONBOOT=yes
IPADDR=10.4.7.xx
NETMASK=255.255.255.0
GATEWAY=10.4.7.254
DNS1=10.4.7.254
```

###### **2.1.4.设置yum源**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo  http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo  http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all
yum makecache
```

###### **2.1.5.安装常用工具**

```
yum install wget net-tools telnet tree nmap sysstat lrzsz dos2unix bind-utils -y
```

##### **2.2.安装bind服务**

**hdss7-11.host.com 上**

###### **2.2.1.安装bind 9**

```
yum install bind -y
```

###### **2.2.2.配置bind 9**

```
vi /etc/named.conf
listen-on port 53 { 10.4.7.11; }; 
allow-query     { any; };
forwarders      { 10.4.7.254; };
recursion yes;
dnssec-enable no;
dnssec-validation no
##########
named-checkconf
vi /etc/named.rfc1912.zones
zone "host.com" IN {
        type  master;
        file  "host.com.zone";
        allow-update { 10.4.7.11; };
};

zone "od.com" IN {
        type  master;
        file  "od.com.zone";
        allow-update { 10.4.7.11; };
};
##########
vi  /var/named/host.com.zone
$ORIGIN host.com.
$TTL 600    ; 10 minutes
@       IN SOA  dns.host.com. dnsadmin.host.com. (
                2020032001 ; serial
                10800      ; refresh (3 hours)
                900        ; retry (15 minutes)
                604800     ; expire (1 week)
                86400      ; minimum (1 day)
                )
            NS   dns.host.com.
$TTL 60 ; 1 minute
dns                A    10.4.7.11
HDSS7-11           A    10.4.7.11
HDSS7-12           A    10.4.7.12
HDSS7-21           A    10.4.7.21
HDSS7-22           A    10.4.7.22
HDSS7-200          A    10.4.7.200
##########
vi  /var/named/od.com.zone
$ORIGIN od.com.
$TTL 600    ; 10 minutes
@           IN SOA  dns.od.com. dnsadmin.od.com. (
                2020032001 ; serial
                10800      ; refresh (3 hours)
                900        ; retry (15 minutes)
                604800     ; expire (1 week)
                86400      ; minimum (1 day)
                )
                NS   dns.od.com.
$TTL 60 ; 1 minute
dns                A    10.4.7.11
```

###### **2.2.3.检查配置并启动bind 9**

```
named-checkconf
systemctl start named
netstat -lntup|grep 53
```

###### **2.2.4.检查**

```
[root@hdss7-11 ~]# dig -t A hdss7-11.host.com @10.4.7.11 +short
10.4.7.11
[root@hdss7-11 ~]# dig -t A hdss7-12.host.com @10.4.7.11  +short
10.4.7.12
[root@hdss7-11 ~]# dig -t A hdss7-21.host.com @10.4.7.11  +short
10.4.7.21
[root@hdss7-11 ~]# dig -t A hdss7-22.host.com @10.4.7.11  +short
10.4.7.22
[root@hdss7-11 ~]# dig -t A hdss7-200.host.com @10.4.7.11  +short
10.4.7.200
```

###### **2.2.5.配置DNS客户端**

**Linux所有主机**

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
DNS1=10.4.7.11
##########
vi /etc/resolv.conf
search host.com
nameserver 10.4.7.11
##########
systemctl restart network
```

**Windows主机**

```
wmnet8网卡更改DNS：10.4.7.11
```

###### **2.2.6.检查**

**Linux**

```
ping www.baidu.com
ping hdss7-200
```

**Windows**

```
ping hdss7-200.host.com
```

##### **2.3.准备签发证书环境**

**hdss7-200.host.com 上**

###### **2.3.1.安装cfssl**

```
wget https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 -O /usr/bin/cfssl
wget https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64 -O /usr/bin/cfssl-json
wget https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64 -O /usr/bin/cfssl-certinfo
chmod +x /usr/bin/cfssl*
```

###### **2.3.2.创建生成ca证书csr的json配置文件**

```
mkdir /opt/certs
vi  /opt/certs/ca-csr.json
{
    "CN": "OldboyEdu",
    "hosts": [
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ],
    "ca": {
        "expiry": "175200h"
    }
}
```

###### **2.3.3.生成ca证书文件**

```
cd /opt/certs
cfssl gencert -initca ca-csr.json | cfssl-json -bare ca
ll
ca.csr  
ca-csr.json  
ca-key.pem
ca.pem
```

##### **2.4.部署docker**

**hdss7-21.host.com，hdss7-22.host.com，hdss7-200.host.com上**

###### **2.4.1.安装**

```
[root@hdss7-21 ~]# curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

###### **2.4.2.配置**

```
mkdir  /etc/docker
vi  /etc/docker/daemon.json
{
  "graph": "/data/docker",
  "storage-driver": "overlay2",
  "insecure-registries": ["registry.access.redhat.com","quay.io","harbor.od.com"],
  "registry-mirrors": ["https://q2gr04ke.mirror.aliyuncs.com"],
  "bip": "172.7.21.1/24",
  "exec-opts": ["native.cgroupdriver=systemd"],
  "live-restore": true
}
##########
bip要根据宿主机ip变化 
注意：hdss7-21.host.com   bip 172.7.21.1/24
     hdss7-22.host.com   bip 172.7.22.1/24
     hdss7-200.host.com  bip 172.7.200.1/24
```

###### **2.4.3.启动**

```
mkdir -p /data/docker
systemctl start docker
systemctl enable docker
docker --version
```

##### **2.5.部署docker镜像私有仓库harbor**

**hdss7-200.host.com 上**

###### **2.5.1.下载软件并解压**

```
harbor官网github地址
https://github.com/goharbor/harbor
[root@hdss7-200 src]# tar xf harbor-offline-installer-v1.8.3.tgz -C /opt/
[root@hdss7-200 opt]# mv harbor/ harbor-v1.8.3
[root@hdss7-200 opt]# ln -s /opt/harbor-v1.8.3/ /opt/harbor
```

###### **2.5.2.配置**

```
[root@hdss7-200 opt]# vi /opt/harbor/harbor.yml
hostname: harbor.od.com
http:
  port: 180
 harbor_admin_password:Harbor12345
data_volume: /data/harbor
log:
    level:  info
    rotate_count:  50
    rotate_size:200M
    location: /data/harbor/logs

[root@hdss7-200 opt]# mkdir -p /data/harbor/logs
```

###### **2.5.3.安装docker-compose**

```
[root@hdss7-200 opt]# yum install docker-compose -y
```

###### **2.5.4.安装harbor**

```
[root@hdss7-200 harbor]# ./install.sh 
```

###### **2.5.5.检查harbor启动情况**

```
[root@hdss7-200 harbor]# docker-compose ps
[root@hdss7-200 harbor]# docker ps -a
```

###### **2.5.6.配置harbor的dns内网解析**

```
[root@hdss7-11 ~]# vi /var/named/od.com.zone
2020032002 ; serial
harbor             A    10.4.7.200
[root@hdss7-11 ~]# systemctl restart named
[root@hdss7-11 ~]# dig -t A harbor.od.com +short
10.4.7.200
```

###### **2.5.7.安装NGINX并配置**

```
[root@hdss7-200 harbor]# yum install nginx -y
[root@hdss7-200 harbor]# vi /etc/nginx/conf.d/harbor.od.com.conf
server {
    listen       80;
    server_name  harbor.od.com;

    client_max_body_size 1000m;

    location / {
        proxy_pass http://127.0.0.1:180;
    }
}
[root@hdss7-200 harbor]# nginx -t
[root@hdss7-200 harbor]# systemctl start nginx
[root@hdss7-200 harbor]# systemctl enable nginx
```

###### **2.5.8.浏览器打开harbor.od.com并测试**

```
[root@hdss7-11 ~]# curl harbor.od.com
```

**1、浏览器输入：harbor.od.com 用户名：admin 密码：Harbor12345**

**2、新建项目：public 访问级别：公开**

**3、下载镜像并给镜像打tag**

```
[root@hdss7-200 harbor]# docker pull nginx:1.7.9
[root@hdss7-200 harbor]# docker images |grep 1.7.9
[root@hdss7-200 harbor]# docker tag 84581e99d807 harbor.od.com/public/nginx:v1.7.9
```

**4、登录harbor并上传到仓库**

```
[root@hdss7-200 harbor]# docker login harbor.od.com
[root@hdss7-200 harbor]# docker push harbor.od.com/public/nginx:v1.7.9
```

###### **2.5.9.检查**

**可以看到NGINX镜像已经上传到public下**

#### **3.部署master节点**

##### **3.1.部署etcd集群**

###### **3.1.1.集群架构**

|      主机名       |  角色  |  ip地址   |
| :---------------: | :----: | :-------: |
| hdss7-12.host.com |  lead  | 10.4.7.12 |
| hdss7-21.host.com | follow | 10.4.7.21 |
| hdss7-22.host.com | follow | 10.4.7.22 |

**部署方法以hdss7-12.host.com为例**

###### **3.1.2.创建基于根证书的config配置文件**

**hdss7-200上**

```
[root@hdss7-200 ~]# vi /opt/certs/ca-config.json
{
    "signing": {
        "default": {
            "expiry": "175200h"
        },
        "profiles": {
            "server": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth"
                ]
            },
            "client": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "client auth"
                ]
            },
            "peer": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth",
                    "client auth"
                ]
            }
        }
    }
} 
```

###### **3.1.3.创建生成自签发证书的csr的json配置文件**

```
[root@hdss7-200 ~]# vi /opt/certs/etcd-peer-csr.json
{
    "CN": "k8s-etcd",
    "hosts": [
        "10.4.7.11",
        "10.4.7.12",
        "10.4.7.21",
        "10.4.7.22"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

###### **3.1.4.生成etcd证书**文件

```
[root@hdss7-200 ~]# cd /opt/certs/
[root@hdss7-200 certs]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=peer etcd-peer-csr.json |cfssl-json -bare etcd-peer
```

###### **3.1.5.检查生成的证书**文件

```
[root@hdss7-200 certs]# ll
etcd-peer.csr
etcd-peer-csr.json
etcd-peer-key.pem
etcd-peer.pem
```

###### **3.1.6.创建etcd用户**

**hdss7-12上**

```
[root@hdss7-12 opt]# useradd -s /sbin/nologin -M etcd
```

###### **3.1.7.下载软件，解压，做软连接**

```
https://github.com/etcd-io/etcd/tags
[root@hdss7-12 src]# tar xf etcd-v3.1.20-linux-amd64.tar.gz -C /opt/
[root@hdss7-12 src]# cd ..
[root@hdss7-12 opt]# mv etcd-v3.1.20-linux-amd64/ etcd-v3.1.20
[root@hdss7-12 opt]# ln -s /opt/etcd-v3.1.20/ /opt/etcd
```

###### **3.1.8.创建目录，拷贝证书文件**

**创建证书目录、数据目录、日志目录**

```
[root@hdss7-12 opt]# mkdir -p /opt/etcd/certs /data/etcd /data/logs/etcd-server
```

**拷贝生成的证书文件**

```
[root@hdss7-12 certs]# scp hdss7-200:/opt/certs/ca.pem .
[root@hdss7-12 certs]# scp hdss7-200:/opt/certs/etcd-peer.pem .
[root@hdss7-12 certs]# scp hdss7-200:/opt/certs/etcd-peer-key.pem .
```

###### **3.1.9.创建etcd服务启动脚本**

```
[root@hdss7-12 ~]# vi /opt/etcd/etcd-server-startup.sh
#!/bin/sh
./etcd --name etcd-server-7-12 \
       --data-dir /data/etcd/etcd-server \
       --listen-peer-urls https://10.4.7.12:2380 \
       --listen-client-urls https://10.4.7.12:2379,http://127.0.0.1:2379 \
       --quota-backend-bytes 8000000000 \
       --initial-advertise-peer-urls https://10.4.7.12:2380 \
       --advertise-client-urls https://10.4.7.12:2379,http://127.0.0.1:2379 \
       --initial-cluster  etcd-server-7-12=https://10.4.7.12:2380,etcd-server-7-21=https://10.4.7.21:2380,etcd-server-7-22=https://10.4.7.22:2380 \
       --ca-file ./certs/ca.pem \
       --cert-file ./certs/etcd-peer.pem \
       --key-file ./certs/etcd-peer-key.pem \
       --client-cert-auth  \
       --trusted-ca-file ./certs/ca.pem \
       --peer-ca-file ./certs/ca.pem \
       --peer-cert-file ./certs/etcd-peer.pem \
       --peer-key-file ./certs/etcd-peer-key.pem \
       --peer-client-cert-auth \
       --peer-trusted-ca-file ./certs/ca.pem \
       --log-output stdout
[root@hdss7-12 ~]# chmod +x /opt/etcd/etcd-server-startup.sh
```

###### **3.1.10.授权目录权限**

```
[root@hdss7-12 ~]# chown -R etcd.etcd /opt/etcd-v3.1.20/   /data/etcd/  /data/logs/etcd-server/
```

###### **3.1.11.安装supervisor软件**

```
[root@hdss7-12 ~]# yum install supervisor -y
[root@hdss7-12 ~]# systemctl start supervisord
[root@hdss7-12 ~]# systemctl enable supervisord
```

###### **3.1.12.创建supervisor配置**

```
[root@hdss7-12 ~]# vi /etc/supervisord.d/etcd-server.ini
[program:etcd-server-7-12]
command=/opt/etcd/etcd-server-startup.sh                        ; the program (relative uses PATH, can take args)
numprocs=1                                                      ; number of processes copies to start (def 1)
directory=/opt/etcd                                             ; directory to cwd to before exec (def no cwd)
autostart=true                                                  ; start at supervisord start (default: true)
autorestart=true                                                ; retstart at unexpected quit (default: true)
startsecs=30                                                    ; number of secs prog must stay running (def. 1)
startretries=3                                                  ; max # of serial start failures (default 3)
exitcodes=0,2                                                   ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                                 ; signal used to kill process (default TERM)
stopwaitsecs=10                                                 ; max num secs to wait b4 SIGKILL (default 10)
user=etcd                                                       ; setuid to this UNIX account to run the program
redirect_stderr=true                                            ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/etcd-server/etcd.stdout.log           ; stdout log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                                    ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                                        ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                                     ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                                     ; emit events on stdout writes (default false)
```

###### **3.1.13.启动etcd服务并检查**

```
[root@hdss7-12 ~]# supervisorctl update
[root@hdss7-12 ~]# supervisorctl status
[root@hdss7-12 ~]# netstat -lntup|grep etcd
```

###### **3.1.14.部署启动所有集群**

**不同的地方**

```
/opt/etcd/etcd-server-startup.sh
--name
--listen-peer-urls
--listen-client-urls
--initial-advertise-peer-urls
--advertise-client-urls
##########
/etc/supervisord.d/etcd-server.ini
[program:etcd-server-7-12]
```

###### **3.1.15.检查集群状态**

```
[root@hdss7-22 etcd]# ./etcdctl  cluster-health
[root@hdss7-22 etcd]# ./etcdctl member list
```

##### **3.2.部署kube-apiserver集群**

###### **3.2.1.集群架构**

|      主机名       |      角色      |  ip地址   |
| :---------------: | :------------: | :-------: |
| hdss7-21.host.com | kube-apiserver | 10.4.7.21 |
| hdss7-22.host.com | kube-apiserver | 10.4.7.22 |

**部署方法以hdss7-21.host.com为例**

###### **3.2.2.下载软件，解压，做软连接**

**hdss7-21.host.com上**

```
https://github.com/kubernetes/kubernetes/releases/tag/v1.15.2
CHANGELOG-1.15.md--→server binaries--→kubernetes-server-linux-amd64.tar.gz
[root@hdss7-21 src]# tar xf kubernetes-server-linux-amd64-v1.15.2.tar.gz  -C /opt
[root@hdss7-21 opt]# mv kubernetes/ kubernetes-v1.15.2
[root@hdss7-21 opt]# ln -s /opt/kubernetes-v1.15.2/ /opt/kubernetes
[root@hdss7-21 opt]# cd kubernetes
[root@hdss7-21 kubernetes]# rm -rf kubernetes-src.tar.gz
[root@hdss7-21 kubernetes]# cd server/bin
[root@hdss7-21 bin]# rm -f *.tar
[root@hdss7-21 bin]# rm -f *_tag
```

###### **3.2.3.签发client证书**

**hdss7-200.host.com上**

**1、创建生成证书csr的json配置文件**

```
[root@hdss7-200 certs]#  vi /opt/certs/client-csr.json
{
    "CN": "k8s-node",
    "hosts": [
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

**2、生成client证书文件**

```
[root@hdss7-200 certs]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=client client-csr.json |cfssl-json -bare client
```

**3、检查生成的证书文件**

```
[root@hdss7-200 certs]# ll
client.csr
client-csr.json
client-key.pem
client.pem
```

###### **3.2.4.签发kube-apiserver证书**

**hdss7-200.host.com上**

**1、创建生成证书csr的json配置文件**

```
[root@hdss7-200 certs]# vi /opt/certs/apiserver-csr.json
{
    "CN": "k8s-apiserver",
    "hosts": [
        "127.0.0.1",
        "192.168.0.1",
        "kubernetes.default",
        "kubernetes.default.svc",
        "kubernetes.default.svc.cluster",
        "kubernetes.default.svc.cluster.local",
        "10.4.7.10",
        "10.4.7.21",
        "10.4.7.22",
        "10.4.7.23"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

**2、生成kube-apiserver证书文件**

```
[root@hdss7-200 certs]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=server apiserver-csr.json |cfssl-json -bare apiserver
```

**3、检查生成的证书文件**

```
[root@hdss7-200 certs]# ll
apiserver.csr
apiserver-csr.json
apiserver-key.pem
apiserver.pem
```

###### **3.2.5.拷贝证书文件至各节点，并创建配置**

**1、拷贝证书文件到/opt/kubernetes/server/bin/cert目录下**

```
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/ca.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/ca-key.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/client.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/client-key.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/apiserver.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/apiserver-key.pem .
```

**2、创建配置**

```
[root@hdss7-21 bin]# mkdir conf
[root@hdss7-21 conf]# vi audit.yaml
apiVersion: audit.k8s.io/v1beta1 # This is required.
kind: Policy
# Don't generate audit events for all requests in RequestReceived stage.
omitStages:
  - "RequestReceived"
rules:
  # Log pod changes at RequestResponse level
  - level: RequestResponse
    resources:
    - group: ""
      # Resource "pods" doesn't match requests to any subresource of pods,
      # which is consistent with the RBAC policy.
      resources: ["pods"]
  # Log "pods/log", "pods/status" at Metadata level
  - level: Metadata
    resources:
    - group: ""
      resources: ["pods/log", "pods/status"]

  # Don't log requests to a configmap called "controller-leader"
  - level: None
    resources:
    - group: ""
      resources: ["configmaps"]
      resourceNames: ["controller-leader"]

  # Don't log watch requests by the "system:kube-proxy" on endpoints or services
  - level: None
    users: ["system:kube-proxy"]
    verbs: ["watch"]
    resources:
    - group: "" # core API group
      resources: ["endpoints", "services"]

  # Don't log authenticated requests to certain non-resource URL paths.
  - level: None
    userGroups: ["system:authenticated"]
    nonResourceURLs:
    - "/api*" # Wildcard matching.
    - "/version"

  # Log the request body of configmap changes in kube-system.
  - level: Request
    resources:
    - group: "" # core API group
      resources: ["configmaps"]
    # This rule only applies to resources in the "kube-system" namespace.
    # The empty string "" can be used to select non-namespaced resources.
    namespaces: ["kube-system"]

  # Log configmap and secret changes in all other namespaces at the Metadata level.
  - level: Metadata
    resources:
    - group: "" # core API group
      resources: ["secrets", "configmaps"]

  # Log all other resources in core and extensions at the Request level.
  - level: Request
    resources:
    - group: "" # core API group
    - group: "extensions" # Version of group should NOT be included.

  # A catch-all rule to log all other requests at the Metadata level.
  - level: Metadata
    # Long-running requests like watches that fall under this rule will not
    # generate an audit event in RequestReceived.
    omitStages:
      - "RequestReceived"
```

###### **3.2.6.创建apiserver启动脚本**

```
[root@hdss7-21 bin]# vi /opt/kubernetes/server/bin/kube-apiserver.sh
#!/bin/bash
./kube-apiserver \
  --apiserver-count 2 \
  --audit-log-path /data/logs/kubernetes/kube-apiserver/audit-log \
  --audit-policy-file ./conf/audit.yaml \
  --authorization-mode RBAC \
  --client-ca-file ./cert/ca.pem \
  --requestheader-client-ca-file ./cert/ca.pem \
  --enable-admission-plugins NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,DefaultTolerationSeconds,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota \
  --etcd-cafile ./cert/ca.pem \
  --etcd-certfile ./cert/client.pem \
  --etcd-keyfile ./cert/client-key.pem \
  --etcd-servers https://10.4.7.12:2379,https://10.4.7.21:2379,https://10.4.7.22:2379 \
  --service-account-key-file ./cert/ca-key.pem \
  --service-cluster-ip-range 192.168.0.0/16 \
  --service-node-port-range 3000-29999 \
  --target-ram-mb=1024 \
  --kubelet-client-certificate ./cert/client.pem \
  --kubelet-client-key ./cert/client-key.pem \
  --log-dir  /data/logs/kubernetes/kube-apiserver \
  --tls-cert-file ./cert/apiserver.pem \
  --tls-private-key-file ./cert/apiserver-key.pem \
  --v 2
```

###### **3.2.7.授权和创建目录**

```
 [root@hdss7-21 bin]# chmod +x kube-apiserver.sh
 [root@hdss7-21 bin]# mkdir -p /data/logs/kubernetes/kube-apiserver
```

###### **3.2.8.创建supervisor配置**

```
[root@hdss7-21 bin]#  vi /etc/supervisord.d/kube-apiserver.ini
[program:kube-apiserver-7-21]
command=/opt/kubernetes/server/bin/kube-apiserver.sh            ; the program (relative uses PATH, can take args)
numprocs=1                                                      ; number of processes copies to start (def 1)
directory=/opt/kubernetes/server/bin                            ; directory to cwd to before exec (def no cwd)
autostart=true                                                  ; start at supervisord start (default: true)
autorestart=true                                                ; retstart at unexpected quit (default: true)
startsecs=30                                                    ; number of secs prog must stay running (def. 1)
startretries=3                                                  ; max # of serial start failures (default 3)
exitcodes=0,2                                                   ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                                 ; signal used to kill process (default TERM)
stopwaitsecs=10                                                 ; max num secs to wait b4 SIGKILL (default 10)
user=root                                                       ; setuid to this UNIX account to run the program
redirect_stderr=true                                            ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/kubernetes/kube-apiserver/apiserver.stdout.log        ; stderr log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                                    ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                                        ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                                     ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                                     ; emit events on stdout writes (default false)
```

###### **3.2.9.启动服务并检查**

```
[root@hdss7-21 bin]# supervisorctl update
[root@hdss7-21 bin]# supervisorctl status
[root@hdss7-21 bin]# netstat -nltup|grep kube-api
```

###### **3.2.10.部署启动所有集群**

**不同的地方**

```
/etc/supervisord.d/kube-apiserver.ini
[program:kube-apiserver-7-21]
```

##### **3.3.部署四层反向代理**

###### **3.3.1.集群架构**

|      主机名       | 角色 |  IP地址   |  VIP地址  |
| :---------------: | :--: | :-------: | :-------: |
| hdss7-11.host.com |  L4  | 10.4.7.11 | 10.4.7.10 |
| hdss7-12.host.com |  L4  | 10.4.7.12 | 10.4.7.10 |

###### **3.3.2.安装NGINX和keepalived**

**1、hdss7-11.host.com和hdss7-12.host.com都安装NGINX和keepalived**

```
[root@hdss7-12 etcd]# yum install nginx keepalived -y
```

**2、hdss7-11.host.com和hdss7-12.host.com配置NGINX**

```
[root@hdss7-11 conf.d]# vi /etc/nginx/nginx.conf
stream {
    upstream kube-apiserver {
        server 10.4.7.21:6443     max_fails=3 fail_timeout=30s;
        server 10.4.7.22:6443     max_fails=3 fail_timeout=30s;
    }
    server {
        listen 7443;
        proxy_connect_timeout 2s;
        proxy_timeout 900s;
        proxy_pass kube-apiserver;
    }
}
[root@hdss7-11 etcd]# nginx -t
```

**3、hdss7-11.host.com和hdss7-12.host.com配置keepalived**

```
检查脚本
[root@hdss7-11 ~]# vi /etc/keepalived/check_port.sh
#!/bin/bash
#keepalived 监控端口脚本
#使用方法：
#在keepalived的配置文件中
#vrrp_script check_port {#创建一个vrrp_script脚本,检查配置
#    script "/etc/keepalived/check_port.sh 6379" #配置监听的端口
#    interval 2 #检查脚本的频率,单位（秒）
#}
CHK_PORT=$1
if [ -n "$CHK_PORT" ];then
        PORT_PROCESS=`ss -lnt|grep $CHK_PORT|wc -l`
        if [ $PORT_PROCESS -eq 0 ];then
                echo "Port $CHK_PORT Is Not Used,End."
                exit 1
        fi
else
        echo "Check Port Cant Be Empty!"
fi

[root@hdss7-11 ~]# chmod +x /etc/keepalived/check_port.sh
##########
配置文件
keepalived 主:
[root@hdss7-11 conf.d]# vi /etc/keepalived/keepalived.conf 

! Configuration File for keepalived

global_defs {
   router_id 10.4.7.11

}

vrrp_script chk_nginx {
    script "/etc/keepalived/check_port.sh 7443"
    interval 2
    weight -20
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 251
    priority 100
    advert_int 1
    mcast_src_ip 10.4.7.11
    nopreempt

    authentication {
        auth_type PASS
        auth_pass 11111111
    }
    track_script {
         chk_nginx
    }
    virtual_ipaddress {
        10.4.7.10
    }
}

keepalived 从:
[root@hdss7-12 conf.d]# vi /etc/keepalived/keepalived.conf 

! Configuration File for keepalived
global_defs {
    router_id 10.4.7.12
}
vrrp_script chk_nginx {
    script "/etc/keepalived/check_port.sh 7443"
    interval 2
    weight -20
}
vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id 251
    mcast_src_ip 10.4.7.12
    priority 90
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 11111111
    }
    track_script {
        chk_nginx
    }
    virtual_ipaddress {
        10.4.7.10
    }
}

nopreempt：非抢占式
```

###### **3.3.3.启动代理并检查**

```
systemctl start nginx keepalived
systemctl enable nginx keepalived
netstat -lntup|grep nginx
ip addr
```

##### **3.4.部署controller-manager**

###### **3.4.1.集群架构**

|      主机名       |        角色        |  IP地址   |
| :---------------: | :----------------: | :-------: |
| hdss7-21.host.com | controller-manager | 10.4.7.21 |
| hdss7-22.host.com | controller-manager | 10.4.7.22 |

**部署方法以hdss7-21.host.com为例**

###### **3.4.2.创建启动脚本**

**hdss7-21.host.com上**

```
[root@hdss7-21 bin]# vi /opt/kubernetes/server/bin/kube-controller-manager.sh
#!/bin/sh
./kube-controller-manager \
  --cluster-cidr 172.7.0.0/16 \
  --leader-elect true \
  --log-dir /data/logs/kubernetes/kube-controller-manager \
  --master http://127.0.0.1:8080 \
  --service-account-private-key-file ./cert/ca-key.pem \
  --service-cluster-ip-range 192.168.0.0/16 \
  --root-ca-file ./cert/ca.pem \
  --v 2 
```

###### **3.4.3.授权文件权限，创建目录**

```
[root@hdss7-21 bin]# chmod +x /opt/kubernetes/server/bin/kube-controller-manager.sh 
[root@hdss7-21 bin]# mkdir -p /data/logs/kubernetes/kube-controller-manager
```

###### **3.4.4.创建supervisor配置**

```
[root@hdss7-21 bin]# vi /etc/supervisord.d/kube-conntroller-manager.ini
[program:kube-controller-manager-7-21]
command=/opt/kubernetes/server/bin/kube-controller-manager.sh                     ; the program (relative uses PATH, can take args)
numprocs=1                                                                        ; number of processes copies to start (def 1)
directory=/opt/kubernetes/server/bin                                              ; directory to cwd to before exec (def no cwd)
autostart=true                                                                    ; start at supervisord start (default: true)
autorestart=true                                                                  ; retstart at unexpected quit (default: true)
startsecs=30                                                                      ; number of secs prog must stay running (def. 1)
startretries=3                                                                    ; max # of serial start failures (default 3)
exitcodes=0,2                                                                     ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                                                   ; signal used to kill process (default TERM)
stopwaitsecs=10                                                                   ; max num secs to wait b4 SIGKILL (default 10)
user=root                                                                         ; setuid to this UNIX account to run the program
redirect_stderr=true                                                              ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/kubernetes/kube-controller-manager/controller.stdout.log  ; stderr log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                                                      ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                                                          ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                                                       ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                                                       ; emit events on stdout writes (default false)
```

###### **3.4.5.启动服务并检查**

```
[root@hdss7-21 bin]# supervisorctl update
[root@hdss7-21 bin]# supervisorctl status
```

###### **3.4.6.部署启动所有集群**

**不同的地方**

```
/etc/supervisord.d/kube-conntroller-manager.ini
[program:kube-controller-manager-7-21]
```

##### **3.5.部署kube-scheduler**

###### **3.5.1.集群架构**

|      主机名       |      角色      |  IP地址   |
| :---------------: | :------------: | :-------: |
| hdss7-21.host.com | kube-scheduler | 10.4.7.21 |
| hdss7-22.host.com | kube-scheduler | 10.4.7.22 |

**部署方法以hdss7-21.host.com为例**

###### **3.5.2.创建启动脚本**

**hdss7-21.host.com上**

```
[root@hdss7-21 bin]# vi /opt/kubernetes/server/bin/kube-scheduler.sh
#!/bin/sh
./kube-scheduler \
  --leader-elect  \
  --log-dir /data/logs/kubernetes/kube-scheduler \
  --master http://127.0.0.1:8080 \
  --v 2
```

###### **3.5.3.授权文件权限，创建目录**

```
[root@hdss7-21 bin]# chmod +x  /opt/kubernetes/server/bin/kube-scheduler.sh
[root@hdss7-21 bin]# mkdir -p /data/logs/kubernetes/kube-scheduler
```

###### **3.5.4.创建supervisor配置**

```
[root@hdss7-21 bin]# vi /etc/supervisord.d/kube-scheduler.ini
[program:kube-scheduler-7-21]
command=/opt/kubernetes/server/bin/kube-scheduler.sh                     ; the program (relative uses PATH, can take args)
numprocs=1                                                               ; number of processes copies to start (def 1)
directory=/opt/kubernetes/server/bin                                     ; directory to cwd to before exec (def no cwd)
autostart=true                                                           ; start at supervisord start (default: true)
autorestart=true                                                         ; retstart at unexpected quit (default: true)
startsecs=30                                                             ; number of secs prog must stay running (def. 1)
startretries=3                                                           ; max # of serial start failures (default 3)
exitcodes=0,2                                                            ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                                          ; signal used to kill process (default TERM)
stopwaitsecs=10                                                          ; max num secs to wait b4 SIGKILL (default 10)
user=root                                                                ; setuid to this UNIX account to run the program
redirect_stderr=true                                                     ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/kubernetes/kube-scheduler/scheduler.stdout.log ; stderr log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                                             ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                                                 ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                                              ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                                              ; emit events on stdout writes (default false)
```

###### **3.5.5.启动服务并检查**

```
[root@hdss7-21 bin]# supervisorctl update
[root@hdss7-21 bin]# supervisorctl status
```

###### **3.5.6.部署启动所有集群**

**不同的地方**

```
/etc/supervisord.d/kube-scheduler.ini
[program:kube-scheduler-7-21]
```

##### **3.6.检查master节点**

###### **3.6.1.建立kubectl软链接**

```
[root@hdss7-21 bin]#  ln -s /opt/kubernetes/server/bin/kubectl /usr/bin/kubectl
```

###### **3.6.2.检查master节点**

```
[root@hdss7-21 bin]# kubectl get cs
```

#### **4.部署node节点**

##### **4.1.部署kubelet**

###### **4.1.1.集群架构**

|      主机名       |  角色   |  IP地址   |
| :---------------: | :-----: | :-------: |
| hdss7-21.host.com | kubelet | 10.4.7.21 |
| hdss7-22.host.com | kubelet | 10.4.7.22 |

**部署方法以hdss7-21.host.com为例**

###### **4.1.2.签发kubelet证书**

**hdss7-200.host.com上**

**1、创建生成证书csr的json配置文件**

```
[root@hdss7-200 certs]# vi kubelet-csr.json
{
    "CN": "k8s-kubelet",
    "hosts": [
    "127.0.0.1",
    "10.4.7.10",
    "10.4.7.21",
    "10.4.7.22",
    "10.4.7.23",
    "10.4.7.24",
    "10.4.7.25",
    "10.4.7.26",
    "10.4.7.27",
    "10.4.7.28"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

**2、生成kubelet证书文件**

```
[root@hdss7-200 certs]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=server kubelet-csr.json | cfssl-json -bare kubelet
```

**3、检查生成的证书文件**

```
[root@hdss7-200 certs]# ll
kubelet.csr
kubelet-csr.json
kubelet-key.pem
kubelet.pem
```

###### **4.1.3.拷贝证书文件至各节点，并创建配置**

**hdss7-21.host.com上**

**1、拷贝证书文件**

```
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/kubelet.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/kubelet-key.pem .
```

**2、创建配置**

**(1)、set-cluster**

```
[root@hdss7-21 conf]# kubectl config set-cluster myk8s \
--certificate-authority=/opt/kubernetes/server/bin/cert/ca.pem \
--embed-certs=true \
--server=https://10.4.7.10:7443 \
--kubeconfig=kubelet.kubeconfig
```

**(2)、set-credentials**

```
[root@hdss7-21 conf]# kubectl config set-credentials k8s-node \
--client-certificate=/opt/kubernetes/server/bin/cert/client.pem \
--client-key=/opt/kubernetes/server/bin/cert/client-key.pem \
--embed-certs=true \
--kubeconfig=kubelet.kubeconfig
```

**(3)、set-context**

```
[root@hdss7-21 conf]# kubectl config set-context myk8s-context \
--cluster=myk8s \
--user=k8s-node \
--kubeconfig=kubelet.kubeconfig
```

**(4)、use-context**

```
[root@hdss7-21 conf]# kubectl config use-context myk8s-context --kubeconfig=kubelet.kubeconfig
```

**(5)、查看生成的kubelet.kubeconfig**

```
[root@hdss7-21 conf]# ll
kubelet.kubeconfig
```

**(6)、k8s-node.yaml**

(1)创建配置文件

```
[root@hdss7-21 conf]# vi k8s-node.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: k8s-node
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:node
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: k8s-node
```

(2)应用资源配置

```
[root@hdss7-21 conf]# kubectl create -f k8s-node.yaml
```

(3)查看集群角色和角色属性

```
[root@hdss7-21 conf]# kubectl get clusterrolebinding k8s-node
[root@hdss7-21 conf]# kubectl get clusterrolebinding k8s-node -o yaml
```

(4)拷贝kubelet.kubeconfig 到hdss7-22.host.com上

```
[root@hdss7-22 conf]# scp hdss7-21:/opt/kubernetes/server/bin/conf/kubelet.kubeconfig .
```

###### **4.1.4.准备pause基础镜像**

**hdss7-200.host.com上**

**1、下载pause镜像**

```
[root@hdss7-200 ~]# docker pull kubernetes/pause
```

**2、上传到docker私有仓库harbor中**

**(1)、给镜像打tag**

```
[root@hdss7-200 ~]# docker images -a
[root@hdss7-200 ~]# docker tag f9d5de079539 harbor.od.com/public/pause:latest
[root@hdss7-200 ~]# docker images -a
```

**(2)、上传到harbor上**

```
[root@hdss7-200 ~]# docker push harbor.od.com/public/pause:latest
```

###### **4.1.5.创建kubelet启动脚本**

**hdss7-21.host.com上**

```
[root@hdss7-21 conf]# vi /opt/kubernetes/server/bin/kubelet.sh
#!/bin/sh
./kubelet \
  --anonymous-auth=false \
  --cgroup-driver systemd \
  --cluster-dns 192.168.0.2 \
  --cluster-domain cluster.local \
  --runtime-cgroups=/systemd/system.slice \
  --kubelet-cgroups=/systemd/system.slice \
  --fail-swap-on="false" \
  --client-ca-file ./cert/ca.pem \
  --tls-cert-file ./cert/kubelet.pem \
  --tls-private-key-file ./cert/kubelet-key.pem \
  --hostname-override hdss7-21.host.com \
  --image-gc-high-threshold 20 \
  --image-gc-low-threshold 10 \
  --kubeconfig ./conf/kubelet.kubeconfig \
  --log-dir /data/logs/kubernetes/kube-kubelet \
  --pod-infra-container-image harbor.od.com/public/pause:latest \
  --root-dir /data/kubelet
```

###### **4.1.6.授权，创建目录**

**hdss7-21.host.com上**

```
[root@hdss7-21 conf]# chmod +x /opt/kubernetes/server/bin/kubelet.sh 
[root@hdss7-21 conf]# mkdir -p /data/logs/kubernetes/kube-kubelet   /data/kubelet
```

###### **4.1.7.创建supervisor配置**

```
[root@hdss7-21 conf]#  vi /etc/supervisord.d/kube-kubelet.ini
[program:kube-kubelet-7-21]
command=/opt/kubernetes/server/bin/kubelet.sh     ; the program (relative uses PATH, can take args)
numprocs=1                                        ; number of processes copies to start (def 1)
directory=/opt/kubernetes/server/bin              ; directory to cwd to before exec (def no cwd)
autostart=true                                    ; start at supervisord start (default: true)
autorestart=true                                  ; retstart at unexpected quit (default: true)
startsecs=30                                      ; number of secs prog must stay running (def. 1)
startretries=3                                    ; max # of serial start failures (default 3)
exitcodes=0,2                                     ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                   ; signal used to kill process (default TERM)
stopwaitsecs=10                                   ; max num secs to wait b4 SIGKILL (default 10)
user=root                                         ; setuid to this UNIX account to run the program
redirect_stderr=true                              ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/kubernetes/kube-kubelet/kubelet.stdout.log   ; stderr log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                      ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                          ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                       ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                       ; emit events on stdout writes (default false)
```

###### **4.1.8.启动服务并检查**

```
[root@hdss7-21 conf]# supervisorctl  update
[root@hdss7-21 conf]# supervisorctl status
```

###### **4.1.9.部署所有节点**

**不同的地方**

```
/opt/kubernetes/server/bin/kubelet.sh
--hostname-override
##########
/etc/supervisord.d/kube-kubelet.ini
[program:kube-kubelet-7-21]
```

###### **4.1.10.检查所有节点并给节点打上标签**

```
[root@hdss7-21 bin]# kubectl get nodes
[root@hdss7-21 bin]# kubectl label node hdss7-21.host.com node-role.kubernetes.io/master=
[root@hdss7-21 bin]# kubectl label node hdss7-21.host.com node-role.kubernetes.io/node=
[root@hdss7-21 bin]# kubectl get nodes
```

##### **4.2.部署kube-proxy**

###### **4.2.1.集群架构**

|      主机名       |    角色    |  IP地址   |
| :---------------: | :--------: | :-------: |
| hdss7-21.host.com | kube-proxy | 10.4.7.21 |
| hdss7-22.host.com | kube-proxy | 10.4.7.22 |

**部署方法以hdss7-21.host.com为例**

###### **4.2.2.签发kube-proxy证书**

**hdss7-200.host.com上**

**1、创建生成证书csr的json配置文件**

```
[root@hdss7-200 certs]# vi kube-proxy-csr.json
{
    "CN": "system:kube-proxy",
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "ST": "beijing",
            "L": "beijing",
            "O": "od",
            "OU": "ops"
        }
    ]
}
```

**2、生成kube-proxy证书文件**

```
[root@hdss7-200 certs]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=client kube-proxy-csr.json |cfssl-json -bare kube-proxy-client
```

**3、检查生成的证书文件**

```
[root@hdss7-200 certs]# ll
kube-proxy-client.csr
kube-proxy-client-key.pem
kube-proxy-client.pem
kube-proxy-csr.json
```

###### **4.2.3.拷贝证书文件至各节点，并创建配置**

**hdss7-21.host.com上**

**1、拷贝证书文件**

```
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/kube-proxy-client.pem .
[root@hdss7-21 cert]# scp hdss7-200:/opt/certs/kube-proxy-client-key.pem .
```

**2、创建配置**

**(1)、set-cluster**

```
[root@hdss7-21 conf]# kubectl config set-cluster myk8s \
--certificate-authority=/opt/kubernetes/server/bin/cert/ca.pem \
--embed-certs=true \
--server=https://10.4.7.10:7443 \
--kubeconfig=kube-proxy.kubeconfig
```

**(2)、set-credentials**

```
[root@hdss7-21 conf]#  kubectl config set-credentials kube-proxy \
  --client-certificate=/opt/kubernetes/server/bin/cert/kube-proxy-client.pem \
  --client-key=/opt/kubernetes/server/bin/cert/kube-proxy-client-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig
```

**(3)、set-context**

```
[root@hdss7-21 conf]# kubectl config set-context myk8s-context \
--cluster=myk8s \
--user=kube-proxy \
--kubeconfig=kube-proxy.kubeconfig
```

**(4)、use-context**

```
[root@hdss7-21 conf]# kubectl config use-context myk8s-context --kubeconfig=kube-proxy.kubeconfig
```

**(5)、拷贝kube-proxy.kubeconfig 到 hdss7-22.host.com的conf目录下**

```
[root@hdss7-22 conf]# scp hdss7-21:/opt/kubernetes/server/bin/conf/kube-proxy.kubeconfig .
```

###### **4.2.4.创建kube-proxy启动脚本**

**hdss7-21.host.com上**

**1、加载ipvs模块**

```
[root@hdss7-21 bin]# lsmod |grep ip_vs
[root@hdss7-21 bin]# vi /root/ipvs.sh
#!/bin/bash
ipvs_mods_dir="/usr/lib/modules/$(uname -r)/kernel/net/netfilter/ipvs"
for i in $(ls $ipvs_mods_dir|grep -o "^[^.]*")
do
  /sbin/modinfo -F filename $i &>/dev/null
  if [ $? -eq 0 ];then
    /sbin/modprobe $i
  fi
done    
[root@hdss7-21 bin]# chmod +x /root/ipvs.sh 
[root@hdss7-21 bin]# sh /root/ipvs.sh 
[root@hdss7-21 bin]# lsmod |grep ip_vs
```

**2、创建启动脚本**

```
[root@hdss7-21 bin]# vi /opt/kubernetes/server/bin/kube-proxy.sh
#!/bin/sh
./kube-proxy \
  --cluster-cidr 172.7.0.0/16 \
  --hostname-override hdss7-21.host.com \
  --proxy-mode=ipvs \
  --ipvs-scheduler=nq \
  --kubeconfig ./conf/kube-proxy.kubeconfig
```

###### **4.2.5.授权，创建目录**

```
[root@hdss7-22 bin]# ls -l /opt/kubernetes/server/bin/conf/|grep kube-proxy
[root@hdss7-22 bin]# chmod +x /opt/kubernetes/server/bin/kube-proxy.sh 
[root@hdss7-22 bin]# mkdir -p /data/logs/kubernetes/kube-proxy
```

###### **4.2.6.创建supervisor配置**

```
[root@hdss7-21 bin]# vi /etc/supervisord.d/kube-proxy.ini
[program:kube-proxy-7-21]
command=/opt/kubernetes/server/bin/kube-proxy.sh                     ; the program (relative uses PATH, can take args)
numprocs=1                                                           ; number of processes copies to start (def 1)
directory=/opt/kubernetes/server/bin                                 ; directory to cwd to before exec (def no cwd)
autostart=true                                                       ; start at supervisord start (default: true)
autorestart=true                                                     ; retstart at unexpected quit (default: true)
startsecs=30                                                         ; number of secs prog must stay running (def. 1)
startretries=3                                                       ; max # of serial start failures (default 3)
exitcodes=0,2                                                        ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                                                      ; signal used to kill process (default TERM)
stopwaitsecs=10                                                      ; max num secs to wait b4 SIGKILL (default 10)
user=root                                                            ; setuid to this UNIX account to run the program
redirect_stderr=true                                                 ; redirect proc stderr to stdout (default false)
stdout_logfile=/data/logs/kubernetes/kube-proxy/proxy.stdout.log     ; stderr log path, NONE for none; default AUTO
stdout_logfile_maxbytes=64MB                                         ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=4                                             ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                                          ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false                                          ; emit events on stdout writes (default false)
```

###### **4.2.7.启动服务并检查**

```
[root@hdss7-21 bin]# supervisorctl update
[root@hdss7-21 bin]# supervisorctl status
[root@hdss7-21 bin]# yum install ipvsadm -y
[root@hdss7-21 bin]# ipvsadm -Ln
[root@hdss7-21 bin]# kubectl get svc
```

###### **4.2.8.部署所有节点**

**不同的地方**

```
/opt/kubernetes/server/bin/kube-proxy.sh
--hostname-override
##########
/etc/supervisord.d/kube-proxy.ini
[program:kube-proxy-7-21]
```

#### **5.验证kubernetes集群**

##### **5.1.在任意一个节点上创建一个资源配置清单**

**hdss7-21.host.com上**

```
[root@hdss7-21 ~]# vi /root/nginx-ds.yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: nginx-ds
spec:
  template:
    metadata:
      labels:
        app: nginx-ds
    spec:
      containers:
      - name: my-nginx
        image: harbor.od.com/public/nginx:v1.7.9
        ports:
        - containerPort: 80
```

##### **5.2.应用资源配置，并检查**

###### **5.2.1.hdss7-21.host.com上**

```
[root@hdss7-21 bin]# kubectl create -f /root/nginx-ds.yaml
[root@hdss7-21 bin]# kubectl get pods
[root@hdss7-21 bin]# kubectl get pods -o wide
[root@hdss7-21 bin]# curl 172.7.21.2
```

###### **5.2.2.hdss7-22.host.com上**

```
[root@hdss7-22 bin]# kubectl get pods
[root@hdss7-22 bin]# kubectl get pods -o wide
[root@hdss7-22 bin]# curl 172.7.22.2
```

###### **5.2.3.查看kubernetes是否搭建好**

```
[root@hdss7-21 bin]# kubectl get cs
[root@hdss7-21 bin]# kubectl get node
[root@hdss7-21 bin]# kubectl get pods
```