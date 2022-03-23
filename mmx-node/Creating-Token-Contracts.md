# Token Contracts
Token contracts can be created either as a mint by owner only token, a mint by owner and staking token, or an ownerless staking token. The advantage of the mint only token is that the owner address controls the supply of the token, similarly for the mint and stake token, the owner can create an initial supply for immediate use, and then future supply can be provided by staking. The advantage of the ownerless staking token is that there is no fixed supply, and new tokens can only be created through staking. Unlike the mint and stake, the token creator can not create a bunch of tokens out of thin air.

To deploy any contract to the blockchain, you will need to have a small amount of mmx in your wallet.

## The Contract Json
To create a token contract, a json file is used, and then the json is deployed by the wallet. The basic json structure is as follows:
```
{
        "__type": "mmx.contract.Token",
        "name": "",
        "symbol": "",
        "decimals": ,
        "icon_url": "",
        "web_url": "",
        "owner": "",
        "stake_factors": "",
        "time_factor": "",
        "version": 0
}
```
To make a minting only token, `stake_factors` and `time_factors` can be left blank. For ownerless tokens, the `owner` can be left blank. The `stake_factors` is an array which contains the currency addresses used for staking, as well as the reward factor for creating new coins. Please see the examples below.

### Minting Token Contract
```
{
        "__type": "mmx.contract.Token",
        "name": "Memecoin",
        "symbol": "MEME",
        "decimals": 6,
        "icon_url": "",
        "owner": "mmx1mqxtcngn4lv0y3n30c5mw4lhh4x7h7zjrs0r2qynethgy20fayaq3ngsm8",
        "stake_factors": "",
        "time_factor": "",
        "version": 0
}
```
In this example for memecoin, the decimals is set to 6 (1 million mojo equivalent) which sets 1 memecoin to the same scale as 1 mmx. The owner address must be set to a wallet address that you control, or else you won't be able to use the contract to mint tokens. This gets saved as a json file, and can then be deployed with `mmx wallet deploy /path/to/file.json`. When deployed, the contract will appear in the owner's wallet and tokens can be minted with `mmx wallet mint -a <amount> -x <token contract address> -t <target wallet address>`.

### Minting and Staking Token Contract
```
{
        "__type": "mmx.contract.Token",
        "name": "BigBux",
        "symbol": "BUX",
        "decimals": 2,
        "icon_url": "",
        "owner": "mmx1mqxtcngn4lv0y3n30c5mw4lhh4x7h7zjrs0r2qynethgy20fayaq3ngsm8",
        "stake_factors": [
                [
                        "mmx1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqdgytev",
                {
                        "__type": "mmx.ulong_fraction_t",
                        "inverse": 10000,
                        "value": 1
                }
                ]
        ],
        "time_factor": {
                "__type": "mmx.ulong_fraction_t",
                "inverse": 1,
                "value": 0
        },
        "version": 0
}
```
Shown here is the example for BigBux token, which can be minted by the owner address, or staked using the currency address shown in `stake_factors` which is mmx in this example. 

The other staking factor is the rate at which the coins are earned from staking, in this example it is 1/10000 of the stake per block. This is the largest increment allowed, so 10000 is the smallest number that can be used for the `inverse` value. 

`time_factor` is currently not implemented yet. If left blank, the `time_factor` will automatically be filled in as shown in the example. The other difference in this token is the decimals is now 2, which means 1 BUX is equivalent in scale to 2 mojos, or 0.000020 mmx. 

This contract is also saved and deployed as a json in the same manner: `mmx wallet deploy /path/to/file.json`. When deployed, the contract will appear in the owner's wallet and tokens can be minted with `mmx wallet mint -a <amount> -x <token contract address> -t <target wallet address>`.

For creating a staking contract to mint a token, see [Stake-Minting](https://github.com/madMAx43v3r/mmx-node/wiki/CLI-Commands#stake-minting).

### Ownerless Staking Token Contract
```
{
        "__type": "mmx.contract.Token",
        "name": "SmallBux",
        "symbol": "$BUX",
        "decimals": 6,
        "icon_url": "",
        "owner": "",
        "stake_factors": [
                [
                        "mmx179h70rfctfahne7eulg29hsnnrjxgprvex7epy08pyq2zdghfyus24wqjn",
                {
                        "__type": "mmx.ulong_fraction_t",
                        "inverse": 100000,
                        "value": 1
                }
                ]
        ],
        "time_factor": {
                "__type": "mmx.ulong_fraction_t",
                "inverse": 1,
                "value": 0
        },
        "version": 0
}
```
In the SmallBux example, the `decimals` has been set back to 6 to match mmx in scale, but now the `owner` has been left blank. When the `owner` is blank, deploying the contract will result in the ownership being assigned to the zero address for mmx:  `mmx1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqdgytev`. Additionally the `inverse` has been set to 100000, so each block will only create $BUX equivalent to 1/100000 of what was staked. 

The final change in this example is that the staking currency has been changed to `mmx179h70rfctfahne7eulg29hsnnrjxgprvex7epy08pyq2zdghfyus24wqjn` which is the contract address for BigBux. This means that SmallBux can only be created by staking BigBux through a staking contract, so the supply is unlimited and uncontrolled by any individual. 

This contract is deployed in the same manner with a json file and `mmx wallet deploy /path/to/file.json`. **Because this contract is ownerless, it will not appear in any wallet when deployed, so make sure you save the address from the output of deploying.** Once the token contract is deployed to the blockchain, you can then create a staking contract using the token address you just created: [Stake-Minting](https://github.com/madMAx43v3r/mmx-node/wiki/CLI-Commands#stake-minting).