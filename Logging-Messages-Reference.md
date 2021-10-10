翻译自[2021年5月6日版本-2#](https://github.com/Chia-Network/chia-blockchain/wiki/Logging-Messages-Reference/59d6c3324ccfb396d1c3498ec50084700c4a164f)
***

# 日志文件（debug.log）
一个Chia区块网络的节点由耕种、收割机、钱包和节点这几个不同的进程所组成。每个进程都将会产生日志信息，并被记录在`debug.log`日志文件内。

## 日志文件路径
|系统|日志文件路径|
|---|---|
|Linux|`~/.chia/mainnet/log/debug.log`|
|Windows|`%systemdrive% %homepath% \.chia\mainnet\debug.log` (C:\Users\<username>\.chia\mainnet\config)|
|MacOS|`/Users/<username>/.chia/mainnet/log/debug.log`|

## 日志文件管理
`debug.log`日志文件默认最大为20MB，当日志文件记录到20MB时会自动重命名为`debug.log.1`,并开始新的记录，记满后依次自动命名为`debug.log.x`，chia系统最多可以保留7个历史日志文件，在此期间，它会自动循环更新覆盖成最新的7个历史日志文件。这也就是说，最多可以保存160MB的日志信息。

## 日志级别详情
Chia默认只记录`WARN`及`ERROR`等级的日志。想要查阅节点的全部日志信息的话，需要将`config.yaml`日志文件中的`log_level`参数值设置为`INFO`。

## 修改日志级别的输出
打开`config.yaml`日志文件，在`farmer`片段中，如下：
```
farmer:
  logging: &id001
    log_filename: log/debug.log
    log_level: WARN
    log_stdout: false
```
修改`log_level`的参数值为INFO，保存文件并重新启动节点。

## 节点组件的架构:
|组件服务|功能|
|---|---|
|农民(farmer_server) | 签名交易确认以及打包区块数据|<br>
|收割机(harvester_server)|收集并向耕种（农民）服务提供农田信息|<br> 
|时戳机(timelord_server)|管理节点的可验证单线程延迟函数（VDF）功能|<br>
|钱包(wallet_server)|钱包管理功能|<br>
|全节点(full_node_server)|与主网的区块网络进行同步|<br>

## 日志消息的格式:
|字段|内容|
|---|---|
|时间(Date/time) | 本机当地时间，ISO国际标准时间格式|<br>
|组件服务(Node Component) | 参照上表所列的组件服务|<br>
|日志级别(Log Level) | ERROR, WARN, INFO|<br>
|方向箭头(Directional Arrow) |<br>
|信息(Message) |参照下面的介绍<br>

## 记录节点运行状态:
1. “x plots were eligible for farming” – 此日志信息来自于收割机，表示本机耕种节点对于区块网络挑战的响应情况。“x”为通过初步筛选的农田数量。查看FAQ中关于[农田筛选器](FAQ#what-is-the-plot-filter-and-why-didnt-my-plot-pass-it)的解释。
> * 此处显示区块的高度, “Found y proofs.” - “y”值为多少块农田找到了符合当前区块的挑战证明，这个数值一般为0。一般来讲，如果农田找到了符合挑战的证明，那基本就能赢得区块奖励，但实际上这并非绝对正确，具体查看此[问答解释](FAQ#is-it-possible-to-have-a-proof-but-not-get-a-reward)。x
> * “Time: x.xxx s.” - which shows how long the node took to respond to the challenge.  The network requires a response in less than 28 seconds from the time the challenge was originated, so a recommended response time is less than 5 seconds. If this value is greater than 3 seconds a warning will be displayed in the GUI.
> * Finally “Total x plots” shows the number of plots recognized by the node.  If this doesn't look right [check your plots are valid.](FAQ#how-do-i-know-if-my-plots-are-ok)
1. “Updated Wallet peak to height x, weight y” message from the wallet_blockchain component.  Value x is the current height of the blockchain, and should match the Height shown in the `chia show -s` command.  This indicates that the node wallet is fully synced with the network.  If that is not the case [check here for a common solution.](https://github.com/Chia-Network/chia-blockchain/wiki/FAQ#why-is-my-wallet-not-synced-why-can-i-not-connect-to-wallet-from-the-gui)
2. <other key messages>

## 一些正常的日志信息参考:

|组件服务	|消息	|方向	|目标	|相关的组件服务|注释|
|---|---|---|---|---|---|
mempool_manager	|add_spendbundle took x seconds|||||
mempool_manager	|It took x to pre validate transaction|||||
full_node|Added unfinished_block x, not farmed by us|||||
full_node|Already compactified block:|||||
full_node|Duplicate compact proof.  Height: x||||
full_node|Finished signage point x/64: ||||
full_node|Scanning the blockchain for uncompact blocks.||||
full_node|Updated peak to height x|||||
full_node_server|new_compact_vdf|to/from|peer|||
full_node_server|new_peak|to/from|peer|||
full_node_server|new_peak_timelord|to|localhost|from timelord_server||
full_node_server|new_peak_wallet|to|localhost|from wallet_server||
full_node_server|new_signage_point|to|localhost|from farmer_server||
full_node_server|new_signage_point_or_end_of_sub_slot|to/from|peer|||
full_node_server|new_transaction|to/from|peer|||
full_node_server|new_unfinished_block|to/from|peer|||
full_node_server|new_unfinished_block_timelord|to/from|localhost|||
full_node_server|request_block|to/from|peer|||
full_node_server|request_block_header|from|localhost|to wallet_server||
full_node_server|request_compact_vdf|to/from|peer|||
full_node_server|request_compact_proof_of_time|to|localhost|from timelord_server||
full_node_server|request_signage_point_or_end_of_sub_slot|to/from|peer|||
full_node_server|request_transaction|to/from|peer|||
full_node_server|request_unfinished_block|to/from|peer|||
full_node_server|respond_block|to/from|peer|||
full_node_server|respond_compact_vdf|to/from|peer|||
full_node_server|respond_signage_point|to/from|peer|||
full_node_server|respond_transaction|to/from|peer|||
full_node_server|respond_unfinished_block|to/from|peer|||
wallet_server|request_block_header|to|localhost|from full_node||
wallet_server|respond_block_header|from|localhost|to full_node||
wallet_server|new_peak_wallet|from|localhost|to full_node||
wallet_blockchain|Updated Wallet peak to height x, weight y||||
timelord_server|new_peak_timelord|from|localhost|to full_node||
timelord_server|new_unfinished_block_timelord|from|localhost|to full_node||
timelord_launcher|VDF client x: VDF Client: Discriminant =||||
VDF Client|Sending Proof, Sent Proof, Stopped everything!||||
harvester_server|farming_info|to/from|localhost|||
harvester_server|new_signage_point_harvester|from|localhost|to farmer_server||
harvester|x plots were eligible for farming||||
plot_tools|Loaded a total of x plots of size y in z seconds||||
plot_tools|Searching directories||||
farmer_server|new_signage_point|from|localhost|to full_node||
farmer_server|farming_info |from|localhost|to full_node||
farmer_server|new_signage_point_harvester|to|localhost|from harvester||

| 源程序 | 日志级别 | 日志信息 | 说明 |
|  ---      | --- | --- | --- |
|daemon asyncio  | ERROR |   Task exception was never retrieved future: `<Task finished coro=<WebSocketServer.statechanged() done, defined at src\daemon\server.py:316> exception=ValueError('list.remove(x): x not in list')>`
|full_node asyncio                 | ERROR |   SSL error in data received protocol: `<asyncio.sslproto.SSLProtocol object at 0x7f762544a8>`
|full_node full_node_server        | ERROR |   Exception: Failed to fetch block `N` from {'host': `IP ADDRESS`, 'port': 8444}, timed out, {'host': `IP ADDRESS`, 'port': 8444}. | Peer disconnected, other peer connections will take over|
|full_node full_node_server        | ERROR |Exception:  `<class 'concurrent.futures._base.CancelledError'>`, closing connection None.|Peer disconnected
|full_node full_node_server            | WARNING | [Errno 32] Broken pipe `IP Address`|Peer disconnected|
|full_node full_node_server            | WARNING | Cannot write to closing transport `IP Address`|Peer disconnected|
|harvester src.plotting.plot_tools    | WARNING | Not farming plot `plotfilename`. Size is `filesize` GiB, but expected at least: 99.06 GiB. We assume the file is being copied.| Periodic scan for new plots have discovered partial file - OK if you are in the middle of copying a file|
|harvester src.plotting.plot_tools | WARNING | Directory: `Dir1` does not exist. | One of your monitored plot folders is no longer accessible - eg external drive offline - if permanent remove from GUI or `chia plots remove -d <Dir1>`|
|harvester src.plotting.plot_tools | WARNING | Have multiple copies of the plot `plotfilename`, not adding it.||
|harvester src.plotting.plot_tools | INFO |    Not checking subdirectory `Dir1/directory`, subdirectories not added by default ||
|full_node full_node_server            | INFO    | Connection closed: `IP Address`, node id: `hex`|Peer disconnected|
|full_node src.full_node.full_node     | INFO    | ⏲️  Finished signage point `N`/64: `hex`
|full_node src.full_node.full_node     | INFO    | Added unfinished_block `hex`, not farmed
|harvester src.plotting.plot_tools | INFO |    Searching directories [`Dir1`,`Dir2`] ||
|harvester src.plotting.plot_tools | INFO |    Loaded a total of `N` plots of size `size` TiB, in `time` seconds||
|harvester src.harvester.harvester | INFO |`X` plots were eligible for farming `hex`... Found `Y` proofs. Time: `Time` s. Total `Z` plots|This is a vital message and should be seen at regular intervals. Note that `Time` is ideally < 1s. If drive is in sleep mode, may show ~10 seconds, and should be prevented |
| wallet src.wallet.wallet_blockchain | INFO | 💰 Updated wallet peak to height `HEIGHT`, weight `WEIGHT`, ||









