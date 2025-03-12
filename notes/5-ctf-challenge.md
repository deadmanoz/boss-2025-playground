# Capture-The-Flag Challenge
This was sort of an "off-piste" group challenge, but it was a lot of fun to participate in! Despite a slow start, with the challenge started in the middle of the night for Australia/South East Asia timezones, our team Team Emerald managed to take down all target Bitcoin nodes within about 24 hours.

![Team Emerald](./images/team-emerald.png)

The following is my take on the challenge, with some notes on the Warnet and Kubernetes side of things. 

## About the challenge
The challenge utilised [Warnet](https://github.com/bitcoin-dev-project/warnet) and was a CTF (Capture-The-Flag) style challenge. Warnet is a tool that allows you to configure and launch a network of Bitcoin nodes connected to each other in some user-specified topology, and then run `scenarios` against them. In leveraging Kubernetes, it is possible to run small networks locally, or connect to large networks running on a remote cluster.

`Scenarios` are specifications of behaviour which are programmed using the Bitcoin Core functional [test_framework language](https://github.com/bitcoin/bitcoin/tree/master/test/functional). They can range from the simple, such as a single node mining a block every X seconds/minutes, to the complex, 10's or more nodes, crafting and sending specific low-level P2P messages, all the while nodes are transacting with each other and blocks are being mined. There are a few basic `scenarios` provided with Warnet to build upon.

The challenge was all about exploring the security of Bitcoin nodes and the Bitcoin network, with Warnet being the infrastructure to do so.

## Interacting with warnet
Despite lots of experience with Docker - in my previous life we used Docker/Docker Compose to deploy the dozen or so services that made up our product onto fixed (non-scalable) on-premise hardware - I have never had any reason to use Kubernetes before (despite engineers often pushing to make the switch for often arbitrary reasons).

So for my own benefit, and maybe those that follow, here's some of the basics of using Kubernetes and then how to use Warnet.

You'll need `kubectl` and `helm` as prerequisites to interact with the Warnet. It's also recommended to use a k8s cluster management tool like `k9s` to monitor and manage your cluster(s). Do this. `k9s` is an interactive terminal-based UI that will make understanding and managing the state of your cluster(s) a lot easier.
