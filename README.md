---
description: An overlay protocol on Bitcoin
---

# 👏 Introduction

## Status

Testnet: [https://testnet.rainbowprotocol.xyz/](https://testnet.rainbowprotocol.xyz/)

<figure><img src=".gitbook/assets/20240611-174350.jpeg" alt=""><figcaption></figcaption></figure>

## Summary

* Bitcoin Native Assets, secured by BTC&#x20;
* Smart contracts enabled Virtual Machine
* Assets Teleport:  No Bridge, No Multi-sig&#x20;
* Smart Contracts enabling Defi and Web3 applications on BTC

## Overview

Rainbow Protocol is an innovative overlay protocol designed to bring programmability to the Bitcoin ecosystem. By adding an interoperability layer on top of Bitcoin, Rainbow Protocol extends Bitcoin's core functionalities, allowing developers to create and execute complex smart contracts and decentralized applications (dApps) on the Bitcoin network. This protocol retains Bitcoin's advantages as the world's most secure and widely adopted blockchain network while introducing flexibility and scalability to support a wider range of use cases.

Rainbow Protocol is inspired by other Bitcoin overlay protocols such as Atomicals, Runes, BRC20, RGB++, etc. It has learned from the strengths and weaknesses of these different protocols to design a protocol that minimizes user understanding and usage barriers, establishing standards for asset issuance and smart contract programming. Through these improvements, Rainbow Protocol facilitates Bitcoin's evolution from a digital currency to a versatile decentralized platform.

Currently, Rainbow Protocol is in the experimental stage, focusing on the programmability of fungible tokens.



## Target Audience

The target audience of this protocol includes but is not limited to:

* Blockchain developers: Technical individuals looking to develop smart contracts and dApps on the Bitcoin network.
* Blockchain enthusiasts: Individuals with a keen interest in Bitcoin and blockchain technology.
* Researchers: Academics and scholars focused on the development and application of blockchain technology.
* Enterprises and institutions: Business entities seeking to build and deploy decentralized solutions on the Bitcoin network.

## Protocol Comparison

| Feature                 | Rainbow Protocol     | BRC20          | Atomicals                    | Runes                | RGB++      |
| ----------------------- | -------------------- | -------------- | ---------------------------- | -------------------- | ---------- |
| Token Issuance          | Taproot              | Taproot        | Taproot                      | Taproot & OP\_RETURN | OP\_RETURN |
| Transfer Script         | Taproot & OP\_RETURN | Taproot        | Taproot                      | OP\_RETURN           | OP\_RETURN |
| Computation Environment | Rainbow VM           | Unknown        | AVM                          | None                 | CKB/RISV-C |
| Smart Contract Language | Rust/JS/Python       | Not applicable | Bitcoin Script + Interpreter | Not applicable       | Rust       |
