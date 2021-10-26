# Intro

To use this, you must know how to install and run docker & docker-compose, which is out of the scope of this tool.

# How to

This document help you to run a lighweight tezos-node (in rolling mode). It will allow you to send transactions to the blockchain, which can help to collect NFTs when public nodes are crawled. It will not help you to become a baker, or query past blockchain data, it's only just a node to send transactions.

1. clone the repository and go into the directory where you stored your clone

2. create a docker volume to store blockchain data, and allow this volume to survive pruning
```
docker volume create --name tezos-data
```

3. download the last **rolling snapshot** that you can find on https://mainnet.xtz-shots.io/ and store it in the same directory as your docker-compose.yml file, under the name **snapshot**

4. import the snapshot by running (if you get any error, you will have to delete and recreate the docker volume)
```
docker-compose run --rm import
```

5. grab a coffee, buy a few NFTs, look at Netflix while waiting for the import to complete, it can take up to a few hours depending on your hardware specs.

6. once import is done, just start your tezos node by running
```
docker-compose up -d tezos
```

7. verify in the logs that everything run fine
```
docker-compose logs -f tezos
```

On first start, you should see the container generating an identity (can take a few minutes), and then starting to catch up the missing block since the snapshot you imported. It can take a few minutes or a few hours depending on the hardware you run it. Once the node is fully synced, you can start to use it with Temple Wallet to make your transactions. The catch up game is done when you see something like this in the logs
```
Oct 26 00:55:31.832 - validator.chain: Chain is bootstrapped
Oct 26 00:55:31.832 - validator.chain: Sync_status: sync
```

You can also check the status of the sync process with the following command
```
docker exec -it CONTAINER_NAME_OR_ID tezos-client bootstrapped
```
If the sync is done, you'll see a message saying `Node is bootstrapped.`


Last, keep in mind that if you run this on your own personal computer, each time you stop your computer, when you relaunch the node, you will have to wait for it to catch up with all blockchains transactions that happened while the node was offline, it can take a few minutes or a few hours depending of how long your node was offline.

# How to use my new tezos node in Temple Wallet ?

In Temple Wallet, you need to add a new node by following [MadFish explaination](https://madfish.crunch.help/temple-wallet/how-to-add-a-custom-rpc-to-the-temple-wallet)

* If you installed your node on your computer, just switch to the predefined localhost node in Temple Wallet
* If you installed your node on a VPS/server, for RPC URL, use http://server.ip:8732

For Lambda view, use: KT1CPuTzwC7h7uLXd5WQmpMFso1HxrLBUtpE

# Need help ?

I run this without any problem, on a Linux VPS (2CPU, 4Gb of RAM, 40Gb of SSD). While writing this doc, I also tried every step on a Mac M1 and it worked without any glitch.

Still, it doesn't mean that it's a bullet proof solution. So just remember that you are alone ... If you don't know how to run this, if anything fails, if you don't understand something, then Google is your best friend, not me ... If you are not able to help yourself, don't DM me, don't open a ticket, don't do anything : forget that all this exist, and either use a public node, or pay for a private node hosting service ;-)

If you are still here, and if you want to contribute to make all this better, then feel free to submit PR, I will gladly review it.

# How to help tezos blockchain ?

The rolling mode is the bare minimum mode to be able to send transactions but it doesn't help the eco-system, it's really only for your personal use. If you have more disk space, you can run tezos-node in full mode, which help the eco-system, and allow you to bake, query past transactions and help other node to bootstrap. Just adapt the docker-compose file to remove the rolling mode (full mode is the default run mode of a tezos node), and import a full snapshot instead of a rolling snapshot when you creator your node at step 3.