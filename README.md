# 0xKYC Documentation Overview

Welcome to the 0xKYC project documentation hub. This README provides a comprehensive overview of all 0xKYC resources, documentation, and repositories.

## What is 0xKYC?

0xKYC is a privacy-preserving identity verification service for Web3 Gaming, Decentralized Finance, and Discord communities. It provides blockchain-based solutions for proof of personhood and uniqueness verification while maintaining user privacy through zero-knowledge proofs.

## 🎯 Use Cases

- **Web3 Gaming**: Anti-cheat and fair reward distribution
- **DAO Governance**: Sybil-resistant voting mechanisms
- **Fairdrops**: Prevent duplicate accounts in token distributions
- **Compliance Verification**: Sanctions checking without revealing identity

## 🌟 Core Products

### 1. **Sunscreen - Proof of Uniqueness**
- Eliminates bots and multiple accounts through biometric verification
- Assigns unique identifiers (UUIDs) to users across multiple wallets
- Frictionless biometric scan process

### 2. **0xKYC - Sanctions + AML**
- Verifies users are not sanctioned without revealing personal information
- Biometric + identity document scanning
- Creates soulbound tokens (SBTs) for compliance verification

## 📚 Documentation Resources

### [docs.0xkyc.id](https://docs.0xkyc.id)

The main documentation site provides developer guides and implementation resources:

> **Note**: The local `IMPLEMENTATION.md` file contains more comprehensive and up-to-date technical implementation details than the current online documentation.

#### **Overview Section**
- **What We Do**: Privacy-preserving identity verification for Web3
- **Our Products**: Details on Sunscreen and 0xKYC offerings

#### **Developer Guides**
- **Start Here**: Tutorial with GitHub repository setup and basic implementation
- **Client or Server Guide**:
  - **Sunscreen Uniqueness Verification**:
    - Define ABI for smart contract integration
    - Get unique identifier for wallet addresses
    - Working with UUIDs for duplicate management
  - **0xKYC Sanctions/AML**:
    - Contract addresses across multiple networks
    - ABI definitions for soulbound tokens
    - Checking for SBT (Soul Bound Token) verification
    - Front-end token gating implementation

#### **Blockchain Guide**
- **Smart Contract**: Solidity modifier examples for token gating
- **On-Chain Verification**: Zero-knowledge proof validation
- **No-Code Etherscan**: Manual verification through blockchain explorers

#### **Public APIs**
- Sandbox API: `https://sandboxapi.0xkyc.id/docs`
- Production API: `https://api.0xkyc.id/docs`
- Endpoints for authentication, SBT checking, UUID retrieval, and wallet pairing

#### **Integration Options**
- **Front-end Token Gate**: Direct client-side verification
- **Smart Contract Integration**: On-chain verification with Solidity modifiers
- **API and Custom Integrations**: RESTful API access for server-side implementations

## 🛠️ GitHub Repositories

### [github.com/0xKYC](https://github.com/0xKYC)

The main organization repository contains multiple projects:

#### **Key Repositories**
- **tutorial-0xkyc**: Implementation guide and examples
- **FRONTEND**: React application for user verification
- **BACKEND**: NestJS server infrastructure
- **discord-bot**: Discord integration for server verification
- **0xkyc-1vote-aragon-plugin**: DAO voting plugin (hackathon winner)
- **mock-uniswap-demo**: Demonstration of SBT integration

#### **Team**
- **CEO**: Adam Zasada
- **CTO**: Dylan Wysocki
- Multi-disciplinary engineering and operations team

#### **Backed By**
- Outlier Ventures
- New Order DAO
- Onfido
- Hinkal Protocol
- Scroll Ecosystem

## 🤖 Discord Bot

### [github.com/0xKYC/discord-bot](https://github.com/0xKYC/discord-bot)

A Discord integration for proof of personhood verification:

#### **Features**
- Two-tier verification system:
  - "Verified" status for confirmed live persons
  - "Unique" status for first-time biometric verification
- KYC integration with Onfido biometric verification
- User invitation tracking
- Anti-duplicate mechanisms for reward systems

#### **Technical Stack**
- Discord.js v14
- TypeScript
- Node.js
- Onfido integration

#### **Deployment**
- Successfully verified 1,000+ users for Galaxia Studios
- Currently functional with basic commands (`/ping`, `/server`, `/user`)
- Known issue with unique role assignments (to be addressed)

#### **Setup**
```bash
git clone https://github.com/0xkyc/discord-bot.git
cd discord-bot
npm install
# Configure .env with DISCORD_TOKEN
npm start
```

## 📋 IMPLEMENTATION.md

The local `IMPLEMENTATION.md` file contains comprehensive technical implementation details:

#### **Content Overview**
- **3-Step Frontend Integration**: Token gating implementation
- **Contract Addresses**: Testnet addresses for Goerli, Mumbai, and Scroll Alpha
- **ABI Definitions**: Complete smart contract interfaces
- **Smart Contract Implementation**: Solidity modifier examples
- **On-Chain Verification**: Zero-knowledge proof validation
- **Proof of Uniqueness**: UUID-based user identification system

#### **Key Features**
- No personal data shared on-chain (privacy-preserving)
- Prevents duplicate accounts across platforms
- Enables fair reward distribution in Web3 applications
- Direct blockchain integration examples
- Web3.js implementation snippets
- Etherscan verification guides
- Production-ready code examples

#### **Networks Supported**
- Polygon Mainnet (Production)
- BNB Smart Chain
- Ethereum Sepolia
- Scroll Mainnet
- Scroll Alpha Testnet
- Mumbai Testnet
- Goerli Testnet

## 🚀 Quick Start

1. **For Developers**: Start with [docs.0xkyc.id/developer-guides/start-here](https://docs.0xkyc.id/developer-guides/start-here)
2. **For Implementation**: Reference the local `IMPLEMENTATION.md` file
3. **For Discord Integration**: Check out the discord-bot repository
4. **For Testing**: Use sandbox environments at `sandbox.0xkyc.id`

## 📞 Support

- **Contact**: [x.com/adag1oeth](https://x.com/adag1oeth)
- **Documentation**: docs.0xkyc.id
- **GitHub Issues**: Available in respective repositories

## 📄 License

This project is open-source. See individual repositories for specific licensing information.

---

*For the most up-to-date documentation, visit [docs.0xkyc.id](https://docs.0xkyc.id)*