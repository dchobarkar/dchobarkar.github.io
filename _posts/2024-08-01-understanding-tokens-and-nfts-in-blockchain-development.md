# From Web to Web3 - 06: Understanding Tokens and NFTs in Blockchain Development

## 🧩 Introduction

In the blockchain world, **tokens** and **NFTs (Non-Fungible Tokens)** have become the backbone of many decentralized applications—spanning finance, gaming, art, identity, and beyond. Whether you're interacting with DeFi protocols, collecting digital art, or building play-to-earn ecosystems, you're dealing with tokens under the hood. 🧠✨

### 🔹 What are Tokens and NFTs in the Context of Blockchain?

At their core, **tokens** are digital representations of assets or utilities that reside on a blockchain. They can be **fungible**, like cryptocurrencies (ETH, USDC, DAI), or **non-fungible**, like a unique piece of art or a collectible card.

- **Fungible tokens** are interchangeable. One USDC is always equal to another USDC.
- **Non-Fungible Tokens (NFTs)** are **unique** digital assets with metadata that makes each one distinct—think of them like digital fingerprints on the blockchain.

These tokens are not native to the blockchain itself but are governed by smart contracts that adhere to specific **token standards**—like ERC-20, ERC-721, and ERC-1155 in the Ethereum ecosystem.

### 🔧 Role of Smart Contracts in Token Creation

Smart contracts are the engine behind tokenization. They define how tokens behave—how they can be transferred, who can mint them, and what metadata they carry.

Here's a simple ERC-20 snippet written in Solidity:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract DevToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("DevToken", "DVT") {
        _mint(msg.sender, initialSupply);
    }
}
```

With just a few lines of code and OpenZeppelin’s battle-tested contracts, you can spin up a fully-functional fungible token! The same goes for NFTs using the ERC-721 or ERC-1155 standards.

### 📜 Evolution of Token Standards in Ethereum and Beyond

The Ethereum ecosystem introduced token standards to streamline how tokens are built and interacted with:

- **ERC-20**: The standard for fungible tokens. Widely used across DeFi projects.
- **ERC-721**: The pioneer standard for NFTs. Ideal for 1:1 unique items.
- **ERC-1155**: A hybrid multi-token standard supporting both fungible and non-fungible assets—perfect for games and metaverse apps.

Outside Ethereum, blockchains like **Solana, Flow, and Tezos** have their own token mechanisms, but the core idea remains: **tokens are programmable assets.**

### 💡 Why Developers Should Care: Power of Programmable Assets

Tokens and NFTs aren't just buzzwords—they're **building blocks of Web3**. For developers, they unlock new ways to:

- **Monetize applications** (via tokens or NFTs)
- **Create programmable economies** (think in-game markets or DAO governance)
- **Build on composable infrastructure** (DeFi lego blocks, NFT marketplaces, lending protocols)

This shift toward **tokenized infrastructure** means your code can now interact with **global digital economies**, opening up powerful new paradigms of value creation 💸🔗
