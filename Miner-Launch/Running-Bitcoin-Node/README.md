---
sort: 1
---

# Setup Bitcoin Node

**【Tips】 In this tutorial, I will use Ubuntu 18.04 LTS as env of cloud server.**

```bash
#get bitcoin release
wget https://bitcoin.org/bin/bitcoin-core-0.21.1/bitcoin-0.21.1-x86_64-linux-gnu.tar.gz
# tar
tar xf bitcoin-0.21.1-x86_64-linux-gnu.tar.gz
# create data dir
mkdir /bitcoin_data
# move bitcoin binary to PATH
cd bitcoin-0.21.1
cd bin
mv * /usr/local/bin
```
try bitcoin + TAP, you will see
```bash
bitcoin-cli  bitcoind  bitcoin-qt  bitcoin-tx  bitcoin-wallet
```
Create Config file

```bash
#create your own bitcoin config file
vim /bitcoin-config-mainnet.conf

#use the dir you create above, use absolute path
datadir=/bitcoin_data/mainnet
server=1
rpcuser=<SET-YOUR-OWN-USER-NAME>
rpcpassword=<SET-YOUR-OWN-USER-PASSWORD>
txindex=1
listen=1
rpcserialversion=0
maxorphantx=1
banscore=1
daemon=1
bind=0.0.0.0:8333
rpcbind=0.0.0.0:8332
rpcport=8332
```

Start Bitcoin Node
```bash
bitcoind -conf=/root/bitcoin-config-mainnet.conf
```

If you want to stop(not shut down) Bitcoin Node
```bash
bitcoin-cli stop
```







