根据你的操作系统来选择相应的Chia客户端安装方式。
安装完后，按照[快速开始](Quick-Start-Guide)里的教程来运行软件。同时还需要关注[版本](releases)的变化信息以及[常见问题](FAQ)的答疑。

| 跳转章节: | [Windows](INSTALL#Windows) |[MacOS](INSTALL#MacOS) | [Ubuntu](INSTALL#ubuntudebian) | [CentOS / Red Hat](INSTALL#centosred-hatfedora) | [WSL2](INSTALL#WSL2) | [Amazon Linux 2](INSTALL#amazon-linux-2) | [Other platforms](INSTALL#other-install-methods-and-environments) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |

## 最低配置
Chia所能支持的最低配置设备是树莓派4。
* 四核处理器 1.5Ghz （必须是64位操作系统）
* 2GB内容
* 系统python版本为3.7及以上


## 支持的硬盘格式

Chia的K32农田大小为108GB(101GiB)。农田需要储存在支持大容量格式的硬盘中，比如，NTFS, APFS, exFAT, ext4, etc.如果使用用FAT格式的硬盘（FAT12, FAT16, or FAT32）,那开垦的农田最终无法储存。Chia客户端在未来会新增检查最终目录是否符合要求的功能，但目前需要用户自己检查硬盘格式。

## 暂停开垦（P图）

Chia在开垦农田的过程中需要好几个小时才能完成。如果开垦过程中，如果计算机或者硬盘休眠/关闭了，开垦任务将会失败，需要重新开始。在启动开垦任务之前，请先确定你的计算机以及硬盘是否关闭了睡眠，休眠还有节能模式。在未来的版本中，可以继续开垦已中断的任务。但现在，如果开垦过程遇到了错误，请删除缓存目录中所有tmp文件，然后重新开垦任务。

## 测试版已升级至正式版，不做阐述


# Windows

Windows安装包 - [Chia Blockchain Windows](https://download.chia.net/latest/Setup-Win64.exe)

由于Chia软件的签名证书比较新，所以在你下载完点击运行时，在弹出的信任相关的对话框里，需要点击“更多信息”以及“依然要运行”，这个过程不需要使用命令行。有些windows系统的杀毒软件会把Chia的安装程序当作危险程序，Chia所有的代码都涉及开源的，你也可以自己编译完成安装，所以如果安全软件询问时建议忽略并信任。

现在你可以开始[快速入门](Quick-Start-Guide)。

# macOS

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

# Ubuntu/Debian

这里有编译好的测试版带图形界面的[安装包](https://download.chia.net/latest/x86_64-Ubuntu-gui),仅供X86架构64位系统的Ubuntu 18.04及以上桌面版，还有Debian Buster及以上带GUI的版本使用。同时也提供了ARM架构64位系统的[带GUI安装包](https://download.chia.net/latest/ARM64-Ubuntu-gui) ，适用于树莓派Ubuntu及Debian的64位系统。

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
git reset --hard FETCH_HEAD

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

### 故障排除

在升级安装新版本之前如果有遗留的进程还在运行的话，会导致安装出错。所以先确认Chia进程是否已经关闭结束，然后再将进行升级安装操作。

一般使用`chia stop -d all`命令来结束chia相关进程，不过还是建议再使用`ps -Af | grep chia`检查一下，是否还有chia进程遗漏未结束关闭。如果在执行完`chia stop -d all`命令以后没有彻底关闭完chia的进程，需要你手动结束。

如果失败了，请使用重启大法。

# CentOS/Red Hat/Fedora

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

# WSL2

在WSL2（适用于 Linux 的 Windows 子系统，自行百度使用，win10可用）中的Ubuntu 20.04 LTS系统也可以运行Chia客户端。
You can run chia-blockchain in Ubuntu 20.04 LTS via WSL2 on Windows.

注意：在WSL2中开垦农田只比在windows系统上快一点点，不过需要你做较多正确的配置才能达到加快开垦速度。如果你觉得这么做比较麻烦，建议你还是使用windows客户端。

**无法开启带图形界面的客户端** 因为WSL2不为子系统提供系统桌面支持。

## 检查你的windows系统是否安装了WSL1或者WSL2:
打开PowerShell, 输入:
```
wsl -l -v
```

If you get a listing of help topics for wsl commands, you have WSL1, and need to upgrade. To upgrade, [follow the instructions here](https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2). If you get a blank result or a listing of installed Linux versions, you have WSL2 and are OK to proceed.

## If WSL is not installed:
From an Administrator PowerShell:
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all
```
You will be prompted to reboot. 

## Installing a new WSL2 instance:
Install Ubuntu 20.04 LTS from the Microsoft Store and run it and complete its initial install steps. You now have a linux bash shell environment that can run linux native software on Windows.

Then follow the steps below which are the same as the usual Ubuntu instructions above with a target of Python 3.8.
```bash
sudo apt-get update
sudo apt-get upgrade -y

git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh

. ./activate

```
Running a standalone Windows wallet gui is deprecated but may return in later versions. You can run the Windows version and share keys. You can also plot in WSL2 and migrate the plots to a Windows farmed plot directory.

## Increasing the WSL Maximum Storage Capacity
WSL2 uses a Virtual Hardware Disk (VHD) to store files, and it automatically resizes as files grow. **However, the VHD has an initial maximum size of 256 GB.** Therefore, the default WSL2 VHD is probably only capable of plotting k=30 plots. To plot anything larger, you will need to increase the maximum allowable size. [Follow the guide here.](https://docs.microsoft.com/en-us/windows/wsl/compare-versions#expanding-the-size-of-your-wsl-2-virtual-hardware-disk)

## Setting a maximum limit to WSL2 memory access
If you try plotting Chia in WSL2 without limiting the memory access, WSL2 will use 100% of your available machine's memory, and your computer will get bogged down and begin swapping memory to your hard drive. This will severely cripple your plotting speeds. To set the maximum memory that WSL2 is allowed to use, create a configuration file [as described in this guide](https://www.bleepingcomputer.com/news/microsoft/windows-10-wsl2-now-allows-you-to-configure-global-options/).

## WSL VHD Plotting Nuances
Plotting within WSL2 can write to either the native VHD (which is EXT4) or to any other drive, which can be NTFS or any other FS-type. Writing to the native VHD is faster than writing out to another drive.

Plotting uses three commands for directory control:

`-t` for initial temp directory. Phases 1 and 2 happen here.

`-2` for secondary temp directory. Phase 3 (compression) happens here.

`-d` for final destination. Phase 4 happens here.

Plotting works such that `-t` and `-2` require the exact same amount of storage space. Therefore, if `-t` and `-2` point to the same drive, that drive needs 2x the final file size + 1x the max working file size.

For maximum speed, `-t` and `-2` should be inside the WSL2 filesystem. Something like: `-t ~/chia_temp -2 ~/chia_temp`. Just beware that the WSL2 VHD will need a much larger maximum capacity.

`-d` can point to any other drive for the final destination.


# Amazon Linux 2

```bash
sudo yum update -y
sudo yum install python3 git -y

git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh

. ./activate


# Or install chia-blockchain as a binary package
curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install -y nodejs

python3.7 -m venv venv
ln -s venv/bin/activate
. ./activate
pip install --upgrade pip
pip install -i https://download.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10

pip install chia-blockchain==1.2.1
```

# Other install methods and environments
* [Raspberry Pi 4](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi)
* [Docker](https://github.com/orgs/Chia-Network/packages/container/package/chia)
* [FreeBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/FreeBSD-Install)
* [Ubuntu Binary Install](https://github.com/Chia-Network/chia-blockchain/wiki/Ubuntu-Binary-Install)
* [OpenBSD Install](https://github.com/Chia-Network/chia-blockchain/wiki/OpenBSD-Install)


You need Python 3.7 or newer.

Chia strives to provide [binary wheels](https://pythonwheels.com/) for modern systems. If your system does not have binary wheels, you may need to install development tools to build some Python extensions from source. If you're attempting to install from source, setting the environment variable BUILD_VDF_CLIENT to N will skip trying to build Timelord components that aren't very cross platform, e.g. `export BUILD_VDF_CLIENT=N`.

## Create a virtual environment

Your installation goes inside a [virtual environment](https://docs.python-guide.org/dev/virtualenvs/).

There are lots of ways to create and manage a virtual environment. This is just one.

```bash
python3.7 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

Wheels can be in source or binary format. Binary wheels are specific to an operating system and python version number. Source wheels require development tools.

Chia hosts some binary wheels that are not available from [PyPI](https://pypi.org/). This step is optional, but it may succeed where building from source can take a while or fail in hard-to-debug ways. If wheels are not available for your system, this step will fail. But you can try it anyway.

```bash
pip install -i https://hosted.chia.net/simple/ miniupnpc==2.1 setproctitle==1.1.10
```

Install chia-blockchain.

```bash
pip install chia-blockchain==1.2.1
```

Before you use chia-blockchain in future, you must "enter" your virtual environment.

```bash
source venv/bin/activate
chia -h
```


# Testnets
To join the testnets, we recommend you keep a separate environment by prepending `CHIA_ROOT="~/.chia/testnetx"` to all
of your cli commands. For example, `CHIA_ROOT="~/.chia/testnet7 chia init`. An easier way to do this, is to just `export CHIA_ROOT="~/.chia/testnet7"` so all commands will use testnet7 instead of mainnet.  You should also update all config values to the testnet values, by doing `chia configure -t true`. 
