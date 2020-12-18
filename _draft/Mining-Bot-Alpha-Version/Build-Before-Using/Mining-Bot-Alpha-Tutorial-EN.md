---
sort: 1
---

# Building Mining-Bot Before Use

Learn how to set up and run mining bot. 

**【Tips】If you are not the first time to start mining-bot, please see [Restart Mining-Bot](#Restart Mining-Bot)**.

## Introduction

This tutorial will walk you through the following steps:

- Download and install Rust and Nodejs
- Install the stacks-node
- Running Mining-Local-Server
- Running Mining-Bot

:artificial_satellite:**【Tips】If your system is `Windows`, we recommend that you can use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to install Mining-Bot.**

## Requirements

### Rust

The tutorial of installing Rust comes from [blockstack official document](https://docs.blockstack.org/understand-stacks/running-testnet-node). You can see it for more information.

If you use Linux, you may need to manually install [`libssl-dev`](https://wiki.openssl.org/index.php/Libssl_API) and other packages. In your command line, run the following to get all packages:

```shell
sudo apt-get install build-essential cmake libssl-dev pkg-config
```

Ensure that you have Rust installed. If you are using macOS, Linux, or another Unix-like OS, run the following. If you are on a different OS, follow the [official Rust installation guide](https://www.rust-lang.org/tools/install).

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

In case you just installed Rust, you will be prompted to run the following command to make the `cargo` command available:

```shell
source $HOME/.cargo/env
```

Next, to check if Rust is installed correctly:

```shell
# use the command
rustc -V
# here is the output
rustc 1.47.0 (18bf6b4f0 2020-10-07)
```

### Nodejs

We recommend that you use `nvm` to control the version of Nodejs and install node. The tutorial of installing `nvm` comes from [official document](https://github.com/nvm-sh/nvm). You can see it for more information.

To **install** or **update** `nvm`, you should run the [install script](https://github.com/nvm-sh/nvm/blob/v0.37.0/install.sh). To do that, you may either download and run the script manually, or use the following cURL or Wget command:

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```

```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```

Run the following command to make the `nvm` command available:

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Then you should use the following command to see if you have installed `nvm`:

```shell
nvm
```

If the installation is successful, the output similar to the following will be displayed:

```shell
Node Version Manager

Note: <version> refers to any version-like string nvm understands. This includes:
  - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
  - default (built-in) aliases: node, stable, unstable, iojs, system
  - custom aliases you define with `nvm alias foo`

 Any options that produce colorized output should respect the `--no-colors` option.

Usage:
  nvm --help                                Show this message
  nvm --version                             Print out the installed version of nvm
  nvm install [-s] <version>                Download and install a <version>, [-s] from source. Uses .nvmrc if available
    --reinstall-packages-from=<version>     When installing, reinstall packages installed in <node|iojs|node version number>
    --lts                                   When installing, only select from LTS (long-term support) versions
    --lts=<LTS name>                        When installing, only select from versions for a specific LTS line
    --skip-default-packages                 When installing, skip the default-packages file if it exists
    --latest-npm                            After installing, attempt to upgrade to the latest working npm on the given node version
    --no-progress                           Disable the progress bar on any downloads
  nvm uninstall <version>                   Uninstall a version
  nvm uninstall --lts                       Uninstall using automatic LTS (long-term support) alias `lts/*`, if available.
  nvm uninstall --lts=<LTS name>            Uninstall using automatic alias for provided LTS line, if available.
  nvm use [--silent] <version>              Modify PATH to use <version>. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm exec [--silent] <version> [<command>] Run <command> on <version>. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm run [--silent] <version> [<args>]     Run `node` on <version> with <args> as arguments. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm current                               Display currently activated version of Node
  nvm ls                                    List installed versions
  nvm ls <version>                          List versions matching a given <version>
  nvm ls-remote                             List remote versions available for install
    --lts                                   When listing, only show LTS (long-term support) versions
  nvm ls-remote <version>                   List remote versions available for install, matching a given <version>
    --lts                                   When listing, only show LTS (long-term support) versions
    --lts=<LTS name>                        When listing, only show versions for a specific LTS line
  nvm version <version>                     Resolve the given description to a single local version
  nvm version-remote <version>              Resolve the given description to a single remote version
    --lts                                   When listing, only select from LTS (long-term support) versions
    --lts=<LTS name>                        When listing, only select from versions for a specific LTS line
  nvm deactivate                            Undo effects of `nvm` on current shell
  nvm alias [<pattern>]                     Show all aliases beginning with <pattern>
  nvm alias <name> <version>                Set an alias named <name> pointing to <version>
  nvm unalias <name>                        Deletes the alias named <name>
  nvm install-latest-npm                    Attempt to upgrade to the latest working `npm` on the current node version
  nvm reinstall-packages <version>          Reinstall global `npm` packages contained in <version> to current version
  nvm unload                                Unload `nvm` from shell
  nvm which [current | <version>]           Display path to installed node version. Uses .nvmrc if available
  nvm cache dir                             Display path to the cache directory for nvm
  nvm cache clear                           Empty cache directory for nvm

Example:
  nvm install 8.0.0                     Install a specific version number
  nvm use 8.0                           Use the latest available 8.0.x release
  nvm run 6.10.3 app.js                 Run app.js using node 6.10.3
  nvm exec 4.8.3 node app.js            Run `node app.js` with the PATH pointing to node 4.8.3
  nvm alias default 8.1.0               Set default node version on a shell
  nvm alias default node                Always default to the latest available node version on a shell

Note:
  to remove, delete, or uninstall nvm - just remove the `$NVM_DIR` folder (usually `~/.nvm`)
```

Then you can use the following command to install Nodejs:

```shell
nvm install 14.15.0
```

Then use the following commands to see if node and npm are installed correctly:

```shell
# use these two commands
node -v
# here is the version output
v14.15.0
npm -v
# here is the version output
6.14.8
```

Next, use `npm` to install `yarn`:

```shell
npm install -g yarn
```

To check if `yarn` is installed correctly:

```shell
# use the command
yarn -v
# here is the version output
1.22.4
```

## Step 1: Installing the stacks-node

The tutorial of installing stacks-node based on [blockstack official document](https://docs.blockstack.org/stacks-blockchain/running-testnet-node). You can see it for more information.

First, clone this repository:

```shell
git clone https://github.com/blockstack/stacks-blockchain.git
cd stacks-blockchain
```

Install the Stacks node by running:

```shell
cargo build --workspace --release --bin stacks-node
# binary will be in target/release/stacks-node
```

:warning:**This process will take a few minutes to complete.**

Then copy the binary file `target/release/stacks-node` to `$HOME/.cargo/bin`:

```shell
cp target/release/stacks-node $HOME/.cargo/bin
```

To check if the `stacks-node` is global command right now:

```shell
# use the command
stacks-node help
```

Here is the output:

```shell
stacks-node <SUBCOMMAND>
Run a stacks-node.

USAGE:
stacks-node <SUBCOMMAND>

SUBCOMMANDS:

mocknet         Start a node based on a fast local setup emulating a burnchain. Ideal for smart contract development.

helium          Start a node based on a local setup relying on a local instance of bitcoind.
                The following bitcoin.conf is expected:
                  chain=regtest
                  disablewallet=0
                  txindex=1
                  server=1
                  rpcuser=helium
                  rpcpassword=helium

argon           Start a node that will join and stream blocks from the public argon testnet, powered by Blockstack (Proof of Burn).

krypton         Start a node that will join and stream blocks from the public krypton testnet, powered by Blockstack via (Proof of Transfer).

xenon           Start a node that will join and stream blocks from the public xenon testnet, decentralized.

start           Start a node with a config of your own. Can be used for joining a network, starting new chain, etc.
                Arguments:
                  --config: path of the config (such as https://github.com/blockstack/stacks-blockchain/blob/master/testnet/Stacks.toml).
                Example:
                  stacks-node start --config=/path/to/config.toml

version         Display informations about the current version and our release cycle.

help            Display this help.
```

## Step 2: Running Mining-Local-Server

First, open a new terminal and clone the repository:

```shell
git clone https://github.com/Daemon-Technologies/Mining-Local-Server.git
cd Mining-Local-Server
```

Install the dependencies:

```shell
npm install
```

Then running Mining-Local-Server:

```shell
npm start
```

If you see the output like the following, that means you start Mining-Local-Server successfully:

```shell
> miningbot-server@1.0.0 start D:\Projects\Blockstack\Mining-Local-Server
> node server.js

(node:4312) ExperimentalWarning: The ESM module loader is experimental.
Example app listening at http://localhost:5000
```

See more information on [Mining-Local-Server](https://github.com/Daemon-Technologies/Mining-Local-Server).

## Step 3: Running Mining-Bot

First, open a new terminal and clone the repository:

```shell
git clone https://github.com/Daemon-Technologies/Mining-Bot.git
cd Mining-Bot
```

Install the dependencies:

```shell
yarn install
```

:warning:**This process will take a few minutes to complete.**

Then running Mining-Bot:

```shell
npm start
```

If you see the output like the following, that means you start Mining-Bot successfully:

```shell
> ant-design-pro@5.0.0-alpha.0 start D:\Projects\Blockstack\Mining-Bot
> umi dev

� Starting Umi UI using umi@3.2.27...
� Umi UI mini Ready on port 3000.
Starting the development server...

√ Webpack
  Compiled successfully in 1.39m

 DONE  Compiled successfully in 83338ms                                                                9:41:13 ├F10: PM┤


  App running at:
  - Local:   http://localhost:8000 (copied to clipboard)
  - Network: http://172.19.112.1:8000
```

Then you can open http://localhost:8000 and you will see the page:

![image-20201112214323389](assets/Homepage.png)

:artificial_satellite:**【Tips】If your system is [Windows WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10), You need to use Network ip to open the page, such as below which is http://172.30.240.213:8000**

![wsl](assets/wsl_mining_ip.png)

Congratulations! Now you can start your mining journey.

## Restart Mining-Bot

If you have successfully run Mining-Bot and have stopped all related programs. Now we will teach you how to run Mining-Bot again. If you are the Windows users, please use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Make sure you have already stopped the programs(you can use `ctrl + C` to stop programs).

**【Tips】Make sure you have successfully completed the above tutorial and stopped all programs mentioned above.**

### Running Mining-Local-Server

Open a new terminal and get into `Mining-Local-Server` directory:

```shell
cd Mining-Local-Server
```

Start Mining-Local-Server:

```shell
npm start
```

If you see the output like the following, that means you start Mining-Local-Server successfully:

```shell
> miningbot-server@1.0.0 start /home/sher/stacks-mining/Mining-Local-Server
> node server.js

Example app listening at http://localhost:5000
```

### Running Mining-Bot

Open a new terminal and get into `Mining-Bot` directory:

```shell
cd Mining-Bot
```

Start Mining-Bot:

```shell
npm start
```

If you see the output like the following, that means you start Mining-Bot successfully:

```shell
> ant-design-pro@5.0.0-alpha.0 start /home/sher/stacks-mining/Mining-Bot
> umi dev

🚀 Starting Umi UI using umi@3.2.27...
🌈 Umi UI mini Ready on port 3000.
Starting the development server...

✔ Webpack
  Compiled successfully in 27.20s

 DONE  Compiled successfully in 27199ms                                                                       5:28:01 PM


  App running at:
  - Local:   http://localhost:8000 (copied to clipboard)
  - Network: http://172.31.214.44:8000
```

Then you can open http://localhost:8000 and you will see the page:

![image-20201112214323389](assets/Homepage.png)

:artificial_satellite:**【Tips】If your system is [Windows WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10), You need to use Network ip to open the page, such as below which is http://172.30.240.213:8000**

![wsl](assets/wsl_mining_ip.png)
