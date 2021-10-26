---
sort: 1
---

# User Guide of Mining-Bot: Beta Version

This tutorial will introduce how to use mining bot for STX mining.

- [Previous Environment Setup Tutorial](../Build-Before-Using/Mining-Bot-Beta-Tutorial-EN.md)

:artificial_satellite:**[Reminder] Please refer to the video for specific instructions.**

- [Stacks Mining Bot Client version 2.1.0-Xenon Testnet](https://www.youtube.com/watch?v=_bWLbvDjj9g)
- [Stacks Mining Bot Client version 2.1.0-Mainnet](https://www.youtube.com/watch?v=8uk21KldZYI)

## 1. Login Page Introduction

When you input **http://localhost:8000/** in your browser, you will see the following interface:

![index-set-lock_en](assets/EN/index/index-set-lock_en.png)

When you log in for the first time, you will be prompted to enter the **Lock Password**. This password is mainly used for login authentication and private key encryption protection. The lock password here has nothing to do with the previous `yarn start node1234` authentication password, and there is no need to keep the same.

After entering the same password twice, you will enter the main page of the mining robot. After entering the main page, you can lock the account through the account status bar in the upper right corner of the figure below.

![index-lock-account_en](assets/EN/index/index-lock-account_en.png)



After clicking **Lock Account,** you will be redirected to the following interface, you need to re-enter the lock password set for the first time to **unlock** the account.



![unlock-account_en](assets/EN/index/unlock-account_en.png)

:artificial_satellite:**[Reminder] The password cannot be recovered.**

The main page consists of four parts: the **Public Data** page, the **Wallet** page, the **Mining Client** page, and the **System Configuration** page. Next, we will explain how to obtain mining data through mining bots and participate in mining.

As shown in the figure below, the public data page is designed to provide rich data sources for mining bot strategies, and the public data page is shown in the figure below. At this stage, the following information is included:

- Currency price information: STX, BTC trading pair information
- Chain information
- Block information

![public-data_en](assets/EN/index/public-data_en.png)

​    

## 2. Wallet page

### 2.1 Bitcoin and Stacks address generation online

:artificial_satellite:**[Reminder] If you have a BTC or STX address with 24 mnemonic words, you can choose to skip this section**

This section refers to the instructions for generating online addresses in the [Official Mining Doc](https://docs.blockstack.org/mining)

Run the following command:

``` bash
npx @stacks/cli make_keychain -t
```

After running the above command, you will see a lot of installation logs, at the end you can see a `JSON`, similar to:

```json
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
:artificial_satellite:**[Reminder] The above information must be saved as core information such as Bitcoin and Stacks private key**

### 2.2 Add account: Bitcoin and Stacks address import
Click the **Add Account** column on the wallet account page, and a dialog box for importing addresses will pop up. Copy and paste the **24 mnemonic words** into the dialog box (separated by spaces), and select the **corresponding type**. The address can be imported.

Click to add an account on the wallet account page:

![importAddress1_en](assets/EN/wallet/importAddress1_en.png)



Copy and paste the **24 mnemonic words** into the dialog box (separated by spaces), select the account type, and click submit:

![importAddress2_en](assets/EN/wallet/importAddress2_en.png)



You can see the newly added address in the list, as well as its corresponding type and account balance.

![importAddress3_en](assets/EN/wallet/importAddress3_en.png)



Get BTC testnet coins: https://testnet-faucet.mempool.co/

**Note**: Switch the network to the **Xenon** test network, the current test network is displayed in the upper right corner of the page.

![xenon-testnet_en](assets/EN/wallet/xenon-testnet_en.png)



## 3. Client interface

Enter the client page and see that the current state is Mining-Local Server is running, but the `stacks-node` program is not found.

![current_state_mining_en](assets/EN/client/current_state_mining_en.png)

 




## 4. System configuration interface

![current_state_mining_en](assets/EN/systemconfig/current_state_mining_en.png)



If the network is normal, the information of the main chain and the local chain is displayed. You can configure the information such as Mining-Local-Server, local Stacks chain, BTC node to be synchronized, etc.

When starting the mining configuration Bitcoin node, the local node information is filled in, and it will be updated synchronously in the system configuration interface：

![btc-node-info-local_cn](assets/CN/client/btc-node-info-local_cn.png)



### Reset Lock Password

If you need to reset the information stored locally in Mining-Bot, you can perform a reset operation, which will clear our stored lock password and account information.



![reset-account-info_en](assets/EN/systemconfig/reset-account-info_en.png)



After clicking confirm, it will return to the Set Your Lock Password interface:

![index-set-lock_en](assets/EN/index/index-set-lock_en.png)


## 5.Practical Mining Guide

After starting the mining bot, logging in by visiting `http://localhost:8000/`, first we need to clear the browser cache. In this tutorial, we will walk through the operations on the `Mainnet` and `Xenon` testnet. The `Mainnet` will use a local Bitcoin node, while `Xenon` will use the node pool. The specific details will be expanded separately in the following two subsections.


### 5.1 Steps for mining on Mainnet

#### 5.1.1 Clear browser cache



Right-click `Inspect` on the main page, go to `Application->Local Storage->http://localhost:8000/`, right-click to clear the current webpage cache.

![clear-cache](assets/CN/client/clear-cache-2.1.0.png)

![clear-cache](assets/CN/publicdata/clear-cache-2.1.0.png)

After completion, lock the account and re-enter the password to log in.

#### 5.1.2 Add Mainnet wallet address

Switch the network to `Mainnet`, click to add an address on the wallet page:

![import-address](assets/CN/wallet/import-address-2.1.0.png)


The bitcoin address imported here needs to be deposited in a certain amount of bitcoin, otherwise the following mining step cannot be performed.

### 5.1.3 System configuration and chain information query


The node information is not configured correctly at the beginning, and the actual information related to the chain cannot be obtained. Therefore, we need to first go to the system configuration interface to complete the configuration of the node information.


![chain-info](assets/CN/client/chain-info-null-2.1.0.png)


Fields to be configured:

- Mining-Monitor Url
- BTC Node Peer Host
- BTC Node Username
- BTC Node Password
- BTC Node RPC Port
- BTC Node Peer Port


The following is a configuration example:

![sys-conf](assets/CN/systemconfig/sysconf-2.1.0-red.png)


The circled fields need to be provided by users.

**Note: Daemon Technology does not provide mainnet bitcoin node**.

When saved, return to the Mining Client interface and refresh to see the chain information updated.

![chain-info](assets/CN/client/chain-info-2.1.0.png)

#### 5.1.4 Download stacks-node


Click to download `stacks-node` on the Mining Client interface:

![stacks-node](assets/CN/client/download-stacks-node-2.1.0.png)


When completed, proceed to the next step of mining parameter configuration operation.

#### 5.1.5 Configure mining parameters

![start-mining](assets/CN/client/start-mining-2.1.0.png)




Click the `Start Mining` button to pop up the parameter configuration page, select the account that has been imported, adjust the burning fee, and select the node information. In `Mainnet`, the local node information during system configuration is used.


<img src="assets/CN/client/config-gas-2.1.0.png" alt="account-gas" style="zoom:33%;"/>

<img src="assets/CN/client/gas-satoshi-2.1.0.png" alt="burn-fee" style="zoom:33%;" />


The Bitcoin node information needs to be provided by users.

<img src="assets/CN/client/btc-node-2.1.0-red.png" alt="btc-node" style="zoom:33%;" />


#### 5.1.6 Start mining


After the configuration is complete, enter `node1234`, which is the authentication password configured during `yarn start`, and then you can start the mining program.

<img src="assets/CN/client/auth-code-2.1.0.png" alt="auth-code" style="zoom: 50%;" />

<img src="assets/CN/client/mining-status-2.1.0.png#pic_center" alt="mining-status" style="zoom:50%;" />


View the synchronization of Mainnet block information on the command line:

![sync-block](assets/CN/client/cli-mainnet-2.1.0.png)


Mining will start after synchronization is complete.

### 5.2 Xenon testnet mining steps

The testnet uses the configuration of the node pool for mining operations.

#### 5.2.1 Switch to Xenon testnet

![xenon](assets/CN/publicdata/xenon-2.1.0.png)


#### 5.2.2 Import testnet wallet account

![import-address](assets/CN/wallet/import-address-2.1.0.png)

Input the `mnemonic`.

#### 5.2.3 Download stacks-node

Click to download `stacks-nod`e on the Mining Client interface:

![stacks-node](assets/CN/client/xenon-download-stacks-node-2.1.0.png)


#### 5.2.4 Import wallet address in node pool

Access：`http://8.210.73.117:8000`，


![node-pool-info](assets/CN/client/xenon-BTC-node-pool-2.1.0.jpeg)


![import-address-in-node-pool](assets/CN/client/node-pool-import-address-2.1.0.png)


When completed, return to the Mining Client interface and click Start Mining to configure mining parameters.

#### 5.2.5 Configure mining parameters

![start-mining](assets/CN/client/start-mining-2.1.0.png)


Click the Start Mining button to pop up the parameter configuration page.

Select the account that has been imported:

<img src="assets/CN/client/account-select-2.1.0.png" alt="account-select" style="zoom:33%;" />

Adjust the burning fee:

<img src="assets/CN/client/xenon-burn-fee-2.1.0.png" alt="burn-fee" style="zoom:33%;" />


And select the node information, in the Xenon testnet, drop down to select the node information provided by the node pool:

<img src="assets/CN/client/xenon-node-pool-2.1.0.png" alt="btc-node-pool" style="zoom:33%;" />

#### 5.2.6 Start mining


After the configuration is complete, enter `node1234`, which is the authentication password configured during `yarn start`, and then you can start the mining program.

<img src="assets/CN/client/auth-code-2.1.0.png" alt="auth-code" style="zoom:50%;" />

<img src="assets/CN/client/xenon-mining-status-2.1.0.png" alt="mining-status" style="zoom:50%;" />

View the output on the command line:

![sync-block](assets/CN/client/cli-xenon-2.1.0.png)


You can see `UTXOs found` in the output, indicating that the mining node has been successfully started and mining has started.