- [General FAQ](#general-faq)
- [Technical FAQ](#technical-faq)
***

# General FAQ

## How can I start pooling?
You can update by installing Chia 1.2+ and following the instructions in the [Pooling User Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Pooling-User-Guide).


## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new K32 or above portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of ~30 minutes (100 blocks) between each switch. Each switch between pools will require a transaction with a smart contract on the blockchain. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

## What is a plot NFT?
A plot NFT (Non Fungible Token), is a smart coin or token on the blockchain, which allows a user to manage their membership in a pool. User's can assign the plot NFT to any pool they want, at any point. When plotting, a plot NFT can be selected, and the plot will be tied to that plot NFT forever. NFTs are "non-fungible" because they are not interchangeable; each plot NFT represents a unique pool contract.


## Will I need pay XCH to create a plot NFT or switch pools?
Each plot NFT you create will require a minimum of 1 mojo (1 trillionth of a XCH) + transaction fee. For switching pools, you need to pay only a transaction fee. to switch pools. On the first few days of pool launch on mainnet, it's likely you can use 0 transaction fee. For those who don't have any XCH, you can get 100 mojos from Chia's official faucet: https://faucet.chia.net/

## Can I farm with both OG (original) plots and portable plots?
Yes. The farmer will support both OG plots and portable plots on one machine. For OG plots, the 0.25XCH and 1.75XCH will both be sent to the farmer. The OG plots will not be affected in any way by the plot NFTs or new plots that you create.

## How do I assign portable plots to a pool?
First you will create a Plot NFT (devs call this singleton in their code) in the new pools tab in the GUI. When you create a new portable plot, you must assign it a specific Plot NFT (for those using CLI, this replaces the Pool Public Key `-p` with a Pool Contract Address `-c`). All plots created with the same Plot NFT can then be assigned to a pool for farming. You can have many plot NFTs on the same key.

## What is the difference between a "Key" and a "Wallet" in the Chia GUI and CLI?
A user can have one or more keys on a machine running Chia. A key is represented by the private information (24 words) and the public identifier called the `fingerprint`. When using the GUI or the CLI, you can only log in to one key at a time. Each key must be synced separately, and you can check if it is synced by clicking on the "wallet" tab. Each key can also have 1 or more wallets associated with it. The standard wallet, which controls your Chia, is created by default. You can also create as many Plot NFTs as you want, which are also wallets, and each have their own "wallet id" as well, and they are tied to the key that you used to create them. In the CLI, you use both `fingerprint` and `wallet_id` to perform operations on Plot NFTs, which represent the key and wallet ID for that Plot NFT.

## How is Chia pooling different from other cryptos?
Chia has three major differences from most other crypto pooling protocol: 1) Joining pools are permissionless. You do not need to sign up to an account on a pool server before joining. 2) Farmers receive 1/8th of XCH rewards plus transaction fees, while the pool receives 7/8th of XCH rewards to redistribute (minus pool fees) amongst all pool participants. 3) The farmer with the winning proof will farm the block, not the pool server.

## How can I start my own pool?
If you have experience writing pool server code for another crypto, adapting that pool code with Chia's reference pool code will be straight forward. We only recommend people who have good OPSEC and business experience to run public pool servers. Depending what country you operate your pooling business, you may be subject to tax, AML and KYC laws specific to your jurisdiction. All pools will be targeted by hackers due to the profitability of XCH and you may be legally liable if you have any losses.

## Where can I find a list of Chia pools?
A crypto community site lists all upcoming Chia pools: https://miningpoolstats.stream/chia

## Can I advertise my pool in Keybase?
You can only advertise your pool in Keybase @chia_network.public#pools once a day. If you're spammy, mods will warn you and then ban you if you persist.

## Why shouldn't I join the original Hpool pool?
Hpool initially created their own version of Chia client that has no source code released with it. There is no telling what kind of malicious activity that client can do. Chia Network Inc discourages everyone from joining any pool that requires custom closed source clients however Hpool has since opened a pool using the official pooling protocol and you should feel free to consider that as you look at which pool to use.

## Why doesn't Chia run their own official pool?
We want there to be a healthy ecosystem of competing pools with no privileged official one having an unfair advantage over the others.

## Can I name my pool chiapool.com?
We are not going to allow pools to use "Chia" as the first word or its equivalent (the chia pool). You can say things like "a Chia pool" though that will probably need a free and easy to get license. Go to https://www.chia.net/terms/ to get more information on obtaining a license.

## If a pool gets 51% of netspace, can they take over the network?
No, Chia's pooling protocol is designed where the blocks are farmed by individual farmer, but the pooling rewards go to the pool operator's wallet. This ensures that even if a pool has 51% netspace, they would also need to control ALL of the farmer nodes (with the 51% netspace) to do any malicious activity. This will be very difficult unless ALL the farmers (with the 51% netspace) downloaded the same malicious Chia client programmed by a Bram like level genius.

## I have more questions, where do I ask?
Join our dedicated Keybase room: @chia_network.public#pools

Friendly reminder: do NOT at `@` or Direct Message (DM) developers or mods. Just post your questions in Keybase and we will answer when we have a moment.

# Technical FAQ

## Where can I see the Chia Pool Reference Code?
You can find it here: https://github.com/Chia-Network/pool-reference.
The README contains an explanation of how it works, and the specification contains details of how to implement it.

## What programming language is the reference pool code written in?
Python

## How hard is it to adapt Chia's reference pool code to my pool code?
If you've written pool code before, the reference pool code will be easy to understand. It's just replacing PoW concepts with Chia's method of evaluating each farmer's participation via PoST and adapting collection and distribution of XCH using Chia's smart contracts.

## I am a programmer, but never wrote pool code, will I be able to run a pool with Chia's reference pool code?
If it's your first time writing pool code, we recommend you look at established BTC or ETH pools source code and features they provide users. You are likely going to compete with big time pool operators from those crypto communities who will provide feature rich pools for Chia on day one. Examples of features: leaderboards, wallet explorer, random prizes, tiered pool fees, etc.

## Variable names used in pooling code
- puzzle_hash: an address but in a different format. Addresses are human readable.
- singleton: a smart coin (contract) that guaranteed to be unique and controlled by the user.
- launcher_id: unique ID of the singleton.
- points: represent the amount of farming that a farmer has done. It is calculated by number of proofs submitted, weighted by difficulty. One k32 farms 10 points per day. To accumulate 1000 points you need 10 TiB farming for a day. This is equivalent to shares in PoW pools.

## How does one calculate a farmer's netspace?
A farmer's netspace can be estimated by the number of points submitted over each unit of time, or points/second.  Each k32 gets on average 10 points per day. So `10 / 86400 = 0.0001157 points/second` for each plot. Per byte, that is `L = 0.0001157 / 106364865085 = 1.088 * 10^-15`. To calculate total space `S`, take the total number of points found `P`, and the time period in seconds `T` and do `S = P / (L*T)`.  
For example for 340 points in 6 hours, use `P=340, T=21600, L=1.088e-15`, `S = 340/(21600*1.088e-15) = 14465621651619 bytes`. Dividing by `1024^4` we get `13.15 TiB`. 

## How does difficulty affect farmer's netspace calculation?
As difficulty goes up, a farmer does less lookups and finds less proofs, but does not receive more points per unit of time. Imagine this scenario: Obtaining 10 proofs a day with difficulty 1 for a k32, is equivalent to obtaining 1 proof a day with difficulty 10. As a pool server, you prefer to receive 1 proof a day per K32 with difficulty 10. This is why we allow pool servers to set a minimum difficulty level to reduce the number of proofs each farmer needs to send to prove their netspace.

## How do you identify the farmer that submitted partial proofs?
The farmer will provide their launcher_id which is the ID of that farmer's pool group. The pool also verifies the proof of space and the farmer's signature, to make sure that only real farmers are compensated.

## Will pool servers need to keep track of all farmers and their share of rewards?
Yes, the pool operator will need to write code to keep track of all farmers and their share of rewards. Chia's pool protocol assumes no registration is needed to join a pool, so every launcher_id that submits a valid partial proof needs to be tracked by the pool server.

## What actions can singleton take?
There are a few things you can do to the singleton:
- Change pool (needs owner signature)
- Escape pool, this is announcing that you will change pool (needs owner signature)
- Claim rewards (does not need any signature, it goes to the specified address in the singleton)

## How do pool collect rewards?
- Farmer joins a pool, they will assign their singleton to the pool_puzzle_hash.
- When a farmer wins a block, the pool rewards will be sent to the p2_singleton_puzzle_hash.
- Pool will scan blockchain to find new rewards sent to Farmer's singletons.
- The pool will send a request to claim rewards to the winning Farmer's singleton.
- Farmer's singleton will send pool rewards XCH to pool_puzzle_hash.
- Pool will periodically distribute rewards to farmers that have points

## How can I tell if the server is receiving enough partials from a particular client? 
The number of partials received is the only thing the pool is aware of, the pool does not know the exact total space
of the farmer. The space can be computed using the fact that each k32 plot will earn on average 10 points a day, on 
mainnet. That means if the difficulty is set to 1, that's 10 partials per day, if the difficulty is 10, 1 partial per 
day per k32 plot.

## Why am I receiving more points in testnet than mainnet?
The 10 points per day per k32 plot only applies to mainnet, which has a `DIFFICULTY_CONSTANT_FACTOR` of 2^67. To get
the points per day per k32 on testnet, divide 2^67 by the testnet `DIFFICULTY_CONSTANT_FACTOR`, found in `config.yaml`, and
multiply by 10. This allows participating easily with k25s on testnet.

## What is the expected ratio between a k32 and a k25? 
Look at the file `win_simulation.py` on this repo. This uses the function `_expected_plot_size` from chia blockchain,
which uses the formula:  `((2 * k) + 1) * (2 ** (k - 1))` to compute plot size. Plug in your k values and divide.

## How to calculate how many partials with X difficulty a certain plot with Y size can get in Z time?
Look at the `win_simulation.py` file.

## Can I use testnet pooling plots on mainnet?
No, you can only use plots created for mainnet in mainnet, and same for testnet.

## Does that mean that forks of Chia cannot use these pooling plots?
Forks of Chia can easily use these pooling plots by sending the 1.75XCH to the farmer target address, making them
all solo plots. If the alternate blockchain wants to do pooling as well, they need to create a special transaction
which `reserves` a singleton by providing the `launcher_id`, and launcher spend (including owner signature). Then
the code can automatically assign this singleton to the user who submitted it.

## What are the API methods a pool server needs to support Chia clients?
There are a few API methods that a pool needs to support. They are documented here:
https://github.com/Chia-Network/pool-reference/blob/main/SPECIFICATION.md

## Where can I see the video Technical Q&A on Chia Pooling:
For those interested in the Chia Pools for Pool Operators video and presentation, you can find it here: 
https://youtu.be/XzSZwxowPzw
https://www.chia.net/assets/presentations/2021-06-02_Pooling_for_Pool_Operators.pdf