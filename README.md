# wireguard

先更新内核到最新稳定版，可以到：

http://kernel.ubuntu.com/~kernel-ppa/mainline下载稳定版本手动安装

也可以使用一键脚本升级并同时开启BBR：

wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/bbr.sh && chmod +x bbr.sh && bash bbr.sh

注意：

一键脚本升级后不带内核，需到上面的地址下载对应的版本内核手动升级：

如：4.9.140版本（下载两个）：

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9.140/linux-headers-4.9.140-0409140_4.9.140-0409140.201811231231_all.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9.140/linux-headers-4.9.140-0409140-lowlatency_4.9.140-0409140.201811231231_amd64.deb

再用安装命令：dpkg -i 文件名

1、wireguard多用户脚本

2、先安装debian_wg_vpn.sh

3、再安装多用户脚本wg5clients.sh

wget -qO- https://raw.githubusercontent.com/zxlhhyccc/wireguard/master/debian_wg_vpn.sh | bash（该一键脚本会先安装单用户然后自动安装多用户，不需单独下载安装多用户脚本）

4、本脚本为2个用户，如果要添加N个用户，同步修改用户数和端口数，可以自定义修改端口

5、一键脚本运行完成后，做以下步骤：

允许转发在VPN通道内的数据包：

iptables -A FORWARD -i wg0 -o wg0 -m conntrack --ctstate NEW -j ACCEPT

设置NAT：

iptables -t nat -A POSTROUTING -s 10.80.80.0/24 -o eth0 -j MASQUERADE

6、安装iptables-persistent使设置在重启后保持有效：

apt-get install iptables-persistent（安装时提示y/n，选择：y）

systemctl enable netfilter-persistent

netfilter-persistent save

7、优化系统配置：

使用脚本进行优化（运行脚本选择优化配置即可）：

cd /usr/src && wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh

设置后运行：sysctl -p

说明：

1、如果要升级内核，请先停止wireguard，命令：wg-quick down wg0

2、内核升级后再运行：wg-quick up wg0

3、升级内核可以手动也可以使用上面的脚本，使用脚本仅升级了系统未升级内核，需按照上面的步骤手动安装新的内核

4、更新系统和安装新的内核后，用命令查看一下系统和内核，命令：

dpkg -l|grep linux-image

dpkg -l|grep linux-headers

用dpkg -l|grep linux-headers查看内核可能有旧版内核，需要卸载，命令：

apt-get remove --purge 旧内核名称  -y（如有提示y/n，选择n）

5、其他：

查看升级包：apt list --upgradable

修复内核：apt --fix-broken install

6、centos7多用户脚本，默认同步安装2用户，如果需要增加用户，请按照上面要求修改

wget -qO- https://raw.githubusercontent.com/zxlhhyccc/wireguard/master/centos7-wg-vpn.sh | bash
