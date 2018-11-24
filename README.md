# wireguard
1、wireguard多用户脚本

2、先安装debian_wg_vpn.sh

3、再安装多用户脚本wg5clients.sh

wget -qO- https://raw.githubusercontent.com/zxlhhyccc/wireguard/master/debian_wg_vpn.sh | bash

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
