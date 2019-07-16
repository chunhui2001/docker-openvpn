# 下载docker镜像文件
$ git clone https://github.com/chunhui2001/docker-openvpn.git
$ cd docker-openvpn/

# 构建镜像
$ docker build -t myownvpn .
$ cd .. && mkdir vpn-data

# 生成配置文件
$ docker run -v $PWD/vpn-data:/etc/openvpn --rm myownvpn ovpn_genconfig -u udp://你的公网ip:3000

# 初始化pki文件
$ docker run -v $PWD/vpn-data:/etc/openvpn --rm -it myownvpn ovpn_initpki

# 运行vpn服务器
$ docker run -v $PWD/vpn-data:/etc/openvpn -d -p 3000:1194/udp --cap-add=NET_ADMIN myownvpn

# 增加一个用户
$ docker run -v $PWD/vpn-data:/etc/openvpn --rm -it myownvpn easyrsa build-client-full user1 nopass

# 导出新用户秘钥文件
$ docker run -v $PWD/vpn-data:/etc/openvpn --rm myownvpn ovpn_getclient user1 > user1.ovpn

# mac 系统安装 Tunnelblick 客户端
> 将秘钥文件导入 Tunnelblick 即可翻墙
