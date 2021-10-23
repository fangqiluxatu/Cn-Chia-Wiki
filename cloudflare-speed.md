**本文参照GitHub中[XIU2/CloudflareSpeedTest](https://github.com/XIU2/CloudflareSpeedTest)的脚本工具及说明进行简化描述，详情请查阅原作者github。**
这个问题是我在连接space矿池时遇到的，所以以下我会使用space.pool及chia.net（是的，官网也用的cloudflare的服务器）做示例。

# Linux
**注意，修改 Hosts 需要 root（管理员）用户权限，因此如果你当前不是 root 用户，请使用 sudo su 切换。**
首先修改一下系统的Hosts文件，先将 pool.space 以及 asia1.pool.space 指定一下IP为1.0.0.1
```
#切换root
sudo su

# 编辑hosts文件
sudo nano /etc/hosts

# 在hosts里添加解析记录后保存退出
1.0.0.1  pool.space
1.0.0.1  asia1.pool.space

# 如果是第一次使用，则建议创建新文件夹
mkdir CloudflareST

# 进入文件夹（后续更新，只需要从这里重复下面的下载、解压命令即可）
cd CloudflareST

# 下载 CloudflareST 压缩包,源码地址：https://github.com/XIU2/CloudflareSpeedTest/releases ，这里注意一下你的CPU架构是AMD还是ARM的。
wget -N https://github.com/XIU2/CloudflareSpeedTest/releases/download/v1.5.0/CloudflareST_linux_amd64.tar.gz

# 解压
tar -zxf CloudflareST_linux_amd64.tar.gz

# 赋予执行权限
chmod +x CloudflareST

# 下载hosts自动更新脚本，源码地址：https://github.com/XIU2/Shell/blob/master/cfst_hosts.sh
wget -N https://shell.xiu2.xyz/cfst_hosts.sh

# 赋予执行权限
chmod +x cfst_hosts.sh

# 运行脚本
bash cfst_hosts.sh
```

首次运行时，脚本会提示以下内容：
```
该脚本的作用为 CloudflareST 测速后获取最快 IP 并替换 Hosts 中的 Cloudflare CDN IP。

第一次使用，请先将 Hosts 中所有 Cloudflare CDN IP 统一改为一个 IP。
输入该 Cloudflare CDN IP 并回车（后续不再需要该步骤）:1.0.0.1
```
在这里输入我们前面添加解析的那个IP，即：1.0.0.1

随后脚本就会开始测速、备份 Hosts 文件、替换 IP 等操作，提示内容大概如下：

```
开始测速...
# XIU2/CloudflareSpeedTest vX.X.X

开始延迟测速（模式：TCP IPv4，端口：443）：
27936 / 27936 [-------------------------------------------------------------------------------------------------] 100.00%
...
完整测速结果已写入 result.csv 文件，请使用记事本/表格软件查看。

旧 IP 为 1.0.0.1
新 IP 为 Y.Y.Y.Y

开始备份 Hosts 文件（hosts_backup）...
已复制         1 个文件。

开始替换...
完成...
```

# Windows
windows及其他详细设定（例如定时任务）可以参照原博主的[介绍](https://github.com/XIU2/CloudflareSpeedTest/issues/42)，非常详尽了。后续只需要做个定时任务，即可自动修改最佳连接的解析地址。