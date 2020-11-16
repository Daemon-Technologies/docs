# 运行挖矿机器人（Mining-Bot）

本文将会将你如何配置并且运行挖矿机器人。

[英文版](README.md)

## 介绍

本教程会带你经历以下步骤：

- 下载并安装Rust与Nodejs
- 安装stacks-node
- 运行Mining-Local-Server
- 运行Mining-Bot

:artificial_satellite:**【提示】如果你的操作系统是`Windows`系统，我们推荐你使用[WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)来安装并运行挖矿机器人。**

## 环境安装与配置

### Rust

关于Rust部分的教程来自[blockstack官方文档](https://docs.blockstack.org/stacks-blockchain/running-testnet-node)，如果想获取更详细的信息请查阅链接中的文档。

如果你使用的是Linux系统，你可能需要手动安装 [`libssl-dev`](https://wiki.openssl.org/index.php/Libssl_API) 和其他依赖包。在命令行中，输入以下命令来安装：

```shell
$ sudo apt-get install build-essential cmake libssl-dev pkg-config
```

确保你的系统已经安装了Rust环境。如果你使用的是macOS, Linux或其他Unix相关的系统，运行以下命令。如果你的系统不是以上系统，请参照[Rust官方文档](https://www.rust-lang.org/tools/install)。

```shell
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

如果你刚安装了Rust，则会提示你运行以下命令以使`cargo`命令可用：

```shell
$ source $HOME/.cargo/env
```

然后，在命令行中输入以下命令检查Rust是否安装成功：

```shell
# 命令
$ rustc -V
# 版本输出如下，版本不同也不影响
rustc 1.47.0 (18bf6b4f0 2020-10-07)
```

### Nodejs

我们推荐你使用`nvm`来控制Nodejs的版本并进行nodejs的安装。本教程来自[nvm官方文档](https://github.com/nvm-sh/nvm)，如果想获取更详细的信息请查阅链接中的文档。

为了安装或更新`nvm`，你需要运行[安装脚本](https://github.com/nvm-sh/nvm/blob/v0.37.0/install.sh)。你需要下载或者直接手动运行脚本，或者可以通过以下命令来进行安装：

```shell
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```

```shell
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```

然后你可以使用以下命令来检验是否`nvm`安装成功：

```shell
$ nvm
```

如果安装成功，输出内容会跟下面类似：

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

然后你可以使用以下命令来安装Nodejs：

```shell
$ nvm install 14.15.0
```

然后使用以下命令检查`node`和`npm`是否安装成功：

```shell
# node命令
$ node -v
# node版本输出
v14.15.0
# npm命令
$ npm -v
# npm版本输出
6.14.8
```

然后我们使用`npm`来安装`yarn`：

```shell
$ npm install -g yarn
```

来检验`yarn`是否安装成功：

```shell
# 使用如下命令
$ yarn -v
# 版本输出
1.22.4
```

## 第一步：安装stacks-node

此步骤教程是基于[blockstack官方文档](https://docs.blockstack.org/stacks-blockchain/running-testnet-node)，如果想获取更详细的信息请查阅链接中的文档。

首先，克隆仓库：

```shell
$ git clone https://github.com/blockstack/stacks-blockchain.git
$ cd stacks-blockchain
```

通过以下命令安装stacks-node：

```shell
$ cargo build --workspace --release --bin stacks-node
# 二进制文件会在 target/release/stacks-node 中
```

:warning:**此过程会花费一定的时间来完成。**

然后通过以下命令将生成的二进制文件`target/release/stacks-node`复制到`$HOME/.cargo/bin`：

```shell
$ cp target/release/stacks-node $HOME/.cargo/bin
```

检验`stacks-node`是否已经是全局命令：

```shell
$ stacks-node help
```

输出类似如下:

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

## 第二步：运行Mining-Local-Server

首先，克隆仓库：

```shell
$ git clone https://github.com/Daemon-Technologies/Mining-Local-Server.git
$ cd Mining-Local-Server
```

安装依赖包：

```shell
$ npm install
```

运行Mining-Local-Server：

```shell
$ npm start
```

如果你看到类似如下输出则代表已成功启动：

```shell
> miningbot-server@1.0.0 start D:\Projects\Blockstack\Mining-Local-Server
> node server.js

(node:4312) ExperimentalWarning: The ESM module loader is experimental.
Example app listening at http://localhost:5000
```

如果想查阅更多信息，请查看[Mining-Local-Server](https://github.com/Daemon-Technologies/Mining-Local-Server)。

## 第三步：运行Mining-Bot

首先打开一个新命令窗口并克隆Mining-Bot仓库：

```shell
$ git clone https://github.com/Daemon-Technologies/Mining-Bot.git
$ cd Mining-Bot
```

安装依赖包，【注意】此处需要用`yarn`命令进行安装：

```shell
$ yarn install
```

:warning:**此过程会花费一定的时间来完成。**

运行Mining-Bot

```shell
$ npm start
```

如果你看到类似如下输出则代表Mining-Bot已成功启动：

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

然后你可以在浏览器打开http://localhost:8000 并会看到如下界面的话：

![image-20201112221844632](assets/Homepage-CN.png)

:artificial_satellite:**【提醒】如果你使用的系统是Windows并且使用了[Windows WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)，你需要使用输出中提示的Network地址来打开网站，比如下方的http://172.30.240.213:8000**

![wsl](assets/wsl_mining_ip.png)

恭喜你！你已经完成了Mining-Bot的启动，接下来可以开启你的挖矿之旅了。