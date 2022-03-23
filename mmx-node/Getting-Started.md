To continue enter the CLI environment:
```
cd mmx-node
source ./activate.sh
```

## Creating a Wallet

```
mmx wallet create [-f filename]
```

The file name argument is optional, by default it is `wallet.dat`, which is already included in the default configuration.

To use more wallets add the paths to `key_files+` array in `config/local/Wallet.json`.

To create a wallet with a known seed value:
```
mmx wallet create <seed> [-f filename]
```

To get the seed value from a wallet:
```
mmx wallet get seed [-j index]
```

Note: A node / farmer restart is needed for the farmer to pick up a new wallet.

Note: Alternatively you can create a wallet in the GUI now: http://localhost:11380/gui/#/wallet/create

## Configuration

#### Custom Farmer Reward Address

Create / Edit file `config/local/Farmer.json`:
```
{
	"reward_addr": "mmx1..."
}
```
By default the first address of the first wallet is used.

#### Custom Timelord Reward Address

Create / Edit file `config/local/TimeLord.json`:
```
{
	"reward_addr": "mmx1..."
}
```
By default the first address of the first wallet is used.

## Plotting

To get the farmer and pool keys for plotting:
```
mmx wallet keys [-j index]
```

The node needs to be running for this command to work. (`-j` to specify the index of a non-default wallet)

Then use the latest version of my plotter with `-x 11337` argument: https://github.com/madMAx43v3r/chia-plotter \
For windows: https://github.com/MMX-World/mmx-plotter/releases/tag/v0.0.1

It will show the following output at the beginning to confirm the new plot format (from testnet3 onwards):
```
Network Port: 11337 [MMX] (unique)
```
The new plots will have a name starting with "plot-mmx-". Plots created before that version are only valid on testnet1/2.

The minimum K size for mainnet will be k30, for testnets it is k26. The plots from testnet3 and onwards can be reused for mainnet later.
However there will be a time limit for k30 and k31, ~3 years for k30 and ~6 years for k31, to prevent grinding attacks in the future.

To add a plot directory add the path to `plot_dirs` array in `config/local/Harvester.json`, for example:
```
{
	"plot_dirs": [
		"/mnt/drive1/plots/",
		"/mnt/drive2/plots/",
		"C:/windows/path/example/"
	]
}
```
Directories are searched recursively by default. To disable recursive search you can set `recursive_search` to `false` in `Harvester.json`.
## Running a Node

First perform the installation and setup steps.

To run a node for the current testnet:
```
./run_node.sh
```

You can enable port forwarding on TCP port 12334 if you want to help out the network and accept incoming connections.

To set a custom storage path for the blockchain, etc:
```
echo /my/path/ > config/local/root_path
```

To set a custom storage path for wallet files create/edit `config/local/Wallet.json`:
```
{
	"storage_path": "/my/path/"
}
```

To disable the `TimeLord` specify `--timelord 0` on the command line.
Alternatively, you can also disable it by default: `echo false > config/local/timelord`.
If you have a slow CPU this is recommended and maybe even needed to stay in sync.

Any config changes require a node restart to become effective.

### Light Node

The node can be run in a "light mode" which filters out any transactions which don't concern your addresses, in addition to disabling message relaying and transaction verification.
A light node relies on other full nodes to validate transactions and only checks that transactions of interest were included in blocks with valid proof (aka SPV, Simplified Payment Verification). 

To run a light node:
```
./run_light_node.sh
```
Light node data will end up in `testnetX/light_node/` by default, there is no conflict with data from a full node.

Note: You cannot add a new wallet afterwards (or increase the number of addresses), if you do so you have to re-sync from scratch.

### Reducing network traffic

If you have a slow internet connection or want to reduce traffic in general you can lower the number of connections in `config/local/Router.json`.
For example to run at the bare recommended minimum:
```
{
	"num_peers_out": 4,
	"max_connections": 4
}
```
`num_peers_out` is the maximum number of outgoing connections to synced peers. `max_connections` is the maximum total number of connections.
Keep in mind this will increase your chances of losing sync.

Another more drastic measure is to disable relaying messages to other nodes, by setting `do_relay` to `false` in `config/local/Router.json`.
However this will hurt the network, so please only disable it if absolutely necessary.

### Running in background

To run a node in the background you can enter a `screen` session:
```
screen -S node
(start node as above)
<Ctrl+A> + D (to detach)
screen -r node (to attach again)
```

### Recover from forking

To re-sync starting from a specific height: `./run_node.sh --Node.replay_height <height>`.
This is needed if for some reason you forked from the network. Just subtract 500 or 1000 blocks from the current height you are stuck at.
To re-sync from scratch delete `block_chain.dat` and `db` folder in `testnetX`.

### Switching to latest testnet

After stopping the node:
```
rm NETWORK
./update.sh
./run_node.sh
```
Blockchain data are now stored in `testnetX` folder by default.