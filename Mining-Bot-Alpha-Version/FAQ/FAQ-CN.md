# 常见问题回答

本页摘录了从多方渠道收集的常见问题的回答。

如果您也想参与讨论，提出您的建议，可以在下述两个平台进行留言或者提交issue。

- [Stacks Forum](https://forum.stacks.org/t/request-for-testing-alpha-mining-bot/11372)
- [Github](https://github.com/Daemon-Technologies)

## 在哪里进行挖矿比赛的注册？

在[挖矿比赛注册](https://daemontechnologies.co/minestx-challenge)界面点击注册按钮进行注册，输入你的姓名、邮箱和地址信息（BTC和STX地址）。

## 如何生成我的密钥（地址）信息？

生成密钥需要预先安装[Node.js](https://nodejs.dev/)环境，之后有两个包可以帮助来创建密钥。还有一个关于Windows系统的[概述过程的视频](https://youtu.be/82b8PGoQYpI)。通过[Stacks文档](https://docs.blockstack.org/start-mining#running-a-miner)中提供的命令来进行密钥创建：

```shell
npx @stacks/cli make_keychain -t > keychain.json
```

或使用Pascal开发的[stacks-gen](https://github.com/psq/stacks-gen)来创建密钥：

```shell
npx -q stacks-gen sk --testnet > keychain.json
```

## 一台机器可以运行多个挖矿程序么？

不行，因为比赛仅需要一名矿工，并且同一个人不能因参与而获得两次奖励。 有关更多信息，请参阅[官方规则](https://daemontechnologies.co/stx-mining-rules)。

## 目前挖矿比赛是基于哪个测试网进行的？

当前挖矿比赛是基于`Krypton`测试网进行的。[Blockstacks官方文档](https://docs.blockstack.org/start-mining)给的教程是基于`Xenon`测试网的，如果你想了解关于`Krypton`测试网的安装部署请参照[Mining Bot Alpha版本教程](https://daemon-technologies.github.io/docs/Mining-Bot-Alpha-Version/)。

## 是否支持使用VPS来运行挖矿机器人？

不行，目前不支持VPS。

## 可以不使用Mining Bot来参与挖矿么？

当然可以。本质上, 挖矿是基于修改[配置文件](https://github.com/Daemon-Technologies/Mining-Local-Server/blob/master/conf/miner-Krypton.toml)（seed/burn_fee/...）后运行`stacks-node`来进行的。当你本地clone了[stacks-node](https://github.com/blockstack/stacks-blockchain)仓库后，你可以在文件夹`stacks-blockchain/testnet/stacks-node/conf`下按照以下的格式创建一个`.toml`文件（名字随意，如`miner-conf.toml`）

**【提醒】记得修改`.toml`文件中的seed和burn_fee，其中seed是你的私钥，burn_fee是你的燃烧比特币量。**

```toml
[node]
rpc_bind = "0.0.0.0:20443"
p2p_bind = "0.0.0.0:20444"
bootstrap_node = "048dd4f26101715853533dee005f0915375854fd5be73405f679c1917a5d4d16aaaf3c4c0d7a9c132a36b8c5fe1287f07dad8c910174d789eb24bdfb5ae26f5f27@krypton.blockstack.org:20444"
miner = true
seed = "Your private key"

[burnchain]
chain = "bitcoin"
mode = "krypton"
peer_host = "bitcoind.krypton.blockstack.org"
rpc_port = 18443
peer_port = 18444
burn_fee_cap = 96400
burn_fee = [Your burn fee]

[[mstx_balance]]
address = "STB44HYPYAT2BB2QE513NSP81HTMYWBJP02HPGK6"
amount = 10000000000000000

[[mstx_balance]]
address = "ST11NJTTKGVT6D1HY4NJRVQWMQM7TVAR091EJ8P2Y"
amount = 10000000000000000

[[mstx_balance]]
address = "ST1HB1T8WRNBYB0Y3T7WXZS38NKKPTBR3EG9EPJKR"
amount = 10000000000000000

[[mstx_balance]]
address = "STRYYQQ9M8KAF4NS7WNZQYY59X93XEKR31JP64CP"
amount = 10000000000000000
```

然后，进入`stacks-blockchain/testnet/stacks-node`目录：

```shell
# 使用以下命令确认自己目前的位置
pwd
# 确保自己在stacks-node目录下
# 输入大致如下
# ..../stacks-blockchain/testnet/stacks-node
```

启动挖矿（使用你自己的`.toml`文件）：

```shell
cargo run start --config=./conf/miner-conf.toml
```

如果你看到类似如下的输出，则代表挖矿程序已经成功启动：

```shell
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `/home/sher/stacks-mining/stacks-blockchain/target/debug/stacks-node start --config=./conf/miner-conf.toml`
==> ./conf/miner-conf.toml
INFO [1608017423.236] [testnet/stacks-node/src/run_loop/neon.rs:109] [ThreadId(1)] Miner node: checking UTXOs at address
: mhQcXvMokx2HRb4zKhe8qDR5SQEft48VMX
INFO [1608017436.220] [testnet/stacks-node/src/run_loop/neon.rs:116] [ThreadId(1)] Miner node: starting up, UTXOs found.
ERROR [1608017436.237] [src/chainstate/stacks/index/storage.rs:1550] [ThreadId(1)] Not found (no file is open)
INFO [1608017436.237] [src/chainstate/stacks/index/marf.rs:216] [ThreadId(1)] First-ever block 0f9188f13cb7b2c71f2a335e3
a4fc328bf5beb436012afca590b1a11466e2206
```

## Mining Bot目前是否支持Xenon测试网？

目前暂不支持`Xenon`测试网，但在不久的将来会支持`Xenon`测试网。