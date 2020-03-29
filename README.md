# Build-vpn
# 快速搭建ssr服务端 (如果想用国外网络使用国外服务器,反之使用国内服务器)
#### 昨天同事问我有没有好的vpn可以推荐,就推荐了ssr,然后查了资料看下ssr服务端如何搭建,正好手上还有一台center os系统的服务器没到期.搭建vpn使用现有的搬瓦工的服务器进行搭建.服务器系统center os系统

#### 用于TCP加速,不是必需,但强烈建议安装，4.9以上的内核版本才能支持bbr，CentOS 6默认为2.6内核，显然不能支持bbr，需要更新内核。
#### 1 如果已经安装过bbr检查是否是最新的 yum update -y 没有安装使用 `1yum -y install wget && wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh` 进行安装
#### 2 安装完成后，脚本会提示需要重启 VPS，输入 y 并回车后重启。
#### 3 检查已经安装的内核 `awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg `
#### 4 使用大于4.9的内核 `grub2-set-default X` (x 检查内核会出现排序的内核版本,填写对应的数字)
#### 5 重启后自动使用最新的内核。 reboot
#### 6 重启后，登录服务器，输入以下命令检查当前内核： `uname -r` 只要内核版本高于4.9都可以，比如4.10/4.11/4.12等等。
#### 7 验证启用情况：`sysctl net.ipv4.tcp_available_congestion_control`
#### 8 输出结果(`net.ipv4.tcp_available_congestion_control = reno cubic bbr`)显示带有bbr的，表明正在启用bbr:sysctl net.ipv4.tcp_congestion_control
#### 9 安装额外epel仓库 `rpm -ivh https://mirrors.aliyun.com/epel/epel-release-latest-6.noarch.rpm` 或者 `yum -y install epel-release`
#### 10 安装shadowsocks这里推荐一键安装进行配置:`wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh`  `chmod +x shadowsocks.sh`  `./shadowsocks.sh 2>&1 | tee shadowsocks.log` 提示设置密码、TCP连接端口号、加密方式进行配置即可 
#### 11 单用户环境已经安装成功显示如下
#### 12 ![avatar](https://github.com/jacky1376130023/Build-vpn/blob/master/image/watch_img.png)
#### 13 如果想配置多用户需要编辑 shadowsocks.json 文件
#### 14 查看配置文件 `cat /etc/shadowsocks.json` 进行修改 `vi /etc/shadowsocks.json` 文件
#### 15 删除原有的shadowsocks.json文件,添加一下文件

`{
    "server":"这里写本机公网IP",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "fast_open": false,
    "port_password": {
        "9001": "user1",
        "9002": "user2",
        "9003": "user3",
        "9004": "user4",
        "9005": "user5",
        "9006": "user6",
        "9007": "user7",
        "9008": "user8",
        "9009": "user9",
        "9010": "user10",
        "9011": "user11",
        "9012": "user12",
        "9013": "user13",
        "9014": "user14"
    },
    "timeout": 300,
    "method": "chacha20-ietf"
}`


#### 16 安装shadowsocks依赖关系 `yum -y install libsodium`
#### 17 重启shadowsocks使配置生效：`/etc/init.d/shadowsocks restart`
#### 18 防火墙开放相关端口：`iptables -I INPUT -p tcp --dport 9001:9014 -j ACCEPT` 如果是单用户的话 端口号设置安装配置时的端口
#### 19 保存防火墙配置并重载配置：`/etc/init.d/iptables save && service iptables reload`
#### 20 启动shadowsocks  `/etc/init.d/shadowsocks start`
#### 21 如果以上没有问题,ssserver应该可以用了，请在客户端(比如Windows系统客户端)配置目标服务器IP、端口、密码、加密的方式，必须与服务器的配置一致。如下
## 客户端配置(依mac为例)
#### 22 ![avatar](https://github.com/jacky1376130023/Build-vpn/blob/master/image/setting_vpn_servers.png)
#### 23 ![avatar](https://github.com/jacky1376130023/Build-vpn/blob/master/image/open_vpn.png)
#### 24 用百度直接查询ip地址 如下说明已经配置成功 ![avatar](https://github.com/jacky1376130023/Build-vpn/blob/master/image/vpn_ss.png)
#### 25 shadowsocks服务管理
#### 卸载shadowsocks

#### [root@zcwyou ~]# `./shadowsocks.sh uninstall`
#### 启动shadowsocks

#### [root@zcwyou ~]# `/etc/init.d/shadowsocks start`
#### 停止shadowsocks

#### [root@zcwyou ~]# `/etc/init.d/shadowsocks stop`
#### 重启shadowsocks

#### [root@zcwyou ~]# `/etc/init.d/shadowsocks restart`
#### 查看shadowsocks状态

#### [root@zcwyou ~]# `/etc/init.d/shadowsocks status`


 
