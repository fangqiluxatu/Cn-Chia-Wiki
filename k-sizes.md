翻译自[2021年6月11日版本-131#](https://github.com/Chia-Network/chia-blockchain/wiki/k-sizes/c1ad453210b06d2901366852ace7ae135d284eaa)
***

## 注释
从2020年十月一日的 Beta 14版本开始，以下参考信息已经过时，仅做历史数据查看使用。（既然如此，我最后更新这篇）

农田文件的最终大小由K值决定，想了解关于开垦程序最新功能的，可以查看[GitHub论坛](https://github.com/Chia-Network/chia-blockchain/discussions)中的这篇[文章](https://github.com/Chia-Network/chia-blockchain/discussions/436)。

**K32格式农田是当前主网所支持的最小格式的农田**

***

The 1.0.4 release changes almost all of the information below for the better. This [blog post](https://chiadecentral.com/chia-plotting-improvements-in-version-1-04/) is a good overview until we can update this.

# 存储空间要求
| K值 | 缓存文件 | 最终大小 |
|-----|-----|-----|
| K=32 | 239 GiB (256.6 GB) | 101.4 GiB (108.9 GB) |
| K=33 | 512 GiB (550 GB) | 208.8 GiB (224.2 GB) |
| K=34 | 1041 GiB (1118 GB) | 429.8 GiB (461.5 GB) |
| K=35 | 2175 GiB (2335 GB) | 884.1 GiB (949.3 GB) |

When planning on how much plotting space is required, only calculate the temporary disk size requirement.

When stagger plotting, disk size requirement may change depending on which Phase the plotting is at.

## Plots larger than k=32

There was an oversight in Beta 14. For plots larger than k=32 the default buffer will not be enough to complete a plot. For k=33 a `-b 3077` is the absolute minimum needed with the rest of the defaults. A `-b 3139` will have better results by completing far more sorts in memory. More recommendations for 34-36 coming soon.

## k=37

There has been at least one k=37 plotted using the Beta 17 plotter. It required 9.9 TiB of temp space, used 130TB of total IO, and took 404.5 hours (16.9 days.)

* [Pre Beta 8](#Pre-Beta-8)  
* [Beta 8 thru Beta 13](#Beta-8-thru-Beta-13)
* [Beta 14 Data](#Beta-14-Data) 


## k=38

There has been at least one k=38 plotted on 1.0b18.dev831. 
Approximate working space used (without final file): 21547.738GiB
Final file size 7657.460 GiB
Total time = 3986304.072 seconds. (1107.3 hrs) 2/23/21



## Beta 17 Data

### i7-9700K Desktop Windows
* 处理器: Intel(R) Core(TM) i7-9700 CPU @ 3.60GHz 8 Cores
* 内存: 16GiB
* 主板: Asus ROG STRIX Z390-H Gaming
* 缓存设备: -t Sabrent Rocket 2TB SB-ROCKET-NVMe4-2TB 

| k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB) | 开垦速度（GiB/minute）  | 缓存大小 (GiB) | CPU利用率 (phase 1) | CPU Total | 内存设置            | 版本 | 注释                          |
|----|---------------|-----------------|-----------------|-------------|---------------|---------------------------|-----------|-----------------------|---------|-------------------------------|
| 34 | 397.25         | 1,406.57          | 429.88          | 0.305622898 | 1176.478        | 100%                      | 100%   |  -b 12000 -u 128 -r 8 | b17     |      
| 33 | 181.17         | 675.17          | 208.83          | 0.309301735 | 588.203        | 100%                      | 100%   |  -b 6000 -u 128 -r 8 | b17     |    

### i5-4690K Debian 10 Desktop
* 处理器: Intel i5-4690k (4 cores @ 3.9GHz)
* 内存: 8GB DDR3 1600Mhz
* 缓存设备: 1TB Samsung 870 EVO SATA SSD

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   |
| 32  |     162.1     |      422.43     |      101.3       |   0.2398   |    269.326    |      101%        |   -e -b 4000 -r 2  |   1.1.1   |
| 33  |     ---       |      896.03     |      208.8       |   0.2330   |    588.227    |      103%        |   -e -b 4000 -r 2  |   1.1.1   |

## Beta 14 Data

The following are data collected from plotting using the older and slower beta beta versions.

### i9-9900 Gaming Desktop, Z390
* 处理器: Intel(R) Core(TM) i9-9900 CPU @ 3.10GHz (turbo up to 5GHz)
* 内存: 2x 32GiB DIMM DDR4 2666MHz
* 主板: Asus TUF Z390-PRO GAMING
* 缓存设备: -t Intel SSD DC P4600 Series, 3.2TB NVMe, first run on 750GB Optane P4800X

| k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB) | 开垦速度（GiB/minute）  | 缓存大小 (GiB) | CPU利用率 (phase 1) | CPU Total | 内存设置            | 版本 | 注释                          |
|----|---------------|-----------------|-----------------|-------------|---------------|---------------------------|-----------|-----------------------|---------|-------------------------------|
| 32 | 68.52         | 248.12          | 101.32          | 0.408345698 | 286.57        | 365%                      | 171.36%   |  -b 4096 -u 128 -r 16 | b14     | single                        |
| 32 | 204.37        | 596.60          | 101.30          | 0.169795508 | 286           | 184.7                     | 128.6     |  -b 5000 -u 128 -r 4  | b14     | 8 at once, 1.36 GiB/min total |

### mini ITX NAS build
* 处理器: Intel(R) Core(TM) i5-9600K CPU @ 4.20GHz
* 内存: 2x 16GiB DIMM DDR4 Synchronous Unbuffered (Unregistered) 2667 MHz
* 缓存设备: -t Intel SSD DC P4600 Series, 3.2TB NVMe

| k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB) | 开垦速度（GiB/minute）  | 缓存大小 (GiB) | CPU利用率 (phase 1) | CPU Total | 内存设置              | 版本 | 注释                  |
|----|---------------|-----------------|-----------------|-------------|---------------|---------------------------|-----------|-------------------------|---------|-----------------------|
| 32 | 130.12   | 359.65     | 101.36          | 0.282 | 286.647       | 172.71                    | 123.08    |  -b 5000 -u 128 -r 4 -t | b14     | running 5 in parallel |
| 32 | 155.28   | 389.45     | 101.326         | 0.260 | 286.58        | 150.33                    | 116.86    |  -b 5000 -u 128 -r 4 -t | b14     |                       |
| 32 | 144.07   | 378.38     | 101.32         | 0.268 | 286.573       | 159.03                    | 119.24    |  -b 5000 -u 128 -r 4 -t | b14     |                       |

### Fedora Server 32
* AMD Ryzen 7 3800x
* 128 GB RAM
* LVM Software RAID-0, 2x NVME Gigabyte Aorus 1TB
* --

|  k  | 第一阶段 F1/Total (min) | 开垦时间 (min) | 农田大小 (GiB) | GiB/min | 缓存大小 (GiB) | CPU 第一阶段 | CPU Total |   -b    |  -r  |  -u  |   -s    | 版本 |
| --- |           ---          |        ---      |       ---       |    ---  |      ---      |     ---     |     ---   |   ---   |  --- |  --- |   ---   |   ---   |
| 32  |       4.36/89.80       |      266.63     |     101.337     |  0.3800 |    286.601    |  220.730%   |  140.130% |  26,700 |   8  |  128 | 1048576 |   1.14  |
| 32  |       4.33/84.82       |      263.58     |     101.418     |  0.3800 |    286.601    |  308.550%   |  166.400% |   5,000 |  16  |  128 |  65536  |   1.14  |

## Beta 8 thru Beta 13

These results from various machines should give a sense of how long and how much space a plot will take on different hardware. The first section is from Beta 1.8 or newer. Historical data is below and is currently still useful at the Beta 1.8 stage. Please add yours here or post the details in the #testnet channel of the [Keybase Chat](https://keybase.io/team/chia_network.public). The theory and process of plotting are described in the [Chia Proof of Space Construction](https://www.chia.net/assets/proof_of_space.pdf) document.

estimated space = 0.762 * k * 2^k bytes

| k | 农田大小 GiB | 缓存大小 GiB | 农田大小 GB | 缓存大小 GB |
| ---|---|---|---|---|
| 28 |5.33 | 32.00 | 5.73 | 34.36 |
| 29 |11.05 | 66.29 | 11.86 | 71.18 |
| 30 |22.86 | 137.16 | 24.55 | 147.27 |
| 31 |47.24 | 283.46 | 50.73 | 304.37 |
| 32 |97.54 | 585.22 | 104.73 | 628.37 |
| 33 |201.17 | 1207.01 | 216.00 | 1296.01 |
| 34 |414.53 | 2487.17 | 445.10 | 2670.58 |
| 35 |853.44 |5120.64 | 916.37 | 5498.25 |

### AWS EC2 r5d.8xlarge - Pre release Beta 12
* 处理器: 32 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* 内存: 256GiB
* 缓存设备: tempdir: Amazon.com, Inc. NVMe SSD (Ubuntu 20.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 |  版本  | 注释 |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |    ---    | ---  |
| 30  |      84.9     |       168.1     |     23.8         |   0.142    |    67.3       |     98.11 %      |   192 GiB  | pre 1.12  |      |
| 31  |     178.1     |       359.0     |     49.2         |   0.140    |   135.0       |     95.52 %      |   192 GiB  | pre 1.12  |      |
| 32  |     399.5     |       796.0     |    101.2         |   0.127    |   391.0       |     92.50 %      |   192 GiB  | pre 1.12  |      |

### Dell Inspiron Desktop 
* Intel Hexacore i5 8400
* 8 GiB DDR4
* Western Digital WD7500AYYS 750GB 7200 RPM
* ~ write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 32  |    1526.128   |      4253.52    |    101.3         |   0.024      |   529         |     100  %.      |   default  |   1.9   | Using Native Window Plotter, not WSL2 |

### Dell Inspiron Desktop 
* Intel Hexacore i5 8400
* 8 GiB DDR4
* Toshiba dt01aca100 7200 RPM - 1TB Hard Drive
* ~ write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 32  |    1001.865   |      2572.58    |    101.3         |   0.039    |   529         |     100  %.      |   default  |   1.9   | Using Native Window Plotter, not WSL2 |

### MacBook Pro (13-inch, 2019, Four Thunderbolt 3 ports)
* 2.8 GHz Quad-Core Intel Core i7
* 16 GiB 2133 MHz LPDDR3
* APPLE SSD AP1024M
* ~ 2900 MB/s write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |     ---          |    ---     |    ---        |      ---         |     ---    |   ---   | ---  |
| 30  |    149.91     |      327.83     |    23.79         |   0.0726   |   125.93      |     83.74%.      |   default  |   1.8   |      |

### Ubuntu 19.04
* Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz (10 cores)
* 64 GiB PC4-21300 DDR4-2666V-R REGISTERED ECC
* Western Digital 4 TB [WD10EZEX](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-blue-hdd/data-sheet-wd-blue-pc-hard-drives-2879-771436.pdf)
* ~ - MB/s write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 29  |     60.36     |       128.9     |     11.52        |   0.0893   |     61.00     |     81.77%.      |   default  |   1.8   |      |
| 30  |    131.25     |       302.8     |     23.82        |   0.0787   |    126.02     |     79.86%.      |   default  |   1.8   |      |
| 31  |    333.50     |       724.3     |     49.17        |   0.0679   |    262.04     |     70.66%.      |   default  |   1.8   |      |
| 32  |    674.69     |      1577.7     |     101.3        |   0.0642   |    523.93     |     63.74%.      |   default  |   1.8   |      |
| 33  |    1738.2     |     4285.56     |     208.8        |   0.0487   |   1095.91     |     52.53%       |   default  |   1.8   |      |

### Ubuntu 20.04.1 LTS
* Intel(R) Xeon(R) E3-1270v6 CPU @ 3.80GHz (4 cores)
* Samsung(R) M391A2K43BB1-CRC 32GB (2x16GB) PC4-19200 DDR4 2400Mhz ECC
* Intel(R) P4510 NVMe SSD 1TB (2x1TB in RAID 1)
* fio 1 MiB randwrite: ~800 MiB/s; fio 4 KiB randwrite: ~360 MiB/s

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 32  |    352.47     |     854.27     |     101.36        |   0.1187   |   641.66     |     83.85%       |   20000  |   1.11   |      used cli -f -p |
| 32  |    343.18     |     838.89     |     101.38        |   0.1209   |   621.71     |     84.43%       |   20000  |   1.10   |      used cli -f -p |
| 32  |    378.90     |      869.49     |     101.37        |   0.1166   |    524.03     |     85.24%      |   20000  |   1.9   |      used cli -f -p |

### Razer Blade Stealth 2018
* Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2001 Mhz, 4 Core(s), 8 Logical 处理器(s)
* 16 GiB RAM
* Samsung SSD PM981 MZVLB512HAJQ
* Crystal - CDM 5 Write Seq: 1468 MB/s

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---    |   ---   | ---  |
| 31  |     278.5     |      613.09     |     49.134       |    0.0801  |    261.944    |     100.0%       |   default  |   1.8   |      |

### Intel Pentium CPU G4500 in WSL2 on ext4
* Intel Pentium CPU G4500 @ 3.50Ghz - no AVX
* 8 GB RAM
* Samsung SSD 860 EVO 250GB
* ? MB/s

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 30  |     244.8     |      540.59     |      23.82       |   0.0441   |    126.014    |     83.62%       |   default  |   1.8   |      |

### Intel Pentium CPU G4500 in Windows on NTFS
* Intel Pentium CPU G4500 @ 3.50Ghz - no AVX
* 8 GiB RAM
* Samsung SSD 860 EVO 250GB
* ? MB/s

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置 | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |    ---     |     ---       |      ---         |     ---    |   ---   | ---  |
| 30  |     153.6     |      359.62     |      23.83       |   0.0663   |    126.038    |      100%        |   default  |   1.8   |      |

### AWS EC2 r5dn.12xlarge
* 处理器: 48 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* 内存: 374GiB
* 缓存设备: tempdir: ramdisk of 310GiB (Ubuntu 18.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置  | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 30  |     81.50     |      165.49     |     23.80        |   0.14382  |    125.95     |     99.99%.      |     64GiB   |   1.8   |      |
| 31  |    185.59     |      361.67     |     49.17        |   0.13595  |    262.04     |     99.99%.      |     64GiB   |   1.8   |      |

### AWS EC2 i3en.large
* 处理器: 2 Intel(R) Xeon(R) Scalable (Skylake) processors with new Intel Advanced Vector Extension (AVX-512) instruction set @ 3.1 GHz
* 内存: 16 GiB
* 缓存设备: 1 x 1,250 NVMe SSD

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置  | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 32  |    576.30     |     1360.41     |     101.338      |    0.0745  |    523.946    |     79.19%.      |      6GiB   |   1.8   |      |

### AWS EC2 i3.xlarge
* 处理器: 4 Intel(R) Xeon(R) E5-2686 v4 (Broadwell) CPU @ 2.30 GHZ
* 内存: 30.5 GiB
* 缓存设备: 1 x 950 NVMe SSD

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置  | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 32  |    616.78     |     1340.32     |     101.338      |    0.0742  |    523.997    |     90.96%.      |    20GiB    |   1.8   |      |

### Raspberry Pi 4
* 处理器: Broadcom BCM2711, Quad core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz
* 内存: 4 GB LPDDR4-3200 SDRAM
* 缓存设备: Samsung 860 EVO V-NAND SSD 1 TB (SATA to USB 3.0 connection)

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置  | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 30  |    448.87     |      919.50     |      23.8151     |   0.0259   |    125.996    |     92.92%.      |     2GiB    |   1.8   |      |
| 31  |    940.39     |     1958.23     |      49.1596     |   0.0251   |    262.008    |     92.45%.      |     2GiB    |   1.8   |      |
| 32  |   1857.45     |     3954.88     |     101.3680     |   0.0256   |    524.017    |     93.68%.      |     2GiB    |   1.8   |      |
| 32  |   1869.62     |     4311.85     |     101.2990     |   0.0235   |    523.855    |     85.38%.      |   2.5GiB    |   1.9   |      |

* 缓存设备: Seagate Backup Plus 10 TB (USB 3.0 connection)

|  k  | 第一阶段 (min) | 开垦时间 (min) | 农田大小 (GiB)  | 开垦速度（GiB/minute） | 缓存大小 (GiB) | CPU利用率  | 内存设置  | 版本 | 注释 |
| --- |      ---      |        ---      |      ---         |     ---    |     ---       |      ---         |     ---     |   ---   | ---  |
| 32  |   1649.80     |     3677.30     |     101.3180     |   0.0276   |    532.690    |     94.09%.      |   1.8GiB    |   1.10  |      |


## Pre Beta 8

incorrectly listed as GB - most should be GiB

### MacBook Pro (13-inch, 2019, Four Thunderbolt 3 ports)
* 2.8 GHz Quad-Core Intel Core i7
* 16 GB 2133 MHz LPDDR3
* APPLE SSD AP1024M
* ~ 2900 MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 26  |  11.6 | 1.296  | 7.06  | 91.86%  |   |
| 27  |  30.1 | 2.697  | 14.51  | 83.04%  |   |
| 28  |  82.1 | 5.58  | 30.27  | 69.4%  |  some use |
| 29  |  179 | 11.5  | 60.9  | 69.68%  |   |
| 30  | 406.5  | 23.8  | 128  | 71.67%  | in use  |
| 30  | 377  | 23.8  | 128  | 73.18%  | machine idle  |
| 31  | 856.7  | 49.16  | 262.0  | 92.29%  | idle - last 80%  |

### iMac (Retina 4K, 21.5-inch, 2017)
* 3.4 GHz Quad-Core Intel Core i5
* 16 GB 2400 MHz DDR4
* 1TB Fusion Drive (Actually attached USB 3.0 4Tb RAID 1)
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  |
|---|---|---|---|---|
| 25  | 4.1 | 0.62 | 3.4 | 97.85 |
| 26  |  27 | 1.3  | -  | -  |
| 27  | 81  | 2.7  | -  | -  |
| 28  | 260  | 5.6  | 30.3  | -  |
| 29  | 787  | 11.5  | 61  | -  |

### Ubuntu 19.04
* Intel(R) Xeon(R) W-2155 CPU @ 3.30GHz (10 cores)
* 64 GB PC4-21300 DDR4-2666V-R REGISTERED ECC
* Western Digital 4 TB [WD10EZEX](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-blue-hdd/data-sheet-wd-blue-pc-hard-drives-2879-771436.pdf)
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 28  |  55.3 | 5.58  | 30.3  | 99.56%  | |
| 28  |  43 | 5.56  | 30.3  | 99.07%  | beta4 |
| 30  | 308  | 23.8  | 128  | 83.78%  | |
| 30  | 273  | 23.8  | 128  | 75.44%  | beta4 |
| 31  | 805  | 49.1  | 262  | 67.36%  | |
| 31  | 621  | 49.1  | 262  | 69.68%  | beta4 |
| 32  | 1,866  | 101.4  | 544  | 60.57%  | |
| 33  | 4663.5  | 208.8  | 1095.97  | 52.2%  | |
| 34  | 10,865.6  | 429.8  | 2,287  | 50.13%  | |
| 35  | 25,703  | 884.1  | 4672.2  | 39.45%  | |

### AWS EC2 r5.12xlarge
* 处理器: 48 Intel(R) Xeon(R) Platinum 8175M CPU @ 2.50GHz
* 内存: 374GB
* 缓存设备: 内存 tempfs (Ubuntu 19.10)
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 31  |  425 | 49.15  | 262  | 99.99%  | alpha 1.3 |    |

### AWS EC2 r5dn.12xlarge
* 处理器: 48 Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz
* 内存: 374GB
* 缓存设备: tempdir: ramdisk of 264GB (Ubuntu 18.04) finaldir: Amazon.com, Inc. NVMe SSD
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  |  GB/minute |  CPU利用率 | 注释         |
| ---|  ---                 |       ---       |    ---        |     ---    |  ---             |    ---       |
| 26 |    9                 |     1.3         |      7.1      |   0.144    |    99.99%        | pre-beta-1.5 |
| 27 |    20                |     2.7         |      14.5     |   0.135    |    99.99%        | pre-beta-1.5 |
| 28 |    37.5              |     5.57        |      30.2     |   0.149    |    99.99%        | pre-beta-1.5 |
| 30 |    200               |     23.8        |      128.1    |   0.119    |    99.99%        | pre-beta-1.5 |
| 31 |    412               |     49.14       |      262*     |   0.119    |    99.99%        | 实际缓存大小 267GB, chiapos 0.12.13 |
| 31 |    408.4             |     49.14       |      262*     |   0.120    |    99.99%        | 实际缓存大小 267GB, [speedy branch](https://github.com/Chia-Network/chiapos/commit/9200540e7e0a0e7cdbf307ef84a4f8e19cf33403) |

### MacBook Pro (15-inch, 2017)
* 2,8 GHz Quad-Core Intel Core i7
* 16 GB 2133 MHz LPDDR3
* 251 GB 缓存设备: APPLE SSD SM0256L
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 30  |  559 | 23.79  | 217.94  | 61.65%  | alpha 1.0 |
| 30  | 614  | 23.8  | 127.992  | 61.13%  | alpha 1.1 |

### MacBook Pro (15-inch, 2017)
* 处理器: 2,8 GHz Quad-Core Intel Core i7
* 内存: 16 GB 2133 MHz LPDDR3
* 缓存设备: 1 TB (TOSHIBA HD)
* ~ - MB/s write

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 30  |  1512 | 23.838  | 128.05  | 27.59%  | alpha 1.0 |    |

### Fedora Server 31
* 处理器: AMD Ryzen 5 3600 6-Core Processor            
* 内存: G-Skill 64 GB DDR4 3000MHz            
* 缓存设备: GIGABYTE AORUS NVMe Gen4 SSD 1 TB               

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 31  |  571.5 | 49.15  | 261.99  | 71.17%  | beta 3.0 3 concurrent |    |
| 32  |  957 | 101.36  | 544.0  | 97.49%  | beta 3.0 |    |

### Fedora Server 31
* 处理器: AMD Ryzen Threadripper 2950X           
* 内存: Corsair 128 GB DDR4 2133MHz           
* 缓存设备: LVM-RAID0 (3 x Inland Performance Gen4 SSD 1 TB)                 

| k  |  开垦时间 (minutes) | 农田大小 (GB)  | 缓存大小 (GB)  | CPU利用率  | 注释 |
|---|---|---|---|---|---|
| 33  |  3093 | 208.85  | 1096.03  | 79.63%  | beta 3.0 2 concurrent |    |