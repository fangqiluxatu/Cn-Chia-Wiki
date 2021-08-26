# 一般问题

## 如何加入矿池？

需要将Chia升级到1.2.0版本以上，然后查阅[Pooling User Guide](Pooling-User-Guide)中的说明。

## 加入官方矿池协议的矿池（后续统称为：新版矿池）的话就必须要重新开垦农田（P图）吗？

是的，想要加入并使用新版矿池，就需要创建新的K32格式农田或者更高格式的（K33等）。新格式的农田可以在矿池和自耕种池中自由切换，每次切换需要等待30分钟左右（100个区块时间），因为切换矿池需要向主网的智能合约发送信息。建议大家可以慢慢地把从老的农田一个一个的切换成新格式的农田，所以在你重新开垦农田的过程中依旧有机会赢取区块奖励（XCH）。

## 什么是农田产权证（含有非同质化代币的农田，plot NFT）

农田产权证是一种主网发行的智能通证（代币），用来帮助用户管理农田与矿池的关系。用户可以在任何时间通过使用农田产权证，来将自己的农田分配给自己想要加入的矿池。当你开垦农田时，需要选择你所创建的农田产权证，这样你所开垦的农田会永久地绑定在该农田产权证上。不同的农田产权证（plot NFTS）是不可互相替代的，每一个农田产权证可以当作是一个独一无二的矿池智能合约。

## 创建农田产权证或者切换矿池的时候需要付手续费吗？

创建农田产权证最少需要1mojo的XCH（0.000000000001 XCH）作为创建智能合约的费用，以及主网转账手续费（目前不需要）。切换矿池只需要支付一点儿转账手续费，在矿池协议刚上线的这段时间里，切换矿池可能不需要手续费。如果你没有XCH的话，可以去官方空投网站里填写你的XCH地址，领取100mojo: https://faucet.chia.net/ 。

## 可以同时耕种老版农田以及新版农田（带农田产权证的）吗？

可以。支持农民同时耕种老版农田以及新版农田。老图如果赢得了挑战，0.25XCH（农民奖励）和1.75XCH（矿池奖励）都会发送给农民。老的农田和新的农田可以一起耕种，互相不受影响。

## 如何为新开垦的农田添加产权证，以便后续加入矿池？
首先，你需要在农业合作社（POOL）分栏里创建一个农田产权证。当你开垦一个新版农田时，需要为农田分配一个农田产权证（使用命令行CLI工具开垦的话，需要将-p参数更换为-c参数，-c参数后面填写你生成农田产权证时所生成的XCH地址），被赋予相同农田产权证的农田可以被分配给同一个矿池里进行耕种。当然了，一个账号可以生成多个农田产权证。

## 密钥及钱包在Chia中的区别？
一台运行Chia的计算机可以生成一个或多个密钥（key），密钥由私钥信息（助记词）及公共指纹（身份）组成。当你使用客户端或者命令行时，一次只能操作一个账号（密钥）。不同密钥（账号）的钱包账本（区块）数据是不一样的，所以需要在“钱包”分栏确认是否已同步至最新区块高度。每个密钥（账号）可以分配多个钱包地址。打开软件登录账号以后的默认钱包是Chia自动生成的钱包地址，就像农田产权证一样，你也可以创建多个钱包，每个钱包都有不一样的地址（XCH开头），这些你所生成的钱包及农田产权证都是跟你的密钥所绑定的，“钱包”分栏内所显示的余额是你所有钱包地址余额的合计。

# Chia官方矿池协议与其他加密货币项目有什么不同？
主要有三个区别：
* 加入矿池无需签名授权，也不需要在矿池供应商注册账号。
* 农民挑战证明成功爆块后获得0.25个XCH（需扣除转账手续费），余下的1.75个XCH（需扣除矿池手续费）与矿池内所有的农民平分。
* 挑战成功并获得区块奖励的是农民，而不是矿池供应商，农民与矿池之间通过智能合约（农田产权证）来进行管理。

## 我如何自行运营矿池？
如果你有其他加密货币项目的矿池服务器运维经验，可以根据Chia矿池源代码来进行定制化修改。我们只会给大家推荐一些安全、有经验的矿池供应商。如果你运营了矿池，请遵守当地的法律法规，不限于纳税、反洗钱、客户身份等。同时，所有的Chia矿池都有可能成为黑客攻击的目标（主要是服务器方面的，例如DDOS攻击、安全漏洞等），如果矿池内的农民有任何损失，你需要承担相应的责任。

## 矿池列表在哪看？
社区做了一个Chia矿池列表网站：https://miningpoolstats.stream/chia

## 我能在官方讨论组里为我的矿池打广告吗？
你一天只能在Keybase @chia_network.public的pools频道里为你的矿池打一次广告。如果你滥发广告会受到管理员的警告直至禁言。

## 为什么不推荐加入Hpool的老版农田矿池？
Hpool在一开始重新构建了一个Chia客户端版本（1.2.0以前的版本），但他们没有开源这些修改内容，所以也就不知道这个客户端是否含有不安全的修改。Chia项目官方不支持任何人加入由封闭未开源软件所构建的矿池，不过Hpool已经针对新版官方矿池协议开设了矿池，你可以根据你自己的意向来选择是否加入hpool的新矿池。

## 为什么官方不运营自己的矿池？
我们希望构建一个健康、无特权且有活力的矿池生态环境，官方如果开始矿池了的话，对其它矿池不公平。

## 我的矿池能起名叫chiapool吗？
我们不允许矿池起以Chia、the chia pool开头的名字。你可以起名叫“a chia pool”，不过需要申请一下许可证，请浏览https://www.chia.net/terms/ 来获取关于许可证的更多信息。

## 如果某个矿池获得了51%的全网空间（算力），他们可以篡改主网吗？
不行，Chia矿池协议是这样设计的，农民与矿池之间是通过农田产权证的智能合约来相互绑定的，区块的打包及区块奖励挑战依旧是由农民完成的，但由于与矿池做了智能合约绑定，所以农民的矿池奖励（1.75XCH）会自动发送到已绑定矿池的钱包地址。这个机制的优点是，即使有矿池拥有全网51%的算力空间，那它还需要控制全网51%农民的节点，以便对主网进行恶意操作（51%攻击）。这是相当难以完成的事情，除非全网51%的农民下载运行了同一个恶意修改过的Chia客户端。（官方还在这尬吹了一下Bram Cohen ==！）

## 我还有疑问，在哪咨询？
加入官方Keybase讨论组：[@chia_network.public#pools](https://keybase.io/team/chia_network.public)

友情提示：别在群里瞎艾特官方开发人员以及管理员，直接陈述问题就行了，有空会回复你的。

# 技术问题
## 在哪查看Chia矿池案例的源代码？
Github上查看： https://github.com/Chia-Network/pool-reference.

自述文件概述了矿池的运行原理、解析以及建立运行矿池的细节。

## 矿池案例的是用什么语言开发的？
Python

## 根据矿池案例代码进行定制化困难吗？
如果你有矿池方面的开发经验，那么这对你来说很容易上手。只需要把POW挖矿概念替换成chia的POST挖矿方式，通过农民与矿池之间的智能合约来完成区块奖励（XCH）的收集与分配。

## 我是个程序员，但从来没有写过矿池的代码，可以根据矿池案例代码定制化自己的矿池吗？
如果你第一次开发矿池，建议你先看一下BTC或者ETH矿池的源代码及用户端的功能。你的矿池可能会面临来自加密社区的大型矿池供应商挑战，他们将在矿池协议上线的第一天就提供了丰富的功能，例如：排行榜、区块钱包浏览器、随机空投、阶梯手续费等等。

## 矿池代码中的变量名
- puzzle_hash: 交易地址一种不同的格式. Addresses are human readable.
- singleton: 一种智能合约（通证），用来保证这是独一无二并由用户控制。
- launcher_id: 智能合约的ID号（独一无二的）
- points: 表示农民已完成的耕作量。它根据用户提交的证明（农田）数量以及难度进行加权计算的。一个K32的农田每天可以获得10个点，一天想要获得1000点就需要10TiB的农田。这个就相当于POW矿池中的算力占比。

## 如何计算农民的空间算力大小？


## How does one calculate a farmer's netspace?
A farmer's netspace can be estimated by the number of points submitted over each unit of time, or points/second.  Each k32 gets on average 10 points per day. So `10 / 86400 = 0.0001157 points/second` for each plot. Per byte, that is `L = 0.0001157 / 106364865085 = 1.088 * 10^-15`. To calculate total space `S`, take the total number of points found `P`, and the time period in seconds `T` and do `S = P / (L*T)`.  
For example for 340 points in 6 hours, use `P=340, T=21600, L=1.088e-15`, `S = 340/(21600*1.088e-15) = 14465621651619 bytes`. Dividing by `1024^4` we get `13.15 TiB`. 

## How does difficulty affect farmer's netspace calculation?
As difficulty goes up, a farmer does less lookups and finds less proofs, but does not receive more points per unit of time. Imagine this scenario: Obtaining 10 proofs a day with difficulty 1 for a k32, is equivalent to obtaining 1 proof a day with difficulty 10. As a pool server, you prefer to receive 1 proof a day per K32 with difficulty 10. This is why we allow pool servers to set a minimum difficulty level to reduce the number of proofs each farmer needs to send to prove their netspace.

## How do you identify the farmer that submitted partial proofs?
The farmer will provide their launcher_id which is the ID of that farmer's pool group. The pool also verifies the proof of space and the farmer's signature, to make sure that only real farmers are compensated.

## Will pool servers need to keep track of all farmers and their share of rewards?
Yes, the pool operator will need to write code to keep track of all farmers and their share of rewards. Chia's pool protocol assumes no registration is needed to join a pool, so every launcher_id that submits a valid partial proof needs to be tracked by the pool server.

## What actions can singleton take?
There are a few things you can do to the singleton:
- Change pool (needs owner signature)
- Escape pool, this is announcing that you will change pool (needs owner signature)
- Claim rewards (does not need any signature, it goes to the specified address in the singleton)

## How do pool collect rewards?
- Farmer joins a pool, they will assign their singleton to the pool_puzzle_hash.
- When a farmer wins a block, the pool rewards will be sent to the p2_singleton_puzzle_hash.
- Pool will scan blockchain to find new rewards sent to Farmer's singletons.
- The pool will send a request to claim rewards to the winning Farmer's singleton.
- Farmer's singleton will send pool rewards XCH to pool_puzzle_hash.
- Pool will periodically distribute rewards to farmers that have points

## How can I tell if the server is receiving enough partials from a particular client? 
The number of partials received is the only thing the pool is aware of, the pool does not know the exact total space
of the farmer. The space can be computed using the fact that each k32 plot will earn on average 10 points a day, on 
mainnet. That means if the difficulty is set to 1, that's 10 partials per day, if the difficulty is 10, 1 partial per 
day per k32 plot.

## Why am I receiving more points in testnet than mainnet?
The 10 points per day per k32 plot only applies to mainnet, which has a `DIFFICULTY_CONSTANT_FACTOR` of 2^67. To get
the points per day per k32 on testnet, divide 2^67 by the testnet `DIFFICULTY_CONSTANT_FACTOR`, found in `config.yaml`, and
multiply by 10. This allows participating easily with k25s on testnet.

## What is the expected ratio between a k32 and a k25? 
Look at the file `win_simulation.py` on this repo. This uses the function `_expected_plot_size` from chia blockchain,
which uses the formula:  `((2 * k) + 1) * (2 ** (k - 1))` to compute plot size. Plug in your k values and divide.

## How to calculate how many partials with X difficulty a certain plot with Y size can get in Z time?
Look at the `win_simulation.py` file.

## Can I use testnet pooling plots on mainnet?
No, you can only use plots created for mainnet in mainnet, and same for testnet.

## Does that mean that forks of Chia cannot use these pooling plots?
Forks of Chia can easily use these pooling plots by sending the 1.75XCH to the farmer target address, making them
all solo plots. If the alternate blockchain wants to do pooling as well, they need to create a special transaction
which `reserves` a singleton by providing the `launcher_id`, and launcher spend (including owner signature). Then
the code can automatically assign this singleton to the user who submitted it.

## What are the API methods a pool server needs to support Chia clients?
There are a few API methods that a pool needs to support. They are documented here:
[矿池协议API](Chia-Pool-Protocol-1.0)

## Where can I see the video Technical Q&A on Chia Pooling:
For those interested in the Chia Pools for Pool Operators video and presentation, you can find it here: 
https://youtu.be/XzSZwxowPzw
https://www.chia.net/assets/presentations/2021-06-02_Pooling_for_Pool_Operators.pdf