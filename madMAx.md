# madMAx43v3r的快速P图程序（chia-plotter (pipelined multi-threaded)）
这里介绍github上madMAx43v3r所开源的多线程队列模式开垦程序（以下简称快P）。下载地址是：https://github.com/madMAx43v3r/chia-plotter ，以下根据原作者的README，对程序的使用及注意事项做简要说明。
## 简述
这是重新构建了一版Chia的开垦（P图）程序，类似于使用显卡来多线程渲染图片的方式，不过本程序设计的是依靠CPU处理器的多线程来进行加速开垦。

该程序通过增加线程的数量，来最大化使用缓存盘的传输带宽，从而加快开垦速度。

## 使用指南

可以加入Discord讨论组来寻求帮助：https://discord.gg/pQwkebKnPB
```bash
* 矿池公钥和农民公钥可以通过‘chia keys show’来查看
* 使用矿池协议的，需要通过-c为<contract>参数指定农田产权证（PLOT NFT）地址，使用 `chia plotnft show`来查看。
* 第一缓存目录<tmpdir>需要220GiB空间，会处理25%的缓存过程读写量。（参数写法例子：'./', '/mnt/tmp/'）
* 第二缓存目录<tmpdir>需要110GiB空间，可以使用内存，会处理会处理75%的缓存过程读写量，缓存目录(一+二)总大小需要至少256GiB。如果未设置第二目录，建议第一目录大小在300gGiB以上，
  因为要考虑到进行下一个开垦任务时，还需要将已经P好的农田拷贝到最终目录，农田需要占用101GiB的空间，你的硬盘性能决定，10分钟左右可以完成拷贝。
* 如果正在开垦多个农田（队列任务），可以按 Ctrl-C 来关闭任务，不过需要等到当前开垦的农田结束以后才会退出停止。如果你想立即关闭任务，需要在终端按两次 Ctrl-C 。

使用方式:
  chia_plot [选项...]
  
  -k, --size arg       农田格式，默认为K32，本程序只支持K32格式
  -n, --count arg      开垦数量（队列模式），默认为1，-1表示无数量限制
  -r, --threads arg    使用线程数量，默认为4线程
  -u, --buckets arg    桶数，默认为256桶数（开垦过程中，缓存目录中的文件数量）
  -v, --buckets3 arg   三、四阶段的桶数，默认与桶数一致
  -t, --tmpdir arg     第一缓存目录，至少需要220GiB
  -2, --tmpdir2 arg    第二缓存目录，至少需要110GiB（可以使用内存），不设置的话，默认为第一缓存目录位置
  -d, --finaldir arg   最终目录，默认为缓存目录
  -w, --waitforcopy    上一个农田拷贝完成后开始下一个任务
  -p, --poolkey arg    矿池公钥 (48 字节)
  -c, --contract arg   农田产权证的XCH地址，即PLOT NFT的合约地址(62 字节)
  -f, --farmerkey arg  农民公钥 (48 字节)
  -G, --tmptoggle      开垦一个队列农田时，在两个缓存目录之间相互切换作为主缓存目录，默认不开启
  -K, --rmulti2 arg    二阶段使用线程数的倍数（相对于前面设置的线程数），默认为1，也就是等于r的数量，设置为2，就是2倍r的数量。
      --help           输出帮助信息
```
如果你有足够多的CPU核心数，请确认开启了<threads>线程参数，默认线程数为4。

内存的使用情况取决于线程使用量及桶数。在默认256桶数时，每条线程最多只需要0.5GB的内存使用量。

-G 参数选项是为有多张NVME/SSD固态设备的用户设计的，两个缓存目录交替使用，开始队列任务以后，两个缓存目录轮流作为主缓存目录使用，直到队列任务完成以后，停止交替，以便2张固态盘的使用量保持平衡。

LINUX系统RAM内存盘的设置方式
```bash
sudo mount -t tmpfs -o size=110G tmpfs /mnt/ram/
```
WINDOWS系统RAM内存盘的设置方式

自行百度：windows设置内存虚拟硬盘

注意：如果使用内存作为第二缓存目录的话，系统至少要有128GiB内存。

## 未来计划
原作者有过GPU显卡挖矿的经验，2014年他是第一个开源了使用GPU显卡进行BTC挖矿的程序，相比于CPU挖BTC，速度提升了40倍以上。有兴趣的可以看看源代码[XPM GPU miner](https://github.com/madMAx43v3r/xpmclient)

同样的，为农田开垦加入OpenCL支持只是时间问题，即使用GPU显卡加速P图，从而大大减少CPU的负荷。

## 如何安装
windows用户可以使用stotiks编译的一版安装程序：https://github.com/stotiks/chia-plotter/releases
```bash
* windos使用方式参考
Windows Powershell：
./chia_plot.exe -c NFT的合约地址 -f 农民公钥 -t D:\t1\ -2 E:\t2\ -d F:\d\ -r 16 -u 256 -n 66 -G  -w
```

ubuntu20.04系统,使用方式同上类似
```bash
sudo apt install -y libsodium-dev cmake g++ git build-essential
# Checkout the source and install
git clone https://github.com/madMAx43v3r/chia-plotter.git 
cd chia-plotter

git submodule update --init
./make_devel.sh
./build/chia_plot --help
```
其他系统或者打算使用docker的，请阅读原作者github中详细说明。

