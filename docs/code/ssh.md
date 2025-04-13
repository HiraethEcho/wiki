# 内网ssh连接

一个ipv6地址的电脑，作为中间机，一个内网的电脑，记作服务机，期望其他的客户机（比如iphone）连接到内网的服务机。

## 思路

将服务机的ssh端口转发到中间机的端口，即数据从中间机的端口传送到服务机；客户机连接中间机的端口即可。中间机有ipv6地址，就可以ssh直连。

## 操作

在服务机上

```sh
ssh -R 1234:localhost:22 midle@ipv6
```

将服务机的22端口和中间机的1234端口连接。

在客户机上用

```sh
ssh -p 1234 server_user@ipv6
```

连接，那么数据就从客户机到中间机的1234端口，再转发到服务机的22端口

## 注意

中间机上在`/etc/ssh/sshd_config`（作为服务端的配置）设置

```text
AllowTcpForwarding yes
GatewayPorts yes
```

在服务机 `/etc/ssh/sshd_config`（作为服务端的配置）设置监听端口，默认为22

根据系统设置防火墙

## X through ssh

to share X11 through ssh, need `Xorg` on server and client.  
on client using

```sh
ssh -X -C remote_user@remote_host
```

where `X` for X forwarding, and `-C` for compression. In this ssh session, GUI like `gvim` should show on client.
