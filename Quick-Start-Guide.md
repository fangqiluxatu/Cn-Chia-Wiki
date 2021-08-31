## 请先浏览新手指南
这是一份更简洁的指南。如果你不太了解Chia的话，建议先阅读[新手指南](Beginners-Guide)。

# 安装

# Install
查看[安装教程](INSTALL),根据你的系统选择相应的方式来安装Chia客户端。只支持64位操作系统。

所有的配置文件及都储存在 $CHIA_ROOT 环境目录下或者在~/.chia/mainnet/（linux就在/home/user/.chia/mainnet/，Windows的话就在C:\Users\user\.chia\mainnet）下，目录下有区块数据以及日志文件。你也可以选择

All configuration data is stored in a directory structure at the $CHIA_ROOT environment variable or at  ~/.chia/mainnet/. You can find databases, and logs there. Optionally, you can set $CHIA_ROOT to the .chia directory in your home directory with `export CHIA_ROOT=~/.chia` and if you add it to your .bashrc or .zshrc to it will remain set across logouts and reboots. If you set $CHIA_ROOT you will have to migrate configuration items by hand or unset the variable for `chia init` to work with `unset CHIA_ROOT`.

如果你使用MacOS或者Windows安装使用的话，首次运行时需要创建账号密钥，建议你保存好你的助记词。接下来就可以在农田分栏里开垦农田了，或者也可以使用命令行工具。这将会消耗比较长的时间，具体时间取决于[农田格式解析](k-sizes)。如果你想参与主网的耕种（挖矿）
If you are using the MacOS or Windows builds, your keys are created during the first run. We recommend saving the mnemonic. You can start plotting a plot file using the Plot tab or the command line. This can take a long time depending on the [size of the plots](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes)
(the k variable). To be competitive on mainnet you will probably have to have a few k=32 or larger plots but a k=32 plot currently takes about 10 hours to plot on an [M.2 PCIe NVMe SSD](https://en.wikipedia.org/wiki/M.2) and requires 232 GiB of temporary working space to create a final plot file of 101.3 GiB. Your likelihood of winning a given plot is only driven by the final size of files.

The minimum plot size is k=32. Plots created with Beta 8 and newer version of the chia software will work on mainnet.

If you want more peers and better network connectivity, you should also try opening port 8444 on your router so other peers can connect to you. Follow [this](https://bitcoin.org/en/full-node#port-forwarding) guide but using port 8444 instead of 8333. This helps the network be more decentralized. For further details about sync issues and port 8444, visit the [Resolving Sync Issues](https://github.com/Chia-Network/chia-blockchain/wiki/Resolving-Sync-Issues---Port-8444) page.

# 使用命令行界面（CLI）

# Using the Command-line Interface (CLI)

Using the CLI with Chia gives you greater and more precise control. For a more details on the commands, read the [CLI Commands Reference](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference).

## Windows系统下使用

参照[新手指南](Beginners-Guide)学习如何使用Chia客户端。

你可以通过命令行工（CLI）来检查Chia命令是否有效：chia.exe目录位置：`~\AppData\Local\Chia-Blockchain\app-1.1.5\resources\app.asar.unpacked\daemon\`.运行命令：`.\chia -h` or `.\chia plots -h`。

1. 打开 *PowerShell* 
	在桌面左下角开始菜单右键打开 "powershell"。

2. 使用`cd`切换目录
	在 "powershell" 中输入 `cd C:\Users\user\AppData\Local\Chia-Blockchain\app-1.1.5\resources\app.asar.unpacked\daemon\`，回车。

3. 阅读Chia命令帮助信息
	在 "powershell" 中输入 `.\chia -h`，回车。

想了解有关windows系统运行Chia时的更多信息，请查阅[Windows下注意事项](Windows-Tips-and-Tricks)。想学习更多Chia命令行工具的用法，请查阅[命令行（CLI）使用](CLI-Commands-Reference)

通过记事本打开 "\.chia\mainnet\log\debug.log"，可以查看日志文件。或者在*PowerShell*中使用命令行 `Get-Content ~\.chia\mainnet\log\debug.log -wait`，也可以查看。

## MacOS系统下使用
## MacOS
There are commands available in `/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon` Try `./chia -h` or `./chia plots -h` for example. You can view your debug.log as it runs in from Terminal, `tail -f ~/.chia/mainnet/log/debug.log`.

A handy trick is to add that directory to your path - `export PATH=/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon:$PATH`. To make it persistent add the same line to your .bashrc or .zshrc

## Linux系统下使用
## Linux
If you installed Chia with the Linux installer files, your chia executable should be in one of the following locations:

`/usr/lib/chia-blockchain/resources/app.asar.unpacked/daemon/chia`

`/lib/chia-blockchain/resources/app.asar.unpacked/daemon/chia`

If you installed from source (using git), just activate and run `chia` directly. 

## 源代码编译安装
## Development/source builds

If you've installed via the installers you can skip these steps.

Remember that once you complete your install you **must be in the [Python virtual environment](https://docs.python-guide.org/dev/virtualenvs/)** which you access from the chia-blockchain directory, or the Windows "Chia Blockchain" directory, or your home directory if you opted for a binary install. Enter the virtual environment with the command `.   ./activate`. Both dots are critical and once executed correctly your cli prompt will look something like `(venv) username@machine:~$` with ``(venv)`` prepended. 

Use `deactivate` should you want to exit the venv. If you're not a fan of dots, an equivalent alternative on most platforms is `source venv/bin/activate` and you'll see that method in places in this documentation.

### 迁移或创建配置文件

```bash
chia init
```

### 生成密钥
如果你没有Chia密钥的话，使用以下命令进行创建：

```bash
chia keys generate
```

### 运行全节点、农民、收割机及钱包进程
运行全节点是为了连接到主网，默认端口为8444，使用以下命令运行全节点，日志文件通常在以下目录：~/.chia/mainnet/logs/debug.log。
```bash
sh install-gui.sh
cd chia-blockchain-gui
npm run electron &
```

农民是Chia区块网络的缔造者，一但通过空间算力获得了区块奖励，他们将打包并创建区块至主网（就像比特币矿工一样）。



You can use the command line tools and change the working directories and output directory for plotting, with the "-t" (temp), "-2" (second temp), and "-d" (destination) arguments to the `chia plots create` command. `-n 2` will create two plots of type k=32 and take about 12 hours on NVMe drives in the example below.
```bash
chia plots create -k 32 -n 2
chia plots check -n 30
```
Note that in the dev build the commands are `chia plots create` and `chia plots check`.

# 运行一个时戳机
*注释*

If you want to run a Timelord on Linux, see [Building-timelords](Building-timelords). Information on blue boxes coming soon.

Timelords execute sequential verifiable delay functions (proofs of time or VDFs), that get added to
blocks to make them valid. This requires fast CPUs and a few cores per VDF as well as completing the install steps above and running the following from the chia-blockchain directory:
```bash
. ./activate
sh install-timelord.sh
chia start timelord &
```
# Alternatively run the local simulation
You can instead run the simulation, which runs all servers and multiple full nodes, locally. Note the the simulation is local only and requires installation of timelords and VDFs. The introducer will only know the local ips of the full nodes, so it cannot broadcast the correct ips to external peers. This should work on MacOS and Linux.

```bash
chia start simulator
```

## Tips
Ubuntu 20.04 LTS or newer, Amazon Linux 2, and CentOS 7.7 or newer are the
easiest linux install environments.

UPnP is enabled by default to open port 8444 for incoming connections.
If this causes issues, you can disable it in `config.yaml`. Or you can run this command: `chia configure -upnp false`
Some routers may require port forwarding, or enabling UPnP
in the router's configuration.

# RPC Interface

The Node has an RPC Interface with [documentation](https://github.com/Chia-Network/chia-blockchain/wiki/RPC-Interfaces).

