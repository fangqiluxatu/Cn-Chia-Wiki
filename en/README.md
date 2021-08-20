# Chia Blockchain Wiki

* [Home](en/)
* [Beginners Guide](en/Beginners-Guide)
* [Install instructions](en/INSTALL)
* [Quick Start Guide](en/Quick-Start-Guide)
* [FAQ](en/FAQ) Frequently Asked Questions
* [Pooling FAQ](en/Pooling-FAQ)
* [Pooling User Guide](en/Pooling-User-Guide)
* [Chia Project FAQ](https://www.chia.net/faq/)
* [Plotting Basics](https://www.chia.net/2021/02/22/plotting-basics.html)
* [Plot Sizes (k-sizes)](en/k-sizes)
* [CLI Commands Reference](en/CLI-Commands-Reference)
* [Windows Tips & Tricks](en/Windows-Tips-and-Tricks)
* [How to Check if Everything is Working (or Not)](en/How-to-Check-If-Everything-is-Working-(or-Not))
* [SSD Endurance](en/SSD-Endurance) - Important notes on using SSD to plot.
* [Reference Plotting Hardware](en/Reference-Plotting-Hardware)
* [Reference Farming Hardware](en/Reference-Farming-Hardware)
* [Farming on Many Machines](en/Farming-on-many-machines)
* [Good Security Practices on Many Machines](en/Good-Security-Practices-on-Many-Machines)
* [Chialisp Documentation (Official)](https://chialisp.com/)
* [Chialisp Notes](en/ChiaLisp)
* [Timelords and Cluster Timelords](en/Timelords)
* [Release Notes](https://www.chia.net/releases/)
* [RPC Interfaces](en/RPC-Interfaces)
* [Sync Issues - Port 8444](en/Resolving-Sync-Issues---Port-8444)
* [Logging Messages Reference](en/Logging-Messages-Reference)

The items below remain useful but we have implemented a significant revision to the consensus algorithm. We invite your comments and questions on our **[working draft of new consensus](https://docs.google.com/document/d/1tmRIb7lgi4QfKkNaxuKOBHRmwbVlGL4f7EsBDr_5xZE/edit)**.

These documents describe the original Chia Network Trunk (or consensus layer),
which is separate from the Foliage layer, which deals with coins, scripting,
and mempools.

Familiarity with either the Bitcoin or Ethereum protocols is assumed for this documentation.
The codebase is written in python, with several performance sensitive components (signatures, proof of space,
and proof of time), written in C++ and the clvm in Rust and Python.

1. [Consensus algorithm summary](en/Consensus-Algorithm-Summary)
2. [Block format](en/Block-Format)
3. [Network Architecture](en/Network-Architecture)
4. [Networking and Serialization](en/Networking-and-Serialization)
5. [Protocols](en/Protocols)
6. [Codebase and Testing](en/Codebase-and-Testing)
7. [Keys in Chia](en/Chia-Keys-Architecture)
