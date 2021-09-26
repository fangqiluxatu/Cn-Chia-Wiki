翻译自[2021年5月12日版本-16#](https://github.com/Chia-Network/chia-blockchain/wiki/How-to-Check-If-Everything-is-Working-(or-Not)/55e29a11ec3475bde71fe685c493a3212bb10e17)
***
🔨 **持续更新中** 🔨

This doc assumes you know how to use the CLI. Using the CLI is the best way to troubleshoot (and to do everything Chia too). The [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) and [CLI Commands Reference](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference) have useful info to get you familiar with the CLI.

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

# 日志
在 `config.yaml` 配置文件中可以设置日志的详细级别。

找到 `config.yaml` 的这个片段。将 `log_level` 的级别由 `WARNING` 改为 `INFO` ，这对问题的定位与排查有很大帮助。

```yaml
logging: &id001
    log_filename: log/debug.log
    log_level: INFO
    log_stdout: false
```

You can run `grep`  ([Linux](https://man7.org/linux/man-pages/man1/grep.1.html), [macOS](https://ss64.com/osx/grep.html)) or `Select-String` ([Windows](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.1)) to search through your logs for relevant information. 

# Is It Working?

If you want to quickly find errors, run this:
* Linux/macOS: `cat ~/.chia/mainnet/log/debug.log | grep -i 'error'`
* Windows: `Get-Content -Path "~\.chia\mainnet\log\debug.log" | Select-String -Pattern "error"`

## Harvester
The time it takes to do a proof challenge should be below 30 seconds. If you see higher times, something is wrong with your setup.

Here are some commands you can use to examine `debug.log` for problems.

* Linux/macOS: `tail ~/.chia/mainnet/log/debug.log | grep eligible`
* Windows:
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “eligible”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Found [^0] proof”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Farmed unfinished_block”`
	* `Get-Content -Path "~\.chia\mainnet\log\debug.log" -Wait | Select-String -Pattern "found"`
    



## Plotting

You can find the documentation for the `check` command on the [CLI Commands Reference - check](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference#check) page

* To check all your plots, run `chia plots check`. This will check all directories you have listed in your `config.yaml` to contain plots.
* Use `chia plots check -h` to see the options for this command