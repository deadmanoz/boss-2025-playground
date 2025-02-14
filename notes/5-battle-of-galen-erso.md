# Battle of Galen Erso
This was sort of an "off-piste" group challenge, but it was a lot of fun to participate in! Despite a slow start, with the challenge started in the middle of the night for Australia/South East Asia timezones, our team Team Emerald managed to take down all target Bitcoin nodes within about 24 hours.

The following is my take on the challenge, with some notes on the warnet and Kubernetes side of things. Be sure to also checkout some material written by other participants in the challenge:
- [jgmontoya's post about exploiting the CAddrMan vulnerability ](https://blog.jgmontoya.com/2025/02/04/CAddrMan-Vulnerability.html)
- [Bronson's post about how he took down other teams nodes before his own teams 😁](https://blog.bakungabronson.com/the-battle-of-galen-erso-recap)

![Team Emerald](./images/team-emerald.png)

## About the challenge
Details of the challenge are openly available on the [Warnet: The Battle of Galen Erso](https://github.com/bitcoin-dev-project/battle-of-galen-erso) GitHub repo. In brief:
> Your mission is to attack Bitcoin Core nodes in a private network running in a Kubernetes cluster. The private network consists of Bitcoin Core nodes that are vulnerable to fully-disclosed historical attacks or novel intentional flaws. A FAKE website with blog posts about all types of vulnerabilities available for exploit on Warnet can be seen here:
> https://bitcorncore.org/en/blog/

## Interacting with warnet

Despite lots of experience with Docker - in my previous life we used Docker/Docker Compose to deploy the dozen or so services that made up our product onto fixed (non-scalable) on-premise hardware - I have never had any reason to use Kubernetes before (despite engineers often pushing to make the switch for often arbitrary reasons).

So for my own benefit, and maybe those that follow, here's some of the basics of using Kubernetes and then how to use Warnet.

You'll need `kubectl` and `helm` as prerequisites to interact with the Warnet. It's also recommended to use a k8s cluster management tool like `k9s` to monitor and manage your cluster(s). Do this. `k9s` is an interactive terminal-based UI that will make understanding and managing the state of your cluster(s) a lot easier.

# The game
The following content is based on the public information available in the [Warnet: The Battle of Galen Erso](https://github.com/bitcoin-dev-project/battle-of-galen-erso) repo. No spoilers here!

## The targets
Of the 11 targets to attack, 5 of them were effectively gifts, with:
- The nodes being custom vulnerable versions of Bitcoin Core, with names/versions that gave a clue as to what they were vulnerable to, and
- There being attack templates in the Battle of Galen Erso repo for each custom vulnerability. Each template required a varying level of customisation or completion to yield a working attack.

These 5 targets were:
- `99.0.0-unknown-message`
- `98.0.0-invalid-blocks`
- `97.0.0-50-orphans`
- `95.0.0-disabled-opcodes`
- `94.0.0-5k-inv`

There was another target that also gave clues as to what it might be vulnerable to, but there was no attack template to work off:
- `96.0.0-no-mp-trim`

The remainder 5 targets were running various (unmodified??) versions of Bitcoin Core:
- `0.16.1`
- `0.17.0`
- `0.19.2`
- `0.20.0`
- `0.21.1`

# The attacks
I do my best to explain how we went about attacking the targets but again I try to avoid too many specifics or spoilers!

## The no mempool trim vulnerability

## The CAddrMan vulnerability

## The huge INV message vulnerability