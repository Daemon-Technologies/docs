---
sort: 1
---
# Setup Stacks Node

## Create Miner Key
**【Tips】 In this chapter, I will use the [official tutorial](https://docs.stacks.co/start-mining/mainnet) which using npx to generate stacks and bitcoin key pairs. This key pair is generated in localhost, you should record the mnemonic and key info carefully before close the command terminal.**

Refer to [official tutorial](https://docs.stacks.co/start-mining/mainnet): Running a miner

>First, a keychain needs to be generated. With this keychain, we'll purchase some BTC from a crytpocurrency exchange, and then use that BTC to start mining.To get a keychain, the simplest way is to use the `stacks-cli`. We'll use the `make_keychain` command.

```bash
npx @stacks/cli make_keychain 2>/dev/null | json_pp > keychain.txt
```

> After this runs, you should see some JSON in the new `keychain.txt` file that looks like this:

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

> **Don't lose this information** - we'll need to use the privateKey field later on.The above BTC address will then need to be **imported into the BTC network**.


## Import BTC Address

For querying miner's UTXOs in bitcoin node fastly, miner should import your own BTC address in the Bitcoin node you setup above.

```bash
bitcoin-cli -conf=/root/bitcoin-config-mainnet.conf importaddress <btcAddress from JSON above>
```


## Setup Stacks Node

**【Tips】 In this chapter, I will not use the [official tutorial](https://docs.stacks.co/understand-stacks/running-mainnet-node) which using docker, because from my experience Miner will change the config file frequently, launching docker is not a friendly option to me.**

```bash
# download official binary release
wget https://github.com/blockstack/stacks-blockchain/releases/download/2.0.11.2.0-rc2/linux-x64.zip
# unzip
unzip linux-x64.zip -d stacks-node
cd stacks-node
ls
```
You will see:
```bash
blockstack-cli  blockstack-core  clarity-cli  stacks-node
```
We will use stacks-node binary later.
Now creating config file
```bash
vim miner.toml

[node]
rpc_bind = "0.0.0.0:20443"
p2p_bind = "0.0.0.0:20444"
seed = "<YOUR-OWN-PRIVATE-KEY>"    # replace your own private key
local_peer_seed = "<YOUR-OWN-PRIVATE-KEY>"     # replace your own private key
miner = true                # should be true
bootstrap_node = "02da7a464ac770ae8337a343670778b93410f2f3fef6bea98dd1c3e9224459d36b@seed-0.mainnet.stacks.co:20444,02afeae522aab5f8c99a00ddf75fbcb4a641e052dd48836408d9cf437344b63516@seed-1.mainnet.stacks.co:20444,03652212ea76be0ed4cd83a25c06e57819993029a7b9999f7d63c36340b34a4e62@seed-2.mainnet.stacks.co:20444"
working_dir = "/root/stacks-node-data"   # dir you create above 


[burnchain]
chain = "bitcoin"
mode = "mainnet"
peer_host = "<YOUR-OWN-BITCOIN-NODE-IP>"   # your cloud server public IP  
rpc_port = 8332    # rpc port in your bitcoin config
peer_port = 8333   # peer port in your bitcoin config
username = "<YOUR-OWN-BITCOIN-RPC-USERNAME>"  # rpc_username in your bitcoin config
password = "<YOUR-OWN-BITCOIN-RPC-PASSWORD>"  # rpc_password in your bitcoin config
satoshis_per_byte = 150   # sats per bytes, it should be dynamicly changed by bitcoin mempool situation, for now 2021.7.29, 150 sats/bytes is big enough for miner commit transaction 
burn_fee_cap = 450000     # commit value(burn-fee), depends on your own strategy
```

The final miner.toml looks like:
```bash
[node]
rpc_bind = "0.0.0.0:20443"
p2p_bind = "0.0.0.0:20444"
seed = "<YOUR-OWN-PRIVATE-KEY>"   
local_peer_seed = "<YOUR-OWN-PRIVATE-KEY>"     
miner = true                
bootstrap_node = "02da7a464ac770ae8337a343670778b93410f2f3fef6bea98dd1c3e9224459d36b@seed-0.mainnet.stacks.co:20444,02afeae522aab5f8c99a00ddf75fbcb4a641e052dd48836408d9cf437344b63516@seed-1.mainnet.stacks.co:20444,03652212ea76be0ed4cd83a25c06e57819993029a7b9999f7d63c36340b34a4e62@seed-2.mainnet.stacks.co:20444"
working_dir = "/root/stacks-node-data"   


[burnchain]
chain = "bitcoin"
mode = "mainnet"
peer_host = "<YOUR-OWN-BITCOIN-NODE-IP>"   
rpc_port = 8332    
peer_port = 8333   
username = "<YOUR-OWN-BITCOIN-RPC-USERNAME>"  
password = "<YOUR-OWN-BITCOIN-RPC-PASSWORD>" 
satoshis_per_byte = 150   
burn_fee_cap = 450000      
```
**【Tips】 Value of `satoshis_per_byte` and `burn_fee_cap` you should get more infomation from stacks mining discord channel or mining-monitor.**

Final Step, start the node. For running stacks-node in deamon, I recommend using `screen`.

```bash
screen     # create new screen
./stacks-node start --config="./miner.toml"  # start stacks-node
```

You will see:
```bash
<YOUR-OWN-BTC-ADDRESS> UTXO found, start a miner node .......
```
That means your BTC address has balance and imported to your own btc node successfully. Syncing blocks info from bitcoin and stacks network will take at least 30 minutes. Then you can use `CTRL+A+D` quit the screen. And come back to your screen by:
```bash
screen -ls

There are screens on:
	5597.pts-0.iZj6c3n5wqfabuvi4en67jZ	(07/01/2021 05:17:01 PM)	(Detached)
```

Then use:
```bash
screen -dr 5597 # will go back to your own screen
```
**【Tips】 Here are some instructions for  `screen` linux package command. If you have more efficient way to run stacks-node in daemon, you can use your own method, don't hesitate to share your experience [here](https://github.com/Daemon-Technologies/docs/issues).**


Any step you meet problems, you can leave your questions in our github [repo](https://github.com/Daemon-Technologies/docs/issues).
