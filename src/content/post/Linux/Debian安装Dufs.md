---
date: 2025/03/06
---
在 Debian 系统上安装 **Dufs**（一个简单的文件服务器工具）可以通过以下步骤完成。Dufs 是一个基于 Rust 的轻量级文件服务器，支持目录列表、文件上传等功能。

---

### 1. **安装依赖**

首先，确保系统已安装必要的依赖工具：

```bash
sudo apt update
sudo apt install curl
```

---

### 2. **下载 Dufs 二进制文件**

Dufs 提供了预编译的二进制文件，可以直接下载并使用。

### （1）访问 Dufs 的 GitHub 发布页面：

打开浏览器，访问 [Dufs GitHub Releases](https://github.com/sigoden/dufs/releases)，找到适合你的系统架构的版本（例如 `dufs-v0.43.0-x86_64-unknown-linux-musl.tar.gz`）。

### （2）下载并解压：

在终端中运行以下命令（以 `v0.43.0` 版本为例）：

```bash
# 下载 Dufs
curl -LO <https://github.com/sigoden/dufs/releases/download/v0.43.0/dufs-v0.43.0-x86_64-unknown-linux-musl.tar.gz>

# 解压
tar -xzf dufs-v0.43.0-x86_64-unknown-linux-musl.tar.gz

# 将二进制文件移动到可执行路径
sudo mv dufs /usr/local/bin/
```

---

### 3. **验证安装**

运行以下命令，检查 Dufs 是否安装成功：

```bash
dufs --version
```

如果显示版本号（例如 `dufs 0.30.0`），说明安装成功。

---

### 4. **使用 Dufs**

Dufs 的使用非常简单。以下是一些常用命令示例：

### （1）启动文件服务器

在当前目录启动文件服务器：

```bash
dufs .
```

默认情况下，Dufs 会监听 `http://127.0.0.1:5000`，你可以在浏览器中访问该地址查看文件列表。

### （2）指定端口和地址

如果需要指定监听的地址和端口，可以使用 `-b` 参数：

```bash
dufs -p 0.0.0.0:8080 .
```

这样，Dufs 会监听所有网络接口的 `8080` 端口。

### （3）启用文件上传

如果需要允许文件上传，可以使用 `-A` 参数：

```bash
dufs -A .
```

### （4）更多选项

查看 Dufs 的所有选项：

```bash
dufs --help
```

---

### 5. **设置为系统服务（可选）**

如果你希望 Dufs 在系统启动时自动运行，可以将其设置为系统服务。

### （1）创建服务文件

编辑一个新的服务文件：

```bash
sudo nano /etc/systemd/system/dufs.service
```

### （2）添加以下内容

```bash
[Unit]Description=Dufs File Server
After=network.target

[Service]ExecStart=/usr/local/bin/dufs -p 0.0.0.0:8080 /path/to/your/directory
Restart=always
User=nobody
Group=nogroup

[Install]WantedBy=multi-user.target
```

将 `/path/to/your/directory` 替换为你希望共享的目录路径。

### （3）启用并启动服务

```bash
sudo systemctl daemon-reload
sudo systemctl enable dufs
sudo systemctl start dufs
```

### （4）检查服务状态

```bash
sudo systemctl status dufs
```

---

### 6. **访问文件服务器**

在浏览器中访问 `http://<你的服务器IP>:8080`，即可查看和下载文件。如果启用了文件上传功能，还可以通过页面直接上传文件。