---
date: 2021/07/04
---
之前在亚马逊上买了`WD`的氦盘，12T算上税费1767，对比上酷狼8T1999的价格，这一波还是赚的吧 然后把12T的硬盘挂载到`nas`上了，就需要改一下`transmission`的下载路径，直接修改为挂载的目录就可以了。 后来找到了**美化**相关内容。  

就顺带着记录一下：  
## 美化`transmission`的界面
### 安装

```bash
mkdir -p /volume1/tools

cd /volume1/tools

wget <https://github.com/ronggang/transmission-web-control/raw/master/release/install-tr-control-cn.sh>
or国内：
wget <https://gitee.com/culturist/transmission-web-control/raw/master/release/install-tr-control-gitee.sh>

chmod +x install-tr-control-cn.sh

bash install-tr-control-cn.sh
```

### 展示
成功之后就这样啦：  
![](https://4b5aa40.webp.li/tr-script.png)  

最后`transmission`的界面为：  
![](https://4b5aa40.webp.li/1851480362.png)  

之后新的界面就舒服多了，上传下载一目了然，分享率也直观体现！  

### 更新
保持更新是很重要的一件事！  
通过群晖的`任务计划`自动安装及定期自动更新  

### 创建任务计划
- 依次打开`DSM`的`“控制面板”`->`“任务计划”`
- 选择`“新增”`->`“计划的任务”`->`“用户定义的脚本”`
- 任务名称用英文，如：`AutoUpdateTrWebControl`，用户帐号选择`root`并选中`已启动`
- 计划时间可根据自己需要来设置，如想保持最新的版本，可以设置定期执行
- 在`“任务设置”`的`“运行命令”`->`“用户定义的脚本”`中输入：  
```bash
curl -s <https://raw.githubusercontent.com/ronggang/transmission-web-control/master/release/install-tr-control-cn.sh> | bash -s auto
```

国内用户可使用`gitee`镜像地址：  
```bash
curl -s <https://gitee.com/culturist/transmission-web-control/raw/master/release/install-tr-control-gitee.sh> | bash -s auto
```
- 脚本最后一定要加入`auto` ，要不然脚本不会自动下载
- **注：**`auto`前有一个空格

## DSM7.0
在这个版本中，需要额外修改权限以实现自动更新的功能 在 `root` 权限下执行以下命令，其中：  
- `YOUR_USERNAME` 替换为你登录和更新脚本时候选择的用户
- `/var/packages/transmission/target/share/transmission/web/` 这串路径为 transmission 的安装路径（默认应该是这个）  
```bash
sed -i '/sc-transmission/s/$/YOUR_USERNAME/' /etc/group
chown sc-transmission:sc-transmission /var/packages/transmission/target/share/transmission/web/* -R
chmod 774 /var/packages/transmission/target/share/transmission/web/* -R
```

## 参考
参考：[官方文档](https://github.com/ronggang/transmission-web-control/wiki/Home-CN)  
## 日志目录
安装官方的`transmission`后，其路径不在默认的安装路径`/volume1/@appstore/transmission` 中  
而是在`/var/packages/transmission`中  
详细位置`/var/packages/transmission/var/transmission.log` 然后复制到对应位置方便查看  
```bash
cp /var/packages/transmission/var/transmission.log /[地址]
```

## 2023年4月9日更新
### 说明
`Transmission`于 _2023年2月8日_ 更新了`4.0.0` 版本，详情请见 [Releases](https://github.com/transmission/transmission/releases/)  

因为部分代码改动，造成了目前`4.*`版本无法再使用 **Web UI**，后面看了看修改的代码，发现仅是构建网页界面的文件夹命名有所改动，那么就好办了！  

### 与众不同
> 由原本的 web 修改为 目前的 public_html  

### 方法
使用`WinSCP`直接覆盖原文件  
将`transmission-web-control-1.6.1-update1`中的`src`文件夹中的内容直接复制粘贴至 `/var/packages/transmission/target/share/transmission/public_html/`中即可  
修改完后记得修改对应权限  
那么，经过如此这般之后，就大功告成了  

### 成功  
![](https://4b5aa40.webp.li/tr.png)