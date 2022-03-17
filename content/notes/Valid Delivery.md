---
title: "Valid Delivery"
disableToc: false #no table of contents
---

## Definition
Formulated in [Layer Zero Whitepaper](notes/Layer%20Zero%20Whitepaper.md)
1. Every message m sent over the network is coupled with a transaction t on the sender-side chain.
2. A message m is delivered to the receiver if and only if the associated transaction t is valid and has been committed on the sender-side chain.

## Example 1
1. Person A sends token to B - transaction 1
2. Only when Person B confirms that transaction 1 is valid, Person B sends another coin/message/balance to Person A
## Example 2 
1. Person A sends [Bitcoin](notes/Bitcoin.md) to [Binance](notes/Binance.md) - transaction 1
2. Only when [Binance](notes/Binance.md) confirms that transaction 1 is valid on the [Bitcoin](notes/Bitcoin.md) blockchain, [Binance](notes/Binance.md) issues some bitcoin balance to Person A's account