翻译自[2021年5月9日版本-3#](https://github.com/Chia-Network/chia-blockchain/wiki/Resolving-Sync-Issues---Port-8444/d1bcb6d8df82813a8258ab27f5466d493fd6530e)
***
可以的话，建议开放并转发chia同步节点所用的8444端口，这样有助于chia主网的稳定健康。

8444端口是用来与chia主网中其它节点相互通信连接的。开放并转发8444端口以后，你本地的chia软件节点将迅速与主网其它节点连接、通信并开始下载区块数据，以保持与主网同步。

随着主网的发展，区块数据日益增长。许多新的加入者未开放节点的8444端口，这将不利于主网的稳定健康发展，在此，我们建议大家，请将已运行chia节点主机的8444端口开放出来。


使用该链接的小工具，来查看你的节点外网8444端口是否开放: [端口扫描 - 站长工具](https://tool.chinaz.com/port/)

## 如何开放端口？
面向外网（互联网）的8444端口转发开放需要在路由器上完成。关于路由器上的设置，需要根据你的型号来操作，详细操作方式请浏览你的路由器使用手册或者自行搜索如何转发开放端口。

当你转发开放8444端口以后，意味着你将允许chia主网的其他节点通过8444端口与你连接通信，共同维护主网的稳定运行。

### 端口转发设置
路由器设置端口转发的方式大同小异，不同厂商的路由器对此设置的命名可能不太一样，但原理上基本是一致的：外网计算机通过8444端口连接到你本地内网中运行chia节点的计算机。

以下是常规路由器的设置方式：
* 通信协议类型设置为 _TCP_ 或者 _TCP & UDP_ 。
* 本地（或转发）IP地址 - 即内网中运行chia节点（计算机）的IP地址；自行搜索如何查看本机的内网IP地址。
* 源IP地址 - 设置为全部或者填 * 。

### 为什么要转发8444端口?

chia主网中所有关闭了8444端口的节点，都是依赖那些开放了8444端口的节点来进行主网区块同步的，因为他们是主网中唯一可供相互通信的节点们。如果主网有1000个节点开放了8444端口，但有20000个节点并未开放8444端口，因为每个开放端口的节点连接数是有限的，chia默认允许的被连接数为10（config.yaml - max_inbound_farmer），也就是说，将会有很多未同步的节点在等待有节点开放8444端口，以保证同步至最新区块。现在，这个情况越来越差，这是由于大量节点没有开发8444端口而导致的。

所以，我们呼吁，如果可以的话，请chia节点们开放8444端口，这样就会有更多的节点与你互相连接，从而使得chia主网更加稳定健康。

### 加快连接到节点的速度
如果你想快速连接到其他节点，并提高同步的速度，可以添加下面这些节点：
* 北亚 `introducer-apne.chia.net:8444`
* 南亚 `introducer-apse.chia.net:8444`
* 美西: `introducer-or.chia.net:8444`
* 美东 `introducer-va.chia.net:8444`
* 欧洲: `introducer-eu.chia.net:8444`

也可以查阅这个分享公共节点地址的网站：
* [chia.keva.app](https://chia.keva.app)

可以在GUI客户端界面点击'连接到其他节点'进行添加，或者使用命令：'chia show -a PEER_ADDRESS:PORT'，其中'PORT'为端口号，无特殊情况设置为8444。

## Detailed explanation 
A regular pc can communicate **out** with endless ports-- if the user is sending a signal out -- pc opens a port -- signal goes out, pc closes the port. 
Chia uses port 8444 as instant verified communication.  So an open port can allow instant communication and start the blockchain sync.  Signal comes in on port 8444- that Chia pc is verified, then **both** user's pc, opens their own "communication ports ex port 8421" and that new user can now sync and now they are linked together forming part of Chia mesh. 

 If the users port 8444 is closed, the users pc has to start sending multiple signals out and hope that a pc with open port 8444 will link with them, then the sync starts. (1) pc can only link up a few pc and with so many other chia users coming on board, they all have to wait.  Keep in mind, Chia is built on a mesh network, the blockchain is shared among all the users, not from central pc. 

### Dealing With Carrier-Grade NAT

Opened port 8444 on your router but still not getting connections? With the exhaustion of the IPv4 space, it's increasingly common for ISPs to use [Carrier-Grade NAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) (CGN, CG-NAT, NAT444) to put multiple customers behind a single IP address. In this case, even if you open 8444 on your router, other nodes will not be able to connect to you. There are a couple options:

1. Ask your ISP for a dedicated IP address. They'll probably want more money, and may require you to upgrade to a "business" plan.

2. Establish a VPN tunnel through the NAT to a cloud server with a public IP address. It's easier than it sounds, and can cost as little as $3-5 a month for a cheap cloud server (some common cloud server providers: AWS, GCP, Digital Ocean, Vultr, Hetzner, Linode). When selecting a provider and server size, pay careful attention to bandwidth; a Chia fullnode isn't too demanding, but can require several GB per day. 1 TB per month is typical of lower-cost VPSs and should be plenty (do keep an eye on it though, bandwidth overage costs can be expensive!).

Setting up a VPN used to be a daunting task, but [Wireguard](https://www.wireguard.com) has greatly simplified it. The summary is you run Wireguard on both your home server and the cloud server:
* the cloud server is configured to listen for VPN connections from your home server, and route all traffic incoming on 8444 to your home server, while also routing outgoing traffic from your home server to the internet.
* the home server is configured to route all internet traffic (but not local) through the cloud server, while periodically sending a "keepalive" packet to ensure the connection stays open.

A more detailed write-up with example wireguard configuration can be found [here](https://www.kmr.me/posts/wireguard/).