Linux及MacOS系统的chiavdf二进制文件需要安装[时戳机](Timelords)以后才能运行。

如果你想在Linux及MacOS系统上运行一个时戳机，你必须在python的虚拟环境中从源码编译安装（安装过程中可能需要安装额外的依赖包）。
```bash
. ./activate

chmod +x ./install-timelord.sh
sh install-timelord.sh
```
如果编译失败了，那么很有可能是缺失了相关依赖包。[时戳机安装脚本(install-timelord.sh)](https://github.com/Chia-Network/chia-blockchain/blob/main/install-timelord.sh)从chiavdf发行版的python源码来调用pip部署chiavdf之前，Linux及MacOS系统需要安装相关的依赖包。

时戳机安装脚本(install-timelord.sh)通过两个环境变量参数来控制 chiavdf 部署时戳机的关键服务。一个服务是 `vdf_client` ，时戳机用它运行可验证延迟函数来进行时间证明的验证。第二个是 `vdf_bench` ，用来检测并显示CPU每秒迭代次数。

- 部署 vdf_client 需要将环境变量参数 BUILD_VDF_CLIENT 设置为 "Y"。
`export BUILD_VDF_CLIENT=Y`
- 同理，构件 vdf_bench 需要将环境变量参数 BUILD_VDF_BENCH 设置为 "Y"。 
`export BUILD_VDF_BENCH=Y`.

暂时不支持在Windows x86-64系统下部署并运行时戳机。