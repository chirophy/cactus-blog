---
date: 2025/04/09
---
### Docker安装

```bash
docker run -d --name bitwarden \\
  --restart unless-stopped \\
  -e WEBSOCKET_ENABLED=true \\
  -v /www/bitwarden/:/data/ \\
  -p 6666:80 \\
  -p 3012:3012 \\
  vaultwarden/server:latest
```

即可

后续进行反向代理就行了。