# FAQ

This page is an excerpt of answers to frequently asked questions collected from multiple sources.

If you want to participate in the discussion and make your suggestions, you can leave a message or submit an issue on the following two platforms.

- [Stacks Forum](https://forum.stacks.org/t/request-for-testing-alpha-mining-bot/11372)
- [Github](https://github.com/Daemon-Technologies)


## How do I generate a keychain?

Generating a keychain requires [Node.js](https://nodejs.dev/) as a prerequisite, and afterward there are two packages that help create the keychain. There is also a [video outlining the process](https://youtu.be/82b8PGoQYpI) on Windows.

Generate keychain command from the [Stacks documentation](https://docs.blockstack.org/start-mining#running-a-miner):

```
npx @stacks/cli make_keychain -t > keychain.json
```

Generate keychain using [stacks-gen](https://github.com/psq/stacks-gen) from Pascal:

```
npx -q stacks-gen sk --testnet > keychain.json
```

