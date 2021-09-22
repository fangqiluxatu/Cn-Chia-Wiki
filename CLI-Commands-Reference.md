翻译自[2021年8月31日版本-67##](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference/d2165e9af8a8afafa507e5d3d529ee082928b4db)
***
本文相对于在CLI中使用 `chia -h` 命令所获得的帮助信息，将提供更进一步的解释。

但并不意味着会更加全面，因为使用 ` -h` 命令获得的帮助信息已经相当清晰明了。建议查看本文及其他相关教程时，先研究一下 `-h` 的帮助信息。

如果你想了解某个命令的选项有哪些，可以在后面加上 `-h` 参数。例如：

* `chia -h`
* `chia plots -h`
* `chia plots check -h`
* `chia start -h`

本文与其余的篇章一样，跟随项目的发展动态保持更新中。在此期间，你可以浏览 [项目源码](https://github.com/Chia-Network/chia-blockchain/tree/main/chia/cmds) 或者查阅文档 [《精通Chia之空间证明》](https://www.chia.net/assets/Chia_Proof_of_Space_Construction_v1.1.pdf)。

## `chia` 可执行文件

### Mac
如果你是在 `/Applications` 目录中安装的Chia，你可以在目录：
`/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon/chia` 找到chia的二进制文件。

使用 `Terminal.app` 并输入命令来检查是否有效：

`/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon/chia -h`

你也可以将其添加到你的环境变量中去，这样你就可以直接使用 `chia -h` 命令：

`
PATH=/Applications/Chia.app/Contents/Resources/app.asar.unpacked/daemon:$PATH
`

### Windows
有不止一个 `chia.exe` 可执行文件。一个是启动GUI客户端界面的 `Chia.exe` ，另一个是CLI命令工具的 `chia.exe` 。他们位于不同的位置，请注意区分一下文件名的大小写（Chia.exe & chia.exe）。

命令行工具 `chia.exe` 位于：

`~\AppData\Local\chia-blockchain\app-1.1.3\resources\app.asar.unpacked\daemon\chia.exe`

## [init](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/cmds/init.py)

命令: `chia init`
首先, `init` 命令会检查 ~/.chia 目录下是否存在旧版本的信息。

如果存在， `init` 命令就会将该目录的这些文件迁移（连接）至新版本下使用：
* config (包含旧版的SSL文件)
* db
* wallet
* config.yaml(通过配置文件，加载钱包秘钥的位置，将耕种奖励地址及相关配置信息与旧版保持同步一致) 

如果没有旧版本信息, `init` 命令就会：
* 创建一个默认的chia配置文件
* 初始化一个加密证书，以保证与GUI客户端安全通信。

## start

命令: `chia start {服务}`
*  `chia start node` ，将只启动chia的全节点服务进程。
*  `chia start farmer` ，将启动农民、收割机、全节点以及钱包进程。
*  
* Service `node` will start only the full node.
* Service `farmer` will start the farmer, harvester, a full node, and the wallet.
* 其它参数， `chia start {*}`:
  {all,node,harvester,farmer,farmer-no-wallet,farmer-only,timelord,timelord-only,timelord-launcher-only,wallet,wallet-only,introducer,simulator}

**标识变量(Flags)**

`-r, --restart`: 可以重启服务进程，例如：
```
chia start timelord -r
```

## plots

### [create](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/plotting/create_plots.py)

命令：`chia plots create [add flags and parameters]`

**标识变量(Flags)**

`-k` [size]：定义农田的格式.，在不同的设备上开垦不同格式农田所花费的时间是不同的，详情查阅文档[农田格式解析](k-sizes)

`-n` [准备开垦的农田数量]：此开垦为队列任务，当一块农田开垦完后边会开始开垦下一块农田，与此同时，农田会被拷贝到 `-d` 参数所指定的目录，

`-b` [内存使用量，单位 MiB]：定义内存的使用量。默认为 4608 MiB（4.6 GiB），使用更多的内存，对开垦速度有少量提升。请注意，这里的内存仅包含了分配给开垦任务的使用量。还有系统代码运行，容器，文件系统等等都需要使用内存。

`-f` [农民公钥（farmer pk）]：通过农民公钥在别的机器上开垦农田，可以避免私钥信息泄露。使用命令： `chia keys show` 来查看你的农民公钥。

`-p` [矿池公钥（pool pk）]: 与农民私钥一同使用（旧版农田），在别的机器上开垦农田，可以避免私钥信息泄露。使用命令： `chia keys show` 来查看你的矿池公钥。

`-c` [矿池合约地址(pool contract address)]: 如果你开垦新版矿池协议农田的话，那“矿池合约地址”参数是用来替换“-p 矿池公钥”的，新版矿池协议的农田可以自耕种，也可以切换加入矿池集体耕作。使用命令： `chia plotnft show` 查看你的矿池合约地址。

`-a` [指纹(fingerprint)]: 密钥指纹用来选择将要使用的农民公钥及矿池公钥。这是为了方便在你的私钥本里选择要使用的账户。使用命令： `chia keys show` 查看你的私钥账户所对应的指纹密钥（纯数字）。

`-t` [临时文件目录(tmp dir)]: 指定开垦农田时的缓存文件目录。

`-t` [tmp dir]: Define the temporary directory for plot creation. This is where Plotting Phase 1 (Forward Propagation) and Phase 2 (Backpropagation) both occur. The `-t` dir requires the largest working space: normally about 2.5 times the size of the final plot.

`-2` [tmp dir 2]: Define a secondary temporary directory for plot creation. This is where Plotting Phase 3 (Compression) and Phase 4 (Checkpoints) occur. Depending on your OS, `-2` might default to either `-t` or `-d`. Therefore, if either `-t` or `-d` are running low on space, it's recommended to set `-2` manually. The `-2` dir requires an equal amount of working space as the final size of the plot.

`-d` [final dir]: Define the final location for plot(s). Of course, `-d` should have enough free space as the final size of the plot. This directory is automatically added to your `~/.chia/VERSION/config/config.yaml` file. You can use `chia plots remove -d` to remove a final directory from the configuration.

`-r` [number of threads]: 2 is usually optimal. Multithreading is only in phase 1 currently.

`-u` [number of buckets]: More buckets require less RAM but more random seeks to disk. With spinning disks you want less buckets and with NVMe more buckets. There is no significant benefit from using smaller buckets - just use 128.

`-e` [bitfield plotting]: Using the `-e` flag will disable the bitfield plotting algorithm, and revert back to the older b17 plotting style. After 1.0.4 it’s better to use bitfield for most cases (not using `-e`). Before 1.0.4 (obsolete) using the `-e` flag (bitfield disabled) lowers memory requirement, but also writes about 12% more data during creation of the plot. For now, SSD temp space will likely plot faster with `-e` (bitfield back propagation disabled) and for slower spinning disks, i.e SATA 5400/7200 rpm, **not** using `-e` (bitfield enabled) is a better option.

`-x` [exclude final dir]: Skips adding [final dir] to harvester for farming.

## Example Plotting Commands

Example below will create a k32 plot and use 4GB (note - not GiB) of memory.

`chia plots create -k 32 -b 4000 -t /path/to/temporary/directory -d /path/to/final/directory`

Example 2 below will create a k34 plot and use 8GB of memory, 2 threads and 64 buckets

`chia plots create -k 34 -e -b 8000 -r 2 -u 64 -t /path/to/temporary/directory -d /path/to/final/directory`

Example 3 below will create five k32 plots (`-n 5`) one at a time using 4GB `-b 4000` (note - not GiB) of memory and uses a secondary temp directory (`-2 /path/to/secondary/temp/directory`).

`chia plots create -k 32 -b 4000 -n 5 -t /path/to/temporary/directory -2 /path/to/secondary/temp/directory -d /path/to/final/directory`


**Additional Plotting Notes**

* During plotting, Phase 1 (Forward Propagation) and Phase 3 (Compression) tend to take the most time. Therefore, to maximize plotting speed, `-t` and `-2` should be on your fastest drives, and `-d` can be on a slow drive.

* There are 4 major phases to plotting. Phase 1 of plotting can utilize multi-threading. Phases 2-3 do not. You can better optimize your plotting by using the `-r` flag in your command and setting it to greater than 2, e.g,. `-r 2`. Above 4 threads there are diminishing returns. Many Chia users have determined it's more efficient to plot in parallel, rather than series. You can do this by just having multiple plotting instances open but staggering when they start 30min or more. 

* It's objectively faster to plot on SSD's instead of HDD's. However, SSD's have significantly more limited lifespans, and early Chia testing has seemed to indicate that plotting on SSD's wears them out pretty quickly. Therefore, many Chia users have decided it's more "green" to plot in parallel on many HDD's at once.

* Plotting is designed to be as efficient as possible. However, to prevent grinding attacks, farmers should not be able to create a plot within the average block interval. That's why the minimum k-size is k32 on mainnet.

## plotnft
Using the CLI, you can perform the same operations as with the GUI. There is a new command, called `chia plotnft`. Type `chia plotnft -h` to see all the available sub-commands:

```
» chia plotnft -h
Usage: chia plotnft [OPTIONS] COMMAND [ARGS]...

Options:
  -h, --help  Show this message and exit.

Commands:
  claim           Claim rewards from a plot NFT
  create          Create a plot NFT
  get_login_link  Create a login link for a pool. To get the launcher id, use
                  plotnft show.

  inspect         Get Detailed plotnft information as JSON
  join            Join a plot NFT to a Pool
  leave           Leave a pool and return to self-farming
  show            Show plotnft information
```

To create a Plot NFT, use `chia plotnft create -u https://poolnamehere.com`, entering the URL of the pool you want to use. To create a plot NFT in self-farming mode, do `chia plotnft create -s local`.
To switch pools, you can use `chia plotnft join`, and to leave a pool (switch to self farming), use `chia plotnft leave`.
The `show` command can be used to check your current points balance. CLI plotting with `create_plots` is the same as before, but the `-p` is replaced with `-c`, and the pool contract address from `chia plotnft show` should be used here.

## [check](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/plotting/check_plots.py)

Command: `chia plots check -n [num checks] -l -g [substring]`

First, this looks in all plot directories from your config.yaml. You can check those directories with `chia plots show`. This command will check whether plots are valid given the plot's associated keys and your machine's stored Chia keys, as well as test the plot with challenges to identify found plots vs. expected number of plots.

`-g` check only plots with directory or file name containing case-sensitive [substring].
**If `-g` isn't specified all plots in every plot directory in your config.yaml will be checked.**

Examples for using `-g`

* Check plots within a long directory name like `/mnt/chia/DriveA` can use `chia plots check -g DriveA`
* Check only k33 plots can use `chia plots check -g k33`
* Check plots created on October 31, 2020 can use `chia plots check -g 2020-10-31`

`-l` allows you to find duplicate plots by ID. It checks all plot directories listed in config.yaml and lists out any plot filenames with the same filename ending; `*-[64 char plot ID].plot`. You should use `-l -n 0` if you only want to check for duplicates.

`-n` represents the number of challenges given. If you don't include an `-n` integer, the default is 30. For instance, if `-n` is 30, then 30 challenges will be given to each plot. The challenges count from 5 (minimum) to `-n`, and are not random.

Each plot will take each challenge and:
* Get the quality for the challenge (Is there a proof of space? You should expect 1 proof per challenge, but there may be 0 or more than 1.)
* Get the full proof(s) for the challenge if a proof was present
* Validate that the ## of full proofs matches the ## of expected quality proofs.

Finally, you'll see a report the final true proofs vs. expected proofs.

Therefore, if `-n` is 20, you would expect 20 proofs, but your plot may have more or fewer.

Running the command with `-n 10` or `-n 20` is good for a very minor check, but won't actually give you much information about if the plots are actually high-quality or not.

Consider using `-n 30` to get a statistically better idea.

For more detail, you can read about the DiskProver commands in [chiapos](https://github.com/Chia-Network/chiapos/blob/master/src/prover_disk.hpp)

**What does the ratio of full proofs vs expected proofs mean?**
* If the ratio is >1, your plot was relatively lucky for this run of challenges.
* If the ratio is <1, your plot was relatively unlucky.
    * This shouldn't really concern you unless your ratio is <0.70 ## If so, do a more thorough `chia plots check` by increasing your `-n` 

The plots check challenge is a static challenge. For example if you run a plots check 20 times, with 30 tries against the same file, it will produce the same result every time. So while you may see a plot ratio << 1 for a plot check with `x` number of tries, it does not mean that the plot itself is worthless. It just means that given these static challenges, the plot is producing however many proofs. As the number of tries (`-n`) increases, we would expect the ratio to not be << 1. Since Mainnet is live, and given that the blockchain has new challenges with every signage point - just because a plot is having a bad time with one specific challenge, does not mean it has the same results versus another challenge.  "Number of plots" and "k-size" are much more influential factors at winning blocks than "proofs produced per challenge". 

**In theory**, a plot with a ratio >> 1 would be more likely to win challenges on the blockchain. Likewise, a plot with a ratio << 1 would be less likely to win. However, in practice, this isn't actually going to be noticeable.  Therefore, don't worry if your plot check ratios are less than 1, unless they're _significantly_ less than 1 for _many_ `-n`. 


## 其他命令 (not yet documented)

```sh
$ chia

选项:
  --root-path PATH  配置文件的根目录  [默认位置: /home/user/.chia/mainnet]
  -h, --help        显示以下信息。

命令:
  configure   修改配置文件
  farm        耕种管理
  init        创建或迁移配置文件
  keys        私钥管理
  netspace    全网空间算力
  plots       农田管理
  run_daemon  后台运行
  show        查看节点信息
  start       启动服务进程
  stop        停止服务进程
  version     查看chia的版本信息
  wallet      钱包管理

```
想要查看上面这些命令有哪些用途，使用 `-h` 查看帮助信息。比如： `chia show -h`。

使用命令 `chia show -s` 检查全节点的状态,将会看到类似如下所述信息。如果你的节点已同步，顶部会显示 `Full Node Synced` ，代表已同步至最新区块高度。

```
Current Blockchain Status: Full Node Synced

Peak: Hash: d8f070ba1425a8c8f30051e3ec28955892da914bc003871912858ca21fe71f52
      Time: Sat Sep 18 2021 22:44:54 CST                  Height:     876939

Estimated network space: 35.360 EiB
Current difficulty: 2992
Current VDF sub_slot_iters: 135266304
Total iterations since the start of the blockchain: 3169553883803

  Height: |   Hash:
   876939 | d8f070ba1425a8c8f30051e3ec28955892da914bc003871912858ca21fe71f52
   876938 | 41c60bb03abf56b1996a721f90ed911c050dc52ed4a1a30b9c429bbcbdc4b36e
   876937 | e65d3088e0c7f90f20fa5504d4c68eccbb5f1c9ee4d38b8549e500e22c3f0ec7
   876936 | c197a70110193ca96ab93a98220d1b614e678f858909eabfafc7d573ad8a033c
   876935 | 3e9d02c108d4e3ee216062f7f6859e5905cc7c2ccecdb43ecf08ee21568cd5f6
   876934 | 8c310b11f7969ce2fe201daddff5929ee98c19a1406fb50ccdf951986b5976ee
   876933 | 03be6b9b2767e0119936a192788d79548bf825481e83b5b246a0c4622df7fa62
   876932 | fcd2396220f747f5d4daf26ef30fc367372b3c1a5b52bef01c8800daad707a09
   876931 | b702267644ec14850fc4b71212d9c8041172ce9d762bd12e607bc054461ef841
   876930 | 8a4d607fecd35c74038b911597933ded1d3d81d663d35f51cf6ed8b8694079ae

```
通过 `chia plots add -d 'your_dir'` 或者 `chia plots remove -d 'your_dir'` 命令，可以添加或者删除所需耕种的农田目录， `chia plots add/remove -h` 命令可以查看两者的区别之处。

## 检查日志及运行状态

使用 `chia wallet show` 命令查看钱包信息， `chia farm summary` 命令查看耕种状态。

查询收割机及农民的日志信息： `grep ~/.chia/mainnet/log/debug.log -e harvester`

示例:

```
17:08:03.191 harvester harvester_server        : INFO     <- harvester_handshake from peer 214b269a425b8223cb50fbd458dab056599348e255f07a018c13ea9efb509ee5 127.0.0.1
17:08:03.194 farmer farmer_server              : INFO     -> harvester_handshake to peer 127.0.0.1 65f3fa0b0407a07da8ccf04dfa0f64c28f714726312aa051d3a8529390db4d7a
17:08:03.218 harvester src.plotting.plot_tools : INFO     Searching directories ['/home/user/slab1/plots']
17:08:03.227 harvester src.plotting.plot_tools : INFO     Found plot /home/user/slab1/plots/plot-k32-2021-01-11-17-26-bf2363828e469a3417b89eb98cfa9d694809e1ce8bef0ffd1d12853d4227aa0a.plot of size 32
17:08:03.227 harvester src.plotting.plot_tools : INFO     Loaded a total of 1 plots of size 0.09895819725716137 TiB
```
也可以使用命令： `tail -F ~/.chia/mainnet/log/debug.log` ，查看Chia的实时日志信息。
