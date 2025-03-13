# ₿OSS 2025 POC
The repo relevant to this project is [Warnet Scenarios](https://github.com/deadmanoz/warnet-scenarios).

## Project selection
I decided on the **Warnet scenario for Replacement Cycling Attack** POC not necessarily because I knew beforehand anything about Replacement Cycling Attacks, but because it seemed to allow me to continue to cover and go deeper on a broad range of aspects of Bitcoin and Lightning (without necessarily committing to any particular rabbit-hole to specialise in yet):
- Bitcoin transaction replacement, CPFP and mempool rules
- Lightning protocol details, specifically the low-level details of HTLCs
- Bitcoin and Lightning security models
- Bitcoin and Lightning network simulation with Warnet
- Bitcoin Core functional test framework
- How to monitor the network for replacement cycling attacks

I also had a lot of fun during the [CTF Challenge](./5-ctf-challenge.md) and was kinda shocked how easy it was to take down/DOS Bitcoin Core nodes! So it seemed prudent to also get a handle on some of the issues with the Lightning network by looking at Replacement Cycling Attacks.

Perhaps the only downside of choosing this project is that it won't require me to use Rust (Warnet is Python), which is the language
I'm picking up during ₿OSS and want to become proficient in (so using it for everything would best).

## Replacement cycling attacks
Straight up, I have no pre-existing knowledge of the Replacement Cycling Attacks. But I do remember hearing about Antoine Riard halting his involvement with the Lightning network, probably on [Rabbit Hole Recap](https://rhr.tv); it turns out this was due to his concern over the Replacement Cycling Attacks:

> _“Effective now, I’m halting my involvement with the development of the Lightning Network and its implementations, including coordinating the handling of security issues at the protocol level... I think this new class of replacement cycling attacks puts Lightning in a very perilous position.”_

[Antoine's post to the Lightning-Dev mailing list](https://diyhpl.us/~bryan/irc/bitcoin/bitcoin-dev/linuxfoundation-pipermail/lightning-dev/2023-October/004154.txt)

It's also become apparent that Replacement Cycling Attacks are a class of attacks rather than a single attack. The original Replacement Cycling Attack was centred on stealing funds from Lightning nodes (see [Replacement Cycling Attacks on the Lightning Network (October 2023)](https://github.com/ariard/mempool-research/blob/2023-10-replacement-paper/replacement-cycling.pdf) by Antoine Riard). But they have since been discovered to also impact Bitcoin miner's block template, with the potential to impact the Bitcoin ecosystem more broadly (see [Replacement Cycling Attacks on Bitcoin Miners Block Templates (January 2025)](https://github.com/ariard/mempool-research/blob/2023-10-replacement-paper/rca-bmbt.pdf) also by Antoine Riard).

Anyway let's get stuck in and learn about them.

## Concept of replacement cycling
Bitcoin Optech have a good overview topic page on the concept of [Replacement Cycling](https://bitcoinops.org/en/topics/replacement-cycling/). Stealing the first sentence on this page:
> _"Replacement cycling is an attack against CPFP fee bumps and transactions using `SIGHASH_SINGLE` that allows an attacker to remove an unconfirmed transaction from the mempools of relaying full nodes without leaving an alternative transaction in its place."_

The phrase Replacement Cycling is used to describe the removal of an unconfirmed transaction from 'the mempool' (using this phrase as a general term) using fee-bumping techniques without leaving an alternative transaction in its place, and this process occurring repeatedly as the innocent party broadcasts their transaction only to have it removed again in this manner by an attacker.

## Problems encountered

### "Non-final" transactions
During implementation I encountered a `non-final` error when broadcasting a transaction as below. Looking into this, a transaction is considered "final" if its time-based constraints are satisfied given the current state of the blockchain. The error I had made was that I used in an incorrect block height, a value into the future, in the `nLockTime` field of the transaction and attempted to broadcast it.

```python
ReplacementCycling1 JSONRPC error
Traceback (most recent call last):
  File "/shared/archive.pyz/test_framework/test_framework.py", line 131, in main
    self.run_test()
  File "/shared/archive.pyz/replacement_cycling_1.py", line 588, in run_test
    bob_timeout_txid_2 = bob.sendrawtransaction(bob_timeout_tx_2.serialize().hex())
                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/shared/archive.pyz/test_framework/coverage.py", line 50, in __call__
    return_val = self.auth_service_proxy_instance.__call__(*args, **kwargs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/shared/archive.pyz/test_framework/authproxy.py", line 129, in __call__
    raise JSONRPCException(response['error'], status)
test_framework.authproxy.JSONRPCException: non-final (-26)
```
