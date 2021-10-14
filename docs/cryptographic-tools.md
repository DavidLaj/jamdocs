---
title: Cryptographic tools
date: 2021-10-13
slug: cryptographic-tools
---

## Hash functions

The role of any hash function is to ensure integrity of the data it hashes, thus preventing its alteration. Bitcoin almost only makes use of SHA-256 hash function<sup name="a1">[1](#footnote1)</sup>, which is a standard of the National Institute of Standards and Technology (NIST) since 2002 and fulfills the three necessary requirements for a hash function to be resistant to cryptanalytic attacks:

*Pre-image resistance* <br>
It is computationally unfeasible to find a message *m* that hashes to a given hash value h such that *hash(m)=h*. This means that is a one-way hash function. 

*Second pre-image resistance* <br>
Given a message *m1* and its corresponding hash value *h1*, such that hash(*m1*)=*h1*, it is computationally unfeasible to find another message *m2* that hashes to the same value as *m1*, such that hash(*m1*)=hash(*m2*)=*h1*.

*Collision resistance* <br>
It is computationally unfeasible to find two different messages *m1* and *m2* that hash to the same value, such that hash(*m1*)=hash(*m2*)=*h*. This means that is a strong one-way hash function.

SHA-256 algorithm follows an iterative scheme that corresponds to the Merkle–Damgård construction as shown in Figure 1. In breve, it takes as input a message of maximum length of 264 bits and divides it into blocks of 512 bits<sup name="a2">[2](#footnote2)</sup>. The first block is combined with an initialization vector (IV)[^3] and processed with the compression function to generate an intermediate 256 bits result. This intermediate result is then combined with the next 512 bits block and processed with the compression function to produce another intermediate 256 bits result, and so forth until the final 256 bits result of the hash function is given with the processing of the last block containing the padding and length of the message. 

![](https://raw.githubusercontent.com/DavidLaj/jamdocs/master/docs/images/SHA256_iterative_diagram.png "Figure 1")

The SHA-256 compression function consists of 64 rounds. It divides each block in 16 words of 32 bits and performs a lot of rotations and bitwise mixing, as shown in Figure 2.

<b name="footnote1">1</b>. Except for Bitcoin addresses, where RIPEMD-160 is used together with SHA-256.[↩](#a1) <br>
<b name="footnote2">2</b>. The initial message is padded to reach a length that is a multiple of 512 bits. The padding structure incorporates the length of the original message in binary: this is called the Merkle-Damgård strengthening and ensures the security of the scheme. [↩](#a2) <br>
[^3]: The IV of SHA-256 is eight 32 bits words corresponding to the first 32 bits of the decimals part of the square root of the first eight prime numbers.

## Merkle trees 

Even though Jamdocs is so simple, you dont really need to set it up localy (you could just fork it on github to edit styles and md-files) - if you want to change it up a bit I recommend setting up localy for a better developer experience.

To set up a new instance of Jamdocs, and start developing just clone the project from Github like, go to the directory and run gridsome:



## Elliptic curves