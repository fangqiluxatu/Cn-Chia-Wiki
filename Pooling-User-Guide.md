- [六步加入矿池](#六步加入矿池)
  - [第一步-同步全节点及钱包](#第一步-同步全节点及钱包)
  - [第二步-获取手续费](#第二步-获取手续费)
  - [第三步-创建“农田产权证”](#第三步-创建农田产权证)
  - [第四步-开垦农田](#第四步-开垦农田)
  - [第五步-管理你的“农田产权证”](#第五步-管理你的农田产权证)
  - [第六步-等待支付收益](#第六步-等待支付收益)
- [补充信息](#补充信息)
  - [多个农田产权证](#多个农田产权证)
  - [多机操作](#多机操作)
  - [Multiple Keys](#multiple-keys)
  - [Pool Fees](#pool-fees)
  - [Blockchain Fees](#blockchain-fees)
  - [Invalid State](#invalid-state)
  - [Payout addresses](#payout-addresses)
  - [Self Pooling](#self-pooling)
  - [Remote Harvesters](#remote-harvesters)
  - [Command Line Interface](#command-line-interface)

矿池协议相关问题可以查看此[文档](Pooling-FAQ)

由于区块网络的总空间算力日益增长。通过独自耕种来赢取区块奖励变得越来越难，有的可能需要花费好几个月甚至一两年。对于空间算力较小的农民来说，加入矿池就可以获得更稳定的收益。比如你独自耕种的话，系统爆块预期是20个星期一次（2XCH），但如果加入矿池就能每星期获得0.1XCH，这样你的收益更加稳定以便做好相应规划。

Chia矿池协议允许你将农田分配给你所创建部署在Chia主网上的一个智能合约，即“农田产权证（Plot NFT）”。这样就能通过操控农田产权证来加入你选择的矿池，当然，你也可以随时切换矿池。

通过矿池协议，你的农田就可以与你想加入的矿池（农业合作社）进行连接，并开始不停地向矿池供应商发送空间证明，以此来证明你所拥有的空间算力大小。因此，矿池时刻追踪着每个农民的空间算力大小，每当有农民赢得了区块奖励时，矿池就会根据每个农民的空间算力占比来分配奖励。

所以，矿池就像是一种中奖保险（低保），矿池内所有的农民都在参与Chia网络的抽奖活动，每当有人中奖了，矿池就会按每个人的空间算力占比来给各位分配奖励，只需要扣除一点儿手续费。这其中，每次爆块的1.75个XCH是分配给池内所有农民的，0.25个XCH则是直接奖励给抽中奖励的农民。

## 六步加入矿池
旧版农田 (OG plots) 只能用于个人独自耕种，无法加入符合矿池协议的矿池。想要加入矿池的话，你需要创建新版农田。不过不用担心，老版农田依旧可以与新版农田一起耕种。

### 第一步-同步全节点及钱包
首先，你的Chia版本必须为1.2.0及以上，然后全节点及钱包需保持已同步状态。

![wallet_sync](images/wallet_sync.png)

重要提示： 如果你在多台主机上登陆了你的Chia账户（24位助记词），在你开垦新的农田时，你需要将所有主机的Chia客户端升级为1.2.0及以上。如果你已经创建了“农田产权证”（plot NFT），其它机子上旧版本的Chia软件（1.2.0以下的版本）是无法显示查看到的。需要你升级软件并且删除 `~/.chia/wallet/db` 目录下钱包的数据，然后重新打开Chia同步钱包。


### 第二步-获取手续费
开始创建“农田产权证”并加入矿池之前，你的钱包需要留一点儿XCH。你可以问你的朋友要几个mojo的XCH，1 mojo等于0.000000000001 XCH。或者你可以访问网站 https://faucet.chia.net/ ，填写你的钱包收款地址，你将会收到几个mojo的XCH，以便后续操作。


### 第三步-创建“农田产权证”
当你的钱包存有足够手续费的 XCH 时，点击矿池（农业合作社）分栏，点击“添加一个农田产权证（Add a Plot NFT）”。此时你有两个选择：
1. 自耕种：创建的农田产权证不连接到任何矿池，爆块后的其中1.75个 XCH 会直接发送到你的钱包里。这个跟老版农田不太一样，老版农田已经被永久锁定在独自耕种模式里了。使用CLI创建的话，用下列命令：
```bash
chia plotnft create -s local
```

2. 连接到矿池：加入矿池并立即开始集体耕种，当然，你还需要在该“农田产权证”下开垦一些农田。使用CLI创建的话，用下列命令：
```bash
chia plotnft create -s pool -u https://bar.examplepool.org
```

注意，即使你创建“农田产权证”时选择了自耕种模式，你依旧可以在以后加入矿池耕种。如果你决定了想要加入的矿池，输入矿池地址（必须是 _https://_ 开头），然后查看说明，创建“农田产权证”（点一次就行了，别点多了，软件及网络有延迟），等几分钟主网确认后。你可以在矿池（农业合作社）分栏看到你新建的“农田产权证”，创建一个就够了。

![join_pool](images/join_pool.png)


### 第四步-开垦农田
现在，你可以在此“农田产权证”下开垦新的农田了，新的农田可以加入矿池进行集体耕种以便你能获得更稳定的收益。点击你所创建的“农田产权证”右上角的三个点，单击“开垦农田”，界面会跳到开垦农田。如果你在加入矿池（农业合作社）选项这里没有选择任何“农田产权证”，那么你开垦的将是老版农田（也就说，无法加入矿池，只能自耕种）。选择了“农田产权证”，所开垦的农田将被永久绑定在此“农田产权证”上。

打开矿池（农村合作社）分栏界面，把鼠标悬浮在“农田产权证”名字的问号上，复制智能合约地址（xch开头的）。这个地址将在你使用CLI或者第三方工具来开垦农田时用到，即参数 `-c` 的值。

注意：当你使用CLI工具开垦农田时，不要再使用 `-p` 参数了，而是用 `-c` 代替。其他参数的设置方式还跟以前一样。

### 第五步-管理你的“农田产权证”
在农业合作社界面查看你的“农田产权证”。状态会显示“合作社耕种”。从这里你看到当前难度、所获得的积点数等等。

难度表示该“农田产权证”下的农田所能找到有效证明的困难度估值。这个值是矿池自动设置的，以便决定你的农田多长时间提交一次证明（每几分钟或者几小时）。该“农田产权证”下所拥有的农田数量越多，那么他所获得的难度值就越大，以此降低硬盘的使用率。（[参照矿池FAQ解答](Pooling-FAQ#难度是如何影响计算农民的空间总算力？)）

积分点数用来计算你的农田提交了多少证明。每个K32农田每天可以获得10点积分，这个数值的变化与难度无关。积分的大小不等同与你所获得XCH数量，这只不过反应了你耕种的工作量，把它当做一个计算工具就行了。矿池根据你所获得的积分点数来定期分配XCH收益，分配完后积分点数便会重置为0。

想要切换矿池的话，只需要点击“更换农业合作社”，然后输入新矿池的网络地址。注意，切换矿池的过程中需要你等待一段时间，可能是几分钟，甚至一小时。所以，在切换过程中，请不要关闭Chia客户端，直到切换完成为止。在耕种期间，随时可以切换矿池，这么做不需要注册账户，也不会有人妨碍处罚。切换到新矿池以后，老矿池将不会再向你支付耕种收益。

确认一下最近24小时获取的积点是否准确。每块K32农田每天可以获得10点积分，也就是说100块K32农田每天可以获得1000点积分。关注一下你的积分余额是否保持增长，当你获得矿池支付的收益以后，当前积分余额便会重置归零。积分的增长变化是随机不定的，因为它是跟着所提交的证明来变化的。因此，预期会有一定的变数，好运和厄运也会共存。

![points](images/points.png)

### 第六步-等待支付收益
判断是否正常接入矿池，只需要关注你的积点余额是否在增长。同时你可以选择登陆矿池，来查看你多长时间可以获得一次收益。

## 补充信息

### 多个农田产权证
同一个账户（密钥）可以有多个农田产权证，不同农田产权证下的农田可以同时在线耕种。老版农田一样也可以同时在线耕种（自耕种，非矿池）。

### 多机操作

You can take your 24 words and enter them into a different computer, and when it is synched, the current Plot NFTs and
pool information will be automatically downloaded from the blockchain. All information about your pool, plot NFTs, and smart contract addresses is completely backed up on the blockchain, and can be recovered using the 24 words.
Make sure to read the important note on step 1, about updating to new version on all computers.

### Multiple Keys
You can also have multiple keys farming at the same time, but be careful with this. Each key has to sync separately,
and if you change pools in one computer (computer A), then you must sync up your wallet on computer B in order to farm
it separately. If computer B has multiple keys, make sure to sync each key up to the latest changes in the Plot NFT.


### Pool Fees
Pool fees refer to the small cut that pools take when they distribute rewards to farmers. 

### Blockchain Fees
Blockchain fees are fees are paid to the creator of the block (farmers), to incentivize them to include your transaction. Fees are currently 0, but they are likely to rise as the blockchain gets more usage. When fees rise, you might have to
pay small amounts of Chia to make transactions. Sending XCH to an address is a blockchain transaction, but creating a plot NFT, or changing pools is also a transaction, which requires fees. The user interface will be updated to include fields for fee amounts, and guidance will be provided here when fees become necessary.

### Invalid State
If you enter into an invalid state, you need to re-join or change to self-pooling again. This can happen if you close
the GUI before a pool switching operation has finalized. Please click "change pool", and re-enter the pool URL, or switch to self pooling. Sometimes you might need to wait a bit for the pool switching timeout to finish.


### Payout addresses
The block reward is divided into two components, the 1.75 XCH pool portion and the 0.25 XCH farmer portion. The 0.25 XCH will go to your farmer target address, which is the same as the OG plots. This is configurable in the Farm tab of the GUI, or in the `config.yaml` under farmer.xch_target_address.
The 1.75XCH gets paid out to the pool, and the payout instructions that the pool will use to pay you can be set in the config file in the pool_list section.


### Self Pooling
If you are self-pooling, you will additionally need to claim your rewards after winning a block. This can be done from the GUI or CLI as well. There is no time limit for this, but if you do not claim your rewards before switching to a pool, the pool will be able to claim those rewards, and you will lose these funds.

### Remote Harvesters
Remote harvesters work the same way as always. They do not need to have any keys, and you can plot directly on another machine with the `-f` and `-c` arguments. The farmer machine needs to have the private key for the `-f` key, and the private key for the wallet that created the plot NFT. Your harvesters will find proofs more often when pooling, since the difficulty is lower. Remote harvester plots will now be visible by doing `chia farm summary`.


### Command Line Interface
Using the CLI, you can perform the same operations as with the GUI. There is a new command, called `chia plotnft`. Type `chia plotnft -h` to see all the available sub-commands:

```
» chia plotnft -h
使用方式: chia plotnft [OPTIONS] COMMAND [ARGS]...

选项:
  -h, --help  显示帮助信息.

Commands:
  claim           claim reward from a plot NFT，从农田产权证领取矿池部分奖励（一个块1.75xch，仅自耕种状态）
  create          create a plot NFT，创建农田
  get_login_link   Create a login link for a pool. To get the launcher id, use plotnft show.创建一个矿池登陆链接，使用'chia plotnft show获取启动器ID'

  inspect         Get Detailed plotnft information as JSON，获取农田产权证的详细信息（JSON格式）
  join            Join a plot NFT to a Pool，将某个农田产权证分配给一个矿池
  leave           Leave a pool and return to self-farming，退出矿池，回到自耕种状态
  show            Show plotnft information，显示农田产权证详细信息
```

To create a Plot NFT, use `chia plotnft create -u https://poolnamehere.com`, entering the URL of the pool you want to use. To create a plot NFT in self-farming mode, do `chia plotnft create -s local`.
To switch pools, you can use `chia plotnft join`, and to leave a pool (switch to self farming), use `chia plotnft leave`.
The `show` command can be used to check your current points balance. CLI plotting with `create_plots` is the same as before, but the `-p` is replaced with `-c`, and the pool contract address from `chia plotnft show` should be used here.