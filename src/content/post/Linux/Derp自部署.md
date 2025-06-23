---
date: 2025/04/09
---
### 使用Docker部署

推荐使用域名容器进行部署

这种方案需要满足以下几个条件：

- 要有自己的域名，并且申请了 SSL 证书
- 需要准备一台或多台云主机
- 如果服务器在国内，域名需要备案

如果以上条件都俱备，就可以按照下面的步骤开始部署了。

使用 `yangchuansheng` 构建的docker进行部署

```bash
docker run --restart always \\
  --name derper -p 12345:12345 -p 3478:3478/udp \\
  -v /root/.acme.sh/xxxx.com/:/app/certs \\
  -e DERP_CERT_MODE=manual \\
  -e DERP_ADDR=:12345 \\
  -e DERP_DOMAIN=xxxx.com \\
  -d ghcr.io/yangchuansheng/ip_derper:latest
```

注意点：

- 默认情况下也会开启 STUN 服务，UDP 端口是 `3478`；
- 防火墙需要放行端口 12345 和 3478；
- 准备好 SSL 证书；
- 域名部分我打了码，请换成你自己的域名。

### **修改Access Controls配置**

部署好 derp 之后，就可以修改 **Access Controls** 的配置来使用自定义的 DERP 服务器了。

```bash
"900": {
				"RegionID":   900,
				"RegionCode": "alinj",
				"RegionName": "ali nanjing",
				"Nodes": [
					{
						"Name":             "900",
						"RegionID":         900,
						"HostName":         "xxxx.com",
						"DERPPort":         12345,
						"IPv4":             "IP",
						"InsecureForTests": true,
						"STUNPort":         3478,
					},
				],
			},
```

### 结果

![image.png](https://4b5aa40.webp.li/derper.png)