To use the CLI:
```
cd mmx-node
source ./activate.sh
```

The node needs to be running, see [Getting Started](https://github.com/madMAx43v3r/mmx-node/wiki/Getting-Started) for how to start it.

## Wallet CLI
To check your balance: `mmx wallet show`

To show wallet activity: `mmx wallet log`

To send coins: `mmx wallet send -a 1.234 -t <address>`

To show the first 10 addresses: `mmx wallet show 10`

To get a specific receiving address: `mmx wallet get address <index>`

To show wallet activity since height: `mmx wallet log <since>`

To show wallet activity for last N heights: `mmx wallet log -N`

To get the seed value from a wallet: `mmx wallet get seed`

To use a non-default wallet specify `-j <index>` with above commands (at the end).

## Node CLI
To check on the node: `mmx node info`

To check on the peers: `mmx node peers`

To check on a transaction: `mmx node tx <txid>`

To check the balance of an address: `mmx node balance <address>`

To check the history of an address: `mmx node history <address> [since]`

To dump a contract: `mmx node get contract <address>`

To dump a transaction: `mmx node get tx <txid>`

To dump a block: `mmx node get block <height>`

To dump a block header: `mmx node get header <height>`

To force a re-sync: `mmx node sync`

To get connected peers: `mmx node get peers`

## Farmer CLI
To check on the farm: `mmx farm info`

To get total space in bytes: `mmx farm get space`

To show plot directories: `mmx farm get dirs`

To reload plots: `mmx farm reload`

## Stake Minting

Some tokens can be minted by staking another currency over time. This is the case with `MMT`, an example token on testnet, it has a stake factor of 1/10000 per block for `MMX`. So if you stake 1000 `MMX` you will create 0.1 `MMT` per block.

To start staking you first create a `mmx.contract.Staking` contract as follows:
```
{
	"__type": "mmx.contract.Staking",
	"owner": "mmx1nn8u9etvnghq7x8atj2y55he76z9yvxalc9t3nx8ym0xqr4yuzvsdf8jp8",
	"currency": "mmx1rywgqms3crugehp22c624w02g6ls7y2frq8kwc0fcyxa5ywusldsc8s5qj",
	"reward_addr": "mmx1nn8u9etvnghq7x8atj2y55he76z9yvxalc9t3nx8ym0xqr4yuzvsdf8jp8"
}
```
Where `currency` is the token contract address, above example is for `MMT`.
Make sure to replace `owner` and `reward_addr` with addresses that you control and save the file with a `.json` extension somewhere.

Once you have written the contract you can deploy it on the blockchain:
```
mmx wallet deploy path/to/contract.json
```
You will see that the contract has been deployed together with it's address:
```
Deployed mmx.contract.Staking as mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q
```
Note: You need some MMX to perform this step, around 0.01 MMX should be enough.

After deploying the contract you should be able to see it in `mmx wallet show`:
```
Contract: mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q (mmx.contract.Staking)
```
Note: The address will be different for you.

To confirm the contracts content you can dump it via:
```
mmx node get contract mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q
```

Now you can send `MMX` to this contract via:
```
mmx wallet send -a 10 -t mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q
```
Make sure to replace the address with your contract address.

You should see the staked balances in `mmx wallet show`:
```
Contract: mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q (mmx.contract.Staking)
  Balance: 10 MMX (10000000)
```

The rewards will be sent to `reward_addr` when you remove the balance from this contract again. However there is a minimum period the coins have to be residing in the contract, on testnet it's currently just 18 blocks. You can withdraw before that time, but you wont get any rewards.

To withdraw balance from the contract and trigger a minting reward:
```
mmx wallet send_from -a 10 -s mmx16e6ecj2a5u5gvxfrwjdh5v7s5x5snclj3vft5atr3pagxddq7d9qdqhc2q -t mmx1nn8u9etvnghq7x8atj2y55he76z9yvxalc9t3nx8ym0xqr4yuzvsdf8jp8
```
Make sure to replace the source `-s` with your contract address and target `-t` with your destination address.

After withdrawing the balance you should see the reward being sent to your `reward_addr` within the same transaction:
```
[9759] RECEIVE + 0.33 MMT (330000) -> mmx1nn8u9etvnghq7x8atj2y55he76z9yvxalc9t3nx8ym0xqr4yuzvsdf8jp8
[9759] RECEIVE + 10 MMX (10000000) -> mmx1nn8u9etvnghq7x8atj2y55he76z9yvxalc9t3nx8ym0xqr4yuzvsdf8jp8
```
You can also send the funds right back to the contract.
