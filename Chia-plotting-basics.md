# 农田开垦入门
## 基本介绍
想成为Chia网络一名合格的农民需要了解两件事情，即开垦农田以及耕种农田。本章会着重讲解农田的开垦过程，开垦农田所用到的计算机配置和储存空间需求与耕种农田的设备是不同的，关于耕种农田所需要的设备配置要求，你可以参考文档[耕种设备](Reference-Farming-Hardware)


**建议刚开始的时候使用你现有的设备进行农田的开垦测试**， 需要注意的是，使用设备自带或者消费级SSD固态盘作为开垦农田过程中的缓存盘时，要关注一下不同的SSD固态盘，它的寿命及可开垦的农田数量是不一样的，详情可以参考文档[SSD寿命概况](SSD-Endurance)。

暂时不推荐开垦大于K32格式的农田，这么做的，要么是为了测试娱乐，要么就是为了尽量填满机械硬盘（储存设备）的空间。一个K32的农田大概占用 101.3 GiB的空间，但在开垦它的过程中，会产生 239 GiB 缓存文件，也就说，开垦一个K32农田任务只需要 239 GiB的空间。这里要注意一下，GiB是文件总字节数除了3次1024而得，GB是文件总字节数除了3次1000而得，即缓存空间 239 GiB = 256.6 GB ，K32农田大小 101.3 GiB = 108.8 GB。熟悉开垦农田的人可以在4小时内开垦完一块K32农田，但更多人开垦一块K32农田需要花5小时甚至更久。

开垦一块农田的过程，需要占用内存，CPU资源以及反复读写硬盘，在开垦的四个阶段中，不同阶段对硬件资源使用的情况也有所不同。每一个开垦农田的人都想知道一个最佳的硬件设备组合，然而每台设备的参数都各尽不同，所以需要自己去尝试并不断调整。你也可以进入社区讨论组， 与社区成员互相讨论来配置适合自己使用的最佳硬件配置。

## 入门开垦
第一阶段将会生成所有的空间证明，它们是由7张加密哈希表组成并保存在缓存目录中。第二阶段就开始对这些哈希表从后向前进行反向传播运算，第三阶段对这些哈希值进行排序及压缩运算，并开始构建形成农田文件。最后在第四阶段完成农田并将其传输到指定的最终目录。

开垦过程中的最大瓶颈就是缓存目录磁盘的持续写入速度。如果你想更快更稳定地开垦农田，推荐使用企业级SSD而不是消费级SSD设备。通常来讲，读写速度的排序：NVME/SSD > SAS >SATA。TBW，或者称之为太字节写入量，用来描述NVME/SSD固态硬盘的寿命（关于硬件缩写不懂的请自行百度查询详细解释）。禁用位域（高级选项中可选）时开垦一个K32农田会消耗1.8 TiB的写入量，启用位域则消耗 1.6 TiB，后面会详细介绍关于位域的内容。

然而以最快的速度只开垦一块农田并合适的方案。大家使用的电脑都是多核处理器，使用并行任务进行开垦，速度会快很多。所以，你需要关注一下你的开垦设备并行开垦农田的话，每天能够完成多少TB的农田，以此来规划你的开垦任务。有些人会使用企业级SSD，或者SAS机械硬盘，使用RAID 0对缓存所用的 SSD 或者 HDD 进行组合连接，可以提升传输速度以及缓存容量，假设组成了一个2TB的缓存分区，那么此RAID硬盘就可以同时容纳5个K32农田的缓存，也就是说可以同时开垦5块K32农田。

综上所述，针对我个人计算机的配置，2017年的MAC主机，12TB桌面式西数硬盘USB3.0连接，同时作为缓存目录及最终目录，这样我开垦一块K32农田需要10个小时。

## 关于位域（bit-field）
按照目前已有的经验总结，是否使用位域会对开垦农田的速度产生一定影响。首先需要解释一下开垦时开启位域与禁止位域有什么区别。最初，开垦程序
There are some good rules of thumb for now. These can change as we will be returning to making some plotting speed improvements after launch. First we need to explain bit-field versus no bit-field plotting. Originally, the plotter did not use bit-field back sorting. The bit-field back sort is theoretically faster than not using the bit-field and we already know that it saves 12% of total writes but requires more RAM. We have a hunch we can speed bit-field up 10% and make it work on more processors but that’s not in there yet. What we do know is that, as long as you’re ok with the 12% more total writes, no bit-field will work faster when SSD or fast SAS is your temporary directory. If your temporary directory is on a regular HDD, like mine is, bit-field is 20% faster than no bit-field. Older CPUs may not see the speed increase as much as noted above.

Returning to the rules, here are a few. Never touch the stripe size of 65536. No one has found a speed up over that value and we are likely removing it from the options list. (Update: as of 3/11/21 stripe size has been removed as an option.) You almost never want to use any bucket values other than 128. Less buckets requires more RAM for each plotting process. 64 buckets requires twice the RAM.

As far as number of threads are concerned you are generally going to want 2 to 4. More than 4 seems to have diminishing returns and 2 threads is a lot better than 1. More threads also require a bit more memory to successfully complete a plot. The threading is only used in phase 1 currently.

As of Chia 1.0.4, RAM requirements are almost identical between bit-field and no bit-field. This is a chart of the various RAM choices assuming a k32 with 128 buckets and 2 to 4 threads:

| 内存需求 MiB: |	 最小值 | 默认 	| 最大值 |
| ---- |	 ---- |	 ---- |	 ---- |
|开启位域(Bitfield)|	900|	2640|	3400|
|禁用位域(No Bitfield)	|900	|2640	|3400|


Below minimum your plot will fail. Medium is enough RAM that you’ll get most speed improvements, but not all. This is useful when you’re trying to get more plotting processes parallel and have limited RAM. Using anything over the maximum is wasting RAM as you will not plot any faster. We are pretty certain of the minimums and maximums but there is community debate about the medium values. We’ll update this chart accordingly as we have better data.

## 精通开垦
Most people start plotting from the GUI. You can successfully complete a couple of plots in parallel from there to get the hang of things. As people choose to get more serious they migrate to the command line. It is worth noting that Windows suffers 5-10% slower plot times versus MacOS or Linux for now.

Once you get some experience you will probably want to know how to create more and more plots in parallel. Luckily we have a replay on YouTube of our cocktails with plotting experts. They had much to share about their various approaches. Some used servers and datacenter SSD, some bought used servers and SAS drives for temporary directories, some expand their consumer/gaming machines, and some focused on lots of smaller used machines. Many of them have compiled a spreadsheet of reference plotting hardware with plot speeds to help get you thinking about any hardware you might want to change or acquire and see how your plotting results measure up.

As you start parallel plotting you need to be careful to not over allocate memory when you are plotting. If you cause your operating system to swap, you are not going to be happy with your outcome. You don’t have to be as careful with thread count.

It is also a very common plotting strategy to plot on say your gaming machine and then move your plots to a Raspberry Pi 4 with a lot of USB ports. All you need is your same 24 word mnemonic on both machines. Alternatively you can just run a remote harvester on your Pi and have it connect to your gaming machine where you are running node and farmer and only have your private keys on one machine.

## 深入探索
建议开垦或者耕种农田的时候可以阅读一下WIKI的[FAQ答疑文档](FAQ)，其中的回答应该可以回答你90%的疑问。

在你阅读FAQ的过程中，你可以在官方讨论组相应的频道中获得社区的帮助与支持。

Keybase频道话题[_chia_network.public_ Keybase](https://keybase.io/team/chia_network.public):**

* \#公告（announcements）
* \#初学者（beginner）
* \#耕种设备（farming hardware）
* \#综合问题（general）
* \#中文区(lang_zh)
* \#农田开垦（plotting）
* \#杂谈（random） 
* \#测试网（support ）


## 感谢
@pyl, @kiwihaitch, @psydafke, and @storage_jm 对本篇文档提供的帮助。后续如有更新及勘误，我将在这里进行做补充更正。

## 更新内容
自1.0.4版本起，内存所使用的最小、默认、最大值已更新。