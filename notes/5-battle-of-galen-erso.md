# Battle of Galen Erso
This was sort of an "off-piste" group challenge, but it was a lot of fun to participate in! Despite a slow start, with the challenge started in the middle of the night for Australia/South East Asia timezones, our team Team Emerald managed to take down all target Bitcoin nodes within about 24 hours.

The following is my take on the challenge, with some notes on the warnet and Kubernetes side of things. Be sure to also checkout some material written by other participants in the challenge:
- [jgmontoya's post about exploiting the CAddrMan vulnerability ](https://blog.jgmontoya.com/2025/02/04/CAddrMan-Vulnerability.html)
- [Bronson's post about how he took down other teams nodes before his own teams 😁](https://blog.bakungabronson.com/the-battle-of-galen-erso-recap)

![Team Emerald](./imgs/team-emerald.png)

## About the challenge
Details of the challenge are openly available on the [Warnet: The Battle of Galen Erso](https://github.com/bitcoin-dev-project/battle-of-galen-erso) GitHub repo. In brief:
> Your mission is to attack Bitcoin Core nodes in a private network running in a Kubernetes cluster. The private network consists of Bitcoin Core nodes that are vulnerable to fully-disclosed historical attacks or novel intentional flaws. A FAKE website with blog posts about all types of vulnerabilities available for exploit on Warnet can be seen here:
> https://bitcorncore.org/en/blog/

## Interacting with Kubernetes pods

Despite lots of experience with Docker - in my previous life we used Docker/Docker Compose to deploy the dozen or so services that made up our product onto fixed (non-scalable) on-premise hardware - I have never had any reason to use Kubernetes before (despite engineers often pushing to make the switch for often arbitary reasons).

So for my own benefit, and maybe those that follow, here's some of the basics of using Kubernetes with warnet and the BOSS 2025 cluster.
