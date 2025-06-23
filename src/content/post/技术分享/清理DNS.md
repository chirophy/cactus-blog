---
date: 2025/01/17
---
### PC

如果你想清理计算机上的DNS缓存，可以使用以下命令：

- **Windows**：
    1. 打开命令提示符（以管理员身份运行）。
    2. 输入命令：`ipconfig /flushdns`，然后按回车。
- **macOS**：
    1. 打开终端。
    2. 输入命令：`sudo killall -HUP mDNSResponder`，然后按回车。

### **谷歌浏览器 Chrome**

要清除 Google Chrome 的 DNS 缓存，请执行以下步骤：

1. 打开一个新标签，然后在地址栏输入 `chrome://net-internals/#dnsChrome`。
2. 点击 “清除主机缓存” 按钮。

如果那对你不起作用，请尝试清除缓存和 Cookie。

1. 按下 `CTRL+Shift+Del` 以打开 “清除浏览数据” 对话框窗口。
2. 选择一个时间范围。选择 “所有时间” 以删除所有内容。
3. 选中 “Cookie 和其他站点数据” 和 “缓存的图像和文件” 框。
4. 点击 “清除数据” 按钮。

此方法适用于所有基于 Chrome 的浏览器，包括 Chromium，Vivaldi 和 Opera。

### 路由端

有清理按键那就清理，没有的话重启最有效啦。