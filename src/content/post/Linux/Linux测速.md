---
date: 2025/04/11
---
在 Linux 云服务器上测试带宽，可以使用多种工具和方法，包括 **本地测速工具**（如 `iperf3`、`speedtest-cli`）和 **远程测速服务**（如 `wget` 下载大文件）。以下是详细的测试方案：

---

## **1. 测试下载/上传带宽**

### **1.1 使用 `speedtest-cli`（推荐）**

Speedtest 是 Ookla 提供的测速工具，适用于测试公网带宽。

### **安装 `speedtest-cli`**

```bash
# Ubuntu/Debian
sudo apt update && sudo apt install -y speedtest-cli

# CentOS/RHEL
sudo yum install -y speedtest-cli

# 或使用 Python pip 安装（最新版）
sudo apt install -y python3-pip
pip3 install speedtest-cli

```

### **运行测速**

```bash
speedtest-cli
```

**输出示例**：

```bash
Retrieving speedtest.net configuration...
Testing from Tencent Cloud (1.2.3.4)...
Hosted by China Telecom (Shanghai) [10.00 km]: 5.123 ms
Download: 98.76 Mbit/s
Upload: 49.32 Mbit/s
```

### **仅测试下载/上传**

```bash
speedtest-cli --no-upload    # 仅测下载
speedtest-cli --no-download  # 仅测上传
```

---

### **1.2 使用 `wget` 下载大文件测试下载带宽**

```bash
# 测试下载速度（以 Ubuntu 官方镜像为例）
wget -O /dev/null <http://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso>
```

**输出示例**：

```bash
100%[====================>] 3.5G  10.2MB/s   in 5m 30s
```

- **计算带宽**：`10.2 MB/s × 8 ≈ 81.6 Mbps`（注意单位：1 Byte = 8 bits）

---

### **1.3 使用 `curl` 测试下载速度**

```bash
curl -o /dev/null <https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso>
```

**输出示例**：

```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3635M  100 3635M    0     0  11.3M      0  0:05:21  0:05:21 --:--:-- 11.3M
```

- **带宽 ≈ 11.3 MB/s × 8 ≈ 90.4 Mbps**

---

## **2. 测试内网带宽（服务器间传输）**

### **2.1 使用 `iperf3`（推荐）**

`iperf3` 是专业的网络带宽测试工具，适用于 **服务器间内网测速**。

### **安装 `iperf3`**

```bash
# Ubuntu/Debian
sudo apt install -y iperf3

# CentOS/RHEL
sudo yum install -y iperf3
```

### **测试步骤**

1. **在目标服务器（Server）上启动服务端**：
    
    ```bash
    iperf3 -s
    ```
    
    ```bash
    Server listening on 5201
    ```
    
2. **在另一台服务器（Client）上测试带宽**：
    
    ```bash
    iperf3 -c <目标服务器IP>
    ```
    
    **输出示例**：
    
    ```bash
    Connecting to host 10.0.0.1, port 5201
    [  5] local 10.0.0.2 port 12345 connected to 10.0.0.1 port 5201
    [ ID] Interval           Transfer     Bitrate
    [  5]   0.00-10.00  sec  1.10 GBytes   945 Mbits/sec     sender
    [  5]   0.00-10.00  sec  1.10 GBytes   945 Mbits/sec     receiver
    ```
    
    - **Bitrate** 即为带宽（945 Mbps）。

### **测试 TCP/UDP 带宽**

```bash
iperf3 -c <目标服务器IP> -u -b 1G  # UDP 测试（1Gbps）
iperf3 -c <目标服务器IP> -R       # 反向测试（Server→Client）
```

---

## **3. 测试上传带宽**

### **3.1 使用 `scp` 或 `rsync`**

```bash
# 生成一个 1GB 的测试文件
dd if=/dev/zero of=testfile bs=1M count=1024

# 上传到另一台服务器并计算时间
scp testfile user@remote_server:/tmp/
```

**观察传输速度**：

```bash
testfile           100% 1024MB  10.2MB/s   01:40
```

- **带宽 ≈ 10.2 MB/s × 8 ≈ 81.6 Mbps**

---

## **4. 测试延迟 & 丢包**

### **4.1 使用 `ping`**

```bash
ping -c 10 google.com
```

**输出示例**：

```bash
10 packets transmitted, 10 received, 0% packet loss, time 9014ms
rtt min/avg/max/mdev = 12.345/15.678/18.901/2.345 ms
```

- **avg** 为平均延迟（15.678 ms），**0% packet loss** 表示无丢包。

### **4.2 使用 `mtr`（更详细）**

```bash
mtr -r google.com
```

**输出示例**：

```bash
Start: 2024-01-01T00:00:00+0800
HOST: example.com           Loss%   Snt   Last   Avg  Best  Wrst StDev
1. 10.0.0.1                 0.0%    10    1.2   1.5   1.0   2.0   0.3
2. 203.0.113.1              0.0%    10    5.6   6.1   5.0   7.2   0.8
3. google.com               0.0%    10   15.6  16.2  15.0  18.9   1.2
```

- **Loss%** 表示丢包率，**Avg** 为平均延迟。

---

## **5. 腾讯云轻量服务器带宽测试注意事项**

1. **避免超出带宽限制**：
    - 轻量服务器通常有 **峰值带宽限制**（如 5Mbps、30Mbps），持续高带宽可能触发限速。
2. **测试时间段**：
    - 避免高峰期（如晚上 8-10 点），网络可能拥堵。
3. **内网测速**：
    - 如果测试腾讯云同地域服务器间带宽，可使用 `iperf3`，内网带宽通常更高（如 1Gbps+）。

---

## **总结**

|**测试类型**|**推荐工具**|**命令示例**|
|---|---|---|
|**公网下载测速**|`speedtest-cli`、`wget`|`speedtest-cli`|
|**公网上传测速**|`scp`、`rsync`|`scp testfile user@server:/tmp/`|
|**内网带宽测速**|`iperf3`|`iperf3 -s`（服务端），`iperf3 -c IP`（客户端）|
|**延迟 & 丢包**|`ping`、`mtr`|`mtr -r google.com`|

如果测试结果远低于预期，建议：

1. 检查 **云服务器带宽限制**（腾讯云控制台）。
    
2. 使用 `iftop` 或 `nload` 查看实时流量占用：
    
    ```bash
    sudo apt install iftop
    sudo iftop -i eth0
    ```
    
3. 联系云服务商确认是否有限速策略。