# ZK Email Introduction

ZK Email is an app for you to anonymously verify email signatures yet mask whatever data you would like. Each email can either be verified to be to/from specific domains or subsets of domains, or have some specific text in the body. These can be used for web2 interoperability, decentralized anonymous KYC, or interesting on-chain anonymity sets.

For a deeper dive, read our full [blog post](https://blog.aayushg.com/posts/zkemail/).

## Sections

### Installation

Get started with zkEmail, install our SDKs so that you can build your own application.

### Package Overviews

Explore our npm sdk packages:

1. **zk-email/helpers**: Utility functions for generating proof inputs.
2. **zk-email/circuits**: Circuits for generating proofs and verifying DKIM signatures.
3. **zk-email/contracts**: Solidity contracts for email verification.

### Usage Guide

This section provides a comprehensive guide on how to use the zkEmail packages. It covers everything from generating proof inputs, creating circuits for proofs, to verifying DKIM signatures.

It is recommended to go through this guide to understand how to effectively use the packages for email verification.

### Project Examples

Here you can find example projects that implement our SDKs:

1. **Twitter email verifier**: Prove you own a twitter username on chain. Link to README
2. **Email wallet**: Send money to anyone anonymously using email. [Email wallet Github](https://github.com/zkemail/email-wallet)
3. **ZKP2P**: ZKP2P is a trustless and privacy-preserving fiat-to-crypto onramp powered by ZK proofs [ZKP2P Github](https://github.com/zkp2p/zk-p2p)
4. **zkEmail Safe**: Operate Safe multisigs through email verified using ZK proofs [zkEmail Safe Github](https://github.com/javiersuweijie/zkemail-safe)

## Terminology

* **DKIM**: DomainKeys Identified Mail. An email authentication method designed to detect email spoofing.
* **Zero-Knowledge Proofs**: A cryptographic method by which one party can prove to another that they know a value x, without conveying any information apart from the fact that they know the value x.
* **RSA**: Rivest–Shamir–Adleman. A public-key cryptosystem widely used for secure data transmission.
* **Circom**: A language for defining arithmetic circuits with a focus on zero-knowledge proofs.
* **SnarkJS**: A JavaScript library for zkSNARKs.
* **zkSNARKs**: Zero-Knowledge Succinct Non-Interactive Argument of Knowledge. A form of zero-knowledge proof that is particularly short and easy to verify.
* **Poseidon Hash**: A cryptographic hash function optimized for zk-SNARKs.
* **vkey**: A verification key used by the verifier to check the proof. Usually contained on the server side of an app.
* **zkey**: Proving key usually on the client side of an application.
* **witness**: In the context of zkSNARKs, a witness is the set of private inputs to the zkSNARK.
* **constraints**: Constraints are the conditions that the zkSNARK must satisfy. The proving time increases with additional constraints!
* **Regex**: Short for regular expression, this term represents sequence of characters that forms a search pattern, commonly used for string matching within text. In the context of zkEmail where it's used to parse email headers and extract relevant information.

## FAQ

Check out our FAQ for more questions!

## Additional Reading

Github Repo for double-blind: https://github.com/doubleblind-xyz/double-blind

RSA: https://en.wikipedia.org/wiki/RSA\_(cryptosystem)

Talk: https://www.youtube.com/watch?v=sPCHiUT3TmA

Circom: https://github.com/iden3/circom

SnarkJS: https://github.com/iden3/snarkjs

## Tutorials

Circom Workshop 1: https://learn.0xparc.org/materials/circom/learning-group-1/circom-1

Circom Workshop 2: https://learn.0xparc.org/materials/circom/learning-group-1/circom-2

## Related Work

https://semaphore.appliedzkp.org/

https://stealthdrop.xyz/ + https://github.com/stealthdrop/stealthdrop

https://github.com/0xPARC/cabal

https://github.com/personaelabs/heyanon/
