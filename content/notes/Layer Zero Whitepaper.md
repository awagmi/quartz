---
title: "Layer Zero Whitepaper"
tags:
- whitepaper
- protocol
disableToc: false #no table of contents
---

Author(s): [Ryan Zarick](notes/Ryan%20Zarick.md), [Bryan Pellegrino](notes/Bryan%20Pellegrino.md), [Caleb Banister](notes/Caleb%20Banister.md)  
Submitted on: [26-05-2021](notes/26-05-2021.md)  
Written on: [16-03-2022](notes/16-03-2022.md)  
URL: https://layerzero.network/pdf/LayerZero_Whitepaper_Release.pdf  

This is a summary of the whitepaper in the URL above. 

## TLDR
[Layer Zero](notes/Layer%20Zero.md) - a Trustless Omnichain interoperability protocol  
- Allows 2 separate blockchains to communicate using 2 independent components 
- General communication layer, NOT a blockchain
- Touts benefits of being less complex, less overheads, and being able to extend to any blockchain without being hindered by its native SDK.

## Introduction
Core pillars of blockchain - [Decentralization](notes/Decentralization.md), [Transparency](notes/Transparency.md), [Immutability](notes/Immutability.md)
### Problem
Liquidity/developers/users are walled within an blockchain
![](notes/images/Pasted%20image%2020220317231855.png)
Bandaid measures:
1. use of centralized services (such as [Binance](notes/Binance.md))
	- Does not align with [Trustlessness](notes/Trustlessness.md) of blockchain
2. use of [Cross Chain Bridge](notes/Cross%20Chain%20Bridge.md)s such as [AnySwap](notes/AnySwap.md) or [THORChain](notes/THORChain.md)
	- Relies on protocol specific intermediate tokens or intermediate consensus layers

### Solution
[Layer Zero](notes/Layer%20Zero.md), a **communication primitive**, allows transactions between multiple blockchain using
- allows transactions between multiple [Layer 1](notes/Layer%201.md) or [Layer 2](notes/Layer%202.md) chains.
- Intuition: if two independent entities can corroborate the validity of a transaction, then Chain B can be sure the transaction from Chain A is valid


## Related Work
1. [Ethereum](notes/Ethereum.md) - original [Ethereum Virtual Machine](notes/Ethereum%20Virtual%20Machine.md) chain, but low 15-45tps makes it hard to scale  
[Polygon](notes/Polygon.md) - [Layer 2](notes/Layer%202.md) [Sidechain](notes/Sidechain.md)  
2. [PolkaDot](notes/PolkaDot.md) - [Parachains](notes/Parachains.md) are interlinked via a common relay chain. However there is additional cost when using this relay chain  
3. [THORChain](notes/THORChain.md) - Uses pairwise liquidity pools to bind 3rd party currency to [$RUNE](notes/$RUNE.md). While authors acknowledge it solves  the scalability problem, it is cumbersome to use as a simple transaction can be quite complicated  
4. [AnySwap](notes/AnySwap.md) - [Decentralized Exchange](notes/Decentralized%20Exchange.md) which relies on intermediate token [$ANY](notes/$ANY.md) , similar to [THORChain](notes/THORChain.md). Similar issues too
5. [Cosmos](notes/Cosmos.md) - [Inter-Blockchain Communication](notes/Inter-Blockchain%20Communication.md) built on [Tendermint BFT](notes/Tendermint%20BFT.md) which allows messaging between chains built on the hub  
	- [Inter-Blockchain Communication](notes/Inter-Blockchain%20Communication.md) requires a full on chain [Light node](notes/Light%20node.md) of each chain  
	- Only provides direct communication between fast finality chains  
	- [Gravity Bridge](notes/Gravity%20Bridge.md) - [AnySwap](notes/AnySwap.md) and [Cosmos](notes/Cosmos.md) equivalent   
6. [Chainlink](notes/Chainlink.md) - Framework for building   

## How does LayerZero Work?
Communication protocol which provides trustless [Valid Delivery](notes/Valid%20Delivery.md)
### Components
#### Endpoints
[Smart Contracts](notes/Smart%20Contracts.md) which implement 4 modules which basically share information with each other:
1. Communicator
2. Validator
3. Network
4. Libraries - each new chain is an added library
1 Endpoint for each chain.
#### Oracle
Can be any oracle to read a block from one chain and send to another  
Expected to be [Chainlink](notes/Chainlink.md) as industry leader

#### Relayer
Off-chain service that fetches proof for a specific transaction
- similar to oracle but for proof of transaction)

### Steps
![](notes/images/Pasted%20image%2020220317234427.png)
**Simplified Version with less jargon**   
1. Person A on Chain A wants to send a *`message`* to Chain B using an application. Application tells the Communicator A relevant details about where the *`message`* wants to go
2. Communicator A tells Validator A that there is a *`message`* to be validated
3. (step 3 and 4 happen concurrently) Validator A tells Network A that the **block header H** needs to be sent to Chain B
4. (step 3 and 4 happen concurrently) Validator A tells Relayer A that he needs the proof that the *`message`* on Chain A has been sent. 
5. Network A tells Oracle to send current **block header H** of Chain A to Chain B
6. (step 6 and 7 happen asynchronously) Oracle gets current **block header H** from Chain A
7. (step 6 and 7 happen asynchronously) Relayer gets the *message transaction proof* of the *`message`* from Chain A, and stores it off-chain
8. Oracle confirms that the block referred to by **block header H** is finalized, then sends H to Chain B
9. Network B receives the block header hash, and gives it to Validator B
10. Validator B sends the hash to Relayer B
11. Relayer B sends the *`message`* and *message transaction proof* to Validator B
12. Validator B gets both the *`message`* and *message transaction proof*, and checks if the *proof* and the **block header H** it received in step 9 matches. If yes, send *`message`* to communicator.
13. Communicator will send *`message`* to App B.


## Application
1. [Cross Chain Bridge](notes/Cross%20Chain%20Bridge.md)  
2. [Multi-Chain](notes/Multi-Chain.md) [Yield Aggregator](notes/Yield%20Aggregator.md)  
3. [Multi-Chain](notes/Multi-Chain.md) [Lending](notes/Lending.md)

## Example transactions
Refer to this [tweet](https://twitter.com/ryanzarick/status/1503893827525951489) here.   
The [Ethereum](notes/Ethereum.md) message was sent at Mar-15-2022 05:46:00 PM +UTC
1. [Ethereum](notes/Ethereum.md) received message 5 min later at 05:55:16 PM +UTC
2. [Fantom](notes/Fantom.md) received message 8 min later at 06:03:19 PM +UTC
3. [Polygon](notes/Polygon.md) received message 2 hours later at 07:51:29 PM +UTC
4. [Optimistic](notes/Optimistic.md) received message 2.5 hours later at 08:27:13 PM +UTC
5. [Avalanche](notes/Avalanche.md) received message 2.5 hours later at 08:27:19 PM +UTC
6. [Arbitum](notes/Arbitum.md) received message 2.5 hours later at 08:26:25 PM +UTC
7. [BNB Chain](notes/BNB%20Chain.md) received message 2.5 hours later at 08:27:19 PM +UTC

Seems like interchain communication still takes some time (up to 2.5 hours). Personally not sure if i would wait 2.5 hours to bridge when there are much faster alternatives. Let me know if i interpreted this wrongly