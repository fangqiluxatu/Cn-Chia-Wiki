## 时戳机类型
一共有两种时戳机类型：常规时戳机（regular timelord）、时戳压缩机（Bluebox timelord）。

第一种是最基本且核心的时戳机，通过使用高频CPU来尽可能快地开展串行顺序型运算，即，在未知阶数的数组中连续做平方运算([class group of unknown order](https://github.com/Chia-Network/vdf-competition/blob/master/classgroups.pdf))。此外，每一个VDF（在源程序中称之为 vdf_client ）还是时间证明的生成装置，用于验证正确完成迭代次数计算（阶）的证明。

第二种是时戳压缩机（Bluebox timelord）。蓝盒可以是很多设备，比如旧服务器，游戏主机等，从历史区块中查找未压缩的时间证明，这样区块网络得以快速运行。常规时戳机（regular timelord）使用更快的方式来生成时间证明，但是会导致证明文件太大，以至于类似于树莓派这样的设备同步验证区块网络时需要花费大量的时间。时戳压缩机（Bluebox timelord）挑选出未压缩的时间证明并重新创建，但这次会消耗更多的时间，最终生成更紧凑的证明。然后将这些压缩过的时间证明传播给区块网络的每一个节点，以便他们使用比之前更紧凑的时间证明，来加快同步验证区块的速度。

## 最快的时戳机
目前已知有3个最快的时戳机。

There are three known fastest Timelords seen so far. The fastest known and seen on the testnet blockchain was in approximately September of 2020 where it was generating VDF iterations at about 360,000 iterations per second (or ips.) It disappeared after a few weeks. We speculate that it was an Intel cloud customer playing with pre-release Rocket Lake CPUs that have two channels of [AVX-512 IFMA](https://en.wikipedia.org/wiki/AVX-512#IFMA) support. The Timelord source code has an implementation of IFMA but it's not enabled by default as the very few CPUs that do have one channel of IFMA don't gain much speed from it - primarily because they're mostly found in power saving laptops. The second "known" fast Timelord is an [academic research project](https://ieeexplore.ieee.org/abstract/document/9301680). They and we speculate that it may be in the 400k-500k ips range once modified to run our 1024 bit width version. They used the [TSMC](https://www.tsmc.com/english) 28-nm CMOS technology for their prototype. Until Rocket Lake is generally available (which is soon as of this writing in early March) the fastest Timelord is a water cooled Intel Core i9-10900K running un-overclocked with 16 GiB or RAM. Sooner or later we will overclock it... It maintains about 200k-210k ips.

## 运行时戳机
## Running a Timelord

First of all, the network only requires one running Timelord to keep moving (liveness.) The way Timelords race is like they are on a series of 100 meter dashes. Each one takes off with the last good Proof of Space and tries to get to the total number of iterations required to complete a given Proof of Space. Better Proofs of Space require less iterations to prove. When the fastest Timelord announces the Proof of Time for this Proof of Space all of the other Timelords stop racing and are magically teleported to the starting line of the next 100 meter dash to start it all over again.

It's good to have a few Timelords out there. There can be things like routing flaps or the overzealous backhoe that takes large swaths of the internet offline. If the fastest Timelord was just about to win the current dash when its internet blinked off in a fury of construction misadventure, then the second fastest will win that dash and the next dashes - until the fastest returns. One of the key things about Proofs of Time is that given the same Proof of Space, their output and proof are always the same (though the proofs can be larger or smaller and harder or easier to validate - they all end up with the same outcome.)

The Company plans to run a few Timelords around the world - and some backups too - just to make sure that all Farmers and nodes can hear the beat that the Timelords are calling.

## 安装时戳机

### 常规时戳机（regular timelord）

Due to restrictions on how [MSVC](https://en.wikipedia.org/wiki/Microsoft_Visual_C%2B%2B) handles 128 bit numbers and how Python relies upon MSVC, it is not possible to build and run Timelords of all types on Windows - yet. We have a plan to use GCC and some tools to enable vdf_client on Windows in a way that will be compatible with a Windows install of chia-blockchain. However it's a bit convoluted to get it working right. On MacOS x86_64 and all Linux distributions, building a Timelord is as easy as running `sh install-timelord.sh` in the venv of a `git clone` style chia-blockchain install. Try `./vdf_bench square_asm 400000` once you've built Timelord to give you a sense of your optimal and unloaded ips. Each run of `vdf_bench` can be surprisingly variable and, in production, the actual ips you will obtain will usually be about 20% lower due to load of creating proofs. The default configuration for Timelords is good enough to just let you start it up. Set your log level to INFO and then grep for "Estimated IPS:" to get a sense of what actual ips your Timelord is achieving. We will shortly modify the Timelord build process to support MacOS ARM64 as well - which is a cakewalk compared to Windows...

### 时戳压缩机（Bluebox timelord）

For now, Blueboxes are also restricted to basically anything but Windows. Our plans to port to Windows will make Blueboxes available there as well though. Once you build the Timelord with `sh install-timelord.sh` in the venv, you will need to make two changes to `~/.chia/VERSION/config.yaml`. In the `timelord:` section you will want to set `sanitizer_mode:` to `True`. Then you need to proceed to the `full_node:` section and set `send_uncompact_interval:` to something greater than 0. We recommend `300` seconds there so that your Bluebox has some time to prove through a lot of the un-compacted Proofs of Time before the node drops more into its lap. The default settings may otherwise work but if the total effort is a little too much for whatever machine you are on you can also lower the `process_count:` from 3 to 2, or even 1, in the `timelord_launcher:` section. You know it is working if you see `VDF Client: Sent proof` in your logs at INFO level.

## 未来计划

Having an open source ASIC Timelord that everyone can buy inexpensively is the Company's goal. We had originally expected that we would proceed from general purpose CPUs to FPGAs and then ASICs. It turns out that squaring in class groups of unknown order at 1024 bit widths is both FPGA hard and slightly ASIC hard. It also was a pleasant surprise that Intel's AVX-512 IFMA was almost perfectly created for this application. As such we will be fostering ASIC efforts over the medium term. We are happy to lose money on an ongoing project to create and enhance an open source PCI card that would be available for say $250 for anyone who wishes to run the fastest Timelords in the world too.

## 时戳机作恶风险

One of the things that is great about the [Chia new consensus](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit) is that it makes it almost impossible for a Farmer with a maliciously faster Timelord to selfishly Farm. Due to the way new consensus works, a Farmer with a faster Timelord is basically compelled to prove time for all the farmers winning blocks around him also. Having an "evil" faster Timelord can give a benefit when attempting to 51% attack the network, so it is still important that over time we push the Timelord speeds as close to the maximum speeds of the silicon processes available. We expect to have the time and the resources to do that right and make open source hardware versions widely available.

## 专业术语
* VDF：可验证单线程延迟函数，或称之为 "时间证明"
* 时戳机启动器(Timelord launcher)：用于自行启动 "vdf client" 的小程序，以便其能执行 VDF 运算。
* VDF客户端(VDF client)：一个执行完一次 VDF 运算就会自动的C++进程。
* 时戳机(Timelord)：时戳机与各个节点进行通信，并将 VDF 任务分配给不同的 vdf clients ， vdf clients 则通过HTTP连接到时戳机。所以你可以独立运行一个时戳机来作为时戳机启动器。