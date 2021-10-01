翻译自[2021年5月17日版本-16#](https://github.com/Chia-Network/chia-blockchain/wiki/Reference-Plotting-Hardware/2b26fd0fa4cd8554726a60e08e6629bc84c44757)


These are machines that work well to quickly create plots in parallel. These serve as starting points to think about acquiring or modifying existing systems to be good plotting machines on a TBs of plots created per day basis. You don't need this level of hardware to then farm plots that have been generated however. For that a [Raspberry Pi](https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi) is enough.

* [Chia budget plotting desktop guide](https://docs.google.com/document/d/18a5_MO88hv_DL_644OGFoCiKnpR9udl-PAWKlyIqqQU/edit?usp=sharing)

* [Chia plotting basics](https://www.chia.net/2021/02/22/plotting-basics.html)

Plotting can be done on consumer systems (laptops), but is much faster done on high-end desktops, workstations, and servers. Plotting takes scaling for CPU cores to improve the parallelism (this is -r number of threads, or number of parallel processes), scaling in DRAM per process, and fast SSDs or many small 10k hard drives for temporary storage space.

The amount of DRAM and temp space changed in [1.0.4](https://chiadecentral.com/chia-plotting-improvements-in-version-1-04/). The new table below reflects the values in the GUI. You need at least one CPU core (but you can use r to increase thread count speeding up phase 1), and the DRAM and temp space below per plot. The easy math for a system is to take the number of cores and use that as the target for number of processes to run in parallel, the multiply the temp space and DRAM requirements by that to find the minimum amount.

| K-value | DRAM (MiB) | Temp Space (GiB) | Temp Space (GB) |
| ------- | ---------- | ---------------- | --------------- |
| 32      | 3390       | 239              | 256.6           |
| 33      | 7400       | 521              | 550             |
| 34      | 14800      | 1041             | 1118            |
| 35      | 29600      | 2175             | 2335            |


The goal of a plotting machine is to create the highest TiB per day of plots, with the lowest system cost. There are many unique combinations of consumer, data center, and enterprise hardware at many different price points that are adequate for plotting. For SSDs, please see the https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance page

This is a spreadsheet of the community systems to compare against:
https://docs.google.com/spreadsheets/d/14Iw5drdvNJuKTSh6CQpTwnMM5855MQ46/