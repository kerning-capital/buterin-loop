Proposing trusted loops running against trustless networks.

A Buterin Loop is a protocol that **couples activity in a trusted computer to activity in an Ethereum contract**. This lets you build custom micro-programs that run local application logic around a standard interface into the chain.

### What is a Buterin Loop?

### Usage

# TODO

### How it works
 
Implementing a loop consists of
 
* An event emitted from a contract that opens a new loop with a unique key
* Logic within your contract to prevent double-starting a loop while a loop with the same key is in progress
* A trusted computer that will
    1) Subscribe to keyed events from an Ethereum contract
    2) Perform some trusted logic internally, then
    3) Post the results back into the chain with the proper key
    4) (optionally) Subscribe to any events that are a result of the 2nd transaction that closes the loop
* Access rules in your contract that restrict loop-closing transactions to an Oracle address you set up with the loop

![Buterin Loop](https://i.imgur.com/3F1sa21.png)

### How can I use this?

By coupling our own computer to chain activity, we can create tiny local systems that allow exotic and diverse activity (things that are not possible on chain) that is driven off of activity on Ethereum. The Ethereum blockchain is a global payment network with a global computer attached. By coupling this network to local machine activity, we can create custom local logic that uses the global computer and payment system.

 We can build a casino, an escrow bookshelf, or a pay-by-use solar drone charger that accepts real money... all with free software and a few dozen lines of code (the future is amazing). Inside the "trusted" portions of your code, you can

* Do things you cannot do on chain
* Run a completely private system keyed simply into chain logic
* Impose local auditing or permissions
* Compose more complex application logic with repeatable, simple interfaces

### Why should I use this?

A Buterin Loop lets us use one common pattern of on/off-chain activity in a repeatable way. The pattern becomes more portable, easier to reason about, and easier to upgrade across systems (with npm install).

Defining a standard class like this also allows us to compose more complex systems out of simple atomic pieces. This in turn yields a more repeatable, coherent, and complete mix between trustless and trusted systems in our code.

### Working Example: slot machine

You can run a fun-money slot machine that runs on a tiny random number generator box that sits on your desk. *disclaimer: this is a theoretical experiment intended for fun money for now. don't gamble if it's illegal*.

**It works like this.**

The slot machine runs off of [this contract system](https://github.com/brynwaldwick/blockwoods/blob/master/contracts/slot-machine.coffee). When you send ether to the SlotMachineResolver contract with the spinWheel function, it produces a transaction that has an Event attached. From this transaction, a tiny server that has subscribed to the Contract generates 3 random numbers.

These numbers are posted back into the Contract by a trusted Oracle address to close the loop. The Contract uses a (pluggable) game template to resolve the resulting random numbers into payouts to bettors based off of different types of games with different jackpot structures.

The loop itself in this case is simple... but having a repeatable class like this lets us scale patterns and develop inside our systems very consistently. We can spawn or replicate trustless interfaces across our system and couple them to local system activity that we code.

### About the trust model

In a fitting way, this loop's trust model matches that of a casino. We are trusting that the slot itself is fair, or at least runs on the terms that are presented. When we walk into a casino, we assume the odds are what they say they are. The casino itself trusts that its games, dice, cards (both phyisical and electronic) will function as specified and report accurate payouts based on suitably random results. We trust our little box to act like a cool little atomic 3-slot slot machine. In fact the loop is structured in such a way that many out-of-loop failures could be restarted with a suitably fair restart function and then close the loop as expected on-chain.

There are many disclaimers to be made about systems like this. The architecture is experimental. Without solid verification to make sure you are running the code you think you are, you can run into problems. Once you leave the chain your security is a matter of degrees. We can build very useful systems with this type of architecture but make sure you understand the tradeoffs you are making.

