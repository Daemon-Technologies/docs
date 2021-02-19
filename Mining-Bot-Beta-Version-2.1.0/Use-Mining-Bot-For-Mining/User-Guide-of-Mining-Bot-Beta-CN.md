---
sort: 2
---

# 挖矿机器人使用说明书 - Beta版

本教程将会介绍如何使用挖矿机器人进行STX挖矿。

- [前序环境搭建教程](../Build-Before-Using/Mining-Bot-Beta-Tutorial-CN.md)

**【提醒】如果想获取更全面的信息，请查看我们的视频教程。**
- [Stacks挖矿机器人客户端2.1.0-Mainnet版](https://www.bilibili.com/video/BV1zb4y1R7F9/)

- [Stacks挖矿机器人客户端2.1.0-Xenon版](https://www.bilibili.com/video/BV1hv411Y7pr/)


## 1. 登陆页面介绍

当你在浏览器中输入 **http://localhost:8000/** 的时候，会看到如下界面：

![index-set-lock](assets/CN/index/index-set-lock.png)


当你第一次登陆的时候，会提示让您输入**锁定密码**，该密码主要用于**登陆认证**、**私钥加密保护**，这里的锁定密码和之前的yarn start node1234认证密码没有关系，没有必要保持一致。

当输入两次相同的密码后，就会进入挖矿机器人的主页面。进入主页面后可以通过下图中右上角的账户状态栏进行账户锁定。

![index-lock-account](assets/CN/index/index-lock-account.png)



点击**锁定账户**后会跳转到如下界面，需要重新输入第一次设置的锁定密码进行账户解锁。

![unlock-account](assets/CN/index/unlock-account.png)

:artificial_satellite:**【提醒】该密码无法被恢复**

主页面由四个部分组成：**公开数据页面**、**钱包账户页面**、与**挖矿客户端页面**，**以及系统配置页面**，接下来将逐个页面讲解如何通过挖矿机器人获取挖矿数据并参与挖矿。

如下图所示，公开数据页面旨在为挖矿机器人策略提供丰富的数据来源，公开数据页面如上图所示。现阶段包含如下信息：
- 币价信息：STX、BTC交易对信息

- 链信息

- 区块信息

![public-data](assets/CN/publicdata/public-data-2.1.0.png)

​    
## 2. 钱包页面

### 2.1 比特币与Stacks地址在线生成

### 比特币与Stacks地址在线生成

:artificial_satellite:**【提醒】如果你有24助记词的BTC或STX地址，可以选择跳过本节**

本小节参考[官方挖矿文档](https://docs.blockstack.org/mining)中对于生成在线地址的指令。

运行如下指令：

``` 
npx @stacks/cli make_keychain -t
```

运行上述指令后会看到很多安装日志，在最后你可以看到一个JSON，类似于：

```
{
  "mnemonic": "exhaust spin topic distance hole december impulse gate century absent breeze ostrich armed clerk oak peace want scrap auction sniff cradle siren blur blur",
  "keyInfo": {
    "privateKey": "2033269b55026ff2eddaf06d2e56938f7fd8e9d697af8fe0f857bb5962894d5801",
    "address": "STTX57EGWW058FZ6WG3WS2YRBQ8HDFGBKEFBNXTF",
    "btcAddress": "mkRYR7KkPB1wjxNjVz3HByqAvVz8c4B6ND",
    "index": 0
  }
}
```
:artificial_satellite:**【提醒】上述信息请务必保存，为比特币、Stacks私钥等核心信息**

### 2.2 添加账户：比特币与Stacks地址导入
点击钱包账户页面中的**添加账户**一栏，会弹出导入地址的对话框，将24个助记词按照拷贝粘贴至对话框中（助记词之间用空格隔开），并选择相应类型，便可完成地址导入。

在钱包账户页面中点击**添加账户**：

![](assets/CN/wallet/importAddress1.png)

将24个助记词按照拷贝粘贴至对话框中（助记词之间用空格隔开），并选择账户类型，点击提交：

![](assets/CN/wallet/importAddress2.png)

可以在列表中看到新增的地址，以及其对应的类型与账户余额。

![](assets/CN/wallet/importAddress3.png)



**获取BTC测试币：**https://testnet-faucet.mempool.co/

注意：将网络切换为Xenon测试网络，在页面的右上角显示的是当前的测试网。

![xenon-testnet](assets/CN/wallet/xenon-testnet.png)



## 3. 客户端界面

进入客户端页面，看到当前状态为Mining-Local Server已运行，但是`stacks-node`程序未找到。

![current_state_mining_cn](assets/CN/client/current_state_mining_cn.png)



## 4. 系统配置界面

![system_config_cn](assets/CN/systemconfig/system_config_cn.png)



如果网络正常，显示的是主链和本地链的信息，可以进行如Mining-Local-Server、本地Stacks链、要同步的BTC节点等信息的配置。

在启动挖矿配置比特币节点时，填入了本地的节点信息，会在系统配置界面进行同步更新。



![btc-node-info-local_cn](assets/CN/client/btc-node-info-local_cn.png)



### 重置锁定密码

如果需要重置Mining-Bot本地存储的信息，可以进行重置操作，这将清空我们存储的锁定密码和账户信息。

![reset-account-info_cn](assets/CN/systemconfig/reset-account-info_cn.png)



点击确认后，会回到界面：

![index-set-lock](assets/CN/index/index-set-lock.png)


## 5.挖矿实际操作指南

在开启挖矿机器人后，通过访问`http://localhost:8000/`登录后，首先需要清除浏览器缓存。在本教程中，将逐步讲解主网和测试网Xenon上的操作，其中主网上会采用自己的比特币节点，而Xenon上则使用节点池。具体细节会在下面两个小节分别展开。


### 5.1 主网挖矿步骤

#### 5.1.1 清除浏览器缓存

在主页面右键`检查`，点击`Application->Local Storage->http://localhost:8000/`，右键清除当前网页缓存。

![clear-cache](assets/CN/client/clear-cache-2.1.0.png)

![clear-cache](assets/CN/publicdata/clear-cache-2.1.0.png)

完成后锁定账户并重新输入密码登录。

#### 5.1.2 添加主网钱包地址

切换网络为`Mainnet`，在钱包页面点击添加地址：

![import-address](assets/CN/wallet/import-address-2.1.0.png)

这里导入的比特币地址需要存入一定的比特币，否则无法执行挖矿步骤。

### 5.1.3 系统配置与链信息查询

在初始时未正确配置节点信息，无法获取到链相关的实际信息，因此需要先到系统配置界面将节点信息等配置完成。

![chain-info](assets/CN/client/chain-info-null-2.1.0.png)

需要配置字段有：

- Mining-Monitor Url
- BTC Node Peer Host
- BTC Node Username
- BTC Node Password
- BTC Node RPC Port
- BTC Node Peer Port

如下为配置示例：

![sys-conf](assets/CN/systemconfig/sysconf-2.1.0-red.png)

其中圈出的字段需要用户自己提供。

**注意：地灵科技并不提供主网比特币节点**。

保存完成后，回到Mining Client界面，刷新可以看到链的信息更新。

![chain-info](assets/CN/client/chain-info-2.1.0.png)

#### 5.1.4 下载stacks-node

在Mining Client界面点击下载stacks-node:

![stacks-node](assets/CN/client/download-stacks-node-2.1.0.png)

完成后进入下一步挖矿参数配置操作。

#### 5.1.5 配置挖矿参数

![start-mining](assets/CN/client/start-mining-2.1.0.png)


点击开始挖矿按钮，弹出参数配置页面，选择已经导入的账户，调节燃烧费率，并选择节点信息，在主网这里，采用的是系统配置时的本地节点信息。

<img src="assets/CN/client/config-gas-2.1.0.png" alt="account-gas" style="zoom:33%;align=right"/>

<img src="assets/CN/client/gas-satoshi-2.1.0.png" alt="burn-fee" style="zoom:33%;align=center;" align=center />

其中比特币节点信息需要用户自己提供。

<img src="assets/CN/client/btc-node-2.1.0-red.png" alt="btc-node" style="zoom:33%;" />


#### 5.1.6 开始挖矿

配置完成后，输入`node1234`，这是在`yarn start`时配置的认证密码，然后即可开启挖矿程序。 

<img src="assets/CN/client/auth-code-2.1.0.png" alt="auth-code" style="zoom: 50%;" />

<img src="assets/CN/client/mining-status-2.1.0.png#pic_center" alt="mining-status" style="zoom:50%;" />


在命令行查看主网节点信息同步：

![sync-block](assets/CN/client/cli-mainnet-2.1.0.png)

同步完成后即开始挖矿。

### 5.2 Xenon测试网挖矿步骤

测试网上采用节点池的配置进行挖矿操作。

#### 5.2.1 切换至Xenon测试网

![xenon](assets/CN/publicdata/xenon-2.1.0.png)


#### 5.2.2 导入测试网钱包账号

![import-address](assets/CN/wallet/import-address-2.1.0.png)

输入助记词即可。

#### 5.2.3 下载stacks-node

在Mining Client界面点击下载stacks-node:

![stacks-node](assets/CN/client/xenon-download-stacks-node-2.1.0.png)


#### 5.2.4 在节点池导入钱包地址

访问：`http://8.210.73.117:8000`，


![node-pool-info](assets/CN/client/xenon-BTC-node-pool-2.1.0.jpeg)


![import-address-in-node-pool](assets/CN/client/node-pool-import-address-2.1.0.png)

完成后回到挖矿界面，点击挖矿配置挖矿参数。

#### 5.2.5 配置挖矿参数

![start-mining](assets/CN/client/start-mining-2.1.0.png)

点击开始挖矿按钮，弹出参数配置页面，选择已经导入的账户，调节燃烧费率，并选择节点信息，在Xenon测试网这里，下拉选择节点池提供的节点信息。

<img src="assets/CN/client/account-select-2.1.0.png" alt="account-select" style="zoom:33%;" />
<img src="assets/CN/client/xenon-burn-fee-2.1.0.png" alt="burn-fee" style="zoom:33%;" />
<img src="assets/CN/client/xenon-node-pool-2.1.0.png" alt="btc-node-pool" style="zoom:33%;" />

#### 5.2.6 开始挖矿

配置完成后，输入`node1234`，这是在`yarn start`时配置的认证密码，然后即可开启挖矿程序。 

<img src="assets/CN/client/auth-code-2.1.0.png" alt="auth-code" style="zoom:50%;" />

<img src="assets/CN/client/xenon-mining-status-2.1.0.png" alt="mining-status" style="zoom:50%;" />

在命令行查看输出：

![sync-block](assets/CN/client/cli-xenon-2.1.0.png)

输出中可以看到`UTXOs found`，表示挖矿节点启动成功，开始挖矿。





