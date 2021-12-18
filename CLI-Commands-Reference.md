翻译自[2021年8月31日版本-67#](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference/d2165e9af8a8afafa507e5d3d529ee082928b4db)

***
本文相对于在CLI中使用 `chia -h` 命令所获得的帮助信息，将提供更进一步的解释。

但并不意味着会更加全面，因为使用 `-h` 命令获得的帮助信息已经相当清晰明了。建议查看本文及其他相关教程时，先研究一下 `-h` 的帮助信息。

如果你想了解某个命令的选项有哪些，可以在后面加上 `-h` 参数。例如：

* `chia -h`
* `chia plots -h`
* `chia plots check -h`
* `chia plotters madmax -h`
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

* `chia start node` ，将只启动chia的全节点服务进程。
* `chia start farmer` ，将启动农民、收割机、全节点以及钱包进程。
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
## plotters
In 1.2.11 the option to use different plotters was introduced. The plotters supported include [Bladebit](https://github.com/Chia-Network/bladebit), [madMAx](https://github.com/madMAx43v3r/chia-plotter), and the original reference chiapos plotter. Each plotter has slightly different hardware requirements and may need slightly different options specified. Learn more about the alternative plotters here: [Alternative Plotters](https://github.com/Chia-Network/chia-blockchain/wiki/Alternative--Plotters)

### madmax
For details on settings and usage of madMAx please refer to the original [madMAx Github repository](https://github.com/madMAx43v3r/chia-plotter), or run the help command `chia plotters madmax -h`.

```
usage: chia madmax [-h] [-k SIZE] [-n COUNT] [-r THREADS] [-u BUCKETS] [-v BUCKETS3] [-t TMPDIR] [-2 TMPDIR2] [-d FINALDIR] [-w]
                   [-p POOL_KEY] [-f FARMERKEY] [-c CONTRACT] [-G] [-K RMULTI2]
Madmax Plotter
optional arguments:
  -h, --help            show this help message and exit
  -k SIZE, --size SIZE  K value.
  -n COUNT, --count COUNT
                        Number of plots to create (default = 1)
  -r THREADS, --threads THREADS
                        Num threads.
  -u BUCKETS, --buckets BUCKETS
                        Number of buckets.
  -v BUCKETS3, --buckets3 BUCKETS3
                        Number of buckets for phase 3+4 (default = 256)
  -t TMPDIR, --tmp_dir TMPDIR
                        Temporary directory 1.
  -2 TMPDIR2, --tmp_dir2 TMPDIR2
                        Temporary directory 2.
  -d FINALDIR, --final_dir FINALDIR
                        Final directory.
  -w, --waitforcopy     Wait for copy to start next plot
  -p POOL_KEY, --pool-key POOL_KEY
                        Pool Public Key (48 bytes)
  -f FARMERKEY, --farmerkey FARMERKEY
                        Farmer Public Key (48 bytes)
  -c CONTRACT, --contract CONTRACT
                        Pool Contract Address (64 chars)
  -G, --tmptoggle       Alternate tmpdir/tmpdir2 (default = false)
  -K RMULTI2, --rmulti2 RMULTI2
                        Thread multiplier for P2 (default = 1)
```

### bladebit
`chia plotters bladebit -h`

```
usage: chia bladebit [-h] [-r THREADS] [-n COUNT] [-f FARMERKEY] [-p POOL_KEY] [-c CONTRACT] [-i ID] [-w] [-m] [-d FINALDIR] [-v]
Bladebit Plotter
optional arguments:
  -h, --help            show this help message and exit
  -r THREADS, --threads THREADS
                        Num threads.
  -n COUNT, --count COUNT
                        Number of plots to create (default = 1)
  -f FARMERKEY, --farmerkey FARMERKEY
                        Farmer Public Key (48 bytes)
  -p POOL_KEY, --pool-key POOL_KEY
                        Pool Public Key (48 bytes)
  -c CONTRACT, --contract CONTRACT
                        Pool Contract Address (64 chars)
  -i ID, --id ID        Plot id
  -w, --warmstart       Warm start
  -m, --nonuma          Disable numa
  -d FINALDIR, --final_dir FINALDIR
                        Final directory.
  -v, --verbose         Set verbose
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

`-t` [缓存目录(tmp dir)]: 指定开垦农田时的缓存文件目录。用以存储开垦过程中，第一阶段（前向传播运算）及第二阶段（反向传播运算）的缓存文件。保守起见，缓存文件目录通常需要2.5倍最终农田文件大小的空间，例如开垦K32格式农田（101 GiB）则需要252.5 GiB。

`-2` [第二缓存目录(tmp dir 2)]: 指定开垦农田时的第二缓存文件目录。用以存储开垦过程中，第三阶段（压缩）及第四阶段（生成检查点）的缓存文件。 `-2` 的路径默认为 `-t` 或者 `-d` 的目录。因此，根据实际情况，如果 `-t` 或者 `-d` 的目录空间不足时，建议手动设置一下 `-2` 的目录。 `-2` 目录所需空间最小为所开垦农田的文件大小，例如开垦K32格式农田（101 GiB）则需要101 GiB。.

`-d` [最终文件目录(final dir)]: 指定开垦完成的农田最终所储存的目录。 `-d` 需要有足够的空间以便存储所需开垦的农田文件。这个目录会自动添加到 `~/.chia/VERSION/config/config.yaml` 配置文件中去。可以使用命令 `chia plots remove -d` 将农田文件目录路径从配置文件中删除。

`-r` [使用线程数(number of threads)]：2个线程是最优的选择，多线程仅在第一阶段有效。

`-u` [桶数(number of buckets)]:桶数越多，使用的内存越少，但对缓存目录磁盘的随机读写能力要求较高。因此，使用机械硬盘开垦的话，就减少桶数使用，使用SSD/NVME的话，就增加桶数。使用较小的桶数并没有什么明显的效果，所以建议使用默认桶数：128.

`-e` [是否开启位域(bitfield plotting)]: 使用 `-e` 参数标记表示开垦过程中禁用位域排序，使用b17版本的开垦方式。自1.0.4版本起，开启位域排序（即，不添加 `-e` 参数标记）对开垦速度有提升。老版禁用位域的开垦方式，可以减少内存使用，但会增加12%的缓存文件。所以，使用SSD/NVME开垦农田可以添加 `-e` 参数标记（即，启用位于排序），而使用机械硬盘开垦的，即，5400转或者7200转的SATA硬盘，**禁用**位域排序是最合适的选择。

`-x` [忽略最终目录(exclude final dir)]: 不将此次开垦任务的最终文件目录添加至正在耕种的农田目录。

## 开垦命令示例

示例一，使用4 GB（注意不是GiB）内存开垦一个K32农田。

`chia plots create -k 32 -b 4000 -t /缓存/文件/目录 -d /最终/文件/目录`

示例二,使用8 GB内存、2个线程、64桶数开垦一个K34农田。

`chia plots create -k 34 -e -b 8000 -r 2 -u 64 -t /缓存/文件/目录 -d /最终/文件/目录`

示例三，使用4 GB内存来启动一个队列任务，共开垦5个农田（`-n 5`），同时指定第二缓存目录。

`chia plots create -k 32 -b 4000 -n 5 -t /缓存/文件/目录 -2 /第二/缓存/目录/t -d /最终/文件/目录`

**关于开垦的其他注意事项**

* 开垦期间，往往在第一阶段（前向传播运算）和第三阶段（压缩）所消耗的时间最多。因此，为了尽可能地加快开垦速度， `-t` 及 `-2` 的目录就需要使用读写速度快的储存设备（NVME/SSD），`-d` 目录则使用一般的存储设备即可（机械硬盘）。

* 开垦过程主要由4个阶段组成。第一阶段可以利用多线程。第二、三阶段无法使用多线程。设置 `-r` 的参数值大于2，即，一阶段参与开垦的线程数大于2，可以优化开垦的速度，例如， `-r 3` 。超过4个线程以后，开垦速度的增益效果将显著降低。相较于队列任务开垦农田，很多人会选择更高效的并行开垦模式。同时启动多个开垦任务程序即可进入并行开垦模式，不过建议程序之间设置30分钟以上间隔时间，使得开垦阶段的大量读写工作可以交错进行，以便更高效地利用磁盘的读写效率。

* 在SSD上开垦农田要比在HDD上速度快的多。尽管如此，SSD的寿命却是有限的，在早期官方测试表明，在SSD上开垦农田将会迅速消耗SSD的寿命（TBW）。因此，有很多用户会选择在多个HDD硬盘上同时开垦农田，可以减少硬件设备的磨损，也就更环保一些。
* 开垦程序的设计是尽可能地高效完成农田。然而，为了防止“粉碎攻击”，在平均区块生成的时间间隔之内，不允许有农民可以在这个时间间隔内完成开垦农田。这也就是为什么，主网只支持K32及以上格式的农田。

## 农田产权证（plotnft）

关于plotnft，命令行工具能完成的操作，GUI客户端一样可以完成。这是新增的一个命令`chia plotnft`。输入 `chia plotnft -h` 查看有效的子命令。

```
» chia plotnft -h
用法: chia plotnft [选项] COMMAND [参数]...

选项:
  -h, --help  Show this message and exit.

命令:
  claim           从智能合约地址（农田产权证）领取爆块后的奖励（仅适用于自耕种）
  create          创建一个农田产权证
  get_login_link  创建一个矿池的登陆链接。查看启动器ID，使用命令 `chia plotnft show`

  inspect         获得JSON格式的plotnft详细信息
  join            将一个农田产权证绑定某个矿池
  leave           解绑矿池，切换至自耕种状态
  show            查看plotnft详细信息
```

使用命令：`chia plotnft create -u https://矿池地址` 创建一个农田产权证，输入你想加入的矿池的地址。也可以创建一个自耕种的农田产权证，使用命令：`chia plotnft create -s local`。

切换矿池使用命令：`chia plotnft join` ,离开矿池切换至自耕种使用命令：`chia plotnft leave` 。

 `chia plotnft show` 命令可以查看矿池耕种所获得的积分详情。开垦矿池协议农田的命令跟之前一样，但需要注意的是，需要将 `-p` 矿池公钥替换成 `-c` 矿池协议智能合约地址，使用命令 `chia plotnft show` 查看矿池协议智能合约地址（Pool contract address，xch开头）。

## [check](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/plotting/check_plots.py)

命令: `chia plots check -n [挑战次数] -l -g [子项]`

默认检查 `config.yaml` 中所设置农田文件目录路径下所有的农田。使用命令 `chia plots show` 查看已添加的农田目录。`check`命令可以检查农田是否与你的私钥账户保持有效的关联，同时
First, this looks in all plot directories from your config.yaml. You can check those directories with `chia plots show`. This command will check whether plots are valid given the plot's associated keys and your machine's stored Chia keys, as well as test the plot with challenges to identify found plots vs. expected number of plots.

添加`-g` 参数，可以检查指定目录下的，或者指定账户私钥关联的，也可以是指定格式的农田，等等。
**如果未添加 `-g` 参数，那么所有 `config.yaml` 配置文件中所设置的耕种目录下的农田文件，都将被检查**

 `-g` 参数使用示例

* 检查某个目录下的农田文件，例如 `/mnt/chia/DriveA` 目录，示例命令： `chia plots check -g DriveA`
* 只检查K33格式的农田，示例命令： `chia plots check -g k33`
* 检查2020年10月31日所创建的农田，示例命令： `chia plots check -g 2020-10-31`

`-l` 参数可以帮助你找出重复的农田文件（已添加至`config.yaml`配置文件中的），使用命令：`chia plots check -l -n 0`,如果有重复的农田，将会输出：`*-[64 char plot ID].plot` ，即重复农田的地址及文件名称。

`-n` 为需要给定的挑战次数. 如果没有设置 `-n` 的参数值，那么默认为30，然后每一块农田将会进行30次挑战，来找出符合测试的证明。挑战次数的参数值最小为5，且不是随机的。

每一个农田都将进行挑战测试:

* 获取农田的挑战质量（每个农田在每次挑战中可能没有符合的证明，也可能有多个，这个看运气。）
* 获取符合挑战的证明。
* 验证测试所获得的证明数与预期的是否匹配。

最后会输出一份报告，将会列出测试所获得证明数与预期证明数的对比。

因此，如果 `-n` 参数值是20，那么测试预期每一块农田有20个证明，但实际可能会大于20，也可能小于20。

运行命令时将 `-n` 参数值设为10或者20是最佳的最小测试数。

 `-n` 参数值为30可以获得一个更具有参考意义的挑战测试，以此来了解你的农田质量。

想了解更多相关信息，你可以查阅[chiapos](https://github.com/Chia-Network/chiapos/blob/master/src/prover_disk.hpp)中的 `prover_disk.hpp` 源代码。

**测试所得证明与预期证明的比率有什么含义？**

* 如果比率大于 1 ，在本次挑战测试中，农田的幸运值较高。
* 如果比率小于 1 ，在本次挑战测试中，农田的幸运值较低。
  * 幸运值比率小于1的话不用担心。如果比率小于0.7，可以通过增加 `-n` 的参数值，来做一次更加彻底的农田检查。

农田挑战测试是一个静态的挑战。比如，如果你对同一块农田进行了20次检查任务，每次都指定了30次挑战，那么每次你都会获得相同的结果。因此当你在做了x次挑战测试以后，可能会发现农田的幸运值比例一直会小于1，但这并不表明这块农田是没有价值的，这只能说明这块农田在这些静态挑战中运气不好，它还是包含了有很多证明的。随着挑战测试次数 (`-n`) 的增加，这个幸运期望值将不会一直小于1。由于主网的挑战是实时变动的，每次区块奖励伴随着新的挑战，农田在一次挑战中无法找到符合的证明，并不意味着它会在后面的挑战中失败。所以，“农田数量多少”和“农田格式大小”才是赢取区块奖励的重要原因，“农田内证明的质量及幸运值”并非关键。

**理论上**，农田测试挑战幸运值大于 1 的话，将更有可能赢取区块奖励。同样的，小于1则赢取奖励的概率相对较小。尽管如此，实际上幸运值影响并不明显。因此，在进行普通挑战测试时，不用担心农田的幸运者比率小于 1。除非测试时，`-n`的次数 _相当多_ ，且农田的幸运者也 _一直小于1_ 。

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
