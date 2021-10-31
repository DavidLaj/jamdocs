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

Past events have demonstrated this hierarchical governance model. In the Bitcoin client version 0.8.2, maintainers unilaterally decided to reduce the default fees for low priority transactions from 0.0005 BTC to 0.0001 BTC. Such control over the protocol modification process clearly gives maintainers the power to affect the entire Bitcoin economy. Another good example showing the lack of democracy in the governance of Bitcoin concerns the events surrounding the debate on the increase of the 1MB block size limit. This debate led to the departure of several of the original top Bitcoin developers who were in favor of increasing the limit<sup id="a1">[1](#footnote1)</sup>. Some argue that the decision not to increase the block size on the reference client was motivated by a desire to limit the growth of the system to the benefit of the Chinese mining pools that controlled the majority of the computational power, at the expense of the Bitcoin community.

This lack of democracy and the centralization of the decision-making power go against the fundamentals of Bitcoin, which claims to be a fully decentralized system. This raises concerns about the legitimacy of the decision-making process and therefore about the security of the system, as it tends to bring back Bitcoin a trusted third party scheme. 

Footnotes: <br>
<b id="footnote1">1</b>. Which led to the launch of Bitcoin XT, a Bitcoin client where miners can vote to change the block size limit. [↩](#a1) <br>

### Changes to the protocol

Bitcoin source code is open, allowing developers to maintain and support the growth of the system by proposing improvements to the protocol through a standard Bitcoin Improvement Proposal (BIP) process, which may require consensus from the community and has to go through various statuses such as *drafted*, *verified*, *accepted*, *rejected* or *replaced*. Ultimately, Bitcoin Core maintainers have the authority to decide on the changes to be applied. Most Bitcoin Core developers are volunteers and contribute without getting paid, while more experienced developers can be sponsored by one person or a company to work on the project. Therefore, the financing component of Bitcoin is described as altruistic.

Open source code also allows anyone to modify the it and run their own version of the protocol on the network, thus creating a fork in the blockchain. This can result in an alternative currency if enough client implementations support the same set of rules (e.g. Bitcoin XT, Bitcoin Cash) or even lead to a new blockchain application (e.g. Namecoin).

When changes are applied to the reference client and users run different versions of Bitcoin Core at the same time on the network, it creates forks in the chain that can be of two types depending on the nature of the changes: *soft fork* or *hard fork*.

* ***Soft fork***<br>
A soft fork is a change to an implementation that is backward compatible (e.g. a reduction in the block size limit). Outdated nodes can continue to transact with the updated nodes, but blocks that violate the new rules are made obsolete by the updated nodes. If a majority of the computational power of the network follows the new rules, there will be no permanent divergence in the chain because outdated nodes will accept the chain of updated nodes as the main chain. A soft fork is illustrated in the next Figure.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SoftFork.png "Figure 1")

* ***Hard fork***<br>
A hard fork is a change to an implementation that makes the client incompatible with previous versions (e.g. an increase in the block size limit or a new transaction feature). Nodes must update their client to the new protocol so that they do not reject the blocks constructed from the new rules. Outdated nodes continue to publish blocks using the old format and only accept blocks in this format. This results in a permanent divergence of the blockchain where two versions exist simultaneously, one from the updated nodes and one from the outdated nodes, as shown in the Figure below. 

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/HardFork.png "Figure 2")