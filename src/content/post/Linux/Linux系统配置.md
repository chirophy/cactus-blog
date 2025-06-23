---
date: 2025/04/11
---

在 Linux 系统中，查看系统配置通常涉及 **硬件信息**、**操作系统信息**、**网络配置**、**存储信息** 等。以下是常用的命令和工具，帮助你快速获取系统配置信息：

---

## **1. 查看 CPU 信息**

### **1.1 查看 CPU 型号、核心数、架构**

```bash
cat /proc/cpuinfo
```

或精简版：

```bash
lscpu
```

**输出示例**：

```bash
Architecture:        x86_64
CPU(s):              4
Model name:          Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz
...
```

### **1.2 查看 CPU 使用情况**

```bash
top          # 动态查看 CPU 占用
htop         # 更友好的交互式查看（需安装）
mpstat -P ALL  # 查看每个 CPU 核心的使用率
```

---

## **2. 查看内存信息**

### **2.1 查看内存总量、已用、剩余**

```bash
free -h
```

**输出示例**：

```bash
              total        used        free      shared  buff/cache   available
Mem:           15Gi       4.2Gi       8.1Gi       1.1Gi       2.7Gi       9.5Gi
Swap:          2.0Gi       0.0Gi       2.0Gi
```

### **2.2 查看详细内存信息**

```bash
cat /proc/meminfo
```

---

## **3. 查看磁盘信息**

### **3.1 查看磁盘分区及挂载情况**

```bash
df -h
```

**输出示例**：

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p2  100G   30G   70G  30% /
/dev/sda1       500G  200G  300G  40% /data
```

### **3.2 查看磁盘 I/O 性能**

```bash
iostat -x 1  # 查看磁盘 I/O 统计
iotop        # 查看实时磁盘 I/O（需安装）
```

### **3.3 查看所有磁盘设备**

```bash
lsblk
```

**输出示例**：

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0 465.8G  0 disk
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
├─nvme0n1p2 259:2    0 464.3G  0 part /
```

---

## **4. 查看操作系统信息**

### **4.1 查看 Linux 发行版**

```bash
cat /etc/os-release
```

**输出示例**：

```bash
NAME="Ubuntu"
VERSION="22.04 LTS (Jammy Jellyfish)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 22.04 LTS"
VERSION_ID="22.04"
```

### **4.2 查看内核版本**

```bash
uname -a
```

**输出示例**：

```bash
Linux ubuntu-server 5.15.0-76-generic #83-Ubuntu SMP Thu Jun 15 19:16:32 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

---

## **5. 查看网络信息**

### **5.1 查看 IP 地址**

```bash
ip a
```

或：

```bash
ifconfig    # 较老系统（需安装 net-tools）
```

### **5.2 查看路由表**

```bash
ip route
```

或：

```bash
route -n
```

### **5.3 查看开放的端口**

```bash
ss -tulnp  # 推荐（替代 netstat）
```

或：

```bash
netstat -tulnp  # 较老系统
```

---

## **6. 查看系统启动时间 & 负载**

### **6.1 查看系统运行时间**

```bash
uptime
```

**输出示例**：

```bash
 15:30:45 up 10 days,  3:20,  2 users,  load average: 0.15, 0.10, 0.05
```

### **6.2 查看系统负载**

```bash
cat /proc/loadavg
```

**输出示例**：

```bash
0.15 0.10 0.05 1/500 12345
```

- 前 3 个数字分别代表 **1分钟、5分钟、15分钟** 的平均负载。
- 第 4 个数字表示 **正在运行的进程数/总进程数**。

---

## **7. 查看系统日志**

### **7.1 查看内核日志**

```bash
dmesg
```

### **7.2 查看系统日志（journalctl）**

```bash
journalctl -xe   # 查看最新日志
journalctl -u nginx --since "2024-01-01"  # 查看 nginx 服务日志
```

---

## **8. 查看 PCI/USB 设备**

### **8.1 查看 PCI 设备（如显卡、网卡）**

```bash
lspci
```

**输出示例**：

```bash
00:00.0 Host bridge: Intel Corporation 8th Gen Core Processor Host Bridge/DRAM Registers (rev 07)
00:02.0 VGA compatible controller: Intel Corporation UHD Graphics 630 (Mobile)
```

### **8.2 查看 USB 设备**

```bash
lsusb
```

---

## **9. 查看系统环境变量**

```bash
env
```

或查看特定变量：

```bash
echo $PATH
```

---

## **10. 查看系统进程**

### **10.1 查看所有进程**

```bash
ps aux
```

### **10.2 查看特定进程**

```bash
pgrep nginx    # 查找 nginx 进程 ID
ps -p 1234 -o %cpu,%mem,cmd  # 查看进程 1234 的 CPU、内存占用
```

---

## **总结**

|**需求**|**命令**|
|---|---|
|**CPU 信息**|`lscpu`, `cat /proc/cpuinfo`|
|**内存信息**|`free -h`, `cat /proc/meminfo`|
|**磁盘信息**|`df -h`, `lsblk`, `iostat`|
|**操作系统信息**|`cat /etc/os-release`, `uname -a`|
|**网络信息**|`ip a`, `ss -tulnp`, `ip route`|
|**系统负载**|`uptime`, `cat /proc/loadavg`|
|**日志查看**|`dmesg`, `journalctl`|
|**硬件设备**|`lspci`, `lsusb`|
|**进程管理**|`ps aux`, `top`, `htop`|

如果需要更详细的报告，可以使用：

```bash
sudo lshw    # 列出所有硬件信息（需安装）
sudo dmidecode  # 查看 BIOS 和主板信息
```

这些命令可以帮助你全面了解 Linux 系统的配置和运行状态！ 🚀