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

The set of rules by which the Bitcoin network operate is dictated by the Bitcoin Core client, also known as the *reference client* or the *Satoshi client* (because it was first developed and released by Satoshi Nakamoto on January 9, 2009). The rules specified in its versions tend to be adopted by other client implementations. However, there is no formal protocol specification. The Bitcoin protocol is open source and anyone has the right to propose and apply changes to the codebase. Bitcoin Core is considered the reference implementation of Bitcoin, from which all other implementations and customizations are derived. 

## Alteration component

This component establishes how the protocol evolves over time. It defines the decision-making process (governance) and how those decisions translate into changes and new implementations of the protocol.

### Protocol governance

At its foundation, the governance of Bitcoin is intended to be anarchic, meaning that proposed changes to the protocol are provided and approved cooperatively and voluntarily, due to the absence of a central authority. However, in reality, while anyone can propose changes through the Bitcoin Improvement Proposal (BIP) process, the Bitcoin Core project is controlled by a small group of developers called *maintainers* who have the authority to decide on improvements. That is, maintainers can unilaterally decide whether or not a proposed modification in a BIP will be adopted in future deployments. This is why Bitcoin governance could best be described as hierarchical, as changes to the protocol are based on the consent of the leaders. 

Past events have demonstrated this hierarchical governance model. In the Bitcoin client version 0.8.2, maintainers unilaterally decided to reduce the default fees for low priority transactions from 0.0005 BTC to 0.0001 BTC. Such control over the protocol modification process clearly gives maintainers the power to affect the entire Bitcoin economy. Another good example showing the lack of democracy in the governance of Bitcoin concerns the events surrounding the debate on the increase of the 1MB block size limit. This debate led to the departure of several of the original top Bitcoin developers who were in favor of increasing the limit. Some argue that the decision not to increase the block size on the reference client was motivated by a desire to limit the growth of the system to the benefit of the Chinese mining pools that controlled the majority of the computational power, at the expense of the Bitcoin community.

This lack of democracy and the centralization of the decision-making power go against the fundamentals of Bitcoin, which claims to be a fully decentralized system. This raises concerns about the legitimacy of the decision-making process and therefore about the security of the system, as it tends to bring back Bitcoin a trusted third party scheme. 

### Changes to the protocol