翻译自[2021年5月3日版本-3#](https://github.com/Chia-Network/chia-blockchain/wiki/Good-Security-Practices-on-Many-Machines/553dea52c8a38e8dd12234213b6b2302ec5fa91f)
***

（本文是基于@mariano54在[github讨论话题中](https://github.com/Chia-Network/chia-blockchain/discussions/1116#discussioncomment-420398)的回复总结而成。）

保证耕种过程的安全是非常必要的。当然了，我们无法做到100%的安全，但还是要尽可能的选择安全的方式来进行耕种。

## 独立使用私钥

换句话说，_只在必须要登陆账户的主机上使用。_

* 你的耕种账户（私钥）不要在开垦机（P图机）上登陆。
* 你的耕种账户（私钥）不要在收割机上登陆。

## 多机耕种

### 多机开垦农田

在[多机集群耕种](Farming-on-many-machines)文档中的前情提要有提过：
> 当你想在其它收割机上开垦农田的话，使用命令： `chia plots create -f farmer_key -p pool_key`，填写对应账户的农民公钥（-f）及矿池私钥（-p）。也可以在收割机上使用命令 `chia keys add` 输入助记词来添加账户，不过这样不安全。

### 多机集群收割
参考[多机集群耕种](Farming-on-many-machines)文档，正确配置收割机的加密证书文件即可。

# 安全钱包的用法
有一种方式可以防止你的钱包不被黑客攻击，那就是不将钱包联网。详见文档[Chia私钥管理](Chia-Keys-Management)。
> 建议将获取Chia区块奖励的地址单独设置成一个离线的钱包地址。你可以在一台离线的计算机上生成私钥，将获取的钱包地址填入耕种机的`config.yaml`（修改farmer.xch_target_address 以及 pool.xch_target_address）。这样的话，如果耕种机被黑客侵入，就不需要担心代币奖励的安全问题了。

## 如何已登录的私钥
_请在安全的系统环境下使用此命令，因为你的公钥及私钥都将会显示出来。_
你需要了解一下[CLI命令行工具的用法](CLI-Commands-Reference.md)。使用命令：`chia keys show`可查看本机所有已登录的账户（私钥）。使用命令 `chia keys show --show-mnemonic-seed`可以查看账户的公钥、私钥及助记词信息。