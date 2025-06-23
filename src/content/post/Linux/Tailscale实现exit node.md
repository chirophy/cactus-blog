---
date: 2025/01/22
---
## 前言

默认情况下，Tailscale 充当覆盖网络：它只路由运行 Tailscale 的设备之间的流量，但不接触您的公共互联网流量，例如您访问 Google 或 Twitter 时的流量。对于需要在敏感设备（如公司服务器或家用电脑）之间进行安全通信，但不需要额外的加密层或公共互联网连接延迟的大多数人来说，覆盖网络配置是理想之选。

![image.png](https://4b5aa40.webp.li/default-behavior.png)

不过，有时可能希望 Tailscale 路由公共互联网流量。

![image.png](https://4b5aa40.webp.li/use-exitnode.png)

通过将网络上的一台设备设置为出口节点，可以路由所有公共互联网流量。当你通过出口节点路由所有流量时，你实际上是在使用默认路由（ `0.0.0.0/0` , `::/0` ），类似于使用典型的 VPN。

## 作用

如此来说，我们可以通过此种办法实现另类的科学上网。

## 实现

### Linux 系统有 `/etc/sysctl.d` 目录

```bash
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
```

### 否则，请使用

```bash
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf
```

### 如果使用了`firewalld` ，需要

```bash
firewall-cmd --permanent --add-masquerade
```

完事之后在设置一下`Tailscale`服务器即可。

```bash
sudo tailscale up --advertise-exit-node
```

如此这般，便在服务器上配置成功了，之后再在网页管理端设置即可。

![image.png](https://4b5aa40.webp.li/tailscale-exitnode.png)