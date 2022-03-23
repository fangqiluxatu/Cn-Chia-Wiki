# Farming FAQ:

### _Do I need a GPU to make plots/farm?_
No, OpenCL is an optional implementation to take some load off of the CPU when running a full node. OpenCL is used for verifying the Verifiable Delay Function (VDF) that is associated with each block. The full node can be run entirely without any GPU acceleration, but in most cases, using a GPU will be faster and more power efficient.
### _Can I use my old GPU from 10 years ago?_
That depends. For best results, devices which support OpenCL 1.2 are recommended. It has been demonstrated that OpenCL 1.1 devices can verify VDFs, but performance is significantly lower than cards even 1 generation newer, and the additional requirement of installing specific drivers to support the older cards can be challenging, depending on the operating system used.
### _Can I use chia plots/chives plots to farm MMX?_
No. Like Chives, mmx is using unique plots to differentiate itself from Chia. The netspace based variable reward structure dictates that growing netspace should require an additional cost to provide a level of stability to the value of the base coin. If the value of the coin rises, additional netspace will be plotted, which will increase the production of new coins, which will re-balance the value downwards in a feedback loop.
### _Will the plots I create now work on mainnet?_
Yes, there was an initial need to replot when a potential exploit was discovered that would allow usage of non-mmx plots. As it currently stands, all plots that are valid on testnet3 that were made with the latest release of the mad max plotter will work on mainnet.
### _Will there be portable pool plots?_
That is an intended feature to be ready before mainnet launch.
### _What is the minimum/maximum k-size for plots?_
For testnet, the minimum k-size is k26, for mainnet the minimum k-size will be k30 for 3 years, and then k31 for an additional 3 years. There is currently no maximum k-size limit (except k40 is currently a coded in limit) but the maximum you can currently plot is k34 with the latest mad max plotter, unless you write a custom implementation of chiapos.
### _Will the mmx coins I farm now stay in my wallet for mainnet?_
No, the coins are on the testnet blockchain only, and each testnet starts a new blockchain with a reset wallet. Mainnet will be a fresh blockchain as well.

# Technical Discussion FAQ:

### _How does the variable reward model work?_
The variable reward function is as follows: \
`reward = max(max(difficulty * const_factor, min_reward), TX fees)`. \
Where `min_reward` and `const_factor` are fixed at launch. This means that when a farmer wins a block, the reward they receive will be the highest one of three values: the minimum allowable farmer reward (set by min reward), the calculated farmer reward (based on netspace/difficulty), or the transaction fees for that block. If transaction fees are higher than farmer reward, no new coins are created, thus restricting coin supply and increasing coin price. This coin price increase will draw in new netspace which will increase the calculated reward until it is above the transaction fees, creating new coins to increase supply and thus stabilizing the price back downwards. There is no limit on coins produced, and instead the value of the coin will depend on cost to farm as well as actual use of the coin.
### _How do you intend to prevent a replotting/grinding attack?_
The replotting attack isn't so much an attack as it is an alternative way to provide proofs. Continuously replotting means using a very fast system to constantly make new plots that will always pass the filter, then dumping those plots out of memory when the next plot filter arrives. This would be the equivalent of having enough plots to always pass the plot filter. For a plot filter of 512, this means having 512 plots. While it could theoretically be possible to conduct such an attack with consumer hardware for k29 right now, it is not economically feasible to do so because it's much cheaper to buy storage for 512 plots. Passing the plot filter does not mean providing a successful proof and winning the block. To maintain mmx as proof of space, and prevent it from turning into a proof of work grind, k30 will only be the minimum k-size for 3 years after mainnet launch, and k31 for 3 years beyond that. In the event that technology advances faster than expected and grindable hardware is affordable sooner than expected, the plot filter can be reduced, thus lowering the number of "simulated" plots each fast plot is worth.