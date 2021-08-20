0. Stop all chia processes. Check that they have stopped with `ps -ef | grep chia`
1. Create a new chia root using `export CHIA_ROOT="~/.chia/my_testnet"`, then `chia init`. Don't forget to export CHIA_ROOT, or prefix your chia commands with `CHIA_ROOT="~/.chia/my_testnet"` if you want to run on my_testnet when starting a new terminal.
2. Create a new entry in config.yaml with a different GENESIS_CHALLENGE, and reduced DIFFICULTY_CONSTANT_FACTOR. 2^67 constant factor is around 110PiB assuming a fast timelord. So if you have around 110GiB, you can set it to 2 ^ 47. Decrease SUB_SLOT_ITERS_STARTING if you are using a slow computer. Decrease PLOT_FILTER if you want to have more proof checks per signage point. 
3. Make sure to add **my_testnet** to all places that need it, like network_overrides.config, and selected_network
4. Change the introducer URLs to point to localhost so you don't contact the real ones
5. Do `sh install-timelord.sh`
6. Run the system with `chia start all`
7. If you have installed the gui, run `(cd chia-blockchain-gui && npm run electron &)`