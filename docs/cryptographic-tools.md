---
title: Cryptographic tools
date: 2021-10-13
slug: cryptographic-tools
---

## Hash functions

The role of any hash function is to ensure integrity of the data it hashes, thus preventing its alteration. Bitcoin almost only makes use of SHA-256 hash function<sup id="a1">[1](#footnote1)</sup>, which is a standard of the National Institute of Standards and Technology (NIST) since 2002 and fulfills the three necessary requirements for a hash function to be resistant to cryptanalytic attacks:

* ***Pre-image resistance*** <br> It is computationally unfeasible to find a message *m* that hashes to a given hash value ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}h) such that ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}hash(m)%20=%20h). This means that is a one-way hash function. 

* ***Second pre-image resistance*** <br> Given a message ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m_1) and its corresponding hash value ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}h_1), such that ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}hash(m_1)%20=%20h_1), it is computationally unfeasible to find another message ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m_2) that hashes to the same value as ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m_1), such that ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}hash(m_1)%20=%20hash(m_2)%20=%20h_1).

* ***Collision resistance*** <br> It is computationally unfeasible to find two different messages ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m_1) and ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m_2) that hash to the same value, such that ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}hash(m_1)%20=%20hash(m_2)%20=%20h). This means that is a strong one-way hash function.

SHA-256 algorithm follows an iterative scheme that corresponds to the Merkle–Damgård construction as shown in Figure 1. In breve, it takes as input a message of maximum length of 264 bits and divides it into blocks of 512 bits<sup id="a2">[2](#footnote2)</sup>. The first block is combined with an initialization vector (IV)<sup id="a3">[3](#footnote3)</sup> and processed with the compression function to generate an intermediate 256 bits result. This intermediate result is then combined with the next 512 bits block and processed with the compression function to produce another intermediate 256 bits result, and so forth until the final 256 bits result of the hash function is given with the processing of the last block containing the padding and length of the message. 

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SHA256_iterative_diagram.png "Figure 1")

The SHA-256 compression function consists of 64 rounds. It divides each block in 16 words of 32 bits and performs a lot of rotations and bitwise mixing, as shown in Figure 2.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SHA256_compression_fn.png "Figure 2")

The following are the many diversified applications of SHA-256 in Bitcoin:
 
* ***Addresses*** <br> Bitcoin addresses are derived from hashing a payload (e.g. public key, script) twice, first with SHA-256 and then with RIPEMD-160. For security or just to make it shorter?

* ***Transaction hash*** <br> Every input of a transaction has a field containing a double SHA-256 hash of the transaction holding the redeemed UTXO, which ensures that the latter had been included in the blockchain and that the UTXO can be spent.

* ***Merkle tree*** <br> All hashes in Bitcoin’s Merkle trees are double SHA-256. Every block header contains the Merkle root hash, which is the hash at the top of the Merkle tree that acts like a fingerprint of all transactions embedded in the block.

* ***Previous block hash*** <br> Every block header contains a double SHA-256 hash of the header of its parent block (i.e. the previous block in the chain), thus linking the blocks together and creating the blockchain.

* ***Proof-of-Work*** <br> To build a valid block, miners have to compute a double SHA-256 hash of the block header that is below the difficulty level.

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

Elliptic curve cryptography (ECC) uses curves of the form ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}y^2%20=%20x^3%20%2B%20ax%20%2Bb%20\mod%20p), where ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}p) is a prime number. Points that satisfy the equation form a cyclic group and obey a group operation called point doubling, which consists of multiplying an integer by one point on the curve, resulting in another point on the curve. This property is at the base of the Elliptic Curve Discrete Logarithm Problem (ECDLP) underlying elliptic curves over finite fields and that provides security to ECC. Specifically, the ECDLP states that given an elliptic curve over a finite field, a generator ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}G) of a cyclic subgroup and a point ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}Q) of that cyclic group, finding an integer ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}d)  so that ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}Q%20=%20d%20\cdot%20G) is computationally unfeasible. Thus, the random integer ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}d) that multiplies a point on the curve can be used as a private key, and the public key can be defined as the point ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}Q) on the curve resulting from the multiplication, along with the curve’s parameters. To help visualize ECC, the next Figure presents three graphs: the one on the left shows an elliptic curve (secp256k1) over the real numbers ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}y^2%20=%20x^3%20%2B%20ax%20%2Bb), the one in the middle illustrates how the point doubling process results in a point on the curve, and the one on the right shows the same doubling process but applied to a curve over a finite field (![formula](https://render.githubusercontent.com/render/math?math=\color{orange}y^2%20=%20x^3%20%2B%20ax%20%2Bb%20\mod%20p)). 

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/ECC1.png "Figure 5")

Elliptic curves are used in Bitcoin to generate key pairs:

* The hashes of the public keys are used as Bitcoin addresses for sending funds
* Private keys are used in the Elliptical Curve Digital Signature Algorithm (ECDSA), which serves to sign transactions and ensure that only the rightful owner can spend the funds

To generate key pairs, Bitcoin uses a curve called secp256k1 recommended by Standards for Efficient Cryptography (SEC) and whose parameters are described in the Table below.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/ECC2.png "Figure 6")

When a Bitcoin transaction is created, the sender identifies the new owner of the funds by his address, which is a hash of his public key, and only him can spend the funds by proving that he possesses the associated private key. When the new owner further creates a transaction to spend those funds, he proves his legitimacy by signing the transaction and providing his public key. The transaction is broadcast to the network and checked by the nodes who receive it. The sender’s public key is hashed to ensure that it corresponds to the address the previous owner sent the funds at, and a verification is maid that the signature was generated with the required private key. The signature and its verification are performed the following way and are represented in the next Figure: 

***Signature***<br>
1. A random integer r is chosen, where ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}0%20<%20r%20<%20n)<br>
2. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}R%20=%20r%20\cdot%20G) is calculated, where ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}R%20=%20(x_R,%20y_R)) is a point on the curve<br>
3. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}s%20=%20r^{-1}%20\big[h(m)%20%2B%20d%20\cdot%20x_R\big]%20\mod%20n)<br>
4. The signature is ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}R%20=%20(x_R,%20s))

***Signature verification***<br>
1. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}u_1%20=%20s^{-1}h(m)_R%20\mod%20n) and ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}u_2%20=%20s^{-1}x_R%20\mod%20n) are calculated<br>
2. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}V%20=%20u_1G%20%2B%20u_2Q) is calculated, where ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}V%20=%20(x_V,%20y_V)) is a point on the curve<br>
3. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}x_V%20=%20x_R) verifies the signature

*Where*,<br>
![formula](https://render.githubusercontent.com/render/math?math=\color{orange}n) and ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}G) are curve’s parameters as described in the above Table<br>
![formula](https://render.githubusercontent.com/render/math?math=\color{orange}m) is the transaction to be signed<br>
![formula](https://render.githubusercontent.com/render/math?math=\color{orange}h(m)) is the hash of the transaction<br>
![formula](https://render.githubusercontent.com/render/math?math=\color{orange}d) is the private key (i.e. a random integer)<br>
![formula](https://render.githubusercontent.com/render/math?math=\color{orange}Q) is the public key (i.e. ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}Q%20=%20d%20\cdot%20G))

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/ECDSA.png "Figure 7")<br>

A big advantage of ECDSA over other algorithms such as DSA (Digital Signature Algorithm)  and RSA  is that it offers a better key-size to security-level ratio, as shown in the next Table.

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/ECDSAvsRSA.png "Figure 8")<br>

For example, for a level of security of 128 bits, which means than it takes an average of 2<sup>128</sup>  operations to compromise the security, secp256k1 needs a key of 256 bits while RSA and DSA need a 3072 bits key [4]. This difference in the key size allows ECDSA to save computation time and reduce memory requirements, two major benefits for a system like Bitcoin. 

Moreover, in order to prevent attacks against the ECDLP (baby-step giant-step , Pollard’s rho , etc.), it is advisable to choose the size in bits of the prime ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}p), and thus the size of the keys, so that it is twice as large as the desired level of security (e.g. for a 128 bits security level, ![formula](https://render.githubusercontent.com/render/math?math=\color{orange}p) should be 256 bits).
