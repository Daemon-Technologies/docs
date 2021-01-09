# 常见问题回答

本页摘录了从多方渠道收集的常见问题的回答。

如果您也想参与讨论，提出您的建议，可以在下述两个平台进行留言或者提交issue。

- [Stacks Forum](https://forum.stacks.org/t/request-for-testing-alpha-mining-bot/11372)
- [Github](https://github.com/Daemon-Technologies)


## 如何生成我的密钥（地址）信息？

生成密钥需要预先安装[Node.js](https://nodejs.dev/)环境，之后有两个包可以帮助来创建密钥。还有一个关于Windows系统的[概述过程的视频](https://youtu.be/82b8PGoQYpI)。通过[Stacks文档](https://docs.blockstack.org/start-mining#running-a-miner)中提供的命令来进行密钥创建：

```shell
npx @stacks/cli make_keychain -t > keychain.json
```

或使用Pascal开发的[stacks-gen](https://github.com/psq/stacks-gen)来创建密钥：

```shell
npx -q stacks-gen sk --testnet > keychain.json
```

