# FAQ

This page is an excerpt of answers to frequently asked questions collected from multiple sources.

If you want to participate in the discussion and make your suggestions, you can leave a message or submit an issue on the following two platforms.

- [Stacks Forum](https://forum.stacks.org/t/request-for-testing-alpha-mining-bot/11372)
- [Github](https://github.com/Daemon-Technologies)

## Where do I sign up?

Use the register button on the [competition sign-up page](https://daemontechnologies.co/minestx-challenge), and enter your name, email address, and keychain information (BTC and STX addresses).

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

## Do I need to run more than one miner?

No, for the competition only running one miner is needed, and the same person cannot be rewarded twice for participation. For more information, see the [official rules](https://daemontechnologies.co/stx-mining-rules).

## Which testnet is the current mining competition based on?

The current mining competition is based on the `Krypton` testnet. The tutorial given at [Blockstacks official document](https://docs.blockstack.org/start-mining)  is based on the `Xenon` testnet. If you want to know about the installation and deployment of the `Krypton` testnet, please refer to [Mining Bot Alpha version tutorial](https://daemon-technologies.github.io/docs/Mining-Bot-Alpha-Version/).

## Can I participate in mining without using Mining Bot?

of course can. Essentially, mining is based on running stacks-node after modifying the [configuration file]()(seed/burn_fee/...). After you clone the [stacks-node](https://github.com/blockstack/stacks-blockchain), you can create a `.toml` file(maybe `miner-conf.toml`) like the following at `stacks-blockchain/testnet/stacks-node/conf`.

**【Tips】Remember to modify the seed and burn_fee in the `.toml` file, where seed is your private key and burn_fee is your burned bitcoin value.**

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

then, get into `stacks-blockchain/testnet/stacks-node`:

```shell
# use the command to check where you are
pwd
# make sure you are at stacks-node directory
# output looks like
# ..../stacks-blockchain/testnet/stacks-node
```

start mining(use your `.toml` file):

```shell
cargo run start --config=./conf/miner-conf.toml
```

If you see the output like the following which means you run the mining program successfully:

```shell
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `/home/sher/stacks-mining/stacks-blockchain/target/debug/stacks-node start --config=./conf/miner-conf.toml`
==> ./conf/miner-conf.toml
INFO [1608017423.236] [testnet/stacks-node/src/run_loop/neon.rs:109] [ThreadId(1)] Miner node: checking UTXOs at address
: mhQcXvMokx2HRb4zKhe8qDR5SQEft48VMX
ERROR [1608017423.691] [testnet/stacks-node/src/burnchains/bitcoin_regtest_controller.rs:444] [ThreadId(1)] Bitcoin RPC 
failure: error listing utxos Network("Bitcoin RPC: status(500) != success, Response { status: InternalServerError, heade
rs: Headers { headers: {HeaderName(\"date\"): [HeaderValue { inner: \"Tue, 15 Dec 2020 07:30:23 GMT\" }], HeaderName(\"c
ontent-length\"): [HeaderValue { inner: \"25\" }], HeaderName(\"content-type\"): [HeaderValue { inner: \"application/jso
n\" }]} }, version: None, sender: Some(Sender { .. }), receiver: Receiver { .. }, body: Body { reader: \"<hidden>\", len
gth: Some(25) }, local: TypeMap }")
INFO [1608017436.220] [testnet/stacks-node/src/run_loop/neon.rs:116] [ThreadId(1)] Miner node: starting up, UTXOs found.
ERROR [1608017436.237] [src/chainstate/stacks/index/storage.rs:1550] [ThreadId(1)] Not found (no file is open)
INFO [1608017436.237] [src/chainstate/stacks/index/marf.rs:216] [ThreadId(1)] First-ever block 0f9188f13cb7b2c71f2a335e3
a4fc328bf5beb436012afca590b1a11466e2206
```

## Does Mining Bot support the Xenon testnet?

It is not currently supported, but will be supported in the near future.

