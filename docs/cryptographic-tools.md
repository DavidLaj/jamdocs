---
title: Cryptographic tools
date: 2021-10-13
slug: cryptographic-tools
---

## Hash functions

The role of any hash function is to ensure integrity of the data it hashes, thus preventing its alteration. Bitcoin almost only makes use of SHA-256 hash function<sup id="a1">[1](#footnote1)</sup>, which is a standard of the National Institute of Standards and Technology (NIST) since 2002 and fulfills the three necessary requirements for a hash function to be resistant to cryptanalytic attacks:

***Pre-image resistance*** <br>
It is computationally unfeasible to find a message *m* that hashes to a given hash value h such that *hash(m)=h*. This means that is a one-way hash function. 

***Second pre-image resistance*** <br>
Given a message *m1* and its corresponding hash value *h1*, such that hash(*m1*)=*h1*, it is computationally unfeasible to find another message *m2* that hashes to the same value as *m1*, such that hash(*m1*)=hash(*m2*)=*h1*.

***Collision resistance*** <br>
It is computationally unfeasible to find two different messages *m1* and *m2* that hash to the same value, such that hash(*m1*)=hash(*m2*)=*h*. This means that is a strong one-way hash function.
<br>
<br>
SHA-256 algorithm follows an iterative scheme that corresponds to the Merkle–Damgård construction as shown in Figure 1. In breve, it takes as input a message of maximum length of 264 bits and divides it into blocks of 512 bits<sup id="a2">[2](#footnote2)</sup>. The first block is combined with an initialization vector (IV)<sup id="a3">[3](#footnote3)</sup> and processed with the compression function to generate an intermediate 256 bits result. This intermediate result is then combined with the next 512 bits block and processed with the compression function to produce another intermediate 256 bits result, and so forth until the final 256 bits result of the hash function is given with the processing of the last block containing the padding and length of the message. 

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SHA256_iterative_diagram.png "Figure 1")

The SHA-256 compression function consists of 64 rounds. It divides each block in 16 words of 32 bits and performs a lot of rotations and bitwise mixing, as shown in Figure 2.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SHA256_compression_fn.png "Figure 2")

The following are the many diversified applications of SHA-256 in Bitcoin:
 
***Addresses*** <br>
Bitcoin addresses are derived from hashing a payload (e.g. public key, script) twice, first with SHA-256 and then with RIPEMD-160. For security or just to make it shorter?

***Transaction hash*** <br>
Every input of a transaction has a field containing a double SHA-256 hash of the transaction holding the redeemed UTXO, which ensures that the latter had been included in the blockchain and that the UTXO can be spent.

***Merkle tree*** <br>
All hashes in Bitcoin’s Merkle trees are double SHA-256. Every block header contains the Merkle root hash, which is the hash at the top of the Merkle tree that acts like a fingerprint of all transactions embedded in the block.

***Previous block hash*** <br>
Every block header contains a double SHA-256 hash of the header of its parent block (i.e. the previous block in the chain), thus linking the blocks together and creating the blockchain.

***Proof-of-Work*** <br> 
To build a valid block, miners have to compute a double SHA-256 hash of the block header that is below the difficulty level.
<br>
<br>
Although Nakamoto did not explain the reason behind his decision, Ferguson and Schneier propose in their book Practical Cryptography that the use of SHA-256d prevents certain types of cryptographic attacks against Merkle-Damgård constructs, called *length extension attacks*. However, Craig Wright does not agree with this idea because other more efficient mechanisms already exist in Bitcoin to prevent this type of attack. Rather, he claims that the use of SHA-256d allows the system to separate the transaction validation and proof-of-work functions between multiple entities, as well as offering the ability to regulate the content of the transactions by filtering the hashes.
<br>
<br>
Footnotes: <br>
<b id="footnote1">1</b>. Except for Bitcoin addresses, where RIPEMD-160 is used together with SHA-256. [↩](#a1) <br>
<b id="footnote2">2</b>. The initial message is padded to reach a length that is a multiple of 512 bits. The padding structure incorporates the length of the original message in binary: this is called the Merkle-Damgård strengthening and ensures the security of the scheme. [↩](#a2) <br>
<b id="footnote3">3</b>. The IV of SHA-256 is eight 32 bits words corresponding to the first 32 bits of the decimals part of the square root of the first eight prime numbers. [↩](#a3)

## Merkle trees 

Merkle trees are hierarchical data structures consisting of hashes and are used in Bitcoin to check the integrity of the transactions embedded in a block. The base of the tree is composed of the leaves, which are SHA-256 double hashes of each transaction. Leaves are paired and hashed twice to form the upper row of the tree<sup id="a4">[4](#footnote4)</sup>. The same process is repeated up to the top of the tree, that consists of a single hash, the Merkle root, derived from the hashes of all transactions, as shown in the next Figure.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/MerkleTree.png "Figure 3")
<br>
<br>
The Merkle root is a part of the block header and acts as a digital fingerprint in the verification process accomplished, by the full nodes, of all transactions included in the block. The nodes that do not store the full blockchain (i.e. lightweight nodes) can also check whether a transaction is included in a block by asking a full node for the Merkle path, or Merkle branch, of the transaction. The Merkle path is a series of hashes that were paired with the transaction hash and its parents, from the leaves to the Merkle root. This method is efficient and requires up to a thousand times less data than the amount of data contained in a block (1 MB). For example, the Figure below shows the hashes (in plain blue) that form the Merkle path of the transaction in green.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/MerklePath.png "Figure 4")
<br>
<br>
Footnotes: <br>
<b id="footnote4">4</b>. A Bitcoin block can contain several hundred transactions. In case there is an odd number of transactions, the last one is duplicated and concatenated with itself to form what is called a balanced tree. [↩](#a4)

## Elliptic curves

Elliptic curve cryptography (ECC) uses curves of the form ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}y^2%20=%20x^3%20%2B%20ax%20%2Bb%20\mod%20p), where ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}p) is a prime number. Points that satisfy the equation form a cyclic group and obey a group operation called point doubling, which consists of multiplying an integer by one point on the curve, resulting in another point on the curve. This property is at the base of the Elliptic Curve Discrete Logarithm Problem (ECDLP) underlying elliptic curves over finite fields and that provides security to ECC. Specifically, the ECDLP states that given an elliptic curve over a finite field, a generator ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}G) of a cyclic subgroup and a point ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}Q) of that cyclic group, finding an integer ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}d)  so that ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}Q%20=%20d%20∙%20G) is computationally unfeasible. Thus, the random integer ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}d) that multiplies a point on the curve can be used as a private key, and the public key can be defined as the point ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}Q) on the curve resulting from the multiplication, along with the curve’s parameters. To help visualize ECC, the next Figure presents three graphs: the one on the left shows an elliptic curve (secp256k1) over the real numbers ![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}y^2%20=%20x^3%20%2B%20ax%20%2Bb), the one in the middle illustrates how the point doubling process results in a point on the curve, and the one on the right shows the same doubling process but applied to a curve over a finite field (![formula](https://render.githubusercontent.com/render/math?math=\color{darkorange}y^2%20=%20x^3%20%2B%20ax%20%2Bb%20\mod%20p)). 