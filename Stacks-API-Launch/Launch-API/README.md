---
sort: 1

---

# How to launch a stacks-blockchain API instance

:artificial_satellite:【Tips】 The reference document is [running-api-from-source](https://github.com/blockstack/stacks-blockchain-api/blob/master/running_api_from_source.md). Thanks to [wileyj](https://github.com/wileyj) and [whoabuddy](https://github.com/blockstack/stacks-blockchain-api/commits?author=whoabuddy) for their contributions to the original document. This article is based on the above document to improve the database installation, background startup, Bitcoin node information and other details.  *

In this document, we'll go over how to run a [stacks-blockchain-api](https://github.com/blockstack/stacks-blockchain-api) instance.  
There are several components involved here to have a working setup, and we'll go over each.  
Please note that the following guide is targetted for Debian based systems - that in mind, most of the commands will work on other Unix systems with some small adjustments.

- [How to launch a stacks-blockchain API instance](#how-to-launch-a-stacks-blockchain-api-instance)
  - [Requirements](#requirements)
    - [Initial Setup](#initial-setup)
  - [Install Requirements](#install-requirements)
  - [Init Postgres](#init-postgres)
    - [postgres permissions](#postgres-permissions)
  - [stacks-blockchain-api](#stacks-blockchain-api)
    - [building stacks-blockchain-api](#building-stacks-blockchain-api)
    - [starting stacks-blockchain-api](#starting-stacks-blockchain-api)
    - [stopping stacks-blockchain-api](#stopping-stacks-blockchain-api)
  - [stacks-blockchain](#stacks-blockchain)
    - [stacks-blockchain binaries](#stacks-blockchain-binaries)
    - [starting stacks-blockchain](#starting-stacks-blockchain)
    - [stopping stacks-blockchain](#stopping-stacks-blockchain)
  - [Verify Everything is running correctly](#verify-everything-is-running-correctly)
    - [Postgres](#postgres)
    - [stacks-blockchain testing](#stacks-blockchain-testing)
    - [stacks-blockchain-api testing](#stacks-blockchain-api-testing)
    - [stacks-node folder tree](#stacks-node-folder-tree)

## Requirements

1. `bash` or some other Unix-like shell (i.e. `zsh`)
2. `sudo` or root level access to the system

PS: The following terminal interaction and output return are performed under Ubuntu 20.04 LTS

### Initial Setup

Since we'll need to create some files/dirs for persistent data,  
we'll first create a base directory structure and set some permissions:

```bash
$ sudo mkdir -p /stacks-node/{persistent-data/stacks-blockchain,bns,config,binaries}
$ sudo chown -R $(whoami) /stacks-node 
$ cd /stacks-node
```

## Install Requirements

**Install Libs:**

```bash
$ sudo apt-get install -y \
    gnupg2 \
    git \
    lsb-release \
    curl \
    jq \
    openjdk-11-jre-headless \
    build-essential \
    zip \
    screen \
```

**Install Postgres:**

```bash
$ sudo apt install postgresql postgresql-contrib
# Success. You can now start the database server using:
#    pg_ctlcluster 12 main start
$ sudo -i -u postgres
# postgres@iZj6c5ejo1w2ttwn79gprvZ:~$
$ exit
# logout
```

**Install Nodejs**

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
$ export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
$ nvm 
# Node Version Manager (v0.37.0)
# Usage: ......
$ nvm install 14
# Now using node v14.18.1 (npm v6.14.15)
# Creating default alias: default -> 14 (-> v14.18.1)
$ node -v
# v14.18.1
$ npm -v
# 6.*.*
```



**Optional but recommended** - If you want the V1 BNS data, there are going to be a few extra steps:

**Download the BNS data:  **
`curl -L https://storage.googleapis.com/blockstack-v1-migration-data/export-data.tar.gz -o /stacks-node/bns/export-data.tar.g.  z`

**Extract the data:  **
`tar -xzvf ./bns/export-data.tar.gz -C /stacks-node/bns/`

**Each file in `./bns` will have a corresponding `sha256` value.**

**To Verify, run a script like the following to check the sha256sum:**

```bash
for file in `ls /stacks-node/bns/* | grep -v sha256 | grep -v .tar.gz`; do
    if [ $(sha256sum $file | awk {'print $1'}) == $(cat ${file}.sha256 ) ]; then
        echo "sha256 Matched $file"
    else
        echo "sha256 Mismatch $file"
    fi
done
#sha256 Matched /stacks-node/bns/chainstate.txt
#sha256 Matched /stacks-node/bns/name_zonefiles.txt
#sha256 Matched /stacks-node/bns/subdomains.csv
#sha256 Matched /stacks-node/bns/subdomain_zonefiles.txt
```

## Init Postgres

### postgres permissions

We'll need to set a basic role, database to store data, and a password for the role.  
Clearly, this password is **insecure** so modify `password` to something stronger before creating the role.  

```bash
$ cat <<EOF> /tmp/file.sql
create role stacks login password 'password';
create database stacks_db;
grant all on database stacks_db to stacks;
EOF
$ sudo su - postgres -c "psql -f /tmp/file.sql" && rm -f /tmp/file.sql
# CREATE ROLE
# CREATE DATABASE
# GRANT
$ echo "local all stacks md5" | sudo tee -a /etc/postgresql/12/main/pg_hba.conf
# local all stacks md5
$ sudo systemctl restart postgresql
```

## stacks-blockchain-api

**Create new screen:**

```bash
$ screen
```

### building stacks-blockchain-api

```bash
$ git clone https://github.com/blockstack/stacks-blockchain-api /stacks-node/stacks-blockchain-api && cd /stacks-node/stacks-blockchain-api \
  && echo "GIT_TAG=$(git tag --points-at HEAD)" >> .env \
  && npm config set unsafe-perm true \
  && npm install \
  && npm run build \
  && npm prune --production
```

### starting stacks-blockchain-api

The stacks blockchain api requires several Environment Variables to be set in order to run properly.  
To reduce complexity, we're going to create a `.env` file that we'll use for these env vars.  

** Note: ** to enable BNS names, uncomment `BNS_IMPORT_DIR` in the below `.env` file. 

Create a new file: `/stacks-node/stacks-blockchain-api/.env` with the following content:

```bash
$ cat <<EOF> /stacks-node/stacks-blockchain-api/.env
NODE_ENV=production
GIT_TAG=master
PG_HOST=localhost
PG_PORT=5432
PG_USER=stacks
PG_PASSWORD=password
PG_DATABASE=stacks_db
STACKS_CHAIN_ID=0x00000001
V2_POX_MIN_AMOUNT_USTX=90000000260
STACKS_CORE_EVENT_PORT=3700
STACKS_CORE_EVENT_HOST=0.0.0.0
STACKS_BLOCKCHAIN_API_PORT=3999
STACKS_BLOCKCHAIN_API_HOST=0.0.0.0
STACKS_BLOCKCHAIN_API_DB=pg
STACKS_CORE_RPC_HOST=localhost
STACKS_CORE_RPC_PORT=20443
#BNS_IMPORT_DIR=/stacks-node/bns
EOF
$ cd /stacks-node/stacks-blockchain-api && node ./lib/index.js

```

**Exit screen:**

```
# CTRL+A+D exit screen
```

### stopping stacks-blockchain-api
*No need to stop stacks-blockchain-api unless there are special circumstances.*

```bash
$ ps -ef | grep "lib/index.js" | grep -v grep
user   17788   827 39 18:14 pts/0    00:07:55 node ./lib/index.js
$ sudo kill $(ps -ef | grep "lib/index.js" | grep -v grep | awk {'print $2'})
```

## stacks-blockchain

**Create new screen:**

```bash
$ screen
```

In order to have a **usable** API instance, it needs to have data from a running [stacks-blockchain](https://github.com/blockstack/stacks-blockchain) instance.

You will need to have the following in your `Config.toml` - this config block will send blockchain events to the API instance that was previously started:

```toml
[[events_observer]]
endpoint = "<fqdn>:3700"
retry_count = 255
events_keys = ["*"]
```

Here is an example `Config.toml` that you can use - create this file as `/stacks-node/config/Config.toml`:

```bash
$ cat <<EOF> /stacks-node/config/Config.toml
[node]
working_dir = "/stacks-node/persistent-data/stacks-blockchain"
rpc_bind = "0.0.0.0:20443"
p2p_bind = "0.0.0.0:20444"
bootstrap_node = "02da7a464ac770ae8337a343670778b93410f2f3fef6bea98dd1c3e9224459d36b@seed-0.mainnet.stacks.co:20444,02afeae522aab5f8c99a00ddf75fbcb4a641e052dd48836408d9cf437344b63516@seed-1.mainnet.stacks.co:20444,03652212ea76be0ed4cd83a25c06e57819993029a7b9999f7d63c36340b34a4e62@seed-2.mainnet.stacks.co:20444"
wait_time_for_microblocks = 10000

[[events_observer]]
endpoint = "localhost:3700"
retry_count = 255
events_keys = ["*"]

[burnchain]
chain = "bitcoin"
mode = "mainnet"
peer_host = "<ENTER-YOUR-OWN-BITCOIN-NODE-IP>"
username = "<ENTER-YOUR-OWN-BITCOIN-NODE-RPC-USERNAME>"
password = "<ENTER-YOUR-OWN-BITCOIN-NODE-RPC-PASSWORD>"
rpc_port = 8332
peer_port = 8333
EOF
```

### stacks-blockchain binaries

Download latest release binary from https://github.com/blockstack/stacks-blockchain/releases/latest

**Linux archive for [latest release](https://github.com/blockstack/stacks-blockchain/releases/latest): **

```bash
$ curl -L https://github.com/blockstack/stacks-blockchain/releases/download/$(curl --silent https://api.github.com/repos/blockstack/stacks-blockchain/releases/latest | jq .name -r | cut -f2 -d " ")/linux-x64.zip -o /tmp/linux-x64.zip
```

**Extract the zip archive:**

```bash
$ unzip /tmp/linux-x64.zip -d /stacks-node/binaries
```

### starting stacks-blockchain

```bash
$ cd /stacks-node && nohup /stacks-node/binaries/stacks-node start --config /stacks-node/config/Config.toml &
```

### stopping stacks-blockchain
*No need to stop stacks-blockchain unless there are special circumstances.*
```bash
$ ps -ef | grep "/stacks-node/binaries/stacks-node" | grep -v grep
user   17835 17834 99 18:17 pts/0    00:20:23 /stacks-node/binaries/stacks-node start --config /stacks-node/config/Config.toml
$ sudo kill $(ps -ef | grep "/stacks-node/binaries/stacks-node" | grep -v grep | awk {'print $2'})
```

## Verify Everything is running correctly

### Postgres

To verfiy the database is ready:

1. Connect to the DB instance:  `psql -h localhost -U stacks stacks_db`
   - use the password from the [Postgres Permissions Step](#postgres-permissions)
2. List current databases: `\l`
3. Disconnect from the DB : `\q`

### stacks-blockchain testing

```bash
$ curl localhost:20443/v2/info | jq
{
  "peer_version": 402653184,
  "pox_consensus": "e99b880a26405d3cda724f4c2b815ca0e7b681a8",
  "burn_block_height": 666201,
  "stable_pox_consensus": "8be2fc9f9156b31af688b9e3c484dc7a26cefc4f",
  "stable_burn_block_height": 666194,
  "server_version": "stacks-node 2.0.11.0.0 (master:bf4a577+, release build, linux [x86_64])",
  "network_id": 1,
  "parent_network_id": 3652501241,
  "stacks_tip_height": 92,
  "stacks_tip": "a251bfa158c7887c575798b79c8df57190690e023af245e45513110399c0cb5f",
  "stacks_tip_consensus_hash": "ae29e81af9e7febfd4af6b53a3e515660a84150c",
  "genesis_chainstate_hash": "74237aa39aa50a83de11a4f53e9d3bb7d43461d1de9873f402e5453ae60bc59b",
  "unanchored_tip": "9fb08dd696d4e05bd042998dba8dd204ee64e11266cd4fba95b3a7bdaa709400",
  "unanchored_seq": 0,
  "exit_at_block_height": null
}
```

### stacks-blockchain-api testing

```bash
$ curl localhost:3999/v2/info | jq
{
  "peer_version": 402653184,
  "pox_consensus": "e99b880a26405d3cda724f4c2b815ca0e7b681a8",
  "burn_block_height": 666201,
  "stable_pox_consensus": "8be2fc9f9156b31af688b9e3c484dc7a26cefc4f",
  "stable_burn_block_height": 666194,
  "server_version": "stacks-node 2.0.11.0.0 (master:bf4a577+, release build, linux [x86_64])",
  "network_id": 1,
  "parent_network_id": 3652501241,
  "stacks_tip_height": 93,
  "stacks_tip": "74223951b9dddfe82b1b116c852f0a14ff9432270d680042a5ef11cb0533b935",
  "stacks_tip_consensus_hash": "2d521daf879f2e00f745c47000b856e1d41b15d4",
  "genesis_chainstate_hash": "74237aa39aa50a83de11a4f53e9d3bb7d43461d1de9873f402e5453ae60bc59b",
  "unanchored_tip": "9fb08dd696d4e05bd042998dba8dd204ee64e11266cd4fba95b3a7bdaa709400",
  "unanchored_seq": 0,
  "exit_at_block_height": null
}
```

### stacks-node folder tree

```bash
root@/stacks-node $ pwd
# /stacks-node
root@/stacks-node $ tree -L 1
.
├── binaries
├── bns
├── config
├── persistent-data
└── stacks-blockchain-api
```

