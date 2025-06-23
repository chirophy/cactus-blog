---
date: 2025/04/10
---
## **使用Docker进行部署**

**特点**：

- 基于 **Docker** 的 Nginx 管理工具
- 提供 **Web UI 管理反向代理、SSL 证书（Let's Encrypt）**
- 适合 **托管多个网站/服务**

**安装（需 Docker）**：

```bash
sudo apt install docker.io docker-compose -y
mkdir nginx-proxy-manager && cd nginx-proxy-manager
nano docker-compose.yml
```

粘贴以下内容：

```yaml
version: '3'
services:
  app:
      image: 'jc21/nginx-proxy-manager:latest'
      restart: unless-stopped
      ports:
      - '80:80'      # HTTP 端口
      - '81:81'      # HTTPS 端口
      - '443:443'    # 管理界面端口
      volumes:
      - ./data:/data # 配置数据
      - ./letsencrypt:/etc/letsencrypt  # SSL 证书
```

启动：

```bash
docker-compose up -d
```

**访问**：

- `http://<服务器IP>:81`
- 默认账号：`admin@example.com`，密码：`changeme`

**优点**： ✅ 专为 Nginx 反向代理优化

✅ 自动 SSL 证书申请（Let's Encrypt）

❌ 依赖 Docker，不适合纯文件配置

## 介绍

- **功能**：
    - 基于 Web 的 Nginx 管理界面（无需手动编辑配置文件）。
    - 支持 **反向代理、负载均衡、HTTP/HTTPS 转发**。
    - 自动申请和续期 **Let's Encrypt SSL 证书**。
    - 支持 **IP 限制、访问控制、HTTP 认证**。
- **适用场景**：
    - 托管多个网站/服务（如 WordPress、Next.js、Node.js）。
    - 统一管理 SSL 证书。
    - 替代手动配置 Nginx 反向代理。

## **配置反向代理**

### **场景 1：将域名代理到本地服务（如 Node.js）**

假设你的应用运行在 `http://localhost:3000`，域名是 `app.example.com`：

1. 点击 **Hosts → Proxy Hosts → Add Proxy Host**。
2. 填写配置：
    - **Domain Names**: `app.example.com`
    - **Scheme**: `http`
    - **Forward Hostname/IP**: `localhost`（或容器名，如 `app`）
    - **Forward Port**: `3000`
    - **开启 SSL**（可选，后续可自动申请证书）。
3. 点击 **Save**。

### **场景 2：负载均衡（多后端服务器）**

1. 点击 **Hosts → Redirection Hosts**。
2. 配置多个后端服务器（如 `192.168.1.100:8080` 和 `192.168.1.101:8080`）。
3. 选择负载均衡策略（轮询、最少连接等）。

## **配置 SSL 证书（HTTPS）**

### **自动申请 Let's Encrypt 证书**

1. 在 **Proxy Host** 配置页面，切换到 **SSL** 选项卡。
2. 选择：
    - **SSL Certificate**: `Let's Encrypt`
    - **Email Address**: 你的邮箱（用于证书到期提醒）。
3. 勾选 **Force SSL**（强制 HTTPS）。
4. 点击 **Save**，NPM 会自动申请并部署证书。

### **手动上传自定义证书**

1. 在 **SSL Certificates → Add SSL Certificate**。
2. 上传 `.crt` 和 `.key` 文件。

## 查看80端口被占用情况

```bash
netstat -tulnp | grep 80
or
lsof -i :80
```