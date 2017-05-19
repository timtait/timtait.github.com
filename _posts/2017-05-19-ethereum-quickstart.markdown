---
layout: post
title:  "Ethereum quick start guide"
date:   2017-05-19 15:00:00 +1200
categories: ethereum
---

My OSX quickstart guide to getting a custom chain / testnet up and running.


&nbsp;

# Install geth

Update brew and install [geth - the official golang implementation of the Ethereum protocol][geth].

```
brew update && brew upgrade

brew tap ethereum/ethereum
brew install ethereum
```

&nbsp;

# Create a directory

```
mkdir ethertest
cd ethertest
```

`./ethertest` is where I keep my test setup. From here on out assume all commands are being run from this directory.

&nbsp;

# Create a test account

First thing you want is an account to hoard our hard earned coins into. Don't forget the password...

- Note the two flags:
  - -\-dev - pre-configured private network with several debugging flags
  - -\-datadir - This is the directory our blockchain and account keys will live

```
geth --dev --datadir ./datatest account new
```

That will spit out an address

```
Address: {e1afb9ccffe8bc04fe9a35c65c3a3cbff4ef1551}
```

This is your account address, if you have a look in `./datatest` you should find a `keystore` directory which contains the account key.

```
./datatest/keystore/UTC--2017-05-18T14-34-51.340427220Z--e1afb9ccffe8bc04fe9a35c65c3a3cbff4ef1551
```

This should now be set as the default account, when you fire up a miner this is where all of our ETH will mine to.

&nbsp;

# Genesis file

Now to get the blockchain setup. I had a bit of trouble here when I was first getting going, a lot of the info available online is bloated with so much shit that it's hard to follow.

So to make things easy, you want to set a very basic starting point for our blockchain. To do this you need a [genesis][genesis] file.

Create `genesis.json`
```
{
  "config": {
    "chainId": 0,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

There is quite a bit of info there, I might make another blog post to go over that in the future but for now, that's all you need.

&nbsp;

# Blockchain setup

Time to initialise our blockchain.

```
geth --datadir ./datatest init genesis.json
```

That should end up outputting `Successfully wrote genesis state`.

Now run our node in console mode
```
geth --datadir ./datatest --ipcpath ~/Library/Ethereum/geth.ipc console genesis.json
```

Note the `--ipcpath`. If you are using the [EthereumWallet][ethereumwallet] app it looks for the ipc file at `~/Library/Ethereum/geth.ipc`. So you **CANNOT** have your ipc file in any other location, it **MUST** be `~/Library/Ethereum/geth.ipc`.

Now you should be sitting at an interactive console running on your private testnet.

&nbsp;

# Mining

```
miner.start()
```

Fire up the wallet app, you should see `PRIVATE-NET` in the top right as it loads.

Watch the ETH roll in.

&nbsp;

# Console

The console is an easy place to explore. It has tabbed autocompletion so just start typing crap in and see where you end up. e.g.

```
> miner
{
  getHashrate: function(),
  setEtherbase: function(),
  setExtra: function(),
  setGasPrice: function(),
  start: function(),
  stop: function()
}
```

[geth]: https://github.com/ethereum/go-ethereum
[genesis]: https://github.com/ethereum/go-ethereum#defining-the-private-genesis-state
[ethereumwallet]: https://github.com/ethereum/mist/releases