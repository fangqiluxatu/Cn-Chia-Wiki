在Windows系统中，你可以通过 Windows PowerShell 使用Chia的CLI工具，以便更加灵活方便地使用Chia软件。PowerShell 类似于SSH一样的工具，输入命令，然后回车，可以切换目录，移动文件，运行类似于Chia的软件程序等等。

# 1. 使用 PowerShell 并发开垦农田
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.2.5\resources\app.asar.unpacked\daemon\
    start-process .\chia.exe -argumentlist "plots create 开垦农田所需的参数及对应值"
    start-process .\chia.exe -argumentlist "plots create 开垦农田所需的参数及对应值"
    ....
```
如果 `start-process` 无法成果运行，尝试使用 `.\chia.exe plots create 开垦农田所需的参数及对应值` 。

或者把 `"%USERPROFILE%\AppData\Local\chia-blockchain\app-1.2.5\resources\app.asar.unpacked\daemon"` 添加到系统环境变量里去（[快速开始](Quick-Start-Guide#Windows系统下使用)中有介绍过），这样就可以直接在 Windows PowerShell 中使用 "chia" 命令，不用切换到CLI工具目录下。

在并发任务之间还需要添加一定的延时，你可以在并发的任务 `chia plots create` 命令之间加上 `sleep <seconds>;`命令。 `sleep 600` 就代表延迟十分钟开始下一个任务
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.2.5\resources\app.asar.unpacked\daemon\
    start-process .\chia.exe -argumentlist "plots create 开垦农田所需的参数及对应值"
    sleep 600;start-process .\chia.exe -argumentlist "plots create 开垦农田所需的参数及对应值"
    ....
```

### 举个例子:
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.2.5\resources\app.asar.unpacked\daemon\
    start-process ./chia.exe -argumentlist "plots create -k 32 -b 4000 -u 128 -r 4 -t d:\tempdrive1 -2 e:\tempdrive2 -d F:\plots -n 1"
```
上述命令表示开启了一个队列任务，此队列共开垦一个农田(注意参数 `-n 1` ，由 n 决定)，想要并行开垦认为，就需要重复输入命令（不要关闭上一个命令窗口）。也就是说，通过改变参数 `-n` 的值，来调整队列任务所需开垦农田的数量。一旦队列任务的第一个农田开垦完了，下一个就会马上开始。

### 提示：使用 PowerShell 开垦农田时查看日志信息
当你在GUI客户端开垦时，开垦机将会把每一个农田开垦过程中的日志信息给保存在 \mainnet\plotter 下。有许多第三方工具（比如[Chia Plot Status](https://github.com/grayfallstown/Chia-Plot-Status)）依靠这些日志信息来分析显示开垦状态。还有种方式来手动查看开垦过程信息，就是再开垦命令后面加上 "tee" 命令：
```
.\chia.exe plots create -k 32 -n 2 -u 128 -t G:\ChiaTemp -d O:\Chia8O -r 2 -n 5 | tee -filepath $env:userprofile\.chia\mainnet\plotter\plots-5-6-2021-B.txt
```
# 2. 查看更完整的日志
配置文件及日志信息在  `~\.chia\mainnet\log` 及 `~\.chia\mainnet\config` 目录下。你可以使用命令 `Get-Content ~\.chia\mainnet\log\debug.log -wait` 来查看日志信息。想要了解更多正在生成的日志信息内容，修改`config\config.yaml` 中 'log_level'参数的 'WARNING' 修改为'INFO' 。或者使用命令 `\.chia.exe configure --set-log-level INFO` 来修改。修改完成以后需要重启Chia软件才会生效。

# 3. 谨防windows自动更新
考虑到Windows可能会在开垦任务之前就完成了系统的更新检查。在开垦或者耕种过程中，系统就有可能重启以便完成更新。所以建议你进入系统高级设置中去禁用系统更新，单次可以禁止35天。

# 4. 运行多个节点时需关闭 uPnP 功能
如果你准备在局域网内运行多个Chia客户端，开启 uPnP 会导致节点之间严重混淆，你需要使用 powershell 命令来关闭所有客户端的 uPnP ，只保留一个客户端为开启状态。

# 4. Disable uPnP when running multiple nodes
If you attempt to run more than one node on your local network, having uPnP on on both will cause both nodes significant confusion. You will need to use powershell to disable uPnP on all but one.
```
    cd $env:userprofile\AppData\Local\Chia-Blockchain\app-1.2.5\resources\app.asar.unpacked\daemon\
    ./chia.exe configure --enable-upnp false
```
# 5. 保持钱包同步在线
钱包的数据可能会损坏。

# 5. Forever "Connecting to wallet"
Sometimes your wallet database can get corrupted. If you get stuck on the "Connecting to wallet" spinner for more than 60 seconds, you will probably want to exit the app, delete your wallet database with Powershell, and then start the app again.

### For version 1.0.x:
```
    del ~\.chia\mainnet\wallet\db\blockchain*
```

# 6. Periodic pause during plotting (PowerShell)
If your PowerShell plotting processes seem to pause, you should disable Quick Edit. Powershell -> Properties -> Options: Disable quick edit.

If you are still seeing this symptom it is almost always a [problem with your RAM](https://www.tomshardware.com/how-to/how-to-test-ram).

# 7. Leave free space on your drive 
If you end up having your HDD or SSD have any issues that require disk repair, you will need to have free space on the disk that is larger than the largest file. For k32 plots, you will need to leave > 101 GB. This way, if your plots ever have errors (as reported by plot check tool), you will at least be able to try to repair them with CHKDSK /r. However, CHKDSK cannot repair files that are larger than the remaining free space on a drive, and will have an error