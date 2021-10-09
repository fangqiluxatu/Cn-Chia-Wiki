翻译自[2021年9月15日版本-61#](https://github.com/Chia-Network/chia-blockchain/wiki/Farming-on-many-machines/67d9b3b3507b9263880756145abb8cd3205788c4)

本文标题也可以是：

# 如何进行多机收割耕种，而不是单机耕种？
本篇指南可以帮助你如何在每一台收割机上只运行收割程序，而不用运行chia的全部程序，例如：全节点、钱包及耕种程序等。这样可以使得你的耕种系统架构简单、安全、易操作、可扩展性高，同时收割机所需的配置（网络带宽、cpu、磁盘空间、内存等）也会更小，在耕种过程中，在应对挑战时可以更快更有效地提供证明文件。

这个耕种系统的架构是由一台耕种机（运行全节点、耕种、钱包程序，也可以运行收割机程序），剩下的主机充当收割机只需要运行收割机程序，只用耕种机连接Chia主网即可。

为保证**耕种主机**与收割机之间通信的安全，使用了安全传输协议TLS。TLS用于**主机**验证专用数字证书签名。每台收割机都必需有经过签名的证书，以此来与**耕种主机**进行通信。

```                                          
                                      _____  收割机 1 (证书 A)
                                     /
主网其他节点  --------   耕种主机 (CA) ------  收割机 2 (证书 B)
                                     \_____  收割机 3 (证书 C)
```
## 前提条件
* 首先需要确认你所有的机子是否已经安装了Chia，且已使用命令：`chia init` 进行初始化。
* 当你想在其它收割机上开垦农田的话，使用命令： `chia plots create -f farmer_key -p pool_key`，填写对应账户的农民公钥（-f）及矿池私钥（-p）。也可以在收割机上使用命令 `chia keys add` 输入助记词来添加账户，不过这样不安全。开垦完农田以后，运行 `chia plots check` 来检查确认农田正常可用。
* 拷贝**耕种主机**的CA目录（位于：`~/.chia/mainnet/config/ssl/ca`）到每一个收割机上（任意你指定的位置）。需要注意的是，如果涉及到大版本更新，`CA`目录要重新拷贝，以确保收割机在连接时不回报错。

## 安装步骤
每台收割机遵循以下步骤进行操作：

*注释：第四步中 `chia init -c [目录]` 的目录要指定你从耕种主机所拷贝`CA`目录，而不是收割机的默认`CA`目录，例如： `chia init -c D:\ca`或者 `chia init -c /home/user/ca`。当你运行完`chia init -c [目录]`命令后，所拷贝的`CA`目录文件就可以删除了。*

1. 确认**耕种主机**的IP地址，8447端口（farmer-耕种进程的端口）是否已经开放，以便收割机连接。
2. 使用命令：`chia stop all -d`关闭chia所有的后台进程。
3. 备份每台收割机的配置文件（`~/.chia/mainnet/config/config.yaml` ）。
4. 收割机上运行命令 `chia init -c [directory]` ，收割机将会创建新的证书。
5. 打开收割机的配置文件 `~/.chia/mainnet/config/config.yaml` ，修改farmer_peer（不是 `full_node`）对应的参数值，改为耕种主机的IP地址。
例如：
``` 
harvester:
  chia_ssl_ca:
    crt: config/ssl/ca/chia_ca.crt
    key: config/ssl/ca/chia_ca.key
  farmer_peer:
    host: Main.Machine.IP
    port: 8447
```
6. 在耕种主机上运行：`chia start farmer -r`，在所有的收割机上运行： `chia start harvester -r`，然后你可以在耕种主机的日志信息中查到新的连接信息，或者在耕种主机上使用命令：`chia farm summary`查看耕种状态的相关信息。
7. 使用命令 `chia stop harvester`可以关闭收割机进程。

*注意:*

不要直接将 `config/ssl` 目录下的文件直接拷贝覆盖至其他收割机。每一台收割机都必须有一个独立的TLS证书供**耕种主机**识别，否则将会出现意外错误，因为不同收割机共享相同的**证书**会导致收割机无法正常工作。

*安全问题:*

从beta27版本开始，多机耕种需要将CA文件复制到每个收割机中，因为收割进程需要通过它来正确启动。这种方式并不完善，所以在主网上线后的后续版本中，我们将改进这种分发证书的方式。因此，连接外网的收割机需要注意安全防护，建议收割机只连接局域网。

*注释:收割机运行一段时间以后，才能在**耕种主机**的GUI客户端界面中查看到相关信息。最简单的方式查看收割机是否正常运行，可以通过“农场”分栏中“最近尝试的证明”界面查看，你将会发现“通过初筛的农田这一列”会出现类似于 0/26 1/412 3/864这样的信息每10秒会不断刷新出现。或者在本栏最下方点就`显示高级选项`，即可查看到你的收割网络连接状态。*

也可以使用命令： `chia farm summary` 来查看远程连接的收割机的信息。
如果你想调试错误信息，在**耕种主机**上通过命令 `chia configure --log-level DEBUG`将日志等级调整为“DEBUG”模式，或者手动修改 `config.yaml` 文件中的 `log-level`。 然后重新启动耕种进程：`chia start -r farmer`。查阅日志文件`~/.chia/mainnet/log/debug.log`，可以看到下列类似信息：
 ```
[time stamp] farmer farmer_server   : DEBUG   -> new_signage_point_harvester to peer [收割机的IP地址] [peer id - 64 char hexadecimal]
[time stamp] farmer farmer_server   : DEBUG   <- farming_info from peer [peer id - 64 char hexadecimal] [收割机的IP地址]
[time stamp] farmer farmer_server   : DEBUG   <- new_proof_of_space from peer [peer id - 64 char hexadecimal] [收割机的IP地址]
 ```
日志信息中的`new_signage_point_harvester` 表示耕种机向收割机发送主网的挑战信息，`farming_info`为从收割机收到的回馈信息。`new_proof_of_space`意味着你的收割机找到了一个符合挑战的证明。总的来说，日志中的`farming_info`以及`new_signage_point_harvester`是远多于`new_proof_of_space`的。

如何查看日志信息，可以查阅日志：[如何排查异常]
Here's how to find your logs: [Where to Find Things](How-to-Check-If-Everything-is-Working-(or-Not))
