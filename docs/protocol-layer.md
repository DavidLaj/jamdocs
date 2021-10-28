---
title: Protocol layer
date: 2021-10-26
slug: protocol-layer
---


The protocol layer is the basis of the Bitcoin system on which all participants agree. It defines, manages and updates the ruleset that governs the system and it codifies its architectural design. The protocol contains two components: the genesis component and the alteration component.

## Genesis component

The genesis component defines the system at the time of the network launch: its architecture, the code base and the rules of engagement within the system, including the first record (*genesis*). 

### Inter-system dependencies

Bitcoin is a self-sufficient system which operates entirely on its own without the need of intervention of any external system. The nodes that make up the P2P network generate, process, distribute and store the system data through the set of rules defined by the Bitcoin protocol. Nonetheless, Bitcoin allows interoperability with various external systems (for example, wallets) that enhance user experience.

### Codebase creation

Unlike many other cryptocurrencies that are based on a derivative of the Bitcoin protocol, the latest was built from scratch, inspired by other schemes such as Adam Back´s *Hashcash* and *b-money*.

### Rule initiation

The set of rules by which the Bitcoin network operate is dictated by the Bitcoin Core client, also known as the *reference client* or the *Satoshi client*, and the rules specified in its versions tend to be adopted by other client implementations. However, there is no formal protocol specification. The Bitcoin protocol is open source and anyone has the right to propose and apply changes to the codebase. Bitcoin Core is considered the reference implementation of Bitcoin, from which all other implementations and customizations are derived. 

## Alteration component


