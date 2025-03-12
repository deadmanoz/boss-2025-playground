# RPC Scavenger Hunt

This was a fairly straightforward challenge that involved familiarising with the Bitcoin RPC and using `bitcoin-cli`.

I think my main takeaway from doing this challenge was to RTFM 📖 properly. Many of the RPC commands have optional arguments that can be used to return more data that can, for example, reduce the number of RPC calls you need to make to get the information you need.

I also gained a new appreciation for the `jq` JSON parsing tool. I had used it in a simple manner in a professional context, but I didn't realise how powerful it was. The chained selection & filtering is pretty, pretty good.

## Learning - don't take shortcuts 🤦

I was originally using a `bitcoin-core` installation on an existing "node-in-a-box" server to do this work as it allowed me to get stuck in quicker. However, I encountered an issue whereupon I lost all my code on a reboot of the system midway through the challenge. I realised that I needed to setup `bitcoin-core` on my development machine with the full blockchain and work from that going forward. I've lost count of the number of times I've been burnt by not taking the time and instead looking for shortcuts!
