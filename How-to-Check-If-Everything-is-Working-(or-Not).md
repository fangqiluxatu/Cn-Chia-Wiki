翻译自[2021年5月12日版本-16#](https://github.com/Chia-Network/chia-blockchain/wiki/How-to-Check-If-Everything-is-Working-(or-Not)/55e29a11ec3475bde71fe685c493a3212bb10e17)
***
🔨 **持续更新中** 🔨
本文档需要你会使用CLI工具。通过CLI工具来排查故障（以及使用Chia的所有功能）是最好的方式。查阅文档[快速开始](Quick-Start-Guide)以及[CLI命令行使用参考](CLI-Commands-Reference)可以熟悉使用CLI工具。

## chia相关工具的位置
下面是Chia软件的文件系统结构，在Windows、Linux、macOS上大同小异。 
```
/home/user
├─ .chia/
│   └── mainnet/
│      ├─ config/
│      │      ├─ config.yaml
│      │      └─ ssl/
│      │            └─ (and more...)
│      ├─ db/
│      ├─ log/
│      │      └─ debug.log
│      ├─ run/
│      │      └─ (and more...)
│      └─ wallet/
│             └─ (and more...)
└── /chia-blockchain
       └─ (and more...)
```

### Linux & macOS
* Chia配置文件: `~/.chia/mainnet/config/config.yaml`
* Chia日志: `~/.chia/mainnet/log/`

### Windows
* Chia配置文件:  `C:\Users\%USERNAME%\.chia\mainnet\config\config.yaml`
* Chia日志:  `C:\Users\%USERNAME%\.chia\mainnet\log\`

## 日志
在 `config.yaml` 配置文件中可以设置日志的详细级别。

找到 `config.yaml` 的这个片段。将 `log_level` 的级别由 `WARNING` 改为 `INFO` ，这对问题的定位与排查有很大帮助。

```yaml
logging: &id001
    log_filename: log/debug.log
    log_level: INFO
    log_stdout: false
```
使用 `grep` 命令（[Linux](https://man7.org/linux/man-pages/man1/grep.1.html), [macOS](https://ss64.com/osx/grep.html)），或者 在Windows的powershell中使用 `Select-String` ，来搜索日志中你想要的相关信息。

## 是否正常运行？

如果你想快速找到错误信息，运行以下命令：
* Linux/macOS: `cat ~/.chia/mainnet/log/debug.log | grep -i 'error'`
* Windows: `Get-Content -Path "~\.chia\mainnet\log\debug.log" | Select-String -Pattern "error"`


### 收割机
验证挑战证明的时间需要小于30秒。如果超过30秒，那说明你的设置是有问题的。

下面的命令示例可以帮助你检查 `debug.log` 日志文件，以找到错误信息。
* Linux/macOS: `tail ~/.chia/mainnet/log/debug.log | grep eligible`
* Windows:
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “eligible”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Found [^0] proof”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Farmed unfinished_block”`
	* `Get-Content -Path "~\.chia\mainnet\log\debug.log" -Wait | Select-String -Pattern "found"`
    



## 农田状况
你可以查看[CLI命令行使用参考](CLI-Commands-Reference#check)中的 check 章节。

* 检查所有的农田，使用命令 `chia plots check`。将会检查 `config.yaml` 配置文件中所设置农田文件目录路径下所有的农田。
* 使用命令 `chia plots check -h` 查看 check 命令的帮助信息。