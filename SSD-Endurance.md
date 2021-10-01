翻译自[2021年8月17日版本-22#](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance/05e2775dbd12dadcdca7cc97153c96c959fe75a5)
***

## SSD寿命估算表


There are various approaches to picking a great plotting SSD, and a lot will depend on the physical system it is going into for form factor and interface compatibility (NVMe/PCIe, SATA, or SAS). The one thing in common will be that you need high endurance, due to the fact that it takes almost ~1.3TiB of writes (post 1.0.4 version) per k-32 plot.

Endurance with madMAx plotter is measured at

* -t & -2 on same drive: 1.43TB or 1.31TiB written per k=32 plot
* -t only (-2 in ramdisk): 396.3 GB, 369.1 GiB (0.36 TiB)

Endurance is how much data can be written to the SSD before it wears out. In Chia this is important because a plotting SSD will generally be at 100% duty cycle and writing all day. 

A mixed use or high endurance data center or enterprise SSD is the best choice for plotting. Used SSDs with plenty of endurance can be found for a good value on eBay, Craigslist, or similar.

Consumer NVMe SSDs are generally not recommended due to the lower endurance, and they often employ caching algorithms to faster media (SLC, or single level cell) for great bursty performance. They do not perform well under heavy workload sustained IO.
There are very high performance consumer NVMe SSDs that will offer great plotting performance, but the lower rated endurance in TBW will result in a faster wearout. more details about [buying SSD for Chia](https://chiadecentral.com/chia-blockchain-ssd-buying-guide/)

working model can be found [here](https://docs.google.com/spreadsheets/d/1mNUYRWeJUaijEZXupwP5k6IuATZGj1FB/edit#gid=1857251151)

You can learn more about SSD endurance from this SNIA whitepaper from JM [here](https://www.snia.org/forums/cmsi/ssd-endurance)

## 计算方式
* NAND P/E Cycles = amount of program / erase cycles NAND can do before wearing out. NAND programs (writes) in pages and erases in blocks (contains many pages)
* Wearing out - SSD no longer meeting UBER (uncorrectable bit error rate),  retention (keeping data safe while powered off), failure rate, or user capacity
* UBER = number of data errors / number of bits read
* WAF (Write Amplification Factor) = NAND writes / host writes
* TBW or PBW – amount of host writes to SSD before wearing out
* TBW = drive capacity * cycles / WAF
* DWPD (drive writes per day): amount of data you can write to device each day of the warranty (typically 5 years) without wearing out
* DWPD = TBW/365/5/drive capacity

## Linux系统查看磁盘耐久度


### NVMe
https://github.com/linux-nvme/nvme-cli

https://nvmexpress.org/open-source-nvme-management-utility-nvme-command-line-interface-nvme-cli/

使用`NVMe-CLI`工具查看NVME固态盘的寿命，
Reading endurance with NVMe-CLI - this is the gas gauge that shows total endurance used 

`sudo nvme smart-log /dev/nvme0 | grep percentage_used`
	
Reading amount of writes that the drive have actually done

`sudo nvme smart-log /dev/nvme0 | grep data_units_written`
	
Bytes written = output * 1000 * 512B

TBW = output * 1000 * 512B / (1000^4) or (1024^4)

To find out NAND writes, you will have use the vendor plugins for NVMe-CLI.

`sudo nvme <vendor name> help`

Example with an Intel SSD

`sudo nvme intel smart-log-add /dev/nvme0`


### SATA
In SATA you can use the following commands

`sudo apt install smartmontools`

`sudo smartctl -x /dev/sda | grep Logical`

`sudo smartctl -a /dev/sda`

looking for Media_Wearout_Indicator

note this does also work for NVMe for basic SMART health info

`sudo smartctl -a /dev/nvme0`

### SAS
`sg_logs /dev/sg1 --page=0x11`

look for

```Percentage used endurance indicator: 0%```


overview of SSD endurance testing from JEDEC industry standard here
https://www.jedec.org/sites/default/files/Alvin_Cox%20%5BCompatibility%20Mode%5D_0.pdf

## Adding new models
Please add your model string below if you want me to put it into my calculator and add to the list!

