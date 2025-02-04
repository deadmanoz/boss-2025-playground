# Lightning RPC Scavenger Hunt
The gist of this challenge was to get familiar with the basic operation of a Lightning node. 

I noticed that some participants proceeded to rapidly complete the challenge after learning of the challenge details (e.g. within an hour or 2, good on them!) but I decided to do some reading first starting with the first 5 chapters of [Mastering the Lightning Network book](https://github.com/lnbook/lnbook). This is essentially the counterpart of [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook) but for Lightning! Not trying to throw shade, mainly provide some justification for some of the notes that follow 😅..

## Random notes about Lightning
### Differences to Bitcoin L1 in fee structure
In Bitcoin, putting transaction confirmation timeliness aside, you pay according to the size of the transaction. Larger transactions such as those with more inputs, outputs, or more complex encumberances to be satisfied, cost more. The value of the transaction is not a factor in the cost.

In Lightning, the value of a payment *IS* a factor in the cost `Chapter 3: How the Lightning Network Works - Mining Fees Versus Routing Fee`: 
> Typically, the routing user will charge the sender based on the *value* of the payment, having established a minimum *base fee* (a flat fee for each payment) and a *fee rate* (a prorated fee proportional to the value of the payment).

Said another way `Chapter 3: How the Lightning Network Works - Incentives for Large Value Payment Versus Small Value Payment`:
> The fee structure in Bitcoin is independent of the transaction value. A $1 million transaction has the same fee as a $1 transaction on Bitcoin, assuming a similar transaction size, in bytes. (..) In Lightning the fee is a fixed-base fee plus a percentage of the transaction value.

### Differences to Bitcoin L1 in interativity
Receiving bitcoin on Bitcoin is a passive and asynchronous activity. You, or software acting on your behalf, doesn't need to be online to receive bitcoin. This is not the case in Lightning, where you (a Lightning node you run) or software acting on your behalf (a Lightning node operated by a third party) needs to be online to receive bitcoin on Lightning.

`Chapter 3: How the Lightning Network Works - Offline Versus Online, Asynchronous Versus Synchronous`:
> Receiving bitcoin on the Bitcoin blockchain is a passive and asynchronous activity that does not require any interaction by the recipient or for the recipient to be online at any time. (..) In Lightning, the recipient must be online to complete the payment before it expires. The recipient must run a node or have someone that runs a node on their behalf (a third-party custodian). To be precise, both nodes, the sender’s and the recipient’s, must be online at the time of payment and must coordinate. Receiving a Lightning payment is an active and synchronous activity between sender and recipient.

More to follow..

## Some notes on the challenge
The most interesting part was putting a toe into the water of custom routing of payments. Seemed pretty straightforward once you explore the LND RPC methods, but there were a few gotchas that tripped me up (I won't note them here as they are part of the challenge!). `INCORRECT_OR_UNKNOWN_PAYMENT_DETAILS` is an annoyingly vague error, but I can appreciate it's necessary to not reveal too much information on failures lest they be exploited.
