---
date: 2017/08/21
---
在使用 `zerotier` 进行内网穿透时，会出现如下报错：  
```bash
ERROR: unable to configure virtual network port: could not open TUN/TAP device: No such file or directory
```

## 检查TUN模块

- 检查是否安装了 `tun` 模块  
```bash
lsmod | grep tun
```

- 若无对应结果，则进行安装  
```bash
insmod /lib/modules/tun.ko
```

## 测试 `tun.ko` 模块是否有效

```bash
mkdir /dev/net
mknod /dev/net/tun c 10 200
chmod 600 /dev/net/tun
cat /dev/net/tun
```
若最后一步 `cat` 后，返回 `File descriptor in bad state` 则表示模块正常工作  

## 模块持久化

此方法在[**zerotier相关**](https://www.notion.so/zerotier-110a3987e31180958fd6ec6fb1003c5e?pvs=21)**`创建一个持续的TUN`**中也已说明，此处就不再补充说明。  

## 完成

重启Nas或者再次执行脚本即可。  