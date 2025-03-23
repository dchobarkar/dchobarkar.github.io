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

## 🧠 Types of Tokens in Blockchain Development

Understanding the difference between **fungible** and **non-fungible** tokens is foundational for any blockchain developer. These token types serve vastly different purposes and power diverse ecosystems—from DeFi to digital art, from DAOs to gaming universes 🎮🖼️

### 📍 Fungible Tokens (ERC-20)

#### 🔍 Definition and Characteristics

Fungible tokens are **identical and interchangeable units of value**. Much like traditional currency, one token holds the same value as another of its kind. These are typically used for:

- **Currencies** (e.g., USDC, DAI)
- **Governance tokens** (e.g., AAVE, COMP)
- **Utility tokens** (e.g., BAT, GRT)

They are defined by the **ERC-20 standard**, which allows any wallet or dApp to understand and interact with these tokens in a consistent way.

#### 🔧 ERC-20 Token Interface: Key Functions

Here are the core functions you’ll find in an ERC-20 contract:

```solidity
function totalSupply() public view returns (uint256);
function balanceOf(address account) public view returns (uint256);
function transfer(address recipient, uint256 amount) public returns (bool);
function approve(address spender, uint256 amount) public returns (bool);
function allowance(address owner, address spender) public view returns (uint256);
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool);
```

These functions handle balance tracking, token transfers, and delegated spending—everything needed for a robust token economy 💹

#### 📢 Events and Their Importance

ERC-20 defines two key events that are crucial for dApp integration and frontends:

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);
```

These events are **emitted during token actions** and enable dApps, explorers, and wallets to track activity and update UI components in real-time.

#### ⚠️ Common Implementation Mistakes

Even experienced devs can stumble on:

- Forgetting to return `true` in `transfer()` and `approve()` (required by the standard)
- Failing to handle `approve` front-running issues (use `increaseAllowance` instead)
- Missing safe math operations (especially before Solidity 0.8 which includes built-in overflow checks)

Using libraries like **OpenZeppelin** can save you from a world of pain 😅

#### 🌍 Real-World Case Studies

- **AAVE**: A DeFi protocol where ERC-20 tokens represent deposits and loans.
- **COMP**: Governance token for Compound, used for protocol decisions.
- **USDT/USDC**: Stablecoins pegged to fiat currencies, essential for DeFi liquidity.

### 📍 Non-Fungible Tokens

#### 🔹 ERC-721: Unique Asset Standard

#### 👤 One-Token-One-Owner Model

ERC-721 tokens are **non-interchangeable** and uniquely identifiable. Each token has its own `tokenId`, and only one owner at a time. Think of them as the digital version of physical collectibles.

#### 🎨 Common Use Cases

- **Digital Art** (e.g., Beeple's artwork)
- **Collectibles** (e.g., CryptoKitties)
- **Domain Names** (e.g., ENS)
- **Identity or Certifications**

#### 🔍 Interface Breakdown

```solidity
function ownerOf(uint256 tokenId) public view returns (address);
function tokenURI(uint256 tokenId) public view returns (string memory);
function safeTransferFrom(address from, address to, uint256 tokenId) public;
```

These enable unique ownership, metadata representation, and secure transfer of individual NFTs.

#### 🔹 ERC-1155: Multi-Token Standard

ERC-1155 was introduced by Enjin to **optimize NFT handling** in gaming and metaverse applications. It supports:

- **Fungible tokens**
- **Non-fungible tokens**
- **Semi-fungible tokens** (think limited edition items)

#### 💡 Why ERC-1155 Was Introduced

ERC-721 was inefficient for games that needed to mint multiple identical tokens. ERC-1155 solves this by allowing **batch operations** and **shared smart contract logic** across token types.

#### ⚙️ Gas Optimization and Batch Transfers

```solidity
function safeBatchTransferFrom(
  address from,
  address to,
  uint256[] memory ids,
  uint256[] memory amounts,
  bytes memory data
) public;
```

Batch minting and transfers drastically reduce gas usage compared to multiple ERC-721 transactions 💸

#### 🕹️ Gaming and Metaverse Integration Examples

- **Gods Unchained**: Uses ERC-1155 to handle thousands of identical card assets.
- **Decentraland**: Integrates both ERC-721 and ERC-1155 for land and wearables.

## 🚀 How to Create Your Own Token

Whether you're launching a DAO governance token, powering in-game economies, or building a DeFi protocol, creating your own token is a must-have skill in the Web3 developer toolkit. Thankfully, frameworks like Hardhat and libraries like OpenZeppelin make the process surprisingly smooth 👨‍💻💡

### 📍 Writing and Deploying a Token Contract

#### 🛠️ Setting Up with Hardhat or Truffle

Hardhat has become the go-to tool for Ethereum devs, offering a modern and extensible environment for compiling, testing, and deploying smart contracts.

Install and initialize your project:

```bash
npm install --save-dev hardhat
npx hardhat
```

Choose "Create a basic sample project" and follow the prompts. You’ll get a ready-to-run template.

#### 🧱 Using OpenZeppelin Contracts for Security and Standards

OpenZeppelin is a battle-tested contract library that saves you from reinventing the wheel—and introducing security bugs 😬

```bash
npm install @openzeppelin/contracts
```

Let’s import the ERC-20 standard:

```solidity
// contracts/MyToken.sol
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}
```

#### 👇 Step-by-Step Deployment

Here’s how to deploy your token using a simple Hardhat script:

```javascript
// scripts/deploy.js
async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying contracts with:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy(ethers.utils.parseEther("1000000"));
  console.log("Token deployed to:", token.address);
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

Run the deploy script:

```bash
npx hardhat run scripts/deploy.js --network goerli
```

#### 🔄 Adding Custom Logic

Want to restrict minting or allow users to burn tokens? Here’s how:

```solidity
function burn(uint256 amount) public {
    _burn(msg.sender, amount);
}

function mint(address to, uint256 amount) public onlyOwner {
    _mint(to, amount);
}
```

With just a few lines, you’ve extended the basic ERC-20 with more powerful capabilities 🔥

### 📍 Testing and Verification

#### 🧪 Writing Unit Tests

Hardhat supports Mocha + Chai for testing:

```javascript
describe("MyToken", function () {
  it("Should assign total supply to owner", async function () {
    const [owner] = await ethers.getSigners();
    const Token = await ethers.getContractFactory("MyToken");
    const token = await Token.deploy(ethers.utils.parseEther("1000"));

    expect(await token.balanceOf(owner.address)).to.equal(
      ethers.utils.parseEther("1000")
    );
  });
});
```

✅ Pro tip: Cover edge cases like transfers, burns, and allowance changes.

#### 🛰️ Using Etherscan for Contract Verification

Once deployed, verify your contract on Etherscan:

```bash
npx hardhat verify --network goerli DEPLOYED_CONTRACT_ADDRESS "1000000000000000000000"
```

This makes your contract transparent and easier for users (and devs) to interact with!

### 📍 Use Cases in DeFi and Gaming

#### 💰 DeFi Use Cases

- **Governance Tokens**: Like COMP or UNI—used to vote on proposals.
- **Staking & Yield Farming**: Earn rewards in native tokens.
- **LP Tokens**: Represent liquidity in pools (e.g., Uniswap, SushiSwap).

#### 🎮 Gaming Use Cases

- **In-Game Currency**: Spendable across marketplaces.
- **Achievement Tokens**: Earned through milestones or challenges.
- **Loot Boxes & Items**: Randomized or rare NFTs tied to a token economy.

#### 🧮 Tokenomics: Design for Sustainability

Designing a token isn’t just about minting—it’s about value:

- **Supply Caps**: Prevent hyperinflation.
- **Vesting Schedules**: Align incentives with long-term goals.
- **Burn Mechanisms**: Drive deflation and scarcity.

Remember: great tokenomics = higher user engagement + economic stability 📊🔥

## 🖼️ Exploring NFTs: Creating, Minting, and Selling

NFTs aren’t just digital art—they’re programmable assets that live on-chain and can represent anything from collectibles and music to event tickets and domain names. Let’s dive into how to create, mint, and sell them step by step 🧑‍🎨🛠️

### 📍 NFT Creation Process

#### 🧬 Metadata: What Goes into an NFT?

Each NFT is backed by a **metadata file**, usually in JSON format, that contains:

```json
{
  "name": "Pixel Ape #001",
  "description": "A funky ape from the pixel jungle.",
  "image": "ipfs://QmImageCID",
  "attributes": [
    { "trait_type": "Background", "value": "Cyber Pink" },
    { "trait_type": "Eyes", "value": "Laser" }
  ]
}
```

Key elements:

- **name**: NFT title
- **description**: What the NFT represents
- **image**: Link to visual/media asset (hosted via IPFS or Arweave)
- **attributes**: Traits that define rarity or uniqueness

These files are often pinned on decentralized storage to ensure permanence and censorship-resistance.

#### ☁️ Hosting Media: IPFS vs Centralized Storage

**IPFS (InterPlanetary File System)** is the preferred choice for decentralization:

- Pin assets via services like **Pinata**, **Web3.Storage**, or **NFT.Storage**
- Avoids broken links due to centralized server outages

👉 Avoid storing large files on-chain due to gas costs—offload them to IPFS and reference the hash.

#### 🧱 NFT Contract Setup Using OpenZeppelin

A basic ERC-721 NFT contract using OpenZeppelin looks like this:

```solidity
// contracts/MyNFT.sol
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721URIStorage, Ownable {
    uint256 public tokenCounter;

    constructor() ERC721("MyNFT", "MNFT") {
        tokenCounter = 0;
    }

    function createNFT(string memory tokenURI) public onlyOwner returns (uint256) {
        uint256 newTokenId = tokenCounter;
        _safeMint(msg.sender, newTokenId);
        _setTokenURI(newTokenId, tokenURI);
        tokenCounter += 1;
        return newTokenId;
    }
}
```

🎯 This lets you mint NFTs with unique metadata URIs stored off-chain.

### 📍 Minting NFTs

#### 🎯 Lazy Minting vs Pre-Minting

- **Pre-Minting**: Tokens are minted upfront and stored in a dev wallet.
- **Lazy Minting**: NFTs are only minted **on-demand**, typically during a purchase or claim.

Lazy minting reduces gas costs for creators and shifts it to the buyer—widely used by marketplaces like OpenSea.

#### 💡 Gas-Efficient Minting Patterns

- Use **ERC721A** (by Chiru Labs) for batch minting with minimal gas
- Avoid writing unnecessary data on-chain
- Leverage `safeMint()` for security

#### 🔄 Dynamic NFTs

Want NFTs that **evolve or update**? Enter **dynamic NFTs**:

- Use **off-chain metadata endpoints** (e.g., AWS Lambda, Cloudflare Workers)
- Combine with **Chainlink Oracles** for real-world data
- Example: A game character NFT that levels up or a ticket that expires

### 📍 Selling NFTs

#### 🏪 Integrating with Marketplaces

You can integrate directly with NFT marketplaces:

- **OpenSea SDK** (JavaScript & Seaport Protocol)
- **Rarible Protocol** (flexible, multi-chain)

Or simply list your NFTs manually via their UI if you’re deploying on mainnet or testnets.

#### 💰 Setting Royalties and Creator Fees

Royalties allow you to **earn on secondary sales**.

- Define royalties in your metadata
- Use marketplaces that respect off-chain royalty rules

For on-chain royalty enforcement, ERC-2981 is your friend.

#### 📜 Understanding ERC-2981 Standard

ERC-2981 allows smart contracts to define royalty info:

```solidity
function royaltyInfo(uint256 _tokenId, uint256 _salePrice) external view returns (address receiver, uint256 royaltyAmount);
```

This ensures marketplaces and aggregators know exactly **how much to pay the creator** and to **whom** on each sale—automating revenue sharing 🔄💸

## 🛠️ Practical Guide to Building an NFT Minting dApp

Creating an NFT is just the beginning—what truly empowers users is a **minting dApp** where they can interact with your NFTs in a seamless, user-friendly way. Let’s build a complete minting experience using modern tools like **React**, **Ethers.js**, and **Solidity**.

### 🧱 Full Stack Overview

Your dApp stack will look like this:

- **Frontend**: React + TailwindCSS (or any styling framework)
- **Web3 Interaction**: Ethers.js (or Web3.js)
- **Smart Contracts**: Solidity (ERC-721 using OpenZeppelin)
- **Deployment**: Hardhat (for contracts), Vercel/Netlify (for frontend)

Folder structure:

```
/contracts      → Smart contract code
/scripts        → Deployment scripts
/frontend       → React frontend
```

### 🦊 Wallet Integration

#### 🪙 MetaMask

Install MetaMask in the browser and connect via Ethers.js:

```javascript
const provider = new ethers.providers.Web3Provider(window.ethereum);
await provider.send("eth_requestAccounts", []);
const signer = provider.getSigner();
```

#### 🔗 WalletConnect

For mobile or multi-wallet support, use the `web3modal` + WalletConnect provider.

### ✍️ Writing Minting Logic on Frontend

Assume you’ve deployed an ERC-721 contract with a `mintNFT(string memory tokenURI)` function. On the frontend:

```javascript
const contract = new ethers.Contract(contractAddress, abi, signer);
const tx = await contract.mintNFT("ipfs://QmYourTokenMetadata");
await tx.wait();
```

Provide user feedback during minting:

```javascript
setStatus("Minting in progress...");
await tx.wait();
setStatus("Mint successful 🎉");
```

### 📡 Handling Blockchain Events & User Feedback

Listen to contract events using Ethers.js:

```javascript
contract.on("Transfer", (from, to, tokenId) => {
  console.log(`NFT minted: TokenID ${tokenId} to ${to}`);
});
```

💡 Tip: Use loading spinners, toast notifications, and error messages to improve UX.

### 🖌️ UI/UX Design for Web3 Experience

Here’s what makes a great minting dApp:

- **Connect Wallet** button at the top
- Display **wallet address**, **minted count**, and **network info**
- Clear **"Mint NFT"** button with pricing/gas warnings
- Real-time **feedback + event listeners**
- Minimalist design with generous padding, rounded cards, and hover effects 🧑‍🎨

Use **shadcn/ui**, Tailwind, or Chakra UI for beautiful, responsive UIs.

### 🌐 Deploying Frontend with Vercel or Netlify

#### 📤 Vercel Deployment

```bash
cd frontend
vercel
```

#### 📤 Netlify Deployment

```bash
npm run build
netlify deploy
```

Make sure to set `.env` variables (like contract address, network) and rebuild when deploying 🚀

## 🔐 Security and Best Practices in Token and NFT Development

Blockchain is immutable—once deployed, smart contracts can't be changed. That’s why **security isn't optional**, it's **essential**. Even small oversights can lead to exploits costing millions 💸😰

Let’s walk through the top security considerations and how to implement bulletproof best practices in your token/NFT projects.

### 🧨 Common Vulnerabilities

#### 🔁 Reentrancy Attacks

A reentrancy attack happens when an external contract calls back into the vulnerable contract before the first execution is complete—potentially draining funds.

**Classic Example**:

```solidity
(bool success, ) = msg.sender.call{value: amount}("");
require(success, "Transfer failed");
```

Fix it by:

- Using **Checks-Effects-Interactions pattern**
- Leveraging **ReentrancyGuard** from OpenZeppelin

```solidity
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

function withdraw() external nonReentrant {
    // safe logic here
}
```

#### ➕ Integer Overflow/Underflow

Prior to Solidity 0.8.x, math operations could overflow:

```solidity
uint256 a = 2**256 - 1;
a += 1; // wraps to 0
```

🔒 **Solidity 0.8+** now includes built-in overflow checks. For older versions, use `SafeMath` from OpenZeppelin.

### 🔐 Protecting Mint Functions

#### ✅ Whitelisting

Only allow approved addresses to mint during pre-sale or limited drops:

```solidity
mapping(address => bool) public whitelist;

modifier onlyWhitelisted() {
    require(whitelist[msg.sender], "Not whitelisted");
    _;
}
```

#### ✍️ Signature-Based Authentication

Generate off-chain signatures that users can redeem:

```solidity
function mint(bytes calldata signature) external {
    require(verifySignature(signature), "Invalid signature");
    _mint(msg.sender, newTokenId);
}
```

Great for private sales or invite-only mints!

### 🧪 Smart Contract Audits and Tools

Before deploying to mainnet, **audit your contracts thoroughly**. Tools that help:

- **🔍 Slither**: Static analysis for vulnerability detection
- **🛡️ MythX**: Deep analysis using symbolic execution
- **🧪 Hardhat Coverage**: Code coverage to ensure test completeness

Also, always:

- Write **comprehensive test cases** (edge cases, reverts, fuzzing)
- Use **OpenZeppelin** libraries for secure boilerplate

### 🕵️‍♂️ Rate Limiting and Anti-Bot Mechanisms

NFT launches attract bots like moths to a flame 🦟🔥. Implementing fair minting access is key.

Options include:

- **Per-wallet mint caps**
- **Block-based cooldowns** (e.g., time between mints)
- **Merkle trees** for efficient whitelisting
- **Gas price checks** to throttle spamming

Example:

```solidity
require(tx.origin == msg.sender, "No contracts allowed");
require(mintCount[msg.sender] < maxPerWallet, "Mint limit exceeded");
```

🧠 Bonus: Use services like **Civic Pass**, **BrightID**, or **Gitcoin Passport** for identity verification if building gated minting flows.

Security is a culture, not a checklist. The more effort you put into protecting your contracts now, the more confident your users (and investors!) will be later 💼🛡️

## 🌐 Cross-Chain and Layer 2 Considerations

Ethereum may be the birthplace of tokens and NFTs, but it’s far from the only place they thrive. As gas fees fluctuate and user adoption grows, **Layer 2 solutions** and **cross-chain strategies** have become essential for scaling your dApps while keeping user experiences affordable and fast ⚡💸

### 🧱 Minting NFTs on Polygon, Arbitrum, or Optimism

#### 🔹 Why Layer 2?

Layer 2 (L2) solutions offload execution from the Ethereum mainnet, offering:

- **Lower gas fees**
- **Faster transactions**
- **High throughput**
- **Ethereum security via rollups or sidechains**

Let’s break down the most popular L2s for NFT minting:

#### 🎨 Polygon (Sidechain)

- EVM-compatible → works out of the box with your Solidity contracts
- Ideal for **high-volume minting** (e.g., gaming, collectibles)
- Supported by major marketplaces like **OpenSea**, **Rarible**, **Zora**
- Use `@maticnetwork`’s bridge to move assets back to Ethereum

Example deploy with Hardhat:

```bash
npx hardhat run scripts/deploy.js --network polygon
```

#### ⛓️ Arbitrum (Optimistic Rollup)

- Cheaper than Ethereum, more secure than sidechains
- Excellent for **DeFi or high-value NFTs**
- Compatible with most dApp infrastructure (Alchemy, Infura, The Graph)

#### 🌈 Optimism (Optimistic Rollup)

- Similar benefits to Arbitrum with growing ecosystem
- Slightly lower latency than Arbitrum for some use cases
- Now integrated with tools like **Superfluid**, **Chainlink**, and **OpenSea**

### 🌉 Bridging Tokens Across Chains

If your token or NFT lives on one chain and needs to be used on another, **bridging** comes into play. Some of the best tools include:

- **Polygon Bridge**: ETH ↔️ Polygon
- **Hop Protocol**: Arbitrum, Optimism, Polygon
- **Wormhole**: Cross-chain messaging and NFTs
- **LayerZero**: Unified cross-chain protocol with omnichain NFT support

Bridging example:

```solidity
// NFT bridging generally involves burning on one chain and minting on another,
// or using a "lock and release" mechanism via a trusted bridge contract.
```

💡 Keep in mind: Bridges come with trade-offs in **security**, **latency**, and **centralization**.

### 💸 Gas Cost Comparisons and Trade-offs

| Chain    | Avg NFT Mint Cost | Speed | Decentralization | Ecosystem Support |
| -------- | ----------------- | ----- | ---------------- | ----------------- |
| Ethereum | $10–$100+         | Slow  | ✅ High          | ✅✅✅            |
| Polygon  | <$0.01            | Fast  | ⚠️ Medium        | ✅✅              |
| Arbitrum | ~$0.05–$0.30      | Fast  | ✅ High          | ✅✅              |
| Optimism | ~$0.05–$0.25      | Fast  | ✅ High          | ✅✅              |

🔍 Trade-off summary:

- Use **Ethereum** for premium NFTs and secure DeFi.
- Use **Polygon** for mass minting and gaming.
- Use **Arbitrum/Optimism** for speed + security balance.

By designing your contracts and infrastructure to be **multi-chain aware**, you’ll future-proof your project and make it accessible to a global, cost-sensitive user base 🌍🔓
