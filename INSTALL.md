翻译自[2022年01月03日版本-208#](https://github.com/Chia-Network/chia-blockchain/wiki/INSTALL/00fda5a8737ea53ab403f3635935363af4f4c92c)
***

**请确认你的chia客户端是从官网（chia.net）下载的，而不是其他第三方地址。**

官方安装程序可以在官网[下载界面](https://www.chia.net/download/)以及本wiki导航中下载。

根据你的操作系统来选择相应的Chia客户端安装方式。
安装完后，按照[快速开始](Quick-Start-Guide)里的教程来运行软件。同时还需要关注[版本](releases)的变化信息以及[常见问题](FAQ)的答疑。

| 跳转章节: | [Windows](INSTALL#Windows) |[MacOS](INSTALL#MacOS) | [Ubuntu](INSTALL#ubuntudebian) | [CentOS / Red Hat](INSTALL#centosred-hatfedora) | [WSL2](INSTALL#WSL2) | [Amazon Linux 2](INSTALL#amazon-linux-2) | [Other platforms](INSTALL#other-install-methods-and-environments) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |

## 最低配置
Chia所能支持的最低配置设备是树莓派4(内存4GB)。
* 四核处理器 1.5Ghz （必须是64位操作系统）
* 4GB 内存
* 系统python版本为3.7，3.8以及3.9（当前已知最高版本为3.10）。


## 支持的硬盘格式

Chia的K32农田大小为108GB(101GiB)。农田需要储存在支持大容量格式的硬盘中，比如，NTFS, APFS, exFAT, ext4, etc.如果使用用FAT格式的硬盘（FAT12, FAT16, or FAT32）,那开垦的农田最终无法储存。Chia客户端在未来会新增检查最终目录是否符合要求的功能，但目前需要用户自己检查硬盘格式。

## 暂停开垦（P图）

Chia在开垦农田的过程中需要好几个小时才能完成。如果开垦过程中，如果计算机或者硬盘休眠/关闭了，开垦任务将会失败，需要重新开始。在启动开垦任务之前，请先确定你的计算机以及硬盘是否关闭了睡眠，休眠还有节能模式。在未来的版本中，可以继续开垦已中断的任务。但现在，如果开垦过程遇到了错误，请删除缓存目录中所有tmp文件，然后重新开垦任务。

## Windows

Windows安装包 - [Chia Blockchain Windows](https://download.chia.net/latest/Setup-Win64.exe)

由于Chia软件的签名证书比较新，所以在你下载完点击运行时，在弹出的信任相关的对话框里，需要点击“更多信息”以及“依然要运行”，这个过程不需要使用命令行。有些windows系统的杀毒软件会把Chia的安装程序当作危险程序，Chia所有的代码都涉及开源的，你也可以自己编译完成安装，所以如果安全软件询问时建议忽略并信任。

现在你可以开始[快速入门](Quick-Start-Guide)。

## macOS

macOS需要Mojave系统10.14.x以上版本。

选择macOS.dmg安装包进行安装。[Chia Blockchain MacOS](https://download.chia.net/latest/Setup-MacOS.dmg)

当安装包首次运行时会向macOS的密钥串中生成并导入多个密钥。你可能需要验证3次你的密码，建议你选择“总是运行”。

现在你可以开始[快速入门](Quick-Start-Guide)。

如果想编译安装一个开发版本Chia的话，在安装之前，请先确认本机的brew（包管理器）可用，以及python是否为3.7以上的版本。

```bash
git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh
. ./activate

sh install-gui.sh

cd chia-blockchain-gui
npm run electron &
```

## Ubuntu/Debian

这里有编译好的带图形界面的[安装包](https://download.chia.net/latest/x86_64-Ubuntu-gui),仅供X86架构64位系统的Ubuntu 18.04及以上桌面版，还有Debian Buster及以上带GUI的版本使用。同时也提供了ARM架构64位系统的[带GUI安装包](https://download.chia.net/latest/ARM64-Ubuntu-gui) ，适用于树莓派Ubuntu及Debian的64位系统。

二进制命令行工具在以下路径： `/usr/lib/chia-blockchain/resources/app.asar.unpacked/daemon/`

下面的安装说明使用的是Ubuntu 20.04 LTS系统。如果你在Ubuntu 18.04 LTS系统上安装Chia的话，需要先确认Python版本是否为3.7及以上，

替换方式：`sudo apt-get install python3.7-venv python3.7-distutils python3.7-dev git lsb-release -y`

```bash
sudo apt-get update
sudo apt-get upgrade -y

# 安装git
sudo apt install git -y

# 检验源文件，开始安装
git clone https://github.com/Chia-Network/chia-blockchain.git -b latest --recurse-submodules
cd chia-blockchain

sh install.sh

. ./activate

***NOTE: As of December 2021, we are aware of issues with the install UI script for the current version, please download an installer instead at chia.net/download***
# 安装带GUI的Chia软件需要系统带有桌面。
# 无法以root身份安装并运行带GUI的Chia软件

sh install-gui.sh

cd chia-blockchain-gui
npm run electron &
```

升级至新版本
```bash
cd chia-blockchain
. ./activate
chia stop -d all
deactivate
git fetch
git checkout latest
git reset --hard FETCH_HEAD --recurse-submodules

# If you get RELEASE.dev0 then delete the package-lock.json in chia-blockchain-gui and install.sh again

git status

# git status should say "nothing to commit, working tree clean", 
# if you have uncommitted changes, RELEASE.dev0 will be reported.

sh install.sh

. ./activate

chia init

# 安装带GUI的Chia软件需要系统带有桌面。
# 无法以root身份安装并运行带GUI的Chia软件
cd chia-blockchain-gui
git fetch
cd ..
chmod +x ./install-gui.sh
./install-gui.sh

cd chia-blockchain-gui
npm run electron &

```

## 故障排除

在升级安装新版本之前如果有遗留的进程还在运行的话，会导致安装出错。所以先确认Chia进程是否已经关闭结束，然后再将进行升级安装操作。

一般使用`chia stop -d all`命令来结束chia相关进程，不过还是建议再使用`ps -Af | grep chia`检查一下，是否还有chia进程遗漏未结束关闭。如果在执行完`chia stop -d all`命令以后没有彻底关闭完chia的进程，需要你手动结束。

如果失败了，请使用重启大法。

## CentOS/Red Hat/Fedora

为RH/CentOS 8.0 and Fedora 28以上的版本系统提供了带GUI的[安装包](https://download.chia.net/latest/x86_64-Redhat-gui)。

```bash
sudo yum install epel-release -y
sudo yum update -y

# 编译安装python3.7及以上的版本，需要centos系统至少为centos 7.7以上的版本。
sudo yum install gcc openssl-devel bzip2-devel zlib-devel libffi libffi-devel -y
sudo yum install libsqlite3x-devel -y
# RHEL 的有些版本需要安装这些依赖。
sudo yum groupinstall "Development Tools" -y
sudo yum install python3-devel gmp-devel  boost-devel libsodium-devel -y

sudo yum install wget -y
sudo wget https://www.python.org/ftp/python/3.7.7/Python-3.7.7.tgz
sudo tar -zxvf Python-3.7.7.tgz ; cd Python-3.7.7
./configure --enable-optimizations; sudo make -j$(nproc) altinstall; cd ..

# 下载源代码并安装
git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh
. ./activate

***NOTE: As of December 2021, we are aware of issues with the install UI script for the current version, please download an installer instead at chia.net/download***
# 安装带GUI的Chia软件需要系统带有桌面。
# 无法以root身份安装并运行带GUI的Chia软件

sh install-gui.sh
cd chia-blockchain-gui
npm run build
npm run electron

# Or install from binary wheels
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10

pip install chia-blockchain==1.2.1

```

Or, combining the last two steps into one, try

```
pip install --extra-index-url https://hosted.chia.net/simple/ chia-blockchain==1.2.1 miniupnpc==2.1
```

## WSL2

在WSL2（适用于 Linux 的 Windows 子系统，自行百度使用，win10可用）中的Ubuntu 20.04 LTS系统也可以运行Chia客户端。

注意：在WSL2中开垦农田只比在windows系统上快一点点，不过需要你做较多正确的配置才能达到加快开垦速度。如果你觉得这么做比较麻烦，建议你还是使用windows客户端。

**无法开启带图形界面的客户端** 因为WSL2不为子系统提供系统桌面。

### 检查你的windows系统是否安装了WSL1或者WSL2:
打开PowerShell, 输入:
```
wsl -l -v
```
如果得到的是关于WSL命令的帮助信息，说明本机自带的是WSL1，需要升级为WSL2。如何[升级至WSL2？](https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2) 
如果得到的是空白结果或者已安装Linux版本的列表，说明已安装WSL2，请继续。

### 如果WSL没有安装：
管理员身份打开PowerShell：
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all
```
安装完后需要重新启动。

##安装新的WSL2实例
从微软应用商店中安装 Ubuntu 20.04 LTS 系统并运行后，你的Windows就拥有了Linux内核交互环境，也就可以运行基于Linux的软件了。

接下来安装Chia就跟在Ubuntu系统的使用方式一样，也需要Python 3.8的运行环境。
```bash
sudo apt-get update
sudo apt-get upgrade -y

git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh

. ./activate

```
建议在WSL2的Linux系统中开垦完农田以后，然后将农田迁移到Windows系统的目录中进行耕种（挖矿）。

### 给WSL子系统空间进行扩容
WSL2使用的是虚拟硬盘作为存储空间，会随着文件的增加而自动调整大小。**尽管如此，虚拟硬盘有一个256GB的初始最大值。**因此，WSL2的默认虚拟硬盘只够开垦K30格式的农田。开垦更大格式的农田就需要给WSL2子系统扩容,[点击查看扩容方式。](https://docs.microsoft.com/en-us/windows/wsl/compare-versions#expanding-the-size-of-your-wsl-2-virtual-hardware-disk)

### 设置WSL2子系统最大内存使用限制
如果你使用WLS2子系统进行Chia的开垦任务而没有设置内存使用量限制，那WSL2会占用你所有的内存，你的计算机会变得卡顿并且开始使用硬盘里的交换内存，从而严重影响农田的开垦速度。所以要根据本机的情况来设置WLS2的最大内存限制，请按照此[指南说明](https://www.bleepingcomputer.com/news/microsoft/windows-10-wsl2-now-allows-you-to-configure-global-options/)来创建相关配置文件。

### 使用WSL进行开垦的区别
在WSL2子系统中进行开垦任务时，缓存的读写盘可以是虚拟硬盘（EXT4格式）也可以是其他本地的挂载盘（NTFS或者别的格式文件系统）。但在虚拟硬盘中的读写速度要比其他的更快一些。

开垦任务使用3个参数命令来控制相关目录

`-t` 第一缓存目录，第一阶段及第二阶段缓存读写目录。

`-2` 第二缓存目录，用于第三阶段压缩过程中使用。

`-d` 最终目录，第四阶段使用。

开垦任务时，针对`-t` 和 `-2`需要精确计算所需要的储存空间。因此，如果两个目录使用的时同一块硬件储存设备，需要至少需要三倍农田文件大小的空间。

为了最大化开垦速度，`-t` 和 `-2`必须时WSL2子系统内的文件目录。比如：`-t ~/chia_temp -2 ~/chia_temp`。只需要注意虚拟硬盘的空间是否设置了足够大小。

-d` 可以分配任意挂载盘来充当最终农田文件目录。

## Amazon Linux 2

```bash
sudo yum update -y
sudo yum install python3 git -y

git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh

. ./activate


# 或者以二进制包（binary package）的形式安装Chia
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10

pip install chia-blockchain==1.2.1
```

## 其他系统环境下的Chia安装方式

* [Raspberry Pi 4](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi)
* [Docker](https://github.com/orgs/Chia-Network/packages/container/package/chia)
* [FreeBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/FreeBSD-Install)
* [Ubuntu Binary Install](https://github.com/Chia-Network/chia-blockchain/wiki/Ubuntu-Binary-Install)
* [OpenBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/OpenBSD-Install)


需要系统的python环境版本至少为3.7及以上。

Chia strives to provide [binary wheels](https://pythonwheels.com/) for modern systems. If your system does not have binary wheels, you may need to install development tools to build some Python extensions from source. If you're attempting to install from source, setting the environment variable BUILD_VDF_CLIENT to N will skip trying to build Timelord components that aren't very cross platform, e.g. `export BUILD_VDF_CLIENT=N`.

## 创建虚拟环境来运行Chia软件
如果你想在[虚拟环境](https://docs.python-guide.org/dev/virtualenvs/)中运行Chia软件的话。

有很多方式管理运行一个虚拟环境，这里只介绍其中一种。
```bash
python3.7 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

此项为可选项，如果你的pip安装或者升级不成功的话。
```bash
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10
```

安装Chia程序

```bash
pip install chia-blockchain==1.2.1
```
在你使用Chia程序之前，你必须进入到虚拟环境。

```bash
source venv/bin/activate
chia -h
```


## 测试网
想要加入使用测试网的话，建议你预先设置一个独立的系统环境。在使用所有的Chia命令时，都在前面加上`CHIA_ROOT="~/.chia/testnetx"`。举个例子，`CHIA_ROOT="~/.chia/testnet7 chia init`。也可以使用命令 `export CHIA_ROOT="~/.chia/testnet7"` ，这样后续所有的命令都将在测试网上运行。同时你还需要执行`chia configure -t true`命令，来将所有的配置更新为测试网的。
